---
title: Debuggen und Troubleshooting bei ML-Pipelines
titleSuffix: Azure Machine Learning
description: Debuggen Sie Ihre Azure Machine Learning-Pipelines in Python. Lernen Sie häufige Fallstricke bei der Entwicklung von Pipelines kennen, und erhalten Sie Tipps, die Ihnen helfen, Ihre Skripts vor und während der Remoteausführung zu debuggen. Erfahren Sie, wie Sie Ihre Machine Learning-Pipelines mithilfe von Visual Studio Code interaktiv debuggen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
author: lobrien
ms.author: laobri
ms.date: 08/28/2020
ms.topic: conceptual
ms.custom: troubleshooting, devx-track-python
ms.openlocfilehash: be68ad35deca754df70bb51e83929e73ff132ba6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91315404"
---
# <a name="debug-and-troubleshoot-machine-learning-pipelines"></a>Debuggen und Problembehandlung für Machine Learning-Pipelines

In diesem Artikel erfahren Sie, wie Sie das Debuggen und Troubleshooting für [Pipelines für maschinelles Lernen](concept-ml-pipelines.md) im [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py&preserve-view=true) und im [Azure Machine Learning-Designer](https://docs.microsoft.com/azure/machine-learning/concept-designer) durchführen. Folgendes wird vermittelt:

## <a name="troubleshooting-tips"></a>Tipps zur Problembehandlung

Die folgende Tabelle enthält häufige Probleme bei der Pipelinentwicklung mit möglichen Lösungen.

| Problem | Mögliche Lösung |
|--|--|
| Daten können nicht in das Verzeichnis `PipelineData` übergeben werden | Stellen Sie sicher, dass Sie im Skript ein Verzeichnis erstellt haben, das dem entspricht, wo Ihre Pipeline die Schrittausgabedaten erwartet. In den meisten Fällen definiert ein Eingabeargument das Ausgabeverzeichnis, und dann erstellen Sie das Verzeichnis explizit. Verwenden Sie `os.makedirs(args.output_dir, exist_ok=True)`, um das Ausgabeverzeichnis zu erstellen. Im [Tutorial](tutorial-pipeline-batch-scoring-classification.md#write-a-scoring-script) finden Sie ein Beispiel für ein Bewertungsskript, das dieses Entwurfsmuster zeigt. |
| Fehler bei Abhängigkeiten | Wenn in Ihrer Remotepipeline Abhängigkeitsfehler angezeigt werden, die beim lokalen Testen nicht aufgetreten sind, vergewissern Sie sich, dass die Abhängigkeiten und Versionen Ihrer Remoteumgebung mit denen in Ihrer Testumgebung übereinstimmen. (Siehe [Erstellen, Zwischenspeichern und Wiederverwenden von Umgebungen](https://docs.microsoft.com/azure/machine-learning/concept-environments#environment-building-caching-and-reuse))|
| Mehrdeutige Fehler bei Computezielen | Versuchen Sie, Computeziele zu löschen und neu zu erstellen. Die Neuerstellung von Computezielen erfolgt schnell und kann vorübergehende Probleme beheben. |
| Pipeline verwendet Schritte nicht wieder | Die Wiederverwendung von Schritten ist standardmäßig aktiviert, aber stellen Sie sicher, dass Sie sie in einem Pipelineschritt nicht deaktiviert haben. Wenn die Wiederverwendung deaktiviert ist, wird der Parameter `allow_reuse` im Schritt auf `False` festgelegt. |
| Pipeline wird unnötigerweise erneut ausgeführt | Um sicherzustellen, dass Schritte nur dann erneut ausgeführt werden, wenn die zugrunde liegenden Daten oder Skripts geändert werden, entkoppeln Sie Ihre Quellcodeverzeichnisse für die einzelnen Schritte. Wenn Sie dasselbe Quellverzeichnis für mehrere Schritte verwenden, kann es zu unnötigen Wiederholungen kommen. Verwenden Sie den Parameter `source_directory` in einem Pipelineschrittobjekt, um auf Ihr isoliertes Verzeichnis für diesen Schritt zu verweisen, und stellen Sie sicher, dass Sie nicht denselben `source_directory`-Pfad für mehrere Schritte verwenden. |
| Verlangsamter Schritt über Trainingsepochen oder andere Schleifenverhalten | Versuchen Sie, alle Dateischreibvorgänge, einschließlich der Protokollierung, von `as_mount()` zu `as_upload()` zu wechseln. Im Modus für die **Einbindung** wird ein remotes virtualisiertes Dateisystem verwendet, und die gesamte Datei wird jedes Mal dann hochgeladen, wenn er angefügt wird. |

## <a name="debugging-techniques"></a>Debuggingverfahren

Es gibt drei Haupttechniken zum Debuggen von Pipelines: 

* Debuggen der einzelnen Pipelineschritte auf dem lokalen Computer
* Verwenden von Protokollierung und Application Insights zum Isolieren und Diagnostizieren der Problemursache
* Anfügen eines Remotedebuggers an eine in Azure ausgeführte Pipeline

### <a name="debug-scripts-locally"></a>Lokales Testen von Skripts

Einer der häufigsten Fehler in einer Pipeline besteht darin, dass das Domänenskript nicht wie beabsichtigt ausgeführt wird oder Laufzeitfehler im Remotecomputekontext enthält, die schwierig zu debuggen sind.

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

## <a name="configure-write-to-and-review-pipeline-logs"></a>Konfigurieren und Überprüfen von Pipelineprotokollen sowie Schreiben in diese

Das lokale Testen von Skripts ist eine hervorragende Möglichkeit, wichtige Codefragmente und eine komplexe Logik zu debuggen, bevor Sie mit dem Erstellen einer Pipeline beginnen. Aber irgendwann werden Sie Skripts wahrscheinlich während der eigentlichen Pipelineausführung selbst debuggen müssen, insbesondere bei der Diagnose von Verhaltensweisen, die während der Interaktion zwischen Pipelineschritten auftreten. Wir empfehlen die großzügige Verwendung von `print()`-Anweisungen in Ihren Schrittskripts, damit Sie den Objektzustand und die erwarteten Werte während der Remoteausführung sehen können, ähnlich wie beim Debuggen von JavaScript-Code.

### <a name="logging-options-and-behavior"></a>Protokollierungsoptionen und -verhalten

In der nachstehenden Tabelle finden Sie Informationen zu verschiedenen Debugoptionen für Pipelines. Es handelt sich nicht um eine vollständige Liste, da neben den hier gezeigten Optionen für Azure Machine Learning, Python und OpenCensus auch andere vorhanden sind.

| Bibliothek                    | type   | Beispiel                                                          | Destination                                  | Ressourcen                                                                                                                                                                                                                                                                                                                    |
|----------------------------|--------|------------------------------------------------------------------|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Azure Machine Learning SDK | Metrik | `run.log(name, val)`                                             | Benutzeroberfläche des Azure Machine Learning-Portals             | [Nachverfolgen von Experimenten](how-to-track-experiments.md)<br>[Klasse „azureml.core.Run“](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py&preserve-view=true)                                                                                                                                                 |
| Python-Druck/-Protokollierung    | Log    | `print(val)`<br>`logging.info(message)`                          | Treiberprotokolle, Azure Machine Learning-Designer | [Nachverfolgen von Experimenten](how-to-track-experiments.md)<br><br>[Python-Protokollierung](https://docs.python.org/2/library/logging.html)                                                                                                                                                                       |
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

## <a name="azure-machine-learning-designer"></a>Azure Machine Learning-Designer

Die **70_driver_log**-Dateien für die im Designer erstellten Pipelines finden Sie entweder auf der Seite zur Dokumenterstellung oder auf der Detailseite zur Pipelineausführung.

### <a name="enable-logging-for-real-time-endpoints"></a>Aktivieren der Protokollierung für Echtzeitendpunkte

Für die Problembehandlung und das Debuggen von Echtzeitendpunkten im Designer müssen Sie die Application Insights-Protokollierung mithilfe des SDK aktivieren. Mithilfe der Protokollierung können Sie Probleme bei der Modellimplementierung und -verwendung behandeln und debuggen. Weitere Informationen finden Sie unter [Protokollierung für bereitgestellte Modelle](how-to-enable-logging.md#logging-for-deployed-models). 

### <a name="get-logs-from-the-authoring-page"></a>Abrufen von Protokollen auf der Seite zur Dokumenterstellung

Wenn Sie eine Pipelineausführung übermitteln und auf der Seite zur Dokumenterstellung bleiben, finden Sie nach der Ausführung der einzelnen Module die jeweils generierten Protokolldateien.

1. Wählen Sie ein Modul aus, dessen Ausführung im Dokumenterstellungsbereich beendet wurde.
1. Wechseln Sie im rechten Bereich des Moduls zur Registerkarte **Ausgaben und Protokolle**.
1. Erweitern Sie den Bereich auf der rechten Seite, und wählen Sie die Datei **70_driver_log.txt** aus, um sie im Browser anzuzeigen. Sie können Protokolle auch lokal herunterladen.

    ![Erweiterter Ausgabebereich im Designer](./media/how-to-debug-pipelines/designer-logs.png)?view=azure-ml-py&preserve-view=true)?view=azure-ml-py&preserve-view=true)

### <a name="get-logs-from-pipeline-runs"></a>Abrufen von Protokollen von Pipelineausführungen

Sie finden die Protokolldateien bestimmter Ausführungen auch auf der Detailseite für Pipelineausführungen, entweder im Abschnitt **Pipelines** oder im Abschnitt **Experimente** des Studios.

1. Wählen Sie eine im Designer erstellte Pipelineausführung aus.

    ![Seite für Pipelineausführungen](./media/how-to-debug-pipelines/designer-pipelines.png)

1. Wählen Sie im Vorschaubereich ein Modul aus.
1. Wechseln Sie im rechten Bereich des Moduls zur Registerkarte **Ausgaben und Protokolle**.
1. Erweitern Sie den Bereich auf der rechten Seite, um die Datei **70_driver_log.txt** im Browser anzuzeigen, oder wählen Sie die Datei aus, um die Protokolle lokal herunterzuladen.

> [!IMPORTANT]
> Wenn Sie eine Pipeline über die Detailseite für Pipelineausführungen aktualisieren möchten, müssen Sie die Pipelineausführung in einen neuen Pipelineentwurf **klonen**. Pipelineausführungen sind Momentaufnahmen von Pipelines. Sie ähneln Protokolldateien und können nicht geändert werden. 

## <a name="application-insights"></a>Application Insights
Weitere Informationen zur Verwendung der OpenCensus-Python-Bibliothek in diesem Zusammenhang finden Sie in diesem Handbuch: [Debugging und Problembehandlung für Pipelines des maschinellen Lernens in Application Insights](how-to-debug-pipelines-application-insights.md)

## <a name="interactive-debugging-with-visual-studio-code"></a>Interaktives Debuggen mit Visual Studio Code

In einigen Fällen muss der in Ihrer ML-Pipeline verwendete Python-Code ggf. interaktiv debugged werden. Mit Visual Studio Code (VS Code) und debugpy können Sie am den Code anfügen, während er in der Trainingsumgebung ausgeführt wird. Weitere Informationen finden Sie im [Leitfaden zum interaktiven Debuggen in VS Code](how-to-debug-visual-studio-code.md#debug-and-troubleshoot-machine-learning-pipelines).

## <a name="next-steps"></a>Nächste Schritte

* Hilfe zu den Paketen [azureml-pipelines-core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py&preserve-view=true) und [azureml-pipelines-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py&preserve-view=true) finden Sie in der SDK-Referenz.

* Weitere Informationen finden Sie in der Liste [Designer-Ausnahmen und-Fehlercodes](algorithm-module-reference/designer-error-codes.md).
