---
title: Skalieren von Servergruppen – Hyperscale (Citus) – Azure Database for PostgreSQL
description: Anpassen von Servergruppenspeicher-, Datenträger- und CPU-Ressourcen, um höhere Auslastungen zu bewältigen
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 8/10/2020
ms.openlocfilehash: 5b1a5da688b162c85d2be8580e29dc6ee9db6d40
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90906482"
---
# <a name="server-group-size"></a>Größe der Servergruppe

Die Bereitstellungsoption „Hyperscale (Citus)“ verwendet kooperierende Datenbankserver, um Abfragen parallel auszuführen und mehr Daten zu speichern. Die „Größe“ der Servergruppe bezieht sich sowohl auf die Anzahl von Servern als auch auf die Hardwareressourcen jedes Servers.

## <a name="picking-initial-size"></a>Auswählen der Anfangsgröße

Die Größe einer Servergruppe in Bezug auf die Anzahl von Knoten und die Hardwarekapazität lässt sich ganz einfach ändern ([siehe unten](#scale-a-hyperscale-citus-server-group)). Für neue Servergruppen müssen Sie aber dennoch eine Anfangsgröße auswählen. Im Folgenden finden Sie einige Tipps für eine sinnvolle Festlegung dieses Werts.

### <a name="multi-tenant-saas-use-case"></a>Anwendungsfall für mehrinstanzenfähige SaaS-Anwendungen

Wenn Sie von einer vorhandenen PostgreSQL-Datenbankinstanz mit einem einzelnen Knoten zu Hyperscale (Citus) migrieren, empfiehlt sich die Auswahl eines Clusters, in dem die Anzahl der virtuellen Workerkerne und der Gesamtarbeitsspeicher den Werten der ursprünglichen Instanz entspricht. In solchen Szenarien konnten wir Leistungssteigerungen auf das Zwei- bis Dreifache feststellen, weil das Sharding die Ressourcennutzung verbessert, kleinere Indizes ermöglicht usw.

Die Anzahl der für den Koordinatorknoten benötigten virtuellen Kerne richtet sich nach Ihrer vorhandenen Workload (Lese-/Schreibdurchsatz). Der Koordinatorknoten benötigt nicht so viel RAM wie Workerknoten, aber die RAM-Zuordnung wird basierend auf der Anzahl von virtuellen Kernen ermittelt (wie in den [Konfigurationsoptionen für Hyperscale](concepts-hyperscale-configuration-options.md) beschrieben). Daher ist die Anzahl von virtuellen Kernen die eigentliche Entscheidung.

### <a name="real-time-analytics-use-case"></a>Anwendungsfall für Echtzeitanalysen

Virtuelle Kerne insgesamt: Wenn Arbeitsdaten in den RAM passen, können Sie von einer linearen Leistungsverbesserung bei Hyperscale (Citus) proportional zur Anzahl der Workerkerne ausgehen. Berücksichtigen Sie beim Ermitteln der richtigen Anzahl von virtuellen Kernen für Ihre Anforderungen die aktuelle Latenz für Abfragen in Ihrer Einzelknoten-Datenbank und die erforderliche Latenz in Hyperscale (Citus). Teilen Sie die aktuelle Wartezeit durch die gewünschte Wartezeit, und runden Sie das Ergebnis.

Worker-RAM: Der beste Fall wäre die Bereitstellung von so viel Speicher, dass der Großteil des Arbeitssatzes in den Speicher passt. Die Art der Abfragen, die Ihre Anwendung verwendet, wirkt sich auf die Arbeitsspeicheranforderungen aus. Sie können EXPLAIN ANALYZE für eine Abfrage ausführen, um zu ermitteln, wie viel Arbeitsspeicher diese Abfrage benötigt. Denken Sie daran, dass virtuelle Kerne und RAM zusammen skaliert werden, wie im Artikel [Konfigurationsoptionen für Hyperscale](concepts-hyperscale-configuration-options.md) beschrieben.

## <a name="scale-a-hyperscale-citus-server-group"></a>Skalieren einer Hyperscale (Citus)-Servergruppe

Azure Database for PostgreSQL – Hyperscale (Citus) unterstützt Self-Service-Skalierung, um höhere Auslastungen zu bewältigen. Das Azure-Portal vereinfacht das Hinzufügen neuer Workerknoten und das Erhöhen der Anzahl der virtuellen Kerne vorhandener Knoten. Das Hinzufügen von Knoten verursacht keine Ausfallzeiten, und auch das Verschieben von Shards auf die neuen Knoten (das so genannte [Ausgleichen von Shards](#rebalance-shards)) erfolgt ohne Unterbrechung der Abfragen.

### <a name="add-worker-nodes"></a>Hinzufügen von Workerknoten

Wenn Sie Knoten hinzufügen möchten, navigieren Sie in Ihrer Servergruppe „Hyperscale (Citus)“ zur Registerkarte **Compute + Speicher**.  Ziehen Sie den Schieberegler für **Anzahl von Workerknoten**, um den Wert zu ändern.

:::image type="content" source="./media/howto-hyperscale-scaling/01-sliders-workers.png" alt-text="Schieberegler für Ressourcen":::

Klicken Sie auf die Schaltfläche **Speichern**, damit der geänderte Wert wirksam wird.

> [!NOTE]
> Nachdem Sie die Anzahl der Workerknoten erhöht und gespeichert haben, kann sie mit dem Schieberegler nicht mehr verringert werden.

#### <a name="rebalance-shards"></a>Ausgleichen von Shards

Um neu hinzugefügte Knoten optimal zu nutzen, müssen Sie die verteilte [Shard](concepts-hyperscale-distributed-data.md#shards)tabelle ausgleichen. Das bedeutet, dass einige Shards von vorhandenen Knoten auf neue Knoten verschoben werden. Vergewissern Sie sich zunächst, dass die Bereitstellung der neuen Worker erfolgreich abgeschlossen wurde. Starten Sie dann den Shardausgleich, indem Sie über „psql“ eine Verbindung mit dem Clusterkoordinatorknoten herstellen und Folgendes ausführen:

```sql
SELECT rebalance_table_shards('distributed_table_name');
```

Die `rebalance_table_shards`-Funktion gleicht alle Tabellen in der [Zusammenstellungs](concepts-hyperscale-colocation.md)gruppe der im Argument genannten Tabelle aus. Daher müssen Sie die Funktion nicht für jede verteilte Tabelle aufrufen, sondern einfach nur für eine repräsentative Tabelle aus jeder Zusammenstellungsgruppe.

### <a name="increase-or-decrease-vcores-on-nodes"></a>Erhöhen und Verringern der Anzahl von virtuellen Kernen auf Knoten

> [!NOTE]
> Diese Funktion steht derzeit als Vorschau zur Verfügung. Um eine Änderung der virtuellen Kerne für Knoten in der Servergruppe anzufordern, [wenden Sie sich an den Azure-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

Zusätzlich zum Hinzufügen neuer Knoten können Sie die Funktionen vorhandener Knoten erweitern. Das Anpassen der Computekapazität nach oben und unten kann für Leistungsexperimente und kurz- oder langfristige Änderungen der Datenverkehrsanforderungen nützlich sein.

Um die virtuellen Kerne für alle Workerknoten zu ändern, passen Sie den Schieberegler **Virtuelle Kerne** unter **Konfiguration (pro Workerknoten)** an. Die virtuellen Kerne des Koordinatorknotens können unabhängig voneinander angepasst werden. Schieben Sie den Schieberegler für **virtuelle Kerne** und **Konfiguration (Koordinatorknoten)** an die gewünschte Position.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Leistungsoptionen](concepts-hyperscale-configuration-options.md) für Servergruppen.
