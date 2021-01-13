---
title: Erste Schritte mit dem Azure-Metrik-Explorer
description: Hier erfahren Sie, wie Sie Ihr erstes Metrikdiagramm mit dem Azure-Metrik-Explorer erstellen.
author: vgorbenko
services: azure-monitor
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: 710de93a24037031784ca8d37a8ae0be76bff9e6
ms.sourcegitcommit: 86acfdc2020e44d121d498f0b1013c4c3903d3f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97614591"
---
# <a name="getting-started-with-azure-metrics-explorer"></a>Erste Schritte mit dem Azure-Metrik-Explorer

## <a name="where-do-i-start"></a>Erste Schritte
Der Azure Monitor-Metrik-Explorer ist eine Komponente des Microsoft Azure-Portals, die das Zeichnen von Diagrammen, das visuelle Korrelieren von Trends und das Untersuchen von Spitzen und Tiefen in Metrikwerten ermöglicht. Verwenden Sie den Metrik-Explorer zum Untersuchen der Integrität und Auslastung Ihrer Ressourcen. Beginnen Sie in der folgenden Reihenfolge:

1. [Wählen Sie eine Ressource und eine Metrik aus](#create-your-first-metric-chart), dann wird Ihnen ein einfaches Diagramm angezeigt. [Wählen Sie anschließend einen Zeitraum aus](#select-a-time-range), der für Ihre Untersuchung relevant ist.

1. Versuchen Sie, [Dimensionsfilter und Aufteilung anzuwenden](#apply-dimension-filters-and-splitting). Mithilfe der Filter und der Aufteilung können Sie analysieren, welche Segmente der Metrik zu dem Gesamtwert der Metrik beitragen und mögliche Ausreißer identifizieren.

1. Verwenden Sie [erweiterte Einstellungen](#advanced-chart-settings), um das Diagramm anzupassen, bevor Sie es an Dashboards anheften. [Konfigurieren Sie Warnungen](alerts-metric-overview.md), um Benachrichtigungen zu erhalten, wenn der Metrikwert einen Schwellenwert überschreitet oder darunter fällt.

## <a name="create-your-first-metric-chart"></a>Erstellen eines ersten Metrikdiagramms

Öffnen Sie die Registerkarte **Metriken**, und führen Sie die folgenden Schritte aus, um ein Metrikdiagramm aus Ihrer Ressource, Ressourcengruppe, Ihrem Abonnement oder einer Azure Monitor-Ansicht zu erstellen:

1. Klicken Sie auf die Schaltfläche „Bereich auswählen“, um die Auswahl für den Ressourcenbereich zu öffnen. Auf diese Weise können Sie die Ressourcen auswählen, für die Sie Metriken anzeigen möchten. Wenn Sie den Metrik-Explorer über das Menü einer Ressource geöffnet haben, sollte die Ressource bereits ausgefüllt sein. In [diesem Artikel](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-dynamic-scope) erfahren Sie, wie Sie Metriken für mehrere Ressourcen anzeigen können.
    > ![Auswählen einer Ressource](./media/metrics-getting-started/scope-picker.png)

2. Für einige Ressourcen müssen Sie einen Namespace auswählen. Der Namespace dient lediglich der Organisation der Metriken, damit Sie sich mühelos finden können. Speicherkonten verfügen beispielsweise über separate Namespaces zum Speichern von Metriken zu Dateien, Tabellen, Blobs und Warteschlangen. Viele Ressourcentypen verfügen über nur einen Namespace.

3. Wählen Sie eine Metrik aus der Liste der verfügbaren Metriken aus.

    > ![Metrik auswählen](./media/metrics-getting-started/metrics-dropdown.png)

4. Optional können Sie die [Metrikaggregation ändern](metrics-charts.md#changing-aggregation), wenn Sie beispielsweise möchten, dass Ihr Diagramm nur die minimalen, maximalen oder durchschnittlichen Werte der Metrik anzeigt.

> [!TIP]
> Klicken Sie auf **Metrik hinzufügen**, und wiederholen Sie diese Schritte, wenn mehrere Metriken im gleichen Diagramm angezeigt werden sollen. Wenn mehrere Diagramme in einer Ansicht angezeigt werden sollen, klicken Sie oben auf **Diagramm hinzufügen**.

## <a name="select-a-time-range"></a>Auswählen eines Zeitbereichs

> [!WARNING]
> [Die meisten Metriken werden in Azure 93 Tage lang gespeichert](data-platform-metrics.md#retention-of-metrics). Sie können jedoch maximal Daten für 30 Tage in einem einzelnen Diagramm abfragen. Diese Einschränkung gilt nicht für [protokollbasierte Metriken](../app/pre-aggregated-metrics-log-metrics.md#log-based-metrics).

Das Diagramm zeigt standardmäßig die Metrikdaten der letzten 24 Stunden an. Verwenden Sie den Bereich **Zeitauswahl**, um den Zeitraum, die Vergrößerung oder die Verkleinerung Ihres Diagramms zu ändern. 

![Bereich zum Ändern des Zeitraums](./media/metrics-getting-started/time.png)

> [!TIP]
> Verwenden Sie **Time Brush**, um einen interessanten Bereich des Diagramms (Spitze oder Senke) zu untersuchen. Setzen Sie den Mauszeiger an den Anfang des Bereichs, klicken und halten Sie die linke Maustaste gedrückt, ziehen Sie den Mauszeiger zur anderen Seite des Bereichs, und lassen Sie die Maustaste dann los. Im Diagramm wird dieser Zeitbereich vergrößert. 

## <a name="apply-dimension-filters-and-splitting"></a>Anwenden von Dimensionsfiltern und Aufteilung

[Filtern](metrics-charts.md#apply-filters-to-charts) und [Aufteilen](metrics-charts.md#apply-splitting-to-a-chart) sind leistungsstarke Diagnosetools für Metriken mit Dimensionen. Diese Features zeigen, wie sich verschiedene Metriksegmente („Dimensionswerte“) auf den Gesamtwert der Metrik auswirken und ermöglichen Ihnen, mögliche Ausreißer zu identifizieren.

- Durch das **Filtern** können Sie entscheiden, welche Dimensionswerte im Diagramm enthalten sind. Wenn Sie beispielsweise die erfolgreichen Anforderungen in einem Diagramm der Metrik für die *Serverantwortzeit* veranschaulichen möchten, müssen Sie den Filter auf die Dimension *success of request* (Erfolg der Anforderung) anwenden. 

- Durch das **Aufteilen** können Sie steuern, ob das Diagramm separate Linien für jeden Wert einer Dimension anzeigt oder die Werte in eine Linie zusammenfügt. Beispielsweise können Sie eine Linie für die durchschnittliche Antwortzeit aller Serverinstanzen oder separate Linien für jeden Server anzeigen. In diesem Fall müssten Sie die Aufteilung auf die Dimension *Serverinstanz* anwenden, damit separate Linien angezeigt werden.

In den [Beispielen für Metrikdiagramme](metric-chart-samples.md) können Sie sich Beispiele mit angewandter Filterung und Aufteilung ansehen. Im Artikel werden auch die Schritte für die Konfiguration der Beispieldiagramme erläutert.

## <a name="advanced-chart-settings"></a>Erweiterte Diagrammeinstellungen

Sie können die Formatierung des Diagramms, den Titel und erweiterte Diagrammeinstellungen anpassen. Wenn Sie Ihre Anpassungen vorgenommen haben, heften Sie das Diagramm an ein Dashboard an, um Ihr Werk zu speichern. Sie können auch Metrikwarnungen konfigurieren. Lesen Sie die [Produktdokumentation](metrics-charts.md), um mehr über diese und weitere erweiterte Features des Azure Monitor-Metrik-Explorers zu erfahren.

## <a name="next-steps"></a>Nächste Schritte

* [Erweiterte Funktionen des Metrik-Explorers](metrics-charts.md)
* [Anzeigen mehrerer Ressourcen in Metrik-Explorer](metrics-dynamic-scope.md)
* [Problembehandlung für den Metrik-Explorer](metrics-troubleshoot.md)
* [Unterstützte Metriken von Azure Monitor](metrics-supported.md)
* [Beispiele für Metrikdiagramme](metric-chart-samples.md)
