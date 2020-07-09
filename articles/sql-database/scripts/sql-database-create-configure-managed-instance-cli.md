---
title: 'Azure CLI: Erstellen einer verwalteten Instanz'
description: Azure CLI-Beispielskript zum Erstellen einer verwalteten Instanz in Azure SQL Managed Instance
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 03/25/2019
ms.openlocfilehash: fd2c7a33c0747aa21a8bda53ad5437c9e79a6557
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84115412"
---
# <a name="use-cli-to-create-an-azure-sql-managed-instance"></a>Erstellen einer Azure SQL Managed Instance-Instanz mithilfe der CLI

In diesem Azure CLI-Beispiel wird eine Azure SQL Managed Instance-Instanz in einem dedizierten Subnetz innerhalb eines neuen virtuellen Netzwerks erstellt. Außerdem werden eine Routingtabelle und eine Netzwerksicherheitsgruppe für das virtuelle Netzwerk konfiguriert. Nach erfolgreicher Ausführung des Skripts kann innerhalb des virtuellen Netzwerks oder über eine lokale Umgebung auf die verwaltete Instanz zugegriffen werden. Siehe [Schnellstart: Konfigurieren einer Azure-VM für das Herstellen einer Verbindung mit Azure SQL Managed Instance]../../azure-sql/managed-instance/connect-vm-instance-configure.md) sowie [Schnellstart: Konfigurieren einer Point-to-Site-Verbindung zwischen einer lokalen Umgebung und Azure SQL Managed Instance](../../azure-sql/managed-instance/point-to-site-p2s-configure.md).

> [!IMPORTANT]
> Informationen zu Einschränkungen finden Sie unter [unterstützte Regionen](../../azure-sql/managed-instance/resource-limits.md#supported-regions) und [unterstützte Abonnementtypen](../../azure-sql/managed-instance/resource-limits.md#supported-subscription-types).

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Artikel die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Beispielskript

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>Führen Sie das Skript aus.

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/managed-instance/create-managed-instance.sh "Create managed instance")]

### <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Verwenden Sie den folgenden Befehl, um die Ressourcengruppe und alle zugehörigen Ressourcen zu entfernen:

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Beispielreferenz

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| | |
|---|---|
| [az network vnet](/cli/azure/network/vnet) | Befehle für virtuelle Netzwerke |
| [az network vnet subnet](/cli/azure/network/vnet/subnet) | Befehle für Subnetze des virtuellen Netzwerks |
| [az network route-table](/cli/azure/network/route-table) | Befehle für Netzwerkroutingtabellen |
| [az sql mi](/cli/azure/sql/mi) | SQL Managed Instance-Befehle |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Zusätzliche SQL-Datenbank-CLI-Skriptbeispiele finden Sie in der [Azure SQL-Datenbank-Dokumentation](../../azure-sql/database/az-cli-script-samples-content-guide.md).
