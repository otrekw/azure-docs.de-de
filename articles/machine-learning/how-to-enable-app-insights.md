---
title: Überwachen und Erfassen von Daten von Machine Learning-Webdienst-Endpunkten
titleSuffix: Azure Machine Learning
description: Überwachen Sie Webdienste, die mit Azure Machine Learning bereitgestellt werden, mithilfe von Azure Application Insights.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.reviewer: jmartens
ms.author: larryfr
author: blackmist
ms.date: 06/09/2020
ms.custom: tracking-python
ms.openlocfilehash: d28cd3b1d8722970505eb313bd8e80589ce9ff87
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84743506"
---
# <a name="monitor-and-collect-data-from-ml-web-service-endpoints"></a>Überwachen und Erfassen von Daten von ML-Webdienst-Endpunkten
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie Daten von Modellen erfassen, die in Webdienstendpunkten in Azure Kubernetes Service (AKS) oder Azure Container Instances (ACI) bereitgestellt wurden, und wie Sie diese Modelle überwachen, indem Sie Azure Application Insights mit einer der folgenden Methoden aktivieren: 
* [Python-SDK für Azure Machine Learning](#python)
* [Azure Machine Learning Studio](#studio) unter https://ml.azure.com

Sie können nicht nur die Ausgabedaten und Antworten eines Endpunkts erfassen, sondern auch Folgendes überwachen:

* Anforderungsraten, Antwortzeiten und Fehlerraten
* Abhängigkeitsraten, Antwortzeiten und Fehlerraten
* Ausnahmen

[Weitere Informationen zu Azure Application Insights](../azure-monitor/app/app-insights-overview.md) 


## <a name="prerequisites"></a>Voraussetzungen

* Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.

* Ein Azure Machine Learning-Arbeitsbereich, ein lokales Verzeichnis mit Ihren Skripts und das Azure Machine Learning SDK für Python müssen installiert sein. Informationen zum Erfüllen dieser Voraussetzungen finden Sie unter [Konfigurieren einer Entwicklungsumgebung](how-to-configure-environment.md).

* Ein trainiertes Machine Learning-Modell, das in Azure Kubernetes Service (AKS) oder Azure Container Instance (ACI) bereitgestellt werden soll. Wenn Sie keines besitzen, sehen Sie sich das Tutorial zum [Trainieren eines Imageklassifizierungsmodells](tutorial-train-models-with-aml.md) an.

## <a name="web-service-metadata-and-response-data"></a>Meta- und -Antwortdaten eines Webdiensts

> [!IMPORTANT]
> Azure Application Insights protokolliert nur Nutzlasten von bis zu 64 KB. Bei Erreichen dieses Grenzwerts treten unter Umständen Fehler auf (etwa aufgrund von unzureichendem Arbeitsspeicher), oder es werden ggf. keine Informationen protokolliert.

Fügen Sie der Datei „score.py“ Anweisungen vom Typ `print` hinzu, um Informationen für eine an den Webdienst gerichtete Anforderung zu protokollieren. Jede Anweisung vom Typ `print` hat einen Eintrag in der Ablaufverfolgungstabelle in Application Insights unter der Meldung `STDOUT` zur Folge. Der Inhalt der Anweisung `print` befindet sich unter `customDimensions` und anschließend unter `Contents` in der Ablaufverfolgungstabelle. Wenn Sie eine JSON-Zeichenfolge ausgeben, wird in der Ablaufverfolgungsausgabe unter `Contents`eine hierarchische Datenstruktur generiert.

Sie können Azure Application Insights für den Zugriff auf diese Daten direkt abfragen oder zur längeren Aufbewahrung oder weiteren Verarbeitung einen [fortlaufenden Export](https://docs.microsoft.com/azure/azure-monitor/app/export-telemetry) in ein Speicherkonto einrichten. Modelldaten können dann im Azure Machine Learning-Dienst zum Einrichten von Bezeichnungen, erneuten Trainings, Erklärungen, Datenanalysen oder zu anderen Zwecken verwendet werden. 

<a name="python"></a>

## <a name="use-python-sdk-to-configure"></a>Verwenden des Python SDK zum Konfigurieren 

### <a name="update-a-deployed-service"></a>Aktualisieren eines bereitgestellten Diensts

1. Suchen Sie den Dienst in Ihrem Arbeitsbereich. Der Wert für `ws` ist der Name des Arbeitsbereichs.

    ```python
    from azureml.core.webservice import Webservice
    aks_service= Webservice(ws, "my-service-name")
    ```
2. Aktualisieren Sie Ihren Dienst, und aktivieren Sie Azure Application Insights.

    ```python
    aks_service.update(enable_app_insights=True)
    ```

### <a name="log-custom-traces-in-your-service"></a>Protokollieren von benutzerdefinierten Ablaufverfolgungen in Ihrem Dienst

Wenn Sie benutzerdefinierte Ablaufverfolgungen protokollieren möchten, führen Sie den im Dokument [Bereitstellung: wie und wo?](how-to-deploy-and-where.md) beschriebenen Standardbereitstellungsprozess für AKS oder ACI aus. Führen Sie dann die folgenden Schritte aus:

1. Wenn Sie Daten während des Rückschließens an Application Insights senden möchten, aktualisieren Sie die Bewertungsdatei durch Hinzufügen von print-Anweisungen. Wenn Sie komplexere Informationen wie etwa die Anforderungsdaten und die Antwort protokollieren möchten, verwenden Sie eine JSON-Struktur. Von der folgenden Beispieldatei „score.py“ werden die Initialisierungszeit des Modells, die Ein- und Ausgabe während des Rückschließens sowie die Zeit protokolliert, zu der Fehler auftreten:

    > [!IMPORTANT]
    > Azure Application Insights protokolliert nur Nutzlasten von bis zu 64 KB. Bei Erreichen dieses Grenzwerts treten unter Umständen Fehler auf (etwa aufgrund von unzureichendem Arbeitsspeicher), oder es werden ggf. keine Informationen protokolliert. Sind die Daten, die Sie protokollieren möchten, größer als 64 KB, speichern Sie sie stattdessen in Blob Storage, wie unter [Sammeln von Daten für Modelle in der Produktion](how-to-enable-data-collection.md) beschrieben.
    
    ```python
    import pickle
    import json
    import numpy 
    from sklearn.externals import joblib
    from sklearn.linear_model import Ridge
    from azureml.core.model import Model
    import time

    def init():
        global model
        #Print statement for appinsights custom traces:
        print ("model initialized" + time.strftime("%H:%M:%S"))
        
        # note here "sklearn_regression_model.pkl" is the name of the model registered under the workspace
        # this call should return the path to the model.pkl file on the local disk.
        model_path = Model.get_model_path(model_name = 'sklearn_regression_model.pkl')
        
        # deserialize the model file back into a sklearn model
        model = joblib.load(model_path)
    

    # note you can pass in multiple rows for scoring
    def run(raw_data):
        try:
            data = json.loads(raw_data)['data']
            data = numpy.array(data)
            result = model.predict(data)
            # Log the input and output data to appinsights:
            info = {
                "input": raw_data,
                "output": result.tolist()
                }
            print(json.dumps(info))
            # you can return any datatype as long as it is JSON-serializable
            return result.tolist()
        except Exception as e:
            error = str(e)
            print (error + time.strftime("%H:%M:%S"))
            return error
    ```

2. Aktualisieren Sie die Dienstkonfiguration.
    
    ```python
    config = Webservice.deploy_configuration(enable_app_insights=True)
    ```

3. Erstellen Sie ein Image, und stellen Sie es in [AKS oder ACI](how-to-deploy-and-where.md) bereit.

### <a name="disable-tracking-in-python"></a>Deaktivieren der Nachverfolgung in Python

Verwenden Sie den folgenden Code, um Azure Application Insights zu deaktivieren:

```python 
## replace <service_name> with the name of the web service
<service_name>.update(enable_app_insights=False)
```

<a name="studio"></a>

## <a name="use-azure-machine-learning-studio-to-configure"></a>Verwenden von Azure Machine Learning Studio für die Konfiguration

Sie können Azure Application Insights auch über Azure Machine Learning Studio aktivieren, wenn Sie Ihr Modell mit den folgenden Schritten bereitstellen möchten:

1. Melden Sie sich unter https://ml.azure.com/ bei Ihrem Arbeitsbereich an.
1. Navigieren Sie zu **Modelle**, und wählen Sie das bereitzustellende Modell aus.
1. Wählen Sie **+ Bereitstellen** aus.
1. Füllen Sie das Formular **Modell bereitstellen** aus.
1. Erweitern Sie das Menü **Erweitert**.

    ![Bereitstellungsformular](./media/how-to-enable-app-insights/deploy-form.png)
1. Aktivieren Sie **Application Insights-Diagnose und -Datensammlung aktivieren**.

    ![Aktivieren von App Insights](./media/how-to-enable-app-insights/enable-app-insights.png)
## <a name="evaluate-data"></a>Evaluieren von Daten
Die Daten Ihres Diensts werden in Ihrem Azure Application Insights-Konto in der Ressourcengruppe gespeichert, in der sich auch Azure Machine Learning befindet.
So zeigen Sie sie an:

1. Navigieren Sie im [Azure-Portal](https://ms.portal.azure.com/) zu Ihrem Azure Machine Learning-Arbeitsbereich, und klicken Sie auf den Link „Application Insights“.

    [![AppInsightsLoc](./media/how-to-enable-app-insights/AppInsightsLoc.png)](././media/how-to-enable-app-insights/AppInsightsLoc.png#lightbox)

1. Wählen Sie auf der Registerkarte **Übersicht** oder im Abschnitt __Überwachung__ in der Liste auf der linken Seite die Option __Protokolle__ aus.

    [![Registerkarte „Übersicht“ im Abschnitt „Überwachung“](./media/how-to-enable-app-insights/overview.png)](./media/how-to-enable-app-insights/overview.png#lightbox)

1. Wenn Sie protokollierte Informationen aus der Datei „score.py“ anzeigen möchten, sehen Sie sich die Tabelle __traces__ an. Mit der folgenden Abfrage wird nach Protokollen gesucht, bei denen der Wert __input__ protokolliert wurde:

    ```kusto
    traces
    | where customDimensions contains "input"
    | limit 10
    ```

   [![Ablaufverfolgungsdaten](./media/how-to-enable-app-insights/model-data-trace.png)](././media/how-to-enable-app-insights/model-data-trace.png#lightbox)

Weitere Informationen zu Azure Application Insights finden Sie unter [Was ist Application Insights?](../azure-monitor/app/app-insights-overview.md).

## <a name="export-data-for-further-processing-and-longer-retention"></a>Exportieren von Daten zur weiteren Verarbeitung und längeren Aufbewahrung

>[!Important]
> Azure Application Insights unterstützt nur Exporte in einen Blobspeicher. Weitere Beschränkungen für diese Exportfunktion finden Sie unter [Exportieren von Telemetriedaten aus App Insights](https://docs.microsoft.com/azure/azure-monitor/app/export-telemetry#continuous-export-advanced-storage-configuration).

Sie können den [fortlaufenden Export](https://docs.microsoft.com/azure/azure-monitor/app/export-telemetry) von Azure Application Insights verwenden, um Nachrichten an ein unterstütztes Speicherkonto zu senden, für das eine längere Aufbewahrungsdauer festgelegt werden kann. Die Daten werden im JSON-Format gespeichert und können ganz einfach zum Extrahieren von Modelldaten analysiert werden. 

Die Daten können bei Bedarf mit Azure Data Factory, Azure ML-Pipelines oder anderen Datenverarbeitungstools transformiert werden. Nach der Transformation der Daten können Sie sie im Azure Machine Learning-Arbeitsbereich als Dataset registrieren. Informationen dazu finden Sie unter [Erstellen und Registrieren von Datasets](how-to-create-register-datasets.md).

   [![Fortlaufender Export](./media/how-to-enable-app-insights/continuous-export-setup.png)](././media/how-to-enable-app-insights/continuous-export-setup.png)


## <a name="example-notebook"></a>Notebook mit Beispielen

Im Notebook [enable-app-insights-in-production-service.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/enable-app-insights-in-production-service/enable-app-insights-in-production-service.ipynb) werden die in diesem Artikel behandelten Konzepte veranschaulicht. 
 
[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Bereitstellen Ihrer Modelle in Webdienst-Endpunkten sowie zum Aktivieren von Azure Application Insights zum Nutzen der Datenerfassung und Endpunktüberwachung finden Sie unter [Bereitstellen eines Modells in einem Azure Kubernetes Service-Cluster](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-azure-kubernetes-service) bzw. [Bereitstellen eines Modells in Azure Container Instances](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-azure-container-instance).
* Unter [MLOps: Verwalten, Bereitstellen und Überwachen von Modellen mit Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/concept-model-management-and-deployment) erfahren Sie, wie Sie von Modellen in der Produktion erfasste Daten nutzen. Solche Daten können Sie dabei unterstützen, Ihren Machine Learning-Prozess kontinuierlich zu verbessern.
