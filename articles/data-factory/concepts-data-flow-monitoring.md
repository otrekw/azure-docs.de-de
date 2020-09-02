---
title: Überwachen von Zuordnungsdatenflüssen
description: Hier erfahren Sie, wie Sie Azure Data Factory-Zuordnungsdatenflüsse visuell überwachen.
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 08/19/2020
ms.openlocfilehash: 77dda42b27aa6f5fb505fe65667876523cb3f5d2
ms.sourcegitcommit: 271601d3eeeb9422e36353d32d57bd6e331f4d7b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88650892"
---
# <a name="monitor-data-flows"></a>Überwachen von Datenflüssen

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Nachdem Sie den Aufbau und das Debugging Ihres Datenflusses abgeschlossen haben, planen Sie Ihren Datenfluss am besten so, dass er nach einem Zeitplan im Kontext einer Pipeline ausgeführt wird. Sie können die Pipeline von Azure Data Factory mit Hilfe von Auslösern planen. Oder Sie können die Option „Jetzt auslösen“ aus dem Azure Data Factory Pipeline Builder verwenden, um eine einmalige Ausführung auszuführen, um Ihren Datenfluss im Kontext der Pipeline zu testen.

Bei der Ausführung können Sie die Pipeline und alle darin enthaltenen Aktivitäten, einschließlich der Datenflussaktivität, überwachen. Klicken Sie auf das Symbol „Überwachen“ im linken Bereich der Azure Data Factory-Benutzeroberfläche. Daraufhin wird ein Bildschirm angezeigt, der dem folgenden ähnelt. Über die hervorgehobenen Symbole können Sie einen Drilldown in die Aktivitäten der Pipeline ausführen, einschließlich der Datenflussaktivität.

![Datenflussüberwachung](media/data-flow/mon001.png "Datenflussüberwachung")

Auf dieser Ebene werden sowohl Statistikdaten als auch die Laufzeiten und der Status angezeigt. Die Ausführungs-ID auf der Aktivitätsebene unterscheidet sich von der Ausführungs-ID auf der Pipelineebene. Die Ausführungs-ID der vorherigen Ebene ist für die Pipeline. Durch Auswählen der Brille erhalten Sie detaillierte Informationen zu Ihrer Datenflussausführung.

![Datenflussüberwachung](media/data-flow/monitoring-details.png "Datenflussüberwachung")

Wenn Sie sich in der grafischen Knotenüberwachungsansicht befinden, wird eine vereinfachte reine Ansichtsversion Ihres Datenflussdiagramms angezeigt.

![Datenflussüberwachung](media/data-flow/mon003.png "Datenflussüberwachung")

Hier sehen Sie ein Video mit einer Übersicht über die Überwachungsleistung Ihre Datenflüsse auf dem ADF-Überwachungsbildschirm:

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4u4mH]

## <a name="view-data-flow-execution-plans"></a>Anzeigen der Ausführungspläne für den Datenfluss

Wenn Ihr Datenfluss in Spark ausgeführt wird, bestimmt Azure Data Factory basierend auf Ihrem gesamten Datenfluss die optimalen Codepfade. Darüber hinaus können die Ausführungspfade auf verschiedenen horizontalen Skalierungsknoten und Datenpartitionen auftreten. Daher stellt das Überwachungsdiagramm den Entwurf Ihres Flusses dar, wobei der Ausführungspfad Ihrer Transformationen berücksichtigt wird. Wenn Sie einzelne Knoten auswählen, werden „Gruppierungen“ angezeigt, die den auf dem Cluster gemeinsam ausgeführten Code darstellen. Die angezeigten Zeitangaben und Zählungen stellen diese Gruppen im Gegensatz zu den einzelnen Schritten in Ihrem Entwurf dar.

![Datenflussüberwachung](media/data-flow/mon004.png "Datenflussüberwachung")

* Wenn Sie das freie Feld im Überwachungsfenster auswählen, zeigen die Statistiken im unteren Bereich die Zeitangabe und die Anzahl der Zeilen für jede Senke und die Transformationen an, die zu den Senkendaten für die Transformationsherkunft geführt haben.

* Wenn Sie einzelne Transformationen auswählen, wird auf der rechten Seite zusätzliches Feedback angezeigt, das Partitionsstatistiken, Spaltenanzahl, Schiefe (wie gleichmäßig die Daten auf Partitionen verteilt sind) und Kurtosis (wie viele Spitzen die Daten haben) enthält.

