---
title: 'PowerShell-Skripts für Durchsatzvorgänge (RU/s): Azure Cosmos DB-API für MongoDB'
description: 'PowerShell-Skripts für Durchsatzvorgänge (RU/s): Azure Cosmos DB-API für MongoDB'
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
ms.date: 10/07/2020
ms.author: mjbrown
ms.openlocfilehash: 0b3c755415185324d427bcd3ae222774bca31284
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92282717"
---
# <a name="throughput-rus-operations-with-powershell-for-a-database-or-collection-for-azure-cosmos-db-api-for-mongodb"></a>Durchsatzvorgänge (RU/s) mit PowerShell für eine Datenbank oder Sammlung für die Azure Cosmos DB-API für MongoDB

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="get-throughput"></a>Durchsatzberechnung

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/mongodb/ps-mongodb-ru-get.ps1 "Get throughput on a database or collection for Azure Cosmos DB API for MongoDB")]

## <a name="update-throughput"></a>Aktualisieren des Durchsatzes

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/mongodb/ps-mongodb-ru-update.ps1 "Update throughput on a database or collection for Azure Cosmos DB API for MongoDB")]

## <a name="migrate-throughput"></a>Migrieren des Durchsatzes

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/mongodb/ps-mongodb-ru-migrate.ps1 "Migrate between standard and autoscale throughput on a database or collection for Azure Cosmos DB API for MongoDB")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
|**Azure Cosmos DB**| |
| [Get-AzCosmosDBMongoDBDatabaseThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbmongodbdatabasethroughput) | Ruft den Durchsatzwert der angegebenen Datenbank der Azure Cosmos DB-API für MongoDB ab. |
| [Get-AzCosmosDBMongoDBCollectionThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbmongodbcollectionthroughput) | Ruft den Durchsatzwert der angegebenen Sammlung der Azure Cosmos DB-API für MongoDB ab. |
| [Update-AzCosmosDBMongoDBDatabaseThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/update-azcosmosdbmongodbdatabasethroughput) | Aktualisiert den Durchsatzwert der Datenbank der Azure Cosmos DB-API für MongoDB. |
| [Update-AzCosmosDBMongoDBCollectionThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/update-azcosmosdbmongodbcollectionthroughput) | Aktualisiert den Durchsatzwert der Sammlung der Azure Cosmos DB-API für MongoDB. |
| [Invoke-AzCosmosDBMongoDBDatabaseThroughputMigration](https://docs.microsoft.com/powershell/module/az.cosmosdb/invoke-azcosmosdbmongodbdatabasethroughputmigration) | Migriert den Durchsatz einer Datenbank der Azure Cosmos DB-API für MongoDB. |
| [Invoke-AzCosmosDBMongoDBCollectionThroughputMigration](https://docs.microsoft.com/powershell/module/az.cosmosdb/invoke-azcosmosdbmongodbcollectionthroughputmigration) | Migriert den Durchsatz einer Sammlung der Azure Cosmos DB-API für MongoDB. |
|**Azure-Ressourcengruppen**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/).
