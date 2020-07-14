---
title: Bereitstellen des Containerdurchsatzes in Azure Cosmos DB
description: Erfahren Sie, wie Sie Durchsatz auf Containerebene in Azure Cosmos DB mit dem Azure-Portal, mit der CLI, mit PowerShell und mit verschiedenen anderen SDKs bereitstellen.
author: markjbrown
ms.service: cosmos-db
ms.topic: how-to
ms.date: 12/13/2019
ms.author: mjbrown
ms.openlocfilehash: 9167df9c763f4004324a3435ba1a2b0fd0171ac4
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85851680"
---
# <a name="provision-standard-manual-throughput-on-an-azure-cosmos-container"></a>Bereitstellen von Standarddurchsatz (manuell) für einen Azure Cosmos-Container

In diesem Artikel erfahren Sie, wie Sie Standarddurchsatz (manuell) für einen Container (Sammlung, Diagramm oder Tabelle) in Azure Cosmos DB bereitstellen. Sie können Durchsatz für einen einzelnen Container oder [für eine Datenbank](how-to-provision-database-throughput.md) bereitstellen und ihn für die in der Datenbank enthaltenen Container freigeben. Der Durchsatz für einen Container kann über das Azure-Portal, über die Azure-Befehlszeilenschnittstelle oder mithilfe der Azure Cosmos DB SDKs bereitgestellt werden.

## <a name="azure-portal"></a>Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. [Erstellen Sie ein neues Azure Cosmos-Konto](create-sql-api-dotnet.md#create-account), oder wählen Sie ein bereits vorhandenes Azure Cosmos-Konto aus.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie **Neue Sammlung** aus. Geben Sie anschließend die folgenden Details an:

   * Geben Sie an, ob Sie eine neue Datenbank erstellen oder eine vorhandene Datenbank verwenden.
   * Geben Sie die ID eines Containers (oder einer Tabelle oder eines Diagramms) ein.
   * Geben Sie einen Partitionsschlüsselwert ein (etwa `/userid`).
   * Geben Sie den bereitzustellenden Durchsatz an (etwa 1.000 RUs).
   * Klicken Sie auf **OK**.

    :::image type="content" source="./media/how-to-provision-container-throughput/provision-container-throughput-portal-all-api.png" alt-text="Screenshot des Daten-Explorers mit hervorgehobener Option „Neue Sammlung“":::

## <a name="azure-cli-or-powershell"></a>Azure CLI oder PowerShell

Informationen zum Erstellen eines Containers mit dediziertem Durchsatz finden Sie unter

* [Erstellen eines Containers über die Azure-Befehlszeilenschnittstelle](manage-with-cli.md#create-a-container)
* [Erstellen eines Containers mithilfe von PowerShell](manage-with-powershell.md#create-container)

> [!Note]
> Verwenden Sie beim Bereitstellen des Durchsatzes für einen Container in einem Azure Cosmos-Konto, das mit der Azure Cosmos DB-API für MongoDB konfiguriert wurde, `/myShardKey` als Partitionsschlüsselpfad. Verwenden Sie beim Bereitstellen des Durchsatzes für einen Container in einem Azure Cosmos-Konto, das mit der Cassandra-API konfiguriert wurde, `/myPrimaryKey` als Partitionsschlüsselpfad.

## <a name="net-sdk"></a>.NET SDK

> [!Note]
> Verwenden Sie die Cosmos SDKs für die SQL-API, um Durchsatz für alle Cosmos DB-APIs (mit Ausnahme der Cassandra- und MongoDB-API) bereitzustellen.

### <a name="sql-gremlin-and-table-apis"></a><a id="dotnet-most"></a>SQL-, Gremlin- und Tabellen-API

# <a name="net-sdk-v2"></a>[.NET SDK V2](#tab/dotnetv2)

```csharp
// Create a container with a partition key and provision throughput of 400 RU/s
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 400 });
```

# <a name="net-sdk-v3"></a>[.NET SDK V3](#tab/dotnetv3)

[!code-csharp[](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos/tests/Microsoft.Azure.Cosmos.Tests/SampleCodeForDocs/ContainerDocsSampleCode.cs?name=ContainerCreateWithThroughput)]

---

## <a name="javascript-sdk"></a>JavaScript SDK

```javascript
// Create a new Client
const client = new CosmosClient({ endpoint, key });

// Create a database
const { database } = await client.databases.createIfNotExists({ id: "databaseId" });

// Create a container with the specified throughput
const { resource } = await database.containers.createIfNotExists({
id: "containerId",
throughput: 1000
});

// To update an existing container or databases throughput, you need to user the offers API
// Get all the offers
const { resources: offers } = await client.offers.readAll().fetchAll();

// Find the offer associated with your container or the database
const offer = offers.find((_offer) => _offer.offerResourceId === resource._rid);

// Change the throughput value
offer.content.offerThroughput = 2000;

// Replace the offer.
await client.offer(offer.id).replace(offer);
```

### <a name="mongodb-api"></a><a id="dotnet-mongodb"></a>MongoDB-API

```csharp
// refer to MongoDB .NET Driver
// https://docs.mongodb.com/drivers/csharp

// Create a new Client
String mongoConnectionString = "mongodb://DBAccountName:Password@DBAccountName.documents.azure.com:10255/?ssl=true&replicaSet=globaldb";
mongoUrl = new MongoUrl(mongoConnectionString);
mongoClientSettings = MongoClientSettings.FromUrl(mongoUrl);
mongoClient = new MongoClient(mongoClientSettings);

// Change the database name
mongoDatabase = mongoClient.GetDatabase("testdb");

// Change the collection name, throughput value then update via MongoDB extension commands
// https://docs.microsoft.com/en-us/azure/cosmos-db/mongodb-custom-commands#update-collection

var result = mongoDatabase.RunCommand<BsonDocument>(@"{customAction: ""UpdateCollection"", collection: ""testcollection"", offerThroughput: 400}");
```

### <a name="cassandra-api"></a><a id="dotnet-cassandra"></a>Cassandra-API

Ähnliche Befehle können über einen beliebigen CQL-konformen Treiber ausgegeben werden.

```csharp
// Create a Cassandra table with a partition (primary) key and provision throughput of 400 RU/s
session.Execute("CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=400");

```
### <a name="alter-or-change-throughput-for-cassandra-table"></a>Ändern des Durchsatzes für Cassandra-Tabellen

```csharp
// Altering the throughput too can be done through code by issuing following command
session.Execute("ALTER TABLE myKeySpace.myTable WITH cosmosdb_provisioned_throughput=5000");
```


## <a name="next-steps"></a>Nächste Schritte

Informationen zur Durchsatzbereitstellung in Azure Cosmos DB finden Sie in den folgenden Artikeln:

* [Bereitstellen von Standarddurchsatz (manuell) für eine Datenbank](how-to-provision-database-throughput.md)
* [Bereitstellen von automatisch skaliertem Durchsatz für eine Datenbank](how-to-provision-autoscale-throughput.md)
* [Durchsatz und Anforderungseinheiten in Azure Cosmos DB](request-units.md)