* Wenn Sie die Senke in der Knotenansicht auswählen, wird die Spaltenherkunft angezeigt. Es gibt drei verschiedene Methoden, mit denen Spalten während des gesamten Datenflusses akkumuliert werden, um in die Senke weitergeleitet zu werden. Sie lauten wie folgt:

  * Berechnet: Sie verwenden die Spalte für die bedingte Verarbeitung oder innerhalb eines Ausdrucks in Ihrem Datenfluss; sie wird aber nicht in die Senke weitergeleitet.
  * Abgeleitet: Die Spalte ist eine neue Spalte, die Sie in Ihrem Datenfluss generiert haben, d. h., sie war in der Quelle nicht vorhanden.
  * Zugeordnet: Die Spalte stammt aus der Quelle und Sie ordnen sie einem Senkenfeld zu.
  * „Data flow status“ (Datenflussstatus): Der aktuelle Status Ihrer Ausführung.
  * „Cluster startup time“ (Startzeit des Clusters): Zeitraum zum Abrufen der JIT-Spark-Computeumgebung für Ihre Datenflussausführung.
  * „Number of transforms“ (Anzahl von Transformationen): Die Anzahl von Transformationsschritten, die in Ihrem Fluss ausgeführt werden.
  
![Datenflussüberwachung](media/data-flow/monitornew.png "Datenflussüberwachung (neu)")

## <a name="total-sink-processing-time-vs-transformation-processing-time"></a>Gegenüberstellung der Gesamtverarbeitungszeit für die Senke und der Verarbeitungszeit für die Transformation

Jede Transformationsphase enthält eine Gesamtzeit bis zum Abschluss der Phase. Hierbei handelt es sich um die Summe der einzelnen Partitionsausführungszeiten. Wenn Sie auf die Senke klicken, wird die Verarbeitungszeit für die Senke angezeigt. Diese Zeit enthält die gesamte Transformationszeit *sowie* die E/A-Zeit, die zum Schreiben Ihrer Daten in den Zielspeicher benötigt wurde. Der Unterschied zwischen der Verarbeitungszeit für die Senke und der Gesamtzeit für die Transformation ist die E/A-Zeit zum Schreiben der Daten.

Wenn Sie die JSON-Ausgabe Ihrer Datenflussaktivität in der ADF-Pipelineüberwachungsansicht öffnen, sehen Sie ausführliche Zeitangaben für die einzelnen Partitionstransformationsschritte. Die JSON-Ausgabe enthält millisekundengenaue Zeitangaben für jede Partition. Die Überwachungsansicht auf der Benutzeroberfläche zeigt dagegen aggregierte Zeitangaben von Partitionen, die miteinander addiert wurden:

```
 {
     "stage": 4,
     "partitionTimes": [
          14353,
          14914,
          14246,
          14912,
          ...
         ]
}
```

### <a name="post-processing-time"></a>Nachbearbeitungszeit

Wenn Sie in der Zuordnung ein Symbol für eine Senkentransformation auswählen, zeigt das Flyout-Panel auf der rechten Seite unten einen zusätzlichen Datenpunkt namens „Nachbearbeitungszeit“. Dies ist die Zeitspanne, die für die Ausführung des Auftrags im Spark-Cluster benötigt wurde, *nachdem* die Daten geladen, transformiert und geschrieben wurden. Dieser Zeitraum kann das Schließen von Verbindungspools, das Herunterfahren von Treibern, das Löschen von Dateien, dass Zusammenfügen von Dateien und weitere Aufgaben umfassen. Wenn Sie in Ihrem Flow Aktionen wie „Dateien verschieben“ und „Ausgabe in einer einzelnen Datei“ ausführen, werden Sie vermutlich einen Anstieg des Werts für die Nachbearbeitungszeit bemerken.
  
## <a name="monitor-icons"></a>Symbole für die Überwachung

Dieses Symbol bedeutet, dass die Transformationsdaten bereits auf dem Cluster zwischengespeichert wurden, sodass die Zeitangaben und der Ausführungspfad dies berücksichtigt haben:

![Datenflussüberwachung](media/data-flow/mon005.png "Datenflussüberwachung")

In der Transformation werden auch grüne Kreissymbole angezeigt. Sie stellen die Anzahl von Senken dar, in die der Datenfluss geleitet wird.
