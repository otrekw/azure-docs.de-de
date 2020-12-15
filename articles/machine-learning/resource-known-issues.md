---
title: Bekannte Probleme und Problembehandlung
titleSuffix: Azure Machine Learning
description: Hier finden Sie Hilfe zum Suchen und Beheben von Fehlern in Azure Machine Learning. Erfahren Sie mehr über bekannte Probleme, Problembehandlung und Problemumgehungen.
services: machine-learning
author: likebupt
ms.author: keli19
ms.reviewer: mldocs
ms.service: machine-learning
ms.subservice: core
ms.topic: troubleshooting
ms.custom: troubleshooting, contperfq4
ms.date: 11/09/2020
ms.openlocfilehash: 55ac11b7888a8e351b52554f76fb44af35633c16
ms.sourcegitcommit: 8b4b4e060c109a97d58e8f8df6f5d759f1ef12cf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2020
ms.locfileid: "96780976"
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

   Im folgenden Code wird sowohl `azureml-datadrift` als auch `azureml-train-automl` über eine einzeilige pip-Installation installiert. 
     ```
       pip install azureml-datadrift, azureml-train-automl
     ```
   Für dieses Beispiel wird angenommen, dass `azureml-datadrift` eine Version > 1.0 und `azureml-train-automl` eine Version < 1.2 benötigt. Wenn die neueste Version von `azureml-datadrift` 1.3 ist, werden beide Pakete trotz der Paketanforderung von `azureml-train-automl` nach einer älteren Version auf 1.3 aktualisiert. 

   Um sicherzustellen, dass die richtigen Versionen für Ihre Pakete installiert werden, führen Sie die Installation in mehreren Zeilen durch, wie im folgenden Code. Die Reihenfolge ist hier kein Problem, da pip im Rahmen des nächsten Zeilenaufrufs explizit eine Herabstufung ausführt. Daher werden die richtigen Versionsabhängigkeiten angewandt.
    
     ```
        pip install azureml-datadrift
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
    
* **KeyError: 'brand' when running AutoML on local compute or Azure Databricks cluster**

    Wenn nach dem 10. Juni 2020 eine neue Umgebung mit der SDK-Version 1.7.0 oder früher erstellt wurde, kann dieser Fehler beim Training aufgrund eines Updates im py-cpuinfo-Pakets auftreten. (Umgebungen, die bis einschließlich 10. Juni 2020 erstellt wurden, sind davon nicht betroffen. Gleiches gilt für Experimente, die auf Remotecomputern ausgeführt werden, weil zwischengespeicherte Trainingsbilder verwendet werden.) Führen Sie einen der folgenden beiden Schritte aus, um das Problem zu umgehen:
    
    * Aktualisieren Sie die SDK-Version auf 1.8.0 oder höher (damit wird auch py-cpuinfo auf 5.0.0 herabgestuft):
    
      ```bash
      pip install --upgrade azureml-sdk[automl]
      ```
    
    * Stufen Sie die installierte Version von py-cpuinfo auf 5.0.0 herab:
    
      ```bash
      pip install py-cpuinfo==5.0.0
      ```
  
* **Fehlermeldung: 'PyYAML' kann nicht deinstalliert werden.**

    Azure Machine Learning SDK für Python: PyYAML ist ein von `distutils` installiertes Projekt. Daher können wir bei einer teilweisen Deinstallation nicht genau bestimmen, welche Dateien zu ihm gehören. Verwenden Sie diesen Befehl, um die Installation des SDKs fortzusetzen und diesen Fehler zu ignorieren:
    
    ```Python
    pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
    ```

* **Fehler bei der Azure Machine Learning SDK-Installation mit einer Ausnahme: ModuleNotFoundError: Kein Modul namens 'ruamel' oder 'ImportError: Kein Modul namens ruamel.yaml'**
   
   Dieses Problem wird durch die Installation des Azure Machine Learning SDK für Python für die neueste pip-Version (> 20.1.1) in der conda-Basisumgebung für alle veröffentlichten Versionen des Azure Machine Learning SDK für Python behoben. Verwenden Sie die folgenden Problemumgehungen:

    * Vermeiden Sie die Installation des Python SDK in der conda-Basisumgebung. Erstellen Sie stattdessen Ihre conda-Umgebung, und installieren Sie das SDK in dieser neu erstellten Benutzerumgebung. Die neueste pip-Version sollte in dieser neuen conda-Umgebung funktionieren.

    * Wenn Sie beim Erstellen von Images in Docker nicht aus der conda-Basisumgebung wechseln können, binden Sie pip<=20.1.1 in die Docker-Datei ein.

    ```Python
    conda install -c r -y conda python=3.6.2 pip=20.1.1
    ```
    
* **Databricks-Fehler beim Installieren von Paketen**

    Bei der Installation des Azure Machine Learning SDK tritt in Azure Databricks ein Fehler auf, wenn mehrere Pakete installiert werden. Einige Pakete, z.B. `psutil`, können Konflikte verursachen. Um Fehler bei der Installation zu vermeiden, frieren Sie die Bibliotheksversion beim Installieren der Pakete ein. Dieses Problem hängt mit Databricks und nicht mit dem Azure Machine Learning SDK zusammen. Es kann auch mit anderen Bibliotheken auftreten. Beispiel:
    
    ```python
    psutil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
    ```

    Falls bei Python-Bibliotheken immer wieder Installationsprobleme auftreten, können Sie alternativ Initialisierungsskripts verwenden. Dieser Ansatz wird nicht offiziell unterstützt. Weitere Informationen finden Sie unter [Initialisierungsskripts im Clusterbereich](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

* **Databricks-Importfehler: Der Name `Timedelta` kann nicht aus `pandas._libs.tslibs` importiert werden**: Wenn dieser Fehler angezeigt wird, wenn Sie automatisiertes Machine Learning verwenden, führen Sie die beiden folgenden Zeilen in Ihrem Notebook aus:
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

* **Azure-Portal**: 
  * Wenn Sie direkt über einen Freigabelink aus dem SDK oder dem Azure-Portal zu Ihrem Arbeitsbereich gelangen, können Sie die standardmäßige **Übersichtsseite** mit Abonnementinformationen in der Erweiterung nicht anzeigen. In diesem Szenario können Sie auch nicht zu einem anderen Arbeitsbereich wechseln. Um einen anderen Arbeitsbereich anzuzeigen, wechseln Sie direkt zu [Azure Machine Learning-Studio](https://ml.azure.com) und suchen nach dem Namen des Arbeitsbereichs.
  * Alle Assets (Datasets, Experimente, Computes usw.) sind nur in [Azure Machine Learning Studio](https://ml.azure.com) verfügbar. Sie sind *nicht* über das Azure-Portal verfügbar.

* **Unterstützte Browser im Azure Machine Learning Studio-Webportal**: Es wird empfohlen, den neuesten Browser zu verwenden, der mit Ihrem Betriebssystem kompatibel ist. Die folgenden Browser werden unterstützt:
  * Microsoft Edge (Die neueste Version von Microsoft Edge. Keine ältere Microsoft Edge-Version.)
  * Safari (neueste Version, nur auf Mac)
  * Chrome (neueste Version)
  * Firefox (neueste Version)

## <a name="set-up-your-environment"></a>Einrichten Ihrer Umgebung

* **Probleme beim Erstellen von AmlCompute**: In seltenen Fällen kann es vorkommen, dass Benutzer, die vor dem allgemein verfügbaren Release ihren Azure Machine Learning-Arbeitsbereich über das Azure-Portal erstellt haben, AmlCompute nicht in diesem Arbeitsbereich erstellen können. In einem solchen Fall können Sie entweder eine Supportanfrage für den Dienst erstellen oder über das Portal oder das SDK einen neuen Arbeitsbereich erstellen und die Blockierung dadurch umgehend aufheben.

* **Azure Container Registry unterstützt derzeit keine Unicode-Zeichen in Ressourcengruppennamen:** Möglicherweise treten bei ACR-Anforderungen Fehler auf, da der zugehörige Ressourcengruppenname Unicode-Zeichen enthält. Um dieses Problem zu beheben, wird empfohlen, eine ACR-Instanz in einer Ressourcengruppe mit abweichendem Namen zu erstellen.

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

### <a name="mount-dataset"></a>Einbinden eines Datasets
* **Dataset initialization failed:  Waiting for mount point to be ready has timed out** (Fehler bei der Initialisierung des Datasets: Timeout beim Warten auf Verfügbarkeit des Bereitstellungpunkts): 
  * Wenn Sie keine Ausgangsregeln für [Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/network-security-groups-overview) haben und `azureml-sdk>=1.12.0` verwenden, aktualisieren Sie `azureml-dataset-runtime` und die zugehörigen Abhängigkeiten auf die neueste Version für die jeweilige Nebenversion. Wenn Sie das SDK in einer Ausführung verwenden, erstellen Sie die Umgebung neu, damit sie über den neuesten Patch mit dem Fix verfügt. 
  * Wenn Sie `azureml-sdk<1.12.0` verwenden, führen Sie ein Upgrade auf die neueste Version aus.
  * Wenn Sie über Ausgangsregeln für Netzwerksicherheitsgruppen verfügen, stellen Sie sicher, dass eine Ausgangsregel vorhanden ist, die sämtlichen Datenverkehr für das Diensttag `AzureResourceMonitor` zulässt.

### <a name="data-labeling-projects"></a>Datenbezeichnungsprojekte

|Problem  |Lösung  |
|---------|---------|
|Es können nur Datasets verwendet werden, die für Blobdatenspeicher erstellt wurden.     |  Dies ist eine bekannte Einschränkung der aktuellen Version.       |
|Nach der Erstellung wird für das Projekt lange der Status „Wird initialisiert“ angezeigt.     | Aktualisieren Sie die Seite manuell. Die Initialisierung sollte mit ungefähr 20 Datenpunkten pro Sekunde durchgeführt werden. Die fehlende automatische Aktualisierung ist ein bekanntes Problem.         |
|Beim Überprüfen von Bildern werden neu bezeichnete Bilder nicht angezeigt.     |   Wählen Sie die Schaltfläche **Erste** aus, um alle bezeichneten Bilder zu laden. Mit der Schaltfläche **Erste** gelangen Sie zurück an den Anfang der Liste, aber es werden alle bezeichneten Daten geladen.      |
|Wird während der Erstellung von Bezeichnungen für die Objekterkennung ESC gedrückt, wird in der linken oberen Ecke eine Bezeichnung mit der Größe null erstellt. In diesem Fall ist die Übermittlung von Bezeichnungen nicht erfolgreich.     |   Löschen Sie die Bezeichnung, indem Sie auf das daneben angezeigte Kreuzsymbol klicken.  |

### <a name="data-drift-monitors"></a><a name="data-drift"></a> Datendriftüberwachungen

Einschränkungen und bekannte Probleme bei Datendriftüberwachungen:

* Der Zeitbereich für die Analyse von Verlaufsdaten ist auf 31 Intervalle der Häufigkeitseinstellung für die Überwachung beschränkt. 
* Es besteht eine Beschränkung auf 200 Features, es sei denn, es wurde keine Featureliste angegeben (alle Features werden verwendet).
* Die Computegröße muss ausreichend sein, um die Daten zu verarbeiten.
* Stellen Sie sicher, dass Ihr Dataset über Daten innerhalb des Zeitraums verfügt, der durch das Start- und Enddatum für die jeweilige Ausführung der Überwachung festgelegt ist.
* Datasetmonitore funktionieren nur bei Datensätzen, die mindestens 50 Zeilen enthalten.
* Spalten bzw. Features im Dataset werden basierend auf den Bedingungen in der unten angegebenen Tabelle als kategorisch oder numerisch klassifiziert. Wenn ein Feature diese Bedingungen nicht erfüllt – beispielsweise bei einer Spalte vom Typ „string“ mit mehr als 100 eindeutigen Werten –, wird es aus dem Datendriftalgorithmus entfernt. In die Profilerstellung wird es aber einbezogen. 

    | Featuretyp | Datentyp | Bedingung | Einschränkungen | 
    | ------------ | --------- | --------- | ----------- |
    | Kategorisch | string, bool, int, float | Die Anzahl von eindeutigen Werten im Feature ist kleiner als 100 und geringer als 5 % der Anzahl von Zeilen. | NULL wird als eigene Kategorie behandelt. | 
    | Numerisch | int, float | Die Werte im Feature weisen einen numerischen Datentyp auf und erfüllen die Bedingung für ein kategorisches Feature nicht. | Das Feature wird entfernt, wenn mehr als 15 % der Werte NULL sind. | 

* Wenn Sie [eine Datendriftüberwachung erstellt haben](how-to-monitor-datasets.md), die Daten auf der Seite **Datasetüberwachungen** in Azure Machine Learning Studio jedoch nicht angezeigt werden, versuchen Sie Folgendes.

    1. Überprüfen Sie, ob Sie oben auf der Seite den richtigen Datumsbereich ausgewählt haben.  
    1. Wählen Sie auf der Registerkarte **Datasetüberwachungen** den Experimentlink aus, um den Ausführungsstatus zu prüfen.  Dieser Link befindet sich ganz rechts in der Tabelle.
    1. Wenn die Ausführung erfolgreich abgeschlossen wurde, überprüfen Sie in den Treiberprotokollen, wie viele Metriken generiert wurden oder ob Warnmeldungen vorhanden sind.  Die Treiberprotokolle finden Sie nach dem Klicken auf ein Experiment auf der Registerkarte **Ausgabe + Protokolle**.

* Wenn die SDK-Funktion `backfill()` nicht die erwartete Ausgabe generiert, kann dies auf ein Authentifizierungsproblem zurückzuführen sein.  Verwenden Sie beim Erstellen des Computeziels, das an diese Funktion übergeben wird, nicht `Run.get_context().experiment.workspace.compute_targets`.  Verwenden Sie stattdessen [ServicePrincipalAuthentication](/python/api/azureml-core/azureml.core.authentication.serviceprincipalauthentication?preserve-view=true&view=azure-ml-py), wie hier zu sehen, um das Computeziel zu erstellen, das Sie an die Funktion `backfill()` übergeben: 

  ```python
   auth = ServicePrincipalAuthentication(
          tenant_id=tenant_id,
          service_principal_id=app_id,
          service_principal_password=client_secret
          )
   ws = Workspace.get("xxx", auth=auth, subscription_id="xxx", resource_group"xxx")
   compute = ws.compute_targets.get("xxx")
   ```

## <a name="azure-machine-learning-designer"></a>Azure Machine Learning-Designer

### <a name="dataset-visualization-in-the-designer"></a>Datasetvisualisierung im Designer

Nachdem Sie ein Dataset auf der Ressourcenseite **Datasets** oder mithilfe des SDK registriert haben, finden Sie es in der Liste links im Designer unter der Kategorie **Datasets**.

Wenn Sie das Dataset jedoch in den Zeichenbereich ziehen und visualisieren, kann es möglicherweise aus folgenden Gründen nicht visualisiert werden:

- Derzeit können nur tabellarische Datasets im Designer visualisiert werden. Wenn Sie ein Dateidataset außerhalb des Designers registrieren, können Sie es nicht im Designer-Zeichenbereich visualisieren.
- Das Dataset ist im virtuellen Netzwerk (VNET) gespeichert. Zum Visualisieren müssen Sie die vom Arbeitsbereich verwaltete Identität des Datenspeichers aktivieren.
    1. Rufen Sie den entsprechenden Datenspeicher auf, und klicken Sie auf **Anmeldeinformationen aktualisieren**
    :::image type="content" source="./media/resource-known-issues/datastore-update-credential.png" alt-text="Anmeldeinformationen aktualisieren":::.
    1. Wählen Sie **Ja** aus, um die vom Arbeitsbereich verwaltete Identität zu aktivieren.
    :::image type="content" source="./media/resource-known-issues/enable-workspace-managed-identity.png" alt-text="Aktivieren der vom Arbeitsbereich verwalteten Identität":::

### <a name="long-compute-preparation-time"></a>Lange Compute-Vorbereitungszeit

Es kann einige Minuten oder sogar länger dauern, wenn Sie zum ersten Mal eine Verbindung zu einem Computeziel herstellen oder ein Computeziel erstellen. 

Es kann bis zu 10 Minuten dauern, bis Daten aus dem Modelldatensammler in Ihrem Blobspeicherkonto eintreffen (in der Regel wird der Vorgang schneller ausgeführt). Warten Sie 10 Minuten, um sicherzustellen, dass die folgenden Zellen ausgeführt werden.

```python
import time
time.sleep(600)
```

### <a name="log-for-real-time-endpoints"></a>Protokoll für Echtzeitendpunkte

Protokolle von Echtzeitendpunkten sind Kundendaten. Für die Problembehandlung für Echtzeitendpunkte können Sie den folgenden Code zum Aktivieren von Protokollen verwenden. 

Weitere Informationen zur Überwachung von Webdienst-Endpunkten finden Sie in [diesem Artikel](./how-to-enable-app-insights.md#query-logs-for-deployed-models).

```python
from azureml.core import Workspace
from azureml.core.webservice import Webservice

