---
title: Ausgleichen von Subclustered-Metriken
description: Auswirkung von Platzierungseinschränkungen auf den Lastenausgleich und deren Behandlung
author: nipavlo
ms.topic: conceptual
ms.date: 03/15/2020
ms.author: nipavlo
ms.openlocfilehash: 7f571a851e4da147240c524b742bcd652bc54181
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "82183114"
---
# <a name="balancing-of-subclustered-metrics"></a>Ausgleichen von Subclustered-Metriken

## <a name="what-is-subclustering"></a>Was ist Subclustering?

Subclustering erfolgt, wenn Dienste mit verschiedenen Platzierungseinschränkungen über eine gemeinsame Metrik verfügen und beide eine Last für diese Metrik melden. Wenn die von den Diensten gemeldete Last erheblich abweicht, weist die Gesamtlast auf den Knoten eine große Standardabweichung auf, und es sieht so aus, als ob der Cluster unausgeglichen ist, auch wenn er das bestmögliche Gleichgewicht aufweist.

## <a name="how-subclustering-affects-load-balancing"></a>Auswirkungen von Subclustering auf den Lastenausgleich

Wenn die von den Diensten auf verschiedenen Knoten gemeldete Last erheblich abweicht, kann es so aussehen, als ob es ein großes Ungleichgewicht gibt, obwohl dies nicht der Fall ist. Wenn das von Subclustering verursachte falsche Ungleichgewicht größer als das tatsächliche Ungleichgewicht ist, besteht außerdem die Möglichkeit, den Resource Manager-Ausgleichsalgorithmus negativ zu beeinflussen und ein suboptimales Gleichgewicht im Cluster zu generieren.

Angenommen, es sind vier Dienste vorhanden, die alle eine Last für die Metrik Metric1 melden:

* Dienst A: Hat eine Platzierungseinschränkung „NodeType==Frontend“, meldet eine Last von 10
* Dienst B: Hat eine Platzierungseinschränkung „NodeType==Frontend“, meldet eine Last von 10
* Dienst C: Hat eine Platzierungseinschränkung „NodeType==Backend“, meldet eine Last von 100
* Dienst D: Hat eine Platzierungseinschränkung „NodeType==Backend“, meldet eine Last von 100
* Und wir haben vier Knoten. Für zwei von ihnen ist NodeType auf „Frontend“ festgelegt, für die anderen beiden auf „Backend“.

Außerdem liegt die die folgende Platzierung vor:

<center>

![Beispiel für Subclustered-Platzierung][Image1]
</center>

Der Cluster kann unausgeglichen aussehen. Auf den Knoten 3 und 4 gibt es eine große Last, aber diese Platzierung schafft das bestmögliche Gleichgewicht in dieser Situation.

Resource Manager kann Subclustering-Situationen erkennen und in fast allen Fällen das optimale Gleichgewicht für die jeweilige Situation herstellen.

In einigen außergewöhnlichen Situationen, in denen Resource Manager nicht in der Lage ist, eine Subclustered-Metrik optimal auszugleichen, wird weiterhin Subclustering erkannt, und es wird ein Integritätsbericht generiert, um Sie beim Beheben des Problems zu unterstützen.

## <a name="types-of-subclustering-and-how-they-are-handled"></a>Typen von Subclustering und deren Behandlung

Subclustering-Situationen können in drei verschiedene Kategorien eingeteilt werden. Die Kategorie einer bestimmten Subclustering-Situation bestimmt, wie sie von Resource Manager behandelt wird.

### <a name="first-category--flat-subclustering-with-disjoint-node-groups"></a>Erste Kategorie: Flaches Subclustering mit zusammenhanglosen Knotengruppen

Diese Kategorie bietet die einfachste Art von Subclustering, bei der Knoten in verschiedene Gruppen aufgeteilt werden können, und jeder Dienst kann nur auf Knoten in einer dieser Gruppen platziert werden. Jeder Knoten gehört nur und ausschließlich zu einer Gruppe. Die oben beschriebene Situation gehört in diese Kategorie, ebenso wie die meisten Subclustering-Situationen. 

In den Situationen in dieser Kategorie kann Resource Manager einen optimalen Ausgleich generieren, und es ist kein weiterer Eingriff erforderlich.

### <a name="second-category--subclustering-with-hierarchical-node-groups"></a>Zweite Kategorie: Subclustering mit hierarchischen Knotengruppen

Diese Situation tritt ein, wenn eine Gruppe von Knoten, die für einen Dienst zulässig ist, eine Untermenge der Gruppe von Knoten ist, die für einen anderen Dienst zulässig ist. Das häufigste Beispiel für diese Situation ist, wenn für einen Dienst eine Platzierungseinschränkung definiert ist und ein anderer Dienst keine Platzierungseinschränkung aufweist und auf jedem Knoten platziert werden kann.

