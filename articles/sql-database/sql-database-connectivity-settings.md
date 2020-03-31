---
title: Konnektivitätseinstellungen für Azure SQL-Datenbank und Data Warehouse
description: In diesem Dokument wird die TLS-Versionsauswahl und der Proxy im Vergleich zur Umleitungseinstellung für Azure SQL behandelt.
services: sql-database
ms.service: sql-database
titleSuffix: Azure SQL Database and SQL Data Warehouse
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: carlrab, vanto
ms.date: 03/09/2020
ms.openlocfilehash: d18fdee85bd0fbabe68fe9890c4a2dc74366041d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79366086"
---
# <a name="azure-sql-connectivity-settings"></a>Azure SQL-Konnektivitätseinstellungen
> [!NOTE]
> Dieser Artikel gilt für den Azure SQL-Datenbankserver sowie für Datenbanken von SQL-Datenbank und SQL Data Warehouse, die auf dem Azure SQL-Datenbankserver erstellt werden. Der Einfachheit halber wird nur SQL-Datenbank verwendet, wenn sowohl SQL-Datenbank als auch SQL Data Warehouse gemeint sind.

> [!IMPORTANT]
> Dieser Artikel gilt *nicht* für **verwaltete Azure SQL-Datenbank-Instanzen**.

In diesem Artikel werden Einstellungen vorgestellt, die die Konnektivität mit Azure SQL-Datenbank auf Serverebene steuern. Diese Einstellungen gelten für **alle** SQL-Datenbanken und SQL Data Warehouse-Datenbanken, die dem Server zugeordnet sind.

> [!NOTE]
> Nachdem diese Einstellungen angewendet wurden, **werden sie sofort wirksam** und können zu einem Verbindungsverlust für Ihre Clients führen, wenn sie die Anforderungen für die einzelnen Einstellungen nicht erfüllen.

Die Konnektivitätseinstellungen können über das Blatt **Firewalls und virtuelle Netzwerke** aufgerufen werden, wie im folgenden Screenshot gezeigt:

 ![Screenshot der Konnektivitätseinstellungen][1]


## <a name="deny-public-network-access"></a>Verweigern des Zugriffs auf öffentliches Netzwerk
Wenn im Azure-Portal die Einstellung **Zugriff auf öffentliches Netzwerk verweigern** auf **Ja** festgelegt ist, sind nur Verbindungen über private Endpunkte zulässig. Wenn diese Einstellung auf **Nein** festgelegt ist, können Clients mithilfe des privaten oder öffentlichen Endpunkts eine Verbindung herstellen.

Nach dem Festlegen von **Zugriff auf öffentliches Netzwerk verweigern** auf **Ja** treten bei Anmeldeversuchen von Clients über den öffentlichen Endpunkt folgende Fehler auf:

```output
Error 47073
An instance-specific error occurred while establishing a connection to SQL Server. The public network interface on this server is not accessible. To connect to this server, use the Private Endpoint from inside your virtual network.
```

