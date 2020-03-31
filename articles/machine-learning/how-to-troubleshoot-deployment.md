---
title: Leitfaden zur Problembehandlung bei der Bereitstellung
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie die häufigsten Docker-Bereitstellungsfehler mit Azure Kubernetes Service und Azure Container Instances mit Azure Machine Learning umgehen, lösen und beheben können.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: clauren42
ms.author: clauren
ms.reviewer: jmartens
ms.date: 03/05/2020
ms.custom: seodec18
ms.openlocfilehash: fab46f7d7ae74ad643ce3f122b27b0dc767f5a78
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78399687"
---
# <a name="troubleshooting-azure-machine-learning-azure-kubernetes-service-and-azure-container-instances-deployment"></a>Problembehandlung bei der Bereitstellung von Azure Machine Learning, Azure Kubernetes Service und Azure Container Instances

Erfahren Sie, wie Sie die häufigsten Docker-Bereitstellungsfehler mit Azure Container Instances (ACI) und Azure Kubernetes Service (AKS) mit Azure Machine Learning umgehen oder beheben können.

Bei der Bereitstellung eines Modells in Azure Machine Learning führt das System eine Reihe von Aufgaben aus.

Der empfohlene und aktuellste Ansatz für die Modellimplementierung erfolgt über die [Model.deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model%28class%29?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-)-API mit einem [Environment](https://docs.microsoft.com/azure/machine-learning/service/how-to-use-environments)-Objekt als Eingabeparameter. In diesem Fall erstellt unser Dienst während der Bereitstellungsphase ein Docker-Basisimage für Sie und bindet die erforderlichen Modelle in einem einzelnen Aufruf ein. Dies sind die grundlegenden Aufgaben bei der Bereitstellung:

1. Registrieren des Modells in der Modellregistrierung des Arbeitsbereichs.

2. Definieren der Rückschlusskonfiguration:
    1. Erstellen Sie ein [Environment](https://docs.microsoft.com/azure/machine-learning/service/how-to-use-environments)-Objekt auf der Grundlage der Abhängigkeiten, die Sie in der YAML-Datei für die Umgebung angeben, oder verwenden Sie eine unserer bezogenen Umgebungen.
    2. Erstellen Sie eine Rückschlusskonfiguration (InferenceConfig-Objekt) auf der Grundlage der Umgebung und des Bewertungsskripts.

3. Stellen Sie das Modell im ACI-Dienst (Azure Container Instance) oder in AKS (Azure Kubernetes Service) bereit.

Weitere Informationen über diesen Prozess finden Sie in der Einführung zur [Modellverwaltung](concept-model-management-and-deployment.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein **Azure-Abonnement**. Wenn Sie keins besitzen, probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) aus.
* Das [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).
* Die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
* Die [CLI-Erweiterung für Azure Machine Learning](reference-azure-machine-learning-cli.md).
* Zum lokalen Debuggen benötigen Sie eine funktionierende Docker-Installation auf Ihrem lokalen System.

    Verwenden Sie den Befehl `docker run hello-world` über ein Terminal oder eine Befehlszeile, um Ihre Docker-Installation zu überprüfen. Informationen zur Installation von Docker oder zur Problembehandlung bei Docker-Fehlern finden Sie in der [Docker-Dokumentation](https://docs.docker.com/).

## <a name="before-you-begin"></a>Voraussetzungen

Beim Auftreten eines Problems besteht der erste Schritt darin, die (zuvor beschriebene) Bereitstellungsaufgabe in einzelne Schritte aufzuschlüsseln, um das Problem zu isolieren.

Angenommen, Sie verwenden die neue/empfohlene Bereitstellungsmethode über die [Model.deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model%28class%29?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-)-API mit einem [Environment](https://docs.microsoft.com/azure/machine-learning/service/how-to-use-environments)-Objekt als Eingabeparameter, dann kann Ihr Code in drei Hauptschritte unterteilt werden:

1. Registrieren des Modells. Hier finden Sie Beispielcode dazu:

    ```python
    from azureml.core.model import Model


    # register a model out of a run record
    model = best_run.register_model(model_name='my_best_model', model_path='outputs/my_model.pkl')

    # or, you can register a file or a folder of files as a model
    model = Model.register(model_path='my_model.pkl', model_name='my_best_model', workspace=ws)
    ```

2. Definieren der Rückschlusskonfiguration für die Bereitstellung:

    ```python
    from azureml.core.model import InferenceConfig
    from azureml.core.environment import Environment


    # create inference configuration based on the requirements defined in the YAML
    myenv = Environment.from_conda_specification(name="myenv", file_path="myenv.yml")
    inference_config = InferenceConfig(entry_script="score.py", environment=myenv)
    ```

3. Stellen Sie das Modell unter Verwendung der im vorhergehenden Schritt erstellten Rückschlusskonfiguration bereit:

    ```python
    from azureml.core.webservice import AciWebservice


    # deploy the model
    aci_config = AciWebservice.deploy_configuration(cpu_cores=1, memory_gb=1)
    aci_service = Model.deploy(workspace=ws,
                           name='my-service',
                           models=[model],
                           inference_config=inference_config,
                           deployment_config=aci_config)
    aci_service.wait_for_deployment(show_output=True)
    ```

Nachdem Sie den Bereitstellungsvorgang in einzelne Aufgaben aufgeschlüsselt haben, können wir uns einige der häufigsten Fehler ansehen.

## <a name="debug-locally"></a>Lokales Debuggen

Wenn bei der Bereitstellung eines Modells für ACI oder AKS Probleme auftreten, versuchen Sie, es als lokalen Webdienst bereitzustellen. Das Verwenden eines lokalen Webdiensts erleichtert die Problembehandlung. Das Docker-Image mit dem Modell wird heruntergeladen und auf dem lokalen System gestartet.

> [!WARNING]
> Bereitstellungen lokaler Webdienste werden nicht für Produktionsszenarien unterstützt.

Ändern Sie zum lokalen Bereitstellen Ihren Code so, dass `LocalWebservice.deploy_configuration()` zum Erstellen einer Bereitstellungskonfiguration verwendet wird. Verwenden Sie dann `Model.deploy()`, um den Dienst bereitzustellen. Im folgenden Beispiel wird ein (in der Modellvariablen enthaltenes) Modell als lokaler Webdienst bereitgestellt:

```python
from azureml.core.environment import Environment
from azureml.core.model import InferenceConfig, Model
from azureml.core.webservice import LocalWebservice


# Create inference configuration based on the environment definition and the entry script
myenv = Environment.from_conda_specification(name="env", file_path="myenv.yml")
inference_config = InferenceConfig(entry_script="score.py", environment=myenv)
# Create a local deployment, using port 8890 for the web service endpoint
deployment_config = LocalWebservice.deploy_configuration(port=8890)
# Deploy the service
service = Model.deploy(
    ws, "mymodel", [model], inference_config, deployment_config)
# Wait for the deployment to complete
service.wait_for_deployment(True)
# Display the port that the web service is available on
print(service.port)
```

Beachten Sie, dass Sie beim Definieren einer eigenen YAML-Datei zur Conda-Spezifikation „azureml-defaults“ mit einer Version größer gleich 1.0.45 als Pip-Abhängigkeit auflisten müssen. Dieses Paket enthält die erforderlichen Funktionen zum Hosten des Modells als Webdienst.

An diesem Punkt können Sie mit dem Dienst wie gewohnt arbeiten. Der folgende Code zeigt beispielsweise, wie Daten an den Dienst gesendet werden:

```python
import json

test_sample = json.dumps({'data': [
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
    [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
]})

test_sample = bytes(test_sample, encoding='utf8')

prediction = service.run(input_data=test_sample)
print(prediction)
```

Weitere Informationen zur Anpassung Ihrer Python-Umgebung finden Sie unter [Erstellen und Verwalten von Umgebungen für Training und Bereitstellung](how-to-use-environments.md). 

### <a name="update-the-service"></a>Aktualisieren des Diensts

Beim lokalen Testen müssen Sie möglicherweise die `score.py`-Datei aktualisieren, um die Protokollierung hinzuzufügen oder ggf. zu versuchen, alle Probleme zu behandeln, die Sie ermittelt haben. Zum erneuten Laden von Änderungen an der `score.py`-Datei verwenden Sie `reload()`. Der folgende Code lädt z.B. das Skript für den Dienst neu und sendet dann Daten an ihn. Die Daten werden mithilfe der aktualisierten `score.py`-Datei bewertet:

> [!IMPORTANT]
> Die Methode `reload` ist nur für lokale Bereitstellungen verfügbar. Informationen zum Aktualisieren einer Bereitstellung auf ein anderes Computeziel finden Sie im Artikel „Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst“ unter [Aktualisieren von Webdiensten](how-to-deploy-and-where.md#update).

```python
service.reload()
print(service.run(input_data=test_sample))
```

> [!NOTE]
> Das Skript wird aus dem Speicherort erneut geladen, der durch das vom Dienst verwendete `InferenceConfig`-Objekt angegeben wird.

Um das Modell, Conda-Abhängigkeiten oder eine Bereitstellungskonfiguration zu ändern, verwenden Sie [update()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#update--args-). Das folgende Beispiel aktualisiert das vom Dienst verwendete Modell:

```python
service.update([different_model], inference_config, deployment_config)
```

### <a name="delete-the-service"></a>Löschen des Diensts

Verwenden Sie zum Löschen des Diensts [delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#delete--).

### <a name="inspect-the-docker-log"></a><a id="dockerlog"></a> Untersuchen des Docker-Protokolls

Das Dienstobjekt erlaubt die Ausgabe von detaillierten Protokollnachrichten der Docker-Engine. Sie können das Protokoll für ACI, AKS und lokale Bereitstellungen anzeigen. Die Ausgabe der Protokolle wird im folgenden Beispiel veranschaulicht.

```python
# if you already have the service object handy
print(service.get_logs())

# if you only know the name of the service (note there might be multiple services with the same name but different version number)
print(ws.webservices['mysvc'].get_logs())
```

## <a name="service-launch-fails"></a>Fehler beim Starten des Diensts

Nach der erfolgreichen Erstellung des Images versucht das System, mithilfe Ihrer Bereitstellungskonfiguration einen Container zu starten. Als Teil des Container-Startprozesses wird die `init()`-Funktion in Ihrem Bereitstellungsskript vom System aufgerufen. Wenn in der `init()`-Funktion nicht abgefangene Ausnahmen auftreten, ist in der Fehlermeldung möglicherweise ein **CrashLoopBackOff**-Fehler zu finden.

Verwenden Sie die Informationen im Abschnitt [Untersuchen des Docker-Protokolls](#dockerlog), um die Protokolle überprüfen.

## <a name="function-fails-get_model_path"></a>Fehler bei der Funktion: get_model_path()

Oftmals wird in der `init()`-Funktion im Bewertungsskript die Funktion [Model.get_model_path()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#get-model-path-model-name--version-none---workspace-none-) aufgerufen, um eine Modelldatei oder einen Ordner mit Modelldateien im Container zu finden. Wenn die Datei oder der Ordner für das Modell nicht gefunden werden kann, tritt bei der Funktion ein Fehler auf. Die einfachste Möglichkeit zum Debuggen dieses Fehlers besteht darin, den unten dargestellten Python-Code in der Containershell auszuführen:

```python
from azureml.core.model import Model
import logging
logging.basicConfig(level=logging.DEBUG)
print(Model.get_model_path(model_name='my-best-model'))
```

In diesem Beispiel wird der lokale Pfad (relativ zu `/var/azureml-app`) in dem Container ausgegeben, unter dem Ihr Bewertungsskript die Modelldatei oder den Modellordner erwartet. Dann können Sie überprüfen, ob sich die Datei oder der Ordner wirklich dort befinden, wo sie erwartet werden.

Das Festlegen der Protokollierungsstufe auf DEBUG kann dazu führen, dass zusätzliche Informationen protokolliert werden, was bei der Identifizierung des Fehlers nützlich sein kann.

## <a name="function-fails-runinput_data"></a>Fehler bei der Funktion: run(input_data)

Wenn der Dienst erfolgreich bereitgestellt wurde, aber beim Veröffentlichen von Daten am Bewertungsendpunkt abstürzt, können Sie Ihrer `run(input_data)`-Funktion eine Anweisung zum Abfangen von Fehlern hinzufügen, damit sie stattdessen eine detaillierte Fehlermeldung zurückgibt. Beispiel:

```python
def run(input_data):
    try:
        data = json.loads(input_data)['data']
        data = np.array(data)
        result = model.predict(data)
        return json.dumps({"result": result.tolist()})
    except Exception as e:
        result = str(e)
        # return error message back to the client
        return json.dumps({"error": result})
```

**Hinweis**: Die Rückgabe von Fehlermeldungen aus dem Aufruf `run(input_data)` sollte nur zu Debugzwecken erfolgen. Aus Sicherheitsgründen sollten Sie in einer Produktionsumgebung keine Fehlermeldungen auf diese Weise zurückgeben.

## <a name="http-status-code-502"></a>HTTP-Statuscode 502

Der Statuscode 502 gibt an, dass der Dienst eine Ausnahme ausgelöst hat oder in der `run()`-Methode der Datei „score.py“ abgestürzt ist. Verwenden Sie die Informationen in diesem Artikel, um die Datei zu debuggen.

## <a name="http-status-code-503"></a>HTTP-Statuscode 503

Azure Kubernetes Service-Bereitstellungen unterstützen die automatische Skalierung, mit der das Hinzufügen von Replikaten ermöglicht wird, um zusätzliche Last zu unterstützen. Die Autoskalierung wurde jedoch für die Behandlung **stetiger** Veränderungen der Last konzipiert. Wenn Sie große Spitzen bei den Anforderungen pro Sekunde erhalten, erhalten Clients möglicherweise den HTTP-Statuscode 503.

Es gibt zwei Möglichkeiten, die beim Verhindern des Statuscodes 503 helfen können:

* Ändern Sie den Auslastungsgrad für die Erstellung neuer Replikate durch die automatische Skalierung.
    
    Der Auslastungsgrad der automatischen Skalierung ist standardmäßig auf 70 % festgelegt, was bedeutet, dass der Dienst Spitzen bei den Anforderungen pro Sekunde (RPS) von bis zu 30 % verarbeiten kann. Sie können den Auslastungsgrad anpassen, indem Sie einen niedrigeren Wert für `autoscale_target_utilization` festlegen.

    > [!IMPORTANT]
    > Durch diese Änderung werden Replikate *nicht schneller* erstellt. Stattdessen werden sie mit einem niedrigeren Schwellenwert für die Auslastung erstellt. Anstatt abzuwarten, bis der Dienst zu 70 % ausgelastet ist, werden Replikate schon bei 30 % Auslastung erstellt, wenn Sie den Wert in 30 % ändern.
    
    Wenn der Webdienst bereits die derzeit maximale Anzahl von Replikaten verwendet und Sie weiterhin den Statuscode 503 erhalten, erhöhen Sie den `autoscale_max_replicas`-Wert, um die maximale Anzahl der Replikate zu erhöhen.

* Ändern Sie die Mindestanzahl der Replikate. Indem Sie die Mindestanzahl der Replikate erhöhen, wird ein größerer Pool für die Verarbeitung eingehender Spitzen bereitgestellt.

    Legen Sie einen höheren Wert für `autoscale_min_replicas` fest, um die Mindestanzahl der Replikate zu erhöhen. Sie können die erforderliche Anzahl von Replikaten berechnen, indem Sie den folgenden Code verwenden und die Werte durch die Werten Ihres Projekts ersetzen:

    ```python
    from math import ceil
    # target requests per second
    targetRps = 20
    # time to process the request (in seconds)
    reqTime = 10
    # Maximum requests per container
    maxReqPerContainer = 1
    # target_utilization. 70% in this example
    targetUtilization = .7

    concurrentRequests = targetRps * reqTime / targetUtilization

    # Number of container replicas
    replicas = ceil(concurrentRequests / maxReqPerContainer)
    ```

    > [!NOTE]
    > Wenn Anforderungsspitzen eingehen, die die neue Mindestanzahl von Replikaten überschreiten, erhalten Sie möglicherweise wieder den Statuscode 503. Wenn sich der Datenverkehr Ihres Diensts beispielsweise erhöht, müssen Sie die Mindestanzahl von Replikaten möglicherweise erhöhen.

Weitere Informationen zum Festlegen von `autoscale_target_utilization`, `autoscale_max_replicas` und `autoscale_min_replicas` finden Sie in der Modulreferenz zu [AksWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py).

## <a name="http-status-code-504"></a>HTTP-Statuscode 504

Der Statuscode 504 gibt an, dass für die Anforderung ein Timeout aufgetreten ist. Das Standardtimeout beträgt 1 Minute.

Sie können das Timeout erhöhen oder versuchen, den Dienst zu beschleunigen, indem Sie unnötige Aufrufe in „score.py“ entfernen. Wenn das Problem durch diese Aktionen nicht behoben werden kann, debuggen Sie die Datei „score.py“ mithilfe der Informationen in diesem Artikel. Möglicherweise reagiert der Code nicht mehr, oder er befindet sich in einer Endlosschleife.

## <a name="advanced-debugging"></a>Erweitertes Debuggen

In einigen Fällen müssen Sie den in der Modellbereitstellung enthaltenen Python-Code ggf. interaktiv debuggen. Dies ist beispielsweise der Fall, wenn das Einstiegsskript fehlschlägt und der Grund nicht durch zusätzliche Protokollierung ermittelt werden kann. Mit Visual Studio Code und Python Tools für Visual Studio (PTVSD) können Sie den Code debuggen, der im Docker-Container ausgeführt wird.

> [!IMPORTANT]
> Diese Methode des Debuggens funktioniert nicht, wenn `Model.deploy()` und `LocalWebservice.deploy_configuration` verwendet werden, um ein Modell lokal bereitzustellen. Stattdessen müssen Sie ein Image mithilfe der [Model.package()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#package-workspace--models--inference-config-none--generate-dockerfile-false-)-Methode erstellen.

Bereitstellungen lokaler Webdienste erfordern eine funktionierende Installation von Docker auf Ihrem lokalen System. Weitere Informationen zum Verwenden von Docker finden Sie in der [Docker-Dokumentation](https://docs.docker.com/).

### <a name="configure-development-environment"></a>Konfigurieren der Entwicklungsumgebung

1. Um Python Tools für Visual Studio (PTVSD) in Ihrer lokalen VS Code-Entwicklungsumgebung zu installieren, verwenden Sie den folgenden Befehl:

    ```
    python -m pip install --upgrade ptvsd
    ```

    Weitere Informationen zur Verwendung von PTVSD mit VS Code finden Sie unter [Remotedebuggen](https://code.visualstudio.com/docs/python/debugging#_remote-debugging).

1. Um VS Code für die Kommunikation mit dem Docker-Image zu konfigurieren, erstellen Sie eine neue Debugkonfiguration:

    1. Wählen Sie aus VS Code das Menü __Debuggen__ aus, und wählen Sie dann __Konfigurationen öffnen__ aus. Eine Datei namens __launch.json__ wird geöffnet.

    1. Suchen Sie in der Datei __launch.json__ nach der Zeile, die `"configurations": [` enthält, und fügen Sie hinter ihr den folgenden Text ein:

        ```json
        {
            "name": "Azure Machine Learning: Docker Debug",
            "type": "python",
            "request": "attach",
            "port": 5678,
            "host": "localhost",
            "pathMappings": [
                {
                    "localRoot": "${workspaceFolder}",
                    "remoteRoot": "/var/azureml-app"
                }
            ]
        }
        ```

        > [!IMPORTANT]
        > Wenn es bereits andere Einträge im Konfigurationsabschnitt gibt, fügen Sie ein Komma (,) nach dem von Ihnen eingegebenen Code ein.

        Dieser Abschnitt stellt die Verbindung mit dem Docker-Container über Port 5678 her.

    1. Speichern Sie die Datei __launch.json__.

### <a name="create-an-image-that-includes-ptvsd"></a>Erstellen eines Image, das PTVSD enthält

1. Ändern Sie die Conda-Umgebung für die Bereitstellung so, dass sie PTVSD enthält. Das folgende Beispiel veranschaulicht, wie PTVSD mithilfe des `pip_packages`-Parameters hinzugefügt wird:

    ```python
    from azureml.core.conda_dependencies import CondaDependencies 


    # Usually a good idea to choose specific version numbers
    # so training is made on same packages as scoring
    myenv = CondaDependencies.create(conda_packages=['numpy==1.15.4',            
                                'scikit-learn==0.19.1', 'pandas==0.23.4'],
                                 pip_packages = ['azureml-defaults==1.0.45', 'ptvsd'])

    with open("myenv.yml","w") as f:
        f.write(myenv.serialize_to_string())
    ```

1. Um PTVSD zu starten und beim Start des Diensts auf eine Verbindung zu warten, fügen Sie Folgendes am Anfang Ihrer Datei `score.py` hinzu:

    ```python
    import ptvsd
    # Allows other computers to attach to ptvsd on this IP address and port.
    ptvsd.enable_attach(address=('0.0.0.0', 5678), redirect_output = True)
    # Wait 30 seconds for a debugger to attach. If none attaches, the script continues as normal.
    ptvsd.wait_for_attach(timeout = 30)
    print("Debugger attached...")
    ```

1. Erstellen Sie ein Image auf der Grundlage der Umgebungsdefinition, und pullen Sie das Image in die lokale Registrierung. Während des Debuggens können Sie Änderungen an den Dateien im Image vornehmen, ohne es neu erstellen zu müssen. Um einen Text-Editor (vim) im Docker-Image zu installieren, verwenden Sie die Eigenschaften `Environment.docker.base_image` und `Environment.docker.base_dockerfile`:

    > [!NOTE]
    > Dieses Beispiel geht davon aus, dass `ws` auf Ihren Azure Machine Learning-Arbeitsbereich zeigt und `model` das Modell ist, das bereitgestellt wird. Die Datei `myenv.yml` enthält die Conda-Abhängigkeiten, die in Schritt 1 erstellt wurden.

    ```python
    from azureml.core.conda_dependencies import CondaDependencies
    from azureml.core.model import InferenceConfig
    from azureml.core.environment import Environment


    myenv = Environment.from_conda_specification(name="env", file_path="myenv.yml")
    myenv.docker.base_image = None
    myenv.docker.base_dockerfile = "FROM mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04\nRUN apt-get update && apt-get install vim -y"
    inference_config = InferenceConfig(entry_script="score.py", environment=myenv)
    package = Model.package(ws, [model], inference_config)
    package.wait_for_creation(show_output=True)  # Or show_output=False to hide the Docker build logs.
    package.pull()
    ```

    Nachdem das Image erstellt und heruntergeladen wurde, wird der Imagepfad (einschließlich Repository, Name und Tag, das in diesem Fall auch der Digest ist) in einer Meldung wie der folgenden angezeigt:

    ```text
    Status: Downloaded newer image for myregistry.azurecr.io/package@sha256:<image-digest>
    ```

1. Um die Arbeit mit dem Image zu erleichtern, verwenden Sie den folgenden Befehl, um ein Tag hinzuzufügen. Ersetzen Sie `myimagepath` durch den Speicherortwert aus dem vorherigen Schritt.

    ```bash
    docker tag myimagepath debug:1
    ```

    Für die restlichen Schritte können Sie sich auf das lokale Image als `debug:1` beziehen, anstatt den vollständigen Wert des Bildpfads zu verwenden.

### <a name="debug-the-service"></a>Debuggen des Diensts

> [!TIP]
> Wenn Sie in der Datei `score.py` ein Timeout für die PTVSD-Verbindung festlegen, müssen Sie VS Code mit der Debugsitzung verbinden, bevor das Timeout abläuft. Starten Sie VS Code, öffnen Sie die lokale Kopie von `score.py`, legen Sie einen Breakpoint fest, und bereiten Sie alles vor, bevor Sie die Schritte in diesem Abschnitt ausführen.
>
> Weitere Informationen zum Debuggen und Festlegen von Breakpoints finden Sie unter [Debuggen](https://code.visualstudio.com/Docs/editor/debugging).

1. Um einen Docker-Container mithilfe des Image zu starten, verwenden Sie den folgenden Befehl:

    ```bash
    docker run --rm --name debug -p 8000:5001 -p 5678:5678 debug:1
    ```

1. Um VS Code an PTVSD im Container anzufügen, öffnen Sie VS Code, und verwenden Sie die Taste F5, oder wählen Sie __Debuggen__ aus. Wenn Sie dazu aufgefordert werden, wählen Sie die Konfiguration __Azure Machine Learning: Docker debuggen__ aus. Sie können auch das Debugsymbol auf der Seitenleiste auswählen, den Eintrag __Azure Machine Learning: Docker debuggen__ aus dem Debugdropdownmenü und dann den grünen Pfeil zum Anfügen des Debuggers verwenden.

    ![Das Symbol „Debuggen“, die Schaltfläche „Debuggen starten“ und die Konfigurationsauswahl](./media/how-to-troubleshoot-deployment/start-debugging.png)

An diesem Punkt stellt VS Code eine Verbindung mit PTVSD im Docker-Container her und hält an dem von Ihnen zuvor festgelegten Breakpoint an. Sie können den Code jetzt während der Ausführung schrittweise durchlaufen, Variablen anzeigen usw.

Weitere Informationen zum Verwenden von VS Code zum Debuggen von Python finden Sie unter [Debuggen von Python-Code](https://docs.microsoft.com/visualstudio/python/debugging-python-in-visual-studio?view=vs-2019).

<a id="editfiles"></a>
### <a name="modify-the-container-files"></a>Ändern der Containerdateien

Um Änderungen an Dateien im Image vornehmen, können Sie eine Verbindung mit dem ausgeführten Container herstellen und eine Bash-Shell ausführen. Von dort aus können Sie vim verwenden, um Dateien zu bearbeiten:

1. Um eine Verbindung mit dem ausgeführten Container herzustellen und eine Bash-Shell im Container zu starten, verwenden Sie den folgenden Befehl:

    ```bash
    docker exec -it debug /bin/bash
    ```

1. Um die vom Dienst verwendeten Dateien zu ermitteln, verwenden Sie den folgenden Befehl in der Bash-Shell im Container, falls das Standardverzeichnis nicht `/var/azureml-app` lautet:

    ```bash
    cd /var/azureml-app
    ```

    Von dort aus können Sie vim verwenden, um die Datei `score.py` zu bearbeiten. Weitere Informationen zur Verwendung von vim finden Sie unter [Verwenden des vim-Editors](https://www.tldp.org/LDP/intro-linux/html/sect_06_02.html).

1. Änderungen in einem Container werden normalerweise nicht persistent gespeichert. Um alle Änderungen zu speichern, die Sie vorgenommen haben, verwenden Sie den folgenden Befehl, bevor Sie die im Schritt oben gestartete Shell beenden (also in einer anderen Shell):

    ```bash
    docker commit debug debug:2
    ```

    Mit diesem Befehl wird ein neues Image mit dem Namen `debug:2` erstellt, das Ihre Bearbeitungen enthält.

    > [!TIP]
    > Sie müssen den aktuellen Container beenden und mit der Verwendung der neuen Version beginnen, damit Änderungen wirksam werden.

1. Stellen Sie sicher, dass die Änderungen, die Sie an Dateien im Container vornehmen, mit den lokalen Dateien synchronisiert werden, die VS Code verwendet. Andernfalls funktioniert der Debugger nicht wie erwartet.

### <a name="stop-the-container"></a>Beenden des Containers

Zum Beenden des Containers verwenden Sie den folgenden Befehl:

```bash
docker stop debug
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Bereitstellung finden Sie hier:

* [Bereitstellung: wie und wo?](how-to-deploy-and-where.md)
* [Tutorial: Trainieren und Bereitstellen von Modellen](tutorial-train-models-with-aml.md)
