---
title: 'Azure CLI-Skripts für Durchsatzvorgänge (RU/s): Ressourcen der Gremlin-API für Azure Cosmos DB'
description: 'Azure CLI-Skripts für Durchsatzvorgänge (RU/s): Ressourcen der Gremlin-API für Azure Cosmos DB'
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: sample
ms.date: 10/07/2020
ms.openlocfilehash: ac8f915a1e9cc73c076879ea8e5ee487c3fe0c3f
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107770631"
---
# <a name="throughput-rus-operations-with-azure-cli-for-a-database-or-graph-for-azure-cosmos-db---gremlin-api"></a>Durchsatzvorgänge (RU/s) mit der Azure CLI für eine Datenbank oder einen Graphen für Azure Cosmos DB: Gremlin-API
[!INCLUDE[appliesto-gremlin-api](../../../includes/appliesto-gremlin-api.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../../includes/azure-cli-prepare-your-environment.md)]

- Für diesen Artikel ist mindestens Version 2.12.1 der Azure CLI erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert.

## <a name="sample-script"></a>Beispielskript

Mit diesem Skript werden eine Gremlin-Datenbank mit gemeinsam genutztem Durchsatz und ein Gremlin-Graph mit dediziertem Durchsatz erstellt, und anschließend wird der Durchsatz sowohl für die Datenbank als auch für den Graphen aktualisiert. Anschließend führt das Skript die Migration vom standardmäßigen zum automatisch skalierten Durchsatz durch, und nach Abschluss der Migration wird dann der Wert des automatisch skalierten Durchsatzes gelesen.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/gremlin/throughput.sh "Throughput operations for a Gremlin database and graph.")]

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
| [az cosmosdb gremlin database throughput update](/cli/azure/cosmosdb/gremlin/database/throughput#az_cosmosdb_gremlin_database_throughput_update) | Aktualisiert die Anforderungseinheiten pro Sekunde (RU/s) für eine Azure Cosmos-Gremlin-Datenbank. |
| [az cosmosdb gremlin graph throughput update](/cli/azure/cosmosdb/gremlin/graph/throughput#az_cosmosdb_gremlin_graph_throughput_update) | Aktualisiert die Anforderungseinheiten pro Sekunde (RU/s) für einen Azure Cosmos-Gremlin-Graphen. |
| [az cosmosdb gremlin database throughput migrate](/cli/azure/cosmosdb/gremlin/database/throughput#az_cosmosdb_gremlin_database_throughput_migrate) | Migriert den Durchsatz für eine Azure Cosmos-Gremlin-Datenbank. |
| [az cosmosdb gremlin graph throughput migrate](/cli/azure/cosmosdb/gremlin/graph/throughput#az_cosmosdb_gremlin_graph_throughput_migrate) | Migriert den Durchsatz für einen Azure Cosmos-Gremlin-Graphen. |
| [az group delete](/cli/azure/resource#az_resource_delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure Cosmos DB-CLI finden Sie in der [Dokumentation zur Azure Cosmos DB-CLI](/cli/azure/cosmosdb).

Alle CLI-Skriptbeispiele für Azure Cosmos DB finden Sie im [GitHub-Repository zur Azure Cosmos DB-CLI](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