## <a name="change-public-network-access-via-powershell"></a>Ändern des Zugriffs auf öffentliche Netzwerke über PowerShell
[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Das PowerShell Azure Resource Manager-Modul wird von Azure SQL-Datenbank weiterhin unterstützt, aber alle zukünftigen Entwicklungen erfolgen für das Az.Sql-Modul. Informationen zu diesen Cmdlets finden Sie unter [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Die Argumente für die Befehle im Az-Modul und den AzureRm-Modulen sind im Wesentlichen identisch. Das folgende Skript erfordert das [Azure PowerShell-Modul](/powershell/azure/install-az-ps).

Das folgende PowerShell-Skript zeigt, wie Sie die **Public Network Access**-Eigenschaft auf der logischen Serverebene mit `Get` und `Set` abrufen und festlegen können:

```powershell
#Get the Public Network Access property
(Get-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).PublicNetworkAccess

# Update Public Network Access to Disabled
$SecureString = ConvertTo-SecureString "password" -AsPlainText -Force

Set-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group -SqlAdministratorPassword $SecureString -PublicNetworkAccess "Enabled" 
```

## <a name="change-public-network-access-via-cli"></a>Ändern des Zugriffs auf öffentliche Netzwerke über die CLI
> [!IMPORTANT]
> Für alle Skripts in diesem Abschnitt ist die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) erforderlich.

### <a name="azure-cli-in-a-bash-shell"></a>Azure CLI in einer Bash-Shell
Das folgende CLI-Skript veranschaulicht, wie Sie den **Zugriff auf ein öffentliches Netzwerk** in einer Bash-Shell ändern:

```azurecli-interactive

# Get current setting for Public Network Access
az sql server show -n sql-server-name -g sql-server-group --query "publicNetworkAccess"

# Update setting for Public Network Access
az sql server update -n sql-server-name -g sql-server-group --set publicNetworkAccess="Disabled"

```


## <a name="connection-policy"></a>Verbindungsrichtlinie
[Verbindungsrichtlinien](sql-database-connectivity-architecture.md#connection-policy) bestimmen, wie Clients eine Verbindung mit Azure SQL Server herstellen. 

## <a name="change-connection-policy-via-powershell"></a>Ändern der Verbindungsrichtlinie über PowerShell
[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Das PowerShell Azure Resource Manager-Modul wird von Azure SQL-Datenbank weiterhin unterstützt, aber alle zukünftigen Entwicklungen erfolgen für das Az.Sql-Modul. Informationen zu diesen Cmdlets finden Sie unter [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Die Argumente für die Befehle im Az-Modul und den AzureRm-Modulen sind im Wesentlichen identisch. Das folgende Skript erfordert das [Azure PowerShell-Modul](/powershell/azure/install-az-ps).

Das folgende PowerShell-Skript veranschaulicht, wie Sie die Verbindungsrichtlinie mit PowerShell ändern können:

```powershell
# Get SQL Server ID
$sqlserverid=(Get-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).ResourceId

# Set URI
$id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
(Get-AzResource -ResourceId $id).Properties.connectionType

# Update connection policy
Set-AzResource -ResourceId $id -Properties @{"connectionType" = "Proxy"} -f
```

## <a name="change-connection-policy-via-azure-cli"></a>Ändern der Verbindungsrichtlinie über die Azure CLI
> [!IMPORTANT]
> Für alle Skripts in diesem Abschnitt ist die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) erforderlich.

### <a name="azure-cli-in-a-bash-shell"></a>Azure CLI in einer Bash-Shell
Das folgende CLI-Skript veranschaulicht, wie Sie die Verbindungsrichtlinie in einer Bash-Shell ändern: 

```azurecli-interactive
# Get SQL Server ID
sqlserverid=$(az sql server show -n sql-server-name -g sql-server-group --query 'id' -o tsv)

# Set URI
ids="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
az resource show --ids $ids

# Update connection policy
az resource update --ids $ids --set properties.connectionType=Proxy
```

### <a name="azure-cli-from-a-windows-command-prompt"></a>Azure CLI über eine Windows-Eingabeaufforderung
Das folgende CLI-Skript veranschaulicht, wie Sie die Verbindungsrichtlinie über eine Windows-Eingabeaufforderung ändern (wenn die Azure CLI installiert ist).

```azurecli
# Get SQL Server ID and set URI
FOR /F "tokens=*" %g IN ('az sql server show --resource-group myResourceGroup-571418053 --name server-538465606 --query "id" -o tsv') do (SET sqlserverid=%g/connectionPolicies/Default)

# Get current connection policy
az resource show --ids %sqlserverid%

# Update connection policy
az resource update --ids %sqlserverid% --set properties.connectionType=Proxy
```

## <a name="next-steps"></a>Nächste Schritte
- Eine Übersicht über die Funktionsweise von Konnektivität in Azure SQL-Datenbank finden Sie unter [Verbindungsarchitektur von Azure SQL-Datenbank](sql-database-connectivity-architecture.md).
- Informationen zum Ändern der Verbindungsrichtlinie von Azure SQL-Datenbank für einen Azure SQL-Datenbank-Server finden Sie unter [conn-policy](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).

<!--Image references-->
[1]: ./media/sql-database-get-started-portal/manage-connectivity-settings.png
