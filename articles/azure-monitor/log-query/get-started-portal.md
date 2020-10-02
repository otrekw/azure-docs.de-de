---
title: 'Tutorial: Erste Schritte mit Log Analytics-Abfragen'
description: In diesem Tutorial erfahren Sie, wie Sie mit Log Analytics Azure Monitor-Protokollabfragen im Azure-Portal schreiben und verwalten.
ms.subservice: logs
ms.topic: tutorial
author: bwren
ms.author: bwren
ms.date: 03/17/2020
ms.openlocfilehash: b337065f125d12e309dd1f7fcc56c2af72b1c28c
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2020
ms.locfileid: "90088356"
---
# <a name="tutorial-get-started-with-log-analytics-queries"></a>Tutorial: Erste Schritte mit Log Analytics-Abfragen

In diesem Tutorial erfahren Sie, wie Sie mit Log Analytics Azure Monitor-Protokollabfragen im Azure-Portal schreiben, ausführen und verwalten. Mithilfe von Log Analytics-Abfragen können Sie nach Begriffen suchen, Trends identifizieren, Muster analysieren und viele andere Erkenntnisse aus Ihren Daten gewinnen. 

In diesem Tutorial erfahren Sie, wie Sie mit Log Analytics:

> [!div class="checklist"]
> * Das Schema von Protokolldaten nachvollziehen
> * Einfache Abfragen schreiben und ausführen sowie den Zeitbereich für Abfragen ändern
> * Abfrageergebnisse filtern, sortieren und gruppieren
> * Visuelle Elemente der Abfrageergebnisse anzeigen, ändern und teilen
> * Abfragen und Ergebnisse speichern, laden, exportieren und kopieren

Weitere Informationen zu Protokollabfragen finden Sie unter [Übersicht über Protokollabfragen in Azure Monitor](log-query-overview.md).<br/>
Ein ausführliches Tutorial zum Schreiben von Protokollabfragen finden Sie unter [Erste Schritte mit Protokollabfragen in Azure Monitor](get-started-queries.md).

