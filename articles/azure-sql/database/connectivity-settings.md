---
title: Konnektivitätseinstellungen für Azure SQL-Datenbank und Data Warehouse
description: In diesem Dokument werden die TLS-Versionsauswahl (Transport Layer Security) und der Proxy erläutert im Vergleich mit der Umleitungseinstellung für Azure SQL-Datenbank und Azure Synapse Analytics
services: sql-database
ms.service: sql-database
titleSuffix: Azure SQL Database and SQL Data Warehouse
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: carlrab, vanto
ms.date: 07/06/2020
ms.openlocfilehash: 6297f7797758069ee75fe14960d72f9c497b0fe6
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2020
ms.locfileid: "87132975"
---
# <a name="azure-sql-connectivity-settings"></a>Azure SQL-Konnektivitätseinstellungen
[!INCLUDE[appliesto-sqldb-asa](../includes/appliesto-sqldb-asa.md)]

In diesem Artikel werden Einstellungen vorgestellt, die die Konnektivität mit dem Server für Azure SQL-Datenbank und Azure Synapse Analytics steuern. Diese Einstellungen gelten für **alle** SQL-Datenbanken und Azure Synapse-Datenbanken, die dem Server zugeordnet sind.

> [!IMPORTANT]
> Dieser Artikel gilt *nicht* für **Azure SQL Managed Instance**.

Die Konnektivitätseinstellungen können über den Bildschirm **Firewalls und virtuelle Netzwerke** aufgerufen werden, wie im folgenden Screenshot gezeigt:

 ![Screenshot der Konnektivitätseinstellungen][1]

> [!NOTE]
> Nachdem diese Einstellungen angewendet wurden, **werden sie sofort wirksam** und können zu einem Verbindungsverlust für Ihre Clients führen, wenn sie die Anforderungen für die einzelnen Einstellungen nicht erfüllen.

## <a name="deny-public-network-access"></a>Verweigern des Zugriffs auf öffentliches Netzwerk

Wenn die Einstellung **Zugriff auf öffentliches Netzwerk verweigern** auf **Ja** festgelegt ist, sind nur Verbindungen über private Endpunkte zulässig. Wenn diese Einstellung auf **Nein** (Standard) festgelegt ist, können Clients über öffentliche Endpunkte (IP-basierte Firewallregeln, VNET-basierte Firewallregeln) oder private Endpunkte (über Private Link) eine Verbindung herstellen, wie in der [Übersicht über den Netzwerkzugriff](network-access-controls-overview.md) beschrieben. 

 ![Screenshot der Konnektivität mit Verweigern des Zugriffs auf das öffentliche Netzwerk][2]

Bei allen Versuchen, die Einstellung **Zugriff auf öffentliches Netzwerk verweigern** auf **Ja** festzulegen, ohne dass auf dem logischen Server private Endpunkte vorhanden sind, wird etwa folgende Fehlermeldung angezeigt:  

```output
Error 42102
Unable to set Deny Public Network Access to Yes since there is no private endpoint enabled to access the server. 
Please set up private endpoints and retry the operation. 
```

Wenn die Einstellung **Zugriff auf öffentliches Netzwerk verweigern** auf **Ja** festgelegt ist, sind nur Verbindungen über private Endpunkte zulässig, und alle Verbindungen über öffentliche Endpunkte werden mit einer Fehlermeldung wie der folgenden verweigert:  

```output
Error 47073
An instance-specific error occurred while establishing a connection to SQL Server. 
The public network interface on this server is not accessible. 
To connect to this server, use the Private Endpoint from inside your virtual network.
```

Wenn die Einstellung **Zugriff auf öffentliches Netzwerk verweigern** auf **Ja** gesetzt wird, werden alle Versuche, Firewallregeln hinzuzufügen oder zu aktualisieren, mit einer Fehlermeldung ähnlich der Folgenden abgelehnt:

```output
Error 42101
Unable to create or modify firewall rules when public network interface for the server is disabled. 
To manage server or database level firewall rules, please enable the public network interface.
```

## <a name="change-public-network-access-via-powershell"></a>Ändern des Zugriffs auf öffentliche Netzwerke über PowerShell

