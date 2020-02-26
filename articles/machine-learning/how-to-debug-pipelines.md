---
title: Debuggen und Problembehandlung für Machine Learning-Pipelines
titleSuffix: Azure Machine Learning
description: Debuggen und Problembehandlung für Machine Learning-Pipelines im Azure Machine Learning SDK für Python. Lernen Sie häufige Fallstricke bei der Entwicklung von Pipelines kennen, und erhalten Sie Tipps, die Ihnen helfen, Ihre Skripts vor und während der Remoteausführung zu debuggen. Erfahren Sie, wie Sie Ihre Machine Learning-Pipelines mithilfe von Visual Studio Code interaktiv debuggen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.date: 12/12/2019
ms.openlocfilehash: 0080b64e16b979b32aa5a91f9ee497e5f9ec47fb
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77485368"
---
# <a name="debug-and-troubleshoot-machine-learning-pipelines"></a>Debuggen und Problembehandlung für Machine Learning-Pipelines
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie das Debuggen und die Problembehandlung für [Machine Learning-Pipelines](concept-ml-pipelines.md) im [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) und im [Azure Machine Learning Designer (Vorschau)](https://docs.microsoft.com/azure/machine-learning/concept-designer) durchführen. Folgendes wird vermittelt:

* Debuggen mithilfe des Azure Machine Learning SDK
* Debuggen mithilfe des Azure Machine Learning-Designers
* Debuggen mithilfe von Application Insights
* Interaktives Debuggen mithilfe von Visual Studio Code (VS Code) und Python Tools für Visual Studio (PTVSD)

## <a name="debug-and-troubleshoot-in-the-azure-machine-learning-sdk"></a>Debuggen und Problembehandlung im Azure Machine Learning SDK
Die folgenden Abschnitte bieten einen Überblick über häufige Fallstricke beim Erstellen von Pipelines und verschiedene Strategien zum Debuggen von Code, der in einer Pipeline ausgeführt wird. Verwenden Sie die folgenden Tipps, wenn Sie Schwierigkeiten haben, eine Pipeline wie erwartet auszuführen.

### <a name="testing-scripts-locally"></a>Lokales Testen von Skripts

Einer der häufigsten Fehler in einer Pipeline ist, dass ein angefügtes Skript (Skript zur Datenbereinigung, Bewertungsskript usw.) nicht wie vorgesehen ausgeführt wird oder Laufzeitfehler im Remotecomputekontext enthält, die in Ihrem Arbeitsbereich in Azure Machine Learning Studio schwierig zu debuggen sind. 

Pipelines selbst können nicht lokal ausgeführt werden. Wenn Sie jedoch die Skripts isoliert auf dem lokalen Computer ausführen, können Sie schneller debuggen, da Sie nicht auf den Compute- und Umgebungserstellungsprozess warten müssen. Hierfür ist ein gewisser Entwicklungsaufwand erforderlich:

* Wenn sich Ihre Daten in einem Clouddatenspeicher befinden, müssen Sie Daten herunterladen und Ihrem Skript zur Verfügung stellen. Die Verwendung einer kleinen Stichprobe Ihrer Daten ist eine gute Möglichkeit, die Laufzeit zu verkürzen und schnell Feedback zum Skriptverhalten zu erhalten.
* Wenn Sie versuchen, einen Pipelinezwischenschritt zu simulieren, müssen Sie die Objekttypen, die das jeweilige Skript vom vorherigen Schritt erwartet, möglicherweise manuell erstellen.
* Sie müssen auch Ihre eigene Umgebung definieren und die in Ihrer Remotecomputeumgebung definierten Abhängigkeiten replizieren.

Nachdem Sie ein Skript für die Ausführung in Ihrer lokalen Umgebung eingerichtet haben, gestalten sich Debugaufgaben wie die Folgenden viel einfacher:

* Anfügen einer benutzerdefinierten Debugkonfiguration
* Anhalten der Ausführung und Überprüfen des Objektzustands
* Abfangen von Typfehlern oder logischen Fehlern, die bis zur Laufzeit nicht verfügbar gemacht werden

> [!TIP] 
> Wenn Sie sichergestellt haben, dass ihr Skript erwartungsgemäß ausgeführt wird, empfiehlt es sich, das Skript nun in einer Einzelschritt-Pipeline auszuführen, anstatt zu versuchen, das Skript in einer Pipeline mit mehreren Schritten auszuführen.

### <a name="debugging-scripts-from-remote-context"></a>Debuggen von Skripts aus Remotekontext

Das lokale Testen von Skripts ist eine hervorragende Möglichkeit, wichtige Codefragmente und eine komplexe Logik zu debuggen, bevor Sie mit dem Erstellen einer Pipeline beginnen. Aber irgendwann werden Sie Skripts wahrscheinlich während der eigentlichen Pipelineausführung selbst debuggen müssen, insbesondere bei der Diagnose von Verhaltensweisen, die während der Interaktion zwischen Pipelineschritten auftreten. Wir empfehlen die großzügige Verwendung von `print()`-Anweisungen in Ihren Schrittskripts, damit Sie den Objektzustand und die erwarteten Werte während der Remoteausführung sehen können, ähnlich wie beim Debuggen von JavaScript-Code.

Die Protokolldatei `70_driver_log.txt` enthält: 

* Alle während der Ausführung Ihres Skripts ausgegebenen Anweisungen
* Die Stapelüberwachung für das Skript 

Um diese und andere Protokolldateien im Portal zu finden, klicken Sie zunächst in Ihrem Arbeitsbereich auf die Pipelineausführung.

![Seite „Pipelineausführungsliste“](./media/how-to-debug-pipelines/pipelinerun-01.png)

Navigieren Sie zur Detailseite der Pipelineausführung.

![Detailseite der Pipelineausführung](./media/how-to-debug-pipelines/pipelinerun-02.png)

Klicken Sie auf das Modul für den jeweiligen Schritt. Navigieren Sie zur Registerkarte **Protokolle**. Andere Protokolle enthalten Informationen zum Erstellungsvorgang für Ihr Umgebungsimage und zu Skripts zur Vorbereitung von Schritten.

![Registerkarte „Protokoll“ auf der Detailseite der Pipelineausführung](./media/how-to-debug-pipelines/pipelinerun-03.png)

> [!TIP]
> Ausführungen für *veröffentlichte Pipelines* finden Sie auf der Registerkarte **Endpunkte** in Ihrem Arbeitsbereich. Ausführungen für *nicht veröffentlichte Pipelines* finden Sie unter **Experimente** oder **Pipelines**.

### <a name="troubleshooting-tips"></a>Tipps zur Problembehandlung

Die folgende Tabelle enthält häufige Probleme bei der Pipelinentwicklung mit möglichen Lösungen.

| Problem | Mögliche Lösung |
|--|--|
| Daten können nicht in das Verzeichnis `PipelineData` übergeben werden | Stellen Sie sicher, dass Sie im Skript ein Verzeichnis erstellt haben, das dem entspricht, wo Ihre Pipeline die Schrittausgabedaten erwartet. In den meisten Fällen definiert ein Eingabeargument das Ausgabeverzeichnis, und dann erstellen Sie das Verzeichnis explizit. Verwenden Sie `os.makedirs(args.output_dir, exist_ok=True)`, um das Ausgabeverzeichnis zu erstellen. Im [Tutorial](tutorial-pipeline-batch-scoring-classification.md#write-a-scoring-script) finden Sie ein Beispiel für ein Bewertungsskript, das dieses Entwurfsmuster zeigt. |
| Fehler bei Abhängigkeiten | Wenn Sie Skripts lokal entwickelt und getestet haben, aber beim Ausführen auf einem Remotecomputer in der Pipeline Abhängigkeitsprobleme auftreten, stellen Sie sicher, dass Ihre Abhängigkeiten und Versionen der Computeumgebung mit Ihrer Testumgebung übereinstimmen. |
| Mehrdeutige Fehler bei Computezielen | Das Löschen und erneute Erstellen von Computezielen kann bestimmte Probleme mit Computezielen beheben. |
| Pipeline verwendet Schritte nicht wieder | Die Wiederverwendung von Schritten ist standardmäßig aktiviert, aber stellen Sie sicher, dass Sie sie in einem Pipelineschritt nicht deaktiviert haben. Wenn die Wiederverwendung deaktiviert ist, wird der Parameter `allow_reuse` im Schritt auf `False` festgelegt. |
| Pipeline wird unnötigerweise erneut ausgeführt | Um sicherzustellen, dass Schritte nur dann erneut ausgeführt werden, wenn sich die zugrunde liegenden Daten oder Skripts ändern, entkoppeln Sie Ihre Verzeichnisse für die einzelnen Schritte. Wenn Sie dasselbe Quellverzeichnis für mehrere Schritte verwenden, kann es zu unnötigen Wiederholungen kommen. Verwenden Sie den Parameter `source_directory` in einem Pipelineschrittobjekt, um auf Ihr isoliertes Verzeichnis für diesen Schritt zu verweisen, und stellen Sie sicher, dass Sie nicht denselben `source_directory`-Pfad für mehrere Schritte verwenden. |

### <a name="logging-options-and-behavior"></a>Protokollierungsoptionen und -verhalten

In der nachstehenden Tabelle finden Sie Informationen zu verschiedenen Debugoptionen für Pipelines. Es handelt sich nicht um eine vollständige Liste, da neben den hier gezeigten Optionen für Azure Machine Learning, Python und OpenCensus auch andere vorhanden sind.

| Bibliothek                    | type   | Beispiel                                                          | Destination                                  | Ressourcen                                                                                                                                                                                                                                                                                                                    |
|----------------------------|--------|------------------------------------------------------------------|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Azure Machine Learning SDK | Metrik | `run.log(name, val)`                                             | Benutzeroberfläche des Azure Machine Learning-Portals             | [Nachverfolgen von Experimenten](how-to-track-experiments.md#available-metrics-to-track)<br>[Klasse „azureml.core.Run“](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=experimental)                                                                                                                                                 |
| Python-Druck/-Protokollierung    | Log    | `print(val)`<br>`logging.info(message)`                          | Treiberprotokolle, Azure Machine Learning-Designer | [Nachverfolgen von Experimenten](how-to-track-experiments.md#available-metrics-to-track)<br><br>[Python-Protokollierung](https://docs.python.org/2/library/logging.html)                                                                                                                                                                       |
| OpenCensus Python          | Log    | `logger.addHandler(AzureLogHandler())`<br>`logging.log(message)` | Application Insights – Ablaufverfolgung                | [Debuggen von Pipelines in Application Insights](how-to-debug-pipelines-application-insights.md)<br><br>[OpenCensus Azure Monitor Exporters](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-azure)<br>[Cookbook zur Python-Protokollierung](https://docs.python.org/3/howto/logging-cookbook.html) |

#### <a name="logging-options-example"></a>Beispiel für Protokollierungsoptionen

```python
import logging

from azureml.core.run import Run
from opencensus.ext.azure.log_exporter import AzureLogHandler

run = Run.get_context()

# Azure ML Scalar value logging
run.log("scalar_value", 0.95)

# Python print statement
print("I am a python print statement, I will be sent to the driver logs.")

# Initialize python logger
logger = logging.getLogger(__name__)
logger.setLevel(args.log_level)

# Plain python logging statements
logger.debug("I am a plain debug statement, I will be sent to the driver logs.")
logger.info("I am a plain info statement, I will be sent to the driver logs.")

handler = AzureLogHandler(connection_string='<connection string>')
logger.addHandler(handler)

# Python logging with OpenCensus AzureLogHandler
logger.warning("I am an OpenCensus warning statement, find me in Application Insights!")
logger.error("I am an OpenCensus error statement with custom dimensions", {'step_id': run.id})
``` 

## <a name="debug-and-troubleshoot-in-azure-machine-learning-designer-preview"></a>Debuggen und Problembehandlung im Azure Machine Learning Designer (Vorschau)

Dieser Abschnitt bietet eine Übersicht zur Problembehandlung für Pipelines im Designer.
Die **Protokolldateien** für die im Designer erstellten Pipelines finden Sie entweder auf der Seite zur Dokumenterstellung oder auf der Detailseite zur Pipelineausführung.

### <a name="access-logs-from-the-authoring-page"></a>Zugreifen auf Protokolle auf der Seite zur Dokumenterstellung

Wenn Sie eine Pipelineausführung übermitteln und auf der Seite zur Dokumenterstellung bleiben, finden Sie die für die einzelnen Module generierten Protokolldateien.

1. Wählen Sie ein beliebiges Modul im Zeichenbereich der Dokumenterstellung aus.
1. Wechseln Sie im Eigenschaftenbereich zur Registerkarte **Protokolle**.
1. Wählen Sie die Protokolldatei `70_driver_log.txt` aus.

    ![Modulprotokolle auf der Seite zur Dokumenterstellung](./media/how-to-debug-pipelines/pipelinerun-05.png)

### <a name="access-logs-from-pipeline-runs"></a>Zugreifen auf Protokolle von Pipelineausführungen

Sie finden die Protokolldateien bestimmter Ausführungen auch auf der Detailseite für Pipelineausführungen, entweder im Abschnitt **Pipelines** oder im Abschnitt **Experimente**.

1. Wählen Sie eine im Designer erstellte Pipelineausführung aus.
    ![Pipelineausführungsseite](./media/how-to-debug-pipelines/pipelinerun-04.png)
1. Wählen Sie im Vorschaubereich ein beliebiges Modul aus.
1. Wechseln Sie im Eigenschaftenbereich zur Registerkarte **Protokolle**.
1. Wählen Sie die Protokolldatei `70_driver_log.txt` aus.

## <a name="debug-and-troubleshoot-in-application-insights"></a>Debuggen und Problembehandlung in Application Insights
Weitere Informationen zur Verwendung der OpenCensus-Python-Bibliothek in diesem Zusammenhang finden Sie in diesem Handbuch: [Debugging und Problembehandlung für Pipelines des maschinellen Lernens in Application Insights](how-to-debug-pipelines-application-insights.md)

## <a name="debug-and-troubleshoot-in-visual-studio-code"></a>Debuggen und Behandeln von Problemen in Visual Studio Code

In einigen Fällen muss der in Ihrer ML-Pipeline verwendete Python-Code ggf. interaktiv debugged werden. Mit Visual Studio Code (VS Code) und Python Tools für Visual Studio (PTVSD) können Sie den Code debuggen, während er in der Trainingsumgebung ausgeführt wird.

### <a name="prerequisites"></a>Voraussetzungen

* __Azure Machine Learning-Arbeitsbereich__, die für die Verwendung eines __virtuellen Azure-Netzwerks__ konfiguriert ist.
* __Azure Machine Learning-Pipeline__, deren Schritte Python-Skripts enthalten (Beispiel: PythonScriptStep).
* Azure Machine Learning Compute-Cluster, der sich __im virtuellen Netzwerk__ befindet und __von der Pipeline zum Trainieren verwendet wird__.
* __Entwicklungsumgebung__, die sich __im virtuellen Netzwerk__ befindet. Mögliche Entwicklungsumgebung wären etwa:

    * Ein virtueller Azure-Computer im virtuellen Netzwerk
    * Eine Compute-Instanz einer Notebook-VM im virtuellen Netzwerk
    * Ein Clientcomputer, der über ein virtuelles privates Netzwerk (VPN) mit dem virtuellen Netzwerk verbunden ist

Weitere Informationen zur Verwendung eines virtuellen Azure-Netzwerks mit Azure Machine Learning finden Sie unter [Sichern von Azure ML-Experiment- und Rückschlussaufträgen in einem virtuellen Azure-Netzwerk](how-to-enable-virtual-network.md).

### <a name="how-it-works"></a>Funktionsweise

Von Ihren ML-Pipelineschritten werden Python-Skripts ausgeführt. Diese Skripts werden geändert, um folgende Aktionen auszuführen:
    
1. Protokollieren der IP-Adresse des Hosts, auf dem sie ausgeführt werden. Die IP-Adresse wird verwendet, um den Debugger mit dem Skript zu verbinden.

2. Starten der PTVSD-Debugkomponente und Warten, bis von einem Debugger eine Verbindung hergestellt wird

3. Von Ihrer Entwicklungsumgebung aus überwachen Sie die Protokolle, die durch den Trainingsprozess erstellt wurden, um die IP-Adresse zu ermitteln, unter der das Skript ausgeführt wird.

4. Sie geben die IP-Adresse, mit der der Debugger verbunden werden soll, an VS Code weiter (unter Verwendung einer Datei vom Typ `launch.json`).

5. Sie fügen den Debugger an, und durchlaufen interaktiv die Schritte des Skripts.

### <a name="configure-python-scripts"></a>Konfigurieren von Python-Skripts

Ändern Sie die von Schritten in Ihrer ML-Pipeline verwendeten Python-Skripts wie folgt, um das Debuggen zu ermöglichen:

1. Fügen Sie die folgenden import-Anweisungen hinzu:

    ```python
    import ptvsd
    import socket
    from azureml.core import Run
    ```

1. Fügen Sie die folgenden Argumente hinzu. Diese Argumente ermöglichen die bedarfsgerechte Aktivierung des Debuggers sowie die Festlegung des Timeouts für das Anfügen des Debuggers:

    ```python
    parser.add_argument('--remote_debug', action='store_true')
    parser.add_argument('--remote_debug_connection_timeout', type=int,
                    default=300,
                    help=f'Defines how much time the Azure ML compute target '
                    f'will await a connection from a debugger client (VSCODE).')
    ```

1. Fügen Sie die folgenden Anweisungen hinzu. Diese Anweisungen dienen zum Laden des aktuellen Ausführungskontexts, um die IP-Adresse des Knotens protokollieren zu können, auf dem der Code ausgeführt wird:

    ```python
    global run
    run = Run.get_context()
    ```

1. Fügen Sie eine `if`-Anweisung hinzu, die PTVSD startet und auf das Anfügen eines Debuggers wartet. Wenn innerhalb des Timeouts kein Debugger angefügt wird, wird das Skript normal fortgesetzt.

    ```python
    if args.remote_debug:
        print(f'Timeout for debug connection: {args.remote_debug_connection_timeout}')
        # Log the IP and port
        ip = socket.gethostbyname(socket.gethostname())
        print(f'ip_address: {ip}')
        ptvsd.enable_attach(address=('0.0.0.0', 5678),
                            redirect_output=True)
        # Wait for the timeout for debugger to attach
        ptvsd.wait_for_attach(timeout=args.remote_debug_connection_timeout)
        print(f'Debugger attached = {ptvsd.is_attached()}')
    ```

Das folgende Python-Beispiel zeigt eine einfache Datei vom Typ `train.py`, die Debuggen ermöglicht:

```python
# Copyright (c) Microsoft. All rights reserved.
# Licensed under the MIT license.

import argparse
import os
import ptvsd
import socket
from azureml.core import Run

print("In train.py")
print("As a data scientist, this is where I use my training code.")

parser = argparse.ArgumentParser("train")

parser.add_argument("--input_data", type=str, help="input data")
parser.add_argument("--output_train", type=str, help="output_train directory")

# Argument check for remote debugging
parser.add_argument('--remote_debug', action='store_true')
parser.add_argument('--remote_debug_connection_timeout', type=int,
                    default=300,
                    help=f'Defines how much time the AML compute target '
                    f'will await a connection from a debugger client (VSCODE).')
# Get run object, so we can find and log the IP of the host instance
global run
run = Run.get_context()

args = parser.parse_args()

# Start debugger if remote_debug is enabled
if args.remote_debug:
    print(f'Timeout for debug connection: {args.remote_debug_connection_timeout}')
    # Log the IP and port
    ip = socket.gethostbyname(socket.gethostname())
    print(f'ip_address: {ip}')
    ptvsd.enable_attach(address=('0.0.0.0', 5678),
                        redirect_output=True)
    # Wait for the timeout for debugger to attach
    ptvsd.wait_for_attach(timeout=args.remote_debug_connection_timeout)
    print(f'Debugger attached = {ptvsd.is_attached()}')

print("Argument 1: %s" % args.input_data)
print("Argument 2: %s" % args.output_train)

if not (args.output_train is None):
    os.makedirs(args.output_train, exist_ok=True)
    print("%s created" % args.output_train)
```

### <a name="configure-ml-pipeline"></a>Konfigurieren der ML-Pipeline

Erstellen Sie eine [Umgebung](), und legen Sie `pip_packages=['ptvsd', 'azureml-sdk==1.0.83']` fest, um die zum Starten von PTVSD erforderlichen Python-Pakete bereitzustellen und den Ausführungskontext abzurufen. Legen Sie die SDK-Version auf die von Ihnen verwendete Version fest. Im folgenden Codeausschnitt wird die Vorgehensweise zum Erstellen einer Umgebung veranschaulicht:

```python
# Use a RunConfiguration to specify some additional requirements for this step.
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core.runconfig import DEFAULT_CPU_IMAGE

# create a new runconfig object
run_config = RunConfiguration()

# enable Docker 
run_config.environment.docker.enabled = True

# set Docker base image to the default CPU-based image
run_config.environment.docker.base_image = DEFAULT_CPU_IMAGE

# use conda_dependencies.yml to create a conda environment in the Docker image for execution
run_config.environment.python.user_managed_dependencies = False

# specify CondaDependencies obj
run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'],
                                                                           pip_packages=['ptvsd', 'azureml-sdk==1.0.83'])
```

Im Abschnitt [Konfigurieren von Python-Skripts](#configure-python-scripts) wurden den von Ihren ML-Pipelineschritten verwendeten Skripts zwei neue Argumente hinzugefügt. Der folgende Codeausschnitt zeigt, wie diese Argumente verwendet werden, um das Debuggen für die Komponente zu aktivieren und ein Timeout festzulegen. Außerdem wird gezeigt, wie Sie die zuvor erstellte Umgebung verwenden, indem Sie `runconfig=run_config` festlegen:

```python
# Use RunConfig from a pipeline step
step1 = PythonScriptStep(name="train_step",
                         script_name="train.py",
                         arguments=['--remote_debug', '--remote_debug_connection_timeout', 300],
                         compute_target=aml_compute, 
                         source_directory=source_directory,
                         runconfig=run_config,
                         allow_reuse=False)
```

Wenn die Pipeline ausgeführt wird, wird durch jeden Schritt eine untergeordnete Ausführung erstellt. Ist das Debuggen aktiviert, protokolliert das geänderte Skript Informationen wie die folgenden in der Datei `70_driver_log.txt` für die untergeordnete Ausführung:

```text
Timeout for debug connection: 300
ip_address: 10.3.0.5
```

Speichern Sie den Wert von `ip_address`. Sie wird im nächsten Abschnitt verwendet.

> [!TIP]
> Die IP-Adresse finden Sie auch in den Ausführungsprotokollen für die untergeordnete Ausführung dieses Pipelineschritts. Weitere Informationen zum Anzeigen dieser Informationen finden Sie unter [Überwachen von Azure ML-Experimentausführungen und -metriken](how-to-track-experiments.md).

### <a name="configure-development-environment"></a>Konfigurieren der Entwicklungsumgebung

1. Verwenden Sie den folgenden Befehl, um Python Tools für Visual Studio (PTVSD) in Ihrer VS Code-Entwicklungsumgebung zu installieren:

    ```
    python -m pip install --upgrade ptvsd
    ```

    Weitere Informationen zur Verwendung von PTVSD mit VS Code finden Sie unter [Remotedebuggen](https://code.visualstudio.com/docs/python/debugging#_remote-debugging).

1. Erstellen Sie eine neue Debugkonfiguration, um VS Code für die Kommunikation mit der Azure Machine Learning Compute-Instanz zu konfigurieren, von der der Debugger ausgeführt wird:

    1. Wählen Sie aus VS Code das Menü __Debuggen__ aus, und wählen Sie dann __Konfigurationen öffnen__ aus. Eine Datei namens __launch.json__ wird geöffnet.

    1. Suchen Sie in der Datei __launch.json__ nach der Zeile, die `"configurations": [` enthält, und fügen Sie danach den folgenden Text ein. Ändern Sie den Eintrag `"host": "10.3.0.5"` in die IP-Adresse, die in Ihren Protokollen aus dem vorherigen Abschnitt zurückgegeben wurde. Ändern Sie den Eintrag `"localRoot": "${workspaceFolder}/code/step"` in ein lokales Verzeichnis, das eine Kopie des zu debuggenden Skripts enthält:

        ```json
        {
            "name": "Azure Machine Learning Compute: remote debug",
            "type": "python",
            "request": "attach",
            "port": 5678,
            "host": "10.3.0.5",
            "redirectOutput": true,
            "pathMappings": [
                {
                    "localRoot": "${workspaceFolder}/code/step1",
                    "remoteRoot": "."
                }
            ]
        }
        ```

        > [!IMPORTANT]
        > Wenn es bereits andere Einträge im Konfigurationsabschnitt gibt, fügen Sie ein Komma (,) nach dem von Ihnen eingegebenen Code ein.

        > [!TIP]
        > Es empfiehlt sich, die Ressourcen für Skripts in separaten Verzeichnissen zu platzieren. Aus diesem Grund wird vom Beispielwert `localRoot` auf `/code/step1` verwiesen.
        >
        > Wenn Sie mehrere Skripts in unterschiedlichen Verzeichnissen debuggen, muss für jedes Skript ein separater Konfigurationsabschnitt erstellt werden.

    1. Speichern Sie die Datei __launch.json__.

### <a name="connect-the-debugger"></a>Herstellen einer Verbindung für den Debugger

1. Öffnen Sie in VS Code eine lokale Kopie des Skripts.
2. Legen Sie Breakpoints an den Stellen fest, an denen das Skript nach dem Anfügen angehalten werden soll.
3. Drücken Sie F5, oder wählen Sie __Debuggen__ aus, während der untergeordnete Prozess das Skript ausführt und das Timeout für die Debugverbindung (`Timeout for debug connection`) in den Protokollen angezeigt wird. Wählen Sie die Konfiguration __Azure Machine Learning Compute: remote debug__ aus, wenn Sie dazu aufgefordert werden. Sie können auch das Debugsymbol auf der Seitenleiste, den Eintrag __Azure Machine Learning: remote debug__ aus dem Dropdownmenü „Debuggen“ und anschließend den grünen Pfeil auswählen, um den Debugger anzufügen.

    An diesem Punkt stellt VS Code eine Verbindung mit PTVSD auf dem Computeknoten her und hält den Vorgang an dem Breakpoint an, den Sie zuvor festgelegt haben. Sie können den Code jetzt während der Ausführung schrittweise durchlaufen, Variablen anzeigen usw.

    > [!NOTE]
    > Wenn das Protokoll einen Eintrag mit der Angabe `Debugger attached = False` enthält, ist das Timeout abgelaufen, und das Skript wurde ohne den Debugger fortgesetzt. Übermitteln Sie die Pipeline erneut, und verbinden Sie den Debugger nach der Nachricht `Timeout for debug connection`, aber noch vor Ablauf des Timeouts.

## <a name="next-steps"></a>Nächste Schritte

* Hilfe zu den Paketen [azureml-pipelines-core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) und [azureml-pipelines-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) finden Sie in der SDK-Referenz.

* Weitere Informationen finden Sie in der Liste [Designer-Ausnahmen und-Fehlercodes](algorithm-module-reference/designer-error-codes.md).
