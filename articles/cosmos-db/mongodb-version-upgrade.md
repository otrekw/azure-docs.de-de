---
title: Aktualisieren der Mongo-Version Ihrer Azure Cosmos DB-API für das MongoDB-Konto
description: Informationen, wie Sie die MongoDB Wire Protocol-Version für Ihre vorhandene Azure Cosmos DB-API für MongoDB-Konten nahtlos aktualisieren können
author: christopheranderson
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: how-to
ms.date: 03/02/2021
ms.author: chrande
ms.openlocfilehash: 1818838a68c2712336a3515b2a82b5fdd518d237
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101661170"
---
# <a name="upgrade-the-api-version-of-your-azure-cosmos-db-api-for-mongodb-account"></a>Upgrade der API-Version Ihrer Azure Cosmos DB-API für das MongoDB-Konto
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

In diesem Artikel wird beschrieben, wie Sie ein Upgrade der API-Version Ihrer Azure Cosmos DB-API für das MongoDB-Konto durchführen. Nach dem Upgrade können Sie die neuesten Funktionen in der Azure Cosmos DB-API für MongoDB verwenden. Der Upgradeprozess unterbricht die Verfügbarkeit Ihres Kontos nicht und verbraucht zu keinem Zeitpunkt RU/s oder verringert die Kapazität der Datenbank. Dieser Prozess wirkt sich nicht auf vorhandene Daten oder Indizes aus. 

Beim Upgrade auf eine neue API-Version beginnen Sie mit den Entwicklungs-/Testworkloads, bevor Sie ein Upgrade der Produktionsworkloads durchführen. Es ist wichtig, ein Upgrade Ihrer Clients auf eine Version durchzuführen, die mit der für das Upgrade angestrebten API-Version kompatibel ist, bevor Sie das Upgrade Ihrer Azure Cosmos DB-API für das MongoDB-Konto ausführen.

