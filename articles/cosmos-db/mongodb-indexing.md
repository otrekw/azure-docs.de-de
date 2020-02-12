---
title: Indizieren in der API für MongoDB von Azure Cosmos-DB
description: Enthält eine Übersicht über die Indizierungsfunktionen mit der API für MongoDB von Azure Cosmos DB.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 12/26/2018
author: sivethe
ms.author: sivethe
ms.openlocfilehash: c8879884cf3d882e6a6b441244ed139072bedeeb
ms.sourcegitcommit: f0f73c51441aeb04a5c21a6e3205b7f520f8b0e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77029468"
---
# <a name="indexing-using-azure-cosmos-dbs-api-for-mongodb"></a>Indizieren mit der API für MongoDB von Azure Cosmos-DB

Die API für MongoDB von Azure Cosmos DB nutzt die Funktionen für automatische Indexverwaltung von Cosmos DB. Daher haben Benutzer Zugriff auf die Standardindizierungsrichtlinien von Cosmos DB. Wenn vom Benutzer also keine Indizes definiert oder gelöscht wurden, werden standardmäßig alle Felder beim Einfügen in eine Sammlung automatisch indiziert. Für die meisten Szenarien empfehlen wir, die Standardindizierungsrichtlinie des Kontos zu verwenden.

## <a name="indexing-for-version-36"></a>Indizierung für die Version 3.6

Konten mit der Wire Protocol-Version 3.6 verfügen über eine andere Standardindizierungsrichtlinie als frühere Versionen. Standardmäßig wird nur das Feld „_id“ indiziert. Sollen zusätzliche Felder indiziert werden, muss der Benutzer die MongoDB-Indexverwaltungsbefehle verwenden. Um eine Abfrage zu sortieren, muss aktuell ein Index für die Felder erstellt werden, die im Sortiervorgang verwendet werden.

### <a name="dropping-the-default-indexes-36"></a>Löschen der Standardindizes (3.6)

Konten mit der Wire Protocol-Version 3.6 haben nur „_id“ als Standardindex, und dieser kann nicht gelöscht werden.

### <a name="creating-a-compound-index-36"></a>Erstellen eines zusammengesetzten Index (3.6)

Echte zusammengesetzte Indizes werden für Konten mit der Wire Protocol-Version 3.6 unterstützt. Der folgende Befehl erstellt einen zusammengesetzten Index für die Felder „a“ und „b“: `db.coll.createIndex({a:1,b:1})`

Zusammengesetzte Indizes ermöglichen eine effiziente gleichzeitige Sortierung auf der Grundlage mehrerer Felder. Beispiel: `db.coll.find().sort({a:1,b:1})`

### <a name="track-the-index-progress"></a>Nachverfolgen des Indexfortschritts

Die Version 3.6 der Azure Cosmos DB-API für MongoDB-Konten unterstützt den `currentOp()`-Befehl zum Nachverfolgen des Indexfortschritts für eine Datenbankinstanz. Dieser Befehl gibt ein Dokument zurück, das Informationen über die aktuell in Bearbeitung befindlichen Vorgänge in einer Datenbankinstanz enthält. Der Befehl `currentOp` wird verwendet, um alle aktiven Vorgänge in nativen MongoDB-Datenbanken nachzuverfolgen. Bei der Azure Cosmos DB-API für MongoDB unterstützt dieser Befehl jedoch nur die Nachverfolgung des Indexvorgangs.

Im Folgenden werden einige Beispiele zur Verwendung des `currentOp`-Befehls zur Nachverfolgung des Indexfortschritts gezeigt:

• Abrufen des Indexfortschritts für eine Sammlung:

   ```shell
   db.currentOp({"command.createIndexes": <collectionName>, "command.$db": <databaseName>})
   ```

• Abrufen des Indexfortschritts für alle Sammlungen in einer Datenbank:

  ```shell
  db.currentOp({"command.$db": <databaseName>})
  ```

• Abrufen des Indexfortschritts für alle Datenbanken und Sammlungen in einem Azure Cosmos-Konto:

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

## <a name="indexing-for-version-32"></a>Indizierung für die Version 3.2

