---
title: Bereitstellen des Datenbankdurchsatzes in Azure Cosmos DB
description: Hier erfahren Sie, wie Sie Durchsatz auf Datenbankebene in Azure Cosmos DB mit dem Azure-Portal, mit der CLI, mit PowerShell und mit verschiedenen anderen SDKs bereitstellen.
author: markjbrown
ms.service: cosmos-db
ms.topic: how-to
ms.date: 09/28/2019
ms.author: mjbrown
ms.custom: devx-track-azurecli
ms.openlocfilehash: a63e5a8b59c1e043b4af7b35c34cde6be0314d22
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87497017"
---
# <a name="provision-standard-manual-throughput-on-a-database-in-azure-cosmos-db"></a>Bereitstellen von Standarddurchsatz (manuell) für eine Datenbank in Azure Cosmos DB

In diesem Artikel erfahren Sie, wie Sie Standarddurchsatz (manuell) für eine Datenbank in Azure Cosmos DB bereitstellen. Sie können Durchsatz für einen einzelnen [Container](how-to-provision-container-throughput.md) oder für eine Datenbank bereitstellen und den Durchsatz für die darin enthaltenen Container freigeben. Wenn Sie erfahren möchten, wann Durchsatz auf Containerebene und wann er auf Datenbankebene verwendet wird, lesen Sie den Artikel [Anwendungsfälle für die Bereitstellung von Durchsatz für Container und Datenbanken](set-throughput.md). Durchsatz auf Datenbankebene kann über das Azure-Portal oder über Azure Cosmos DB SDKs bereitgestellt werden.

## <a name="provision-throughput-using-azure-portal"></a>Bereitstellen des Durchsatzes mithilfe des Azure-Portals

### <a name="sql-core-api"></a><a id="portal-sql"></a>SQL-API (Core-API)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. [Erstellen Sie ein neues Azure Cosmos-Konto](create-sql-api-dotnet.md#create-account), oder wählen Sie ein bereits vorhandenes Azure Cosmos-Konto aus.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie **Neue Datenbank** aus. Geben Sie die folgenden Details an:

   * Geben Sie eine Datenbank-ID ein.
   * Aktivieren Sie das Kontrollkästchen **Durchsatz bereitstellen**.
   * Geben Sie einen Durchsatz ein (beispielsweise 1.000 RUs).
   * Klicken Sie auf **OK**.

    :::image type="content" source="./media/how-to-provision-database-throughput/provision-database-throughput-portal-all-api.png" alt-text="Screenshot: Dialogfeld „Neue Datenbank“":::

## <a name="provision-throughput-using-azure-cli-or-powershell"></a>Bereitstellen von Durchsatz über Azure CLI oder PowerShell

Weitere Informationen zum Erstellen einer Datenbank mit gemeinsamem Durchsatz finden Sie unter

* [Erstellen einer Datenbank über Azure CLI](manage-with-cli.md#create-a-database-with-shared-throughput)
* [Erstellen einer Azure Cosmos DB-Datenbank mit gemeinsam genutztem Durchsatz](manage-with-powershell.md#create-db-ru)

## <a name="provision-throughput-using-net-sdk"></a>Bereitstellen des Durchsatzes mithilfe des .NET SDK

> [!Note]
> Sie können Cosmos SDKs für die SQL-API verwenden, um Durchsatz für alle APIs bereitzustellen. Sie können optional auch das folgende Beispiel für die Cassandra-API verwenden.

### <a name="all-apis"></a><a id="dotnet-all"></a>Alle APIs

# <a name="net-sdk-v2"></a>[.NET SDK V2](#tab/dotnetv2)

```csharp
//set the throughput for the database
RequestOptions options = new RequestOptions
{
    OfferThroughput = 500
};

//create the database
await client.CreateDatabaseIfNotExistsAsync(
    new Database {Id = databaseName},  
    options);
```

# <a name="net-sdk-v3"></a>[.NET SDK V3](#tab/dotnetv3)

[!code-csharp[](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos/tests/Microsoft.Azure.Cosmos.Tests/SampleCodeForDocs/DatabaseDocsSampleCode.cs?name=DatabaseCreateWithThroughput)]

---

### <a name="cassandra-api"></a><a id="dotnet-cassandra"></a>Cassandra-API

Ein ähnlicher Befehl kann über einen beliebigen CQL-kompatiblen Treiber ausgeführt werden.

```csharp
// Create a Cassandra keyspace and provision throughput of 400 RU/s
session.Execute("CREATE KEYSPACE IF NOT EXISTS myKeySpace WITH cosmosdb_provisioned_throughput=400");
```
 
## <a name="next-steps"></a>Nächste Schritte

Informationen zum Bereitstellen des Durchsatzes in Azure Cosmos DB finden Sie in den folgenden Artikeln:

* [Globales Skalieren von bereitgestelltem Durchsatz](scaling-throughput.md)
* [Bereitstellen des Durchsatzes für Container und Datenbanken](set-throughput.md)
* [Bereitstellen von Standarddurchsatz (manuell) für einen Azure Cosmos-Container](how-to-provision-container-throughput.md)
* [Bereitstellen von automatisch skalierendem Durchsatz für eine Datenbank oder einen Container in Azure Cosmos DB](how-to-provision-autoscale-throughput.md)
* [Durchsatz und Anforderungseinheiten in Azure Cosmos DB](request-units.md)