Beispiel:

* Dienst A: Keine Platzierungseinschränkung
* Dienst B: Platzierungseinschränkung „NodeType==Frontend“
* Dienst C: Platzierungseinschränkung „NodeType==Backend“

Diese Konfiguration erstellt eine Teilmengen-/Obermengenbeziehung zwischen Knotengruppen für verschiedene Dienste.

<center>

![Teilmengen-/Obermengencluster][Image2]
</center>

In dieser Situation besteht die Möglichkeit, dass ein suboptimaler Ausgleich vorgenommen wird.

Resource Manager erkennt diese Situation und generiert einen Integritätsbericht, der empfiehlt, Dienst A in zwei Dienste aufzuteilen: Dienst A1, der auf Frontend-Knoten platziert werden kann, und Dienst A2, der auf Backend-Knoten platziert werden kann. Damit kommen wir zurück zur Situation der ersten Kategorie, die optimal ausgeglichen werden kann.

### <a name="third-category--subclustering-with-partial-overlap-between-node-sets"></a>Dritte Kategorie: Subclustering mit partieller Überschneidung zwischen Knotensätzen

Diese Situation tritt auf, wenn es eine partielle Überschneidung zwischen Knotengruppen gibt, auf denen einige Dienste platziert werden können.

Wenn z. B. eine Knoteneigenschaft namens „NodeColor“ vorhanden ist und drei Knoten verwendet werden:

* Knoten 1: NodeColor=Red
* Knoten 2: NodeColor=Blue
* Knoten 2: NodeColor=Green

Und es gibt zwei Dienste:

* Dienst A: Mit Platzierungseinschränkung „Color==Red || Color==Blue“
* Dienst B: Mit Platzierungseinschränkung „Color==Blue || Color==Green“

Aus diesem Grund kann Dienst A auf Knoten 1 und 2 und Dienst B auf Knoten 2 und 3 platziert werden.

In dieser Situation besteht die Möglichkeit, dass ein suboptimaler Ausgleich vorgenommen wird.

Resource Manager erkennt diese Situation und generiert einen Integritätsbericht, in dem empfohlen wird, einige der Dienste aufzuteilen.

Für diese Situation ist Resource Manager nicht in der Lage, einen Vorschlag zu unterbreiten, wie die Dienste aufgeteilt werden können, da mehrere Aufteilungen möglich sind und es keine Möglichkeit gibt, abzuschätzen, welcher Weg der optimale zur Aufteilung der Dienste wäre.

## <a name="configuring-subclustering"></a>Konfigurieren von Subclustering

Das Verhalten von Resource Manager zu Subclustering kann durch Ändern der folgenden Konfigurationsparameter geändert werden:
* SubclusteringEnabled: Der Parameter bestimmt, ob Resource Manager Subclustering beim Lastenausgleich berücksichtigt. Wenn dieser Parameter deaktiviert ist, ignoriert Resource Manager Subclustering und versucht, ein optimales Gleichgewicht auf globaler Ebene zu erzielen. Der Standardwert dieses Parameters ist FALSE.
* SubclusteringReportingPolicy: Bestimmt, wie Resource Manager Integritätsberichte für hierarchisches und sich partiell überschneidendes Subclustering ausgibt. Der Wert NULL bedeutet, dass Integritätsberichte zu Subclustering deaktiviert werden. „1“ bedeutet, dass Warnungsintegritätsberichte für suboptimale Subclustering-Situationen generiert werden, und der Wert „2“ besagt, dass Integritätsberichte vom Typ „OK“ generiert werden. Der Standardwert für diesen Parameter ist „1“.

ClusterManifest.xml

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="SubclusteringEnabled" Value="true" />
            <Parameter Name="SubclusteringReportingPolicy" Value="1" />
        </Section>
```

Über „ClusterConfig.json“ für eigenständige Bereitstellungen bzw. über „Template.json“ für in Azure gehostete Cluster:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "SubclusteringEnabled",
          "value": "true"
      },
      {
          "name": "SubclusteringReportingPolicy",
          "value": "1"
      },
    ]
  }
]
```

## <a name="next-steps"></a>Nächste Schritte
* Informationen darüber, wie der Clusterressourcen-Manager die Auslastung im Cluster verwaltet und verteilt, finden Sie im Artikel zum [Lastenausgleich](service-fabric-cluster-resource-manager-balancing.md)
* Informationen dazu, wie die Platzierung Ihrer Dienste auf bestimmte Knoten beschränkt werden kann, finden Sie unter [Knoteneigenschaften und Platzierungseinschränkungen.](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints)

[Image1]:./media/cluster-resource-manager-subclustering/subclustered-placement.png
[Image2]:./media/cluster-resource-manager-subclustering/subset-superset-nodes.png
