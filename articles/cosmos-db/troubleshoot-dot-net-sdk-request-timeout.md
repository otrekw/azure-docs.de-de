---
title: Troubleshooting für HTTP-408- oder Anforderungstimeoutfehler in Azure Cosmos DB mithilfe von .NET SDK
description: Diagnose und Fehlerbehebung bei Timeoutausnahmen bei Anforderungen im .NET SDK
author: j82w
ms.service: cosmos-db
ms.date: 07/13/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 29b0c6237ae04ea5da9ec496498fc7c20890b173
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87293809"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-net-sdk-request-timeout"></a>Diagnose und Troubleshooting für .NET SDK-Anforderungstimeouts in Azure Cosmos DB
Der HTTP-408-Fehler tritt auf, wenn das SDK die Anforderung nicht abschließen konnte, bevor das Timeoutlimit überschritten wird.

## <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung
Die folgende Liste enthält bekannte Gründe und Lösungen für Anforderungstimeoutausnahmen.

### <a name="1-high-cpu-utilization-most-common-case"></a>1. Hohe CPU-Auslastung (häufigster Fall)
Für eine ideale Wartezeit sollte die CPU-Nutzung etwa 40 Prozent betragen. Es wird empfohlen, zehn Sekunden als Intervall zu nutzen, um die maximale (nicht die durchschnittliche) CPU-Nutzung zu überwachen. CPU-Spitzenwerte treten bei partitionsübergreifenden Abfragen häufig auf, wenn es möglicherweise erforderlich ist, mehrere Verbindungen für eine einzelne Abfrage herzustellen.

#### <a name="solution"></a>Lösung:
Die Clientanwendung, die das SDK verwendet, sollte entsprechend skaliert werden.

### <a name="2-socket--port-availability-might-be-low"></a>2. Die Verfügbarkeit von Sockets und Ports kann niedrig sein
Bei Ausführung in Azure kann es auf Clients, die das .NET SDK verwenden, zur Azure SNAT-Portauslastung (PAT) kommen.

#### <a name="solution-1"></a>Lösung 1:
Befolgen Sie die Anleitung im Artikel [Azure SNAT-Portauslastung (PAT)](troubleshoot-dot-net-sdk.md#snat).

#### <a name="solution-2"></a>Lösung 2:
Wenn Sie einen HTTP-Proxy verwenden, vergewissern Sie sich, dass er die Anzahl von Verbindungen unterstützt, die in `ConnectionPolicy` des SDK konfiguriert ist.
Andernfalls treten Verbindungsprobleme auf.

### <a name="3-creating-multiple-client-instances"></a>3. Erstellen mehrerer Clientinstanzen
Das Erstellen mehrerer Clientinstanzen kann zu Verbindungskonflikten und Timeoutproblemen führen.

#### <a name="solution"></a>Lösung:
Befolgen Sie die [Leistungstipps](performance-tips-dotnet-sdk-v3-sql.md#sdk-usage), und verwenden Sie eine einzige CosmosClient-Instanz für den gesamten Prozess.

### <a name="4-hot-partition-key"></a>4. Partitionsschlüssel auf heißer Ebene
Azure Cosmos DB verteilt den gesamten bereitgestellten Durchsatz gleichmäßig auf die physischen Partitionen. Wenn es eine Partition auf heißer Ebene gibt, benötigt mindestens ein logischer Partitionsschlüssel auf einer physischen Partition alle Anforderungseinheiten pro Sekunde der physischen Partition. Die Anforderungseinheiten pro Sekunde aller anderen physischen Partitionen werden nicht genutzt. Die insgesamt genutzten Anforderungseinheiten pro Sekunde sind weniger als die insgesamt bereitgestellten Anforderungseinheiten pro Sekunde in der Datenbank oder im Container. Anforderungen für den logischen Partitionsschlüssel auf heißer Ebene werden jedoch weiterhin gedrosselt (429s). Im Artikel [Überwachen von normalisierten RU/s für einen Azure Cosmos-Container oder ein -Konto](monitor-normalized-request-units.md) finden Sie weitere Informationen, wie Sie erfahren, ob die Workload Probleme mit der Partition auf heißer Ebene hat. 

#### <a name="solution"></a>Lösung:
Wählen Sie einen geeigneten Partitionsschlüssel aus, der Anforderungsvolume und -speicher gleichmäßig verteilt. Weitere Informationen finden Sie unter [Ändern des Partitionsschlüssels in Azure Cosmos DB](https://devblogs.microsoft.com/cosmosdb/how-to-change-your-partition-key/).

### <a name="5-high-degree-of-concurrency"></a>5. Hoher Parallelitätsgrad
Die Anwendung weist einen hohen Parallelitätsgrad auf, was zu Konflikten im Kanal führen kann.

#### <a name="solution"></a>Lösung:
Die Clientanwendung, die das SDK verwendet, sollte entsprechend skaliert werden.

### <a name="6-large-requests-andor-responses"></a>6. Umfangreiche Anforderungen und/oder Antworten
Große Anforderungen oder Antworten können zu Head-of-Line-Blockierungen im Kanal führen und die Konfliktsituation verschärfen, selbst bei einem relativ geringen Grad an Parallelität.

#### <a name="solution"></a>Lösung:
Die Clientanwendung, die das SDK verwendet, sollte entsprechend skaliert werden.

### <a name="7-failure-rate-is-within-cosmos-db-sla"></a>7. Die Fehlerrate ist von der SLA in Cosmos DB abgedeckt
Die Anwendung sollte vorübergehende Fehler verarbeiten können und bei Bedarf wiederholte Versuche ausführen. Bei 408-Ausnahmen wird kein wiederholter Versuch ausgeführt, da es bei Erstellpfaden nicht möglich ist, zu wissen, ob der Dienst das Element erstellt hat oder nicht. Wenn dasselbe Element noch mal für einen Erstellvorgang gesendet wird, führt dies zu einer Konfliktausnahme. In der Geschäftslogik von Benutzeranwendungen ist möglicherweise eine Logik enthalten, um Konflikte zu verarbeiten. Dabei würde es aufgrund der Ambiguität eines vorhandenen Elements und dem Konflikt eines wiederholten Erstellversuchs jedoch zu einem Fehler kommen.

### <a name="8-failure-rate-is-violating-the-cosmos-db-sla"></a>8. Die Fehlerrate verletzt die Cosmos DB-SLA
Wenden Sie sich an den Azure-Support.

## <a name="next-steps"></a>Nächste Schritte
* [Diagnostizieren und Behandeln von Problemen bei Verwendung des .NET SDK für Azure Cosmos DB](troubleshoot-dot-net-sdk.md)
* Weitere Informationen zu Leistungsrichtlinien für [.NET-Version 3](performance-tips-dotnet-sdk-v3-sql.md) und [.NET-Version 2](performance-tips.md)