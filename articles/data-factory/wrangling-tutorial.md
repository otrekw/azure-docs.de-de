---
title: Erste Schritte mit Wranglingdatenflüssen in Azure Data Factory
description: Tutorial zum Vorbereiten von Daten in Azure Data Factory mithilfe von Wranglingdatenflüssen
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/19/2021
ms.openlocfilehash: 3d43a532f57feab361c6a3de79269991f46fc55d
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98684021"
---
# <a name="prepare-data-with-data-wrangling"></a>Vorbereiten von Daten mit Datenwrangling

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Mittels Datenwrangling in Data Factory können Sie interaktive Power Query-Mash-ups nativ in ADF erstellen und dann im großen Stil in einer ADF-Pipeline ausführen.

> [!NOTE]
> Power Query-Aktivität in ADF ist zurzeit in der öffentlichen Vorschau verfügbar.

## <a name="create-a-power-query-activity"></a>Erstellen einer Power Query-Aktivität

Eine Power Query kann in Azure Data Factory auf zwei Arten erstellt werden. Bei der einen Methode klicken Sie auf das Plussymbol und wählen im Bereich mit den Factory-Ressourcen die Option **Datenfluss** aus.

> [!NOTE]
> Zuvor befand sich das Datenwranglingfeature im Datenflussworkflow. Nun erstellen Sie Ihr Datenwrangling-Mash-Up aus ```New > Power query```.

![Screenshot: Power Query im Bereich „Factoryressourcen“.](media/data-flow/power-query-wrangling.png)

Die andere Möglichkeit befindet sich im Bereich „Aktivitäten“ der Pipelinecanvas. Öffnen Sie das Accordion-Element **Power Query**, und ziehen Sie die **Power Query**-Aktivität auf die Canvas.

![Screenshot: Hervorhebung der Datenwranglingoption.](media/data-flow/power-query-activity.png)

## <a name="author-a-power-query-data-wrangling-activity"></a>Erstellen einer Power Query-Datenwranglingaktivität

Fügen Sie ein **Quelldataset** für Ihr Power Query-Mash-Up hinzu. Wählen Sie entweder ein vorhandenes Dataset aus, oder erstellen Sie ein neues. Sie können auch ein Senkendataset auswählen. Sie können ein oder mehrere Quelldatasets auswählen, doch ist derzeit nur eine Senke zulässig. Die Auswahl eines Senkendatasets ist optional, aber es ist mindestens ein Quelldataset erforderlich.

![Wrangling](media/wrangling-data-flow/tutorial4.png)

Klicken Sie auf **Erstellen**, um den Power Query Online-Mashup-Editor zu öffnen.

![Screenshot: Schaltfläche „Erstellen“ zum Öffnen des Power Query Online-Mashup-Editors](media/wrangling-data-flow/tutorial5.png)

Erstellen Sie Ihre Wrangling-Power Query mithilfe der codefreien Datenvorbereitung. Die Liste der verfügbaren Funktionen finden Sie unter [Transformationsfunktionen](wrangling-functions.md). ADF übersetzt das M-Skript in ein Datenflussskript, sodass Sie Ihre Power Query im großen Stil mithilfe der Azure Data Factory-Datenfluss-Spark-Umgebung ausführen können.

![Screenshot: Prozess zum Erstellen Ihrer Datenwrangling-Power Query.](media/wrangling-data-flow/tutorial6.png)

## <a name="running-and-monitoring-a-power-query-data-wrangling-activity"></a>Ausführen und Überwachen einer Power Query-Datenwranglingaktivität

Um eine Ausführung einer Power Query-Aktivität zum Debuggen einer Pipeline zu starten, klicken Sie in der Pipelinecanvas auf **Debuggen**. Nachdem Sie Ihre Pipeline veröffentlicht haben, können Sie mit **Jetzt auslösen** eine bedarfsgesteuerte Ausführung der letzten veröffentlichten Pipeline durchführen. Power Query-Pipelines können mit allen vorhandenen Azure Data Factory-Triggern geplant werden.

![Screenshot: Hinzufügen einer Power Query-Datenwranglingaktivität.](media/wrangling-data-flow/tutorial3.png)

Wechseln Sie zur Registerkarte **Überwachen**, um die Ausgabe einer ausgelösten Power Query-Aktivität zu visualisieren.

![Screenshot: Ausgabe einer ausgelösten Wrangling-Power Query-Aktivitätsausführung.](media/wrangling-data-flow/tutorial2.png)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [einen Zuordnungsdatenfluss erstellen](tutorial-data-flow.md).
