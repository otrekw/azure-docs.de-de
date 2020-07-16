---
title: Sammeln von Daten zu Ihren Produktionsmodellen
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Daten des Azure Machine Learning-Eingabemodells in einem Azure-Blobspeicher gesammelt werden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.reviewer: laobri
ms.author: copeters
author: lostmygithubaccount
ms.date: 11/12/2019
ms.custom: seodec18
ms.openlocfilehash: 75402c71316f7cc7d068c12a240f3123569a00ea
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84432997"
---
# <a name="collect-data-for-models-in-production"></a>Sammeln von Daten für Modelle in der Produktion

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel wird beschrieben, wie Eingabemodelldaten aus Azure Machine Learning erfasst werden. Darüber hinaus wird gezeigt, wie die Eingabedaten in einem AKS-Cluster (Azure Kubernetes Service) bereitgestellt und die Ausgabedaten in Azure Blob Storage gespeichert werden.

Nach der Aktivierung der Sammlung unterstützen die gesammelten Daten Sie bei den folgenden Aufgaben:

* [Überwachen von Datendrifts](how-to-monitor-data-drift.md) bei der Aufnahme von Produktionsdaten in das Modell

* Fundiertere Entscheidungen in Bezug auf den Zeitpunkt zum erneuten Trainieren oder Optimieren des Modells

* Erneutes Trainieren des Modells mit den gesammelten Daten

## <a name="what-is-collected-and-where-it-goes"></a>Welche Daten werden gesammelt, und wo werden sie gespeichert?

Die folgenden Daten können gesammelt werden:

* Modelleingabedaten von Webdiensten, die in einem AKS-Cluster bereitgestellt werden. Sprachaudiodaten, Bilder und Videos werden *nicht* erfasst.
  
* Modellvorhersagen, die auf Eingabedaten aus der Produktion gestützt sind.

>[!NOTE]
> Vorabaggregation und -berechnungen für diese Daten sind zurzeit nicht Teil des Sammlungsdiensts.

Die Ausgabe wird im Blobspeicher gespeichert. Da die Daten dem Blobspeicher hinzugefügt werden, können Sie Ihr bevorzugtes Tool zum Ausführen der Analyse auswählen.

Der Pfad zu den Ausgabedaten im Blob folgt dieser Syntax:

```
/modeldata/<subscriptionid>/<resourcegroup>/<workspace>/<webservice>/<model>/<version>/<designation>/<year>/<month>/<day>/data.csv
# example: /modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/12/31/data.csv
```

>[!NOTE]
> In älteren Versionen des Azure Machine Learning SDK für Python als Version 0.1.0a16 hat das Argument `designation` den Namen `identifier`. Wenn Ihr Code mit einer älteren Version entwickelt wurde, müssen Sie ihn entsprechend aktualisieren.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://aka.ms/AMLFree) erstellen, bevor Sie beginnen.

- Ein Azure Machine Learning-Arbeitsbereich, ein lokales Verzeichnis mit Ihren Skripts und das Azure Machine Learning SDK für Python müssen installiert sein. Informationen zur Installation finden Sie unter [Konfigurieren einer Entwicklungsumgebung](how-to-configure-environment.md).

- Ein trainiertes Machine Learning-Modell muss in AKS bereitgestellt werden. Wenn Sie keines besitzen, sehen Sie sich das Tutorial zum [Trainieren eines Bildklassifizierungsmodells](tutorial-train-models-with-aml.md) an.

- Sie benötigen einen AKS-Cluster. Informationen zum Erstellen und Bereitstellen finden Sie unter [Bereitstellen von Modellen mit Azure Machine Learning](how-to-deploy-and-where.md).