>[!Note]
> Zurzeit kann nur für qualifizierende Konten, die Serverversion 3.2 verwenden, ein Upgrade auf Version 3.6 oder 4.0 durchgeführt werden. Wenn Ihr Konto die Upgradeoption nicht anzeigt, [erstellen Sie ein Supportticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="upgrading-to-40-or-36"></a>Upgrade auf 4.0 oder 3.6

### <a name="benefits-of-upgrading-to-version-40"></a>Vorteile des Upgrades auf Version 4.0

Die folgenden neuen Features sind in Version 4.0 enthalten:
- Unterstützung für Transaktionen mit mehreren Dokumenten in Sammlungen ohne Shards
- Neue Aggregationsoperatoren
- Verbesserte Leistung bei Überprüfungen
- Schnellerer, effizienterer Speicher

### <a name="benefits-of-upgrading-to-version-36"></a>Vorteile des Upgrades auf Version 3.6

Die folgenden neuen Features sind in Version 3.6 enthalten:
- Verbesserte Leistung und Stabilität
- Unterstützung für neue Datenbankbefehle
- Standardunterstützung für Aggregationspipeline und neue Aggregationsphasen
- Unterstützung für Änderungsdatenströme
- Unterstützung für zusammengesetzte Indizes
- Partitionsübergreifende Unterstützung für die folgenden Vorgänge: Aktualisieren, Löschen, Zählen und Sortieren
- Verbesserte Leistung für die folgenden zusammengesetzten Operationen: „$count“, „$skip“, „$limit“ und „$group“
- Platzhalterindizierung wird jetzt unterstützt

### <a name="changes-from-version-32"></a>Änderungen gegenüber Version 3.2

- Das Feature für die [serverseitige Wiederholung (Server-Side Retry, SSR)](prevent-rate-limiting-errors.md) ist standardmäßig aktiviert, sodass Anforderungen von der Clientanwendung keine 16500-Fehler zurückgeben. Stattdessen werden Anforderungen so lange fortgesetzt, bis sie abgeschlossen sind oder das Zeitlimit von 60 Sekunden erreicht wird.
- Das Pro-Anforderungstimeout ist auf 60 Sekunden festgelegt.
- Bei MongoDB-Sammlungen, die in der neuen Wire Protocol-Version erstellt wurden, wird nur die Eigenschaft `_id` standardmäßig indiziert.

### <a name="action-required-when-upgrading-from-32"></a>Erforderliche Aktion beim Upgrade von 3.2

Beim Upgrade von 3.2 wird das Endpunktsuffix des Datenbankkontos auf das folgende Format aktualisiert:

```
<your_database_account_name>.mongo.cosmos.azure.com
```

Beim Upgrade von Version 3.2 müssen Sie den vorhandenen Endpunkt in Ihren Anwendungen und Treibern ersetzen, die eine Verbindung mit diesem Datenbankkonto herstellen. **Nur Verbindungen, die den neuen Endpunkt verwenden, können auf die Features in der neuen API-Version zugreifen**. Der vorherige 3.2-Endpunkt sollte das Suffix `.documents.azure.com` haben.

>[!Note]
> Dieser Endpunkt weist möglicherweise geringfügige Unterschiede auf, wenn Ihr Konto in einer unabhängigen, behördlichen oder eingeschränkten Azure-Cloud erstellt wurde.

## <a name="how-to-upgrade"></a>Ausführen eines Upgrades

1. Wechseln Sie zum Azure-Portal, und navigieren Sie zum Übersichtsblatt für Ihre Azure Cosmos DB-API für das MongoDB-Konto. Vergewissern Sie sich, dass die aktuelle Serverversion Ihren Erwartungen entspricht.

    :::image type="content" source="./media/mongodb-version-upgrade/1.png" alt-text="Azure-Portal mit MongoDB-Konto – Übersicht" border="false":::

2. Wählen Sie in den Optionen auf der linken Seite das Blatt `Features` aus. Dadurch werden die Features auf Kontoebene angezeigt, die für Ihr Datenbankkonto zur Verfügung stehen.

    :::image type="content" source="./media/mongodb-version-upgrade/2.png" alt-text="Übersicht über das Azure-Portal mit dem MongoDB-Konto; das Blatt „Features“ ist hervorgehoben" border="false":::

3. Klicken Sie auf die Zeile `Upgrade Mongo server version`. Wenn diese Option nicht angezeigt wird, ist Ihr Konto für dieses Upgrade möglicherweise nicht qualifiziert. Wenn das zutrifft, erstellen Sie [ein Supportticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

    :::image type="content" source="./media/mongodb-version-upgrade/3.png" alt-text="Blatt „Features“ mit Optionen." border="false":::

4. Überprüfen Sie die für das Upgrade angezeigten Informationen. Sobald Sie zum Starten des Prozesses bereit sind, klicken Sie auf `Enable`.

    :::image type="content" source="./media/mongodb-version-upgrade/4.png" alt-text="Erweiterte Upgradeanleitung." border="false":::

5. Nachdem der Prozess gestartet wurde, wird im Menü `Features` der Status des Upgrades angezeigt. Der Status wechselt von `Pending` über `In Progress` zu `Upgraded`. Dieser Prozess wirkt sich nicht auf die vorhandenen Funktionen oder Vorgänge des Datenbankkontos aus.

    :::image type="content" source="./media/mongodb-version-upgrade/5.png" alt-text="Upgradestatus nach dem Initiieren." border="false":::

6. Sobald das Upgrade abgeschlossen ist, wird der Status als `Upgraded` angezeigt. Klicken Sie darauf, um weitere Informationen zu den nächsten Schritten und Aktionen zu erhalten, die Sie zum Abschließen des Prozesses ausführen müssen. Wenn beim Verarbeiten Ihrer Anforderung ein Problem aufgetreten ist, [wenden Sie sich an den Support](https://azure.microsoft.com/en-us/support/create-ticket/).

    :::image type="content" source="./media/mongodb-version-upgrade/6.png" alt-text="Kontostatus „Upgraded“." border="false":::

7. 
    1. Wenn Sie ein Upgrade von 3.2 durchgeführt haben, kehren Sie zum Blatt `Overview` zurück, und kopieren Sie die neue Verbindungszeichenfolge für die Verwendung in Ihrer Anwendung. Die alte Verbindungszeichenfolge, mit der 3.2 ausgeführt wird, wird nicht unterbrochen. Zur Gewährleistung eines konsistenten Verhaltens müssen alle Ihre Anwendungen den neuen Endpunkt verwenden.
    2. Wenn Sie ein Upgrade von 3.6 durchgeführt haben, wird die vorhandene Verbindungszeichenfolge auf die angegebene Version aktualisiert und sollte weiterhin verwendet werden.

    :::image type="content" source="./media/mongodb-version-upgrade/7.png" alt-text="Neues Übersichtsblatt." border="false":::


## <a name="how-to-downgrade"></a>Ausführen eines Downgrades
Anhand der gleichen Schritte im Abschnitt „Ausführen eines Upgrades“ können Sie auch ein Downgrade Ihres Kontos von 4.0 auf 3.6 durchführen. 

Wenn Sie ein Upgrade von 3.2 auf 4.0 oder 3.6 durchgeführt haben und nun ein Downgrade auf 3.2 vornehmen möchten, können Sie einfach mit dem Host `accountname.documents.azure.com`, auf dem auch nach dem Upgrade noch Version 3.2 aktiv bleibt, zurück zur vorherigen Verbindungszeichenfolge (3.2) wechseln.


## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die unterstützten und nicht unterstützten [Features von MongoDB, Version 4.0](mongodb-feature-support-40.md).
- Informieren Sie sich über die unterstützten und nicht unterstützten [Features von MongoDB, Version 3.6](mongodb-feature-support-36.md).
- Weitere Informationen finden Sie unter [Features der Mongo-Version 3.6](https://devblogs.microsoft.com/cosmosdb/azure-cosmos-dbs-api-for-mongodb-now-supports-server-version-3-6/)
