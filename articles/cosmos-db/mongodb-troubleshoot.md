---
title: Behandeln häufiger Fehler in der Azure Cosmos DB-API für MongoDB
description: In diesem Dokument erfahren Sie, wie Sie häufige Probleme in der Azure Cosmos DB-API für MongoDB behandeln.
author: christopheranderson
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: troubleshooting
ms.date: 07/15/2020
ms.author: chrande
ms.openlocfilehash: de39aee73a6f4b422af4524d3302f8858f8b060b
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101692231"
---
# <a name="troubleshoot-common-issues-in-azure-cosmos-dbs-api-for-mongodb"></a>Behandeln häufiger Probleme in der Azure Cosmos DB-API für MongoDB
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

Im folgenden Artikel werden allgemeine Fehler und Lösungen für Bereitstellung beschrieben, die die Azure Cosmos DB-API für MongoDB verwenden.

>[!Note]
> Azure Cosmos DB hostet nicht die MongoDB-Engine. Der Dienst bietet eine Implementierung der [Wire Protocol-Versionen 4.0](mongodb-feature-support-40.md) und [3.6](mongodb-feature-support-36.md) von MongoDB sowie Legacy-Unterstützung für die [Wire Protocol-Version 3.2](mongodb-feature-support.md). Daher finden sich einige dieser Fehler nur in der Azure Cosmos DB-API für MongoDB.

## <a name="common-errors-and-solutions"></a>Häufige Fehler und Lösungen

