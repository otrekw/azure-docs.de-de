---
title: 'Schnellstart: Verwenden eines Beispielnotebooks aus dem Synapse Analytics-Katalog'
description: Hier erfahren Sie, wie Sie ein Beispielnotebook aus dem Synapse Analytics-Katalog verwenden, um Daten zu untersuchen und ein Machine Learning-Modell zu erstellen.
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: quickstart
ms.reviewer: ''
ms.date: 06/11/2021
author: garyericson
ms.author: garye
ms.openlocfilehash: 3fadfc3fdead7249480b2ce841601149f750b958
ms.sourcegitcommit: 23040f695dd0785409ab964613fabca1645cef90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112063633"
---
# <a name="quickstart-use-a-sample-notebook-from-the-synapse-analytics-gallery"></a>Schnellstart: Verwenden eines Beispielnotebooks aus dem Synapse Analytics-Katalog

In dieser Schnellstartanleitung erfahren Sie, wie Sie ein Machine Learning-Beispielnotebook aus dem Synapse Analytics-Katalog in Ihren Arbeitsbereich kopieren, es ändern und ausführen.
Das Beispielnotebook erfasst das Open Dataset „NYC Taxi Trips“ und verwendet Visualisierung, um Sie bei der Aufbereitung der Daten zu unterstützen. Anschließend wird ein Modell trainiert, um vorherzusagen, ob es bei einer bestimmten Fahrt ein Trinkgeld geben wird.

Dieses Notebook veranschaulicht die grundlegenden Schritte zum Erstellen eines Modells: **Datenimport**, **Datenaufbereitung**, **Modelltraining** und **Auswertung**. Sie können dieses Beispiel als Ausgangspunkt für die Erstellung eines Modells mit Ihren eigenen Daten verwenden.

## <a name="prerequisites"></a>Voraussetzungen

* [Azure Synapse Analytics-Arbeitsbereich](../get-started-create-workspace.md) mit einem als Standardspeicher konfigurierten Azure Data Lake Storage Gen2-Speicherkonto. Für das hier verwendete Data Lake Storage Gen2-Dateisystem müssen Sie über die Rolle *Mitwirkender an Storage-Blobdaten* verfügen.
* [Spark-Pool](../get-started-analyze-spark.md) in Ihrem Azure Synapse Analytics-Arbeitsbereich.

## <a name="copy-the-notebook-to-your-workspace"></a>Kopieren des Notebooks in Ihren Arbeitsbereich

1. Öffnen Sie Ihren Arbeitsbereich, und wählen Sie auf der Startseite **Learn** aus.
1. Wählen Sie im **Knowledge Center** die Option **Katalog durchsuchen** aus.
1. Wählen Sie im Katalog **Notebooks** aus.
1. Suchen Sie das Notebook „Data Exploration and ML Modeling - NYC taxi predict using Spark MLib“ (Datenerkundung und ML-Modellierung – NYC-Taxivorhersage mit Spark MLib), und wählen Sie es aus.

   :::image type="content" source="media\quickstart-gallery-sample-notebook\gallery-select-ml-notebook.png" alt-text="Wählen Sie das Machine Learning-Beispielnotebook im Katalog aus.":::

1. Wählen Sie **Weiter**.
1. Wählen Sie auf der Notebookvorschauseite **Notebook öffnen** aus. Das Beispielnotebook wird in Ihren Arbeitsbereich kopiert und geöffnet.

    :::image type="content" source="media\quickstart-gallery-sample-notebook\gallery-open-ml-notebook.png" alt-text="Öffnen Sie das Machine Learning-Beispielnotebook in Ihrem Arbeitsbereich.":::

1. Wählen Sie im geöffneten Notebook im Menü **Anfügen an** Ihren Apache Spark-Pool aus.

## <a name="run-the-notebook"></a>Ausführen des Notebooks

Das Notebook ist in mehrere Zellen unterteilt, die jeweils eine bestimmte Funktion ausführen.
Sie können jede Zelle manuell ausführen, Zellen sequenziell ausführen oder **Alle ausführen** auswählen, um alle Zellen auszuführen.

