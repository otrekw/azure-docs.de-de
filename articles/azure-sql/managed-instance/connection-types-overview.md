---
title: Verbindungstypen
titleSuffix: Azure SQL Managed Instance
description: Erfahren Sie mehr über Verbindungstypen für Azure SQL Managed Instance
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto
ms.date: 10/07/2019
ms.openlocfilehash: cee913e846ebfef174a3cd6383401eace89187f0
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84030101"
---
# <a name="azure-sql-managed-instance-connection-types"></a>Azure SQL Managed Instance: Verbindungstypen
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

In diesem Artikel wird erläutert, wie Clients abhängig vom Verbindungstyp eine Verbindung mit Azure SQL Managed Instance herstellen. Skriptbeispiele zum Ändern von Verbindungstypen finden Sie weiter unten, ebenso wie Überlegungen zum Ändern der Verbindungsstandardeinstellungen.

## <a name="connection-types"></a>Verbindungstypen

Azure SQL Managed Instance unterstützt die zwei folgenden Verbindungstypen:

- **Umleiten (empfohlen):** Clients stellen Verbindungen direkt mit dem Knoten her, der die Datenbank hostet. Zum Aktivieren der Konnektivität mithilfe von Umleitung müssen Sie Firewalls und Netzwerksicherheitsgruppen (NSG) öffnen, um den Zugriff auf die Ports 1433 und 11000 bis 11999 zuzulassen. Pakete werden direkt an die Datenbank gesendet. Daher gibt es Latenz- und Durchsatzleistungsverbesserungen bei der Umleitung im Vergleich zum Proxy.
- **Proxy (Standardeinstellung):** In diesem Modus verwenden alle Verbindungen eine Proxygatewaykomponente. Zum Aktivieren der Konnektivität müssen nur Port 1433 für private Netzwerke und Port 3342 für eine öffentliche Verbindung geöffnet werden. Dieser Modus kann je nach Workload höhere Latenzen und geringeren Durchsatz verursachen. Es wird empfohlen, die Verbindungsrichtlinie „Umleiten“ statt der Verbindungsrichtlinie „Proxy“ zu verwenden, um die niedrigste Latenz und den höchsten Durchsatz zu erzielen.

## <a name="redirect-connection-type"></a>Umleitungsverbindungstyp

Der Umleitungsverbindungstyp bedeutet, dass die Clientsitzung nach dem Einrichten der TCP-Sitzung mit der SQL-Engine die virtuelle IP-Zieladresse des virtuellen Clusterknotens vom Lastenausgleich abruft. Nachfolgende Pakete gelangen unter Umgehung des Gateways direkt zum virtuellen Clusterknoten. Das folgende Diagramm veranschaulicht diesen Datenverkehrfluss.

![redirect.png](./media/connection-types-overview/redirect.png)

> [!IMPORTANT]
> Der Umleitungsverbindungstyp funktioniert zurzeit nur für den privaten Endpunkt. Unabhängig von der Verbindungstypeinstellung erfolgen Verbindungen über den öffentlichen Endpunkt über einen Proxy.

## <a name="proxy-connection-type"></a>Proxyverbindungstyp

Der Proxyverbindungstyp bedeutet, dass die TCP-Sitzung über das Gateway eingerichtet wird und alle nachfolgenden Pakete dieses durchlaufen. Das folgende Diagramm veranschaulicht diesen Datenverkehrfluss.

![proxy.png](./media/connection-types-overview/proxy.png)

## <a name="script-to-change-connection-type-settings-using-powershell"></a>Skript zum Ändern der Verbindungstypeinstellungen mit PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Das folgende PowerShell-Skript zeigt, wie der Verbindungstyp für eine SQL Managed Instance in `Redirect` geändert werden kann.

```powershell
Install-Module -Name Az
Import-Module Az.Accounts
Import-Module Az.Sql

Connect-AzAccount
# Get your SubscriptionId from the Get-AzSubscription command
Get-AzSubscription
# Use your SubscriptionId in place of {subscription-id} below
Select-AzSubscription -SubscriptionId {subscription-id}
# Replace {rg-name} with the resource group for your SQL Managed Instance, and replace {mi-name} with the name of your SQL Managed Instance
$mi = Get-AzSqlInstance -ResourceGroupName {rg-name} -Name {mi-name}
$mi = $mi | Set-AzSqlInstance -ProxyOverride "Redirect" -force
```

## <a name="next-steps"></a>Nächste Schritte

- [Wiederherstellen einer Datenbank in einer verwalteten SQL-Instanz](restore-sample-database-quickstart.md)
- Weitere Informationen zum [Konfigurieren eines öffentlichen Endpunkts für SQL Managed Instance](public-endpoint-configure.md)
- Weitere Informationen zur [Konnektivitätsarchitektur für SQL Managed Instance](connectivity-architecture-overview.md)