---
title: 'PowerShell-Skripts für Durchsatzvorgänge (RU/s): Azure Cosmos DB-Gremlin-API'
description: 'PowerShell-Skripts für Durchsatzvorgänge (RU/s): Gremlin-API'
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: sample
ms.date: 10/07/2020
ms.author: mjbrown
ms.openlocfilehash: c2c598c65b1277248fea545f6bb25e5bed5aedaf
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92280062"
---
# <a name="throughput-rus-operations-with-powershell-for-a-database-or-graph-for-azure-cosmos-db---gremlin-api"></a>Durchsatzvorgänge (RU/s) mit PowerShell für eine Datenbank oder einen Graphen für Azure Cosmos DB: Gremlin-API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="get-throughput"></a>Durchsatzberechnung

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/gremlin/ps-gremlin-ru-get.ps1 "Get throughput on a database or graph for Gremlin API")]

## <a name="update-throughput"></a>Aktualisieren des Durchsatzes

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/gremlin/ps-gremlin-ru-update.ps1 "Update throughput on a database or graph for Gremlin API")]

## <a name="migrate-throughput"></a>Migrieren des Durchsatzes

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/gremlin/ps-gremlin-ru-migrate.ps1 "Migrate between standard and autoscale throughput on a database or graph for Gremlin API")]

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
| [Get-AzCosmosDBGremlinDatabaseThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbgremlindatabasethroughput) | Ruft den Durchsatzwert der Gremlin-API-Datenbank ab. |
| [Get-AzCosmosDBGremlinGraphThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbgremlingraphthroughput) | Ruft den Durchsatzwert des Gremlin-API-Diagramms ab. |
| [Update-AzCosmosDBGremlinDatabaseThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/update-azcosmosdbgremlindatabasethroughput) | Aktualisiert den Durchsatzwert der Gremlin-API-Datenbank. |
| [Update-AzCosmosDBGremlinGraphThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/update-azcosmosdbgremlingraphthroughput) | Aktualisiert den Durchsatzwert des Gremlin-API-Diagramms. |
| [Invoke-AzCosmosDBGremlinDatabaseThroughputMigration](https://docs.microsoft.com/powershell/module/az.cosmosdb/invoke-azcosmosdbgremlindatabasethroughputmigration) | Migriert den Durchsatz einer Gremlin-API-Datenbank. |
| [Invoke-AzCosmosDBGremlinGraphThroughputMigration](https://docs.microsoft.com/powershell/module/az.cosmosdb/invoke-azcosmosdbgremlingraphthroughputmigration) | Migriert den Durchsatz eines Gremlin-API-Graphen. |
|**Azure-Ressourcengruppen**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/).
