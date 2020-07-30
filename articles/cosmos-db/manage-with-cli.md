---
title: Verwalten von Azure Cosmos DB-Ressourcen mit der Azure CLI
description: Verwenden Sie die Azure CLI zum Verwalten Ihrer Konten, Datenbanken und Containern für Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: how-to
ms.date: 06/03/2020
ms.author: mjbrown
ms.openlocfilehash: 97b5118f74cbd098beea804c312ed08f1a152873
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87067176"
---
# <a name="manage-azure-cosmos-resources-using-azure-cli"></a>Verwalten von Azure Cosmos-Ressourcen mit der Azure CLI

Der folgende Leitfaden erläutert die gängigen Befehle zum Automatisieren der Verwaltung Ihrer Azure Cosmos DB-Konten und -Container mithilfe von Azure CLI. Referenzseiten für alle CLI-Befehle für Azure Cosmos DB sind in der [Referenz zur Azure CLI](https://docs.microsoft.com/cli/azure/cosmosdb) verfügbar. Weitere Beispiele finden Sie auch in [Azur CLI-Beispiele für Azure Cosmos DB](cli-samples.md), einschließlich der Erstellung und Verwaltung von Cosmos DB-Konten, Datenbanken und Containern für MongoDB, Gremlin, Cassandra und die Tabellen-API.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, ist es für dieses Thema erforderlich, die Azure CLI-Version 2.6.0 oder höher auszuführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

## <a name="azure-cosmos-accounts"></a>Azure Cosmos DB-Konten

In den folgenden Abschnitten erfahren Sie, wie Sie Azure Cosmos DB-Konten verwalten. Dabei lernen Sie u. a. Folgendes:

* [Erstellen eines Azure Cosmos DB-Kontos](#create-an-azure-cosmos-db-account)
* [Hinzufügen oder Entfernen von Regionen](#add-or-remove-regions)
* [Aktivieren von Schreibvorgängen in mehreren Regionen](#enable-multiple-write-regions)
* [Festlegen der Priorität beim Regionsfailover](#set-failover-priority)
* [Aktivieren des automatischen Failovers](#enable-automatic-failover)
* [Auslösen eines manuellen Failovers](#trigger-manual-failover)
* [Auflisten von Kontoschlüsseln](#list-account-keys)
* [Auflisten schreibgeschützter Kontoschlüssel](#list-read-only-account-keys)
* [Verbindungszeichenfolgen auflisten](#list-connection-strings)
* [Erneutes Generieren eines Kontoschlüssels](#regenerate-account-key)

### <a name="create-an-azure-cosmos-db-account"></a>Erstellen eines Azure Cosmos DB-Kontos

Erstellen Sie ein Azure Cosmos DB-Konto mit SQL-API und Sitzungskonsistenz für die Regionen „USA, Westen 2“ und „USA, Osten 2“:

> [!IMPORTANT]
> Der Name des Azure Cosmos-Kontos muss in Kleinbuchstaben geschrieben sein und weniger als 44 Zeichen umfassen.

```azurecli-interactive
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount' #needs to be lower case and less than 44 characters

az cosmosdb create \
    -n $accountName \
    -g $resourceGroupName \
    --default-consistency-level Session \
    --locations regionName='West US 2' failoverPriority=0 isZoneRedundant=False \
    --locations regionName='East US 2' failoverPriority=1 isZoneRedundant=False
```

### <a name="add-or-remove-regions"></a>Hinzufügen oder Entfernen von Regionen

Erstellen Sie ein Azure Cosmos-Konto mit zwei Regionen, fügen Sie eine Region hinzu, und entfernen Sie eine Region.

> [!NOTE]
> Sie können nicht gleichzeitig Regionselemente vom Typ `locations` hinzufügen oder entfernen und andere Eigenschaften für ein Azure Cosmos-Konto ändern. Die Anpassung von Regionen muss getrennt von anderen Änderungen an der Kontoressource vorgenommen werden.
> [!NOTE]
> Mit diesem Befehl können Sie Regionen hinzufügen und entfernen, aber weder Failoverprioritäten anpassen noch ein manuelles Failover auslösen. Weitere Informationen finden Sie unter [Festlegen der Failoverpriorität](#set-failover-priority) und [Auslösen eines manuellen Failovers](#trigger-manual-failover).

```azurecli-interactive
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'

# Create an account with 2 regions
az cosmosdb create --name $accountName --resource-group $resourceGroupName \
    --locations regionName="West US 2" failoverPriority=0 isZoneRedundant=False \
    --locations regionName="East US 2" failoverPriority=1 isZoneRedundant=False

# Add a region
az cosmosdb update --name $accountName --resource-group $resourceGroupName \
    --locations regionName="West US 2" failoverPriority=0 isZoneRedundant=False \
    --locations regionName="East US 2" failoverPriority=1 isZoneRedundant=False \
    --locations regionName="South Central US" failoverPriority=2 isZoneRedundant=False

# Remove a region
az cosmosdb update --name $accountName --resource-group $resourceGroupName \
    --locations regionName="West US 2" failoverPriority=0 isZoneRedundant=False \
    --locations regionName="East US 2" failoverPriority=1 isZoneRedundant=False
```

### <a name="enable-multiple-write-regions"></a>Aktivieren mehrerer Schreibregionen

Aktivieren von Multimaster für ein Cosmos-Konto

```azurecli-interactive
# Update an Azure Cosmos account from single to multi-master
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'

# Get the account resource id for an existing account
accountId=$(az cosmosdb show -g $resourceGroupName -n $accountName --query id -o tsv)

az cosmosdb update --ids $accountId --enable-multiple-write-locations true
```

### <a name="set-failover-priority"></a>Festlegen der Failoverpriorität

Festlegen der Failoverpriorität für ein Azure Cosmos-Konto, das für automatische Failover konfiguriert ist

```azurecli-interactive
# Assume region order is initially 'West US 2'=0 'East US 2'=1 'South Central US'=2 for account
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'

# Get the account resource id for an existing account
accountId=$(az cosmosdb show -g $resourceGroupName -n $accountName --query id -o tsv)

# Make South Central US the next region to fail over to instead of East US 2
az cosmosdb failover-priority-change --ids $accountId \
    --failover-policies 'West US 2=0' 'South Central US=1' 'East US 2=2'
```

### <a name="enable-automatic-failover"></a>Aktivieren des automatischen Failovers

```azurecli-interactive
# Enable automatic failover on an existing account
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'

# Get the account resource id for an existing account
accountId=$(az cosmosdb show -g $resourceGroupName -n $accountName --query id -o tsv)

az cosmosdb update --ids $accountId --enable-automatic-failover true
```

### <a name="trigger-manual-failover"></a>Auslösen eines manuellen Failovers

> [!CAUTION]
> Wenn die Priorität einer Region in „0“ geändert wird, wird ein manuelles Failover für ein Azure Cosmos-Konto ausgelöst. Bei anderen Prioritätsänderungen wird kein Failover ausgelöst.

```azurecli-interactive
# Assume region order is initially 'West US 2=0' 'East US 2=1' 'South Central US=2' for account
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'

# Get the account resource id for an existing account
accountId=$(az cosmosdb show -g $resourceGroupName -n $accountName --query id -o tsv)

# Trigger a manual failover to promote East US 2 as new write region
az cosmosdb failover-priority-change --ids $accountId \
    --failover-policies 'East US 2=0' 'South Central US=1' 'West US 2=2'
```

### <a name="list-all-account-keys"></a><a id="list-account-keys"></a> Auflisten aller Kontoschlüssel

Rufen Sie alle Schlüssel für ein Cosmos-Konto ab.

```azurecli-interactive
# List all account keys
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'

az cosmosdb keys list \
   -n $accountName \
   -g $resourceGroupName
```

### <a name="list-read-only-account-keys"></a>Auflisten schreibgeschützter Kontoschlüssel

Rufen Sie schreibgeschützte Schlüssel für ein Cosmos-Konto ab.

```azurecli-interactive
# List read-only account keys
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'

az cosmosdb keys list \
    -n $accountName \
    -g $resourceGroupName \
    --type read-only-keys
```

### <a name="list-connection-strings"></a>Auflisten von Verbindungszeichenfolgen

Rufen Sie die Verbindungszeichenfolgen für ein Cosmos-Konto ab.

```azurecli-interactive
# List connection strings
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'

az cosmosdb keys list \
    -n $accountName \
    -g $resourceGroupName \
    --type connection-strings
```

### <a name="regenerate-account-key"></a>Erneutes Generieren eines Kontoschlüssels

Generieren Sie einen neuen Schlüssel für ein Cosmos-Konto erneut.

```azurecli-interactive
# Regenerate secondary account keys
# key-kind values: primary, primaryReadonly, secondary, secondaryReadonly
az cosmosdb keys regenerate \
    -n $accountName \
    -g $resourceGroupName \
    --key-kind secondary
```

## <a name="azure-cosmos-db-database"></a>Azure Cosmos DB-Datenbank

In den folgenden Abschnitten erfahren Sie, wie Sie Azure Cosmos DB-Datenbanken verwalten. Dabei lernen Sie u. a. Folgendes:

* [Erstellen einer Datenbank](#create-a-database)
* [Erstellen einer Datenbank mit gemeinsam genutztem Durchsatz](#create-a-database-with-shared-throughput)
* [Ändern des Durchsatzes einer Datenbank](#change-database-throughput)
* [Verwalten von Sperren in einer Datenbank](#manage-lock-on-a-database)

### <a name="create-a-database"></a>Erstellen einer Datenbank

Erstellen Sie eine Cosmos-Datenbank.

```azurecli-interactive
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'

az cosmosdb sql database create \
    -a $accountName \
    -g $resourceGroupName \
    -n $databaseName
```

### <a name="create-a-database-with-shared-throughput"></a>Erstellen einer Datenbank mit gemeinsam genutztem Durchsatz

Erstellen Sie eine Cosmos-Datenbank mit gemeinsam genutztem Durchsatz.

```azurecli-interactive
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
throughput=400

az cosmosdb sql database create \
    -a $accountName \
    -g $resourceGroupName \
    -n $databaseName \
    --throughput $throughput
```

### <a name="change-database-throughput"></a>Ändern des Durchsatzes einer Datenbank

Erhöhen Sie den Durchsatz einer Cosmos-Datenbank um 1.000 RU/s.

```azurecli-interactive
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
newRU=1000

# Get minimum throughput to make sure newRU is not lower than minRU
minRU=$(az cosmosdb sql database throughput show \
    -g $resourceGroupName -a $accountName -n $databaseName \
    --query resource.minimumThroughput -o tsv)

if [ $minRU -gt $newRU ]; then
    newRU=$minRU
fi

az cosmosdb sql database throughput update \
    -a $accountName \
    -g $resourceGroupName \
    -n $databaseName \
    --throughput $newRU
```

### <a name="manage-lock-on-a-database"></a>Verwalten von Sperren in einer Datenbank

Legen Sie eine Löschsperre für eine Datenbank fest. Weitere Informationen zum Aktivieren dieses Features finden Sie unter [Verhindern von Änderungen von SDKs](role-based-access-control.md#prevent-sdk-changes).

```azurecli-interactive
resourceGroupName='myResourceGroup'
accountName='my-cosmos-account'
databaseName='myDatabase'

lockType='CanNotDelete' # CanNotDelete or ReadOnly
databaseParent="databaseAccounts/$accountName"
databaseLockName="$databaseName-Lock"

# Create a delete lock on database
az lock create --name $databaseLockName \
    --resource-group $resourceGroupName \
    --resource-type Microsoft.DocumentDB/sqlDatabases \
    --lock-type $lockType \
    --parent $databaseParent \
    --resource $databaseName

# Delete lock on database
lockid=$(az lock show --name $databaseLockName \
        --resource-group $resourceGroupName \
        --resource-type Microsoft.DocumentDB/sqlDatabases \
        --resource $databaseName \
        --parent $databaseParent \
        --output tsv --query id)
az lock delete --ids $lockid
```

## <a name="azure-cosmos-db-container"></a>Azure Cosmos DB-Container

In den folgenden Abschnitten erfahren Sie, wie Sie Azure Cosmos DB-Container verwalten. Dabei lernen Sie u. a. Folgendes:

* [Container erstellen](#create-a-container)
* [Erstellen eines Container mit aktivierter Gültigkeitsdauer](#create-a-container-with-ttl)
* [Erstellen eines Containers mit benutzerdefinierter Indexrichtlinie](#create-a-container-with-a-custom-index-policy)
* [Ändern des Containerdurchsatzes](#change-container-throughput)
* [Verwalten von Sperren in einem Container](#manage-lock-on-a-container)

### <a name="create-a-container"></a>Erstellen eines Containers

Erstellen Sie einen Cosmos-Container mit standardmäßiger Indexrichtlinie, Partitionsschlüssel und einem Durchsatz von 400 RU/s.

```azurecli-interactive
# Create a SQL API container
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
containerName='container1'
partitionKey='/myPartitionKey'
throughput=400

az cosmosdb sql container create \
    -a $accountName -g $resourceGroupName \
    -d $databaseName -n $containerName \
    -p $partitionKey --throughput $throughput
```

### <a name="create-a-container-with-ttl"></a>Erstellen eines Containers mit Gültigkeitsdauer

Erstellen Sie einen Cosmos-Container mit aktivierter Gültigkeitsdauer.

```azurecli-interactive
# Create an Azure Cosmos container with TTL of one day
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
containerName='container1'

az cosmosdb sql container update \
    -g $resourceGroupName \
    -a $accountName \
    -d $databaseName \
    -n $containerName \
    --ttl=86400
```

### <a name="create-a-container-with-a-custom-index-policy"></a>Erstellen eines Containers mit benutzerdefinierter Indexrichtlinie

Erstellen Sie einen Cosmos-Container mit einer benutzerdefinierten Indexrichtlinie, einem räumlichen Index, einem zusammengesetzten Index, einem Partitionsschlüssel und einem Durchsatz von 400 RU/s.

```azurecli-interactive
# Create a SQL API container
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
containerName='container1'
partitionKey='/myPartitionKey'
throughput=400

# Generate a unique 10 character alphanumeric string to ensure unique resource names
uniqueId=$(env LC_CTYPE=C tr -dc 'a-z0-9' < /dev/urandom | fold -w 10 | head -n 1)

# Define the index policy for the container, include spatial and composite indexes
idxpolicy=$(cat << EOF
{
    "indexingMode": "consistent",
    "includedPaths": [
        {"path": "/*"}
    ],
    "excludedPaths": [
        { "path": "/headquarters/employees/?"}
    ],
    "spatialIndexes": [
        {"path": "/*", "types": ["Point"]}
    ],
    "compositeIndexes":[
        [
            { "path":"/name", "order":"ascending" },
            { "path":"/age", "order":"descending" }
        ]
    ]
}
EOF
)
# Persist index policy to json file
echo "$idxpolicy" > "idxpolicy-$uniqueId.json"


az cosmosdb sql container create \
    -a $accountName -g $resourceGroupName \
    -d $databaseName -n $containerName \
    -p $partitionKey --throughput $throughput \
    --idx @idxpolicy-$uniqueId.json

# Clean up temporary index policy file
rm -f "idxpolicy-$uniqueId.json"
```

### <a name="change-container-throughput"></a>Ändern des Containerdurchsatzes

Erhöhen Sie den Durchsatz eines Cosmos-Containers um 1.000 RU/s.

```azurecli-interactive
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
containerName='container1'
newRU=1000

# Get minimum throughput to make sure newRU is not lower than minRU
minRU=$(az cosmosdb sql container throughput show \
    -g $resourceGroupName -a $accountName -d $databaseName \
    -n $containerName --query resource.minimumThroughput -o tsv)

if [ $minRU -gt $newRU ]; then
    newRU=$minRU
fi

az cosmosdb sql container throughput update \
    -a $accountName \
    -g $resourceGroupName \
    -d $databaseName \
    -n $containerName \
    --throughput $newRU
```

### <a name="manage-lock-on-a-container"></a>Verwalten von Sperren in einem Container

Legen Sie eine Löschsperre für einen Container fest. Weitere Informationen zum Aktivieren dieses Features finden Sie unter [Verhindern von Änderungen von SDKs](role-based-access-control.md#prevent-sdk-changes).

```azurecli-interactive
resourceGroupName='myResourceGroup'
accountName='my-cosmos-account'
databaseName='myDatabase'
containerName='myContainer'

lockType='CanNotDelete' # CanNotDelete or ReadOnly
databaseParent="databaseAccounts/$accountName"
containerParent="databaseAccounts/$accountName/sqlDatabases/$databaseName"
containerLockName="$containerName-Lock"

# Create a delete lock on container
az lock create --name $containerLockName \
    --resource-group $resourceGroupName \
    --resource-type Microsoft.DocumentDB/containers \
    --lock-type $lockType \
    --parent $containerParent \
    --resource $containerName

# Delete lock on container
lockid=$(az lock show --name $containerLockName \
        --resource-group $resourceGroupName \
        --resource-type Microsoft.DocumentDB/containers \
        --resource-name $containerName \
        --parent $containerParent \
        --output tsv --query id)
az lock delete --ids $lockid
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in:

- [Installieren der Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli)
- [Azure-CLI-Referenz](https://docs.microsoft.com/cli/azure/cosmosdb)
- [Zusätzliche Azure CLI-Beispiele für Azure Cosmos DB](cli-samples.md)
