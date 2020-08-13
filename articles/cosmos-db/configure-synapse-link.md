---
title: Konfigurieren und Verwenden von Azure Synapse Link für Azure Cosmos DB (Vorschau)
description: Es wird beschrieben, wie Sie Synapse Link für Azure Cosmos-Konten aktivieren, einen Container mit aktiviertem Analysespeicher erstellen, für die Azure Cosmos-Datenbank eine Verbindung mit dem Synapse-Arbeitsbereich herstellen und Abfragen durchführen.
author: Rodrigossz
ms.service: cosmos-db
ms.topic: how-to
ms.date: 05/19/2020
ms.author: rosouz
ms.openlocfilehash: 9499fe2140f4a345d48bce6ef010989cfc22c58e
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2020
ms.locfileid: "88037081"
---
# <a name="configure-and-use-azure-synapse-link-for-azure-cosmos-db-preview"></a>Konfigurieren und Verwenden von Azure Synapse Link für Azure Cosmos DB (Vorschau)

Synapse Link für Azure Cosmos DB ist eine cloudnative hybride Verarbeitungsfunktion für Transaktionen und Analysen (Hybrid Transactional and Analytical Processing, HTAP), mit der Sie in Azure Cosmos DB in Quasi-Echtzeit Analysen für operative Daten ausführen können. Synapse Link sorgt für eine nahtlose Integration zwischen Azure Cosmos DB und Azure Synapse Analytics.


