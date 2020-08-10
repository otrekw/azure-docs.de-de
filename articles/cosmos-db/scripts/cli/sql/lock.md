---
title: Erstellen einer Ressourcensperre für eine Datenbank und einen Container für die Core-API (SQL) von Azure Cosmos DB
description: Erstellen einer Ressourcensperre für eine Datenbank und einen Container für die Core-API (SQL) von Azure Cosmos DB
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: 92912475b6420c0fd3ffc3a31cefb269475431f9
ms.sourcegitcommit: 0b8320ae0d3455344ec8855b5c2d0ab3faa974a3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2020
ms.locfileid: "87431793"
---
# <a name="create-resource-lock-for-a-azure-cosmos-db-core-sql-api-database-and-container-using-azure-cli"></a>Erstellen einer Ressourcensperre für eine Datenbank und einen Container für die Core-API (SQL) von Azure Cosmos DB mithilfe der Azure CLI

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, ist es für dieses Thema erforderlich, mindestens die Azure CLI-Version 2.9.1 auszuführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

> [!IMPORTANT]
> Ressourcensperren funktionieren nicht bei Änderungen, die von Benutzern vorgenommen werden, die eine Verbindung über ein Cosmos DB SDK, über Tools mit Verbindungsherstellung über Kontoschlüssel oder über das Azure-Portal herstellen, es sei denn, das Cosmos DB-Konto wird zunächst bei aktivierter Eigenschaft `disableKeyBasedMetadataWriteAccess` gesperrt. Weitere Informationen zum Aktivieren dieser Eigenschaft finden Sie unter [Verhindern von Änderungen im Cosmos SDK](../../../role-based-access-control.md#prevent-sdk-changes).

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/sql/lock.sh "Create a resource lock for an Azure Cosmos DB Core (SQL) API database and container.")]

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
