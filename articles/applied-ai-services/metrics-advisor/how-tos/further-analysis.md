---
title: Weiteres Analysieren eines Incidents und Bewerten seiner Auswirkungen
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie Analysetools nutzen, um einen Incident weiter zu analysieren.
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: conceptual
ms.date: 04/15/2021
ms.author: mbullwin
ms.openlocfilehash: 0aefc4207f064550b41f9341e946cae2f1611961
ms.sourcegitcommit: 192444210a0bd040008ef01babd140b23a95541b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2021
ms.locfileid: "114341778"
---
# <a name="further-analyze-an-incident-and-evaluate-impact"></a>Weiteres Analysieren eines Incidents und Bewerten seiner Auswirkungen

## <a name="metrics-drill-down-by-dimensions"></a>Drilldown von Metriken nach Dimensionen

Wenn Sie Incidentinformationen anzeigen, müssen Sie ggf. ausführlichere Informationen abrufen, z. B. für verschiedene Dimensionen und Zeitstempel. Wenn Ihre Daten über mindestens eine Dimension verfügen, können Sie mit der Drilldownfunktion eine detailliertere Ansicht erhalten. 

Um die Drilldownfunktion zu verwenden, klicken Sie unter **Incident hub** (Incidenthub) auf die Registerkarte **Metric drilling** (Metrikdrilling). 

:::image type="content" source="../media/diagnostics/metric-drilling.png" lightbox="../media/diagnostics/metric-drilling.png" alt-text="Metric drilling (Metrikdrilling)":::

Die Einstellung **Dimensionen** ist eine Liste von Dimensionen für einen Incident, Sie können jeweils andere verfügbare Dimensionswerte auswählen. Nach dem Ändern der Dimensionswerte Mit der Einstellung **Zeitstempel** können Sie den aktuellen Incident zu anderen Zeitpunkten anzeigen.

### <a name="select-drilling-options-and-choose-a-dimension"></a>Auswählen von Drillingoptionen und einer Dimension

Es gibt zwei Typen von Drilldownoptionen: **Drilldown** und **Horizontal comparison** (Horizontaler Vergleich).

> [!Note]
> - Bei einem Drilldown können Sie die Daten unterschiedlicher Dimensionswerte untersuchen, mit Ausnahme der aktuell ausgewählten Dimensionen. 
> - Bei einem horizontalen Vergleich können Sie die Daten unterschiedlicher Dimensionswerte untersuchen, mit Ausnahme der Gesamtdimensionen.

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

## <a name="compare-time-series"></a>Vergleichen von Zeitreihen

Wenn eine Anomalie für eine bestimmte Zeitreihe erkannt wird, ist es manchmal hilfreich, sie mit mehreren anderen Reihen in einer einzelnen Visualisierung zu vergleichen. Klicken Sie auf die Registerkarte **Vergleichstools** und anschließend auf die blaue Schaltfläche **+Hinzufügen**. 

:::image type="content" source="../media/diagnostics/add-series.png" alt-text="Zu vergleichende Reihe hinzufügen" lightbox="../media/diagnostics/add-series.png":::

Wählen Sie eine Reihe aus dem Datenfeed aus. Sie können dieselbe oder eine andere Granularität auswählen. Wählen Sie die Zieldimensionen aus, und laden Sie den Reihentrend. Klicken Sie anschließend auf **OK**, um ihn mit einer früheren Reihe zu vergleichen. Die Reihen werden in einer Visualisierung zusammengefügt. Sie können weitere Reihen für Vergleiche hinzufügen und weitere Erkenntnisse erhalten. Klicken Sie auf das Dropdownmenü oben auf der Registerkarte **Vergleichstools**, um die Zeitreihendaten über einen zeitversetzten Zeitraum zu vergleichen.  

> [!Warning]
> Für einen Vergleich kann die Zeitreihendatenanalyse Verschiebungen der Datenpunkte erfordern, sodass die Granularität Ihrer Daten dies unterstützen muss. Wenn Sie beispielsweise über wöchentliche Daten verfügen und den Vergleich **Day over day** (Tag für Tag) verwenden, erhalten Sie keine Ergebnisse. Verwenden Sie in diesem Beispiel stattdessen den Vergleich **Month over month** (Monat für Monat).

Nachdem Sie den zeitversetzten Vergleich ausgewählt haben, können Sie auswählen, ob Sie die Datenwerte, die Deltawerte oder die Prozentsatz-Deltawerte vergleichen möchten.

## <a name="view-similar-anomalies-using-time-series-clustering"></a>Anzeigen ähnlicher Anomalien mithilfe von Zeitreihen-Clustering

Beim Anzeigen eines Incidents können Sie die Registerkarte **Similar time-series-clustering** (Clustering ähnlicher Zeitreihen) verwenden, um die verschiedenen zugeordneten Reihen anzuzeigen. Die Reihen in einer Gruppe werden zusammengefasst. Aus der obigen Abbildung ist zu erkennen, dass mindestens zwei Reihengruppen vorhanden sind. Diese Funktion ist nur verfügbar, wenn die folgenden Anforderungen erfüllt sind:

- Metriken müssen mindestens eine Dimension oder mindestens einen Dimensionswert aufweisen.
- Die Reihe innerhalb einer Metrik muss einen ähnlichen Trend aufweisen.

Die verfügbaren Dimensionen werden oben auf der Registerkarte aufgelistet, und Sie können eine Auswahl treffen, um die Reihe anzugeben.

:::image type="content" source="../media/diagnostics/series-group.png" lightbox="../media/diagnostics/series-group.png" alt-text="Reihengruppe":::