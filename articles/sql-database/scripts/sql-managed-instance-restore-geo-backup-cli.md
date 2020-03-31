---
title: 'CLI-Beispiel: Wiederherstellen einer Geosicherung – Azure SQL-Datenbank'
description: Azure CLI-Beispielskript zum Wiederherstellen einer Datenbank einer verwalteten Azure SQL-Instanz aus einer georedundanten Sicherung.
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 07/03/2019
ms.openlocfilehash: 1bd0322aee83fb980c60382a2ff3eaab1cd1313c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "80061750"
---
# <a name="use-cli-to-restore-a-managed-instance-database-to-another-geo-region"></a>Wiederherstellen einer Datenbank einer verwalteten Instanz in einer anderen geografischen Region per CLI

Mit diesem Azure CLI-Beispielskript wird eine Datenbank einer verwalteten Azure SQL-Instanz aus einer geografischen Remoteregion wiederhergestellt (Geowiederherstellung).  

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Artikel die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Beispielskript

### <a name="prerequisites"></a>Voraussetzungen

Informationen zu einem vorhandenen Paar mit verwalteten Instanzen finden Sie unter [Verwenden der Azure CLI zum Erstellen einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-create-configure-managed-instance-cli.md).

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>Führen Sie das Skript aus.

```azurepowershell-interactive
#!/bin/bash
$instance = "<instanceId>" # add instance here
$targetInstance = "<targetInstanceId>" # add target instance here
$resource = "<resourceId>" # add resource here

$randomIdentifier = $(Get-Random)
$managedDatabase = "managedDatabase-$randomIdentifier"

echo "Creating $($managedDatabase) on $($instance)..."
az sql midb create -g $resource --mi $instance -n $managedDatabase

echo "Restoring $($managedDatabase) to $($targetInstance)..."
az sql midb restore -g $resource --mi $instance -n $managedDatabase --dest-name $targetInstance --time "2018-05-20T05:34:22"
```

## <a name="sample-reference"></a>Beispielreferenz

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| | |
|---|---|
| [az sql midb](/cli/azure/sql/midb) | Befehle für Datenbanken verwalteter Instanzen |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Zusätzliche SQL-Datenbank-CLI-Skriptbeispiele finden Sie in der [Azure SQL-Datenbank-Dokumentation](../sql-database-cli-samples.md).