> [!IMPORTANT]
> Das PowerShell Azure Resource Manager-Modul wird von Azure SQL-Datenbank weiterhin unterstützt, aber alle zukünftigen Entwicklungen erfolgen für das Az.Sql-Modul. Informationen zu diesen Cmdlets finden Sie unter [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Die Argumente für die Befehle im Az-Modul und den AzureRm-Modulen sind im Wesentlichen identisch. Das folgende Skript erfordert das [Azure PowerShell-Modul](/powershell/azure/install-az-ps).

Das folgende PowerShell-Skript zeigt, wie Sie die **Public Network Access**-Eigenschaft auf der Serverebene mit `Get` und `Set` abrufen und festlegen können:

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

## <a name="minimal-tls-version"></a>TLS-Mindestversion 

Die Einstellung der Mindestversion für [Transport Layer Security (TLS)](https://support.microsoft.com/help/3135244/tls-1-2-support-for-microsoft-sql-server) ermöglicht Kunden die Steuerung der TLS-Version, die von ihrer Azure SQL-Datenbank-Instanz verwendet wird.

Derzeit unterstützen wir TLS 1.0, 1.1 und 1.2. Durch Festlegen einer TLS-Mindestversion wird sichergestellt, dass nachfolgende neuere TLS-Versionen unterstützt werden. Beispiel: Die Auswahl einer höheren TLS-Version als 1.1. bedeutet, dass nur Verbindungen mit TLS 1.1 und 1.2 akzeptiert werden. Verbindungen mit TLS 1.0 werden abgelehnt. Wenn Sie überprüft haben, ob Ihre Anwendungen die TLS-Version 1.2 unterstützen, sollten Sie „Mindestens erforderliche TLS-Version“ auf 1.2 festzulegen, da diese Version Fehlerbehebungen für Sicherheitsrisiken enthält, die in früheren Versionen gefunden wurden. Gleichzeitig ist 1.2 die höchste TLS-Version, die derzeit in Azure SQL-Datenbank unterstützt wird.

> [!IMPORTANT]
> Standardmäßig sind für „Mindestens erforderliche TLS-Version“ alle Versionen zugelassen. Wenn Sie jedoch eine Version von TLS erzwingen, können Sie nicht mehr zum Standardwert zurück.

Kunden mit Anwendungen, die ältere TLS-Versionen erfordern, wird empfohlen, die TLS-Mindestversion gemäß den Anforderungen der Anwendungen festzulegen. Kunden, die Anwendungen mit unverschlüsselten Verbindungen verwenden, wird empfohlen, keine TLS-Mindestversion festzulegen.

Weitere Informationen finden Sie unter [Überlegungen zu TLS für Verbindungen mit einer SQL-Datenbank](connect-query-content-reference-guide.md#tls-considerations-for-database-connectivity).

Nachdem Sie eine TLS-Mindestversion für den Server festgelegt haben, tritt bei Anmeldeversuchen von Clients, die eine niedrigere TLS-Version verwenden, folgender Fehler auf:

```output
Error 47072
Login failed with invalid TLS version
```

## <a name="set-minimal-tls-version-via-powershell"></a>Festlegen einer TLS-Mindestversion über PowerShell

> [!IMPORTANT]
> Das PowerShell Azure Resource Manager-Modul wird von Azure SQL-Datenbank weiterhin unterstützt, aber alle zukünftigen Entwicklungen erfolgen für das Az.Sql-Modul. Informationen zu diesen Cmdlets finden Sie unter [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Die Argumente für die Befehle im Az-Modul und den AzureRm-Modulen sind im Wesentlichen identisch. Das folgende Skript erfordert das [Azure PowerShell-Modul](/powershell/azure/install-az-ps).

Das folgende PowerShell-Skript zeigt, wie Sie die Eigenschaft **TLS-Mindestversion** auf der Ebene des logischen Servers mit `Get` und `Set` abrufen und festlegen können:

```powershell
#Get the Minimal TLS Version property
(Get-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).MinimalTlsVersion

# # Update Minimal TLS Version to 1.2
$SecureString = ConvertTo-SecureString "password" -AsPlainText -Force

Set-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group -SqlAdministratorPassword $SecureString  -MinimalTlsVersion "1.2"
```

## <a name="set-minimal-tls-version-via-azure-cli"></a>Festlegen einer TLS-Mindestversion über die Azure CLI

> [!IMPORTANT]
> Für alle Skripts in diesem Abschnitt ist die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) erforderlich.

### <a name="azure-cli-in-a-bash-shell"></a>Azure CLI in einer Bash-Shell

Das folgende CLI-Skript veranschaulicht, wie Sie die Einstellung **TLS-Mindestversion** in einer Bash-Shell ändern:

```azurecli-interactive
# Get current setting for Minimal TLS Version
az sql server show -n sql-server-name -g sql-server-group --query "minimalTlsVersion"

# Update setting for Minimal TLS Version
az sql server update -n sql-server-name -g sql-server-group --set minimalTlsVersion="1.2"
```


## <a name="change-connection-policy"></a>Ändern der Verbindungsrichtlinie

[Verbindungsrichtlinien](connectivity-architecture.md#connection-policy) bestimmen, wie Clients eine Verbindung mit Azure SQL-Datenbank herstellen.


## <a name="change-connection-policy-via-powershell"></a>Ändern der Verbindungsrichtlinie über PowerShell

> [!IMPORTANT]
> Das PowerShell Azure Resource Manager-Modul wird von Azure SQL-Datenbank weiterhin unterstützt, aber alle zukünftigen Entwicklungen erfolgen für das Az.Sql-Modul. Informationen zu diesen Cmdlets finden Sie unter [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Die Argumente für die Befehle im Az-Modul und den AzureRm-Modulen sind im Wesentlichen identisch. Das folgende Skript erfordert das [Azure PowerShell-Modul](/powershell/azure/install-az-ps).

Das folgende PowerShell-Skript veranschaulicht, wie Sie die Verbindungsrichtlinie mit PowerShell ändern können:

```powershell
# Get SQL Server ID
$sqlserverid=(Get-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).ResourceId

# Set URI
$id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
(Get-AzResource -ResourceId $id -ApiVersion 2014-04-01 -Verbose).Properties.ConnectionType

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

- Eine Übersicht über die Funktionsweise von Konnektivität in Azure SQL-Datenbank finden Sie unter [Verbindungsarchitektur](connectivity-architecture.md).
- Informationen zum Ändern der Verbindungsrichtlinie für einen Server finden Sie unter [conn-policy](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).

<!--Image references-->
[1]: media/single-database-create-quickstart/manage-connectivity-settings.png
[2]: media/single-database-create-quickstart/manage-connectivity-flowchart.png
