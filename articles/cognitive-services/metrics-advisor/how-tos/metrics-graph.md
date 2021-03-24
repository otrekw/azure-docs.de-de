---
title: Metrikgraph von Metrics Advisor
titleSuffix: Azure Cognitive Services
description: Es wird beschrieben, wie Sie Ihren Metrikgraphen konfigurieren und zugehörige Anomalien in Ihren Daten visualisieren.
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: conceptual
ms.date: 09/08/2020
ms.author: mbullwin
ms.openlocfilehash: bcedef4a1339dacaff3fe841e97f985c42320819
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "92043152"
---
# <a name="how-to-build-a-metrics-graph-to-analyze-related-metrics"></a>Gewusst wie: Erstellen eines Metrikgraphen zum Analysieren von verwandten Metriken

Jede Metrik in Metrics Advisor wird separat mit einem Modell überwacht, das aus Verlaufsdaten lernen kann, um zukünftige Trends vorherzusagen. Jede Metrik verfügt über ein separates Modell, das darauf angewendet wird. In einigen Fällen können unter Umständen aber mehrere Metriken zusammenhängen, sodass Anomalien übergreifend für mehrere Metriken analysiert werden müssen. Hierbei dient der **Metrikgraph** als Hilfe. 

Beispiel: Wenn Sie über unterschiedliche Telemetriedatenströme in separaten Metriken verfügen, werden diese von Metrics Advisor separat überwacht. Falls Anomalien in einer Metrik zu Anomalien in anderen Metriken führen, kann die Ermittlung dieser Beziehungen und der Grundursache in Ihren Daten beim Verarbeiten von Incidents hilfreich sein. Mit dem Metrikgraphen können Sie einen visuellen Topologiegraphen mit den gefundenen Anomalien erstellen. 

## <a name="select-a-metric-to-put-the-first-node-to-the-graph"></a>Auswählen einer Metrik zum Einfügen des ersten Knotens in den Graphen

Klicken Sie in der Navigationsleiste auf die Registerkarte **Metrics Graph** (Metrikgraph). Der erste Schritt beim Erstellen eines Metrikgraphen besteht darin, einen Knoten in den Graphen einzufügen. Wählen Sie oben auf der Seite einen Datenfeed und eine Metrik aus. Im unteren Bereich wird ein Knoten angezeigt. 

:::image type="content" source="../media/graph/metrics-graph-select.png" alt-text="Metrik auswählen":::

## <a name="add-a-noderelation-on-existing-node"></a>Hinzufügen eines Knotens bzw. einer Beziehung im vorhandenen Knoten

Als Nächstes müssen Sie einen weiteren Knoten hinzufügen und eine Beziehung zu einem oder mehreren vorhandenen Knoten angeben. Wählen Sie einen vorhandenen Knoten aus, und klicken Sie mit der rechten Maustaste darauf. Ein Kontextmenü mit mehreren Optionen wird angezeigt. 

Klicken Sie auf **Add relation** (Beziehung hinzufügen). Sie können eine weitere Metrik auswählen und den Typ der Beziehung zwischen den beiden Knoten angeben. Sie können auch bestimmte Dimensionsfilter anwenden. 

:::image type="content" source="../media/graph/metrics-graph-node-action.png" alt-text="Hinzufügen eines Knotens und einer Beziehung":::

Nachdem Sie die obigen Schritte wiederholt haben, verfügen Sie über einen Metrikgraphen, mit dem die Beziehungen zwischen allen verwandten Metriken beschrieben werden.
**Hinweis zu Knotenfarben**
> [!TIP]
> - Bei Auswahl eines Metrik- und Dimensionsfilters werden alle darunter enthaltenen Knoten im Graphen **<font color=blue>blau</font>** gekennzeichnet.
> - Nicht ausgewählte Knoten, die für eine Metrik im Graphen stehen, sind **<font color=green>grün</font>** gekennzeichnet.
> - Falls in der aktuellen Metrik eine Anomalie erkannt wird, wird der Knoten **<font color=red>rot</font>** gekennzeichnet.

## <a name="view-related-metrics-anomaly-status-in-incident-hub"></a>Anzeigen des Anomaliestatus für verwandte Metriken im Incidenthub

Beim Erstellen des Metrikgraphen können Sie bei jeder Erkennung einer Anomalie für die Metriken im Graphen den zugehörigen Anomaliestatus anzeigen und eine allgemeine Ansicht des Incidents aufrufen. 

Klicken Sie im Graphen in einen Incident, und scrollen Sie nach unten zur Option **Cross Metrics Analysis** (Metrikübergreifende Analyse), die sich unterhalb der Diagnoseinformationen befindet.

:::image type="content" source="../media/graph/metrics-graph-cross-metrics-analysis.png" alt-text="Anzeigen verwandter Metriken und Anomalien":::

## <a name="next-steps"></a>Nächste Schritte

- [Anpassen der Anomalieerkennung anhand von Feedback](anomaly-feedback.md)
- [Diagnostizieren eines Incidents](diagnose-incident.md)
- [Konfigurieren von Metriken und Optimieren der Konfigurationserkennung](configure-metrics.md)
