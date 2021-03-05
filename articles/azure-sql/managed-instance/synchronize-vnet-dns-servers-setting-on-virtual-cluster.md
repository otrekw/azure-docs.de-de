---
title: Synchronisieren der Einstellung für DNS-Server virtueller Netzwerke in einem virtuellen SQL Managed Instance-Cluster
description: Erfahren Sie, wie Sie die Einstellung für DNS-Server virtueller Netzwerke in einem virtuellen SQL Managed Instance-Cluster synchronisieren.
services: sql-database
ms.service: sql-managed-instance
author: srdan-bozovic-msft
ms.author: srbozovi
ms.topic: how-to
ms.date: 01/17/2021
ms.openlocfilehash: b95afe513dba2f1da9556b27ec17bcccc9fe88e1
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102173550"
---
# <a name="synchronize-virtual-network-dns-servers-setting-on-sql-managed-instance-virtual-cluster"></a>Synchronisieren der Einstellung für DNS-Server virtueller Netzwerke in einem virtuellen SQL Managed Instance-Cluster
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

In diesem Artikel wird beschrieben, wie Sie die Einstellung für DNS-Server virtueller Netzwerke in einem virtuellen SQL Managed Instance-Cluster synchronisieren.

## <a name="when-to-synchronize-the-dns-setting"></a>Gründe für das Synchronisieren der DNS-Einstellung

Es gibt einige Szenarien (z. B. Datenbank-E-Mail, Verbindungsserver für andere SQL Server-Instanzen in Ihrer Cloud- oder Hybridumgebung), bei denen private Hostnamen von SQL Managed Instance aufgelöst werden müssen. In diesem Fall müssen Sie einen benutzerdefinierten DNS in Azure konfigurieren. Weitere Details finden Sie unter [Konfigurieren eines benutzerdefinierten DNS für Azure SQL Managed Instance](custom-dns-configure.md).

Wenn diese Änderung nach dem Erstellen des [virtuellen Clusters](connectivity-architecture-overview.md#virtual-cluster-connectivity-architecture) implementiert wird, der die verwaltete Instanz hostet, müssen Sie die DNS-Servereinstellung im virtuellen Cluster mit der Konfiguration des virtuellen Netzwerks synchronisieren.

> [!IMPORTANT]
> Das Synchronisieren der DNS-Servereinstellung wirkt sich auf alle verwalteten Instanzen aus, die im virtuellen Cluster gehostet werden.

## <a name="how-to-synchronize-the-dns-setting"></a>Synchronisieren der DNS-Einstellung

### <a name="azure-rbac-permissions-required"></a>Erforderliche Azure RBAC-Berechtigungen

Benutzer, die die DNS-Serverkonfiguration synchronisieren, benötigen eine der folgenden Azure-Rollen:

- Rolle „Besitzer des Abonnements“ oder
- Rolle „Mitwirkender“ der verwalteten Instanz oder
- Benutzerdefinierte Rolle mit der folgenden Berechtigung:
  - `Microsoft.Sql/virtualClusters/updateManagedInstanceDnsServers/action`

### <a name="use-azure-powershell"></a>Mithilfe von Azure PowerShell

Rufen Sie das virtuelle Netzwerk ab, in dem die DNS-Servereinstellung aktualisiert wurde.

```PowerShell
$ResourceGroup = 'enter resource group of virtual network'
$VirtualNetworkName = 'enter virtual network name'
$virtualNetwork = Get-AzVirtualNetwork -ResourceGroup $ResourceGroup -Name $VirtualNetworkName
```
Verwenden Sie den PowerShell-Befehl [Invoke-AzResourceAction](/powershell/module/az.resources/invoke-azresourceaction), um die DNS-Serverkonfiguration für alle virtuellen Cluster im Subnetz zu synchronisieren.

```PowerShell
Get-AzSqlVirtualCluster `
    | where SubnetId -match $virtualNetwork.Id `
    | select Id `
    | Invoke-AzResourceAction -Action updateManagedInstanceDnsServers -Force
```
### <a name="use-the-azure-cli"></a>Verwenden der Azure-CLI

Rufen Sie das virtuelle Netzwerk ab, in dem die DNS-Servereinstellung aktualisiert wurde.

```Azure CLI
resourceGroup="auto-failover-group"
virtualNetworkName="vnet-fog-eastus"
virtualNetwork=$(az network vnet show -g $resourceGroup -n $virtualNetworkName --query "id" -otsv)
```

Verwenden Sie den Azure CLI-Befehl [az resource invoke-action](/cli/azure/resource#az_resource_invoke_action), um die DNS-Serverkonfiguration für alle virtuellen Cluster im Subnetz zu synchronisieren.

```Azure CLI
az sql virtual-cluster list --query "[? contains(subnetId,'$virtualNetwork')].id" -o tsv \
    | az resource invoke-action --action updateManagedInstanceDnsServers --ids @-
```
## <a name="next-steps"></a>Nächste Schritte

- Unter [Konfigurieren eines benutzerdefinierten DNS für Azure SQL Managed Instance](custom-dns-configure.md) erfahren Sie mehr über das Konfigurieren eines benutzerdefinierten DNS.
- Eine Übersicht finden Sie unter [Was ist Azure SQL Managed Instance?](sql-managed-instance-paas-overview.md).
