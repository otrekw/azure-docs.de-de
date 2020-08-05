---
title: Troubleshooting der Timeoutausnahme bei Anforderungen in Azure Cosmos DB
description: Diagnose und Fehlerbehebung bei Timeoutausnahmen bei Anforderungen in Azure Cosmos DB
author: j82w
ms.service: cosmos-db
ms.date: 07/13/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: bd8102ee73721e2a56185709d1076e489ecc7607
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87293784"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-request-timeout"></a>Diagnose und Troubleshooting für Anforderungstimeouts in Azure Cosmos DB
In Azure Cosmos DB wurde ein HTTP-408-Anforderungstimeout zurückgegeben.

## <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung
Die folgende Liste enthält bekannte Gründe und Lösungen für Anforderungstimeoutausnahmen.

### <a name="1-check-the-sla"></a>1. Überprüfen des SLA
Kunden sollten über die [Überwachungsfunktionen in Azure Cosmos DB](monitor-cosmos-db.md) überprüfen, ob 408-Ausnahmen die SLA für Cosmos DB verletzen.

#### <a name="solution-1-it-did-not-violate-the-cosmos-db-sla"></a>Lösung 1: Die Cosmos DB-SLA wurde nicht verletzt
Die Anwendung sollte mit diesem Szenario umgehen können und für diese vorübergehenden Fehler jeweils wiederholte Versuche durchführen.

#### <a name="solution-2-it-did-violate-the-cosmos-db-sla"></a>Lösung 2: Die Cosmos DB-SLA wurde verletzt
Wenden Sie sich an den Azure-Support: https://aka.ms/azure-support.
 
### <a name="2-hot-partition-key"></a>2. Partitionsschlüssel auf heißer Ebene
Azure Cosmos DB verteilt den gesamten bereitgestellten Durchsatz gleichmäßig auf die physischen Partitionen. Wenn es eine Partition auf heißer Ebene gibt, benötigt mindestens ein logischer Partitionsschlüssel auf einer physischen Partition alle Anforderungseinheiten pro Sekunde der physischen Partition. Die Anforderungseinheiten pro Sekunde aller anderen physischen Partitionen werden nicht genutzt. Die insgesamt genutzten Anforderungseinheiten pro Sekunde sind weniger als die insgesamt bereitgestellten Anforderungseinheiten pro Sekunde in der Datenbank oder im Container. Anforderungen für den logischen Partitionsschlüssel auf heißer Ebene werden jedoch weiterhin gedrosselt (429s). Im Artikel [Überwachen von normalisierten RU/s für einen Azure Cosmos-Container oder ein -Konto](monitor-normalized-request-units.md) finden Sie weitere Informationen, wie Sie erfahren, ob die Workload Probleme mit der Partition auf heißer Ebene hat. 

#### <a name="solution"></a>Lösung:
Wählen Sie einen geeigneten Partitionsschlüssel aus, der Anforderungsvolume und -speicher gleichmäßig verteilt. Weitere Informationen finden Sie unter [Ändern des Partitionsschlüssels in Azure Cosmos DB](https://devblogs.microsoft.com/cosmosdb/how-to-change-your-partition-key/).

## <a name="next-steps"></a>Nächste Schritte
* [Diagnostizieren und Behandeln von Problemen bei Verwendung des .NET SDK für Azure Cosmos DB](troubleshoot-dot-net-sdk.md)
* Weitere Informationen zu Leistungsrichtlinien für [.NET-Version 3](performance-tips-dotnet-sdk-v3-sql.md) und [.NET-Version 2](performance-tips.md)