> [!IMPORTANT]
> Stellen Sie zur Verwendung von Azure Synapse Link sicher, dass Sie Ihr Azure Cosmos-Konto und Ihren Azure Synapse Analytics-Arbeitsbereich in einer der oben angegebenen Regionen bereitstellen. Eine Liste mit unterstützten Regionen finden Sie auf der [Seite mit den Updateinformationen für Azure-Dienste](https://azure.microsoft.com/updates/). 

Führen Sie die folgenden Schritte aus, um mit Synapse Link für Azure Cosmos DB Analyseabfragen durchzuführen:

* [Aktivieren von Synapse Link für Ihre Azure Cosmos-Konten](#enable-synapse-link)
* [Erstellen eines für einen Analysespeicher aktivierten Azure Cosmos-Containers](#create-analytical-ttl)
* [Herstellen einer Verbindung mit einem Synapse-Arbeitsbereich für Ihre Azure Cosmos-Datenbank](#connect-to-cosmos-database)
* [Abfragen des Analysespeichers mit Synapse Spark](#query-analytical-store)

## <a name="enable-azure-synapse-link-for-azure-cosmos-accounts"></a><a id="enable-synapse-link"></a>Aktivieren von Azure Synapse Link für Azure Cosmos-Konten

### <a name="azure-portal"></a>Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. [Erstellen Sie ein neues Azure-Konto](create-sql-api-dotnet.md#create-account), oder wählen Sie ein vorhandenes Azure Cosmos-Konto aus.

1. Navigieren Sie zu Ihrem Azure Cosmos-Konto, und öffnen Sie den Bereich **Features**.

1. Wählen Sie in der Liste mit den Features die Option **Synapse Link** aus.

   :::image type="content" source="./media/configure-synapse-link/find-synapse-link-feature.png" alt-text="Suchen nach der Previewfunktion „Synapse Link“":::

1. Als Nächstes werden Sie aufgefordert, Synapse Link für Ihr Konto zu aktivieren. Wählen Sie „Aktivieren“ aus.

   :::image type="content" source="./media/configure-synapse-link/enable-synapse-link-feature.png" alt-text="Aktivieren des Features „Synapse Link“":::

1. Ihr Konto ist jetzt für die Verwendung von Synapse Link aktiviert. Im nächsten Abschnitt wird beschrieben, wie Sie für Analysespeicher aktivierte Container erstellen, um mit dem automatischen Replizieren Ihrer operativen Daten aus dem Transaktionsspeicher im Analysespeicher zu beginnen.

### <a name="azure-resource-manager-template"></a>Azure Resource Manager-Vorlage

Mit der [Azure Resource Manager-Vorlage](manage-sql-with-resource-manager.md#azure-cosmos-account-with-analytical-store) wird für die SQL-API ein Azure Cosmos-Konto erstellt, das Synapse Link-fähig ist. Mit dieser Vorlage wird ein Konto für die Core-API (SQL) in einer Region mit einem Container erstellt, für den die analytische Gültigkeitsdauer aktiviert ist, und es ist eine Option vorhanden, mit der die manuelle oder automatische Skalierung des Durchsatzes festgelegt werden kann. Klicken Sie zum Bereitstellen dieser Vorlage auf der Infoseite auf **Bereitstellen in Azure**.

## <a name="create-an-azure-cosmos-container-with-analytical-store"></a><a id="create-analytical-ttl"></a> Erstellen eines Azure Cosmos-Containers mit Analysespeicher

Beim Erstellen des Containers können Sie den Analysespeicher in einem Azure Cosmos-Container aktivieren. Sie können das Azure-Portal verwenden oder die Eigenschaft `analyticalTTL` bei der Erstellung des Containers konfigurieren, indem Sie die Azure Cosmos DB-SDKs nutzen.

> [!NOTE]
> Derzeit können Sie den Analysespeicher für **neue** Container aktivieren (sowohl in neuen als auch in vorhandenen Konten).

### <a name="azure-portal"></a>Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) oder beim [Azure Cosmos-Explorer](https://cosmos.azure.com/) an.

1. Navigieren Sie zu Ihrem Azure Cosmos-Konto, und öffnen Sie die Registerkarte **Daten-Explorer**.

1. Wählen Sie **Neuer Container** aus, und geben Sie einen Namen für Ihre Datenbank, den Container, den Partitionsschlüssel und die Durchsatzdetails ein. Aktivieren Sie die Option **Analysespeicher**. Nach dem Aktivieren des Analysespeichers wird ein Container erstellt, für den die Eigenschaft `AnalyicalTTL` auf den Standardwert „-1“ (unbegrenzte Aufbewahrung) festgelegt ist. In diesem Analysespeicher wird der gesamte Versionsverlauf der Datensätze aufbewahrt.

   :::image type="content" source="./media/configure-synapse-link/create-container-analytical-store.png" alt-text="Aktivieren des Analysespeichers für Azure Cosmos-Container":::

1. Falls Sie Synapse Link für dieses Konto bisher noch nicht aktiviert haben, werden Sie dazu aufgefordert. Der Grund ist, dass die Erstellung eines für Analysespeicher aktivierten Containers zwingend erforderlich ist. Wählen Sie **Enable Synapse Link** (Synapse Link aktivieren) aus, wenn Sie dazu aufgefordert werden.

1. Wählen Sie **OK** aus, um einen für Analysespeicher aktivierten Azure Cosmos-Container zu erstellen.

### <a name="net-sdk"></a>.NET SDK

Mit dem folgenden Code wird ein Container mit Analysespeicher erstellt, indem das .NET SDK verwendet wird. Legen Sie die Eigenschaft für die analytische Gültigkeitsdauer auf den erforderlichen Wert fest. Eine Liste mit den zulässigen Werten finden Sie im Artikel zu den [für die analytische Gültigkeitsdauer unterstützten Werten](analytical-store-introduction.md#analytical-ttl):

```csharp
// Create a container with a partition key, and analytical TTL configured to  -1 (infinite retention)
string containerId = “myContainerName”;
int analyticalTtlInSec = -1;
ContainerProperties cpInput = new ContainerProperties()
            {
Id = containerId,
PartitionKeyPath = "/id",
AnalyticalStorageTimeToLiveInSeconds = analyticalTtlInSec,
};
 await this. cosmosClient.GetDatabase("myDatabase").CreateContainerAsync(cpInput);
```

### <a name="java-v4-sdk"></a>Java V4 SDK

Mit dem folgenden Code wird ein Container mit Analysespeicher erstellt, indem das Java V4 SDK verwendet wird. Legen Sie die Eigenschaft `AnalyticalStoreTimeToLiveInSeconds` auf den erforderlichen Wert fest:

```java
// Create a container with a partition key and  analytical TTL configured to  -1 (infinite retention) 
CosmosContainerProperties containerProperties = new CosmosContainerProperties("myContainer", "/myPartitionKey");

containerProperties.setAnalyticalStoreTimeToLiveInSeconds(-1);

container = database.createContainerIfNotExists(containerProperties, 400).block().getContainer();
```

### <a name="python-v3-sdk"></a>Python V3 SDK

Mit dem folgenden Code wird ein Container mit Analysespeicher erstellt, indem das Python SDK verwendet wird:

```python
import azure.cosmos.cosmos_client as cosmos_client
def create_collection_if_not_exists(cosmosEndpoint, cosmosKey, databaseName, collectionName):
    client = cosmos_client.CosmosClient(url_connection=cosmosEndpoint, auth={'masterKey': cosmosKey})

db = client.QueryDatabases("select * from c where c.id = '" + databaseName + "'").fetch_next_block()[0]
options = {
    'offerThroughput': 1000
}

container_definition = {
    'id': collectionName,
    "partitionKey": {  
        "paths": [  
        "/id"  
        ],  
        "kind": "Hash" 
    },
    "indexingPolicy": {  
    "indexingMode": "consistent",  
    "automatic": True,  
    "includedPaths": [],  
    "excludedPaths": [{
        "path": "/*"
    }]  
    },
    "defaultTtl": -1,
    "analyticalStorageTtl": -1
}

container = client.CreateContainer(db['_self'], container_definition, options)
```

### <a name="update-the-analytical-store-time-to-live"></a><a id="update-analytical-ttl"></a> Aktualisieren der Gültigkeitsdauer für den Analysespeicher

Nachdem der Analysespeicher mit einem bestimmten Wert für die Gültigkeitsdauer aktiviert wurde, können Sie ihn später auf einen anderen gültigen Wert aktualisieren. Sie können den Wert über das Azure-Portal oder mit SDKs aktualisieren. Informationen zu den verschiedenen Konfigurationsoptionen für die analytische Gültigkeitsdauer finden Sie im Artikel zu den [für die analytische Gültigkeitsdauer unterstützten Werten](analytical-store-introduction.md#analytical-ttl).

#### <a name="azure-portal"></a>Azure-Portal

Wenn Sie einen für Analysespeicher aktivierten Container über das Azure-Portal erstellt haben, weist dieser für die analytische Gültigkeitsdauer den Standardwert „-1“ auf. Verwenden Sie die folgenden Schritte, um diesen Wert zu aktualisieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) oder beim [Azure Cosmos-Explorer](https://cosmos.azure.com/) an.

1. Navigieren Sie zu Ihrem Azure Cosmos-Konto, und öffnen Sie die Registerkarte **Daten-Explorer**.

1. Wählen Sie einen vorhandenen Container aus, für den Analysespeicher aktiviert ist. Erweitern Sie ihn, und ändern Sie die folgenden Werte:

  * Öffnen Sie das Fenster **Scale & Settings** (Skalierung und Einstellungen).
  * Suchen Sie unter **Einstellung** nach **Analytical Storage Time to Live** (Gültigkeitsdauer für Analysespeicher).
  * Wählen Sie **Ein (Standard)** aus, oder wählen Sie **Ein** aus, und legen Sie einen Wert für die Gültigkeitsdauer fest.
  * Klicken Sie zum Speichern der Änderungen auf **Speichern**.

#### <a name="net-sdk"></a>.NET SDK

Der folgende Code veranschaulicht, wie Sie die Gültigkeitsdauer für Analysespeicher aktualisieren, indem Sie das .NET SDK verwenden:

```csharp
// Get the container, update AnalyticalStorageTimeToLiveInSeconds 
ContainerResponse containerResponse = await client.GetContainer("database", "container").ReadContainerAsync();
// Update analytical store TTL
containerResponse.Resource. AnalyticalStorageTimeToLiveInSeconds = 60 * 60 * 24 * 180  // Expire analytical store data in 6 months;
await client.GetContainer("database", "container").ReplaceContainerAsync(containerResponse.Resource);
```

#### <a name="java-v4-sdk"></a>Java V4 SDK

Der folgende Code veranschaulicht, wie Sie die Gültigkeitsdauer für Analysespeicher aktualisieren, indem Sie das Java V4 SDK verwenden:

```java
CosmosContainerProperties containerProperties = new CosmosContainerProperties("myContainer", "/myPartitionKey");

// Update analytical store TTL to expire analytical store data in 6 months;
containerProperties.setAnalyticalStoreTimeToLiveInSeconds (60 * 60 * 24 * 180 );  
 
// Update container settings
container.replace(containerProperties).block();
```

## <a name="connect-to-a-synapse-workspace"></a><a id="connect-to-cosmos-database"></a> Herstellen einer Verbindung mit einem Synapse-Arbeitsbereich

Befolgen Sie die Anleitung unter [Herstellen einer Verbindung mit Azure Synapse Link](../synapse-analytics/synapse-link/how-to-connect-synapse-link-cosmos-db.md), um aus Azure Synapse Analytics Studio mit Azure Synapse Link auf eine Azure Cosmos DB-Datenbank zuzugreifen.

## <a name="query-using-synapse-spark"></a><a id="query-analytical-store"></a> Durchführen von Abfragen mit Synapse Spark

Befolgen Sie die Anleitung im Artikel [Abfragen des Azure Cosmos DB-Analysespeichers](../synapse-analytics/synapse-link/how-to-query-analytical-store-spark.md), um mit Synapse Spark Abfragen durchzuführen. Dieser Artikel enthält einige Beispiele dafür, wie Sie mithilfe von Synapse-Gesten mit dem Analysespeicher interagieren können. Diese Gesten werden angezeigt, wenn Sie mit der rechten Maustaste auf einen Container klicken. Mit Gesten können Sie schnell Code generieren und an Ihre Anforderungen anpassen. Sie eignen sich auch ideal zum Ermitteln von Daten mit nur einem Mausklick.

## <a name="getting-started-with-azure-synpase-link---samples"></a><a id="cosmosdb-synapse-link-samples"></a> Erste Schritte mit Azure Synpase Link – Beispiele

Beispiele für die ersten Schritte mit Azure Synapse Link finden Sie auf [GitHub](https://aka.ms/cosmosdb-synapselink-samples). Diese zeigen End-to-End-Lösungen für IoT- und Einzelhandelsszenarien.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Azure Synapse Link für Azure Cosmos DB](synapse-link.md)

* [Übersicht über den Azure Cosmos DB-Analysespeicher](analytical-store-introduction.md)

* [Häufig gestellte Fragen zu Azure Synapse Link für Azure Cosmos DB](synapse-link-frequently-asked-questions.md)

* [Grundlegende Konzepte für Apache Spark in Azure Synapse Analytics](../synapse-analytics/spark/apache-spark-concepts.md)

* [SQL On-Demand (Vorschauversion) in Azure Synapse Analytics](../synapse-analytics/sql/on-demand-workspace-overview.md)
