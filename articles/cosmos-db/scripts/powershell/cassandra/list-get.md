---
title: PowerShell-Skript zum Auflisten und Abrufen von Cassandra-API-Ressourcen in Azure Cosmos DB
description: 'Azure PowerShell-Skript: list- und get-Vorgänge von Azure Cosmos DB für die Cassandra-API'
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
ms.date: 03/18/2020
ms.author: mjbrown
ms.openlocfilehash: cf29bae8b2aa011593e4d9acb45bad71a6ad0167
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87505454"
---
# <a name="list-and-get-keyspaces-and-tables-for-azure-cosmos-db---cassandra-api"></a>Auflisten und Abrufen von Keyspaces und Tabellen für Azure Cosmos DB: Cassandra-API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/cassandra/ps-cassandra-list-get.ps1 "List or get keyspace or table for Cassandra API")]

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
| [Get-AzCosmosDBAccount](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbaccount) | Listet Cosmos DB-Konten auf oder ruft ein bestimmtes Cosmos DB-Konto ab. |
| [Get-AzCosmosDBCassandraKeyspace](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbcassandrakeyspace) | Listet Cosmos DB-Cassandra-API-Keyspaces in einem Konto auf oder ruft einen bestimmten Cosmos DB-Cassandra-API-Keyspace in einem Konto ab. |
| [Get-AzCosmosDBCassandraTable](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbcassandratable) | Listet Cosmos DB-Cassandra-API-Tabellen in einem Keyspace auf oder ruft eine bestimmte Cosmos DB-Cassandra-API-Tabelle in einem Keyspace ab. |
|**Azure-Ressourcengruppen**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/).

Zusätzliche PowerShell-Skriptbeispiele für Azure Cosmos DB finden Sie unter [PowerShell-Skripts für Azure Cosmos DB](../../../powershell-samples.md).