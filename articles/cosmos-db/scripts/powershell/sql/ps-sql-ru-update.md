---
title: PowerShell-Skript zum Aktualisieren des Durchsatzes (RU/s) für eine Datenbank oder einen Container der Azure Cosmos DB-SQL-API
description: PowerShell-Skript zum Aktualisieren des Durchsatzes (RU/s) für eine Datenbank oder einen Container der Azure Cosmos DB-SQL-API
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 05/01/2020
ms.author: mjbrown
ms.openlocfilehash: 19b13db83b2c2533e963a503dc8ea3e362b5b5ed
ms.sourcegitcommit: d662eda7c8eec2a5e131935d16c80f1cf298cb6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2020
ms.locfileid: "82652962"
---
# <a name="update-throughput-rus-for-an-azure-cosmos-db-sql-api-database-or-container"></a>Aktualisieren des Durchsatzes (RU/s) für eine Datenbank oder einen Container der Azure Cosmos DB-SQL-API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/sql/ps-sql-ru-update.ps1 "Update throughput (RU/s) for an Azure Cosmos DB SQL API Database or Container")]

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
| [Get-AzCosmosDBSqlContainerThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbsqlcontainerthroughput) | Ruft den Durchsatzwert des SQL-Containers ab. |
| [Update-AzCosmosDBSqlContainerThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbsqlcontainer) | Aktualisiert den Durchsatzwert des SQL-Containers. |
|**Azure-Ressourcengruppen**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/).

Zusätzliche PowerShell-Skriptbeispiele für Azure Cosmos DB finden Sie unter [PowerShell-Skripts für Azure Cosmos DB](../../../powershell-samples.md).