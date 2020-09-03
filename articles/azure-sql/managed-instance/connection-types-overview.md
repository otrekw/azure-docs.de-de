---
title: Verbindungstypen
titleSuffix: Azure SQL Managed Instance
description: Erfahren Sie mehr über Verbindungstypen für Azure SQL Managed Instance
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto
ms.date: 10/07/2019
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: e46c6d1c14d226522a1d534418b91076efeaaccf
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89070716"
---
# <a name="azure-sql-managed-instance-connection-types"></a>Azure SQL Managed Instance: Verbindungstypen
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

In diesem Artikel wird erläutert, wie Clients abhängig vom Verbindungstyp eine Verbindung mit Azure SQL Managed Instance herstellen. Skriptbeispiele zum Ändern von Verbindungstypen finden Sie weiter unten, ebenso wie Überlegungen zum Ändern der Verbindungsstandardeinstellungen.

## <a name="connection-types"></a>Verbindungstypen

Azure SQL Managed Instance unterstützt die zwei folgenden Verbindungstypen:

- **Umleiten (empfohlen):** Clients stellen Verbindungen direkt mit dem Knoten her, der die Datenbank hostet. Zum Aktivieren der Konnektivität mithilfe von Umleitung müssen Sie Firewalls und Netzwerksicherheitsgruppen (NSG) öffnen, um den Zugriff auf die Ports 1433 und 11000 bis 11999 zuzulassen. Pakete werden direkt an die Datenbank gesendet. Daher gibt es Latenz- und Durchsatzleistungsverbesserungen bei der Umleitung im Vergleich zum Proxy.
- **Proxy (Standardeinstellung):** In diesem Modus verwenden alle Verbindungen eine Proxygatewaykomponente. Zum Aktivieren der Konnektivität müssen nur Port 1433 für private Netzwerke und Port 3342 für eine öffentliche Verbindung geöffnet werden. Dieser Modus kann je nach Workload höhere Latenzen und geringeren Durchsatz verursachen. Es wird empfohlen, die Verbindungsrichtlinie „Umleiten“ statt der Verbindungsrichtlinie „Proxy“ zu verwenden, um die niedrigste Latenz und den höchsten Durchsatz zu erzielen.

## <a name="redirect-connection-type"></a>Umleitungsverbindungstyp

Im Umleitungsverbindungstyp ruft die Clientsitzung nach dem Einrichten der TCP-Sitzung mit der SQL-Engine die virtuelle IP-Zieladresse des virtuellen Clusterknotens vom Lastenausgleich ab. Nachfolgende Pakete gelangen unter Umgehung des Gateways direkt zum virtuellen Clusterknoten. Das folgende Diagramm veranschaulicht diesen Datenverkehrfluss.

![redirect.png](./media/connection-types-overview/redirect.png)

> [!IMPORTANT]
> Der Umleitungsverbindungstyp funktioniert zurzeit nur für einen privaten Endpunkt. Unabhängig von der Verbindungstypeinstellung erfolgen Verbindungen über den öffentlichen Endpunkt über einen Proxy.

## <a name="proxy-connection-type"></a>Proxyverbindungstyp

Im Proxyverbindungstyp wird die TCP-Sitzung über das Gateway eingerichtet und alle nachfolgenden Pakete durchlaufen es. Das folgende Diagramm veranschaulicht diesen Datenverkehrfluss.

![proxy.png](./media/connection-types-overview/proxy.png)

## <a name="changing-connection-type"></a>Ändern des Verbindungstyps

- **Verwenden des Portals:** Um den Verbindungstyp mithilfe des Azure-Portals zu ändern, öffnen Sie die Seite „Virtuelles Netzwerk“, und verwenden Sie die Einstellung **Verbindungstyp**, um den Verbindungstyp zu ändern und die Änderungen zu speichern.

- **Skript zum Ändern der Verbindungstypeinstellungen mit PowerShell:**

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Das folgende PowerShell-Skript zeigt, wie der Verbindungstyp für eine verwaltete Instanz in `Redirect` geändert werden kann.

```powershell
Install-Module -Name Az
Import-Module Az.Accounts
Import-Module Az.Sql

Connect-AzAccount
# Get your SubscriptionId from the Get-AzSubscription command
Get-AzSubscription
# Use your SubscriptionId in place of {subscription-id} below
Select-AzSubscription -SubscriptionId {subscription-id}
# Replace {rg-name} with the resource group for your managed instance, and replace {mi-name} with the name of your managed instance
$mi = Get-AzSqlInstance -ResourceGroupName {rg-name} -Name {mi-name}
$mi = $mi | Set-AzSqlInstance -ProxyOverride "Redirect" -force
```

## <a name="next-steps"></a>Nächste Schritte

- [Wiederherstellen einer Datenbank in SQL Managed Instance](restore-sample-database-quickstart.md)
- Weitere Informationen zum [Konfigurieren eines öffentlichen Endpunkts für SQL Managed Instance](public-endpoint-configure.md)
- Weitere Informationen zur [Konnektivitätsarchitektur für SQL Managed Instance](connectivity-architecture-overview.md)
