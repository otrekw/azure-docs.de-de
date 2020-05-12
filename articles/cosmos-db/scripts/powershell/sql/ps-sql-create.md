---
title: PowerShell-Skript zum Erstellen einer Datenbank und eines Containers für die SQL-API in Azure Cosmos DB
description: 'Azure PowerShell-Skript: Azure Cosmos DB – Erstellen einer Datenbank und eines Containers für die SQL-API'
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 05/01/2020
ms.author: mjbrown
ms.openlocfilehash: 434cc6df326a0028f327c6d9e2eda0d19989ca01
ms.sourcegitcommit: d662eda7c8eec2a5e131935d16c80f1cf298cb6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2020
ms.locfileid: "82653163"
---
# <a name="create-a-database-and-container-for-azure-cosmos-db---sql-api"></a>Erstellen einer Datenbank und eines Containers für Azure Cosmos DB – SQL-API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Beispielskript

Dieses Skript erstellt ein Cosmos-Konto für die SQL-API (Core-API) in zwei Regionen mit Konsistenz auf Sitzungsebene, eine Datenbank und einen Container mit einem Partitionsschlüssel, benutzerdefinierter Indizierungsrichtlinie, einer Richtlinie für eindeutige Schlüssel, TTL, dediziertem Durchsatz und einer Konfliktlösungsrichtlinie vom Typ „Letzter Schreiber gewinnt“ mit einem benutzerdefinierten Konfliktauflösungspfads, der im Fall von `multipleWriteLocations=true` verwendet wird.

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/sql/ps-sql-create.ps1 "Create an account, database, and container for SQL API")]

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
| [New-AzCosmosDBAccount](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbaccount) | Erstellt ein neues Cosmos DB-Konto. |
| [Set-AzCosmosDBSqlDatabase](https://docs.microsoft.com/powershell/module/az.cosmosdb/set-azcosmosdbsqldatabase) | Erstellt eine neue Cosmos DB-SQL-Datenbank oder aktualisiert eine vorhandene Cosmos DB-SQL-Datenbank. |
| [New-AzCosmosDBSqlUniqueKey](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqluniquekey) | Erstellt ein neues Cosmos DB-SQL-Objekt vom Typ „PSSqlUniqueKey“, das als Parameter für „New-AzCosmosDBSqlUniqueKeyPolicy“ verwendet wird. |
| [New-AzCosmosDBSqlUniqueKeyPolicy](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqluniquekeypolicy) | Erstellt ein neues Cosmos DB-SQL-Objekt vom Typ „PSSqlUniqueKeyPolicy“, das als Parameter für „Set-AzCosmosDBSqlContainer“ verwendet wird. |
| [New-AzCosmosDBSqlCompositePath](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqlcompositepath) | Erstellt ein neues Cosmos DB-SQL-Objekt vom Typ „PSCompositePath“, das als Parameter für „New-AzCosmosDBSqlIndexingPolicy“ verwendet wird. |
| [New-AzCosmosDBSqlIncludedPathIndex](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqlincludedpathindex) | Erstellt ein neues Objekt vom Typ „PSIndexes“, das als Parameter für „Set-AzCosmosDBSqlIncludedPath“ verwendet wird. |
| [New-AzCosmosDBSqlIncludedPath](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqlincludedpath) | Erstellt ein neues Objekt vom Typ „PSIncludedPath“, das als Parameter für „New-AzCosmosDBSqlIndexingPolicy“ verwendet wird. |
| [New-AzCosmosDBSqlIndexingPolicy](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqlindexingpolicy) | Erstellt ein neues Objekt vom Typ „PSSqlIndexingPolicy“, das als Parameter für „Set-AzCosmosDBSqlContainer“ verwendet wird. |
| [New-AzCosmosDBSqlConflictResolutionPolicy](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqlconflictresolutionpolicy) | Erstellt ein neues Objekt vom Typ „PSSqlConflictResolutionPolicy“, das als Parameter für „Set-AzCosmosDBSqlContainer“ verwendet wird. |
| [Set-AzCosmosDBSqlContainer](https://docs.microsoft.com/powershell/module/az.cosmosdb/set-azcosmosdbsqlcontainer) | Erstellt einen neuen Cosmos DB-SQL-Container oder aktualisiert einen vorhandenen Cosmos DB-SQL-Container. |
|**Azure-Ressourcengruppen**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/).

Zusätzliche PowerShell-Skriptbeispiele für Azure Cosmos DB finden Sie unter [PowerShell-Skripts für Azure Cosmos DB](../../../powershell-samples.md).
