---
title: Umwandlung von Azure Monitor-Ansichts-Designer in Arbeitsmappen – häufige Aufgaben
description: ''
author: austonli
ms.author: aul
ms.subservice: ''
ms.topic: conceptual
ms.date: 02/07/2020
ms.openlocfilehash: 5559dac916262998d621b40757398088ec613609
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77658743"
---
# <a name="view-designer-to-workbooks-conversion-common-tasks"></a>Umwandlung von Ansichts-Designer in Arbeitsmappen – häufige Aufgaben
Der [Ansicht-Designer](view-designer.md) ist eine Funktion in Azure Monitor, mit der Sie verschiedene benutzerdefinierten Ansichten erstellen können, die Ihnen bei der Visualisierung von Daten in Ihrem Log Analytics-Arbeitsbereich mittels Diagrammen, Listen und Zeitachsen helfen. Sie laufen aus und werden durch Arbeitsmappen ersetzt, die zusätzliche Funktionalität bereitstellen. In diesem Artikel werden die Aufgaben erläutert, die beim Umwandeln von Ansichten in Arbeitsmappen häufig vorkommen.


## <a name="quickstart-with-preset-view-designer-templates"></a>Schnellstart mit vordefinierten Ansicht-Designer-Vorlagen

Arbeitsmappen in Log Analytics-Arbeitsbereichen enthalten bereits Vorlagen, die darauf ausgelegt sind, einigen der Ansichten im Ansicht-Designer zu entsprechen. Wählen Sie unter der Kategorie **Ansicht-Designer-Handbücher** das **Handbuch für den Übergang vom Ansicht-Designer** aus, um Informationen zu Ihren Optionen zu erhalten oder eine der vordefinierten Vorlagen auszuwählen.

![Beispielvorlagen](media/view-designer-conversion-tasks/templates.png)

## <a name="enabling-time-range-filter"></a>Aktivieren von Zeitbereichsfiltern
Der Ansicht-Designer verfügt über einen integrierten Standardzeitbereichsfilter. In Arbeitsmappen ist diese Einstellung jedoch nicht standardmäßig aktiviert. Arbeitsmappen gestatten es Benutzern, eigene Zeitbereichsfilter zu erstellen, die für ihre Datenprotokolle geeigneter sind. Die Schritte zum Generieren des Filters finden Sie im Folgenden:

Wählen Sie die Option **Parameter hinzufügen** aus. Der Standardwert von **Stil** ist auf *Ovale Steuerelemente* festgelegt.

![Parameter hinzufügen](media/view-designer-conversion-tasks/add-param.png)

 Wählen Sie die Schaltfläche **Parameter hinzufügen** aus.

![Parameter hinzufügen](media/view-designer-conversion-tasks/add-parameter.png)

Geben Sie im Menü der Randleiste im Textfeld **Parametername** den Wert *TimeRange* ein. Legen Sie den **Parametertyp** auf *Zeitbereichsauswahl* fest. Aktivieren Sie das Kontrollkästchen **Erforderlich?** .

![Parametermenü](media/view-designer-conversion-tasks/parameter-menu.png)

Speichern Sie den Parameter oben links im Randleistenmenü. Sie können die Dropdownliste standardmäßig *nicht festgelegt* lassen oder einen Standardwert für **TimeRange** auswählen wie *24 Stunden*. Wählen Sie **Bearbeitung abgeschlossen** aus.

