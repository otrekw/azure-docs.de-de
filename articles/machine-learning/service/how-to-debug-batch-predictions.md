---
title: Debugging und Problembehandlung von ParallelRunStep
titleSuffix: Azure Machine Learning
description: Debuggen und Problembehandlung für Machine Learning-Pipelines im Azure Machine Learning SDK für Python. Lernen Sie häufige Fallstricke bei der Entwicklung mit Pipelines kennen, und erhalten Sie Tipps, die Ihnen helfen, Ihre Skripts vor und während der Remoteausführung zu debuggen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: trbye, jmartens, larryfr, vaidyas
ms.author: trmccorm
author: tmccrmck
ms.date: 11/21/2019
ms.openlocfilehash: db17dca1a859807f4d92b4953ec0e2785a3a4160
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2019
ms.locfileid: "74851976"
---
# <a name="debug-and-troubleshoot-using-parallelrun"></a>Debugging und Problembehandlung mit ParallelRun
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie Debugging und Problembehandlung für die [ParallelRunStep](https://docs.microsoft.com/python/api/azureml-contrib-pipeline-steps/azureml.contrib.pipeline.steps.parallel_run_step.parallelrunstep?view=azure-ml-py)-Klasse im [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) durchführen.

## <a name="testing-scripts-locally"></a>Lokales Testen von Skripts

Weitere Informationen finden Sie im Abschnitt [Lokales Testen von Skripts](how-to-debug-pipelines.md#testing-scripts-locally) für Pipelines des maschinellen Lernens. Ihre ParallelRunStep-Instanz wird als Schritt in ML-Pipelines ausgeführt, sodass die gleiche Antwort für beide gilt.

## <a name="debugging-scripts-from-remote-context"></a>Debuggen von Skripts aus Remotekontext

Der Übergang vom lokalen Debuggen eines Bewertungsskripts zum Debuggen eines Bewertungsskripts in einer echten Pipeline kann einen schwierigen Schritt darstellen. Informationen zum Suchen Ihrer Protokolle im Portal finden Sie im [Abschnitt zu Pipelines des maschinellen Lernens über das Debuggen von Skripts aus einem Remotekontext](how-to-debug-pipelines.md#debugging-scripts-from-remote-context). Die Informationen in diesem Abschnitt gelten auch für das Ausführen eines Batchrückschlusses.

Beispielsweise enthält die Protokolldatei `70_driver_log.txt` auch Folgendes: 

* Alle während der Ausführung Ihres Skripts ausgegebenen Anweisungen
* Die Stapelüberwachung des Skripts

Aufgrund der verteilten Struktur von Batchrückschlussaufträgen gibt es Protokolle aus mehreren verschiedenen Quellen. Es werden jedoch zwei konsolidierte Dateien erstellt, die allgemeine Informationen bieten:

- `~/logs/overview.txt`: Diese Datei enthält allgemeine Informationen zur Anzahl der bisher erstellten Minibatches (auch als Aufgaben bezeichnet) und der Anzahl der bisher verarbeiteten Minibatches. Am Ende wird das Ergebnis des Auftrags aufgeführt. Wenn bei dem Auftrag ein Fehler aufgetreten ist, wird die Fehlermeldung angezeigt, und es wird beschrieben, wo mit der Problembehandlung begonnen werden sollte.

- `~/logs/master.txt`: Diese Datei stellt die Masterknotenansicht (auch als Orchestrator bezeichnet) des laufenden Auftrags bereit. Dies umfasst die Aufgabenerstellung, die Fortschrittsüberwachung und das Ergebnis der Ausführung.

Wenn Sie detailliert erfahren möchten, wie die einzelnen Knoten das Bewertungsskript ausgeführt haben, sehen Sie sich die jeweiligen Prozessprotokolle für jeden Knoten an. Die Prozessprotokolle befinden sich im Ordner `worker`, gruppiert nach Workerknoten:

- `~/logs/worker/<ip_address>/Process-*.txt`: Diese Datei enthält ausführliche Informationen zu den einzelnen Minibatches, die von einem Worker abgerufen oder abgeschlossen werden. Für jeden Minibatch umfasst diese Datei Folgendes:

    - Die IP-Adresse und die PID des Workerprozesses. 
    - Die Gesamtanzahl der Elemente und die Anzahl der erfolgreich verarbeiteten Elemente. 
    - Die Start- und Endzeit als reguläre Uhrzeit (`start1` und `end1`). 
    - Die Start- und Endzeit als aufgewendete Prozessorzeit (`start2` und `end2`). 

Sie finden auch Informationen zur Ressourcennutzung der Prozesse für jeden Worker. Diese Informationen liegen im CSV-Format vor und befinden sich unter `~/logs/performance/<ip_address>/`. Wenn Sie z. B. die Ressourcennutzung überprüfen, sehen Sie sich die folgenden Dateien an:

- `process_resource_monitor_<ip>_<pid>.csv`: Ressourcennutzung pro Workerprozess. 
- `sys_resource_monitor_<ip>.csv`: Protokoll pro Knoten.

### <a name="how-do-i-log-from-my-user-script-from-a-remote-context"></a>Wie protokolliere ich mein Benutzerskript aus einem Remotekontext?
Sie können über die folgenden Schritte eine Protokollierung einrichten, um die Protokolle im Ordner **logs/users** im Portal anzuzeigen:
1. Speichern Sie den ersten Codeabschnitt unten in der Datei „entry_script_helper.py“, und platzieren Sie die Datei im selben Ordner wie das Eingabeskript. Diese Klasse ruft den Pfad in AmlCompute ab. Zum lokalen Testen können Sie `get_working_dir()` ändern, sodass ein lokaler Ordner zurückgegeben wird.
2. Konfigurieren Sie eine Protokollierung in Ihrer `init()`-Methode, und verwenden Sie sie dann. Der zweite Codeabschnitt unten ist ein Beispiel. 

**entry_script_helper.py:**
```python
"""
This module provides helper features for entry script.
This file should be in Python search paths or in the same folder as the entry script.
"""
import os
import socket
import logging
import time
from multiprocessing import current_process
from azureml.core import Run


class EntryScriptHelper:
    """ A helper to provide common features for entry script."""

    LOG_CONFIGED = False

    def get_logger(self, name="EntryScript"):
        """ Return a logger.
            The logger will write to the 'users' folder and show up in azure portal.
        """
        return logging.getLogger(name)

    def config(self, name="EntryScript", level="INFO"):
        """ Config a logger. This should be called in init() in score module.
            Config the logger one time if not configed.
            The logger will write to the 'users' folder and show up in azure portal.
        """
        logger = logging.getLogger(name)

        formatter = logging.Formatter(
            "%(asctime)s|%(name)s|%(levelname)s|%(process)d|%(thread)d|%(funcName)s()|%(message)s"
        )
        formatter.converter = time.gmtime

        logger.setLevel(level)

        handler = logging.FileHandler(self.get_log_file_path())
        handler.setLevel(level)
        handler.setFormatter(formatter)
        logger.addHandler(handler)

        return logger

    def get_log_file_path(self):
        """ Get the log file path for users.
            Each process has its own log file, so there is not race issue among multiple processes.
        """
        ip_address = socket.gethostbyname(socket.gethostname())
        log_dir = os.path.join(self.get_log_dir(), "user", ip_address)
        os.makedirs(log_dir, exist_ok=True)
        return os.path.join(log_dir, f"{current_process().name}.txt")

    def get_log_dir(self):
        """ Return the folder for logs.
            Files and folders in it will be uploaded and show up in run detail page in the azure portal.
        """
        log_dir = os.path.join(self.get_working_dir(), "logs")
        os.makedirs(log_dir, exist_ok=True)
        return log_dir

    def get_working_dir(self):
        """ Return the working directory."""
        return os.path.join(os.environ.get("AZ_BATCHAI_INPUT_AZUREML", ""), self.get_run().id)

    def get_temp_dir(self):
        """ Return local temp directory."""
        local_temp_dir = os.path.join(
            os.environ.get("AZ_BATCHAI_JOB_TEMP", ""), "azureml-bi", str(os.getpid())
        )
        os.makedirs(local_temp_dir, exist_ok=True)
        return local_temp_dir

    def get_run(self):
        """ Return the Run from the context."""
        return Run.get_context(allow_offline=False)

```

**Ein Beispiel für Eingabeskript mit der Protokollierung:**
```python
"""
This is a sample scoring module.

This module provides a sample which passes the input back without any change.
"""
import os
import logging
from entry_script_helper import EntryScriptHelper

LOG_NAME = "score_file_list"


def init():
    """ Init """
    EntryScriptHelper().config(LOG_NAME)
    logger = logging.getLogger(LOG_NAME)
    output_folder = os.path.join(os.environ.get("AZ_BATCHAI_INPUT_AZUREML", ""), "temp/output")
    logger.info(f"{__file__}.output_folder:{output_folder}")
    logger.info("init()")
    os.makedirs(output_folder, exist_ok=True)


def run(mini_batch):
    """ Accept and return the list back."""
    logger = logging.getLogger(LOG_NAME)
    logger.info(f"{__file__}, run({mini_batch})")

    output_folder = os.path.join(os.environ.get("AZ_BATCHAI_INPUT_AZUREML", ""), "temp/output")
    for file_name in mini_batch:
        with open(file_name, "r") as file:
            lines = file.readlines()
        base_name = os.path.basename(file_name)
        name = os.path.join(output_folder, base_name)
        logger.info(f"{__file__}: {name}")
        with open(name, "w") as file:
            file.write(f"ouput file {name} from {__file__}:\n")
            for line in lines:
                file.write(line)

    return mini_batch
```

## <a name="next-steps"></a>Nächste Schritte

* In der SDK-Referenz finden Sie Hilfe zum Paket [azureml-contrib-pipeline-step](https://docs.microsoft.com/python/api/azureml-contrib-pipeline-steps/azureml.contrib.pipeline.steps?view=azure-ml-py) und die [Dokumentation](https://docs.microsoft.com/python/api/azureml-contrib-pipeline-steps/azureml.contrib.pipeline.steps.parallelrunstep?view=azure-ml-py) für die ParallelRunStep-Klasse.

* Befolgen Sie das [erweiterte Tutorial](tutorial-pipeline-batch-scoring-classification.md) zur Verwendung von Pipelines für die Batchbewertung.
