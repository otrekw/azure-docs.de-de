---
title: Verwalten der Indizierung in der Azure Cosmos DB-API für MongoDB
description: Dieser Artikel enthält eine Übersicht über die Indizierungsfunktionen von Azure Cosmos DB über die MongoDB-API.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 04/03/2020
author: timsander1
ms.author: tisande
ms.openlocfilehash: fd602f88acf26e821e57e0a844f543aac08dad0d
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732705"
---
# <a name="manage-indexing-in-azure-cosmos-dbs-api-for-mongodb"></a>Verwalten der Indizierung in der Azure Cosmos DB-API für MongoDB

Die Azure Cosmos DB-API für MongoDB nutzt die Kernfunktionen für automatische Indexverwaltung von Azure Cosmos DB. Dieser Artikel konzentriert sich auf das Hinzufügen von Indizes mithilfe der Azure Cosmos DB-API für MongoDB. Sie können auch den Artikel [Indizierung in Azure Cosmos DB: Übersicht](index-overview.md) lesen, der für alle APIs relevant ist.

## <a name="indexing-for-mongodb-server-version-36"></a>Indizierung für MongoDB-Serverversion 3.6

Die Azure Cosmos DB-API für die MongoDB-Serverversion 3.6 indiziert automatisch das Feld `_id`, das nicht gelöscht werden kann. Sie erzwingt automatisch die Eindeutigkeit des Felds `_id` durch einen Shardschlüssel.

Um zusätzliche Felder zu indizieren, verwenden Sie die MongoDB-Indexverwaltungsbefehle. Wie bei MongoDB indiziert die Azure Cosmos DB-API für MongoDB automatisch auch nur das Feld `_id`. Diese Standardindizierungsrichtlinie unterscheidet sich von der Azure Cosmos DB-SQL-API, die standardmäßig alle Felder indiziert.

Um eine Abfrage zu sortieren, müssen Sie einen Index für die Felder erstellen, die beim Sortiervorgang verwendet werden.

## <a name="index-types"></a>Indextypen

### <a name="single-field"></a>Einzelfeld

Sie können für jedes einzelne Feld Indizes erstellen. Die Sortierreihenfolge des Einzelfeldindexes ist unerheblich. Der folgende Befehl erstellt einen Index für das Feld `name`:

`db.coll.createIndex({name:1})`

Bei einer Abfrage werden mehrere Einzelfeldindizes verwendet, soweit verfügbar. Sie können pro Container bis zu 500 Einzelfeldindizes erstellen.

### <a name="compound-indexes-mongodb-server-version-36"></a>Zusammengesetzte Indizes (MongoDB-Serverversion 3.6)

Die Azure Cosmos DB-API für MongoDB unterstützt zusammengesetzte Indizes für Konten, die Version 3.6 des Wire-Protokolls verwenden. Sie können bis zu acht Felder in einen zusammengesetzten Index einschließen. Anders als in MongoDB sollten Sie nur dann einen zusammengesetzten Index erstellen, wenn die Abfrage über mehrere Felder gleichzeitig effizient sortiert werden muss. Für Abfragen mit mehreren Filtern, die nicht sortiert werden müssen, sollten Sie anstelle eines einzelnen zusammengesetzten Indexes mehrere Einzelfeldindizes erstellen.

Der folgende Befehl erstellt einen zusammengesetzten Index für die Felder `name` und `age`:

`db.coll.createIndex({name:1,age:1})`

Sie können zusammengesetzte Indizes zum gleichzeitigen Sortieren anhand mehrerer Felder verwenden, wie im folgenden Beispiel gezeigt:

`db.coll.find().sort({name:1,age:1})`

Sie können obigen zusammengesetzten Index auch für die effiziente Sortierung einer Abfrage mit der umgekehrten Sortierreihenfolge anhand aller Felder verwenden. Hier sehen Sie ein Beispiel:

`db.coll.find().sort({name:-1,age:-1})`

Allerdings muss die Reihenfolge der Pfade im zusammengesetzten Index exakt mit der Abfrage übereinstimmen. Hier sehen Sie ein Beispiel für eine Abfrage, die einen zusätzlichen zusammengesetzten Index erfordern würde:

`db.coll.find().sort({age:1,name:1})`

### <a name="multikey-indexes"></a>Indizes mit mehreren Schlüsseln

Azure Cosmos DB erstellt Indizes mit mehreren Schlüsseln, um in Arrays gespeicherte Inhalte zu indizieren. Wenn Sie ein Feld mit einem Arraywert indizieren, indiziert Azure Cosmos DB automatisch alle Elemente im Array.

### <a name="geospatial-indexes"></a>Räumliche Indizes

Viele räumliche Operatoren profitieren von räumlichen Indizes. Derzeit unterstützt die API für MongoDB von Azure Cosmos DB `2dsphere`-Indizes. Die API unterstützt `2d`-Indizes noch nicht.

