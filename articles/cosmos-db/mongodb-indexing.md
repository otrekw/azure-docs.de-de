---
title: Indizieren in der API für MongoDB von Azure Cosmos-DB
description: Enthält eine Übersicht über die Indizierungsfunktionen mit der API für MongoDB von Azure Cosmos DB.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 04/03/2020
author: timsander1
ms.author: tisande
ms.openlocfilehash: f3f369928270c77557337bfdb1037cc5174c39f2
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2020
ms.locfileid: "80637961"
---
# <a name="indexing-using-azure-cosmos-dbs-api-for-mongodb"></a>Indizieren mit der API für MongoDB von Azure Cosmos-DB

Die API für MongoDB von Azure Cosmos DB nutzt die Kernfunktionen für automatische Indexverwaltung von Azure Cosmos DB. Dieses Dokument konzentriert sich auf das Hinzufügen von Indizes mithilfe der Azure Cosmos DB-API für MongoDB. Sie können auch den Artikel [Indizierung in Azure Cosmos DB: Übersicht](index-overview.md) lesen, der für alle APIs relevant ist.

## <a name="indexing-for-version-36"></a>Indizierung für die Version 3.6

Das `_id`-Feld wird immer automatisch indiziert und kann nicht gelöscht werden. Die API für MongoDB von Azure Cosmos DB erzwingt automatisch die Eindeutigkeit des `_id`-Felds pro Shard-Schlüssel.

Um zusätzliche Felder zu indizieren, sollten Sie die MongoDB-Indexverwaltungsbefehle verwenden. Wie in MongoDB wird nur das `_id`-Feld automatisch indiziert. Diese Standardindizierungsrichtlinie unterscheidet sich von der Azure Cosmos DB-SQL-API, die standardmäßig alle Felder indiziert.

Um eine Abfrage zu sortieren, muss ein Index für die Felder erstellt werden, die im Sortiervorgang verwendet werden.

## <a name="index-types"></a>Indextypen

### <a name="single-field"></a>Einzelfeld

Sie können für jedes einzelne Feld Indizes erstellen. Die Sortierreihenfolge des Einzelfeldindexes ist unerheblich. Der folgende Befehl erstellt einen Index für das Feld `name`:

`db.coll.createIndex({name:1})`

Bei einer Abfrage werden mehrere Einzelfeldindizes verwendet, soweit verfügbar. Sie können pro Container bis zu 500 Einzelfeldindizes erstellen.

### <a name="compound-indexes-36"></a>Zusammengesetzte Indizes (3.6)

Zusammengesetzte Indizes werden für Konten mit der Wire Protocol-Version 3.6 unterstützt. Sie können bis zu 8 Felder in einen zusammengesetzten Index einschließen. Anders als in MongoDB sollten Sie nur dann einen zusammengesetzten Index erstellen, wenn die Abfrage auf mehrere Felder gleichzeitig effizient sortiert werden muss. Für Abfragen mit mehreren Filtern, die nicht sortiert werden müssen, sollten Sie anstelle eines einzelnen zusammengesetzten Indexes mehrere Einzelfeldindizes erstellen.

Der folgende Befehl erstellt einen zusammengesetzten Index für die Felder `name` und `age`:

`db.coll.createIndex({name:1,age:1})`

Zusammengesetzte Indizes ermöglichen eine effiziente gleichzeitige Sortierung auf der Grundlage mehrerer Felder. Beispiel:

`db.coll.find().sort({name:1,age:1})`

Der obige zusammengesetzte Index kann auch für die effiziente Sortierung aufgrund einer Abfrage mit der umgekehrten Sortierreihenfolge für alle Felder verwendet werden. Hier sehen Sie ein Beispiel:

`db.coll.find().sort({name:-1,age:-1})`

Allerdings muss die Reihenfolge der Pfade im zusammengesetzten Index exakt mit der Abfrage übereinstimmen. Hier sehen Sie ein Beispiel für eine Abfrage, die einen zusätzlichen zusammengesetzten Index erfordern würde:

`db.coll.find().sort({age:1,name:1})`

### <a name="multikey-indexes"></a>Indizes mit mehreren Schlüsseln

Azure Cosmos DB erstellt Indizes mit mehreren Schlüsseln, um in Arrays gespeicherte Inhalte zu indizieren. Wenn Sie ein Feld mit einem Arraywert indizieren, indiziert Azure Cosmos DB automatisch alle Elemente im Array.

### <a name="geospatial-indexes"></a>Räumliche Indizes

Viele räumliche Operatoren profitieren von räumlichen Indizes. Derzeit unterstützt die API für MongoDB von Azure Cosmos DB `2dsphere`-Indizes. `2d`-Indizes werden noch nicht unterstützt.

Hier sehen Sie ein Beispiel für das Erstellen eines georäumlichen Indexes für das `location`-Feld:

`db.coll.createIndex({ location : "2dsphere" })`

### <a name="text-indexes"></a>Textindizes