ws = Workspace.from_config()
service = Webservice(name="service-name", workspace=ws)
logs = service.get_logs()
```
Wenn Sie über mehrere Mandanten verfügen, müssen Sie vor `ws = Workspace.from_config()` ggf. den folgenden Code für die Authentifizierung hinzufügen:

```python
from azureml.core.authentication import InteractiveLoginAuthentication
interactive_auth = InteractiveLoginAuthentication(tenant_id="the tenant_id in which your workspace resides")
```

## <a name="train-models"></a>Trainieren von Modellen

* **ModuleErrors (Kein Modul benannt)** :  Wenn Sie beim Übermitteln von Experimenten in Azure ML auf „ModuleErrors“ treffen, bedeutet dies, dass das Trainingsskript die Installation eines Pakets erwartet, es aber nicht hinzugefügt wird. Nachdem Sie den Paketnamen angegeben haben, installiert Azure ML das Paket in der für Ihr Training verwendeten Umgebung. 

    Wenn Sie Estimators verwenden, um Experimente zu übermitteln, können Sie einen Paketnamen über den Parameter `pip_packages` oder `conda_packages` im Estimator auf der Grundlage angeben, aus welcher Quelle Sie das Paket installieren möchten. Sie können auch eine YML-Datei mit allen Ihren Abhängigkeiten mit `conda_dependencies_file` angeben oder alle Ihre pip-Anforderungen in einer TXT-Datei mit dem Parameter `pip_requirements_file` auflisten. Wenn Sie Ihr eigenes Azure ML-Umgebungsobjekt besitzen, das das vom Estimator verwendete Standardimage außer Kraft setzen soll, können Sie diese Umgebung über den `environment`-Parameter des Estimatorkonstruktors angeben.

    Azure ML bietet auch frameworkspezifische Estimators für Tensorflow, PyTorch, Chainer und SKLearn. Die Verwendung dieser Estimators stellt sicher, dass die hauptsächlichen Frameworkabhängigkeiten in Ihrem Namen in der für das Training verwendeten Umgebung installiert werden. Sie haben die Möglichkeit, zusätzliche Abhängigkeiten wie oben beschrieben anzugeben. 
 
    Die von Azure ML verwalteten Docker-Images und deren Inhalt werden unter [AzureML-Container](https://github.com/Azure/AzureML-Containers) angezeigt.
    Frameworkspezifische Abhängigkeiten sind in der jeweiligen Dokumentation des Frameworks aufgeführt: [Chainer](/python/api/azureml-train-core/azureml.train.dnn.chainer?preserve-view=true&view=azure-ml-py#&preserve-view=trueremarks), [PyTorch](/python/api/azureml-train-core/azureml.train.dnn.pytorch?preserve-view=true&view=azure-ml-py#&preserve-view=trueremarks), [TensorFlow](/python/api/azureml-train-core/azureml.train.dnn.tensorflow?preserve-view=true&view=azure-ml-py#&preserve-view=trueremarks), [sklearn](/python/api/azureml-train-core/azureml.train.sklearn.sklearn?preserve-view=true&view=azure-ml-py#&preserve-view=trueremarks).

    > [!Note]
    > Wenn Sie der Meinung sind, dass ein bestimmtes Paket häufig genug vorkommt, um in von Azure ML verwalteten Images und Umgebungen hinzugefügt zu werden, erstellen Sie ein GitHub-Problem unter [AzureML-Container](https://github.com/Azure/AzureML-Containers). 
 
* **NameError (Name nicht definiert), AttributeError (Objekt besitzt kein Attribut)** : Diese Ausnahme sollte von Ihren Trainingsskripts stammen. Sie können sich die Protokolldateien des Azure-Portals ansehen, um weitere Informationen über den nicht definierten Namen oder den Attributfehler zu erhalten. Aus dem SDK können Sie `run.get_details()` verwenden, um die Fehlermeldung anzuzeigen. Dadurch werden auch alle für Ihre Ausführung generierten Protokolldateien aufgelistet. Werfen Sie unbedingt einen Blick auf das Trainingsskript, und beheben Sie den Fehler, bevor Sie Ihren Durchlauf erneut übermitteln. 

* **Horovod wurde heruntergefahren**: In den meisten Fällen, wenn „AbortedError: Horovod wurde heruntergefahren“ auftritt, bedeutet dies, dass in einem der Prozesse eine Ausnahme aufgetreten ist, die Horovod zum Herunterfahren veranlasst hat. Jeder Rang im MPI-Auftrag erhält eine eigene dedizierte Protokolldatei in Azure ML. Diese Protokolle haben die Bezeichnung `70_driver_logs`. Im Falle von verteiltem Training werden die Protokollnamen durch das Suffix `_rank` ergänzt, um eine einfachere Unterscheidung der Protokolle zu ermöglichen. Um den genauen Fehler zu finden, der das Herunterfahren von Horovod verursacht hat, gehen Sie alle Protokolldateien durch, und suchen Sie am Ende der driver_log-Dateien nach `Traceback`. Eine dieser Dateien enthält die eigentliche zugrunde liegende Ausnahme. 

* **Löschen von Ausführungen oder Experimenten**:  Experimente können mit der Methode [Experiment.archive](/python/api/azureml-core/azureml.core.experiment%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=truearchive--) oder über die Registerkartenansicht „Experiment“ im Azure Machine Learning Studio-Client über die Schaltfläche „Experiment archivieren“ archiviert werden. Durch diese Aktion wird das Experiment aus Abfragelisten und Ansichten ausgeblendet, aber nicht gelöscht.

    Das endgültige Löschen einzelner Experimente oder Ausführungen wird derzeit nicht unterstützt. Weitere Informationen zum Löschen von Arbeitsbereichsressourcen finden Sie unter [Exportieren oder Löschen Ihrer Arbeitsbereichsdaten im Machine Learning-Dienst](how-to-export-delete-data.md).

* **Metrikdokument ist zu groß**: Azure Machine Learning weist interne Beschränkungen der Größe von Metrikobjekten auf, die gleichzeitig aus einem Trainingsdurchlauf protokolliert werden können. Wenn bei der Protokollierung einer Metrik mit Listenwert der Fehler „Metrikdokument ist zu groß“ angezeigt wird, versuchen Sie, die Liste in kleinere Blöcke aufzuteilen, z. B.:

    ```python
    run.log_list("my metric name", my_metric[:N])
    run.log_list("my metric name", my_metric[N:])
    ```

    Intern verkettet Azure ML die Blöcke mit demselben Metriknamen zu einer zusammenhängenden Liste.

## <a name="automated-machine-learning"></a>Automatisiertes maschinelles Lernen

* **Das letzte Upgrade der AutoML-Abhängigkeiten auf neuere Versionen führt zu Inkompatibilität**:  Ab Version 1.13.0 des SDK werden Modelle in älteren SDKs nicht mehr geladen, da die in den früheren Paketen angehefteten Versionen mit den neueren, jetzt angehefteten Versionen nicht kompatibel sind. Es wird ein Fehler wie der folgende angezeigt:
  * Das Modul wurde nicht gefunden: Beispiel: `No module named 'sklearn.decomposition._truncated_svd`,
  * Importfehler: Beispiel: `ImportError: cannot import name 'RollingOriginValidator'`,
  * Attributfehler: Ex. `AttributeError: 'SimpleImputer' object has no attribute 'add_indicator`
  
  Um dieses Problem zu umgehen, führen Sie je nach Ihrer AutoML SDK-Trainingsversion einen der beiden folgenden Schritte aus:
  1. Wenn Ihre AutoML SDK-Trainingsversion größer als 1.13.0 ist, benötigen Sie `pandas == 0.25.1` und `sckit-learn==0.22.1`. Wenn die Version nicht übereinstimmt, führen Sie ein Upgrade von scikit-learn oder Pandas auf die richtige Version durch, wie nachstehend dargestellt:
  
  ```bash
     pip install --upgrade pandas==0.25.1
     pip install --upgrade scikit-learn==0.22.1
  ```
  
  2. Wenn Ihre AutoML SDK-Trainingsversion kleiner als oder gleich 1.12.0 ist, benötigen Sie `pandas == 0.23.4` und `sckit-learn==0.20.3`. Wenn die Version nicht übereinstimmt, führen Sie ein Downgrade von scikit-learn oder Pandas auf die richtige Version durch, wie nachstehend dargestellt:
  
  ```bash
    pip install --upgrade pandas==0.23.4
    pip install --upgrade scikit-learn==0.20.3
  ```
 
