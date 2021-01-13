---
title: 'Azure CLI: Importieren einer BACPAC-Datei in eine Datenbank in Azure SQL-Datenbank'
description: Azure CLI-Beispielskript zum Importieren einer BACPAC-Datei in eine Datenbank in Azure SQL-Datenbank
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: load & move data, devx-track-azurecli
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 05/24/2019
ms.openlocfilehash: a8c544f1d3b7d1255cf5a0e6d05975ea5f5ec203
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87503802"
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

| Get-Help | BESCHREIBUNG |
|---|---|
| [az sql server](/cli/azure/sql/server) | Serverbefehle |
| [az sql db import](/cli/azure/sql/db#az-sql-db-import) | Befehl für Datenbankimport |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Zusätzliche SQL-Datenbank-CLI-Skriptbeispiele finden Sie in der [Azure SQL-Datenbank-Dokumentation](../../azure-sql/database/az-cli-script-samples-content-guide.md).
