---
title: 'Mit PowerShell: Erstellen einer verwalteten Instanz'
titleSuffix: Azure SQL Managed Instance
description: Dieser Artikel enthält ein Azure PowerShell-Beispielskript zum Erstellen einer verwalteten Instanz.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/25/2019
ms.openlocfilehash: fd091cedb67c07b7de2c7e8540e99c3e8daf7dcf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91323753"
---
# <a name="use-powershell-to-create-a-managed-instance"></a>Verwenden von PowerShell zum Erstellen einer verwalteten Instanz

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqlmi.md)]

In diesem PowerShell-Skriptbeispiel wird eine verwaltete Instanz in einem dedizierten Subnetz innerhalb eines neuen virtuellen Netzwerks erstellt. Außerdem werden eine Routingtabelle und eine Netzwerksicherheitsgruppe für das virtuelle Netzwerk konfiguriert. Nach erfolgreicher Ausführung des Skripts kann innerhalb des virtuellen Netzwerks oder über eine lokale Umgebung auf die verwaltete Instanz zugegriffen werden. Siehe [Konfigurieren einer Azure-VM zum Herstellen einer Verbindung mit einer verwalteten Azure SQL-Datenbank-Instanz](../connect-vm-instance-configure.md) sowie [Konfigurieren einer Point-to-Site-Verbindung zwischen einem lokalen Computer und einer verwalteten Azure SQL-Instanz](../point-to-site-p2s-configure.md).

> [!IMPORTANT]
> Informationen zu Einschränkungen finden Sie unter [unterstützte Regionen](../resource-limits.md#supported-regions) und [unterstützte Abonnementtypen](../resource-limits.md#supported-subscription-types).

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Tutorial mindestens die Version 1.4.0 von Azure PowerShell verwenden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="sample-script"></a>Beispielskript

[!code-powershell-interactive[main](../../../../powershell_scripts/sql-database/managed-instance/create-and-configure-managed-instance.ps1 "Create managed instance")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Verwenden Sie den folgenden Befehl, um die Ressourcengruppe und alle dazugehörigen Ressourcen zu entfernen:

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet einige der folgenden Befehle. Wenn Sie weitere Informationen zu den verwendeten und anderen Befehlen in der nachstehenden Tabelle benötigen, klicken Sie auf die Links zur befehlsspezifischen Dokumentation.

| Get-Help | Notizen |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind.
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Erstellen Sie ein virtuelles Netzwerk. |
| [Add-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/Add-AzVirtualNetworkSubnetConfig) | Fügt einem virtuellen Netzwerk eine Subnetzkonfiguration hinzu |
| [Get-AzVirtualNetwork](/powershell/module/az.network/Get-AzVirtualNetwork) | Ruft ein virtuelles Netzwerk in einer Ressourcengruppe ab |
| [Set-AzVirtualNetwork](/powershell/module/az.network/Set-AzVirtualNetwork) | Legt den Zielzustand für ein virtuelles Netzwerk fest. |
| [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/Get-AzVirtualNetworkSubnetConfig) | Ruft ein Subnetz in einem virtuellen Netzwerk ab |
| [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/Set-AzVirtualNetworkSubnetConfig) | Konfiguriert den Zielzustand für eine Subnetzkonfiguration in einem virtuellen Netzwerk. |
| [New-AzRouteTable](/powershell/module/az.network/New-AzRouteTable) | Erstellt eine Routingtabelle |
| [Get-AzRouteTable](/powershell/module/az.network/Get-AzRouteTable) | Ruft Routingtabellen ab. |
| [Set-AzRouteTable](/powershell/module/az.network/Set-AzRouteTable) | Legt den Zielzustand für eine Routingtabelle fest. |
| [New-AzSqlInstance](/powershell/module/az.sql/New-AzSqlInstance) | Erstellt eine verwaltete Instanz. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/).

Weitere PowerShell-Skriptbeispiele für die verwaltete Azure SQL-Instanz finden Sie unter [Skriptbeispiele für die verwaltete Azure SQL-Instanz](../../database/powershell-script-content-guide.md).
