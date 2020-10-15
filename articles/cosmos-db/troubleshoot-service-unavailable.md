---
title: Troubleshooting der Ausnahme „Azure Cosmos DB nicht verfügbar“
description: Erfahren Sie mehr über das Diagnostizieren und Beheben von Ausnahmen, wenn der Azure Cosmos DB-Dienst nicht verfügbar ist.
author: j82w
ms.service: cosmos-db
ms.date: 08/06/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 749592b778612c6903c9c15e336de3fb00978199
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88870866"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-service-unavailable-exceptions"></a>Diagnose und Troubleshooting der Ausnahme „Azure Cosmos DB nicht verfügbar“
Das SDK war nicht in der Lage, eine Verbindung mit Azure Cosmos DB herzustellen.

## <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung
Die folgende Liste enthält bekannte Gründe und Lösungen für Ausnahmen, dass ein Dienst nicht verfügbar ist.

### <a name="the-required-ports-are-being-blocked"></a>Die erforderlichen Ports werden blockiert
Vergewissern Sie sich, dass alle [erforderlichen Ports](performance-tips-dotnet-sdk-v3-sql.md#networking) aktiviert sind.

### <a name="client-side-transient-connectivity-issues"></a>Clientseitige vorübergehende Konnektivitätsprobleme
Ausnahmen vom Typ „Dienst nicht verfügbar“ können auftreten, wenn vorübergehende Konnektivitätsprobleme auftreten, die Timeouts verursachen. In der Regel enthält die Stapelüberwachung im Zusammenhang mit diesem Szenario einen `TransportException`-Fehler. Beispiel:

```C#
TransportException: A client transport error occurred: The request timed out while waiting for a server response. 
(Time: xxx, activity ID: xxx, error code: ReceiveTimeout [0x0010], base error: HRESULT 0x80131500
```

Befolgen Sie die [Schritte zur Problembehandlung für Anforderungstimeouts](troubleshoot-dot-net-sdk-request-timeout.md#troubleshooting-steps), um das Problem zu beheben.

### <a name="service-outage"></a>Dienstunterbrechung
Überprüfen Sie den [Azure-Status](https://status.azure.com/status), um festzustellen, ob ein Problem vorliegt.


## <a name="next-steps"></a>Nächste Schritte
* [Diagnostizieren und Behandeln](troubleshoot-dot-net-sdk.md) von Problemen bei Verwendung des .NET SDK für Azure Cosmos DB
* Weitere Informationen zu Leistungsrichtlinien für [.NET Version 3](performance-tips-dotnet-sdk-v3-sql.md) und [.NET Version 2](performance-tips.md)