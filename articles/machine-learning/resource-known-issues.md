---
title: Bekannte Probleme und Problembehandlung
titleSuffix: Azure Machine Learning
description: Hier finden Sie Hilfe zum Suchen und Beheben von Fehlern in Azure Machine Learning. Erfahren Sie mehr über bekannte Probleme, Problembehandlung und Problemumgehungen.
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: contperfq4
ms.date: 03/31/2020
ms.openlocfilehash: 169dd7f71b86c77717226872fecb493a6eb5bf0d
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2020
ms.locfileid: "84309848"
---
# <a name="known-issues-and-troubleshooting-in-azure-machine-learning"></a>Bekannte Probleme und Problembehandlung in Azure Machine Learning

Dieser Artikel hilft Ihnen, bekannte Probleme zu beheben, die beim Verwenden von Azure Machine Learning auftreten können. 

Weitere Informationen zur Problembehandlung finden Sie unter [Nächste Schritte](#next-steps) am Ende dieses Artikels.

> [!TIP]
> Fehler könnten das Ergebnis von [Ressourcenkontingenten](how-to-manage-quotas.md) sein, die Sie im Rahmen von Azure Machine Learning antreffen könnten. 

## <a name="access-diagnostic-logs"></a>Zugreifen auf Diagnoseprotokolle

Manchmal kann es hilfreich sein, Diagnoseinformationen bereitstellen zu können, wenn Sie um Hilfe bitten. So zeigen Sie einige Protokolle an 
1. Besuchen Sie [Azure Machine Learning-Studio](https://ml.azure.com). 
1. Wählen Sie auf der linken Seite **Experiment** aus. 
1. Wählen Sie ein Experiment aus.
1. Wählen Sie eine Ausführung aus.
1. Wählen Sie oben **Ausgaben + Protokolle** aus.

> [!NOTE]
> Azure Machine Learning protokolliert während des Trainings auch Informationen aus zahlreichen Quellen, wie z. B. aus AutoML oder dem Docker-Container, in dem der Trainingsauftrag ausgeführt wird. Viele dieser Protokolle sind nicht dokumentiert. Wenn Sie Probleme haben und sich an den Microsoft-Support wenden, können diese Protokolle möglicherweise bei der Problembehandlung verwendet werden.


## <a name="installation-and-import"></a>Installation und Import
                           
* **pip-Installation: Es ist nicht gewährleistet, dass Abhängigkeiten mit der Installation in einer Zeile konsistent sind:** 

   Dies ist eine bekannte Einschränkung von pip, da keine funktionsfähige Abhängigkeitskonfliktauflösung vorhanden ist, wenn Sie die Installation in einer einzigen Zeile ausführen. Es wird nur die erste eindeutige Abhängigkeit untersucht. 

   Im folgenden Code wird sowohl `azure-ml-datadrift` als auch `azureml-train-automl` über eine einzeilige pip-Installation installiert. 
     ```
       pip install azure-ml-datadrift, azureml-train-automl
     ```
   Für dieses Beispiel wird angenommen, dass `azure-ml-datadrift` eine Version > 1.0 und `azureml-train-automl` eine Version < 1.2 benötigt. Wenn die neueste Version von `azure-ml-datadrift` 1.3 ist, werden beide Pakete trotz der Paketanforderung von `azureml-train-automl` nach einer älteren Version auf 1.3 aktualisiert. 

   Um sicherzustellen, dass die richtigen Versionen für Ihre Pakete installiert werden, führen Sie die Installation in mehreren Zeilen durch, wie im folgenden Code. Die Reihenfolge ist hier kein Problem, da pip im Rahmen des nächsten Zeilenaufrufs explizit eine Herabstufung ausführt. Daher werden die richtigen Versionsabhängigkeiten angewandt.
    
     ```
        pip install azure-ml-datadrift
        pip install azureml-train-automl 
     ```
     
* **Es ist nicht garantiert, dass das Erklärungspaket installiert wird, wenn azureml-train-automl-client installiert wird:** 
   
   Wenn Sie AutoML remote mit aktivierter Modellerklärung ausführen, wird Ihnen folgende Fehlermeldung angezeigt: „Please install azureml-explain-model package for model explanations“ (Installieren Sie das azureml-explain-model-Paket für Modellerklärungen). Dies ist ein bekanntes Problem. Führen Sie als Problemumgehung einen der folgenden Schritte aus:
  
  1. Installieren Sie azureml-explain-model lokal.
   ```
      pip install azureml-explain-model
   ```
  2. Deaktivieren Sie die Erklärungsfunktion vollständig, indem Sie bei der AutoML-Konfiguration „model_explainability=False“ festlegen.
   ```
      automl_config = AutoMLConfig(task = 'classification',
                             path = '.',
                             debug_log = 'automated_ml_errors.log',
                             compute_target = compute_target,
                             run_configuration = aml_run_config,
                             featurization = 'auto',
                             model_explainability=False,
                             training_data = prepped_data,
                             label_column_name = 'Survived',
                             **automl_settings)
    ``` 
    
* **Pandas-Fehler: Normalerweise bei AutoML-Experimenten angezeigt:**
   
   Wenn Sie Ihre Umgebung manuell mithilfe von pip einrichten, können Ihnen Attributfehler auffallen (insbesondere von Pandas) auf, da nicht unterstützte Paketversionen installiert werden. Um diese Fehler zu vermeiden, [installieren Sie das AutoML SDK mithilfe von „automl_setup.cmd“](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/README.md):
   
    1. Öffnen Sie eine Anaconda-Eingabeaufforderung, und klonen Sie das GitHub-Repository für eine Reihe von Beispielnotebooks.

    ```bash
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```
    
    2. Wechseln Sie in den Ordner „how-to-use-azureml/automated-machine-learning“, in dem die Beispielnotebooks extrahiert wurden, und führen Sie dann Folgendes aus:
    
    ```bash
    automl_setup
    ```
  
* **Fehlermeldung: 'PyYAML' kann nicht deinstalliert werden.**

    Azure Machine Learning SDK für Python: PyYAML ist ein von `distutils` installiertes Projekt. Daher können wir bei einer teilweisen Deinstallation nicht genau bestimmen, welche Dateien zu ihm gehören. Verwenden Sie diesen Befehl, um die Installation des SDKs fortzusetzen und diesen Fehler zu ignorieren:
    
    ```Python
    pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
    ```

* **Databricks-Fehler beim Installieren von Paketen**

    Bei der Installation des Azure Machine Learning SDK tritt in Azure Databricks ein Fehler auf, wenn mehrere Pakete installiert werden. Einige Pakete, z.B. `psutil`, können Konflikte verursachen. Um Fehler bei der Installation zu vermeiden, frieren Sie die Bibliotheksversion beim Installieren der Pakete ein. Dieses Problem hängt mit Databricks und nicht mit dem Azure Machine Learning SDK zusammen. Es kann auch mit anderen Bibliotheken auftreten. Beispiel:
    
    ```python
    psutil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
    ```

    Falls bei Python-Bibliotheken immer wieder Installationsprobleme auftreten, können Sie alternativ Initialisierungsskripts verwenden. Dieser Ansatz wird nicht offiziell unterstützt. Weitere Informationen finden Sie unter [Initialisierungsskripts im Clusterbereich](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

* **Databricks-Importfehler: Name „Timedelta“ kann nicht aus „pandas._libs.tslibs“ importiert werden**: Wenn dieser Fehler angezeigt wird, wenn Sie automatisiertes Machine Learning verwenden, führen Sie die beiden folgenden Zeilen in Ihrem Notebook aus:
    ```
    %sh rm -rf /databricks/python/lib/python3.7/site-packages/pandas-0.23.4.dist-info /databricks/python/lib/python3.7/site-packages/pandas
    %sh /databricks/python/bin/pip install pandas==0.23.4
    ```

* **Databricks-Importfehler: Kein Modul mit dem Namen „pandas.core.indexes“** : Wenn diese Fehlermeldung bei Verwendung von automatisiertem maschinellem Lernen angezeigt wird, gehen Sie folgendermaßen vor:

    1. Führen Sie diesen Befehl aus, um zwei Pakete in Ihrem Azure Databricks-Cluster zu installieren:
    
       ```bash
       scikit-learn==0.19.1
       pandas==0.22.0
       ```
    
    1. Trennen Sie den Cluster auf, und fügen Sie ihn dann wieder an Ihr Notebook an.
    
    Wenn diese Schritte das Problem nicht beheben, versuchen Sie, den Cluster neu zu starten.

* **Databricks FailToSendFeather**: Sollte beim Lesen von Daten aus einem Azure Databricks-Cluster ein Fehler vom Typ `FailToSendFeather` auftreten, haben Sie folgende Möglichkeiten:
    
    * Upgraden Sie das Paket `azureml-sdk[automl]` auf die aktuelle Version.
    * Fügen Sie mindestens die Version 1.1.8 von `azureml-dataprep` hinzu.
    * Fügen Sie mindestens die Version 0.11 von `pyarrow` hinzu.
    
## <a name="create-and-manage-workspaces"></a>Erstellen und Verwalten von Arbeitsbereichen

> [!WARNING]
> Das Verschieben des Azure Machine Learning-Arbeitsbereichs in ein anderes Abonnement oder das Verschieben des besitzenden Abonnements in einen neuen Mandanten wird nicht unterstützt. Andernfalls können Fehler auftreten.

* **Azure-Portal**: Wenn Sie Ihren Arbeitsbereich direkt über einen Freigabelink aus dem SDK oder dem Portal anzeigen, können Sie die normale **Übersichtsseite** mit Abonnementinformationen in der Erweiterung nicht anzeigen. Außerdem können Sie nicht zu einem anderen Arbeitsbereich wechseln. Wenn Sie einen anderen Arbeitsbereich anzeigen möchten, wechseln Sie direkt zu [Azure Machine Learning-Studio](https://ml.azure.com) und suchen nach dem Namen des Arbeitsbereichs.

## <a name="set-up-your-environment"></a>Einrichten Ihrer Umgebung

* **Probleme beim Erstellen von AmlCompute**: In seltenen Fällen kann es vorkommen, dass Benutzer, die vor dem allgemein verfügbaren Release ihren Azure Machine Learning-Arbeitsbereich über das Azure-Portal erstellt haben, AmlCompute nicht in diesem Arbeitsbereich erstellen können. In einem solchen Fall können Sie entweder eine Supportanfrage für den Dienst erstellen oder über das Portal oder das SDK einen neuen Arbeitsbereich erstellen und die Blockierung dadurch umgehend aufheben.

## <a name="work-with-data"></a>Arbeiten mit Daten

### <a name="overloaded-azurefile-storage"></a>Überladener AzureFile-Speicher

Wenn Sie eine Fehlermeldung `Unable to upload project files to working directory in AzureFile because the storage is overloaded` erhalten, wenden Sie die folgenden Problemumgehungen an.

Falls Sie eine Dateifreigabe für andere Workloads (beispielsweise die Datenübertragung) verwenden, empfiehlt es sich, Blobs zu verwenden, damit die Dateifreigabe für die Übermittlung von Ausführungen frei ist. Alternativ kann die Workload auch auf zwei verschiedene Arbeitsbereiche aufgeteilt werden.

### <a name="passing-data-as-input"></a>Übergeben von Daten als Eingabe

*  **TypeError: FileNotFound: Datei oder Verzeichnis nicht vorhanden**: Dieser Fehler tritt auf, wenn sich die Datei nicht an dem von Ihnen angegebenen Dateipfad befindet. Sie müssen sicherstellen, dass Ihre Verweise auf die Datei konsistent mit dem Einbindungsort des Datasets auf Ihrem Computeziel ist. Um einen deterministischen Zustand sicherzustellen, empfiehlt es sich, für die Einbindung eines Datasets an einem Computeziel den abstrakten Pfad zu verwenden. Im folgenden Code wird das Dataset beispielsweise unter dem Stammverzeichnis des Dateisystems des Computeziels eingebunden: `/tmp`. 
    
    ```python
    # Note the leading / in '/tmp/dataset'
    script_params = {
        '--data-folder': dset.as_named_input('dogscats_train').as_mount('/tmp/dataset'),
    } 
    ```

    Wenn Sie den vorangestellten Schrägstrich („/“) nicht einschließen, müssen Sie das Arbeitsverzeichnis auf dem Computeziel als Präfix hinzufügen, z. B. `/mnt/batch/.../tmp/dataset`, um anzugeben, wo das Dataset eingebunden werden soll.

### <a name="data-labeling-projects"></a>Datenbezeichnungsprojekte

|Problem  |Lösung  |
|---------|---------|
|Es können nur Datasets verwendet werden, die für Blobdatenspeicher erstellt wurden.     |  Dies ist eine bekannte Einschränkung der aktuellen Version.       |
|Nach der Erstellung wird für das Projekt lange der Status „Wird initialisiert“ angezeigt.     | Aktualisieren Sie die Seite manuell. Die Initialisierung sollte mit ungefähr 20 Datenpunkten pro Sekunde durchgeführt werden. Die fehlende automatische Aktualisierung ist ein bekanntes Problem.         |
|Beim Überprüfen von Bildern werden neu bezeichnete Bilder nicht angezeigt.     |   Wählen Sie die Schaltfläche **Erste** aus, um alle bezeichneten Bilder zu laden. Mit der Schaltfläche **Erste** gelangen Sie zurück an den Anfang der Liste, aber es werden alle bezeichneten Daten geladen.      |
|Wird während der Erstellung von Bezeichnungen für die Objekterkennung ESC gedrückt, wird in der linken oberen Ecke eine Bezeichnung mit der Größe null erstellt. In diesem Fall ist die Übermittlung von Bezeichnungen nicht erfolgreich.     |   Löschen Sie die Bezeichnung, indem Sie auf das daneben angezeigte Kreuzsymbol klicken.  |

## <a name="azure-machine-learning-designer"></a>Azure Machine Learning-Designer

Bekannte Probleme:

* **Lange Compute-Vorbereitungszeit**: Es kann einige Minuten oder sogar länger dauern, wenn Sie zum ersten Mal eine Verbindung zu einem Computeziel herstellen oder ein Computeziel erstellen. 

## <a name="train-models"></a>Trainieren von Modellen

* **ModuleErrors (Kein Modul benannt)** :  Wenn Sie beim Übermitteln von Experimenten in Azure ML auf „ModuleErrors“ treffen, bedeutet dies, dass das Trainingsskript die Installation eines Pakets erwartet, es aber nicht hinzugefügt wird. Nachdem Sie den Paketnamen angegeben haben, installiert Azure ML das Paket in der für Ihr Training verwendeten Umgebung. 

    Wenn Sie [Estimators](concept-azure-machine-learning-architecture.md#estimators) verwenden, um Experimente zu übermitteln, können Sie einen Paketnamen über den Parameter `pip_packages` oder `conda_packages` im Estimator auf der Grundlage angeben, aus welcher Quelle Sie das Paket installieren möchten. Sie können auch eine YML-Datei mit allen Ihren Abhängigkeiten mit `conda_dependencies_file` angeben oder alle Ihre pip-Anforderungen in einer TXT-Datei mit dem Parameter `pip_requirements_file` auflisten. Wenn Sie Ihr eigenes Azure ML-Umgebungsobjekt besitzen, das das vom Estimator verwendete Standardimage außer Kraft setzen soll, können Sie diese Umgebung über den `environment`-Parameter des Estimatorkonstruktors angeben.

    Azure ML bietet auch frameworkspezifische Estimators für Tensorflow, PyTorch, Chainer und SKLearn. Die Verwendung dieser Estimators stellt sicher, dass die hauptsächlichen Frameworkabhängigkeiten in Ihrem Namen in der für das Training verwendeten Umgebung installiert werden. Sie haben die Möglichkeit, zusätzliche Abhängigkeiten wie oben beschrieben anzugeben. 
 
    Die von Azure ML verwalteten Docker-Images und deren Inhalt werden unter [AzureML-Container](https://github.com/Azure/AzureML-Containers) angezeigt.
    Frameworkspezifische Abhängigkeiten sind in der jeweiligen Dokumentation des Frameworks aufgeführt: [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py#remarks), [PyTorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py#remarks), [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py#remarks), [sklearn](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py#remarks).

    > [!Note]
    > Wenn Sie der Meinung sind, dass ein bestimmtes Paket häufig genug vorkommt, um in von Azure ML verwalteten Images und Umgebungen hinzugefügt zu werden, erstellen Sie ein GitHub-Problem unter [AzureML-Container](https://github.com/Azure/AzureML-Containers). 
 
* **NameError (Name nicht definiert), AttributeError (Objekt besitzt kein Attribut)** : Diese Ausnahme sollte von Ihren Trainingsskripts stammen. Sie können sich die Protokolldateien des Azure-Portals ansehen, um weitere Informationen über den nicht definierten Namen oder den Attributfehler zu erhalten. Aus dem SDK können Sie `run.get_details()` verwenden, um die Fehlermeldung anzuzeigen. Dadurch werden auch alle für Ihre Ausführung generierten Protokolldateien aufgelistet. Werfen Sie unbedingt einen Blick auf das Trainingsskript, und beheben Sie den Fehler, bevor Sie Ihren Durchlauf erneut übermitteln. 

* **Horovod wurde heruntergefahren**: In den meisten Fällen, wenn „AbortedError: Horovod wurde heruntergefahren“ auftritt, bedeutet dies, dass in einem der Prozesse eine Ausnahme aufgetreten ist, die Horovod zum Herunterfahren veranlasst hat. Jeder Rang im MPI-Auftrag erhält eine eigene dedizierte Protokolldatei in Azure ML. Diese Protokolle haben die Bezeichnung `70_driver_logs`. Im Falle von verteiltem Training werden die Protokollnamen durch das Suffix `_rank` ergänzt, um eine einfachere Unterscheidung der Protokolle zu ermöglichen. Um den genauen Fehler zu finden, der das Herunterfahren von Horovod verursacht hat, gehen Sie alle Protokolldateien durch, und suchen Sie am Ende der driver_log-Dateien nach `Traceback`. Eine dieser Dateien enthält die eigentliche zugrunde liegende Ausnahme. 

* **Löschen von Ausführungen oder Experimenten**:  Experimente können mit der Methode [Experiment.archive](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#archive--) oder über die Registerkartenansicht „Experiment“ im Azure Machine Learning Studio-Client über die Schaltfläche „Experiment archivieren“ archiviert werden. Durch diese Aktion wird das Experiment aus Abfragelisten und Ansichten ausgeblendet, aber nicht gelöscht.

    Das endgültige Löschen einzelner Experimente oder Ausführungen wird derzeit nicht unterstützt. Weitere Informationen zum Löschen von Arbeitsbereichsressourcen finden Sie unter [Exportieren oder Löschen Ihrer Arbeitsbereichsdaten im Machine Learning-Dienst](how-to-export-delete-data.md).

* **Metrikdokument ist zu groß**: Azure Machine Learning weist interne Beschränkungen der Größe von Metrikobjekten auf, die gleichzeitig aus einem Trainingsdurchlauf protokolliert werden können. Wenn bei der Protokollierung einer Metrik mit Listenwert der Fehler „Metrikdokument ist zu groß“ angezeigt wird, versuchen Sie, die Liste in kleinere Blöcke aufzuteilen, z. B.:

    ```python
    run.log_list("my metric name", my_metric[:N])
    run.log_list("my metric name", my_metric[N:])
    ```

    Intern verkettet Azure ML die Blöcke mit demselben Metriknamen zu einer zusammenhängenden Liste.

## <a name="automated-machine-learning"></a>Automatisiertes maschinelles Lernen

* **TensorFlow**: Das automatisierte maschinelle Lernen unterstützt derzeit nicht TensorFlow Version 1.13. Die Installation dieser Version führt dazu, dass Paketabhängigkeiten nicht mehr funktionieren. Wir arbeiten daran, dieses Problem in einer zukünftigen Version zu beheben.

* **Diagramm mit Experimenten**: Seit dem 12. April werden Diagramme für die binäre Klassifizierung (Genauigkeit und Trefferquote, ROC, Gewinnkurve usw.), die in automatisierten ML-Experimentiterationen angezeigt werden, auf der Benutzeroberfläche nicht richtig gerendert. In Diagrammen werden derzeit invertierte Ergebnisse angezeigt, was dazu führt, dass Modelle mit besserer Leistung mit schlechteren Ergebnissen angezeigt werden. Derzeit wird an einer Lösung gearbeitet.

* **Databricks – Abbrechen einer automatisierten Ausführung des maschinellen Lernens**: Wenn Sie Funktionen für automatisiertes maschinelles Lernen in Azure Databricks verwenden um eine Ausführung abzubrechen und eine neue Experimentausführung zu starten, starten Sie Ihren Azure Databricks-Cluster neu.

* **Databricks > Mehr als 10 Iterationen von automatisiertem maschinellem Lernen**: Sofern in den Einstellungen für automatisiertes maschinelles Lernen mehr als 10 Iterationen vorgesehen sind, legen Sie `show_output` auf `False` fest, wenn Sie die Ausführung übermitteln.

* **Databricks-Widget für das Azure Machine Learning SDK und automatisiertes maschinelles Lernen**: Das Azure Machine Learning SDK-Widget wird in Databricks-Notebooks nicht unterstützt, da die Notebooks keine HTML-Widgets analysieren können. Sie können das Widget im Portal anzeigen, indem Sie diesen Python-Code in die Zelle Ihres Azure Databricks-Notebooks einfügen:

    ```
    displayHTML("<a href={} target='_blank'>Azure Portal: {}</a>".format(local_run.get_portal_url(), local_run.id))
    ```

## <a name="deploy--serve-models"></a>Bereitstellen und Verarbeiten von Modellen

Führen Sie diese Aktionen für die folgenden Fehler aus:

|Fehler  | Lösung  |
|---------|---------|
|Fehler bei der Imageerstellung beim Bereitstellen des Webdiensts.     |  Fügen Sie „pynacl==1.2.1“ als pip-Anhängigkeit zur Conda-Datei für die Imagekonfiguration hinzu.       |
|`['DaskOnBatch:context_managers.DaskOnBatch', 'setup.py']' died with <Signals.SIGKILL: 9>`     |   Ändern Sie die SKU für virtuelle Computer in Ihrer Bereitstellung in eine Variante mit mehr Arbeitsspeicher. |
|FPGA-Fehler     |  Um Modelle auf FPGAs bereitzustellen, müssen Sie zuerst ein FPGA-Kontingent anfordern und dessen Genehmigung abwarten. Füllen Sie das Formular zur Kontingentanforderung aus, um Zugriff anzufordern: https://aka.ms/aml-real-time-ai       |

### <a name="updating-azure-machine-learning-components-in-aks-cluster"></a>Aktualisieren von Azure Machine Learning-Komponenten in einem AKS-Cluster

Updates für Azure Machine Learning-Komponenten, die in einem Azure Kubernetes Service-Cluster installiert sind, müssen manuell angewendet werden. 

Sie können diese Updates anwenden, indem Sie den Cluster vom Azure Machine Learning-Arbeitsbereich trennen und ihn dann erneut an den Arbeitsbereich anfügen. Wenn TLS im Cluster aktiviert ist, müssen Sie das TLS/SSL-Zertifikat und den privaten Schlüssel bereitstellen, wenn Sie den Cluster erneut anfügen. 

```python
compute_target = ComputeTarget(workspace=ws, name=clusterWorkspaceName)
compute_target.detach()
compute_target.wait_for_completion(show_output=True)

attach_config = AksCompute.attach_configuration(resource_group=resourceGroup, cluster_name=kubernetesClusterName)

## If SSL is enabled.
attach_config.enable_ssl(
    ssl_cert_pem_file="cert.pem",
    ssl_key_pem_file="key.pem",
    ssl_cname=sslCname)

attach_config.validate_configuration()

compute_target = ComputeTarget.attach(workspace=ws, name=args.clusterWorkspaceName, attach_configuration=attach_config)
compute_target.wait_for_completion(show_output=True)
```

Wenn Sie das TLS/SSL-Zertifikat und den privaten Schlüssel nicht mehr haben, oder wenn Sie ein von Azure Machine Learning generiertes Zertifikat verwenden, können Sie die Dateien vor dem Abtrennen des Clusters abrufen, indem Sie mit `kubectl` eine Verbindung mit dem Cluster herstellen und das Geheimnis `azuremlfessl` abrufen.

```bash
kubectl get secret/azuremlfessl -o yaml
```

>[!Note]
>Kubernetes speichert die Geheimnisse im Base64-codierten Format. Sie müssen die `cert.pem`- und die `key.pem`-Komponente jedes Geheimnisses aus dem Base64-Format decodieren, bevor Sie diese Komponenten für `attach_config.enable_ssl` bereitstellen. 

### <a name="webservices-in-azure-kubernetes-service-failures"></a>Webdienstefehler in Azure Kubernetes Service

Viele Webdienstefehler in Azure Kubernetes Service können debuggt werden, indem mit `kubectl` eine Verbindung mit dem Cluster hergestellt wird. Sie können die `kubeconfig.json`-Datei für einen Azure Kubernetes Service-Cluster abrufen, indem Sie folgenden Befehl ausführen:

```azurecli-interactive
az aks get-credentials -g <rg> -n <aks cluster name>
```

## <a name="authentication-errors"></a>Authentifizierungsfehler

Wenn Sie einen Verwaltungsvorgang innerhalb eines Remoteauftrags auf ein Computeziel anwenden, erhalten Sie eine der folgenden Fehlermeldungen: 

```json
{"code":"Unauthorized","statusCode":401,"message":"Unauthorized","details":[{"code":"InvalidOrExpiredToken","message":"The request token was either invalid or expired. Please try again with a valid token."}]}
```

```json
{"error":{"code":"AuthenticationFailed","message":"Authentication failed."}}
```

Sie erhalten beispielsweise eine Fehlermeldung, wenn Sie versuchen, ein Computeziel anhand einer ML-Pipeline zu erstellen oder anzuhängen, die zur Remoteausführung übermittelt wird.

## <a name="next-steps"></a>Nächste Schritte

Hier finden Sie weitere Artikel zur Problembehandlung in Azure Machine Learning:

* [Problembehandlung bei der Bereitstellung von Azure Machine Learning, Azure Kubernetes Service und Azure Container Instances](how-to-troubleshoot-deployment.md)
* [Debuggen und Problembehandlung für Machine Learning-Pipelines](how-to-debug-pipelines.md)
* [Debugging und Problembehandlung von ParallelRunStep](how-to-debug-parallel-run-step.md)
* [Interaktives Debuggen auf einer Azure Machine Learning Compute-Instanz mit VS Code Remote](how-to-set-up-vs-code-remote.md)
* [Debugging und Problembehandlung für Pipelines des maschinellen Lernens in Application Insights](how-to-debug-pipelines-application-insights.md)