Textindizes werden derzeit nicht unterstützt. Bei Textsuchabfragen für Zeichenfolgen sollten Sie die [Azure Cognitive Search](https://docs.microsoft.com/azure/search/search-howto-index-cosmosdb)-Integration in Azure Cosmos DB verwenden.

## <a name="index-properties"></a>Indexeigenschaften

Die folgenden Vorgänge können sowohl für Konten mit der Wire Protocol-Version 3.6 als auch für Konten mit früheren Wire Protocol-Versionen verwendet werden: Sie können auch mehr über [unterstützte Indizes und indizierte Eigenschaften](mongodb-feature-support-36.md#indexes-and-index-properties) erfahren.

### <a name="unique-indexes"></a>Eindeutige Indizes

[Eindeutige Indizes](unique-keys.md) sind nützlich zum Erzwingen eines Zustands, in dem zwei oder mehr Dokumente den gleichen Wert für die indizierten Felder enthalten.

>[!Important]
> Eindeutige Indizes können nur erstellt werden, wenn die Sammlung leer ist (keine Dokumente enthält).

Mit dem folgenden Befehl wird ein eindeutiger Index im Feld „student_id“ erstellt:

```shell
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1 }, {unique:true} )
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 1,
        "numIndexesAfter" : 4
}
```

Für Sammlungen mit Shards ist für die Erstellung eines eindeutigen Indexes die Angabe des Shard-Schlüssels (Partitionsschlüssel) erforderlich. Anders ausgedrückt: Alle eindeutigen Indizes einer Sammlung mit Shards sind zusammengesetzte Indizes, bei denen eines der Felder der Partitionsschlüssel ist.

Mit den folgenden Befehlen wird die Sammlung ```coll``` mit Shards erstellt (Shard-Schlüssel lautet ```university```), die für die Felder „student_id“ und „university“ über einen eindeutigen Index verfügt:

```shell
globaldb:PRIMARY> db.runCommand({shardCollection: db.coll._fullName, key: { university: "hashed"}});
{
        "_t" : "ShardCollectionResponse",
        "ok" : 1,
        "collectionsharded" : "test.coll"
}
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1, "university" : 1 }, {unique:true})
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 3,
        "numIndexesAfter" : 4
}
```

Wenn im obigen Beispiel die ```"university":1```-Klausel weggelassen wird, wird ein Fehler mit der folgenden Meldung zurückgegeben:

```"cannot create unique index over {student_id : 1.0} with shard key pattern { university : 1.0 }"```

### <a name="ttl-indexes"></a>TTL-Indizes

Es muss ein [„TTL-Index“ (Index für die Gültigkeitsdauer)](../cosmos-db/time-to-live.md) erstellt werden, um den Dokumentablauf in einer bestimmten Sammlung zu aktivieren. Ein TTL-Index ist ein Index im Feld „_ts“ mit dem Wert „expireAfterSeconds“.

Beispiel:

```JavaScript
globaldb:PRIMARY> db.coll.createIndex({"_ts":1}, {expireAfterSeconds: 10})
```

Der obige Befehl bewirkt das Löschen beliebiger Dokumente in der Sammlung ```db.coll```, die innerhalb der letzten zehn Sekunden nicht geändert wurden.

> [!NOTE]
> **_ts** ist ein Azure Cosmos DB-spezifisches Feld, auf das nicht über MongoDB-Clients zugegriffen werden kann. Dies ist eine reservierte Eigenschaft (Systemeigenschaft), die den Zeitstempel der letzten Änderung eines Dokuments enthält.

## <a name="track-the-index-progress"></a>Nachverfolgen des Indexfortschritts

Die Version 3.6 der Azure Cosmos DB-API für MongoDB-Konten unterstützt den `currentOp()`-Befehl zum Nachverfolgen des Indexfortschritts für eine Datenbankinstanz. Dieser Befehl gibt ein Dokument zurück, das Informationen über die aktuell in Bearbeitung befindlichen Vorgänge in einer Datenbankinstanz enthält. Der Befehl `currentOp` wird verwendet, um alle aktiven Vorgänge in nativen MongoDB-Datenbanken nachzuverfolgen. Bei der Azure Cosmos DB-API für MongoDB unterstützt dieser Befehl jedoch nur die Nachverfolgung des Indexvorgangs.

Im Folgenden werden einige Beispiele zur Verwendung des `currentOp`-Befehls zur Nachverfolgung des Indexfortschritts gezeigt:

* Abrufen des Indexfortschritts für eine Sammlung:

   ```shell
   db.currentOp({"command.createIndexes": <collectionName>, "command.$db": <databaseName>})
   ```

* Abrufen des Indexfortschritts für alle Sammlungen in einer Datenbank:

  ```shell
  db.currentOp({"command.$db": <databaseName>})
  ```

* Abrufen des Indexfortschritts für alle Datenbanken und Sammlungen in einem Azure Cosmos-Konto:

  ```shell
  db.currentOp({"command.createIndexes": { $exists : true } })
  ```

Die Details zum Indexfortschritt zeigen den Fortschritt des aktuellen Indexvorgangs in Prozent an. Im folgenden Beispiel wird das Format des ausgegebenen Dokuments für verschiedene Phasen des Indexfortschritts veranschaulicht:

1. Wenn ein Indexvorgang für die Sammlung „foo“ und die Datenbank „bar“ einen Fortschritt von 60 % aufweist, wird das folgende Dokument ausgegeben. `Inprog[0].progress.total` zeigt „100“ als Abschlussziel an.

   ```json
   {
        "inprog" : [
        {
                ………………...
                "command" : {
                        "createIndexes" : foo
                        "indexes" :[ ],
                        "$db" : bar
                },
                "msg" : "Index Build (background) Index Build (background): 60 %",
                "progress" : {
                        "done" : 60,
                        "total" : 100
                },
                …………..…..
        }
        ],
        "ok" : 1
   }
   ```

2. Bei einem Indexvorgang, der erst für die Sammlung „foo“ und die Datenbank „bar“ gestartet wurde, zeigt das Ausgabedokument einen Fortschritt von 0 % an, bis ein messbarer Wert erreicht wurde.

   ```json
   {
        "inprog" : [
        {
                ………………...
                "command" : {
                        "createIndexes" : foo
                        "indexes" :[ ],
                        "$db" : bar
                },
                "msg" : "Index Build (background) Index Build (background): 0 %",
                "progress" : {
                        "done" : 0,
                        "total" : 100
                },
                …………..…..
        }
        ],
       "ok" : 1
   }
   ```

3. Wenn der aktive Indexvorgang abgeschlossen wird, zeigt das Ausgabedokument leere Vorgänge an.

   ```json
   {
      "inprog" : [],
      "ok" : 1
   }
   ```

### <a name="background-index-updates"></a>Indexaktualisierungen im Hintergrund

Unabhängig von dem für die **Hintergrund**-Indexeigenschaft angegebenen Wert werden Indexaktualisierungen immer im Hintergrund durchgeführt. Indexaktualisierungen verbrauchen RUs mit einer niedrigeren Priorität als andere Datenbankvorgänge. Daher führen Indexänderungen bei Schreib-, Aktualisierungs- oder Löschvorgängen nicht zu Ausfallzeiten.

Wenn Sie einen neuen Index hinzufügen, verwenden Abfragen ihn sofort. Dies bedeutet, dass Abfragen möglicherweise nicht alle übereinstimmenden Ergebnisse zurückgeben und dabei keine Fehler zurückgeben. Nachdem die Indextransformation abgeschlossen ist, werden die Abfrageergebnisse konsistent. Sie können [den Indexfortschritt nachverfolgen](#track-the-index-progress).

## <a name="migrating-collections-with-indexes"></a>Migrieren von Sammlungen mit Indizes

Derzeit ist die Erstellung von eindeutigen Indizes nur möglich, wenn die Sammlung keine Dokumente enthält. Bei gängigen MongoDB-Migrationstools wird versucht, die eindeutigen Indizes nach dem Importieren der Daten zu erstellen. Zur Problemumgehung wird empfohlen, dass Benutzer die entsprechenden Sammlungen und eindeutigen Indizes manuell erstellen, anstatt das Migrationstool zuzulassen. (Für ```mongorestore``` wird dieses Verhalten erreicht, indem in der Befehlszeile das Flag `--noIndexRestore` verwendet wird.)

## <a name="indexing-for-version-32"></a>Indizierung für die Version 3.2

Bei Azure Cosmos DB-Konten, die mit der Version 3.2 des MongoDB-Wire-Protokolls kompatibel sind, sind die verfügbaren Indizierungsfeatures und die Standards unterschiedlich. Sie können [die Version Ihres Kontos überprüfen](mongodb-feature-support-36.md#protocol-support). Sie können ein Upgrade auf die Version 3.6 durchführen, indem Sie eine [Supportanfrage](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) einreichen.

Wenn Sie Version 3.2 verwenden, beachten Sie die in diesem Abschnitt erläuterten wichtigen Unterschiede zu Version 3.6.

### <a name="dropping-the-default-indexes-32"></a>Löschen der Standardindizes (3.2)

Anders als bei der Version 3.6 der API für MongoDB von Azure Cosmos DB werden alle Eigenschaften in der Version 3.2 standardmäßig indiziert. Der folgende Befehl kann verwendet werden, um diese Standardindizes für eine Sammlung ```coll``` zu löschen:

```JavaScript
> db.coll.dropIndexes()
{ "_t" : "DropIndexesResponse", "ok" : 1, "nIndexesWas" : 3 }
```

Nachdem Sie die Standardindizes gelöscht haben, können Sie zusätzliche Indizes wie in Version 3.6 hinzufügen.

### <a name="compound-indexes-32"></a>Zusammengesetzte Indizes (3.2)

Zusammengesetzte Indizes enthalten Verweise auf mehrere Felder eines Dokuments. Wenn Sie einen zusammengesetzten Index erstellen möchten, führen Sie ein Upgrade auf die Version 3.6 durch, indem Sie eine [Supportanfrage](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) einreichen.

## <a name="next-steps"></a>Nächste Schritte

* [Indizierung in Azure Cosmos DB](../cosmos-db/index-policy.md)
* [Festlegen einer Gültigkeitsdauer für den automatischen Ablauf von Daten in Azure Cosmos DB](../cosmos-db/time-to-live.md)
