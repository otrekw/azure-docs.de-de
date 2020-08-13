---
title: Behandeln von Problemen im Zusammenhang mit zu großem Anforderungsheader oder mit ungültigen Anforderungen (400) in Azure Cosmos DB
description: Hier erfahren Sie, wie Sie Ausnahmen diagnostizieren und beheben, die auf einen zu großen Anforderungsheader zurückzuführen sind.
author: j82w
ms.service: cosmos-db
ms.date: 07/13/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 942af52c43a1d4526a42f20df40abf3f510e0ee6
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87293817"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-request-header-too-large-message"></a>Diagnostizieren und Behandeln von Problemen im Zusammenhang mit der Azure Cosmos DB-Meldung, dass der Anforderungsheader zu groß ist
Die Meldung, dass der Anforderungsheader zu groß ist, wird mit dem HTTP-Fehlercode 400 ausgelöst. Dieser Fehler tritt auf, wenn der Anforderungsheader zu groß geworden ist und die maximal zulässige Größe übersteigt. Es wird empfohlen, die neueste Version des SDK zu verwenden. Verwenden Sie mindestens die Version 3.x oder 2.x, da der Ausnahmemeldung in diesen Versionen eine Verfolgung der Headergröße hinzugefügt wird.

## <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung
Die Meldung, dass der Anforderungsheader zu groß ist, tritt auf, wenn das Sitzungs- oder Fortsetzungstoken zu groß ist. In den folgenden Abschnitten werden jeweils die Ursache und die Lösung des Problems in den einzelnen Kategorien beschrieben:

### <a name="1-session-token-too-large"></a>1. Sitzungstoken zu groß

#### <a name="cause"></a>Ursache:
Die ungültige Anforderung (400) ist wahrscheinlich auf ein zu großes Sitzungstoken zurückzuführen. Wenn die folgenden Aussagen zutreffend sind, ist das Sitzungstoken zu groß:

* Der Fehler tritt bei Punktvorgängen ohne Fortsetzungstoken auf (beispielsweise beim Erstellen, Lesen, Aktualisieren oder Ähnlichem).
* Die Ausnahme tritt auf, ohne dass Änderungen an der Anwendung vorgenommen wurden. Die Größe des Sitzungstokens nimmt mit der steigenden Anzahl von Partitionen im Container zu. Die Anzahl von Partitionen erhöht sich, wenn die Datenmenge zunimmt oder der Durchsatz erhöht wird.

#### <a name="temporary-mitigation"></a>Vorübergehende Lösung: 
Starten Sie Ihre Clientanwendung neu, um alle Sitzungstoken zurückzusetzen. Das Sitzungstoken erreicht irgendwann wieder die Größe, durch die das Problem verursacht wurde. Verwenden Sie daher die Lösung im nächsten Abschnitt, um dieses Problem vollständig zu vermeiden.

#### <a name="solution"></a>Lösung:
1. Folgen Sie der Anleitung im Artikel mit Leistungstipps für [.NET V3](performance-tips-dotnet-sdk-v3-sql.md) oder [.NET V2](performance-tips.md), und konvertieren Sie die Anwendung, sodass sie den Direktverbindungsmodus mit TCP verwendet. Im direkten Modus mit TCP besteht keine Headergrößenbeschränkung wie bei HTTP, sodass dieses Problem vermieden wird. Verwenden Sie die neueste Version des SDK. Diese enthält eine Korrektur für Abfragevorgänge, wenn die Dienstinteroperabilität nicht verfügbar ist.
2. Sollte der Direktverbindungsmodus mit TCP für Ihre Workload nicht in Frage kommen, ändern Sie die [Konsistenzebene des Clients](how-to-manage-consistency.md). Das Sitzungstoken wird nur für die Sitzungskonsistenz verwendet. Dies ist die Standardkonsistenzebene für Azure Cosmos DB. Von anderen Konsistenzebenen wird das Sitzungstoken nicht verwendet.

### <a name="2-continuation-token-too-large"></a>2. Fortsetzungstoken zu groß

#### <a name="cause"></a>Ursache:
Die ungültige Anforderung (400) tritt bei Abfragevorgängen auf, bei denen das Fortsetzungstoken verwendet wird und Folgendes zutrifft: Das Fortsetzungstoken ist zu groß geworden, oder verschiedene Abfragen weisen unterschiedlich große Fortsetzungstoken auf.
    
#### <a name="solution"></a>Lösung:
1. Folgen Sie der Anleitung im Artikel mit Leistungstipps für [.NET V3](performance-tips-dotnet-sdk-v3-sql.md) oder [.NET V2](performance-tips.md), und konvertieren Sie die Anwendung, sodass sie den Direktverbindungsmodus mit TCP verwendet. Im direkten Modus mit TCP besteht keine Headergrößenbeschränkung wie bei HTTP, sodass dieses Problem vermieden wird. 
3. Sollte der Direktverbindungsmodus mit TCP für Ihre Workload nicht in Frage kommen, legen Sie die Option `ResponseContinuationTokenLimitInKb` fest. Diese Option befindet sich unter `FeedOptions` (v2) bzw. unter `QueryRequestOptions` (v3).

## <a name="next-steps"></a>Nächste Schritte
* [Diagnostizieren und Behandeln von Problemen bei Verwendung des .NET SDK für Azure Cosmos DB](troubleshoot-dot-net-sdk.md)
* Weitere Informationen zu Leistungsrichtlinien für [.NET-Version 3](performance-tips-dotnet-sdk-v3-sql.md) und [.NET-Version 2](performance-tips.md)
