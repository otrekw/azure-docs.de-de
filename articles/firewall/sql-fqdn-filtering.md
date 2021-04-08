---
title: Konfigurieren von Azure Firewall-Anwendungsregeln mit SQL-FQDNs
description: In diesem Artikel erfahren Sie, wie SQL-FQDNs in Azure Firewall-Anwendungsregeln konfigurieren.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 06/18/2020
ms.author: victorh
ms.openlocfilehash: c65f32cc3ce56ddf3fd235de8c002528e7a3cebd
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98791441"
---
# <a name="configure-azure-firewall-application-rules-with-sql-fqdns"></a>Konfigurieren von Azure Firewall-Anwendungsregeln mit SQL-FQDNs

Sie können nun Azure Firewall-Anwendungsregeln mit SQL-FQDNs konfigurieren. So können Sie den Zugriff aus Ihren virtuellen Netzwerken auf die angegebenen SQL Server-Instanzen beschränken.

Mit SQL-FQDNs können Sie Datenverkehr wie folgt filtern:

- Von Ihren VNETs zu Azure SQL-Datenbank oder Azure Synapse Analytics. Beispiel: Sie haben die Möglichkeit, nur Zugriffe auf *sql-server1.database.windows.net* zuzulassen.
- Von Ihrer lokalen Umgebung an verwaltete Azure SQL-Instanzen oder an SQL-IaaS in Ihren VNETs
- Von Spoke-zu-Spoke an verwaltete Azure SQL-Instanzen oder an SQL-IaaS in Ihren VNETs

