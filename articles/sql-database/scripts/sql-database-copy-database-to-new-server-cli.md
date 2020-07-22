---
title: 'Azure CLI: Kopieren einer Datenbank in Azure SQL-Datenbank auf einen neuen Server'
description: Azure CLI-Beispielskript zum Kopieren einer Datenbank in Azure SQL-Datenbank auf einen neuen Server
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 03/12/2019
ms.openlocfilehash: 3f180f075a32ac193ed003b58a99483ea3210cc7
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86528896"
---
# <a name="use-cli-to-copy-a-database-in-azure-sql-database-to-a-new-server"></a>Verwenden der CLI zum Kopieren einer Datenbank in Azure SQL-Datenbank auf einen neuen Server

In diesem Azure CLI-Skriptbeispiel wird eine Kopie einer vorhandenen Datenbank auf einem neuen Server erstellt.

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Artikel die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Beispielskript

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

```azurecli-interactive
$subscription = "<subscriptionId>" # add subscription here

az account set -s $subscription # ...or use 'az login'
```

### <a name="run-the-script"></a>Führen Sie das Skript aus.

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/copy-database-to-new-server/copy-database-to-new-server.sh "Copy database to new server")]

### <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Verwenden Sie den folgenden Befehl, um die Ressourcengruppe und alle zugehörigen Ressourcen zu entfernen:

```azurecli-interactive
az group delete --name $resource
az group delete --name $targetResource
```

## <a name="sample-reference"></a>Beispielreferenz

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Beschreibung |
|---|---|
| [az sql db copy](/cli/azure/sql/db#az-sql-db-copy) | Erstellt eine Kopie einer Datenbank, die die Momentaufnahme zum jetzigen Zeitpunkt verwendet. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Zusätzliche SQL-Datenbank-CLI-Skriptbeispiele finden Sie in der [Azure SQL-Datenbank-Dokumentation](../../azure-sql/database/az-cli-script-samples-content-guide.md).
