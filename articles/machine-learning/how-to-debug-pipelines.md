---
title: Debuggen und Problembehandlung für Machine Learning-Pipelines
titleSuffix: Azure Machine Learning
description: Debuggen und Problembehandlung für Machine Learning-Pipelines im Azure Machine Learning SDK für Python. Lernen Sie häufige Fallstricke bei der Entwicklung von Pipelines kennen, und erhalten Sie Tipps, die Ihnen helfen, Ihre Skripts vor und während der Remoteausführung zu debuggen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.date: 12/12/2019
ms.openlocfilehash: 991f7ebf51be5f805a8b12fa0af0fefeff0ef582
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76309556"
---
# <a name="debug-and-troubleshoot-machine-learning-pipelines"></a>Debuggen und Problembehandlung für Machine Learning-Pipelines
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie das Debuggen und Troubleshooting für [Pipelines für maschinelles Lernen](concept-ml-pipelines.md) im [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) und im [Azure Machine Learning-Designer](https://docs.microsoft.com/azure/machine-learning/concept-designer) durchführen.


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

## <a name="debug-and-troubleshoot-in-azure-machine-learning-designer"></a>Debuggen und Troubleshooting im Azure Machine Learning-Designer

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

## <a name="next-steps"></a>Nächste Schritte

* Hilfe zu den Paketen [azureml-pipelines-core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) und [azureml-pipelines-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) finden Sie in der SDK-Referenz.

* Weitere Informationen finden Sie in der Liste [Designer-Ausnahmen und-Fehlercodes](algorithm-module-reference/designer-error-codes.md).