### <a name="dropping-the-default-indexes-32"></a>Löschen der Standardindizes (3.2)

Der folgende Befehl kann verwendet werden, um die Standardindizes für die Sammlung ```coll``` zu löschen:

```JavaScript
> db.coll.dropIndexes()
{ "_t" : "DropIndexesResponse", "ok" : 1, "nIndexesWas" : 3 }
```

### <a name="creating-a-compound-index-32"></a>Erstellen eines zusammengesetzten Index (3.2)

Zusammengesetzte Indizes enthalten Verweise auf mehrere Felder eines Dokuments. In logischer Hinsicht entspricht dies dem Erstellen von mehreren einzelnen Indizes pro Feld. Um in den Genuss der Vorteile von Optimierungen durch Cosmos DB-Indizierungsverfahren zu kommen, empfehlen wir die Erstellung von mehreren einzelnen Indizes anstelle eines einzelnen (nicht eindeutigen) zusammengesetzten Index.

## <a name="common-indexing-operations"></a>Allgemeine Indizierungsvorgänge

Die folgenden Vorgänge können sowohl für Konten mit der Wire Protocol-Version 3.6 als auch für Konten mit früheren Wire Protocol-Versionen verwendet werden: 

## <a name="creating-unique-indexes"></a>Erstellen von eindeutigen Indizes

[Eindeutige Indizes](unique-keys.md) sind nützlich zum Erzwingen eines Zustands, in dem zwei oder mehr Dokumente den gleichen Wert für die indizierten Felder enthalten.

>[!Important]
> Derzeit können eindeutige Indizes nur erstellt werden, wenn die Sammlung leer ist (keine Dokumente enthält).

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

Für Sammlungen mit Shards ist für die Erstellung eines eindeutigen Index gemäß MongoDB-Verhalten die Angabe des Shard-Schlüssels (Partitionsschlüssel) erforderlich. Anders ausgedrückt: Alle eindeutigen Indizes einer Sammlung mit Shards sind zusammengesetzte Indizes, bei denen eines der Felder der Partitionsschlüssel ist.

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

## <a name="ttl-indexes"></a>TTL-Indizes

Es muss ein [„TTL-Index“ (Index für die Gültigkeitsdauer)](../cosmos-db/time-to-live.md) erstellt werden, um den Dokumentablauf in einer bestimmten Sammlung zu aktivieren. Ein TTL-Index ist ein Index im Feld „_ts“ mit dem Wert „expireAfterSeconds“.

Beispiel:

```JavaScript
globaldb:PRIMARY> db.coll.createIndex({"_ts":1}, {expireAfterSeconds: 10})
```

Der obige Befehl bewirkt das Löschen beliebiger Dokumente in der Sammlung ```db.coll```, die innerhalb der letzten zehn Sekunden nicht geändert wurden.

> [!NOTE]
> **_ts** ist ein Cosmos DB-spezifisches Feld, auf das über MongoDB-Clients nicht zugegriffen werden kann. Dies ist eine reservierte Eigenschaft (Systemeigenschaft), die den Zeitstempel der letzten Änderung eines Dokuments enthält.

## <a name="migrating-collections-with-indexes"></a>Migrieren von Sammlungen mit Indizes

Derzeit ist die Erstellung von eindeutigen Indizes nur möglich, wenn die Sammlung keine Dokumente enthält. Bei gängigen MongoDB-Migrationstools wird versucht, die eindeutigen Indizes nach dem Importieren der Daten zu erstellen. Zur Problemumgehung wird empfohlen, dass Benutzer die entsprechenden Sammlungen und eindeutigen Indizes manuell erstellen, anstatt das Migrationstool zuzulassen. (Für ```mongorestore``` wird dieses Verhalten erreicht, indem in der Befehlszeile das Flag `--noIndexRestore` verwendet wird.)

## <a name="next-steps"></a>Nächste Schritte

* [Indizierung in Azure Cosmos DB](../cosmos-db/index-policy.md)
* [Festlegen einer Gültigkeitsdauer für den automatischen Ablauf von Daten in Azure Cosmos DB](../cosmos-db/time-to-live.md)
