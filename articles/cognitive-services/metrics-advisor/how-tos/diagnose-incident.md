---
title: Diagnostizieren von Incidents mit Metrics Advisor
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie mit Metrics Advisor Incidents diagnostizieren und detaillierte Ansichten zu Anomalien in Ihren Daten erhalten.
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: conceptual
ms.date: 08/19/2020
ms.author: mbullwin
ms.openlocfilehash: ecbfb2d9acf6c62f95c264a14e306442db25e483
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "101703421"
---
# <a name="how-to-diagnose-an-incident-using-metrics-advisor"></a>Gewusst wie: Diagnostizieren eines Incidents mit Metrics Advisor

Metrics Advisor bietet verschiedene Diagnosefunktionen sowie eine detaillierte Ansicht zu erkannten Incidents und stellt eine Fehlerursachenanalyse bereit. Wenn eine Gruppe von Anomalien für eine Metrik erkannt wird, gruppiert Metrics Advisor Anomalien in einer Hierarchie und führt zusätzlich eine Analyse dafür aus.

> [!NOTE]
> Metrics Advisor unterstützt derzeit die Diagnose von Incidents für Metriken mit mindestens einer Dimension und die Messung mit dem *numerischen* Typ. Die Metrik muss über einen aggregierten Dimensionswert wie „Summe“ für jede Dimension verfügen, der zum Erstellen der Diagnosehierarchie verwendet wird. Metrics Advisor bietet [**Einstellungen für automatisches Rollup**](onboard-your-data.md#automatic-roll-up-settings), um die Erstellung aggregierter Werte zu unterstützen. 

Klicken Sie im linken Navigationsfenster auf **Incident hub** (Incidenthub), um alle Incidents unter einer bestimmten Metrik anzuzeigen. Oben auf der Seite können Sie verschiedene Metriken auswählen, um deren Erkennungskonfigurationen und Erkennungsergebnisse anzuzeigen und den Zeitbereich zu ändern.

> [!TIP]
> Zum **Incident hub** (Incidenthub) gelangen Sie auch wie folgt:
> * Klicken Sie auf einen Datenpunkt in der Visualisierung für die Metrik, und verwenden Sie die Links am unteren Rand des angezeigten Fensters **Feedback hinzufügen**.
> * Klicken Sie auf eine der Anomalien auf der Registerkarte **Incidents** für Ihre Metrik. 

Der Abschnitt **Übersicht** enthält Erkennungsergebnisse, einschließlich der Anzahl von Anomalien und Warnungen innerhalb des ausgewählten Zeitraums.

:::image type="content" source="../media/diagnostics/incident-hub-overview.png" alt-text="Incident hub (Incidenthub)" lightbox="../media/diagnostics/incident-hub-overview.png":::

Erkannte Incidents innerhalb der ausgewählten Metrik und des Zeitbereichs werden in der **Incident list** (Incidentliste) aufgeführt. Es gibt Optionen zum Filtern und Sortieren der Incidents. Beispielsweise nach Schweregrad. Klicken Sie auf einen Incident, um zur **Incidentseite** zu gelangen, um eine weitere Diagnose durchzuführen.

:::image type="content" source="../media/diagnostics/incident-list.png" alt-text="Incident list (Incidentliste)" lightbox="../media/diagnostics/incident-list.png":::

Im Abschnitt **Diagnose** können Sie eine detaillierte Analyse eines Incidents ausführen und erhalten Tools zum Identifizieren der Ursachen.

:::image type="content" source="../media/diagnostics/diagnose-incident.png" alt-text="Diagnostizieren eines Incidents" lightbox="../media/diagnostics/diagnose-incident.png" :::

## <a name="root-cause-advice"></a>Ursachenratschlag

Wenn eine Gruppe von Anomalien in einer Metrik erkannt wird und dies einen Incident verursacht, versucht Metrics Advisor, die Grundursache des Vorfalls zu analysieren. **Root cause advice** (Ursachenratschlag) bietet automatische Vorschläge für wahrscheinliche Grundursachen eines Vorfalls. Diese Funktion ist nur verfügbar, wenn ein aggregierter Wert innerhalb der Dimension vorhanden ist. Wenn die Metrik keine Dimension hat, ist sie selbst die Grundursache. Die Grundursachen sind im rechten Bereich aufgelistet und umfassen ggf. mehrere Gründe. Wenn keine Daten in der Tabelle vorhanden sind, bedeutet dies, dass Ihre Dimension die Anforderungen nicht erfüllt, um eine Analyse durchzuführen.

:::image type="content" source="../media/diagnostics/root-cause-advice.png" lightbox="../media/diagnostics/root-cause-advice.png" alt-text="Root cause advice (Ursachenratschlag)":::


Wenn die Grundursachenmetrik mit bestimmten Dimensionen bereitgestellt wird, klicken Sie auf **go to metric** (Zu Metrik wechseln), um weitere Details der Metrik anzuzeigen.

## <a name="incident-tree"></a>Incidentstruktur

Zusammen mit der automatisierten Analyse zu möglichen Grundursachen unterstützt Metrics Advisor die manuelle Ursachenanalyse mithilfe von **Incident Tree** (Incidentstruktur). Auf der Incidentseite gibt es zwei Arten von Incidentstrukturen: die Struktur **quick diagnose** (Schnelldiagnose) und **interactive tree** (interaktive Struktur).

Die Schnelldiagnosestruktur dient zur Diagnose eines aktuellen Incidents, und der Stammknoten ist auf den aktuellen Incidentstammknoten beschränkt. Sie können die Strukturknoten erweitern und reduzieren, indem Sie darauf klicken, und die zugehörige Reihe wird zusammen mit der aktuellen Incidentreihe im Diagramm über der Struktur angezeigt.

Mit der interaktiven Struktur können Sie aktuelle Incidents sowie ältere und verknüpfte Incidents diagnostizieren. Wenn Sie die interaktive Struktur verwenden, klicken Sie mit der rechten Maustaste auf einen Knoten, um ein Aktionsmenü zu öffnen. Hier können Sie eine Dimension für ein Drillup für die Stammknoten sowie eine Dimension für ein Drilldown für die einzelnen Knoten hinzufügen. Wenn Sie auf die Schaltfläche „Abbrechen“ der Dimensionsliste oben klicken, können Sie das Drillup oder das Drilldown für diese Dimension entfernen. Klicken Sie mit der linken Maustaste auf einen Knoten, um ihn auszuwählen und die zugehörige Reihe mit der aktuellen Incidentreihe im Diagramm anzuzeigen.

:::image type="content" source="../media/diagnostics/incident-tree.png" alt-text="Incident tree (Incidentstruktur)" lightbox="../media/diagnostics/incident-tree.png" :::

## <a name="anomaly-drill-down"></a>Drilldown für Anomalien

Wenn Sie Incidentinformationen anzeigen, müssen Sie ggf. ausführlichere Informationen abrufen, z. B. für verschiedene Dimensionen und Zeitstempel. Wenn Ihre Daten über mindestens eine Dimension verfügen, können Sie mit der Drilldownfunktion eine detailliertere Ansicht erhalten. 

Um die Drilldownfunktion zu verwenden, klicken Sie unter **Incident hub** (Incidenthub) auf die Registerkarte **Metric drilling** (Metrikdrilling). 

:::image type="content" source="../media/diagnostics/metric-drilling.png" lightbox="../media/diagnostics/metric-drilling.png" alt-text="Metric drilling (Metrikdrilling)":::

Die Einstellung **Dimensionen** ist eine Liste von Dimensionen für einen Incident, Sie können jeweils andere verfügbare Dimensionswerte auswählen. Nach dem Ändern der Dimensionswerte Mit der Einstellung **Zeitstempel** können Sie den aktuellen Incident zu anderen Zeitpunkten anzeigen.

### <a name="select-drilling-options-and-choose-a-dimension"></a>Auswählen von Drillingoptionen und einer Dimension

Es gibt zwei Typen von Drilldownoptionen: **Drilldown** und **Horizontal comparison** (Horizontaler Vergleich).

> [!Note]
> 1. Bei einem Drilldown können Sie die Daten unterschiedlicher Dimensionswerte untersuchen, mit Ausnahme der aktuell ausgewählten Dimensionen. 
> 2. Bei einem horizontalen Vergleich können Sie die Daten unterschiedlicher Dimensionswerte untersuchen, mit Ausnahme der Gesamtdimensionen.

:::image type="content" source="../media/diagnostics/drill-down-dimension.png" lightbox="../media/diagnostics/drill-down-dimension.png" alt-text="Drilldowndimension":::

### <a name="value-comparison-for-different-dimension-values"></a>Wertvergleich für verschiedene Dimensionswerte

Der zweite Abschnitt der Registerkarte „Drilldown“ ist eine Tabelle mit Vergleichen für unterschiedliche Dimensionswerte. Sie enthält den Wert, den Grundwert, den Differenzwert und den Deltawert und gibt an, ob es sich um eine Anomalie handelt.
 
:::image type="content" source="../media/diagnostics/drill-down-comparison.png" alt-text="Drilldownvergleich" lightbox="../media/diagnostics/drill-down-comparison.png":::


### <a name="value-and-expected-value-comparisons-for-different-dimension-value"></a>Vergleiche von Werten und erwarteten Werten für unterschiedliche Dimensionswerte

Der dritte Abschnitt der Registerkarte „Drilldown“ ist ein Histogramm mit den Werten und den erwarteten Werten für verschiedene Dimensionswerte. Das Histogramm ist nach dem Unterschied zwischen dem Wert und dem erwarteter Wert sortiert. Sie können den unerwarteten Wert mit der größten Auswirkung leicht finden. In der obigen Abbildung können Sie beispielsweise feststellen, dass **US7** (mit Ausnahme des Gesamtwerts) am meisten zur Anomalie beiträgt.

:::image type="content" source="../media/diagnostics/drill-down-table.png" alt-text="Drilldowntabelle" lightbox="../media/diagnostics/drill-down-table.png":::

### <a name="raw-value-visualization"></a>Visualisierung von Rohwerten
Der letzte Teil der Drilldowntabelle ist ein Liniendiagramm der Rohwerte. Mit diesem Diagramm müssen Sie nicht zur Metrikseite navigieren, um Details anzuzeigen.

:::image type="content" source="../media/diagnostics/drill-down-line-chart.png" alt-text="Drilldown-Liniendiagramm" lightbox="../media/diagnostics/drill-down-line-chart.png":::

## <a name="view-similar-anomalies-using-time-series-clustering"></a>Anzeigen ähnlicher Anomalien mithilfe von Zeitreihen-Clustering

Beim Anzeigen eines Incidents können Sie die Registerkarte **Similar time-series-clustering** (Clustering ähnlicher Zeitreihen) verwenden, um die verschiedenen zugeordneten Reihen anzuzeigen. Die Reihen in einer Gruppe werden zusammengefasst. Aus der obigen Abbildung ist zu erkennen, dass mindestens zwei Reihengruppen vorhanden sind. Diese Funktion ist nur verfügbar, wenn die folgenden Anforderungen erfüllt sind:

1. Metriken müssen mindestens eine Dimension oder mindestens einen Dimensionswert aufweisen.
2. Die Reihe innerhalb einer Metrik muss einen ähnlichen Trend aufweisen.

Die verfügbaren Dimensionen werden oben auf der Registerkarte aufgelistet, und Sie können eine Auswahl treffen, um die Reihe anzugeben.

:::image type="content" source="../media/diagnostics/series-group.png" lightbox="../media/diagnostics/series-group.png" alt-text="Reihengruppe":::

## <a name="compare-time-series"></a>Vergleichen von Zeitreihen

Wenn eine Anomalie für eine bestimmte Zeitreihe erkannt wird, ist es manchmal hilfreich, sie mit mehreren anderen Reihen in einer einzelnen Visualisierung zu vergleichen. Klicken Sie auf die Registerkarte **Vergleichstools** und anschließend auf die blaue Schaltfläche **+Hinzufügen**. 

:::image type="content" source="../media/diagnostics/add-series.png" alt-text="Zu vergleichende Reihe hinzufügen" lightbox="../media/diagnostics/add-series.png":::

Wählen Sie eine Reihe aus dem Datenfeed aus. Sie können dieselbe oder eine andere Granularität auswählen. Wählen Sie die Zieldimensionen aus, und laden Sie den Reihentrend. Klicken Sie anschließend auf **OK**, um ihn mit einer früheren Reihe zu vergleichen. Die Reihen werden in einer Visualisierung zusammengefügt. Sie können weitere Reihen für Vergleiche hinzufügen und weitere Erkenntnisse erhalten. Klicken Sie auf das Dropdownmenü oben auf der Registerkarte **Vergleichstools**, um die Zeitreihendaten über einen zeitversetzten Zeitraum zu vergleichen.  

> [!Warning]
> Für einen Vergleich kann die Zeitreihendatenanalyse Verschiebungen der Datenpunkte erfordern, sodass die Granularität Ihrer Daten dies unterstützen muss. Wenn Sie beispielsweise über wöchentliche Daten verfügen und den Vergleich **Day over day** (Tag für Tag) verwenden, erhalten Sie keine Ergebnisse. Verwenden Sie in diesem Beispiel stattdessen den Vergleich **Month over month** (Monat für Monat).

Nachdem Sie den zeitversetzten Vergleich ausgewählt haben, können Sie auswählen, ob Sie die Datenwerte, die Deltawerte oder die Prozentsatz-Deltawerte vergleichen möchten.

> [!Note]
> * Der **Datenwert** ist der Rohdatenwert.
> * **Delta value** (Deltawert) ist der Unterschied zwischen dem Rohwert und dem verglichenen Wert.
> * Der **Percentage delta value** (Prozentsatz-Deltawert) ist der Unterschied zwischen dem Rohwert und dem verglichenen Wert, dividiert durch den verglichenen Wert.

## <a name="related-incidents-across-metrics"></a>Verknüpfte Incidents über Metriken hinweg

Manchmal müssen Sie ggf. gleichzeitig die Incidents verschiedener Metriken oder verknüpfte Incidents in anderen Metriken überprüfen. Eine Liste verknüpfter Incidents finden Sie im Abschnitt **Cross Metrics Analysis** (Metrikübergreifende Analyse). 

:::image type="content" source="../media/graph/metrics-graph-cross-metrics-analysis.png" alt-text="Metrikübergreifende verknüpfte Incidents":::

Bevor Sie verknüpfte Incidents für die aktuelle Metrik anzeigen können, müssen Sie eine Beziehung zwischen den Metriken hinzufügen. Klicken Sie auf **Metrics Graph Settings** (Metrikdiagrammeinstellungen), um eine Beziehung hinzuzufügen. Nur Metriken mit denselben Dimensionsnamen können verknüpft werden. Verwenden Sie die folgenden Parameter.

- „Current Data feed & Metric“ (Aktueller Datenfeed und Metrik): der Datenfeed und die Metrik des aktuellen Incidents
- Richtung: die Richtung der Beziehung zwischen zwei Metriken (keine Auswirkung auf die Liste verwandter Vorfälle)
- „Another Data feed & Metric“ (Weiterer Datenfeed und Metrik): der Datenfeed und die Metrik zum Herstellen einer Verbindung mit der aktuellen Metrik


## <a name="next-steps"></a>Nächste Schritte 

- [Anpassen der Anomalieerkennung anhand von Feedback](anomaly-feedback.md)
- [Konfigurieren von Metriken und Optimieren der Konfigurationserkennung](configure-metrics.md)
