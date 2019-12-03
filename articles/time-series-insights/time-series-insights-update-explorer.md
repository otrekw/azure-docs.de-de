---
title: Visualisieren von Daten im Preview-Explorer – Azure Time Series Insights | Microsoft-Dokumentation
description: Erfahren Sie über die im Azure Time Series Insights Preview-Explorer verfügbaren Funktionen und Optionen.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/13/2019
ms.custom: seodec18
ms.openlocfilehash: 17ba808ebfabb68765cf35bbf0799d117bc6383b
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74133439"
---
# <a name="azure-time-series-insights-preview-explorer"></a>Azure Time Series Insights Preview-Explorer

In diesem Artikel werden die in der Azure Time Series Insights Preview-[Demowebanwendung](https://insights.timeseries.azure.com/preview/demo) verfügbaren verschiedenen Funktionen und Optionen beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

Zum Einstieg in den Azure Time Series Insights Preview-Explorer müssen Sie Folgendes erledigt haben:

* Bereitstellung einer Time Series Insights-Umgebung. Weitere Informationen zur Bereitstellung einer Instanz erhalten Sie im Tutorial [Azure Time Series Insights Preview](./time-series-insights-update-create-environment.md).
* [Bereitstellung von Datenzugriff](./time-series-insights-data-access.md) auf die Time Series Insights-Umgebung, die Sie für das Konto erstellt haben. Außer Ihnen, können Sie auch anderen Zugriff gewähren.
* Hinzufügen einer Ereignisquelle zur Time Series Insights-Umgebung zum Senden von Daten per Push an die Umgebung:
  * Informationen zum [Herstellen einer Verbindung mit einem Event Hub](./time-series-insights-how-to-add-an-event-source-eventhub.md) 
  * Informationen zum [Herstellen einer Verbindung mit einem IoT-Hub](./time-series-insights-how-to-add-an-event-source-iothub.md)

## <a name="explore-the-time-series-insights-preview-explorer"></a>Erkunden des Time Series Insights Preview-Explorers

Der Azure Time Series Insights Preview-Explorer besteht aus den folgenden sieben Elementen:

[![Time Series Insights Preview-Explorer – Übersicht](media/v2-update-explorer/preview-explorer-overview.png)](media/v2-update-explorer/preview-explorer-overview.png#lightbox)

1. [Umgebungsbereich](#1-environment-panel): Zeigt alle Ihre Azure Time Series Insights-Umgebungen an.
1. [Navigationsleiste](#2-navigation-bar): Ermöglicht ihnen das Wechseln zwischen den Seiten **Analysieren** und **Modell**.
1. [Hierarchiestruktur und Suchbereich](#3-hierarchy-tree-and-search-panel): Ermöglicht Ihnen die Auswahl und Suche bestimmter Datenelemente für die Diagrammdarstellung.
1. [Zeitreihenquelle](#4-time-series-well): Zeigt alle Ihre aktuell ausgewählten Datenelemente an.
1. [Diagrammbereich](#5-chart-panel): Zeigt Ihr aktuelles Arbeitsdiagramm an.
1. [Zeitachse](#6-time-editor-panel): Ermöglicht Ihnen die Änderung Ihres Arbeitszeitraums.
1. [App-Leiste](#7-app-bar): Enthält Ihre Benutzerverwaltungsoptionen (z. B. den aktuellen Mandanten) und gestattet Ihnen die Änderung von Design- und Spracheinstellungen.


## <a name="1-environment-panel"></a>1. Umgebungsbereich

Im Umgebungsbereich werden alle Time Series Insights-Umgebungen angezeigt, auf die Sie Zugriff haben. Die Liste enthält Umgebungen mit nutzungsbasierter Bezahlung (Preview) sowie S1/S2-Umgebungen (allgemeine Verfügbarkeit). Klicken Sie einfach auf die Time Series Insights-Umgebung, die Sie verwenden möchten, um sofort dorthin zu wechseln.

1. Wählen Sie den Dropdownpfeil neben der angezeigten Umgebung aus.

   [![Umgebungsbereich](media/v2-update-explorer/environment-panel.png)](media/v2-update-explorer/environment-panel.png#lightbox)

1. Wählen Sie anschließend die gewünschte Umgebung aus.

## <a name="2-navigation-bar"></a>2. Navigationsleiste

  [![ Die Navigationsleiste](media/v2-update-explorer/navigation-bar.png)](media/v2-update-explorer/navigation-bar.png#lightbox)

In der Navigationsleiste können Sie zwischen zwei Ansichten auswählen:

* **Analysieren**: Ermöglicht umfangreiche Analysen für Ihre modellierten oder nicht modellierten Zeitreihendaten sowie deren Darstellung als Diagramme.
* **Modell**: Ermöglicht das Pushen neuer Time Series Insights Preview-Typen, -Hierarchien und -Instanzen an Ihr Time Series Insights-Modell.

### <a name="model-authoring"></a>Modellerstellung

Azure Time Series Insights Preview unterstützt vollständige CRUD-Vorgänge („Create“, „Read“, „Update“, „Delete“; „Erstellen“, „Lesen“, „Aktualisieren“, „Löschen“) für Ihr Zeitreihenmodell.

[![Der Modellsuchbereich](media/v2-update-explorer/model-search-panel.png)](media/v2-update-explorer/model-search-panel.png#lightbox)

* **Zeitreihenmodelltyp**: Mithilfe von Time Series Insights-Typen können Sie Variablen oder Formeln für Berechnungen definieren. Sie werden mit einer bestimmten Time Series Insights-Instanz verknüpft. Ein Typ kann eine oder mehrere Variablen enthalten.
* **Zeitreihenmodellhierarchie**: Hierarchien sind systematische Organisationen Ihrer Daten. Hierarchien stellen die Beziehungen zwischen verschiedenen Entitäten in Ihren Time Series Insights-Daten dar.
* **Zeitreihenmodellinstanz**: Instanzen sind die Zeitreihen selbst. In den meisten Fällen handelt es sich dabei um die Geräte-ID (**DeviceID**) oder die Objekt-ID (**AssetID**) – also um den eindeutigen Bezeichner des Objekts in der Umgebung.

Weitere Informationen zu Zeitreihenmodellen finden Sie unter [Zeitreihenmodelle](./time-series-insights-update-tsm.md).

## <a name="3-hierarchy-tree-and-search-panel"></a>3. Hierarchiestruktur und Suchbereich

Mit der HIerarchiestruktur und dem Suchbereich können Sie ganz einfach in Ihrer [Zeitreihenmodell](./time-series-insights-update-tsm.md)hierarchie suchen und navigieren, um die spezifischen Zeitreiheninstanzen zu finden, die Sie in Ihrem Diagramm anzeigen möchten. Wenn Sie Ihre Instanzen auswählen, werden sie nicht nur dem aktuellen Diagramm, sondern auch den Daten hinzugefügt. 

[![Hierarchiestruktur und Suchbereich](media/v2-update-explorer/hierarchy-search.png)](media/v2-update-explorer/hierarchy-search.png#lightbox)

Der Suchergebnissebereich erlaubt Ihnen außerdem, Ihre Ergebnisse in einer Hierarchie- oder Listenansicht anzuzeigen, die das Auffinden der anzuzeigenden Instanzen erleichtern.
 
## <a name="4-time-series-well"></a>4. Zeitreihenquelle

In der Quelle werden Instanzfelder und andere Metadaten angezeigt, die ausgewählten Time Series Insights-Instanzen zugeordnet sind. Mithilfe der Kontrollkästchen auf der rechten Seite können Sie bestimmte Instanzen im aktuellen Diagramm ausblenden oder anzeigen. 

  [![Die Vorschauquelle](media/v2-update-explorer/preview-well.png)](media/v2-update-explorer/preview-well.png#lightbox)

Sie können bestimmte Datenelemente aus Ihrer aktuellen Datenquelle entfernen, indem Sie auf der linken Seite des Elements auf das rote Steuerelement **Löschen** (Papierkorb) klicken. Über die Quelle können Sie außerdem steuern, wie jedes Element in dem Diagramm angezeigt wird. Sie können auswählen, Mindest-/Höchstwert für Schatten und Datenpunkte hinzufügen, das Element zeitlich zu verschieben und die Instanz auf abgestufte Weise zu visualisieren. 

Darüber hinaus können Sie mithilfe des Steuerelements für Erkundungen problemlos Zeitverschiebungen und Punktdiagramme erstellen.  

  [![Quellenlayoutoptionen](media/v2-update-explorer/well-layout-options.png)](media/v2-update-explorer/well-layout-options.png#lightbox)

> [!NOTE]
> Sollte die folgende Meldung angezeigt werden, liegen im ausgewählten Zeitraum keine Daten für die Instanz vor. Verwenden Sie zur Behebung des Problems einen größeren Zeitraum, oder vergewissern Sie sich, dass von der Instanz Daten gepusht werden.
>
> ![Keine Datenbenachrichtigung](media/v2-update-explorer/no-data-warning.png)

## <a name="5-chart-panel"></a>5. Diagrammbereich

Mit dem Diagramm können Sie Zeitreiheninstanzen als Linien anzeigen. Sie können den Umgebungsbereich, das Datenmodell und den Zeitraumsteuerelement-Bereich reduzieren, indem Sie auf die Websteuerelemente klicken, um das Diagramm zu vergrößern. 

  [![Vorschaudiagramm „Übersicht“](media/v2-update-explorer/chart-overview.png)](media/v2-update-explorer/chart-overview.png#lightbox)

1. **Diagrammtyp**: Steuert, welche Datenelemente zur Visualisierung verfügbar sind.

1. **Intervallgröße**: Mit dem Schieberegler für die Intervallgröße können Sie die Intervalle in einer Zeitspanne vergrößern und verkleinern. Dies ermöglicht eine genauere Steuerung der Bewegung zwischen größeren Zeitabschnitten, die Trends gleichmäßig bis hin zu kleinsten Zeitabschnitten in Millisekunden anzeigen, sodass Sie präzise, hochauflösende Schnitte Ihrer Daten anzeigen können. Der Standardstartpunkt des Schiebereglers ist als optimierte Ansicht der Daten aus Ihrer Auswahl festgelegt und gleicht somit Auflösung, Abfragegeschwindigkeit und Granularität aus.

1. **Zoomen und Schwenken**: Klicken Sie auf dieses Steuerelement, um das Diagramm zu zoomen und zu schwenken.

1. **Y-Achsen-Steuerelement**: Durchläuft die verfügbaren Optionen für die Y-Achse:

    * `Stacked`: Jede Zeile hat eine eigene Y-Achse.
    * `Overlap`: Ermöglicht das Stapeln mehrerer Zeilen auf derselben Y-Achse, wobei sich die Y-Achsendaten je nach ausgewählter Zeile ändern.
    * `Shared`: Anzeige aller Y-Achsendaten gemeinsam.

1. **Marker-Element**: Das aktuell ausgewählte Datenelement und seine zugehörigen Details.

Sie können ein Drilldown in einen bestimmten Datenschnitt ausführen, indem Sie auf einen Datenpunkt im aktuellen Diagramm **klicken**, die Maustaste gedrückt halten und dann den ausgewählten Bereich zum Endpunkt Ihrer Wahl ziehen. **Klicken Sie mit der rechten Maustaste** auf den abgeblendeten, ausgewählten Bereich, und klicken Sie, wie unten dargestellt, auf **Zoom**. Sie können die Telemetrieereignisse auch in der ausgewählten Zeitspanne anzeigen und herunterladen.

  [![Vorschaudiagramm „Zoom“](media/v2-update-explorer/preview-chart-zoom.png)](media/v2-update-explorer/preview-chart-zoom.png#lightbox)

Nach dem **Zoomen** wird Ihr ausgewähltes Dataset angezeigt. Wählen Sie das Format-Steuerelement aus, um zwischen den drei Y-Achsendarstellungen Ihrer Time Series Insights-Daten zu wechseln.

  [![Vorschaudiagramm für die Y-Achse](media/v2-update-explorer/standard-chart.png)](media/v2-update-explorer/standard-chart.png#lightbox)

Hier sehen Sie ein Beispiel für ein **überlappendes Diagramm**:

  [![Option „Überlappendes Diagramm“](media/v2-update-explorer/overlapping-chart.png)](media/v2-update-explorer/overlapping-chart.png#lightbox)

Die Schaltfläche **Weitere Aktionen** wird erweitert und zeigt die Optionen **Als CSV herunterladen** und **Nach Power BI exportieren** an.

  [![Option „Weitere Aktionen“](media/v2-update-explorer/more-actions-icon.png)](media/v2-update-explorer/more-actions-icon.png#lightbox)

Weitere Informationen zum [Nativen Power BI-Connector für Time Series Insights](concepts-power-bi.md).

## <a name="6-time-editor-panel"></a>6. Editor-Bereich für Zeiten

Wenn Sie mit Time Series Insights arbeiten, wählen Sie zunächst einen Zeitraum aus. Der ausgewählte Zeitraum steuert das Dataset, das für die Manipulation durch die Time Series Insights Update-Widgets verfügbar ist.

  [![Zeitauswahlbereich](media/v2-update-explorer/timeline-element.png)](media/v2-update-explorer/timeline-element.png#lightbox)

> [!TIP]
> Ein Teil der Zeitachse wird in Bernstein oder Orange hervorgehoben, um die Datenspanne anzugeben, die im warmen Speicher verfügbar ist.

Im Time Series Insights Update stehen folgende Websteuerelemente zum Auswählen Ihres Arbeitszeitraums zur Verfügung. 

  [![Steuerelement für Erkundungsquelle](media/v2-update-explorer/exploration-well-control.png)](media/v2-update-explorer/exploration-well-control.png#lightbox)

1. **Schieberegler für den inneren Datumsbereich**: Ziehen Sie die beiden Endpunktsteuerelemente über den gewünschten Zeitraum. Dieser innere Datumsbereich wird durch den Schieberegler für den äußeren Datumsbereich begrenzt.

1. **Schaltflächen zum Erhöhen/Verringern des Datumsbereichs**: Sie können Ihren Zeitraum erhöhen oder verringern, indem Sie eine der Schaltflächen für das gewünschte Intervall auswählen.

1. **Zeitraum-Reduzierungssteuerelement**: Mit diesem Websteuerelement können Sie alle Steuerelemente ausblenden (mit Ausnahme des Schiebereglers für den inneren Datumsbereich).

1. **Schieberegler für den äußeren Datumsbereich**: Verwenden Sie die Endpunktsteuerelemente, um den äußeren Datumsbereich auszuwählen. Dieser steht dann für das Steuerelement für den inneren Datumsbereich zur Verfügung.

1. **Schieberegler für den Zeitbereich**: Hiermit können Sie schnell zwischen voreingestellten Zeitbereichen wechseln. Beispiele wären etwa die letzten **30 Minuten**, die letzten **12 Stunden** oder ein **benutzerdefinierter Bereich**. Durch das Ändern dieses Werts werden auch die verfügbaren Intervallbereiche geändert, die im Intervallgrößen-Schiebereglertool besprochen werden.

   [![Auswahlbereich „In“ und „Aus“](media/v2-update-explorer/to-and-from-element.png)](media/v2-update-explorer/to-and-from-element.png#lightbox)

## <a name="7-app-bar"></a>7. App-Leiste

Der Navigationsbereich von Time Series Insights Preview wird oben in Ihrer Time Series Insights-App angezeigt. Er bietet folgende Funktionen:

### <a name="current-session-share-link-control"></a>Steuerelement für Freigabelink der aktuellen Sitzung

  [![Symbol „Freigeben“](media/v2-update-explorer/share-icon.png)](media/v2-update-explorer/share-icon.png#lightbox)

Wählen Sie das neue Symbol **Freigeben** aus, um einen URL-Link für Ihr Team freizugeben.

  [![Freigeben Ihrer Instanz-URL](media/v2-update-explorer/share-your-view.png)](media/v2-update-explorer/share-your-view.png#lightbox)

### <a name="tenant-section"></a>Mandantenabschnitt

  [![Mandantenauswahl](media/v2-update-explorer/tenant-selection.png)](media/v2-update-explorer/tenant-selection.png#lightbox)

* Zeigt Ihre aktuellen Time Series Insights-Anmeldekontoinformationen an.
* Ermöglicht das Wechseln zwischen den verfügbaren Time Series Insights-Designs.
* Ermöglicht das Anzeigen der [Demo-Web-App](https://insights.timeseries.azure.com/preview/demo) für die Vorschauversion.

### <a name="theme-selection"></a>Designauswahl

Wenn Sie ein neues Design auswählen möchten, wählen Sie rechts oben Ihr Profilsymbol aus. Wählen Sie dann **Design ändern** aus.

  [![Designauswahl](media/v2-update-explorer/theme-selection.png)](media/v2-update-explorer/theme-selection.png#lightbox)

> [!TIP]
> Die Sprachauswahl ist auch verfügbar, wenn Sie Ihr Profilsymbol auswählen.

Azure Time Series Insights Preview unterstützt zwei Designs:

* **Helles Design**: Das Standarddesign, das im gesamten vorliegenden Dokument gezeigt wird.
* **Dunkles Design**: Stellt den Explorer wie hier gezeigt dar:

  [![„Dunkles Design“ ausgewählt](media/v2-update-explorer/dark-theme-selected.png)](media/v2-update-explorer/dark-theme-selected.png#lightbox)

## <a name="s1s2-environment-controls"></a>Steuerelemente für S1/S2-Umgebungen

### <a name="preview-terms-panel"></a>Begriffebereich der Preview

Dieser Abschnitt gilt nur für vorhandene S1/S2-Umgebungen, die versuchen, den Explorer mit der aktualisierten Benutzeroberfläche zu verwenden. Das allgemein verfügbare Produkt und die Vorschauversion können gemeinsam verwendet werden. Wir haben dem aktualisierten Explorer einige Funktionen aus der vorhandenen Benutzeroberfläche hinzugefügt, aber Sie können die vollständige Benutzeroberflächenerfahrung für S1/S2-Umgebungen im vorhandenen Time Series Insights-Explorer erhalten. 

Anstelle der Hierarchie wird der Time Series Insights-Begriffebereich angezeigt, in dem Sie Abfragen in Ihrer Umgebung definieren können. Dadurch können Sie Ihre Daten auf der Grundlage eines Prädikats filtern.

  [![Abfragebereich „Where“](media/v2-update-explorer/s1-s2-preview-query.png)](media/v2-update-explorer/s1-s2-preview-query.png#lightbox)

Der Time Series Insights Preview-Begriffe-Editor-Bereich akzeptiert die folgenden Parameter:

**Where**: Mit der Where-Klausel können Sie Ihre Ereignisse schnell mithilfe der in der folgenden Tabelle aufgeführten Gruppe von Operanden filtern. Wenn Sie eine Suche durch Auswählen eines Operanden ausführen, wird das Prädikat automatisch auf Basis dieser Suche aktualisiert. Folgende Operandentypen werden unterstützt:

| Vorgang | Unterstützte Typen   | Notizen |
| --- | --- | --- |
| `<`, `>`, `<=`, `>=` | Double, DateTime, TimeSpan | |
| `=`, `!=`, `<>` | String, Bool, Double, DateTime, TimeSpan, NULL |
| `IN` | String, Bool, Double, DateTime, TimeSpan, NULL | Alle Operanden müssen denselben Typ aufweisen oder eine NULL-Konstante sein. |
| `HAS` | Zeichenfolge | Im rechten Bereich sind nur konstante Zeichenfolgenliterale zulässig. Leere Zeichenfolgen und NULL sind nicht zulässig. |

Informationen zu den unterstützten Abfragevorgängen und Datentypen finden Sie unter [Time Series Expression (TSX)](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax).

### <a name="examples-of-where-clauses"></a>Beispiele für Where-Klauseln

  [![Beispiele für die „Where-Klausel“](media/v2-update-explorer/example-queries.png)](media/v2-update-explorer/example-queries.png#lightbox)

**Measure**: Eine Dropdownliste mit allen numerischen Spalten (**Doubles**), die Sie als Elemente für Ihr aktuelles Diagramm verwenden können.

**Aufteilen nach**: In dieser Dropdownliste werden alle in Ihrem Modell verfügbaren kategorischen Spalten (Zeichenfolgen) angezeigt, nach denen Sie Ihre Daten gruppieren können. Sie können bis zu fünf Begriffe hinzufügen, um sie auf der gleichen X-Achse anzuzeigen. Geben Sie die gewünschten Parameter ein, und wählen Sie dann **Hinzufügen** aus, um einen neuen Begriff hinzuzufügen.

  [![Abgerufene und gefilterte Ansicht eins](media/v2-update-explorer/s1-s2-preview-filtered-view.png)](media/v2-update-explorer/s1-s2-preview-filtered-view.png#lightbox)

Sie können Elemente im Diagrammbereich ein- und ausblenden, indem Sie das Symbol „Sichtbar“ auswählen, wie in der folgenden Abbildung dargestellt. Wenn Sie Abfragen vollständig entfernen möchten, wählen Sie das rote **X** aus.

  [![Abbrechen einer abgerufenen und gefilterten Option](media/v2-update-explorer/s1-s2-preview-filtered-view-cancel.png)](media/v2-update-explorer/s1-s2-preview-filtered-view-cancel.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über [Datenspeicherung und Dateneingang](./time-series-insights-update-storage-ingress.md) in Azure Time Series Insights Preview.

- Lesen Sie das Time Series Insights Preview-Dokument zur [Datenmodellierung](./time-series-insights-update-tsm.md).

- Informieren Sie sich über die [Vorgehensweise zur Diagnose und Problembehandlung](./time-series-insights-update-how-to-troubleshoot.md) in Ihrer Time Series Insights-Instanz.
