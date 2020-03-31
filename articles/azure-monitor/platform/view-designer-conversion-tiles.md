---
title: Umwandlung von Kacheln von Azure Monitor-Ansichts-Designer in Arbeitsmappen
description: ''
author: austonli
ms.author: aul
ms.subservice: ''
ms.topic: conceptual
ms.date: 02/07/2020
ms.openlocfilehash: f07d15521c787dfd588c285bff57616059caa2f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658625"
---
# <a name="azure-monitor-view-designer-tile-conversions"></a>Umwandlung von Azure Monitor-Ansichts-Designer-Kacheln
Der [Ansicht-Designer](view-designer.md) ist eine Funktion in Azure Monitor, mit der Sie verschiedene benutzerdefinierten Ansichten erstellen können, die Ihnen bei der Visualisierung von Daten in Ihrem Log Analytics-Arbeitsbereich mittels Diagrammen, Listen und Zeitachsen helfen. Sie laufen aus und werden durch Arbeitsmappen ersetzt, die zusätzliche Funktionalität bereitstellen. Dieser Artikel enthält Details zum Umwandeln verschiedener Kacheln in Arbeitsmappen.

## <a name="donut--list-tile"></a>Ring- und Listenkachel

![Ring Liste](media/view-designer-conversion-tiles/donut-list.png)

Das Neuerstellen der Ring- und Listenkachel in Arbeitsmappen umfasst zwei separate Visualisierungen. Für den Ringteil gibt es zwei Optionen.
Beginnen Sie für beide zunächst damit, dass Sie **Abfrage hinzufügen** auswählen und die ursprüngliche Abfrage aus dem Ansicht-Designer in die Zelle einfügen.

**Option 1:** Wählen Sie in der Dropdownliste **Visualisierung** den Eintrag **Kreisdiagramm** aus: ![Visualisierungsmenü für Kreisdiagramm](media/view-designer-conversion-tiles/pie-chart.png)

**Option 2:** Wählen Sie in der Dropdownliste **Visualisierung** die Option **Festgelegt nach Abfrage** aus, und fügen Sie der Abfrage `| render piechart` hinzu:

 ![Visualisierungsmenü](media/view-designer-conversion-tiles/set-by-query.png)

**Beispiel**

Ursprüngliche Abfrage
```KQL
search * 
| summarize AggregatedValue = count() by Type 
| order by AggregatedValue desc
```

Aktualisierte Abfrage
```KQL
search * 
| summarize AggregatedValue = count() by Type 
| order by AggregatedValue desc 
| render piechart
```

Informationen zum Erstellen einer Liste und zum Aktivieren von Sparklines finden Sie im Artikel zu [allgemeinen Aufgaben](view-designer-conversion-tasks.md).

Im Folgenden finden Sie ein Beispiel dafür, wie die Ring- und Listenkachel in Arbeitsmappen neu interpretiert werden kann:

![Ring Liste Arbeitsmappen](media/view-designer-conversion-tiles/donut-workbooks.png)

## <a name="line-chart--list-tile"></a>Liniendiagramm und Listenkachel
![Liniendiagramm Liste](media/view-designer-conversion-tiles/line-list.png) 

Zum Neuerstellen des Liniendiagrammteils aktualisieren Sie die Abfrage wie folgt:

Ursprüngliche Abfrage
```KQL
search * 
| summarize AggregatedValue = count() by Type
```

Aktualisierte Abfrage
```KQL
search * 
| make-series Count = count() default=0 on TimeGenerated from {TimeRange:start} to {TimeRange:end} step {TimeRange:grain} by Type
```

Es gibt zwei Möglichkeiten zum Visualisieren des Liniendiagramms

**Option 1:** Wählen Sie in der Dropdownliste **Visualisierung** den Eintrag **Liniendiagramm** aus:
 
 ![Liniendiagrammmenü](media/view-designer-conversion-tiles/line-visualization.png)

