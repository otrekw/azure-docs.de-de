---
title: Hochverfügbarkeit – Hyperscale (Citus) – Azure Database for PostgreSQL
description: Konzepte für Hochverfügbarkeit und Notfallwiederherstellung
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 10679ab02826fb606af65c72621f2afb609bc81b
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74975532"
---
# <a name="high-availability-in-azure-database-for-postgresql--hyperscale-citus"></a>Hochverfügbarkeit in Azure Database for PostgreSQL – Hyperscale (Citus)

Hochverfügbarkeit (High Availability, HA) vermeidet Datenbankausfälle, indem Standbyreplikate jedes Knotens in einer Servergruppe verwaltet werden. Wenn ein Knoten ausfällt, schaltet Hyperscale eingehende Verbindungen vom fehlerhaften Knoten auf den Standbyknoten um. Ein Failover erfolgt innerhalb weniger Minuten, und höher gestufte Knoten verfügen durch die synchrone PostgreSQL-Streamingreplikation immer über aktuelle Daten.

Um die Vorteile von Hochverfügbarkeit auf dem Koordinatorknoten zu nutzen, müssen Datenbankanwendungen getrennte Verbindungen und fehlerhafte Transaktionen erkennen und Wiederholungsvorgänge für diese ausführen. Auf den höher gestuften neuen Koordinatorknoten kann mit der gleichen Verbindungszeichenfolge zugegriffen werden.

Die Wiederherstellung kann in drei Phasen unterteilt werden: Erkennung, Failover und vollständige Wiederherstellung.  Hyperscale führt regelmäßige Integritätsprüfungen für jeden Knoten aus, und nach vier fehlgeschlagenen Prüfungen wird festgelegt, dass ein Knoten ausgefallen ist. Hyperscale stuft dann einen Standbyknoten auf den primären Knotenstatus höher (Failover) und stellt einen neuen zukünftigen Standbyknoten bereit.
Die Streamingreplikation beginnt, sodass der neue Knoten auf den neuesten Stand gebracht wird.  Nachdem alle Daten repliziert wurden, hat der Knoten die vollständige Wiederherstellung erreicht.

### <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [Hochverfügbarkeit](howto-hyperscale-high-availability.md) in einer Hyperscale-Servergruppe aktivieren.