* **Der Prognose-R2-Score ist immer 0 (null)** : Dieses Problem tritt auf, wenn die bereitgestellten Trainingsdaten Zeitreihen enthalten, die für die letzten `n_cv_splits` + `forecasting_horizon` Datenpunkte den gleichen Wert aufweisen. Wenn dieses Muster in Ihrer Zeitreihe erwartet wird, können Sie die primäre Metrik in die normalisierte mittlere quadratische Gesamtabweichung ändern.
 
* **TensorFlow**: Ab der SDK-Version 1.5.0 installiert das automatisierte maschinelle Lernen TensorFlow-Modelle nicht mehr standardmäßig. Wenn Sie TensorFlow installieren und bei Ihren automatisierten ML-Experimenten verwenden möchten, installieren Sie „tensorflow==1.12.0“ über CondaDependecies. 
 
   ```python
   from azureml.core.runconfig import RunConfiguration
   from azureml.core.conda_dependencies import CondaDependencies
   run_config = RunConfiguration()
   run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['tensorflow==1.12.0'])
  ```
* **Diagramm mit Experimenten**: Seit dem 12. April werden Diagramme für die binäre Klassifizierung (Genauigkeit und Trefferquote, ROC, Gewinnkurve usw.), die in automatisierten ML-Experimentiterationen angezeigt werden, auf der Benutzeroberfläche nicht richtig gerendert. In Diagrammen werden derzeit invertierte Ergebnisse angezeigt, was dazu führt, dass Modelle mit besserer Leistung mit schlechteren Ergebnissen angezeigt werden. Derzeit wird an einer Lösung gearbeitet.

