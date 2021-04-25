---
title: Skalieren von Servergruppen – Hyperscale (Citus) – Azure Database for PostgreSQL
description: Anpassen von Servergruppenspeicher-, Datenträger- und CPU-Ressourcen, um höhere Auslastungen zu bewältigen
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 04/07/2021
ms.openlocfilehash: 905224119b9df4e4003b1736443406e9548a49e3
ms.sourcegitcommit: 6ed3928efe4734513bad388737dd6d27c4c602fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "107012528"
---
# <a name="scale-a-hyperscale-citus-server-group"></a>Skalieren einer Hyperscale (Citus)-Servergruppe

Azure Database for PostgreSQL – Hyperscale (Citus) unterstützt Self-Service-Skalierung, um höhere Auslastungen zu bewältigen. Das Azure-Portal vereinfacht das Hinzufügen neuer Workerknoten und das Erhöhen der Anzahl der virtuellen Kerne vorhandener Knoten. Das Hinzufügen von Knoten verursacht keine Ausfallzeiten, und auch das Verschieben von Shards auf die neuen Knoten (das so genannte [Ausgleichen von Shards](howto-hyperscale-scale-rebalance.md)) erfolgt ohne Unterbrechung der Abfragen.

## <a name="add-worker-nodes"></a>Hinzufügen von Workerknoten

Wenn Sie Knoten hinzufügen möchten, navigieren Sie in Ihrer Servergruppe „Hyperscale (Citus)“ zur Registerkarte **Compute + Speicher**.  Ziehen Sie den Schieberegler für **Anzahl von Workerknoten**, um den Wert zu ändern.

> [!NOTE]
>
> Bei einer Hyperscale (Citus)-Servergruppe, die mit dem [Basic-Tarif (Vorschauversion)](concepts-hyperscale-tiers.md) erstellt wurde, gibt es keine Worker. Wenn Sie die Anzahl der Worker erhöhen, wird die Servergruppe automatisch auf den Standard-Tarif umgestuft.
> Nachdem Sie eine Servergruppe auf den Standard-Tarif umgestuft haben, können Sie sie nicht wieder auf den Basic-Tarif herabstufen.

:::image type="content" source="./media/howto-hyperscale-scaling/01-sliders-workers.png" alt-text="Schieberegler für Ressourcen":::

Klicken Sie auf die Schaltfläche **Speichern**, damit der geänderte Wert wirksam wird.

> [!NOTE]
> Nachdem Sie die Anzahl der Workerknoten erhöht und gespeichert haben, kann sie mit dem Schieberegler nicht mehr verringert werden.

> [!NOTE]
> Um neu hinzugefügte Knoten optimal zu nutzen, müssen Sie die verteilte [Shardtabelle ausgleichen](howto-hyperscale-scale-rebalance.md). Dabei werden einige [Shards](concepts-hyperscale-distributed-data.md#shards) von vorhandenen Knoten auf neue Knoten verschoben.

## <a name="increase-or-decrease-vcores-on-nodes"></a>Erhöhen und Verringern der Anzahl von virtuellen Kernen auf Knoten

Zusätzlich zum Hinzufügen neuer Knoten können Sie die Funktionen vorhandener Knoten erweitern. Das Anpassen der Computekapazität nach oben und unten kann bei Leistungsexperimenten und kurz- oder langfristigen Änderungen der Datenverkehrsanforderungen nützlich sein.

Um die virtuellen Kerne für alle Workerknoten zu ändern, passen Sie den Schieberegler **Virtuelle Kerne** unter **Konfiguration (pro Workerknoten)** an. Die virtuellen Kerne des Koordinatorknotens können unabhängig voneinander angepasst werden. Schieben Sie den Schieberegler für **virtuelle Kerne** und **Konfiguration (Koordinatorknoten)** an die gewünschte Position.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Leistungsoptionen](concepts-hyperscale-configuration-options.md) für Servergruppen.
- [Ausgleichen verteilter Tabellenshards](howto-hyperscale-scale-rebalance.md), damit alle Workerknoten an parallelen Abfragen teilnehmen können
