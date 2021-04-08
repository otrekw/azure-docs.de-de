---
title: Apache Cassandra- und Azure Cosmos DB-Konsistenzebenen
description: Hier werden die Apache Cassandra- und Azure Cosmos DB-Konsistenzebenen erläutert.
author: TheovanKraay
ms.author: thvankra
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: conceptual
ms.date: 10/12/2020
ms.reviewer: sngun
ms.openlocfilehash: 3107610215d5b37c43124ce4129b2eb5437e3b62
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97516862"
---
# <a name="apache-cassandra-and-azure-cosmos-db-cassandra-api-consistency-levels"></a>Konsistenzebenen von Apache Cassandra und der Cassandra-API von Azure Cosmos DB
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

Im Gegensatz zu Azure Cosmos DB bietet Apache Cassandra nativ keine genau definierten Konsistenzgarantien. Stattdessen bietet Apache Cassandra eine Schreib- und Lesekonsistenzebene, um in den Genuss von Vorteilen in Bezug auf Hochverfügbarkeit, Konsistenz und Latenz zu kommen. Bei Verwenden der Cassandra-API von Azure Cosmos DB gilt Folgendes:

* Die Schreibkonsistenzebene von Apache Cassandra wird der Standardkonsistenzebene zugeordnet, die in Ihrem Azure Cosmos-Konto konfiguriert ist. Die Konsistenz für einen Schreibvorgang (CL) kann nicht anforderungsweise geändert werden.

* Azure Cosmos DB ordnet die vom Cassandra-Clienttreiber angegebene Lesekonsistenzebene dynamisch einer der Azure Cosmos DB-Konsistenzebenen zu, die bei einer Leseanforderung dynamisch konfiguriert wird.

## <a name="multi-region-writes-vs-single-region-writes"></a>Schreibvorgänge in mehreren Regionen und Schreibvorgänge in einer einzelnen Region

Die Apache Cassandra-Datenbank ist standardmäßig ein Multimaster-System und bietet keine Standardoption für Schreibvorgänge in einer einzelnen Region mit Replikation für Lesevorgänge in mehreren Regionen. Azure Cosmos DB bietet jedoch die sofort einsatzbereite Möglichkeit, Schreibkonfigurationen für eine einzelne Region oder [mehrere Regionen](how-to-multi-master.md) zu haben. Einer der Vorteile bei der Möglichkeit zur Auswahl einer Schreibkonfiguration für eine einzelne Region in mehreren Regionen ist die Vermeidung regionsübergreifender Konfliktszenarien und die Option, eine starke Konsistenz in mehreren Regionen zu wahren. 

Bei Schreibvorgängen in einer einzelnen Region können Sie eine starke Konsistenz wahren und gleichzeitig eine regionsübergreifende hohe Verfügbarkeit mit [automatischem Failover](high-availability.md#multi-region-accounts-with-a-single-write-region-write-region-outage) weiterhin aufrechterhalten. In dieser Konfiguration können Sie die Datenlokalität weiterhin zur Verringerung der Leselatenz ausnutzen, indem Sie anforderungsweise auf letztliche Konsistenz herabstufen. Zusätzlich zu diesen Funktionen bietet die Azure Cosmos DB-Plattform auch die Möglichkeit zum Aktivieren der [Zonenredundanz](high-availability.md#availability-zone-support) bei der Auswahl einer Region. Im Gegensatz zu nativem Apache Cassandra ermöglicht es Ihnen Azure Cosmos DB daher, im [Kompromissspektrum](consistency-levels.md#rto) des CAP-Theorems mit einer höheren Granularität zu navigieren.

## <a name="mapping-consistency-levels"></a>Zuordnen von Konsistenzebenen

Die Azure Cosmos DB-Plattform stellt eine Gruppe von fünf klar definierten, an Anwendungsfällen für Unternehmen orientierten Konsistenzeinstellungen in Bezug auf Replikation sowie die durch das [CAP-Theorem](https://en.wikipedia.org/wiki/CAP_theorem) und das [PACLC-Theorem](https://en.wikipedia.org/wiki/PACELC_theorem) definierten Kompromisse bereit. Da sich dieser Ansatz erheblich von Apache Cassandra unterscheidet, sollten Sie sich Zeit nehmen, sich in unserer [Dokumentation](consistency-levels.md) über Azure Cosmos DB-Konsistenzeinstellungen zu informieren und sie zu verstehen. Oder sehen Sie sich diese kurze [Videoanleitung](https://www.youtube.com/watch?v=t1--kZjrG-o) an, damit Sie Konsistenzeinstellungen in der Azure Cosmos DB-Plattform verstehen.

In der folgenden Tabelle werden die möglichen Zuordnungen zwischen Apache Cassandra- und Azure Cosmos DB-Konsistenzebenen bei Verwendung der Cassandra-API veranschaulicht. Sie zeigt Konfigurationen für eine einzelne Region, Lesevorgänge in mehreren Regionen mit Schreibvorgängen in einer einzelnen Region und Schreibvorgänge in mehreren Regionen.

> [!NOTE]
> Dies sind keine exakten Zuordnungen. Stattdessen haben wir Apache Cassandra die naheliegendsten Äquivalente zur Verfügung gestellt und qualitative Unterschiede in der Spalte ganz rechts vermieden. Wie oben erwähnt, empfehlen wir, dass Sie sich über [Konsistenzeinstellungen](consistency-levels.md) für Azure Cosmos DB informieren. 

:::image type="content" source="./media/cassandra-consistency/account.png" alt-text="Cassandra-Konsistenz – Zuordnung auf Kontoebene" lightbox="./media/cassandra-consistency/account.png" :::

:::image type="content" source="./media/cassandra-consistency/dynamic.png" alt-text="Cassandra-Konsistenz – dynamische Zuordnung" lightbox="./media/cassandra-consistency/dynamic.png" :::

Wenn Ihr Azure Cosmos-Konto mit einer anderen Konsistenzebene als der starken Konsistenz konfiguriert ist, können Sie anhand der *PBS*-Metrik (Probabilistically Bounded Staleness) die Wahrscheinlichkeit ermitteln, dass Ihre Clients starke und konsistente Lesevorgänge für Ihre Workloads erzielen. Diese Metrik wird im Azure-Portal verfügbar gemacht. Weitere Informationen finden Sie unter [Überwachen der PBS-Metrik (Probabilistically Bounded Staleness)](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric).

PBS (Probabilistic Bounded Staleness) zeigt, wie letztendlich Ihre letztendliche Konsistenz ist. Diese Metrik gibt Aufschluss darüber, wie oft Sie eine stärkere Konsistenz erreichen können als die Konsistenzebene, die Sie aktuell in Ihrem Azure Cosmos-Konto konfiguriert haben. Das heißt, Sie sehen die Wahrscheinlichkeit (gemessen in Millisekunden), dass stark konsistente Lesevorgänge in Regionen mit Schreib- und Lesevorgängen auftreten.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich weiter über die globale Verteilung und Konsistenzebenen in Azure Cosmos DB:

* [Übersicht über die globale Verteilung](distribute-data-globally.md)
* [Was sind Konsistenzebenen in Azure Cosmos DB?](consistency-levels.md)
* [Hochverfügbarkeit](high-availability.md)
