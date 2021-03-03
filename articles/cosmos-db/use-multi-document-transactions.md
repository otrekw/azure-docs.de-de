---
title: Verwenden von Transaktionen mit mehreren Dokumenten in der Azure Cosmos DB-API für MongoDB
description: Hier erfahren Sie, wie Sie eine Mongo-Shell-Beispiel-App erstellen, die eine Transaktion mit mehreren Dokumenten (Alles-oder-Nichts-Semantik) für eine feste Sammlung in der Azure Cosmos DB-API für MongoDB 4.0 ausführen kann.
author: gahl-levy
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: how-to
ms.date: 03/02/2021
ms.author: gahllevy
ms.openlocfilehash: 4d7dcc829f25b7f1b7c6cb6b1d13a664d301bfe6
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678460"
---
# <a name="use-multi-document-transactions-in-azure-cosmos-db-api-for-mongodb"></a>Verwenden von Transaktionen mit mehreren Dokumenten in der Azure Cosmos DB-API für MongoDB
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

In diesem Artikel erstellen Sie eine Mongo-Shell-App, die eine Transaktion mit mehreren Dokumenten für eine feste Sammlung in der Azure Cosmos DB-API für MongoDB mit Serverversion 4.0 ausführt.

## <a name="what-are-multi-document-transactions"></a>Was sind Transaktionen mit mehreren Dokumenten?

In der Azure Cosmos DB-API für MongoDB sind Vorgänge für ein einzelnes Dokument unteilbar (atomar). Transaktionen mit mehreren Dokumenten ermöglichen es Anwendungen, atomare Vorgänge für mehrere Dokumente auszuführen. Sie bieten eine „Alles-oder-Nichts“-Semantik für die Vorgänge. Beim Commit werden die in den Transaktionen vorgenommenen Änderungen persistent gespeichert. Falls die Transaktion fehlschlägt, werden alle Änderungen innerhalb der Transaktion verworfen.

Transaktionen mit mehreren Dokumenten folgen der **ACID**-Semantik:

* Atomarität: Alle Vorgänge werden als einer behandelt.
* Konsistenz: Daten, für die ein Commit ausgeführt wurde, sind gültig.
* Isolation: Von anderen Vorgängen isoliert.
* Dauerhaftigkeit: Transaktionsdaten werden persistent gespeichert, wenn der Client entsprechend angewiesen wird.

## <a name="requirements"></a>Anforderungen

Transaktionen mit mehreren Dokumenten werden in einer Sammlung ohne Shards in Version 4.0 unterstützt. Transaktionen mit mehreren Dokumenten werden nicht sammlungsübergreifend oder in Sammlungen mit Shards unterstützt.

Alle Treiber, die Wire Protocol-Version 4.0 oder höher unterstützen, bieten auch Unterstützung für Transaktionen mit mehreren Dokumenten in der Azure Cosmos DB-API für MongoDB.

## <a name="run-multi-document-transactions-in-mongodb-shell"></a>Ausführen von Transaktionen mit mehreren Dokumenten in der MongoDB-Shell

1. Öffnen Sie eine Eingabeaufforderung, und navigieren Sie zu dem Verzeichnis, in dem die Mongo-Shell, Version 4.0 oder höher, installiert ist:

   ```powershell
   cd <path_to_mongo_shell_>
   ```

2. Erstellen Sie ein Mongo-Shellskript *connect_friends.js*, und fügen Sie den folgenden Inhalt hinzu.

   ```javascript
   // insert data into friends collection
   db.getMongo().getDB("users").friends.insert({name:"Tom"})
   db.getMongo().getDB("users").friends.insert({name:"Mike"})
   // start transaction
   var session = db.getMongo().startSession();
   var friendsCollection = session.getDatabase("users").friends;
   session.startTransaction();
   // operations in transaction
   try {
       friendsCollection.updateOne({ name: "Tom" }, { $set: { friendOf: "Mike" } } );
       friendsCollection.updateOne({ name: "Mike" }, { $set: { friendOf: "Tom" } } );
   } catch (error) {
       // abort transaction on error
       session.abortTransaction();
       throw error;
   }

    // commit transaction
    session.commitTransaction();

    ```

3. Führen Sie den folgenden Befehl aus, um die Transaktion mit mehreren Dokumenten auszuführen. Host, Port, Benutzer und Schlüssel finden Sie im Azure-Portal.

   ```powershell
   mongo "<HOST>:<PORT>" -u "<USER>" -p "KEY" --ssl connect_friends.js
   ```

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die Neuerungen in der [Azure Cosmos DB-API für MongoDB 4.0](mongodb-feature-support-40.md).