Parameter können in Abfragen verwendet werden, indem Sie den Parameternamen in geschweifte Klammern ({}) einschließen. Weitere Informationen zu Parametern finden Sie in der [Arbeitsmappendokumentation zu Parametern](https://github.com/microsoft/Application-Insights-Workbooks/blob/master/Documentation/Parameters/Parameters.md).

## <a name="updating-queries-with-the-timerange-parameter"></a>Aktualisieren von Abfragen mit dem TimeRange-Parameter

### <a name="option-1-select-timerange-from-the-time-range-dropdown"></a>Option 1: Auswählen von „TimeRange“ im TimeRange-Dropdown

![Zeitparameter](media/view-designer-conversion-tasks/time-parameter.png)

### <a name="option-2-update-your-log-queries"></a>Option 2: Aktualisieren Ihrer Protokollabfragen

Fügen Sie in Ihrer Abfrage die Zeile: `| where TimeGenerated {TimeRange}` hinzu, wie im folgenden Beispiel gezeigt:

Ursprüngliche Abfrage
```KQL
search * 
| summarize count() by Type
```

Aktualisierte Abfrage
```KQL
search * 
| where TimeGenerated {TimeRange} 
| summarize count() by Type
```

## <a name="including-a-list"></a>Einschließen einer Liste
Die meisten Ansichten des Ansicht-Designers enthalten eine Liste, und Sie können diese Standardliste in einer Arbeitsmappe reproduzieren.

![Kachel Liste](media/view-designer-conversion-tasks/tile-list.png)

Fügen Sie eine Visualisierung hinzu, indem Sie in den Zellenoptionen auf **Abfrage hinzufügen** klicken.

![Parameter hinzufügen](media/view-designer-conversion-tasks/add-param.png)

Der Ansicht-Designer setzt eine Standardabfrage ein, die mit der Syntax aus dem ursprünglichen Beispiel übereinstimmt. Diese kann aktualisiert werden, indem Sie die Abfrage in das aktualisierte Formular ändern, wie im folgenden Beispiel gezeigt:

Ursprüngliche Abfrage
```KQL
search * 
| summarize AggregatedValue = count() by Type
```

Aktualisierte Abfrage
```KQL
search * 
| summarize Count = count() by Type
```

Hierdurch wird unter eine Liste generiert, die ähnlich wie folgt aussieht:

![Listenbeispiel](media/view-designer-conversion-tasks/list-example.png)

## <a name="enabling-sparklines"></a>Sparklines aktivieren
Eine häufige Funktion für Raster besteht im Hinzufügen von Sparklines zum Zusammenfassen verschiedener Datenmuster im Zeitverlauf. Der Ansicht-Designer bietet die Funktion **Sparklines aktivieren** für alle Listen, ebenso wie Arbeitsmappen. Um Sparklines in Ihre Daten einzuschließen, die dem Ansicht-Designer entsprechen, verknüpfen Sie die Daten mit Ihrer ursprünglichen Abfrage wie im folgenden Beispiel:

Ursprüngliche Abfrage
```KQL
search *
| summarize AggregatedValue = count() by Type) on Type
```

Aktualisierte Abfrage
```KQL
search * 
| summarize AggregatedValue = count() by Type
| join kind = inner (search * 
    | make-series Trend = count() default=0 on TimeGenerated from {TimeRange:start} to {TimeRange:end} step {TimeRange:grain} by Type) on Type
| project Type, AggregatedValue, Trend
```

Wählen Sie **Spalteneinstellungen** aus.
![Spalteneinstellungen](media/view-designer-conversion-tasks/column-settings.png)

Aktualisieren Sie das Dropdown **Säulenrenderer** auf *Spark-Bereich*.
![Sparklines](media/view-designer-conversion-tasks/sparkline.png)

Speichern Sie die Einstellungen, und führen Sie die Abfrage erneut aus, um die Tabelle für die Aufnahme von Sparklines zu aktualisieren.

Das resultierende Raster sollte in etwa wie folgt aussehen: ![Sparklinebeispiel](media/view-designer-conversion-tasks/sparkline-example.png)

## <a name="advanced-cell-settings"></a>Erweiterte Zelleneinstellungen
Zum Imitieren des Ansicht-Designers können Sie Aufgaben wie das Ändern der Größe von Arbeitsmappenzellen oder das Hinzufügen von Stecknadelsymbolen und externen Links zu Protokollen ausführen.

Um auf **Erweiterte Einstellungen** zuzugreifen, wählen Sie das Zahnradsymbol unten in jeder Zelle aus.

![Erweiterte Einstellungen](media/view-designer-conversion-tasks/advanced-settings.png)

Dadurch wird ein Menü mit verschiedenen Optionen angezeigt:

![Einstellungen unter „Erweiterte Einstellungen“](media/view-designer-conversion-tasks/advanced-settings-settings.png)

Aktivieren Sie die entsprechenden Kontrollkästchen, um einer externen Abfrage ein Stecknadelsymbol und einen Link hinzuzufügen. Um Ihrer Zelle einen Titel hinzuzufügen, geben Sie den gewünschten Titel in den Abschnitt **Diagrammtitel** ein.

Standardmäßig ist jede Arbeitsmappenzelle so festgelegt, dass sie die gesamte Seitenbreite ausfüllt. Sie können dies jedoch anpassen, indem Sie die Zelle im Menü **Erweiterte Einstellungen** auf der Registerkarte **Stil** herunterskalieren.

![„Stil“unter „Erweiterte Einstellungen“](media/view-designer-conversion-tasks/advanced-settings-style.png)

 
## <a name="additional-parameters"></a>Zusätzliche Parameter
Wählen Sie **Parameter** hinzufügen aus, um einen neuen Parameter in Ihrer Arbeitsmappe zu erstellen. 

Wenn Sie ein Abonnement auswählen möchten, geben Sie *Abonnement* in das Feld **Parametername** im Seitenmenü ein, und wählen Sie *Abonnementauswahl* im Dropdown **Parametertyp** aus.

![Abonnementmenü](media/view-designer-conversion-tasks/subscription-filter.png)

Wenn Sie eine Ressource auswählen möchten, geben Sie *Ressource* in das Feld **Parametername** im Seitenmenü ein, und wählen Sie *Ressourcenauswahl* im Dropdown **Parametertyp** aus.

![Ressourcenmenü](media/view-designer-conversion-tasks/resource-filter.png)

Hierdurch werden Dropdownlisten eingefügt, über die Sie auf Ihre verschiedenen Abonnements und Ressourcen zugreifen können.

![Abonnement-/Ressourcendropdown](media/view-designer-conversion-tasks/subscription-resource.png)


## <a name="next-steps"></a>Nächste Schritte
- [Kachelumwandlungen](view-designer-conversion-tiles.md)
