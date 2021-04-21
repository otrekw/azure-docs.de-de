---
title: 'Azure CLI: Überwachen und Skalieren einer Einzeldatenbank in Azure SQL­-Datenbank'
description: Verwenden Sie ein Azure CLI-Beispielskript zum Überwachen und Skalieren einer einzelnen Datenbank in Azure SQL-Datenbank.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: sqldbrb=1, devx-track-azurecli
ms.devlang: azurecli
ms.topic: sample
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: sstein
ms.date: 06/25/2019
ms.openlocfilehash: bd982ab6dc66674e705f080511282bcfeb909872
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107787173"
---
# <a name="use-the-azure-cli-to-monitor-and-scale-a-single-database-in-azure-sql-database"></a>Verwenden von Azure CLI zum Überwachen und Skalieren einer einzelnen Datenbank in Azure SQL-Datenbank

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

Dieses Azure CLI-Skriptbeispiel skaliert eine einzelne Datenbank in Azure SQL-Datenbank nach Abfrage der Größeninformationen für die Datenbank auf eine andere Computegröße.

Wenn Sie die Azure-Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diesen Artikel die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Beispielskript

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

```azurecli-interactive
$subscription = "<subscriptionId>" # add subscription here

az account set -s $subscription # ...or use 'az login'
```

### <a name="run-the-script"></a>Führen Sie das Skript aus.

[!code-azurecli-interactive[main](../../../../cli_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.sh "Monitor and scale a database in Azure SQL Database")]

> [!TIP]
> Verwenden Sie [az sql db op list](/cli/azure/sql/db/op?#az_sql_db_op_list), um eine Liste von in der Datenbank ausgeführten Vorgängen zu erhalten, und verwenden Sie [az sql db op cancel](/cli/azure/sql/db/op#az_sql_db_op_cancel), um einen Aktualisierungsvorgang in der Datenbank abzubrechen.

### <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Verwenden Sie den folgenden Befehl, um die Ressourcengruppe und alle zugehörigen Ressourcen zu entfernen:

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Beispielreferenz

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Skript | BESCHREIBUNG |
|---|---|
| [az sql server](/cli/azure/sql/server) | Serverbefehle |
| [az sql db show-usage](/cli/azure/sql#az_sql_show_usage) | Zeigt die Größennutzungsinformationen für eine Datenbank an. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere CLI-Skriptbeispiele finden Sie unter [Azure CLI Beispielskripts](../az-cli-script-samples-content-guide.md).
