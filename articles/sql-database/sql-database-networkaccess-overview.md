---
title: Kontrolle des Netzwerkzugriffs
description: Übersicht über die Netzwerkzugriffssteuerung für Azure SQL-Datenbank und Data Warehouse, um den Zugriff zu verwalten und eine einzelne oder eine Pooldatenbank zu konfigurieren.
services: sql-database
ms.service: sql-database
ms.subservice: security
titleSuffix: Azure SQL Database and SQL Data Warehouse
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: vanto
ms.date: 03/09/2020
ms.openlocfilehash: 822fab5c00501d415c3c184587141e869523e417
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78945392"
---
# <a name="azure-sql-database-and-data-warehouse-network-access-controls"></a>Netzwerkzugriffssteuerung für Azure SQL-Datenbank und Data Warehouse

> [!NOTE]
> Dieser Artikel gilt für den Azure SQL-Datenbankserver sowie für Datenbanken von SQL-Datenbank und SQL Data Warehouse, die auf dem Azure SQL-Datenbankserver erstellt werden. Der Einfachheit halber wird nur SQL-Datenbank verwendet, wenn sowohl SQL-Datenbank als auch SQL Data Warehouse gemeint sind.

> [!IMPORTANT]
> Dieser Artikel gilt *nicht* für **verwaltete Azure SQL-Datenbank-Instanzen**. Weitere Informationen zur Netzwerkkonfiguration finden Sie unter [Herstellen einer Verbindung mit einer verwalteten Instanz](sql-database-managed-instance-connect-app.md).

Wenn Sie eine neue Azure SQL Server-Instanz im [Azure-Portal](sql-database-single-database-get-started.md) erstellen, ist das Ergebnis ein öffentlicher Endpunkt im Format *IhrServername.database.windows.net*.

Sie können die folgenden Netzwerkzugriffssteuerungen verwenden, um den Zugriff auf die SQL-Datenbank selektiv über den öffentlichen Endpunkt zuzulassen:
- Azure-Dienste zulassen: Bei einer Festlegung auf „Ein“ können andere Ressourcen innerhalb der Azure-Grenze (z. B. ein virtueller Azure-Computer) auf die SQL-Datenbank zugreifen.

- IP-Firewallregeln: Verwenden Sie dieses Feature, um Verbindungen von einer bestimmten IP-Adresse explizit zuzulassen (z. B. von lokalen Computern).

Sie können den privaten Zugriff auf die SQL-Datenbank auch aus [virtuellen Netzwerken](../virtual-network/virtual-networks-overview.md) wie folgt zulassen:
- Virtual Network-Firewallregeln: Verwenden Sie dieses Feature, um Datenverkehr von einem bestimmten virtuellen Netzwerk innerhalb der Azure-Grenze zuzulassen.

- Private Link: Verwenden Sie dieses Feature zum Erstellen eines privaten Endpunkts für Azure SQL Server innerhalb eines bestimmten Virtual Network.



Im folgenden Video finden Sie eine Übersicht über diese Zugriffssteuerungen und ihre Aufgaben:
> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Data-Exposed--SQL-Database-Connectivity-Explained/player?WT.mc_id=dataexposed-c9-niner]


## <a name="allow-azure-services"></a>Azure-Dienste zulassen 
Diese Einstellung wird während der Erstellung einer neuen Azure SQL Server-Instanz im [Azure-Portal](sql-database-single-database-get-started.md) deaktiviert.



Sie können diese Einstellung auch über den Bereich „Firewall“ wie folgt ändern, nachdem die Azure SQL Server-Instanz erstellt wurde.
  
 ![Screenshot: Verwalten der Serverfirewall][2]

Wenn die Einstellung auf **Ein** festgelegt wird, erlaubt Azure SQL Server Azure die Kommunikation von allen Ressourcen innerhalb der Azure-Grenze, egal ob sie Teil Ihres Abonnements sind oder nicht.

In vielen Fällen lässt die Einstellung **Ein** mehr zu, als die meisten Kunden wünschen. Möglicherweise sollten sie diese Einstellung auf **Aus** festlegen und durch restriktivere IP-Firewallregeln oder Virtual Network-Firewallregeln ersetzen. Dies wirkt sich auf die folgenden Features auf virtuellen Computern in Azure aus, die nicht Teil Ihres VNET sind und somit über eine Azure-IP-Adresse eine Verbindung mit SQL-Datenbank herstellen.

