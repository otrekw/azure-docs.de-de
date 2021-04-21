---
title: Erstellen einer Ressourcensperre für eine Datenbank und Sammlung für die MongoDB-API für Azure Cosmos DB
description: Erstellen einer Ressourcensperre für eine Datenbank und Sammlung für die MongoDB-API für Azure Cosmos DB
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: ee298b33736ae25b365cc54ee1bb5ec9f38bfb62
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107763600"
---
# <a name="create-a-resource-lock-for-azure-cosmos-dbs-api-for-mongodb-using-azure-cli"></a>Erstellen Sie mit der Azure CLI eine Ressourcensperre für die Azure Cosmos DB-API für MongoDB.
[!INCLUDE[appliesto-mongodb-api](../../../includes/appliesto-mongodb-api.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../../includes/azure-cli-prepare-your-environment.md)]

- Für diesen Artikel ist mindestens Version 2.9.1 der Azure CLI erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert.

> [!IMPORTANT]
> Ressourcensperren funktionieren nicht bei Änderungen, die von Benutzern vorgenommen werden, die eine Verbindung über ein MongoDB SDK, die Mongo-Shell, Tools oder das Azure-Portal herstellen, es sei denn, das Cosmos DB-Konto wird zuvor bei aktivierter Eigenschaft `disableKeyBasedMetadataWriteAccess` gesperrt. Weitere Informationen zum Aktivieren dieser Eigenschaft finden Sie unter [Verhindern von Änderungen im Cosmos SDK](../../../role-based-access-control.md#prevent-sdk-changes).

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/mongodb/lock.sh "Create a resource lock for an Azure Cosmos DB MongoDB API database and collection.")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az lock create](/cli/azure/lock#az_lock_create) | Erstellt eine Sperre. |
| [az lock list](/cli/azure/lock#az_lock_list) | Listet Sperrinformationen auf. |
| [az lock show](/cli/azure/lock#az_lock_show) | Zeigt Eigenschaften einer Sperre an. |
| [az lock delete](/cli/azure/lock#az_lock_delete) | Löscht eine Sperre. |

## <a name="next-steps"></a>Nächste Schritte

- [Sperren von Ressourcen, um unerwartete Änderungen zu verhindern](../../../../azure-resource-manager/management/lock-resources.md)

- [Dokumentation zur CLI für Azure Cosmos DB](/cli/azure/cosmosdb)

- [GitHub-Repository für die CLI für Azure Cosmos DB](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)
