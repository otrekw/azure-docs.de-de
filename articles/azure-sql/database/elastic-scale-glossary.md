---
title: Tools für elastische Datenbanken – Glossar
description: Erläuterung von Begriffen, die für die Tools für flexible Datenbanken verwendet werden
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 12/04/2018
ms.openlocfilehash: 166e365a4ead8ad6d0f7e543c081161ebff5f027
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92330460"
---
# <a name="elastic-database-tools-glossary"></a>Tools für elastische Datenbanken – Glossar
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Die folgenden Begriffe werden für die [Tools für elastische Datenbanken](elastic-scale-introduction.md) definiert. Die Tools werden zur Verwaltung von [Shardzuordnungen](elastic-scale-shard-map-management.md) verwendet. Sie beinhalten die [Clientbibliothek](elastic-database-client-library.md), das [Split-Merge-Tool](elastic-scale-overview-split-and-merge.md), [Pools für elastische Datenbanken](elastic-pool-overview.md) und [Abfragen](elastic-query-overview.md). 

Diese Begriffe werden in [Hinzufügen eines Shards mithilfe der Tools für elastische Datenbanken](elastic-scale-add-a-shard.md) und [Beheben von Problemen bei der Shardzuordnung mithilfe der RecoveryManager-Klasse](elastic-database-recovery-manager.md) verwendet.

![Begriffe zur elastischen Skalierung][1]

**Datenbank**: Eine Datenbank in Azure SQL-Datenbank 

**Datenabhängiges Routing**: Die Funktion, die es einer Anwendung ermöglicht, unter Verwendung eines bestimmten Shardschlüssels eine Verbindung mit einem Shard herzustellen. Siehe [Datenabhängiges Routing](elastic-scale-data-dependent-routing.md). Vgl. **[Abfragen mehrerer Shards](elastic-scale-multishard-querying.md)** .

**Globale Shardzuordnung**: Die Zuordnung zwischen Shardschlüsseln und den zugehörigen Shards in einer **Shardgruppe**. Die globale Shardzuordnung wird im **Shardzuordnungs-Manager** gespeichert. Vgl. **lokale Shardzuordnung**.

**Listenshardzuordnung**: Eine Shardzuordnung, in der die Shardschlüssel einzeln zugeordnet sind. Vgl. **Bereichsshardzuordnung**.   

**Lokale Shardzuordnung**: Wird in einem Shard gespeichert und enthält die Zuordnungen für die Shardlets, die sich in diesem Shard befinden.

**Abfragen von mehreren Shards**: Die Möglichkeit, eine Abfrage für mehrere Shards durchzuführen. Die Resultsets werden über die UNION ALL-Semantik zurückgegeben (auch als „Auffächerungsabfrage“ bezeichnet). Vgl. **Datenabhängiges Routing**.

**Mehrinstanzenfähig** und **Einzelmandant**: Die folgende Abbildung zeigt eine Einzelmandantendatenbank und eine mehrinstanzenfähige Datenbank:

![Screenshot einer Einzelmandantendatenbank und einer mehrinstanzenfähige Datenbank.](./media/elastic-scale-glossary/multi-single-simple.png)

Hier ist eine Darstellung von Einzelmandanten- und mehrinstanzenfähigen **Sharddatenbanken** . 

![Einzelmandanten- und mehrinstanzenfähige Datenbanken](./media/elastic-scale-glossary/shards-single-multi.png)

**Bereichsshardzuordnung**: Eine Shardzuordnung, in der die Strategie der Shardverteilung auf mehreren Bereichen zusammenhängender Werte basiert. 

**Verweistabellen**: Tabellen, die nicht partitioniert sind, aber über mehrere Shards hinweg repliziert werden. Postleitzahlen können z. B. in einer Verweistabelle gespeichert werden. 

**Shard**: Eine Datenbank in Azure SQL-Datenbank, in der die Daten aus einem horizontal partitionierten Dataset gespeichert werden. 

**Shardelastizität**: Die Fähigkeit, sowohl **horizontal** als auch **vertikal** zu skalieren.

**Shardtabellen**: Tabellen, die horizontal partitioniert sind, deren Daten also anhand ihrer Shardschlüsselwerte auf Shards verteilt werden. 

**Shardschlüssel**: Ein Spaltenwert, der bestimmt, wie Daten auf Shards verteilt werden. Folgende Werttypen sind zulässig: **int**, **bigint**, **varbinary** oder **uniqueidentifier**. 

**Shardgruppe**: Die Sammlung von Shards, die zur gleichen Shardzuordnung im Shardzuordnungs-Manager gehören.  

**Shardlet**: Alle Daten, die mit einem einzelnen Wert eines Shardschlüssels in einem Shard verknüpft sind. Ein Shardlet ist die kleinste Einheit der Datenverschiebung, die bei der erneuten Verteilung von Shard-Tabellen möglich ist. 

**Shardzuordnung**: Der Satz von Zuordnungen zwischen Shardschlüsseln und den zugehörigen Shards.

**Shardzuordnungs-Manager**: Ein Verwaltungsobjekt und Datenspeicher mit den Shardzuordnungen, Shardspeicherorten und Zuordnungen für eine oder mehrere Shardgruppen.

![Das Diagramm zeigt einen Shard-Zuordnungs-Manager, der shardmaps_global, shards_global und shard_mappings_global zugeordnet ist.][2]

## <a name="verbs"></a>Verben
**Horizontale Skalierung**: Das Hoch- oder Herunterskalieren einer Sammlung von Shards durch Hinzufügen oder Entfernen von Shards zu bzw. aus einer Shardzuordnung, wie unten dargestellt.

![Horizontale und vertikale Skalierung][3]

**Zusammenführen**: Das Verschieben von Shardlets aus zwei Shards in einen Shard und das entsprechende Aktualisieren der Shardzuordnung.

**Shardletverschiebung**: Das Verschieben eines einzelnen Shardlets in einen anderen Shard. 

**Shard**: Das horizontale Partitionieren identisch strukturierter Daten über mehrere Datenbanken hinweg anhand eines Shardschlüssels.

**Teilen**: Das Verschieben mehrerer Shardlets aus einem Shard in einen anderen (in der Regel neuen) Shard. Als Aufteilungspunkt wird ein Sharding-Schlüssel vom Benutzer bereitgestellt.

**Vertikale Skalierung**: Das Herauf- oder Herunterskalieren der Computegröße eines einzelnen Shards. Dies erfolgt z. B. durch Ändern eines Shards von Standard in Premium (wodurch mehr Computerressourcen zur Verfügung stehen). 

[!INCLUDE [elastic-scale-include](../../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/elastic-scale-glossary/glossary.png
[2]: ./media/elastic-scale-glossary/mappings.png
[3]: ./media/elastic-scale-glossary/h_versus_vert.png