Im Folgenden finden Sie Beschreibungen für die einzelnen Zellen im Notebook:

1. Importieren Sie PySpark-Funktionen, die das Notebook verwendet.
1. **Erfassungsdatum** – Erfassen Sie Daten aus dem Azure Open Dataset **NycTlcYellow** zur Verarbeitung in einem lokalen Datenrahmen. Der Code extrahiert Daten innerhalb eines bestimmten Zeitraums. Sie können das Anfangs- und Enddatum ändern, um unterschiedliche Daten zu erhalten.
1. Erstellen Sie ein Downsample des Datasets, um die Entwicklung zu beschleunigen. Sie können diesen Schritt ändern, um die Stichprobengröße oder den Sampling-Startwert zu ändern.
1. **Explorative Datenanalyse** – Zeigen Sie Daten in Diagrammen an. Dies kann Ihnen eine Vorstellung davon geben, welche Datenaufbereitung erforderlich sein kann, bevor Sie das Modell erstellen.
1. **Datenaufbereitung und Featurisierung** – Filtern Sie Ausreißerdaten, die durch Visualisierung gefunden wurden, und erstellen Sie einige nützliche abgeleitete Variablen.
1. **Datenaufbereitung und Featurisierung, Teil 2** – Verwerfen Sie nicht benötigte Spalten, und erstellen Sie einige zusätzliche Features.
1. **Codierung** – Konvertieren Sie Zeichenfolgenvariablen in Zahlen, die vom logistischen Regressionsmodell erwartet werden.
1. **Generierung von Test- und Trainingsdatasets** – Teilen Sie die Daten in separate Test- und Trainingsdatasets auf. Sie können den Bruch und den zufälligen Startwert ändern, der zum Aufteilen der Daten verwendet wird.
1. **Trainieren des Modells** – Trainieren Sie ein logistisches Regressionsmodell, und zeigen Sie die Metrik „Area under ROC“ (Bereich unter ROC) an, um zu sehen, wie gut das Modell funktioniert. Dieser Schritt speichert auch das trainierte Modell, falls Sie es an anderer Stelle verwenden möchten.
1. **Auswerten und Visualisieren** – Zeichnen Sie die ROC-Kurve des Modells, um das Modell weiter zu bewerten.

## <a name="save-the-notebook"></a>Speichern des Notebooks

Klicken Sie auf der Befehlsleiste des Arbeitsbereichs auf **Veröffentlichen,** um Ihr Notebook zu speichern.

## <a name="copying-the-sample-notebook"></a>Kopieren des Beispielnotebooks

Klicken Sie in der oberen Befehlsleiste auf die Auslassungspunkte, und wählen Sie **Klonen** aus, um eine Kopie dieses Notebooks in Ihrem Arbeitsbereich zu erstellen, oder **Exportieren**, um eine Kopie der Notebookdatei (`.ipynb`) herunterzuladen.

:::image type="content" source="media\quickstart-gallery-sample-notebook\copy-notebook.png" alt-text="Erstellen Sie mit dem Befehl „Exportieren“ oder „Klonen“ eine Kopie des Notebooks.":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Beenden Sie alle verbundenen Sitzungen (Notebooks), um sicherzustellen, dass die Spark-Instanz heruntergefahren wird, wenn Sie fertig sind. Der Pool wird heruntergefahren, wenn die im Apache Spark-Pool angegebene **Leerlaufzeit** erreicht wird. Sie können auch auf der Statusleiste am oberen Rand des Notebooks die Option **Sitzung beenden** auswählen.

:::image type="content" source="media\quickstart-gallery-sample-notebook\stop-session.png" alt-text="Sitzung beenden.":::

## <a name="next-steps"></a>Nächste Schritte

* [Weitere Synapse-Beispielnotebooks auf GitHub](https://github.com/Azure-Samples/Synapse/tree/main/MachineLearning)
* [Maschinelles Lernen mit Apache Spark](../spark/apache-spark-machine-learning-concept.md)
