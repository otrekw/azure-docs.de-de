---
title: Erstellen einer Gremlin-Datenbank und eines Gremlin-Graphs für Azure Cosmos DB
description: Erstellen einer Gremlin-Datenbank und eines Gremlin-Graphs für Azure Cosmos DB
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: 8b551517dfd0066091086944d02a464fd484b8af
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107770682"
---
# <a name="create-an-azure-cosmos-gremlin-api-account-database-and-graph-using-azure-cli"></a>Erstellen eines Gremlin-API-Kontos, einer Datenbank und eines Graphs für Azure Cosmos DB mithilfe der Azure CLI
[!INCLUDE[appliesto-gremlin-api](../../../includes/appliesto-gremlin-api.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../../includes/azure-cli-prepare-your-environment.md)]

- Für diesen Artikel ist mindestens Version 2.9.1 der Azure CLI erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert.

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/gremlin/create.sh "Create an Azure Cosmos DB Gremlin API account, database, and graph.")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```azurecli-interactive
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az cosmosdb create](/cli/azure/cosmosdb#az_cosmosdb_create) | Erstellt ein Konto für Azure Cosmos DB. |
| [az cosmosdb gremlin database create](/cli/azure/cosmosdb/gremlin/database#az_cosmosdb_gremlin_database_create) | Erstellt eine Azure Cosmos-Gremlin-Datenbank. |
| [az cosmosdb gremlin graph create](/cli/azure/cosmosdb/gremlin/graph#az_cosmosdb_gremlin_graph_create) | Erstellt einen Azure Cosmos-Gremlin-Graphen. |
| [az group delete](/cli/azure/resource#az_resource_delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure Cosmos DB-CLI finden Sie in der [Dokumentation zur Azure Cosmos DB-CLI](/cli/azure/cosmosdb).

Alle CLI-Skriptbeispiele für Azure Cosmos DB finden Sie im [GitHub-Repository zur Azure Cosmos DB-CLI](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