**Option 2:** Wählen Sie in der Dropdownliste **Visualisierung** die Option **Festgelegt nach Abfrage** aus, und fügen Sie der Abfrage `| render linechart` hinzu:

 ![Visualisierungsmenü](media/view-designer-conversion-tiles/set-by-query.png)

**Beispiel**

```KQL
search * 
| make-series Count = count() default=0 on TimeGenerated from {TimeRange:start} to {TimeRange:end} step {TimeRange:grain} by Type 
| render linechart_
```

Informationen zum Erstellen einer Liste und zum Aktivieren von Sparklines finden Sie im Artikel zu [allgemeinen Aufgaben](view-designer-conversion-tasks.md).

Im Folgenden finden Sie ein Beispiel dafür, wie die Liniendiagramm- und Listenkachel in Arbeitsmappen neu interpretiert werden kann:

![Liniendiagramm Liste Arbeitsmappen](media/view-designer-conversion-tiles/line-workbooks.png)

## <a name="number--list-tile"></a>Zahl- und Listenkachel

 ![Kachel Liste](media/view-designer-conversion-tiles/tile-list-example.png)

Aktualisieren Sie für die Zahlkachel die Abfrage wie folgt:

Ursprüngliche Abfrage
```KQL
search * 
| summarize AggregatedValue = count() by Computer | count
```

Aktualisierte Abfrage
```KQL
search *
| summarize AggregatedValue = count() by Computer 
| summarize Count = count()
```

Ändern Sie die Dropdownliste „Visualisierung“ in **Kacheln**, und wählen Sie dann **Kacheleinstellungen** aus.
 ![Kachelvisualisierung](media/view-designer-conversion-tiles/tile-visualization.png)

Lassen Sie den Abschnitt **Titel** unausgefüllt, und wählen Sie **Links**aus. Ändern Sie den Wert für **Use column:** in **Count** und für **Column Renderer** in **Big Number**:

![Kacheleinstellungen](media/view-designer-conversion-tiles/tile-settings.png)

 
Informationen zum Erstellen einer Liste und zum Aktivieren von Sparklines finden Sie im Artikel zu [allgemeinen Aufgaben](view-designer-conversion-tasks.md).

Im Folgenden finden Sie ein Beispiel dafür, wie die Zahl- und Listenkachel in Arbeitsmappen neu interpretiert werden kann:

![Zahl Liste Arbeitsmappen](media/view-designer-conversion-tiles/number-workbooks.png)

## <a name="timeline--list"></a>Zeitachse und Liste

 ![Zeitachse Liste](media/view-designer-conversion-tiles/time-list.png)

Aktualisieren Sie die Abfrage für die Zeitachse wie folgt:

Ursprüngliche Abfrage
```KQL
search * 
| sort by TimeGenerated desc
```

Aktualisierte Abfrage
```KQL
search * 
| summarize Count = count() by Computer, bin(TimeGenerated,{TimeRange:grain})
```

Es gibt zwei Möglichkeiten zum Visualisieren des Balkendiagramms:

**Option 1:** Wählen Sie in der Dropdownliste **Visualisierung** den Eintrag **Balkendiagramm** aus: ![Balkendiagrammvisualisierung](media/view-designer-conversion-tiles/bar-visualization.png)
 
**Option 2:** Wählen Sie in der Dropdownliste **Visualisierung** die Option **Festgelegt nach Abfrage** aus, und fügen Sie der Abfrage `| render barchart` hinzu:

 ![Visualisierungsmenü](media/view-designer-conversion-tiles/set-by-query.png)

 
Informationen zum Erstellen einer Liste und zum Aktivieren von Sparklines finden Sie im Artikel zu [allgemeinen Aufgaben](view-designer-conversion-tasks.md).

Im Folgenden finden Sie ein Beispiel dafür, wie die Zeitachsen- und Listenkachel in Arbeitsmappen neu interpretiert werden kann:

![Zeitachse Liste Arbeitsmappen](media/view-designer-conversion-tiles/time-workbooks.png)

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht der Übergänge vom Ansicht-Designer zu Arbeitsmappen](view-designer-conversion-overview.md)