| Code       | Fehler                | BESCHREIBUNG  | Lösung  |
|------------|----------------------|--------------|-----------|
| 2 | BadValue | Ein häufiger Grund ist, dass der Indexpfad zum angegebenen ORDER BY-Element ausgeschlossen ist. Es kann auch sein, dass die ORDER BY-Abfrage nicht über einen entsprechenden zusammengesetzten Index für die Bereitstellung verfügt. Die Abfrage fordert eine Sortierung für ein Feld an, das nicht indiziert ist. | Erstellen Sie einen übereinstimmenden Index (oder zusammengesetzten Index) für die durchzuführende Sortierabfrage. |
| 2 | Die Transaktion ist nicht aktiv. | Bei der Transaktion mit mehreren Dokumenten wurde das feste Zeitlimit von fünf Sekunden überschritten. | Wiederholen Sie die Transaktion mit mehreren Dokumenten, oder schränken Sie den Umfang von Vorgängen innerhalb der Transaktion mit mehreren Dokumenten ein, damit sie innerhalb des Zeitlimits von fünf Sekunden abgeschlossen wird. |
| 13 | Nicht autorisiert | Der Anforderung fehlen die Berechtigungen für die vollständige Ausführung. | Achten Sie darauf, dass Sie die richtigen Schlüssel verwenden.  |
| 26 | NamespaceNotFound | Die Datenbank oder Sammlung, auf die in der Abfrage verwiesen wird, wird nicht gefunden. | Stellen Sie sicher, dass der Name Ihrer Datenbank bzw. Sammlung exakt dem Namen in Ihrer Abfrage entspricht.|
| 50 | ExceededTimeLimit | Die Anforderung hat das Ausführungstimeout von 60 Sekunden überschritten. |  Dieser Fehler kann vielfältige Ursachen haben. Eine dieser Ursachen kann darin bestehen, dass die Kapazität der aktuell zugeordneten Anforderungseinheiten nicht ausreicht, um die Anforderung abzuschließen. Dieses Problem kann durch Erhöhen der Anforderungseinheiten dieser Sammlung oder Datenbank behoben werden. In anderen Fällen kann dieser Fehler umgangen werden, indem eine große Anforderung in kleinere Anforderungen aufgeteilt wird. Das Wiederholen eines Schreibvorgangs, bei dem dieser Fehler aufgetreten ist, kann zu einem doppelten Schreibvorgang führen. <br><br>Gehen Sie wie folgt vor, wenn Sie große Datenmengen löschen möchten, ohne dass sich dies auf RUs auswirkt: <br>- Erwägen Sie die Verwendung einer Gültigkeitsdauer (basierend auf dem Zeitstempel): [Ablauf von Daten mit der Azure Cosmos DB-API für MongoDB](mongodb-time-to-live.md) <br>- Verwenden Sie Cursor-/Batchgröße, um den Löschvorgang durchzuführen. Sie können jeweils ein Dokument abrufen und per Schleife löschen. Dies ermöglicht das langsame Löschen von Daten ohne Beeinträchtigung Ihrer Produktionsanwendung.|
| 61 | ShardKeyNotFound | Das Dokument in Ihrer Anforderung enthielt den Shardschlüssel (Azure Cosmos DB-Partitionsschlüssel) der Sammlung nicht. | Stellen Sie sicher, dass in der Anforderung der Shardschlüssel der Sammlung verwendet wird.|
| 66 | ImmutableField | Die Anforderung versucht, ein unveränderliches Feld zu ändern. | Felder vom Typ „_id“ sind unveränderlich. Stellen Sie sicher, dass Ihre Anforderung nicht versucht, dieses Feld oder das Feld für den Shardschlüssel zu aktualisieren. |
| 67 | CannotCreateIndex | Die Anforderung zum Erstellen eines Indexes kann nicht abgeschlossen werden. | In einem Container können bis zu 500 Einzelfeldindizes erstellt werden. Ein zusammengesetzter Index kann bis zu acht Felder enthalten (zusammengesetzte Indizes werden in Version 3.6 und höher unterstützt). |
| 112 | WriteConflict | Die Transaktion mit mehreren Dokumenten war aufgrund eines Konflikts mit einer anderen Transaktion mit mehreren Dokumenten nicht erfolgreich. | Wiederholen Sie die Transaktion mit mehreren Dokumenten, bis sie erfolgreich ist. |
| 115 | CommandNotSupported | Die Anforderung wird nicht unterstützt. | In der Fehlermeldung sollten weitere Informationen angegeben sein. Wenn diese Funktion für Ihre Bereitstellungen wichtig ist, erstellen Sie im [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ein Supportticket. Das Azure Cosmos DB-Team setzt sich dann mit Ihnen in Verbindung. |
| 11000 | DuplicateKey | Der Shardschlüssel (Azure Cosmos DB-Partitionsschlüssel) des einzufügenden Dokuments ist in der Sammlung bereits vorhanden, oder eine Einschränkung in Bezug auf eindeutige Indexfelder wurde verletzt. | Verwenden Sie die update()-Funktion, um ein vorhandenes Dokument zu aktualisieren. Wenn eine Einschränkung in Bezug auf eindeutige Indexfelder verletzt wurde, fügen Sie einen Feldwert in das Dokument ein, der im Shard bzw. in der Partition noch nicht vorhanden ist, oder aktualisieren Sie das Dokument mit einem entsprechenden Feldwert. Eine weitere Möglichkeit ist die Verwendung eines Felds, das eine Kombination aus den Feldern für die ID und den Shardschlüssel enthält. |
| 16500 | TooManyRequests  | Die Gesamtanzahl der verbrauchten Anforderungseinheiten hat die bereitgestellte Anforderungseinheitenrate für die Sammlung überschritten und wurde gedrosselt. | Skalieren Sie über das Azure-Portal ggf. den Durchsatz, der einem Container oder einer Gruppe von Containern zugewiesen ist, oder wiederholen Sie den Vorgang. Wenn Sie SSR (Server-Side Retry, serverseitige Wiederholung) aktivieren, versucht Azure Cosmos DB automatisch, die Anforderungen zu wiederholen, die aufgrund dieses Fehlers nicht ausgeführt werden konnten. |
| 16501 | ExceededMemoryLimit | Der Vorgang ist ein mehrinstanzenfähiger Dienst und hat die Speicherzuweisung des Clients überschritten. Dies gilt nur für Version 3.2 der Azure Cosmos DB-API für MongoDB. | Verringern Sie den Umfang des Vorgangs mithilfe restriktiverer Abfragekriterien, oder wenden Sie sich im [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) an den Support. Beispiel: `db.getCollection('users').aggregate([{$match: {name: "Andy"}}, {$sort: {age: -1}}]))` |
| 40324 | Nicht erkannter Name einer Pipelinestage | Der Stagename in der Anforderung Ihrer Aggregationspipeline wurde nicht erkannt. | Stellen Sie sicher, dass alle Aggregationspipelinenamen in Ihrer Anforderung gültig sind. |
| - | Probleme mit der MongoDB-Wire-Version | Ältere Versionen von MongoDB-Treibern können den Namen des Azure Cosmos-Kontos in den Verbindungszeichenfolgen nicht erkennen. | Fügen Sie am Ende Ihrer Verbindungszeichenfolge `appName=@accountName@` an. `accountName` ist dabei der Name Ihres Azure Cosmos DB-Kontos. |
| - | Netzwerkprobleme beim MongoDB-Client (beispielsweise Socket- oder endOfStream-Ausnahmen)| Bei der Netzwerkanforderung ist ein Fehler aufgetreten. Dies wird häufig dadurch verursacht, dass der MongoDB-Client versucht, eine inaktive TCP-Verbindung zu verwenden. MongoDB-Treiber nutzen häufig Verbindungspools, was dazu führt, dass für Anforderungen zufällige Verbindungen aus dem Pool ausgewählt werden. Bei inaktiven Verbindungen tritt im Azure Cosmos DB-Endpunkt in der Regel nach vier Minuten ein Timeout auf. | Sie können diese fehlerhaften Anforderungen über den Anwendungscode wiederholen, die Treibereinstellungen Ihres MongoDB-Clients so ändern, dass inaktive TCP-Verbindungen vor Ablauf des vierminütigen Timeoutfensters abgebrochen werden, oder die `keepalive`-Einstellungen des Betriebssystems so konfigurieren, dass TCP-Verbindungen im aktiven Status beibehalten werden.<br><br>Zur Vermeidung von Konnektivitätsmeldungen kann es ratsam sein, die Verbindungszeichenfolge zu ändern und `maxConnectionIdleTime` auf 1 - 2 Minuten festzulegen.<br>- Mongo-Treiber: Konfigurieren Sie Folgendes: `maxIdleTimeMS=120000` <br>- Node.js: Konfigurieren Sie Folgendes: `socketTimeoutMS=120000`, `autoReconnect` = true, `keepAlive` = true, `keepAliveInitialDelay` = 3 minutes
| - | Mongo-Shell funktioniert im Azure-Portal nicht | Wenn der Benutzer versucht, eine Mongo-Shell zu öffnen, passiert nichts, und die Registerkarte bleibt leer.  | Überprüfen Sie die Firewall. Die Firewall wird für die Mongo-Shell im Azure-Portal nicht unterstützt. <br>- Installieren Sie die Mongo-Shell auf dem lokalen Computer innerhalb der Firewallregeln. <br>- Verwenden Sie eine Legacy-Mongo-Shell.
| - | Verbindung kann mit Verbindungszeichenfolge nicht hergestellt werden  | Beim Upgrade von 3.2 auf 3.6 hat sich die Verbindungszeichenfolge geändert | Beachten Sie bei Verwendung von Konten mit der MongoDB-API von Azure Cosmos DB, dass der Endpunkt bei der Kontoversion 3.6 das Format `*.mongo.cosmos.azure.com` hat, während der Endpunkt bei der Kontoversion 3.2 das Format `*.documents.azure.com` hat.  

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [Studio 3T](mongodb-mongochef.md) mit der API für MongoDB von Azure Cosmos DB verwenden.
- Erfahren Sie, wie Sie [Robo 3T](mongodb-robomongo.md) mit der API für MongoDB von Azure Cosmos DB verwenden.
- Untersuchen Sie MongoDB-[Beispiele](mongodb-samples.md) mit der API für MongoDB von Azure Cosmos DB.