Die SQL-FQDN-Filterung wird nur im [Proxymodus](../azure-sql/database/connectivity-architecture.md#connection-policy) unterstützt (Port 1433). Wenn Sie SQL im standardmäßigen Umleitungsmodus verwenden, können Sie zur Filterung des Zugriffs das SQL-Diensttag im Rahmen von [Netzwerkregeln](features.md#network-traffic-filtering-rules) verwenden.
Wenn Sie nicht standardmäßige Ports für SQL-IaaS-Datenverkehr verwenden, können Sie diese Ports in den Firewallanwendungsregeln konfigurieren.

## <a name="configure-using-azure-cli"></a>Konfigurieren über die Azure-Befehlszeilenschnittstelle

1. [Stellen Sie eine Azure Firewall-Instanz unter Verwendung der Azure-Befehlszeilenschnittstelle bereit.](deploy-cli.md)
2. Wenn Sie Datenverkehr zu Azure SQL-Datenbank, Azure Synapse Analytics oder SQL Managed Instance filtern möchten, muss der SQL-Konnektivitätsmodus auf **Proxy** festgelegt sein. Informationen zum Ändern des SQL-Konnektivitätsmodus finden Sie unter [Azure SQL-Konnektivitätseinstellungen](../azure-sql/database/connectivity-settings.md#change-the-connection-policy-via-the-azure-cli).

   > [!NOTE]
   > Im SQL-Modus *Proxy* kommt es unter Umständen zu längeren Wartezeiten als im Modus *Umleiten*. Wenn Sie weiterhin den Umleitungsmodus (Standardmodus für Clients, die eine Verbindung innerhalb von Azure herstellen) verwenden möchten, können Sie den Zugriff mit dem SQL-[Diensttag](service-tags.md) in [Netzwerkregeln](tutorial-firewall-deploy-portal.md#configure-a-network-rule) der Firewall filtern.

3. Erstellen Sie eine neue Regelsammlung mit einer Anwendungsregel unter Verwendung des SQL-FQDN, um den Zugriff auf einen SQL-Server zu ermöglichen:

   ```azurecli
    az extension add -n azure-firewall
    
    az network firewall application-rule create \ 
    -g FWRG \
    --f azfirewall \ 
    --c sqlRuleCollection \
    --priority 1000 \
    --action Allow \
    --name sqlRule \
    --protocols mssql=1433 \
    --source-addresses 10.0.0.0/24 \
    --target-fqdns sql-serv1.database.windows.net
   ```

## <a name="configure-using-azure-powershell"></a>Konfigurieren mit Azure PowerShell

1. Stellen Sie eine [Azure Firewall-Instanz unter Verwendung von Azure PowerShell](deploy-ps.md) bereit.
2. Wenn Sie Datenverkehr zu Azure SQL-Datenbank, Azure Synapse Analytics oder SQL Managed Instance filtern möchten, muss der SQL-Konnektivitätsmodus auf **Proxy** festgelegt sein. Informationen zum Ändern des SQL-Konnektivitätsmodus finden Sie unter [Azure SQL-Konnektivitätseinstellungen](../azure-sql/database/connectivity-settings.md#change-the-connection-policy-via-the-azure-cli).

   > [!NOTE]
   > Im SQL-Modus *Proxy* kommt es unter Umständen zu längeren Wartezeiten als im Modus *Umleiten*. Wenn Sie weiterhin den Umleitungsmodus (Standardmodus für Clients, die eine Verbindung innerhalb von Azure herstellen) verwenden möchten, können Sie den Zugriff mit dem SQL-[Diensttag](service-tags.md) in [Netzwerkregeln](tutorial-firewall-deploy-portal.md#configure-a-network-rule) der Firewall filtern.

3. Erstellen Sie eine neue Regelsammlung mit einer Anwendungsregel unter Verwendung des SQL-FQDN, um den Zugriff auf einen SQL-Server zu ermöglichen:

   ```azurepowershell
   $AzFw = Get-AzFirewall -Name "azfirewall" -ResourceGroupName "FWRG"
    
   $sqlRule = @{
      Name          = "sqlRule"
      Protocol      = "mssql:1433" 
      TargetFqdn    = "sql-serv1.database.windows.net"
      SourceAddress = "10.0.0.0/24"
   }
    
   $rule = New-AzFirewallApplicationRule @sqlRule
    
   $sqlRuleCollection = @{
      Name       = "sqlRuleCollection" 
      Priority   = 1000 
      Rule       = $rule
      ActionType = "Allow"
   }
    
   $ruleCollection = New-AzFirewallApplicationRuleCollection @sqlRuleCollection
    
   $Azfw.ApplicationRuleCollections.Add($ruleCollection)    
   Set-AzFirewall -AzureFirewall $AzFw    
   ```

## <a name="configure-using-the-azure-portal"></a>Konfigurieren über das Azure-Portal
1. [Stellen Sie eine Azure Firewall-Instanz unter Verwendung der Azure-Befehlszeilenschnittstelle bereit.](deploy-cli.md)
2. Wenn Sie Datenverkehr zu Azure SQL-Datenbank, Azure Synapse Analytics oder SQL Managed Instance filtern möchten, muss der SQL-Konnektivitätsmodus auf **Proxy** festgelegt sein. Informationen zum Ändern des SQL-Konnektivitätsmodus finden Sie unter [Azure SQL-Konnektivitätseinstellungen](../azure-sql/database/connectivity-settings.md#change-the-connection-policy-via-the-azure-cli).  

   > [!NOTE]
   > Im SQL-Modus *Proxy* kommt es unter Umständen zu längeren Wartezeiten als im Modus *Umleiten*. Wenn Sie weiterhin den Umleitungsmodus (Standardmodus für Clients, die eine Verbindung innerhalb von Azure herstellen) verwenden möchten, können Sie den Zugriff mit dem SQL-[Diensttag](service-tags.md) in [Netzwerkregeln](tutorial-firewall-deploy-portal.md#configure-a-network-rule) der Firewall filtern.
3. Fügen Sie die Anwendungsregel mit den entsprechenden Angaben für Protokoll, Port und SQL-FQDN hinzu, und wählen Sie anschließend **Speichern** aus.
   ![Anwendungsregel mit SQL-FQDN](media/sql-fqdn-filtering/application-rule-sql.png)
4. Greifen Sie über einen virtuellen Computer in einem VNET mit firewallbasierter Datenverkehrsfilterung auf SQL zu. 
5. Vergewissern Sie sich anhand von [Azure Firewall-Protokollen](./firewall-workbook.md), dass der Datenverkehr zugelassen wird.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den SQL-Modi „Proxy“ und „Umleiten“ finden Sie unter [Verbindungsarchitektur von Azure SQL](../azure-sql/database/connectivity-architecture.md).