---
title: Problembehandlung für die Azure Cosmos DB-Anforderungsrate bei einer großen Ausnahme
description: Diagnostizieren und Beheben von Problemen mit der Anforderungsrate bei einer großen Ausnahme
author: j82w
ms.service: cosmos-db
ms.date: 07/13/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: b20ee2e10648d2d8eb248fc3f98c4fb33805f914
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87293783"
---
# <a name="diagnose-and-troubleshoot-cosmos-db-too-many-requests"></a>Diagnostizieren und Beheben von Problemen bei zu vielen Cosmos DB-Anforderungen
Die Fehlermeldung ''Request rate too large'' (Anforderungsrate zu groß) oder der Fehlercode 429 deuten darauf hin, dass Ihre Anforderungen gedrosselt werden.

## <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung
Die folgende Liste enthält bekannte Gründe und Lösungen für zu viele Anforderungen.

### <a name="1-check-the-metrics"></a>1. Überprüfen der Metriken
Kunden sollten über die [Überwachungsfunktionen in Azure Cosmos DB](monitor-cosmos-db.md) überprüfen, ob 429-Ausnahmen vorliegen.

## <a name="cause"></a>Ursache:
Der verbrauchte Durchsatz (RU/s) liegt über dem [bereitgestellten Durchsatz](set-throughput.md). Das SDK wiederholt Anforderungen basierend auf der angegebenen Wiederholungsrichtlinie automatisch. Wenn dieser Fehler häufig auftritt, erwägen Sie das Erhöhen des Durchsatzes für die Sammlung. Überprüfen Sie die Metriken im Portal auf 429-Fehler. Überprüfen Sie Ihren Partitionsschlüssel, um sicherzustellen, dass eine [gleichmäßige Verteilung von Speicher- und Anforderungsvolumen](partition-data.md) gewährleistet ist.

## <a name="solution"></a>Lösung:
1. Verwenden Sie das [Portal oder das SDK](set-throughput.md), um den bereitgestellten Durchsatz zu erhöhen.
2. Wählen Sie für die Datenbank oder den Container die Option [Autoskalierung](provision-throughput-autoscale.md) aus.

## <a name="next-steps"></a>Nächste Schritte
* [Diagnostizieren und Behandeln](troubleshoot-dot-net-sdk.md) von Problemen bei Verwendung des .NET SDK für Azure Cosmos DB
* Weitere Informationen zu Leistungsrichtlinien für [.NET-Version 3](performance-tips-dotnet-sdk-v3-sql.md) und [.NET-Version 2](performance-tips.md)