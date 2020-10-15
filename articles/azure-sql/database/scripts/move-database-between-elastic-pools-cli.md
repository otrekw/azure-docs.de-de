---
title: 'Azure CLI: Verschieben einer Datenbank zwischen Pools für elastische Datenbanken'
description: Verwenden Sie ein Azure CLI-Beispielskript, um zwei Pools für elastische Datenbanken zu erstellen und eine Datenbank in Azure SQL-Datenbank von einem Pool für elastische Datenbanken in einen anderen zu verschieben.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: sqldbrb=1
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 06/25/2019
ms.openlocfilehash: a71e3e05e486c09d148062eed210c9f4b21e8226
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91319349"
---
# <a name="use-the-azure-cli-to-move-a-database-in-sql-database-in-a-sql-elastic-pool"></a>Verwenden Sie die Azure CLI zum Verschieben einer Datenbank in Azure SQL-Datenbank in einen Pool für elastische SQL-Datenbanken

In diesem Azure CLI-Skriptbeispiel werden zwei Pools für elastische Datenbanken erstellt. Anschließend wird eine Pooldatenbank aus einem Pool für elastische SQL-Datenbanken in einen anderen und dann aus dem Pool für elastische Datenbanken heraus in eine Einzeldatenbank in Azure SQL-Datenbank verschoben.

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI in Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Beispielskript

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>Führen Sie das Skript aus.

[!code-azurecli-interactive[main](../../../../cli_scripts/sql-database/move-database-between-pools/move-database-between-pools.sh "Move database between pools")]

### <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Verwenden Sie den folgenden Befehl, um die Ressourcengruppe und alle dazugehörigen Ressourcen zu entfernen:

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Beispielreferenz

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | BESCHREIBUNG |
|---|---|
| [az sql server](/cli/azure/sql/server) | Serverbefehle |
| [az sql elastic-pools](/cli/azure/sql/elastic-pool) | Befehle für Pools für elastische Datenbanken |
| [az sql db](/cli/azure/sql/db) | Datenbankbefehle |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Zusätzliche SQL-Datenbank-CLI-Skriptbeispiele finden Sie in der [Azure SQL-Datenbank-Dokumentation](../az-cli-script-samples-content-guide.md).