* **Databricks – Abbrechen einer automatisierten Ausführung des maschinellen Lernens**: Wenn Sie Funktionen für automatisiertes maschinelles Lernen in Azure Databricks verwenden um eine Ausführung abzubrechen und eine neue Experimentausführung zu starten, starten Sie Ihren Azure Databricks-Cluster neu.

* **Databricks > Mehr als 10 Iterationen von automatisiertem maschinellem Lernen**: Sofern in den Einstellungen für automatisiertes maschinelles Lernen mehr als 10 Iterationen vorgesehen sind, legen Sie `show_output` auf `False` fest, wenn Sie die Ausführung übermitteln.

* **Databricks-Widget für das Azure Machine Learning SDK und automatisiertes maschinelles Lernen**: Das Azure Machine Learning SDK-Widget wird in Databricks-Notebooks nicht unterstützt, da die Notebooks keine HTML-Widgets analysieren können. Sie können das Widget im Portal anzeigen, indem Sie diesen Python-Code in die Zelle Ihres Azure Databricks-Notebooks einfügen:

    ```
    displayHTML("<a href={} target='_blank'>Azure Portal: {}</a>".format(local_run.get_portal_url(), local_run.id))
    ```
* **Fehler bei „automl_setup“** : 
    * Unter Windows führen Sie „automl_setup“ von einer Anaconda-Eingabeaufforderung aus. Verwenden Sie diesen Link, um [Miniconda zu installieren](https://docs.conda.io/en/latest/miniconda.html).
    * Stellen Sie sicher, dass die 64-Bit-Version von Conda installiert ist, nicht die32-Bit-Version, indem Sie den Befehl `conda info` ausführen. `platform` sollte für Windows `win-64` und für Mac `osx-64` lauten.
    * Stellen Sie sicher, dass Conda 4.4.10 oder höher installiert ist. Sie können die Version mit dem Befehl `conda -V` überprüfen. Wenn eine frühere Version installiert ist, können Sie diese mit dem folgenden Befehl aktualisieren: `conda update conda`.
    * Linux: `gcc: error trying to exec 'cc1plus'`
      *  Wenn der Fehler `gcc: error trying to exec 'cc1plus': execvp: No such file or directory` auftritt, installieren Sie das „build-essential“-Paket über den Befehl `sudo apt-get install build-essential`.
      * Übergeben Sie einen neuen Namen als ersten Parameter an „automl_setup“, um eine neue Conda-Umgebung zu erstellen. Vorhandene Conda-Umgebungen können Sie mit `conda env list` anzeigen und mit `conda env remove -n <environmentname>` entfernen.
      
* **Fehler bei „automl_setup_linux.sh“** : Gehen Sie folgendermaßen vor, wenn bei „automl_setup_linus.sh“ unter Ubuntu Linux folgender Fehler auftritt: `unable to execute 'gcc': No such file or directory`-
  1. Stellen Sie sicher, dass die ausgehenden Ports 53 und 80 aktiviert sind. Auf einer Azure-VM können Sie zu diesem Zweck zum Azure-Portal wechseln, die VM auswählen und auf „Netzwerk“ klicken.
  2. Führen Sie den folgenden Befehl aus: `sudo apt-get update`
  3. Führen Sie den folgenden Befehl aus: `sudo apt-get install build-essential --fix-missing`
  4. Führen Sie `automl_setup_linux.sh` erneut aus.

* **Fehler bei „configuration.ipynb“** :
  * Stellen Sie bei einer lokalen Conda-Instanz zunächst sicher, dass „automl_setup“ erfolgreich ausgeführt wurde.
  * Stellen Sie sicher, dass die „subscription_id“ richtig ist. Suchen Sie die „subscription_id“ im Azure-Portal, indem Sie „Alle Dienste“ und dann „Abonnements“ auswählen. Die Zeichen „<“ und „>“ dürfen im subscription_id-Wert nicht enthalten sein. Dies ist ein Beispiel für ein gültiges Format: `subscription_id = "12345678-90ab-1234-5678-1234567890abcd"`.
  * Stellen Sie sicher, dass Sie Zugriff als Besitzer oder Mitwirkender auf das Abonnement haben.
  * Überprüfen Sie, ob die verwendete Region unterstützt wird: `eastus2`, `eastus`, `westcentralus`, `southeastasia`, `westeurope`, `australiaeast`, `westus2`, `southcentralus`.
  * Stellen Sie über das Azure-Portal sicher, dass Zugriff auf die Region besteht.
  
* **Fehler beim Import von „AutoMLConfig“** : In Version 1.0.76 des automatisierten maschinellen Lernens gab es Paketänderungen. Daher muss die vorherige Version deinstalliert werden, bevor die neue Version installiert werden kann. Wenn nach dem Upgrade einer SDK-Version vor v1.0.76 auf v1.0.76 oder höher der Fehler `ImportError: cannot import name AutoMLConfig` auftritt, führen Sie erst `pip uninstall azureml-train automl` und dann `pip install azureml-train-auotml` aus. Das Skript „automl_setup.cmd“ erledigt dies automatisch. 

* **Fehler bei „workspace.from_config“** : Wenn beim Aufruf „ws = Workspace.from_config()“ ein Fehler auftritt, gehen Sie folgendermaßen vor:
  1. Stellen Sie sicher, dass das Notebook „configuration.ipynb“ erfolgreich ausgeführt wurde.
  2. Wenn das Notebook in einem Ordner ausgeführt werden soll, der sich nicht in dem Ordner befindet, in dem `configuration.ipynb` ausgeführt wurde, kopieren Sie den Ordner „aml_config“ und die darin enthaltene Datei „config.json“ in den neuen Ordner. „Workspace.from_config“ liest die Datei „config.json“ für den Notebookordner oder den übergeordneten Ordner.
  3. Wenn ein neues Abonnement, eine neue Ressourcengruppe, ein neuer Arbeitsbereich oder eine neue Region verwendet wird, stellen Sie sicher, dass Sie das Notebook `configuration.ipynb` erneut ausführen. Eine direkte Änderung von „config.json“ funktioniert nur, wenn der Arbeitsbereich bereits in der angegebenen Ressourcengruppe im angegebenen Abonnement vorhanden ist.
  4. Wenn Sie die Region ändern möchten, ändern Sie den Arbeitsbereich, die Ressourcengruppe oder das Abonnement. `Workspace.create` erstellt keinen Arbeitsbereich und aktualisiert auch keinen vorhandenen Arbeitsbereich, auch dann nicht, wenn eine andere Region angegeben wird.
  
* **Fehler im Beispielnotebook**: Wenn bei einem Beispielnotebook ein Fehler aufgrund einer fehlenden Eigenschaft, Methode oder Bibliothek auftritt, gehen Sie folgendermaßen vor:
  * Stellen Sie sicher, dass im Jupyter-Notebook der richtige Kernel ausgewählt wurde. Der Kernel wird oben rechts auf der Seite des Notebooks angezeigt. Der Standardwert lautet „azure_automl“. Beachten Sie, dass der Kernel als Teil des Notebooks gespeichert wird. Wenn Sie zu einer neuen Conda-Umgebung wechseln, müssen Sie den neuen Kernel im Notebook auswählen.
      * Bei Azure Notebooks sollte Python 3.6 verwendet werden. 
      * Bei lokalen Conda-Umgebungen sollte es sich um den Namen der Conda-Umgebung handeln, die Sie in „automl_setup“ angegeben haben.
  * Stellen Sie sicher, dass das Notebook auf die von Ihnen verwendete SDK-Version ausgelegt ist. Sie können die SDK-Version überprüfen, indem Sie `azureml.core.VERSION` in einer Jupyter-Notebook-Zelle ausführen. Wenn Sie vorherige Versionen der Beispielnotebooks von GitHub herunterladen möchten, klicken Sie auf die Schaltfläche `Branch` und wählen dann die Registerkarte `Tags` und dort die Version aus.

* **Fehler beim NumPy-Import in Windows**: In einigen Windows-Umgebungen tritt beim Laden von NumPy mit der neuesten Python-Version 3.6.8 ein Fehler auf. Versuchen Sie in diesem Fall die Python-Version 3.6.7.

* **Fehler beim NumPy-Import**: Überprüfen Sie die TensorFlow-Version in der Conda-Umgebung für automatisiertes ML. Es werden Versionen unter 1.13 unterstützt. Wenn die vorhandene Version 1.13 oder höher ist, deinstallieren Sie TensorFlow in der Umgebung. Sie können die TensorFlow-Version folgendermaßen überprüfen und deinstallieren:
  1. Starten Sie eine Befehlsshell, und aktivieren Sie die Conda-Umgebung, in der die Pakete für das automatisierte maschinelle Lernen installiert sind.
  2. Geben Sie `pip freeze` ein, und suchen Sie nach `tensorflow`. Die aufgelistete Version sollte kleiner als 1.13 sein.
  3. Wenn die aufgelistete Version keine unterstützte Version ist, geben Sie in der Befehlsshell `pip uninstall tensorflow` ein, und bestätigen Sie den Befehl mit „y“.

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

### <a name="detaching-azure-kubernetes-service"></a>Trennen von Azure Kubernetes Service

Wenn Sie einen AKS-Cluster mit dem Azure Machine Learning Studio, dem SDK oder der Azure CLI-Erweiterung für maschinelles Lernen trennen, wird der AKS-Cluster nicht gelöscht. Informationen zum Löschen des Clusters finden Sie unter [Schnellstart: Bereitstellen eines Azure Kubernetes Service-Clusters über die Azure-Befehlszeilenschnittstelle](../aks/kubernetes-walkthrough.md#delete-the-cluster).

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

## <a name="missing-user-interface-items-in-studio"></a>Fehlende Benutzeroberflächenelemente in Studio

Die rollenbasierte Zugriffssteuerung in Azure kann verwendet werden, um Aktionen einzuschränken, die Sie mit Azure Machine Learning durchführen können. Diese Einschränkungen können verhindern, dass Benutzeroberflächenelemente in Azure Machine Learning Studio angezeigt werden. Wenn Ihnen z. B. eine Rolle zugewiesen wurde, mit der Sie keine Compute-Instanz erstellen können, wird die Option zum Erstellen einer Compute-Instanz nicht in Studio angezeigt.

Weitere Informationen finden Sie unter [Verwalten von Benutzern und Rollen](how-to-assign-roles.md).

## <a name="compute-cluster-wont-resize"></a>Die Größe des Computeclusters ändert sich nicht

Wenn Ihr Azure Machine Learning-Computecluster bei der Größenänderung bei (0 > 0) für den Knotenstatus stehenzubleiben scheint, wird dies möglicherweise von Azure-Ressourcensperren verursacht.

[!INCLUDE [resource locks](../../includes/machine-learning-resource-lock.md)]

## <a name="next-steps"></a>Nächste Schritte

Hier finden Sie weitere Artikel zur Problembehandlung in Azure Machine Learning:

* [Problembehandlung bei der Bereitstellung von Azure Machine Learning, Azure Kubernetes Service und Azure Container Instances](how-to-troubleshoot-deployment.md)
* [Debuggen und Problembehandlung für Machine Learning-Pipelines](how-to-debug-pipelines.md)
* [Debugging und Problembehandlung von ParallelRunStep](how-to-debug-parallel-run-step.md)
* [Interaktives Debuggen auf einer Azure Machine Learning Compute-Instanz mit VS Code Remote](how-to-debug-visual-studio-code.md)
* [Debugging und Problembehandlung für Pipelines des maschinellen Lernens in Application Insights](./how-to-log-pipelines-application-insights.md)