## <a name="open-log-analytics"></a>Öffnen von Log Analytics
Um Log Analytics verwenden zu können, müssen Sie bei einem Azure-Konto angemeldet sein. Falls Sie kein Azure-Konto haben, können Sie [kostenlos eines erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

Die meisten Schritte in diesem Tutorial können Sie [in dieser Demoumgebung](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring_Logs/DemoLogsBlade) ausführen, die zahlreiche Beispieldaten enthält. In der Demoumgebung können Sie weder Abfragen speichern noch Ergebnisse an ein Dashboard anheften.

Sie können auch Ihre eigene Umgebung verwenden, wenn Sie mit Azure Monitor Protokolldaten für mindestens eine Azure-Ressource sammeln. Wählen Sie im linken Navigationsbereich von Azure Monitor **Protokolle** aus, um einen Log Analytics-Arbeitsbereich zu öffnen. 

## <a name="understand-the-schema"></a>Grundlegendes zum Schema
 
Das *Schema* ist eine Sammlung von Tabellen, die unter einer logischen Kategorie gruppiert sind. Das Demoschema umfasst mehrere Kategorien, die aus Überwachungslösungen stammen. Die Kategorie **LogManagement** enthält beispielsweise Windows- und Syslog-Ereignisse, Leistungsdaten und Agent-Takte.

Die Schematabellen werden auf der Registerkarte **Tabellen** des Log Analytics-Arbeitsbereichs angezeigt. Die Tabellen enthalten Spalten, deren Datentyp durch das Symbol neben dem Spaltennamen angezeigt wird. Beispielsweise enthält die Tabelle **Event** Textspalten wie **Computer** und numerische Spalten wie **EventCategory**.

![Screenshot: Seite „Protokolle“ im Azure-Portal mit einer neuen Abfrage. Der Bereich „Tabellen“ sowie „Computer“ und „EventCategory“ sind hervorgehoben.](media/get-started-portal/schema.png)

## <a name="write-and-run-basic-queries"></a>Schreiben und Ausführen einfacher Abfragen

Log Analytics wird mit einer neuen leeren Abfrage im **Abfrage-Editor** geöffnet.

![Log Analytics](media/get-started-portal/homepage.png)

### <a name="write-a-query"></a>Schreiben Sie eine Abfrage.

Für Azure Monitor-Protokollabfragen wird eine Version der Kusto-Abfragesprache verwendet. Abfragen können mit einem Tabellennamen oder einem [search](/azure/kusto/query/searchoperator)-Befehl beginnen. 

Die folgende Abfrage ruft alle Datensätze aus der Tabelle **Event** ab:

```Kusto
Event
```

Der senkrechte Strich (|) trennt Befehle, d. h. die Ausgabe des ersten Befehls ist die Eingabe für den nächsten Befehl. Sie können eine beliebige Anzahl von Befehlen zu einer einzelnen Abfrage hinzufügen. Die folgende Abfrage ruft die Datensätze aus der Tabelle **Event** ab und sucht dann nach Datensätzen, die den Begriff **error** in einer Eigenschaft enthalten:

```Kusto
Event 
| search "error"
```

Mit einem einzelnen Zeilenumbruch kann die Lesbarkeit von Abfragen verbessert werden. Durch mehrere Zeilenumbrüche wird die Abfrage in separate Abfragen aufgeteilt.

Die gleiche Abfrage kann auch wie folgt geschrieben werden:

```Kusto
search in (Event) "error"
```

Im zweiten Beispiel sucht der **search**-Befehl nur nach Datensätzen in der Tabelle **Events**, die den Begriff **error** enthalten.

Standardmäßig beschränkt Log Analytics Abfragen auf die letzten 24 Stunden. Wenn Sie einen anderen Zeitbereich festlegen möchten, können Sie der Abfrage einen expliziten **TimeGenerated**-Filter hinzufügen oder die **Zeitbereichssteuerung** verwenden.

### <a name="use-the-time-range-control"></a>Verwenden der Zeitbereichssteuerung
Um die **Zeitbereichssteuerung** zu verwenden, wählen Sie sie zunächst in der oberen Leiste aus. Wählen Sie dann in der Dropdownliste einen Wert aus, oder klicken Sie auf **Benutzerdefiniert**, um einen benutzerdefinierten Zeitbereich zu erstellen.

![Zeitauswahl](media/get-started-portal/time-picker.png)

- Zeitbereichswerte werden in der koordinierten Weltzeit (UTC) angezeigt, die möglicherweise von Ihrer lokalen Zeitzone abweicht.
- Wenn die Abfrage einen expliziten **TimeGenerated**-Filter enthält, wird in der Zeitbereichssteuerung **In Abfrage festlegen** angezeigt. Zudem ist sie deaktiviert, um einen Konflikt zu verhindern.

### <a name="run-a-query"></a>Ausführen einer Abfrage
Platzieren Sie den Cursor zum Ausführen einer Abfrage an einer beliebigen Stelle in der Abfrage, und wählen Sie in der oberen Leiste **Ausführen** aus, oder drücken Sie **UMSCHALT**+**EINGABETASTE**. Die Abfrage wird ausgeführt, bis eine leere Zeile gefunden wird.

## <a name="filter-results"></a>Ergebnisse filtern
Log Analytics beschränkt Ergebnisse auf maximal 10.000 Datensätze. Eine allgemeine Abfrage wie `Event` gibt zu viele Ergebnisse zurück. Sie können Abfrageergebnisse filtern, indem Sie die Tabellenelemente in der Abfrage einschränken oder den Ergebnissen explizit einen Filter hinzufügen. Beim Filtern mithilfe der Tabellenelemente wird ein neues Resultset zurückgegeben, während ein expliziter Filter auf das vorhandene Resultset angewendet wird.

### <a name="filter-by-restricting-table-elements"></a>Filtern durch Einschränken der Tabellenelemente
So filtern Sie die Ergebnisse der `Event`-Abfrage durch Einschränken der Tabellenelemente nach Ereignissen vom Typ **Error**:

1. Klicken Sie in den Abfrageergebnissen auf den Dropdownpfeil neben einem Datensatz mit dem Wert **Error** in der Spalte **EventLevelName**. 
   
1. Zeigen Sie in den erweiterten Details neben **EventLevelName** auf **...** , klicken Sie darauf, und wählen Sie dann **„Error“ einschließen** aus. 
   
   ![Hinzufügen eines Filters zur Abfrage](media/get-started-portal/add-filter.png)
   
1. Beachten Sie, dass die Abfrage im **Abfrage-Editor** jetzt wie folgt lautet:
   
   ```Kusto
   Event
   | where EventLevelName == "Error"
   ```
   
1. Wählen Sie **Ausführen** aus, um die neue Abfrage auszuführen.

### <a name="filter-by-explicitly-filtering-results"></a>Explizites Filtern der Ergebnisse
So filtern Sie die Ergebnisse der `Event`-Abfrage durch explizites Filtern nach Ereignissen vom Typ **Error**:

1. Klicken Sie in den Abfrageergebnissen auf das Symbol **Filter** neben der Spaltenüberschrift **EventLevelName**. 
   
1. Wählen Sie im ersten Feld des Popupfensters **Ist gleich** aus, und geben Sie im nächsten Feld *error* ein. 
   
1. Wählen Sie **Filtern** aus.
   
   ![Screenshot: Tabelle mit Ergebnissen mit einem Kontextmenü zum Filtern der Ergebnisse nach EventLevelName](media/get-started-portal/filter.png)

## <a name="sort-group-and-select-columns"></a>Sortieren, Gruppieren und Auswählen von Spalten
Um Abfrageergebnisse nach einer bestimmten Spalte (z. B. **TimeGenerated [UTC]** ) zu sortieren, klicken Sie auf die Spaltenüberschrift. Klicken Sie erneut auf die Überschrift, um zwischen auf- und absteigender Reihenfolge zu wechseln.

![Sortieren einer Spalte](media/get-started-portal/sort-column.png)

Eine weitere Möglichkeit zum Organisieren von Ergebnissen sind Gruppen. Um Ergebnisse nach einer bestimmten Spalte zu gruppieren, ziehen Sie die Spaltenüberschrift in die Leiste **Ziehen Sie eine Spaltenüberschrift hierher, um die Einträge nach dieser Spalte zu gruppieren** über der Ergebnistabelle. Wenn Sie Untergruppen erstellen möchten, ziehen Sie weitere Spalten in die obere Leiste. Sie können die Hierarchie und Sortierung der Gruppen und Untergruppen in der Leiste ändern.

![Screenshot: Abfrageergebnisse mit Untergruppen für EventLevelName und Computer](media/get-started-portal/groups.png)

Um Spalten in den Ergebnissen aus- oder einzublenden, klicken Sie über der Tabelle auf **Spalten**, und wählen Sie dann in der Dropdownliste die gewünschten Spalten aus, bzw. heben Sie ihre Auswahl auf.

![Spalten auswählen](media/get-started-portal/select-columns.png)

## <a name="view-and-modify-charts"></a>Anzeigen und Bearbeiten von Diagrammen
Sie können Abfrageergebnisse auch in visuellen Formaten anzeigen. Geben Sie als Beispiel die folgende Abfrage ein:

```Kusto
Event 
| where EventLevelName == "Error" 
| where TimeGenerated > ago(1d) 
| summarize count() by Source 
```

Standardmäßig werden Ergebnisse in einer Tabelle angezeigt. Wählen Sie über der Tabelle **Diagramm** aus, um die Ergebnisse in einer grafischen Ansicht anzuzeigen.

![Balkendiagramm](media/get-started-portal/bar-chart.png)

Die Ergebnisse werden in einem gestapelten Balkendiagramm angezeigt. Wählen Sie andere Optionen aus (z. B. **Gestapelte Säule** oder **Kreis**), um die Ergebnisse in weiteren Ansichten anzuzeigen.

![Kreisdiagramm](media/get-started-portal/pie-chart.png)

Sie können die Eigenschaften der Ansicht manuell über die Steuerleiste ändern, z. B. x- und y-Achsen oder Gruppierungs- und Aufteilungseinstellungen.

Mit dem [render](/azure/kusto/query/renderoperator)-Operator können Sie die bevorzugte Ansicht auch in der Abfrage selbst festlegen.

## <a name="pin-results-to-a-dashboard"></a>Anheften von Ergebnissen an ein Dashboard

Um eine Ergebnistabelle oder ein Ergebnisdiagramm aus Log Analytics an ein freigegebenes Azure-Dashboard anzuheften, wählen Sie in der oberen Leiste **An Dashboard anheften** aus. 

![An Dashboard anheften](media/get-started-portal/pin-dashboard.png)

Wählen Sie im Bereich **An anderes Dashboard anheften** ein freigegebenes Dashboard aus, und klicken Sie auf **Übernehmen**. Die Tabelle bzw. das Diagramm wird im ausgewählten Azure-Dashboard angezeigt.

![An Dashboard angeheftetes Diagramm](media/get-started-portal/pin-dashboard2.png)

Tabellen oder Diagramme, die Sie an ein freigegebenes Dashboard anheften, werden wie folgt vereinfacht: 

- Die Daten sind auf die letzten 14 Tage beschränkt.
- In einer Tabelle werden nur bis zu vier Spalten und die ersten sieben Zeilen angezeigt.
- Bei Diagrammen mit vielen separaten Kategorien werden Kategorien mit weniger Daten automatisch in einem Container **Andere** zusammengefasst.

## <a name="save-load-or-export-queries"></a>Speichern, Laden oder Exportieren von Abfragen

Nachdem Sie eine Abfrage erstellt haben, können Sie die Abfrage oder die Ergebnisse speichern und mit anderen teilen. 

### <a name="save-queries"></a>Speichern von Abfragen

So speichern Sie eine Abfrage:

1. Wählen Sie in der oberen Leiste **Speichern** aus.
   
1. Geben Sie im Dialogfeld **Speichern** einen **Namen** für die Abfrage ein. Folgende Zeichen sind zulässig: a–z, A–Z, 0–9, Leerzeichen, Bindestrich, Unterstrich, Punkt, Klammer oder senkrechter Strich. 
   
1. Wählen Sie aus, ob die Abfrage als **Abfrage** oder als **Funktion** gespeichert werden soll. Funktionen sind Abfragen, auf die andere Abfragen verweisen können. 
   
   Um eine Abfrage als Funktion zu speichern, geben Sie einen **Funktionsalias** an. Dies ist ein Kurzname, den andere Abfragen zum Aufrufen der Abfrage verwenden.
   
1. Wenn Sie sich in einem Log Analytics-Arbeitsbereich befinden, geben Sie eine **Kategorie** an, die im **Abfrage-Explorer** für die Abfrage verwendet werden soll. (Kategorien sind für Application Insights-Abfragen nicht verfügbar.)
   
1. Wählen Sie **Speichern** aus.
   
   ![Funktion speichern](media/get-started-portal/save-function.png)

### <a name="load-queries"></a>Laden von Abfragen
Um eine gespeicherte Abfrage zu laden, wählen Sie oben rechts **Abfrage-Explorer** aus. Der Bereich **Abfrage-Explorer** wird mit einer nach Kategorie sortierten Liste aller Abfragen geöffnet. Erweitern Sie die Kategorien, oder geben Sie einen Abfragenamen in die Suchleiste ein, und wählen Sie dann eine Abfrage aus, um sie in den **Abfrage-Editor** zu laden. Sie können eine Abfrage als **Favorit** markieren, indem Sie auf den Stern neben dem Abfragenamen klicken.

![Abfrage-Explorer](media/get-started-portal/query-explorer.png)

### <a name="export-and-share-queries"></a>Exportieren und Freigeben von Abfragen
Um eine Abfrage zu exportieren, klicken Sie in der oberen Leiste auf **Export**, und wählen Sie dann in der Dropdownliste **In CSV-Datei exportieren – Alle Spalten**, **In CSV-Datei exportieren – Angezeigte Spalten** oder **In Power BI exportieren (M-Abfrage)** aus.

Im folgenden Video wird gezeigt, wie Sie Log Analytics in Excel integrieren.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4Asme]

Um einen Link zu einer Abfrage zu teilen, wählen Sie in der oberen Leiste **Link kopieren** und dann **Link in Abfrage kopieren**, **Abfragetext kopieren** oder **Ergebnisse kopieren** aus, um den Link in die Zwischenablage zu kopieren. Sie können den Abfragelink an andere Personen senden, die Zugriff auf denselben Arbeitsbereich haben.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Tutorial erfahren Sie mehr über das Schreiben von Azure Monitor-Protokollabfragen.
> [!div class="nextstepaction"]
> [Schreiben von Azure Monitor-Protokollabfragen](get-started-queries.md)
