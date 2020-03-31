---
title: Änderungsdatenströme in der API für MongoDB von Azure Cosmos-DB
description: Erfahren Sie, wie Sie Änderungsdatenströme in der API für MongoDB von Azure Cosmos DB verwenden, um die an Ihren Daten vorgenommenen Änderungen abzurufen.
author: srchi
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 11/16/2019
ms.author: srchi
ms.openlocfilehash: ec1ec1a8a80953f8988355341ee7128bd29b982d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77467776"
---
# <a name="change-streams-in-azure-cosmos-dbs-api-for-mongodb"></a>Änderungsdatenströme in der API für MongoDB von Azure Cosmos-DB

Die Unterstützung von [Änderungsfeeds](change-feed.md) in der API für MongoDB von Azure Cosmos DB ist über die API für Änderungsdatenströme verfügbar. Mithilfe der API für Änderungsdatenströme können Ihre Anwendungen die Änderungen an der Sammlung oder an den Elementen in einem einzelnen Shard abrufen. Später können Sie auf der Grundlage der Ergebnisse weitere Maßnahmen ergreifen. Änderungen an den Elementen in der Sammlung werden in der Reihenfolge ihres Änderungszeitpunkts erfasst und die Sortierreihenfolge ist für die einzelnen Shardschlüssel sichergestellt.

> [!NOTE]
> Um Änderungsdatenströme zu verwenden, erstellen Sie das Konto mit Version 3.6 der API für MongoDB von Azure Cosmos DB oder einer höheren Version. Wenn Sie die Änderungsdatenstrom-Beispiele für eine frühere Version ausführen, wird möglicherweise die Fehlermeldung `Unrecognized pipeline stage name: $changeStream` angezeigt. 

Das folgende Beispiel zeigt, wie Sie Änderungsdatenströme für alle Elemente der Sammlung abrufen können. In diesem Beispiel wird ein Cursor erstellt, um Elemente zu überwachen, wenn sie eingefügt, aktualisiert oder ersetzt werden. Die Phase „$match“, „$project“ und die Option „fullDocument“ sind erforderlich, um die Änderungsdatenströme abzurufen. Die Überwachung auf Löschvorgänge mit Änderungsdatenströmen wird derzeit nicht unterstützt. Als Problemumgehung können Sie den zu löschenden Elementen eine schwache Markierung hinzufügen. Sie können z. B. ein Attribut zum Element „deleted“ (Gelöscht) hinzufügen und es auf „true“ festlegen sowie eine Gültigkeitsdauer (TTL) für das Element einstellen, damit Sie es sowohl automatisch löschen als auch nachverfolgen können.

```javascript
var cursor = db.coll.watch(
    [
        { $match: { "operationType": { $in: ["insert", "update", "replace"] } } },
        { $project: { "_id": 1, "fullDocument": 1, "ns": 1, "documentKey": 1 } }
    ],
    { fullDocument: "updateLookup" });

while (!cursor.isExhausted()) {
    if (cursor.hasNext()) {
        printjson(cursor.next());
    }
}
```

Das folgende Beispiel zeigt, wie Sie Änderungen an den Elementen in einem einzelnen Shard abrufen können. In diesem Beispiel werden die Änderungen von Elementen abgerufen, deren ShardsSchlüssel gleich „a“ und der Shardschlüsselwert gleich „1“ ist.

```javascript
var cursor = db.coll.watch(
    [
        { 
            $match: { 
                $and: [
                    { "fullDocument.a": 1 }, 
                    { "operationType": { $in: ["insert", "update", "replace"] } }
                ]
            }
        },
        { $project: { "_id": 1, "fullDocument": 1, "ns": 1, "documentKey": 1} }
    ],
    { fullDocument: "updateLookup" });

```

## <a name="current-limitations"></a>Aktuelle Einschränkungen

Es gelten die folgenden Einschränkungen, wenn Änderungsdatenströme verwendet werden:

* Die Eigenschaften `operationType` und `updateDescription` werden im Ausgabedokument noch nicht unterstützt.
* Die Vorgangstypen `insert`, `update` und `replace` werden derzeit unterstützt. Löschvorgänge oder andere Ereignisse werden noch nicht unterstützt.

Aufgrund dieser Einschränkungen sind die Phasen „$match“, „$project“ und die „fullDocument“-Optionen erforderlich, wie in den vorherigen Beispielen gezeigt.

## <a name="error-handling"></a>Fehlerbehandlung

Die folgenden Fehlercodes und Meldungen werden bei der Verwendung von Änderungsdatenströmen unterstützt:

* **HTTP-Fehlercode 429**: Wenn der Änderungsdatenstrom gedrosselt wird, wird eine leere Seite zurückgegeben.

* **NamespaceNotFound (OperationType Invalidate)** : Wenn Sie den Änderungsdatenstrom für die nicht existierende Sammlung ausführen oder die Sammlung verworfen wird, wird ein `NamespaceNotFound`-Fehler zurückgegeben. Da die `operationType`-Eigenschaft nicht im Ausgabedokument zurückgegeben werden kann, wird anstelle des Fehlers `operationType Invalidate` der Fehler `NamespaceNotFound` zurückgegeben.

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden der Gültigkeitsdauer für den automatischen Ablauf von Daten in der API für MongoDB von Azure Cosmos DB](mongodb-time-to-live.md)
* [Indizieren in der API für MongoDB von Azure Cosmos-DB](mongodb-indexing.md)
