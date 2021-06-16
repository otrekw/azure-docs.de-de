---
title: 'Azure CLI: Importieren einer BACPAC-Datei in eine Datenbank in Azure SQL-Datenbank'
description: Azure CLI-Beispielskript zum Importieren einer BACPAC-Datei in eine Datenbank in Azure SQL-Datenbank
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: load & move data, devx-track-azurecli
ms.devlang: azurecli
ms.topic: sample
author: shkale-msft
ms.author: shkale
ms.reviewer: mathoma
ms.date: 05/24/2019
ms.openlocfilehash: d0c20ee8e64d5ed9887e04efeaa721e2ea8d08c9
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110688655"
---
# <a name="use-cli-to-import-a-bacpac-file-into-a-database-in-sql-database"></a>Verwenden der CLI zum Importieren einer BACPAC-Datei in eine Datenbank in SQL-Datenbank

In diesem Azure CLI-Skriptbeispiel wird eine Datenbank aus einer *BACPAC*-Datei in eine Datenbank in SQL-Datenbank importiert.  

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Artikel die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Beispielskript

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>Führen Sie das Skript aus.

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/import-from-bacpac/import-from-bacpac.sh "Create SQL Database")]

### <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Verwenden Sie den folgenden Befehl, um die Ressourcengruppe und alle zugehörigen Ressourcen zu entfernen:

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Beispielreferenz

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Beschreibung |
|---|---|
| [az sql server](/cli/azure/sql/server) | Serverbefehle |
| [az sql db import](/cli/azure/sql/db#az_sql_db_import) | Befehl für Datenbankimport |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Zusätzliche SQL-Datenbank-CLI-Skriptbeispiele finden Sie in der [Azure SQL-Datenbank-Dokumentation](../../azure-sql/database/az-cli-script-samples-content-guide.md).