Hier sehen Sie ein Beispiel für das Erstellen eines räumlichen Indexes für das Feld `location`:

`db.coll.createIndex({ location : "2dsphere" })`

### <a name="text-indexes"></a>Textindizes

Derzeit unterstützt die Azure Cosmos DB-API für MongoDB Textindizes noch nicht. Bei Textsuchabfragen für Zeichenfolgen sollten Sie die [Azure Cognitive Search](https://docs.microsoft.com/azure/search/search-howto-index-cosmosdb)-Integration mit Azure Cosmos DB verwenden.

## <a name="index-properties"></a>Indexeigenschaften

Die folgenden Vorgänge können für Konten mit der Wire-Protokollversion 3.6 und Konten mit früheren Versionen verwendet werden. Sie können noch mehr über [unterstützte Indizes und indizierte Eigenschaften](mongodb-feature-support-36.md#indexes-and-index-properties) erfahren.

### <a name="unique-indexes"></a>Eindeutige Indizes

[Eindeutige Indizes](unique-keys.md) sind nützlich, um zu erzwingen, dass zwei oder mehr Dokumente nicht denselben Wert für indizierte Felder enthalten dürfen.

> [!IMPORTANT]
> Eindeutige Indizes können nur erstellt werden, wenn die Sammlung leer ist (keine Dokumente enthält).

Mit dem folgenden Befehl wird ein eindeutiger Index mit dem Feld `student_id` erstellt:

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

Bei partitionierten Sammlungen muss für die Erstellung eines eindeutigen Indexes der Shardschlüssel (Partitionsschlüssel) angegeben werden. Anders ausgedrückt: Alle eindeutigen Indizes einer Sammlung mit Shards sind zusammengesetzte Indizes, bei denen eines der Felder der Partitionsschlüssel ist.

Mit den folgenden Befehlen erstellen Sie die partitionierte Sammlung ```coll``` (der Shardschlüssel lautet ```university```) mit einem eindeutigen Index für die Felder `student_id` und `university`:

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

Im vorherigen Beispiel wird beim Weglassen der ```"university":1```-Klausel folgende Fehlermeldung zurückgegeben:

```"cannot create unique index over {student_id : 1.0} with shard key pattern { university : 1.0 }"```

### <a name="ttl-indexes"></a>TTL-Indizes

Um den Dokumentablauf in einer bestimmten Sammlung zu aktivieren, müssen Sie einen [TTL-Index](../cosmos-db/time-to-live.md) (Time To Live, Gültigkeitsdauer) erstellen. Ein TTL-Index ist ein Index für das Feld `_ts` mit einem Wert für `expireAfterSeconds`.

Beispiel:

```JavaScript
globaldb:PRIMARY> db.coll.createIndex({"_ts":1}, {expireAfterSeconds: 10})
```

Der obige Befehl löscht alle Dokumente in der Sammlung ```db.coll```, die nicht innerhalb der letzten zehn Sekunden geändert wurden.

> [!NOTE]
> **_ts** ist ein Azure Cosmos DB-spezifisches Feld, auf das nicht über MongoDB-Clients zugegriffen werden kann. Dies ist eine reservierte Eigenschaft (Systemeigenschaft), die den Zeitstempel der letzten Änderung eines Dokuments enthält.

## <a name="track-index-progress"></a>Nachverfolgen des Indizierungsfortschritts

Version 3.6 der Azure Cosmos DB-API für MongoDB unterstützt den Befehl `currentOp()` zum Nachverfolgen des Indizierungsfortschritts für eine Datenbankinstanz. Dieser Befehl gibt ein Dokument zurück, das Informationen zu den aktuell in Bearbeitung befindlichen Vorgängen in einer Datenbankinstanz enthält. Verwenden Sie den Befehl `currentOp`, um alle laufenden Vorgänge in der nativen MongoDB-Datenbank zu verfolgen. In der Azure Cosmos DB-API für MongoDB unterstützt dieser Befehl nur die Nachverfolgung des Indizierungsvorgangs.

Im Folgenden werden einige Beispiele zur Verwendung des Befehls `currentOp` zur Nachverfolgung des Indizierungsfortschritts gezeigt:

* Abrufen des Indexfortschritts für eine Sammlung:

   ```shell
   db.currentOp({"command.createIndexes": <collectionName>, "command.$db": <databaseName>})
   ```

* Abrufen des Indizierungsfortschritts für alle Sammlungen in einer Datenbank:

  ```shell
  db.currentOp({"command.$db": <databaseName>})
  ```

* Abrufen des Indizierungsfortschritts für alle Datenbanken und Sammlungen in einem Azure Cosmos-Konto:

  ```shell
  db.currentOp({"command.createIndexes": { $exists : true } })
  ```

### <a name="examples-of-index-progress-output"></a>Beispiele für die Ausgabe des Indizierungsfortschritts

Die Details zum Indizierungsfortschritt zeigen den Fortschritt des aktuellen Indizierungsvorgangs in Prozent an. Im folgenden Beispiel wird das Format des ausgegebenen Dokuments für verschiedene Phasen des Indizierungsfortschritts veranschaulicht:

- Wenn ein Indizierungsvorgang für die Sammlung „foo“ und die Datenbank „bar“ einen Fortschritt von 60 % aufweist, wird das folgende Dokument ausgegeben. Im Feld `Inprog[0].progress.total` wird 100 als Zielprozentsatz für den Abschluss angezeigt.

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

- Bei einem Indizierungsvorgang, der erst für die Sammlung „foo“ und die Datenbank „bar“ gestartet wurde, zeigt das Ausgabedokument einen Fortschritt von 0 % an, bis ein messbarer Wert erreicht wurde.

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

- Wenn der aktive Indizierungsvorgang abgeschlossen wird, zeigt das Ausgabedokument leere `inprog`-Vorgänge an.

   ```json
   {
      "inprog" : [],
      "ok" : 1
   }
   ```

### <a name="background-index-updates"></a>Indexaktualisierungen im Hintergrund

Unabhängig von dem für die **Hintergrund**-Indexeigenschaft angegebenen Wert werden Indexaktualisierungen immer im Hintergrund durchgeführt. Da Indexaktualisierungen Anforderungseinheiten (Request Units, RUs) mit einer niedrigeren Priorität als andere Datenbankvorgänge nutzen, führen Indexänderungen nicht zu Ausfallzeiten bei Schreib-, Update- oder Löschvorgängen.

Wenn Sie einen neuen Index hinzufügen, verwenden Abfragen diesen sofort. Dies bedeutet, dass Abfragen möglicherweise nicht alle übereinstimmenden Ergebnisse zurückgeben, aber keinen zugehörigen Fehler zurückgeben. Nachdem die Indextransformation abgeschlossen ist, werden die Abfrageergebnisse konsistent. Sie können [den Indizierungsfortschritt nachverfolgen](#track-index-progress).

## <a name="migrate-collections-with-indexes"></a>Migrieren von Sammlungen mit Indizes

Derzeit ist die Erstellung von eindeutigen Indizes nur möglich, wenn die Sammlung keine Dokumente enthält. Bei gängigen MongoDB-Migrationstools wird versucht, die eindeutigen Indizes nach dem Importieren der Daten zu erstellen. Um dieses Problem zu umgehen, können Sie die entsprechenden Sammlungen und eindeutigen Indizes manuell erstellen, damit das Migrationstool dies nicht versucht. (Sie können dieses Verhalten für ```mongorestore``` erzielen, indem Sie das `--noIndexRestore`-Flag an der Befehlszeile verwenden.)

## <a name="indexing-for-mongodb-version-32"></a>Indizierung für MongoDB-Version 3.2

Bei Azure Cosmos-Konten, die mit Version 3.2 des MongoDB-Wire-Protokolls kompatibel sind, weichen die verfügbaren Indizierungsfeatures und Standardwerte ab. Sie können [die Version Ihres Kontos überprüfen](mongodb-feature-support-36.md#protocol-support). Sie können ein Upgrade auf die Version 3.6 durchführen, indem Sie eine [Supportanfrage](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) einreichen.

Wenn Sie Version 3.2 verwenden, beachten Sie die in diesem Abschnitt erläuterten wichtigen Unterschiede zu Version 3.6.

### <a name="dropping-default-indexes-version-32"></a>Löschen der Standardindizes (Version 3.2)

Anders als bei der Version 3.6 der Azure Cosmos DB-API für MongoDB werden in der Version 3.2 standardmäßig alle Eigenschaften indiziert. Mit dem folgenden Befehl können Sie diese Standardindizes für eine Sammlung (```coll```) löschen:

```JavaScript
> db.coll.dropIndexes()
{ "_t" : "DropIndexesResponse", "ok" : 1, "nIndexesWas" : 3 }
```

Nachdem Sie die Standardindizes gelöscht haben, können Sie zusätzliche Indizes wie in Version 3.6 hinzufügen.

### <a name="compound-indexes-version-32"></a>Zusammengesetzte Indizes (Version 3.2)

Zusammengesetzte Indizes enthalten Verweise auf mehrere Felder eines Dokuments. Wenn Sie einen zusammengesetzten Index erstellen möchten, führen Sie ein Upgrade auf Version 3.6 durch, indem Sie eine [Supportanfrage](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) erstellen.

## <a name="next-steps"></a>Nächste Schritte

* [Indizierung in Azure Cosmos DB](../cosmos-db/index-policy.md)
* [Festlegen einer Gültigkeitsdauer für den automatischen Ablauf von Daten in Azure Cosmos DB](../cosmos-db/time-to-live.md)