### <a name="import-export-service"></a>Import/Export-Dienst
Der Import/Export-Dienst funktioniert nicht, wenn **Azure-Diensten Zugriff auf den Server erlauben** deaktiviert ist. Sie können dieses Problem jedoch umgehen, indem Sie [„sqlpackage.exe“ manuell auf einem virtuellen Azure-Computer oder den Export mithilfe der DACFx-API direkt in Ihrem Code ausführen](https://docs.microsoft.com/azure/sql-database/import-export-from-vm).

### <a name="data-sync"></a>Datensynchronisierung
Wenn **Azure-Diensten Zugriff auf den Server erlauben** deaktiviert ist und Sie das Datensynchronisierungsfeature verwenden möchten, müssen Sie zum [Hinzufügen von IP-Adressen](sql-database-server-level-firewall-rule.md) einzelne Firewallregeleinträge auf der Grundlage des **SQL-Diensttags** für die Region erstellen, in der die **Hub-Datenbank** gehostet wird.
Fügen Sie diese Firewallregeln auf der Serverebene den logischen Servern hinzu, die sowohl die **Hub-Datenbank** als auch die **Mitgliedsdatenbanken** hosten (können sich in unterschiedlichen Regionen befinden).

Verwenden Sie das folgende PowerShell-Skript, um die entsprechenden IP-Adressen für das SQL-Diensttag für die Region „USA, Westen“ zu generieren:
```powershell
PS C:\>  $serviceTags = Get-AzNetworkServiceTag -Location eastus2
PS C:\>  $sql = $serviceTags.Values | Where-Object { $_.Name -eq "Sql.WestUS" }
PS C:\> $sql.Properties.AddressPrefixes.Count
70
PS C:\> $sql.Properties.AddressPrefixes
13.86.216.0/25
13.86.216.128/26
13.86.216.192/27
13.86.217.0/25
13.86.217.128/26
13.86.217.192/27
```

> [!TIP]
> „Get-AzNetworkServiceTag“ gibt trotz Angabe des Parameters „Location“ den globalen Bereich für das SQL-Diensttag zurück. Filtern Sie daher nach der Region, in der die von Ihrer Synchronisierungsgruppe verwendete Hub-Datenbank gehostet wird.

Beachten Sie, dass die Ausgabe des PowerShell-Skripts in CIDR-Notation (Classless Inter-Domain Routing; klassenloses domänenübergreifendes Routing) erfolgt und wie folgt unter Verwendung von [Get-IPrangeStartEnd.ps1](https://gallery.technet.microsoft.com/scriptcenter/Start-and-End-IP-addresses-bcccc3a9) in ein Format mit Start- und End-IP-Adresse konvertiert werden muss:
```powershell
PS C:\> Get-IPrangeStartEnd -ip 52.229.17.93 -cidr 26                                                                   
start        end
-----        ---
52.229.17.64 52.229.17.127
```

Führen Sie die folgenden zusätzlichen Schritte aus, um alle IP-Adressen von CIDR in das Format mit Start- und End-IP-Adresse zu konvertieren:

```powershell
PS C:\>foreach( $i in $sql.Properties.AddressPrefixes) {$ip,$cidr= $i.split('/') ; Get-IPrangeStartEnd -ip $ip -cidr $cidr;}                                                                                                                
start          end
-----          ---
13.86.216.0    13.86.216.127
13.86.216.128  13.86.216.191
13.86.216.192  13.86.216.223
```
Diese können nun als individuelle Firewallregeln hinzugefügt werden. Danach können Sie **Azure-Diensten Zugriff auf den Server erlauben** deaktivieren.


## <a name="ip-firewall-rules"></a>IP-Firewallregeln
Die IP-basierte Firewall ist ein Feature von Azure SQL Server, das den gesamten Zugriff auf den Datenbankserver verhindert, bis Sie explizit [IP-Adressen der Clientcomputer hinzufügen](sql-database-server-level-firewall-rule.md).


## <a name="virtual-network-firewall-rules"></a>Virtual Network-Firewallregeln

Zusätzlich zu den IP-Regeln können Sie mit der Azure SQL Server-Firewall *Regeln für virtuelle Netzwerke* definieren.  
Weitere Informationen finden Sie unter [Virtual Network service endpoints and rules for Azure SQL Database](sql-database-vnet-service-endpoint-rule-overview.md) (VNET-Dienstendpunkte und -Regeln für Azure SQL-Datenbank) oder in diesem Video:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Data-Exposed--Demo--Vnet-Firewall-Rules-for-SQL-Database/player?WT.mc_id=dataexposed-c9-niner]

 ### <a name="azure-networking-terminology"></a>Azure-Netzwerkterminologie  
Beachten Sie die folgenden Azure-Netzwerkbegriffe beim Erkunden von Virtual Network-Firewallregeln.

**Virtuelles Netzwerk:** Sie können Ihrem Azure-Abonnement virtuelle Netzwerke zuordnen. 

**Subnetz:** Ein virtuelles Netzwerk enthält **Subnetze**. Ihre virtuellen Azure-Computer (VMs) sind Subnetzen zugewiesen. Ein Subnetz kann mehrere VMs oder andere Computeknoten enthalten. Computeknoten, die sich außerhalb Ihres virtuellen Netzwerks befinden, können nicht auf Ihr virtuelles Netzwerk zugreifen, es sei denn, Sie konfigurieren für sie den sicheren Zugriff.

**Virtual Network-Dienstendpunkt:** Ein [VNET-Dienstendpunkt](../virtual-network/virtual-network-service-endpoints-overview.md) ist ein Subnetz, dessen Eigenschaftswerte mindestens einen formalen Azure-Diensttypnamen enthalten. In diesem Artikel beschäftigen wir uns mit dem Typnamen **Microsoft.Sql**, der auf einen Azure-Dienst mit dem Namen „SQL-Datenbank“ verweist.

**VNET-Regel:** Eine VNET-Regel für Ihren SQL-Datenbank-Server ist ein Subnetz, das in der Zugriffssteuerungsliste des SQL-Datenbank-Servers aufgeführt ist. Um in die Zugriffssteuerungsliste für Ihre SQL-Datenbank-Instanz zu gelangen, muss das Subnetz den Typnamen **Microsoft.Sql** enthalten. Eine VNET-Regel weist Ihren Azure SQL-Datenbank-Server an, Nachrichten von jedem Knoten anzunehmen, der zum Subnetz gehört.


## <a name="ip-vs-virtual-network-firewall-rules"></a>IP-Firewallregeln im Vergleich zu Virtual Network-Firewallregeln

Die Firewall von Azure SQL Server ermöglicht Ihnen das Angeben von IP-Adressbereichen, aus denen Nachrichten an die SQL-Datenbank-Instanz gesendet werden dürfen. Dieser Ansatz eignet sich gut für statische IP-Adressen, die sich außerhalb des privaten Azure-Netzwerks befinden. Doch viele virtuelle Computer (VMs) innerhalb des privaten Azure-Netzwerks sind mit *dynamischen* IP-Adressen konfiguriert. Dynamische IP-Adressen können sich ändern, wenn der virtuelle Computer neu gestartet wird, und damit die IP-basierte Firewallregel ungültig machen. Es wäre töricht, eine dynamische IP-Adresse in einer Firewallregel in einer Produktionsumgebung anzugeben.

Sie können diese Einschränkung umgehen, indem Sie eine *statische* IP-Adresse für Ihre VM abrufen. Einzelheiten finden Sie unter [Konfigurieren von privaten IP-Adressen für einen virtuellen Computer über das Azure-Portal](../virtual-network/virtual-networks-static-private-ip-arm-pportal.md). Der Ansatz mit statischen IP-Adressen kann jedoch schwierig zu handhaben und aufwendig sein, wenn er in großem Maßstab befolgt wird. 

Regeln für virtuelle Netzwerke stellen eine einfachere Alternative zum Einrichten und Verwalten des Zugriffs von einem bestimmten Subnetz mit Ihren virtuellen Computern dar.

> [!NOTE]
> Azure SQL-Datenbank wird noch nicht in einem Subnetz unterstützt. Wenn sich Ihr Azure SQL-Datenbank-Server auf einem Knoten in einem Subnetz in Ihrem virtuellen Netzwerk befände, könnten alle Knoten innerhalb des virtuellen Netzwerks mit Ihrer SQL-Datenbank-Instanz kommunizieren. In diesem Fall könnten Ihre virtuellen Computer mit der SQL-Datenbank-Instanz kommunizieren, ohne dass VNET-Regeln oder IP-Regeln erforderlich sind.

## <a name="private-link"></a>Private Link 
Private Link ermöglicht das Herstellen von Verbindungen mit Azure SQL Server über einen **privaten Endpunkt**. Ein privater Endpunkt ist eine private IP-Adresse in einem bestimmten [virtuellen Netzwerk](../virtual-network/virtual-networks-overview.md) und Subnetz.

## <a name="next-steps"></a>Nächste Schritte

- Einen Schnellstart zum Erstellen einer IP-Firewallregel auf Serverebene finden Sie unter [Erstellen einer Azure SQL-Datenbank-Instanz](sql-database-single-database-get-started.md).

- Einen Schnellstart zum Erstellen einer VNET-Firewallregel auf Serverebene finden Sie unter [VNET-Dienstendpunkte und Regeln für Azure SQL-Datenbank](sql-database-vnet-service-endpoint-rule-overview.md).

- Hilfe beim Herstellen einer Verbindung mit einer Azure SQL-Datenbank-Instanz über Open-Source-Anwendungen oder Anwendungen von Drittanbietern finden Sie unter [Clientschnellstarts für SQL-Datenbank](https://msdn.microsoft.com/library/azure/ee336282.aspx).

- Informationen zu zusätzlichen Ports, die Sie ggf. öffnen müssen, finden Sie im Abschnitt **SQL-Datenbank: ,Außerhalb‘ im Vergleich zu ,Innerhalb‘** im Artikel [Andere Ports als 1433 für ADO.NET 4.5 und SQL-Datenbank](sql-database-develop-direct-route-ports-adonet-v12.md).

- Eine Übersicht über die Azure SQL-Datenbank-Konnektivität finden Sie unter [Verbindungsarchitektur von Azure SQL-Datenbank](sql-database-connectivity-architecture.md).

- Eine Übersicht über die Sicherheit von Azure SQL-Datenbank finden Sie unter [Sichern der SQL-Datenbank](sql-database-security-overview.md).

<!--Image references-->
[1]: ./media/sql-database-get-started-portal/new-server2.png
[2]: ./media/sql-database-get-started-portal/manage-server-firewall.png