- [Einrichten Ihrer Umgebung](how-to-configure-environment.md) und Installieren des [Azure Machine Learning Monitoring SDK](https://aka.ms/aml-monitoring-sdk)

## <a name="enable-data-collection"></a>Aktivieren der Datensammlung

Sie können die Datensammlung ungeachtet des über Azure Machine Learning oder andere Tools bereitgestellten Modells aktivieren.

Führen Sie zum Aktivieren der Datensammlung folgende Schritte aus:

1. Öffnen Sie die Bewertungsdatei.

1. Fügen Sie den [folgenden Code](https://aka.ms/aml-monitoring-sdk) am Anfang der Datei hinzu:

   ```python 
   from azureml.monitoring import ModelDataCollector
   ```

1. Deklarieren Sie die Variablen für die Datensammlung in der `init`-Funktion:

    ```python
    global inputs_dc, prediction_dc
    inputs_dc = ModelDataCollector("best_model", designation="inputs", feature_names=["feat1", "feat2", "feat3". "feat4", "feat5", "feat6"])
    prediction_dc = ModelDataCollector("best_model", designation="predictions", feature_names=["prediction1", "prediction2"])
    ```

    Der Parameter *CorrelationId* ist optional. Sie müssen ihn nicht verwenden, wenn er für das Modell nicht erforderlich ist. Die Verwendung von *CorrelationId* erleichtert die Zuordnung zu anderen Daten, etwa *LoanNumber* oder *CustomerId*.
    
    Der Parameter *Identifier* wird später zum Erstellen der Ordnerstruktur in Ihrem Blob verwendet. Sie können ihn verwenden, um Rohdaten von verarbeiteten Daten zu unterscheiden.

1. Fügen Sie die folgenden Codezeilen der `run(input_df)`-Funktion hinzu:

    ```python
    data = np.array(data)
    result = model.predict(data)
    inputs_dc.collect(data) #this call is saving our input data into Azure Blob
    prediction_dc.collect(result) #this call is saving our input data into Azure Blob
    ```

1. Die Datensammlung wird *nicht* automatisch auf **true** festgelegt, wenn Sie einen Dienst in AKS bereitstellen. Aktualisieren Sie Ihre Konfigurationsdatei, wie im folgenden Beispiel gezeigt:

    ```python
    aks_config = AksWebservice.deploy_configuration(collect_model_data=True)
    ```

    Sie können auch Application Insights für die Dienstüberwachung aktivieren, indem Sie diese Konfiguration ändern:

    ```python
    aks_config = AksWebservice.deploy_configuration(collect_model_data=True, enable_app_insights=True)
    ```

1. Informationen zum Erstellen eines neuen Images und zum Bereitstellen des Diensts finden Sie im Dokument [Bereitstellen von Modellen mit Azure Machine Learning](how-to-deploy-and-where.md).

Wenn Sie in der Umgebungsdatei und Bewertungsdatei bereits einen Dienst mit den Abhängigkeiten installiert haben, aktivieren Sie die Datensammlung mit den folgenden Schritten:

1. Wechseln Sie zu [Azure Machine Learning](https://ml.azure.com).

1. Öffnen Sie Ihren Arbeitsbereich.

1. Wählen Sie **Bereitstellungen** > **Dienst auswählen** > **Bearbeiten** aus.

   ![Bearbeiten des Diensts](././media/how-to-enable-data-collection/EditService.PNG)

1. Aktivieren Sie in **Erweiterte Einstellungen** die Option **Application Insights-Diagnose und -Datensammlung aktivieren**.

1. Wählen Sie **Aktualisieren** aus, um die Änderungen zu übernehmen.

## <a name="disable-data-collection"></a>Datensammlung deaktivieren

Sie können das Sammeln von Daten jederzeit beenden. Deaktivieren Sie die Datensammlung mithilfe von Python-Code oder Azure Machine Learning.

### <a name="option-1---disable-data-collection-in-azure-machine-learning"></a>Option 1: Deaktivieren der Datensammlung in Azure Machine Learning

1. Melden Sie sich bei [Azure Machine Learning](https://ml.azure.com) an.

1. Öffnen Sie Ihren Arbeitsbereich.

1. Wählen Sie **Bereitstellungen** > **Dienst auswählen** > **Bearbeiten** aus.

   [![Auswählen der Option „Bearbeiten“](././media/how-to-enable-data-collection/EditService.PNG)](./././media/how-to-enable-data-collection/EditService.PNG#lightbox)

1. Deaktivieren Sie in **Erweiterte Einstellungen** die Option **Application Insights-Diagnose und -Datensammlung aktivieren**.

1. Klicken Sie auf **Aktualisieren**, um die Änderungen zu übernehmen.

Sie können auf diese Einstellungen auch in Ihrem Arbeitsbereich in [Azure Machine Learning](https://ml.azure.com) zugreifen.

### <a name="option-2---use-python-to-disable-data-collection"></a>Option 2: Deaktivieren der Datensammlung mithilfe von Python

  ```python 
  ## replace <service_name> with the name of the web service
  <service_name>.update(collect_model_data=False)
  ```

## <a name="validate-and-analyze-your-data"></a>Überprüfen und Analysieren Ihrer Daten

Sie können ein beliebiges Tool Ihrer Wahl verwenden, um die im Blobspeicher erfassten Daten zu analysieren.

### <a name="quickly-access-your-blob-data"></a>Schnelles Zugreifen auf Ihre Blobdaten

1. Melden Sie sich bei [Azure Machine Learning](https://ml.azure.com) an.

1. Öffnen Sie Ihren Arbeitsbereich.

1. Wählen Sie **Speicher**.

    [![Auswählen der Speicheroption](./media/how-to-enable-data-collection/StorageLocation.png)](././media/how-to-enable-data-collection/StorageLocation.png#lightbox)

1. Folgen Sie dem Pfad zu den Ausgabedaten des Blobs mit dieser Syntax:

   ```
   /modeldata/<subscriptionid>/<resourcegroup>/<workspace>/<webservice>/<model>/<version>/<designation>/<year>/<month>/<day>/data.csv
   # example: /modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/12/31/data.csv
   ```

### <a name="analyze-model-data-using-power-bi"></a>Analysieren von Modelldaten mithilfe von Power BI

1. Laden Sie [Power BI Desktop](https://www.powerbi.com) herunter, und öffnen Sie diese Anwendung.

1. Wählen Sie **Daten abrufen** und anschließend [**Azure Blob Storage**](https://docs.microsoft.com/power-bi/desktop-data-sources) aus.

    [![Power BI-Blob-Setup](./media/how-to-enable-data-collection/PBIBlob.png)](././media/how-to-enable-data-collection/PBIBlob.png#lightbox)

1. Fügen Sie den Namen Ihres Speicherkontos hinzu, und geben Sie Ihren Speicherschlüssel ein. Sie finden diese Informationen, indem Sie in Ihrem Blob **Einstellungen** > **Zugriffsschlüssel** auswählen.

1. Wählen Sie den Container **Modelldaten** und dann **Bearbeiten** aus.

    [![Power BI-Navigator](./media/how-to-enable-data-collection/pbiNavigator.png)](././media/how-to-enable-data-collection/pbiNavigator.png#lightbox)

1. Klicken Sie im Abfrage-Editor unter der Spalte **Name**, und fügen Sie Ihr Speicherkonto hinzu.

1. Geben Sie Ihren Modellpfad in den Filter ein. Erweitern Sie einfach den Filterpfad, wenn Sie sich nur die Dateien für ein bestimmtes Jahr oder einen bestimmten Monat ansehen möchten. Verwenden Sie beispielsweise den folgenden Filterpfad, wenn Sie nur die Daten vom März anzeigen möchten:

   /modeldata/\<subscriptionid>/\<resourcegroupname>/\<workspacename>/\<webservicename>/\<modelname>/\<modelversion>/\<designation>/\<year>/3

1. Filtern Sie die Daten, die für Sie relevant sind, nach den Werten für **Name**. Wenn Sie Vorhersagen und Eingaben gespeichert haben, müssen Sie für jede jeweils eine separate Abfrage erstellen.

1. Wählen Sie den nach unten zeigenden Doppelpfeil neben der Spaltenüberschrift **Inhalt** aus, um die Dateien zu kombinieren.

    [![Power BI: Inhalt](./media/how-to-enable-data-collection/pbiContent.png)](././media/how-to-enable-data-collection/pbiContent.png#lightbox)

1. Klicken Sie auf **OK**. Die Daten werden vorab geladen.

    [![Power BI: Kombinieren von Dateien](./media/how-to-enable-data-collection/pbiCombine.png)](././media/how-to-enable-data-collection/pbiCombine.png#lightbox)

1. Wählen Sie **Schließen und übernehmen** aus.

1. Wenn Sie Eingaben und Vorhersagen hinzugefügt haben, werden Ihre Tabellen automatisch anhand der **RequestId**-Werte sortiert.

1. Beginnen Sie mit der Erstellung Ihrer benutzerdefinierten Berichte in Ihren Modelldaten.

### <a name="analyze-model-data-using-azure-databricks"></a>Analysieren von Modelldaten mithilfe von Azure Databricks

1. Erstellen Sie einen [Azure Databricks-Arbeitsbereich](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal).

1. Greifen Sie auf Ihren Databricks-Arbeitsbereich zu.

1. Wählen Sie in Ihrem Databricks-Arbeitsbereich die Option **Daten hochladen** aus.

    [![Auswählen der Databricks-Option „Daten hochladen“](./media/how-to-enable-data-collection/dbupload.png)](././media/how-to-enable-data-collection/dbupload.png#lightbox)

1. Wählen Sie **Neue Tabelle erstellen** und dann **Weitere Datenquellen** > **Azure Blob Storage** > **Tabelle in Notebook erstellen** aus.

    [![Databricks: Erstellen einer Tabelle](./media/how-to-enable-data-collection/dbtable.PNG)](././media/how-to-enable-data-collection/dbtable.PNG#lightbox)

1. Aktualisieren Sie den Speicherort Ihrer Daten. Beispiel:

    ```
    file_location = "wasbs://mycontainer@storageaccountname.blob.core.windows.net/modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/*/*/data.csv" 
    file_type = "csv"
    ```

    [![Databricks-Setup](./media/how-to-enable-data-collection/dbsetup.png)](././media/how-to-enable-data-collection/dbsetup.png#lightbox)

1. Führen Sie die Schritte in der Vorlage aus, um Ihre Daten anzuzeigen und zu analysieren.
