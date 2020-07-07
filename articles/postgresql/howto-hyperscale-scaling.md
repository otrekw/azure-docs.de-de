---
title: Skalieren von Servergruppen – Hyperscale (Citus) – Azure Database for PostgreSQL
description: Anpassen von Servergruppenspeicher-, Datenträger- und CPU-Ressourcen, um höhere Auslastungen zu bewältigen
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 3/16/2020
ms.openlocfilehash: f8e8e1672f754e843a3bd1c75d496599d31e5f11
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "82583999"
---
# <a name="scale-a-hyperscale-citus-server-group"></a>Skalieren einer Hyperscale (Citus)-Servergruppe

Azure Database for PostgreSQL – Hyperscale (Citus) unterstützt Self-Service-Skalierung, um höhere Auslastungen zu bewältigen. Das Azure-Portal vereinfacht das Hinzufügen neuer Workerknoten und das Erhöhen der Anzahl der virtuellen Kerne vorhandener Knoten.

## <a name="add-worker-nodes"></a>Hinzufügen von Workerknoten

Navigieren Sie zum Hinzufügen von Knoten in der Servergruppe „Hyperscale (Citus)“ zur Registerkarte **Konfigurieren**.  Ziehen Sie den Schieberegler für **Anzahl von Workerknoten**, um den Wert zu ändern.

![Schieberegler für Ressourcen](./media/howto-hyperscale-scaling/01-sliders-workers.png)

Klicken Sie auf die Schaltfläche **Speichern**, damit der geänderte Wert wirksam wird.

> [!NOTE]
> Nachdem Sie die Anzahl der Workerknoten erhöht und gespeichert haben, kann sie mit dem Schieberegler nicht mehr verringert werden.

### <a name="rebalance-shards"></a>Ausgleichen von Shards

Um neu hinzugefügte Knoten optimal zu nutzen, müssen Sie die verteilte [Shard](concepts-hyperscale-distributed-data.md#shards)tabelle ausgleichen. Das bedeutet, dass einige Shards von vorhandenen Knoten auf neue Knoten verschoben werden. Vergewissern Sie sich zunächst, dass die Bereitstellung der neuen Worker erfolgreich abgeschlossen wurde. Starten Sie dann den Shardausgleich, indem Sie über „psql“ eine Verbindung mit dem Clusterkoordinatorknoten herstellen und Folgendes ausführen:

```sql
SELECT rebalance_table_shards('distributed_table_name');
```

Die `rebalance_table_shards`-Funktion gleicht alle Tabellen in der [Zusammenstellungs](concepts-hyperscale-colocation.md)gruppe der im Argument genannten Tabelle aus. Daher müssen Sie die Funktion nicht für jede verteilte Tabelle aufrufen, sondern einfach nur für eine repräsentative Tabelle aus jeder Zusammenstellungsgruppe.

## <a name="increase-or-decrease-vcores-on-nodes"></a>Erhöhen und Verringern der Anzahl von virtuellen Kernen auf Knoten

> [!NOTE]
> Diese Funktion steht derzeit als Vorschau zur Verfügung. Um eine Änderung der virtuellen Kerne für Knoten in der Servergruppe anzufordern, [wenden Sie sich an den Azure-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

Zusätzlich zum Hinzufügen neuer Knoten können Sie die Funktionen vorhandener Knoten erweitern. Das Anpassen der Computekapazität nach oben und unten kann für Leistungsexperimente und kurz- oder langfristige Änderungen der Datenverkehrsanforderungen nützlich sein.

Um die virtuellen Kerne für alle Workerknoten zu ändern, passen Sie den Schieberegler **Virtuelle Kerne** unter **Konfiguration (pro Workerknoten)** an. Die virtuellen Kerne des Koordinatorknotens können unabhängig voneinander angepasst werden. Klicken Sie unter **Koordinatorknoten** auf den Link **Konfiguration ändern**. Es wird ein Dialogfeld mit Schiebereglern für die virtuellen Kerne und die Speicherkapazität des Koordinators angezeigt. Ändern Sie die Schieberegler gemäß Ihren Anforderungen, und wählen Sie **OK** aus.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Leistungsoptionen](concepts-hyperscale-configuration-options.md) für Servergruppen.
