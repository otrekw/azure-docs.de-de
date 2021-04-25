---
title: Ausgleichen von Shards – Hyperscale (Citus) – Azure Database for PostgreSQL
description: Verteilen Sie Shards gleichmäßig auf Server, um die Leistung zu verbessern.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 04/09/2021
ms.openlocfilehash: 63322fac4c6ad5b705deedcd8a80466ddd803814
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107305700"
---
# <a name="rebalance-shards-in-hyperscale-citus-server-group"></a>Ausgleichen von Shards in einer Hyperscale (Citus)-Servergruppe

Um neu hinzugefügte Knoten optimal zu nutzen, müssen Sie die verteilte [Shard](concepts-hyperscale-distributed-data.md#shards)tabelle ausgleichen. Das bedeutet, dass einige Shards von vorhandenen Knoten auf neue Knoten verschoben werden.

## <a name="determine-if-the-server-group-needs-a-rebalance"></a>Ermitteln, ob für die Servergruppe ein Ausgleich erforderlich ist

Über das Azure-Portal können Sie anzeigen, ob Daten gleichmäßig auf Workerknoten in einer Servergruppe verteilt werden. Wechseln Sie dazu im Menü **Servergruppenverwaltung** zur Seite **Shard rebalancer** (Shardausgleich). Wenn Daten zwischen Workern verzerrt werden, wird die Meldung **Rebalancing is recommended** (Ausgleich wird empfohlen) zusammen mit einer Liste der Größe der einzelnen Knoten angezeigt.

Wenn die Daten bereits ausgeglichen sind, wird die Meldung **Rebalancing is not recommended at this time** (Ausgleich wird zurzeit nicht empfohlen) angezeigt.

## <a name="run-the-shard-rebalancer"></a>Ausführen des Shardausgleichs

Zum Starten des Shardausgleichs müssen Sie eine Verbindung mit dem Koordinatorknoten der Servergruppe herstellen und die SQL-Funktion [rebalance_table_shards](reference-hyperscale-functions.md#rebalance_table_shards) für verteilte Tabellen ausführen. Die Funktion gleicht alle Tabellen in der Gruppe [Colocation](concepts-hyperscale-colocation.md) der im Argument genannten Tabelle aus. Daher müssen Sie die Funktion nicht für jede verteilte Tabelle aufrufen, sondern einfach nur für eine repräsentative Tabelle aus jeder Zusammenstellungsgruppe.

```sql
SELECT rebalance_table_shards('distributed_table_name');
```

## <a name="monitor-rebalance-progress"></a>Überwachen des Ausgleichsfortschritts

Wenn Sie den Ausgleich nach dem Start überwachen möchten, wechseln Sie zum Azure-Portal. Öffnen Sie im Menü **Servergruppenverwaltung** die Seite **Shard rebalancer** (Shardausgleich). Die Meldung **Rebalancing is underway** (Ausgleich wird ausgeführt) wird zusammen mit zwei Tabellen angezeigt.

Die erste Tabelle zeigt die Anzahl der Shards, die in einen oder aus einem Knoten verschoben werden, z. B. „6 von 24 verschoben“. Die zweite Tabelle zeigt den Fortschritt pro Datenbanktabelle an: Name, betroffene Shardanzahl, betroffene Datengröße und Ausgleichsstatus.

Wählen Sie die Schaltfläche **Aktualisieren** aus, um die Seite zu aktualisieren. Wenn der Ausgleich abgeschlossen ist, wird erneut die Meldung **Rebalancing is not recommended at this time** angezeigt.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Leistungsoptionen](concepts-hyperscale-configuration-options.md) für Servergruppen.
- [Zentrales oder horizontales Hochskalieren einer Servergruppe](howto-hyperscale-scale-grow.md)
- Weitere Informationen finden Sie in den Referenzmaterialien zur [rebalance_table_shards](reference-hyperscale-functions.md#rebalance_table_shards)-Funktion.
