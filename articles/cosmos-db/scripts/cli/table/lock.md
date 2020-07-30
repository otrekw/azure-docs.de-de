---
title: Erstellen einer Ressourcensperre für eine Azure Cosmos DB-Tabellen-API-Tabelle
description: Erstellen einer Ressourcensperre für eine Azure Cosmos DB-Tabellen-API-Tabelle
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: sample
ms.date: 06/03/2020
ms.openlocfilehash: bb66f215241755ae0fbcef3c95f5ea67a686a4fd
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87092574"
---
# <a name="create-resource-lock-for-a-azure-cosmos-db-table-api-table-using-azure-cli"></a>Erstellen einer Ressourcensperre für eine Azure Cosmos DB-Tabellen-API Tabelle mithilfe der Azure CLI

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, ist es für dieses Thema erforderlich, die Azure CLI-Version 2.6.0 oder höher auszuführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

> [!IMPORTANT]
> Ressourcensperren funktionieren nicht bei Änderungen, die von Benutzern vorgenommen werden, die eine Verbindung über ein Cosmos DB Table SDK, ein Azure Storage Table SDK, über Tools mit Verbindungsherstellung über Kontoschlüssel oder über das Azure-Portal herstellen, es sei denn, das Cosmos DB-Konto wird zunächst bei aktivierter Eigenschaft `disableKeyBasedMetadataWriteAccess` gesperrt. Weitere Informationen zum Aktivieren dieser Eigenschaft finden Sie unter [Verhindern von Änderungen im Cosmos SDK](../../../role-based-access-control.md#prevent-sdk-changes).

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/table/lock.sh "Create a resource lock for an Azure Cosmos DB Table API table.")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az lock create](/cli/azure/lock#az-lock-create) | Erstellt eine Sperre. |
| [az lock list](/cli/azure/lock#az-lock-list) | Listet Sperrinformationen auf. |
| [az lock show](/cli/azure/lock#az-lock-show) | Zeigt Eigenschaften einer Sperre an. |
| [az lock delete](/cli/azure/lock#az-lock-delete) | Löscht eine Sperre. |

## <a name="next-steps"></a>Nächste Schritte

-[Sperren von Ressourcen, um unerwartete Änderungen zu verhindern](../../../../azure-resource-manager/management/lock-resources.md)

-[Dokumentation zur CLI für Azure Cosmos DB](/cli/azure/cosmosdb)

-[GitHub-Repository für die CLI für Azure Cosmos DB](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)
