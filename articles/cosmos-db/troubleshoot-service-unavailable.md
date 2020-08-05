---
title: Troubleshooting der Ausnahme „Azure Cosmos DB nicht verfügbar“
description: Diagnose und Fehlerbehebung bei der Ausnahme, dass der Cosmos DB-Dienst nicht verfügbar ist
author: j82w
ms.service: cosmos-db
ms.date: 07/13/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 12ecec7cf8f406ed53fb5e054fc304bf672cbbb0
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87293782"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-service-unavailable"></a>Diagnose und Fehlerbehebung der Ausnahme „Azure Cosmos DB nicht verfügbar“
Das SDK war nicht in der Lage, eine Verbindung mit dem Azure Cosmos DB-Dienst herzustellen.

## <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung
Die folgende Liste enthält bekannte Gründe und Lösungen für Ausnahmen, dass ein Dienst nicht verfügbar ist.

### <a name="1-the-required-ports-are-not-enabled"></a>1. Die erforderlichen Ports sind nicht aktiviert.
Vergewissern Sie sich, dass alle [erforderlichen Ports](performance-tips-dotnet-sdk-v3-sql.md#networking) aktiviert sind.

## <a name="if-an-existing-application-or-service-started-getting-503"></a>Eine vorhandene Anwendung oder ein Dienst löst den Fehler 503 aus

### <a name="1-there-is-an-outage"></a>1. Ein Ausfall ist aufgetreten
Überprüfen Sie den [Azure-Status](https://status.azure.com/status), um festzustellen, ob ein Problem vorliegt.

### <a name="2-snat-port-exhaustion"></a>2. SNAT-Portauslastung
Befolgen Sie den [Leitfaden der SNAT-Portauslastung](troubleshoot-dot-net-sdk.md#snat).

### <a name="3-the-required-ports-are-being-blocked"></a>3. Die erforderlichen Ports werden blockiert
Vergewissern Sie sich, dass alle [erforderlichen Ports](performance-tips-dotnet-sdk-v3-sql.md#networking) aktiviert sind.

## <a name="next-steps"></a>Nächste Schritte
* [Diagnostizieren und Behandeln von Problemen bei Verwendung des .NET SDK für Azure Cosmos DB](troubleshoot-dot-net-sdk.md)
* Weitere Informationen zu Leistungsrichtlinien für [.NET-Version 3](performance-tips-dotnet-sdk-v3-sql.md) und [.NET-Version 2](performance-tips.md)