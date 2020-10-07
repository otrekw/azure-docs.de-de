---
title: Problembehandlung für Docker-Bereitstellungen
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie die häufigsten Docker-Bereitstellungsfehler mit Azure Kubernetes Service und Azure Container Instances mit Azure Machine Learning umgehen, lösen und beheben können.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
author: clauren42
ms.author: clauren
ms.reviewer: jmartens
ms.date: 08/06/2020
ms.topic: conceptual
ms.custom: troubleshooting, contperfq4, devx-track-python
ms.openlocfilehash: 82b9db2f3575e50367ed154246f9fb69b74c60cf
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91333771"
---
# <a name="troubleshoot-docker-deployment-of-models-with-azure-kubernetes-service-and-azure-container-instances"></a>Problembehandlung bei der Docker-Bereitstellung von Modellen mit Azure Kubernetes Service und Azure Container Instances 

Erfahren Sie, wie Sie die häufigsten Docker-Bereitstellungsfehler mit Azure Container Instances (ACI) und Azure Kubernetes Service (AKS) mit Azure Machine Learning behandeln, beheben oder umgehen können.

## <a name="prerequisites"></a>Voraussetzungen

* Ein **Azure-Abonnement**. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) aus.
* Das [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true).
* Die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true)
* Die [CLI-Erweiterung für Azure Machine Learning](reference-azure-machine-learning-cli.md).
* Zum lokalen Debuggen benötigen Sie eine funktionierende Docker-Installation auf Ihrem lokalen System.

    Verwenden Sie den Befehl `docker run hello-world` über ein Terminal oder eine Befehlszeile, um Ihre Docker-Installation zu überprüfen. Informationen zur Installation von Docker oder zur Problembehandlung bei Docker-Fehlern finden Sie in der [Docker-Dokumentation](https://docs.docker.com/).

## <a name="steps-for-docker-deployment-of-machine-learning-models"></a>Schritte für die Docker-Bereitstellung von Machine Learning-Modellen

Wenn Sie ein Modell in Azure Machine Learning bereitstellen, verwenden Sie die [Model.deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model%28class%29?view=azure-ml-py&preserve-view=true#&preserve-view=truedeploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-)-API und ein [Environment](how-to-use-environments.md)-Objekt. Der Dienst erstellt während der Bereitstellungsphase ein Docker-Basisimage und bindet die erforderlichen Modelle in einem einzelnen Aufruf ein. Dies sind die grundlegenden Aufgaben bei der Bereitstellung:

1. Registrieren des Modells in der Modellregistrierung des Arbeitsbereichs.

2. Definieren der Rückschlusskonfiguration:
    1. Erstellen Sie ein [Environment](how-to-use-environments.md)-Objekt. Dieses Objekt kann die Abhängigkeiten in einer YAML-Umgebungsdatei verwenden (eine unserer zusammengestellten Umgebungen).
    2. Erstellen Sie eine Rückschlusskonfiguration (InferenceConfig-Objekt) auf der Grundlage der Umgebung und des Bewertungsskripts.

3. Stellen Sie das Modell im ACI-Dienst (Azure Container Instance) oder in AKS (Azure Kubernetes Service) bereit.

Weitere Informationen über diesen Prozess finden Sie in der Einführung zur [Modellverwaltung](concept-model-management-and-deployment.md).

## <a name="before-you-begin"></a>Voraussetzungen

Beim Auftreten eines Problems besteht der erste Schritt darin, die (zuvor beschriebene) Bereitstellungsaufgabe in einzelne Schritte aufzuschlüsseln, um das Problem zu isolieren.

Wenn Sie [Model.deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model%28class%29?view=azure-ml-py&preserve-view=true#&preserve-view=truedeploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) mit einem [Environment](how-to-use-environments.md)-Objekt als Eingabeparameter verwenden, kann Ihr Code in drei Hauptschritte unterteilt werden:

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

Durch das Unterteilen des Bereitstellungsprozesses in einzelne Aufgaben können einige der gängigeren Fehler einfacher identifiziert werden.

## <a name="debug-locally"></a>Lokales Debuggen

Wenn bei der Bereitstellung eines Modells für ACI oder AKS Probleme auftreten, versuchen Sie, es als lokalen Webdienst bereitzustellen. Das Verwenden eines lokalen Webdiensts erleichtert die Problembehandlung.

Im Repository [MachineLearningNotebooks](https://github.com/Azure/MachineLearningNotebooks) finden Sie ein beispielhaftes [lokales Bereitstellungsnotebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/deploy-to-local/register-model-deploy-local.ipynb), das Sie erkunden können.

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

Wenn Sie Ihre eigene YAML-Datei für die Conda-Spezifikation definieren, listen Sie „azureml-defaults“ mit einer Version größer oder gleich 1.0.45 als Pip-Abhängigkeit auf. Dieses Paket ist erforderlich, um das Modell als Webdienst zu hosten.

An diesem Punkt können Sie mit dem Dienst wie gewohnt arbeiten. Der folgende Code zeigt, wie Daten an den Dienst gesendet werden:

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
> Die Methode `reload` ist nur für lokale Bereitstellungen verfügbar. Informationen zum Aktualisieren einer Bereitstellung auf ein anderes Computeziel finden Sie unter [Aktualisieren von Webdiensten](how-to-deploy-update-web-service.md).

```python
service.reload()
print(service.run(input_data=test_sample))
```

> [!NOTE]
> Das Skript wird aus dem Speicherort erneut geladen, der durch das vom Dienst verwendete `InferenceConfig`-Objekt angegeben wird.

Um das Modell, Conda-Abhängigkeiten oder eine Bereitstellungskonfiguration zu ändern, verwenden Sie [update()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py&preserve-view=true#&preserve-view=trueupdate--args-). Das folgende Beispiel aktualisiert das vom Dienst verwendete Modell:

```python
service.update([different_model], inference_config, deployment_config)
```

### <a name="delete-the-service"></a>Löschen des Diensts

Verwenden Sie zum Löschen des Diensts [delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py&preserve-view=true#&preserve-view=truedelete--).

### <a name="inspect-the-docker-log"></a><a id="dockerlog"></a> Untersuchen des Docker-Protokolls

Das Dienstobjekt erlaubt die Ausgabe von detaillierten Protokollnachrichten der Docker-Engine. Sie können das Protokoll für ACI, AKS und lokale Bereitstellungen anzeigen. Die Ausgabe der Protokolle wird im folgenden Beispiel veranschaulicht.

```python
# if you already have the service object handy
print(service.get_logs())

# if you only know the name of the service (note there might be multiple services with the same name but different version number)
print(ws.webservices['mysvc'].get_logs())
```
Wenn die Zeile `Booting worker with pid: <pid>` in den Protokollen mehrmals angezeigt wird, ist nicht ausreichend Arbeitsspeicher vorhanden, um den Worker zu starten.
Sie können den Fehler beheben, indem Sie den Wert von `memory_gb` in `deployment_config` ändern.
 
## <a name="container-cannot-be-scheduled"></a>Planen des Containers nicht möglich

Beim Bereitstellen eines Diensts für ein Azure Kubernetes Service-Computeziel versucht Azure Machine Learning, den Dienst mit der angeforderten Menge von Ressourcen zu planen. Wenn nach 5 Minuten keine Knoten mit der entsprechenden Menge von verfügbaren Ressourcen im Cluster vorhanden sind, schlägt die Bereitstellung fehl. Die Fehlermeldung ist `Couldn't Schedule because the kubernetes cluster didn't have available resources after trying for 00:05:00`. Sie können diesen Fehler beheben, indem Sie entweder weitere Knoten hinzufügen, die SKU Ihrer Knoten oder die Ressourcenanforderungen für Ihren Dienst ändern. 

In der Fehlermeldung ist in der Regel angegeben, von welcher Ressource mehr benötigt wird. Wenn beispielsweise eine Fehlermeldung angezeigt wird, in der `0/3 nodes are available: 3 Insufficient nvidia.com/gpu` (0/3 Knoten sind verfügbar: 3 unzureichend nvidia.com/gpu) steht, bedeutet dies, dass der Dienst GPUs benötigt und drei Knoten im Cluster vorhanden sind, die nicht über verfügbare GPUs verfügen. Dieses Problem kann durch Hinzufügen weiterer Knoten behoben werden, wenn Sie eine GPU-SKU verwenden, oder durch Wechseln zu einer GPU-fähigen SKU, wenn nicht. Alternativ können Sie Ihre Umgebung so ändern, dass keine GPUs erforderlich sind.  

## <a name="service-launch-fails"></a>Fehler beim Starten des Diensts

Nach der erfolgreichen Erstellung des Images versucht das System, mithilfe Ihrer Bereitstellungskonfiguration einen Container zu starten. Als Teil des Container-Startprozesses wird die `init()`-Funktion in Ihrem Bereitstellungsskript vom System aufgerufen. Wenn in der `init()`-Funktion nicht abgefangene Ausnahmen auftreten, ist in der Fehlermeldung möglicherweise ein **CrashLoopBackOff**-Fehler zu finden.

Verwenden Sie die Informationen im Abschnitt [Untersuchen des Docker-Protokolls](#dockerlog), um die Protokolle überprüfen.

## <a name="function-fails-get_model_path"></a>Fehler bei der Funktion: get_model_path()

Oftmals wird in der `init()`-Funktion im Bewertungsskript die Funktion [Model.get_model_path()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py&preserve-view=true#&preserve-view=trueget-model-path-model-name--version-none---workspace-none-) aufgerufen, um eine Modelldatei oder einen Ordner mit Modelldateien im Container zu finden. Wenn die Datei oder der Ordner für das Modell nicht gefunden werden kann, tritt bei der Funktion ein Fehler auf. Die einfachste Möglichkeit zum Debuggen dieses Fehlers besteht darin, den unten dargestellten Python-Code in der Containershell auszuführen:

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

Azure Kubernetes Service-Bereitstellungen unterstützen die automatische Skalierung, mit der das Hinzufügen von Replikaten ermöglicht wird, um zusätzliche Last zu unterstützen. Die Autoskalierung wurde für die Behandlung **gradueller** Änderungen der Auslastung konzipiert. Wenn Sie große Spitzen bei den Anforderungen pro Sekunde erhalten, erhalten Clients möglicherweise den HTTP-Statuscode 503. Obwohl die Autoskalierung schnell reagiert, nimmt die Erstellung zusätzlicher Container in AKS beträchtliche Zeit in Anspruch.

Skalierungsentscheidungen werden auf der Grundlage der Auslastung der aktuellen Containerreplikate getroffen. Zur Ermittlung der aktuellen Auslastung wird die Anzahl ausgelasteter Replikate (Replikate, die eine Anforderung verarbeiten) durch die Gesamtanzahl aktueller Replikate geteilt. Übersteigt dieser Wert `autoscale_target_utilization`, werden weitere Replikate erstellt. Ist der Wert niedriger, wird die Replikatanzahl verringert. Entscheidungen zum Hinzufügen von Replikaten sind eifrig und schnell (ungefähr 1 Sekunde). Die Entscheidung, Replikate zu entfernen, erfolgt zurückhaltend (etwa 1 Minute). Der Auslastungsgrad der Autoskalierung ist standardmäßig auf **70 %** festgelegt, was bedeutet, dass der Dienst Spitzen bei den Anforderungen pro Sekunde (RPS) von **bis zu 30 %** verarbeiten kann.

Es gibt zwei Möglichkeiten, die beim Verhindern des Statuscodes 503 helfen können:

> [!TIP]
> Diese beiden Ansätze können einzeln oder in Kombination verwendet werden.

* Ändern Sie den Auslastungsgrad für die Erstellung neuer Replikate durch die automatische Skalierung. Sie können den Auslastungsgrad anpassen, indem Sie einen niedrigeren Wert für `autoscale_target_utilization` festlegen.

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

Weitere Informationen zum Festlegen von `autoscale_target_utilization`, `autoscale_max_replicas` und `autoscale_min_replicas` finden Sie in der Modulreferenz zu [AksWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py&preserve-view=true).

## <a name="http-status-code-504"></a>HTTP-Statuscode 504

Der Statuscode 504 gibt an, dass für die Anforderung ein Timeout aufgetreten ist. Das Standardtimeout beträgt 1 Minute.

Sie können das Timeout erhöhen oder versuchen, den Dienst zu beschleunigen, indem Sie unnötige Aufrufe in „score.py“ entfernen. Wenn das Problem durch diese Aktionen nicht behoben werden kann, debuggen Sie die Datei „score.py“ mithilfe der Informationen in diesem Artikel. Der Code kann sich in einem nicht reaktionsfähigen Zustand oder in einer Endlosschleife befinden.

## <a name="advanced-debugging"></a>Erweitertes Debuggen

Sie müssen den in der Modellimplementierung enthaltenen Python-Code ggf. interaktiv debuggen. Dies ist beispielsweise der Fall, wenn das Einstiegsskript fehlschlägt und der Grund nicht durch zusätzliche Protokollierung ermittelt werden kann. Mit Visual Studio Code und debugpy können Sie an den Code anfügen, der im Docker-Container ausgeführt wird.

Weitere Informationen finden Sie im [Leitfaden „Interaktives Debuggen in VS Code“](how-to-debug-visual-studio-code.md#debug-and-troubleshoot-deployments).

## <a name="model-deployment-user-forum"></a>[Benutzerforum zur Modellimplementierung](https://docs.microsoft.com/answers/topics/azure-machine-learning-inference.html)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Bereitstellung finden Sie hier:

* [Bereitstellung: wie und wo?](how-to-deploy-and-where.md)
* [Tutorial: Trainieren und Bereitstellen von Modellen](tutorial-train-models-with-aml.md)
