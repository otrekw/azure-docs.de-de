---
title: Erstellen einer Datenbank mit Autoskalierung und von freigegebenen Sammlungen für die MongoDB-API für Azure Cosmos DB
description: Erstellen einer Datenbank mit Autoskalierung und von freigegebenen Sammlungen für die MongoDB-API für Azure Cosmos DB
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
ms.date: 7/29/2020
ms.openlocfilehash: a24b38e6a3fa524f3d564e62d844c24f6a8549f8
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107763791"
---
# <a name="create-a-database-with-autoscale-and-shared-collections-for-mongodb-api-for-azure-cosmos-db-using-azure-cli"></a>Erstellen einer Datenbank mit Autoskalierung und von freigegebenen Sammlungen für die MongoDB-API für Azure Cosmos DB mithilfe der Azure CLI
[!INCLUDE[appliesto-mongodb-api](../../../includes/appliesto-mongodb-api.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../../includes/azure-cli-prepare-your-environment.md)]

- Für diesen Artikel ist mindestens Version 2.9.1 der Azure CLI erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert.

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/mongodb/autoscale.sh "Create an Azure Cosmos DB MongoDB API account, database with autoscale, and two shared throughput collections.")]

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
| [az cosmosdb mongodb database create](/cli/azure/cosmosdb/mongodb/database#az_cosmosdb_mongodb_database_create) | Erstellt eine Azure Cosmos-MongoDB-API-Datenbank. |
| [az cosmosdb mongodb collection create](/cli/azure/cosmosdb/mongodb/collection#az_cosmosdb_mongodb_collection_create) | Erstellt eine Azure Cosmos-MongoDB-API-Sammlung. |
| [az group delete](/cli/azure/resource#az_resource_delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure Cosmos DB-CLI finden Sie in der [Dokumentation zur Azure Cosmos DB-CLI](/cli/azure/cosmosdb).

Alle CLI-Skriptbeispiele für Azure Cosmos DB finden Sie im [GitHub-Repository zur Azure Cosmos DB-CLI](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
