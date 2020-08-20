---
title: Troubleshooting der Ausnahme „Azure Cosmos DB nicht verfügbar“
description: Diagnose und Fehlerbehebung bei der Ausnahme, dass der Cosmos DB-Dienst nicht verfügbar ist
author: j82w
ms.service: cosmos-db
ms.date: 08/06/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: b9c24ee3b94be86ccf9d27b928c42fc194800a3b
ms.sourcegitcommit: 25bb515efe62bfb8a8377293b56c3163f46122bf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87987374"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-service-unavailable"></a>Diagnose und Fehlerbehebung der Ausnahme „Azure Cosmos DB nicht verfügbar“
Das SDK war nicht in der Lage, eine Verbindung mit dem Azure Cosmos DB-Dienst herzustellen.

## <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung
Die folgende Liste enthält bekannte Gründe und Lösungen für Ausnahmen, dass ein Dienst nicht verfügbar ist.

### <a name="1-the-required-ports-are-being-blocked"></a>1. Die erforderlichen Ports werden blockiert
Vergewissern Sie sich, dass alle [erforderlichen Ports](performance-tips-dotnet-sdk-v3-sql.md#networking) aktiviert sind.

### <a name="2-client-side-transient-connectivity-issues"></a>2. Clientseitige vorübergehende Konnektivitätsprobleme
Ausnahmen vom Typ „Dienst nicht verfügbar“ können auftreten, wenn vorübergehende Konnektivitätsprobleme auftreten, die Timeouts verursachen. In der Regel enthält die Stapelüberwachung im Zusammenhang mit diesem Szenario ein `TransportException`-Element, z. B.:

```C#
TransportException: A client transport error occurred: The request timed out while waiting for a server response. 
(Time: xxx, activity ID: xxx, error code: ReceiveTimeout [0x0010], base error: HRESULT 0x80131500
```

Befolgen Sie die [Schritte zur Problembehandlung für Anforderungstimeouts](troubleshoot-dot-net-sdk-request-timeout.md#troubleshooting-steps), um das Problem zu beheben.

### <a name="3-service-outage"></a>3. Dienstunterbrechung
Überprüfen Sie den [Azure-Status](https://status.azure.com/status), um festzustellen, ob ein Problem vorliegt.


## <a name="next-steps"></a>Nächste Schritte
* [Diagnostizieren und Behandeln von Problemen bei Verwendung des .NET SDK für Azure Cosmos DB](troubleshoot-dot-net-sdk.md)
* Weitere Informationen zu Leistungsrichtlinien für [.NET-Version 3](performance-tips-dotnet-sdk-v3-sql.md) und [.NET-Version 2](performance-tips.md)