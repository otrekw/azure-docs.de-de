---
title: 'CLI-Beispiel: Verschieben von Azure SQL-Datenbank – Pool für elastische SQL-Datenbanken'
description: Azure CLI-Beispielskript zum Verschieben einer SQL-Datenbank in einem Pool für elastische SQL-Datenbanken
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 06/25/2019
ms.openlocfilehash: b50f873977357522d2db87fd5132176b55874fbf
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "80061784"
---
# <a name="use-cli-to-move-an-azure-sql-database-in-a-sql-elastic-pool"></a>Verwenden Sie zum Verschieben einer Azure SQL-Datenbank in einem Pool für elastische SQL-Datenbanken eine Befehlszeilenschnittstelle.

Dieses Azure CLI-Beispielskript erstellt zwei Pools für elastische Datenbanken, verschiebt eine Azure SQL-Datenbank aus einem Pool für elastische SQL-Datenbanken in einen anderen und verschiebt die Datenbank dann aus dem Pool für elastische Datenbanken in eine Einzeldatenbank-Computegröße.

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI in Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Beispielskript

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>Führen Sie das Skript aus.

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/move-database-between-pools/move-database-between-pools.sh "Move database between pools")]

### <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Verwenden Sie den folgenden Befehl, um die Ressourcengruppe und alle dazugehörigen Ressourcen zu entfernen:

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Beispielreferenz

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| | |
|---|---|
| [az sql server](/cli/azure/sql/server) | Serverbefehle |
| [az sql elastic-pools](/cli/azure/sql/elastic-pool) | Befehle für Pools für elastische Datenbanken |
| [az sql db](/cli/azure/sql/db) | Datenbankbefehle |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Zusätzliche SQL-Datenbank-CLI-Skriptbeispiele finden Sie in der [Azure SQL-Datenbank-Dokumentation](../sql-database-cli-samples.md).
