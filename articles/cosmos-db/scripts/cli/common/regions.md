---
title: Hinzufügen von Regionen, Anpassen der Failoverpriorität, Auslösen des Failovers für ein Azure Cosmos-Konto
description: Hinzufügen von Regionen, Anpassen der Failoverpriorität, Auslösen des Failovers für ein Azure Cosmos-Konto
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: 4a1a061945fe1c6c6a95eb62d286d40a158281ca
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107770764"
---
# <a name="add-regions-change-failover-priority-trigger-failover-for-an-azure-cosmos-account-using-azure-cli"></a>Hinzufügen von Regionen, Anpassen der Failoverpriorität, Auslösen des Failovers für ein Azure Cosmos-Konto mithilfe der Azure CLI
[!INCLUDE[appliesto-all-apis](../../../includes/appliesto-all-apis.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../../includes/azure-cli-prepare-your-environment.md)]

- Für diesen Artikel ist mindestens Version 2.9.1 der Azure CLI erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert.

## <a name="sample-script"></a>Beispielskript

Dieses Skript veranschaulicht drei Vorgänge.

- Hinzufügen einer Region zu einem vorhandenen Azure Cosmos-Konto
- Ändern der Priorität des regionalen Failovers (gilt für Konten mit automatischem Failover)
- Auslösen eines manuelles Failovers von der primären zur sekundären Region aus (gilt für Konten mit manuellem Failover).

> [!NOTE]
> Während andere Eigenschaften geändert werden, können in einem Cosmos-Konto keine Regionsvorgänge hinzugefügt oder entfernt werden.

> [!NOTE]
> Dieses Beispiel veranschaulicht die Verwendung eines SQL-API-Kontos (Core), aber diese Vorgänge sind für alle Datenbank-APIs in Cosmos DB identisch.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/common/regions.sh "Regional operations for Cosmos DB.")]

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
| [az cosmosdb update](/cli/azure/cosmosdb#az_cosmosdb_update) | Aktualisiert ein Azure Cosmos DB-Konto (Hinzufügen oder Entfernen einer Region). |
| [az cosmosdb failover-priority-change](/cli/azure/cosmosdb#az_cosmosdb_failover_priority_change) | Aktualisiert die Failoverpriorität oder löst ein Failover für ein Azure Cosmos DB-Konto aus. |
| [az group delete](/cli/azure/resource#az_resource_delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure Cosmos DB-CLI finden Sie in der [Dokumentation zur Azure Cosmos DB-CLI](/cli/azure/cosmosdb).

Alle CLI-Skriptbeispiele für Azure Cosmos DB finden Sie im [GitHub-Repository zur Azure Cosmos DB-CLI](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
