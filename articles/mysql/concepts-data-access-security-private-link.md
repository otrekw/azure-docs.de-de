---
title: Private Link für Azure Database for MySQL (Vorschau)
description: Erfahren Sie, wie Private Link für Azure Database for MySQL funktioniert.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 01/09/2020
ms.openlocfilehash: d9738a1dca39e1b43f690bd65ff05d20b6a94fa1
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2020
ms.locfileid: "75898926"
---
# <a name="private-link-for-azure-database-for-mysql-preview"></a>Private Link für Azure Database for MySQL (Vorschau)

Private Link ermöglicht die Verbindungsherstellung mit verschiedenen PaaS-Diensten in Azure über einen privaten Endpunkt. Im Prinzip macht Azure Private Link Azure-Dienste in Ihrem privaten virtuellen Netzwerk (VNET) verfügbar. Der Zugriff auf die PaaS-Ressourcen erfolgt wie bei anderen Ressourcen im VNET über die private IP-Adresse.

Eine Liste der PaaS-Dienste, die die Private Link-Funktion unterstützen, finden Sie in der [Dokumentation zu Private Link](https://docs.microsoft.com/azure/private-link/index). Ein privater Endpunkt ist eine private IP-Adresse in einem bestimmten [VNET](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) und Subnetz.

> [!NOTE]
> Dieses Feature steht in allen Azure-Regionen zur Verfügung, in denen Azure Database for MySQL die Tarife „Universell“ und „Arbeitsspeicheroptimiert“ unterstützt.

## <a name="data-exfiltration-prevention"></a>Verhinderung der Datenexfiltration

Datenexfiltration in Azure Database for MySQL bedeutet, dass ein autorisierter Benutzer (etwa ein Datenbankadministrator) Daten aus einem System extrahieren und an einen anderen Speicherort oder in ein anderes System außerhalb der Organisation verschieben kann. So kann ein Benutzer beispielsweise Daten in ein Speicherkonto eines Dritten verschieben.

Stellen Sie sich ein Szenario vor, in dem ein Benutzer MySQL Workbench auf einem virtuellen Azure-Computer (Virtual Machine, VM) ausführt, der eine Verbindung mit einem in der Region „USA, Westen“ bereitgestellten Azure Database for MySQL-Server herstellt. Das folgende Beispiel zeigt, wie Sie den Zugriff mit öffentlichen Endpunkten in Azure Database for MySQL unter Verwendung von Netzwerkzugriffssteuerungen einschränken.

* Deaktivieren Sie den gesamten Datenverkehr von Azure-Diensten zu Azure Database for MySQL über den öffentlichen Endpunkt, indem Sie die Option zum *Zulassen von Azure-Diensten* auf „AUS“ festlegen. Stellen Sie entweder mithilfe von [Firewallregeln](https://docs.microsoft.com/azure/mysql/concepts-firewall-rules) oder mithilfe von [VNET-Dienstendpunkten](https://docs.microsoft.com/azure/mysql/concepts-data-access-and-security-vnet) sicher, dass keine IP-Adressen oder IP-Adressbereiche auf den Server zugreifen können.

* Lassen Sie nur Datenverkehr an Azure Database for MySQL mit der privaten IP-Adresse des virtuellen Computers zu. Weitere Informationen finden Sie in den Artikeln [Verwenden von VNET-Dienstendpunkten und -Regeln für Datenbankserver](concepts-data-access-and-security-vnet.md) und [IP-Firewallregeln für Azure SQL-Datenbank und Azure SQL Data Warehouse](howto-manage-vnet-using-portal.md).

* Beschränken Sie auf dem virtuellen Azure-Computer den Bereich der ausgehenden Verbindung mithilfe von Netzwerksicherheitsgruppen (NSGs) und Diensttags:

    * Geben Sie eine NSG-Regel an, um Datenverkehr für das *Diensttag „SQL.WestUs“* zuzulassen. Dadurch ist nur eine Verbindung mit Azure Database for MySQL in der Region „USA, Westen“ möglich.
    * Geben Sie eine NSG-Regel (mit einer höheren Priorität) an, um Datenverkehr für das *Diensttag „SQL“* zu verweigern. Dadurch werden Verbindungen mit dem Update von Azure Database for MySQL in allen Regionen verweigert.</br></br>

Am Ende dieser Einrichtung kann der virtuelle Azure-Computer nur eine Verbindung mit Azure Database for MySQL in der Region „USA, Westen“ herstellen. Die Konnektivität ist jedoch nicht auf eine einzelne Azure Database for MySQL-Instanz beschränkt. Der virtuelle Computer kann weiterhin eine Verbindung mit beliebigen Azure Database for MySQL-Instanzen in der Region „USA, Westen“ herstellen – also auch mit den Datenbanken, die nicht Teil des Abonnements sind. Wir haben den Bereich der Datenexfiltration im obigen Szenario auf eine bestimmte Region reduziert, sie aber nicht vollständig unterbunden.</br>

Mit Private Link können Sie nun Netzwerkzugriffssteuerungen wie NSGs einrichten, um den Zugriff auf den privaten Endpunkt einzuschränken. Einzelne Azure-PaaS-Ressourcen werden dann bestimmten privaten Endpunkten zugeordnet. Ein böswilliger Insider kann somit nur auf die zugeordnete PaaS-Ressource (etwa eine Azure Database for MySQL-Instanz), aber nicht auf andere Ressourcen zugreifen.

## <a name="on-premises-connectivity-over-private-peering"></a>Lokale Konnektivität über privates Peering

Wenn Sie von lokalen Computern aus eine Verbindung mit dem öffentlichen Endpunkt herstellen, muss Ihre IP-Adresse mithilfe einer Firewallregel auf Serverebene der IP-basierten Firewall hinzugefügt werden. Dieses Modell eignet sich zwar gut, um den Zugriff auf einzelne Computer für Entwicklungs- oder Testworkloads zuzulassen, in einer Produktionsumgebung gestaltet sich die Verwaltung jedoch schwierig.

Mit Private Link können Sie standortübergreifenden Zugriff auf den privaten Endpunkt mittels [ExpressRoute](https://azure.microsoft.com/services/expressroute/) (ER), privatem Peering oder [VPN-Tunnel](https://docs.microsoft.com/azure/vpn-gateway/) ermöglichen. Anschließend kann der gesamte Zugriff über einen öffentlichen Endpunkt deaktiviert und auf die Verwendung der IP-basierten Firewall verzichtet werden.

## <a name="configure-private-link-for-azure-database-for-mysql"></a>Konfigurieren von Private Link für Azure Database for MySQL

### <a name="creation-process"></a>Erstellungsprozess

Für die Aktivierung von Private Link werden private Endpunkte benötigt. Entsprechende Schrittanleitungen finden Sie hier:

* [Azure portal](https://docs.microsoft.com/azure/mysql/howto-configure-privatelink-portal)
* [BEFEHLSZEILENSCHNITTSTELLE (CLI)](https://docs.microsoft.com/azure/mysql/howto-configure-privatelink-cli)

### <a name="approval-process"></a>Genehmigungsprozess
Nachdem der Netzwerkadministrator den privaten Endpunkt (PE) erstellt hat, kann der MySQL-Administrator die private Endpunktverbindung (Private Endpoint Connection, PEC) mit Azure Database for MySQL verwalten.

> [!NOTE]
> Zurzeit unterstützt Azure Database for MySQL nur die automatische Genehmigung für den privaten Endpunkt.

* Navigieren Sie im Azure-Portal zur Azure Database for MySQL-Serverressource. 
    * Wählen Sie im linken Bereich die privaten Endpunktverbindungen aus.
    * Liste mit allen privaten Endpunktverbindungen (Private Endpoint Connections, PECs)
    * Diese enthält den entsprechenden erstellten privaten Endpunkt (PE).

![Auswählen des privaten Endpunkts im Portal](media/concepts-data-access-and-security-private-link/select-private-link-portal.png)

* Wählen Sie eine einzelne PEC aus der Liste aus.

![Auswählen des privaten Endpunkts mit ausstehender Genehmigung](media/concepts-data-access-and-security-private-link/select-private-link.png)

* Der MySQL-Serveradministrator kann eine PEC genehmigen oder ablehnen und optional eine kurze Textantwort hinzufügen.

![Auswählen der Nachricht für den privaten Endpunkt](media/concepts-data-access-and-security-private-link/select-private-link-message.png)

* Nach der Genehmigung oder Ablehnung wird der entsprechende Zustand zusammen mit dem Antworttext in der Liste angezeigt.

![Auswählen des Endstatus des privaten Endpunkts](media/concepts-data-access-and-security-private-link/show-private-link-approved-connection.png)

## <a name="use-cases-of-private-link-for-azure-database-for-mysql"></a>Anwendungsfälle von Private Link für Azure Database for MySQL

Clients können eine Verbindung mit dem privaten Endpunkt über das gleiche VNET, über das mittels Peering verbundene VNET in der gleichen Region oder regionsübergreifend über eine VNET-zu-VNET-Verbindung herstellen. Darüber hinaus können Clients von der lokalen Umgebung aus eine Verbindung über ExpressRoute, privates Peering oder VPN-Tunneling herstellen. Die gängigen Anwendungsfälle sind im folgenden Diagramm vereinfacht dargestellt:

![Auswählen der Übersicht über den privaten Endpunkt](media/concepts-data-access-and-security-private-link/show-private-link-overview.png)

### <a name="connecting-from-an-azure-vm-in-peered-virtual-network-vnet"></a>Herstellen einer Verbindung über einen virtuellen Azure-Computer in einem virtuellen Netzwerk (VNET) mit Peering
Konfigurieren Sie das [VNET-Peering](https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-powershell), um über einen virtuellen Azure-Computer in einem VNET mit Peering eine Verbindung mit der Azure Database for MySQL-Instanz herzustellen.

### <a name="connecting-from-an-azure-vm-in-vnet-to-vnet-environment"></a>Herstellen einer Verbindung über einen virtuellen Azure-Computer in einer VNET-zu-VNET-Umgebung
Konfigurieren Sie eine [VNET-zu-VNET-VPN-Gatewayverbindung](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal), um über einen virtuellen Azure-Computer in einer anderen Region oder in einem anderen Abonnement eine Verbindung mit einer Azure Database for MySQL-Instanz herzustellen.

### <a name="connecting-from-an-on-premises-environment-over-vpn"></a>Herstellen einer VPN-Verbindung in einer lokalen Umgebung
Verwenden oder implementieren Sie eine der folgenden Optionen, um in einer lokalen Umgebung eine Verbindung mit der Azure Database for MySQL-Instanz herzustellen:

* [Point-to-Site-Verbindung](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)
* [Site-to-Site-VPN-Verbindung](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell)
* [ExpressRoute-Verbindung](https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-portal-resource-manager)

## <a name="private-link-combined-with-firewall-rules"></a>Private Link in Kombination mit Firewallregeln

Folgende Fälle und Ergebnisse sind bei Verwendung von Private Link in Verbindung mit Firewallregeln möglich:

* Wenn Sie keine Firewallregeln konfigurieren, kann standardmäßig kein Datenverkehr auf die Azure Database for MySQL-Instanz zugreifen.

* Wenn Sie öffentlichen Datenverkehr oder einen Dienstendpunkt konfigurieren und private Endpunkte erstellen, werden verschiedene Arten von eingehendem Datenverkehr durch den entsprechenden Typ der Firewallregel autorisiert.

* Wenn Sie keinen öffentlichen Datenverkehr oder Dienstendpunkt konfigurieren und private Endpunkte erstellen, kann auf Azure Database for MySQL nur über die privaten Endpunkte zugegriffen werden. Wenn Sie keinen öffentlichen Datenverkehr oder einen Dienstendpunkt konfigurieren, ist nach dem Ablehnen oder Löschen aller genehmigten privaten Endpunkte kein Datenverkehr mit Azure Database for MySQL möglich.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Sicherheitsfunktionen in Azure Database for MySQL finden Sie in folgenden Artikeln:

* Informationen zum Konfigurieren einer Firewall für Azure Database for MySQL finden Sie unter [Firewallunterstützung](https://docs.microsoft.com/azure/mysql/concepts-firewall-rules).

* Informationen zum Konfigurieren eines VNET-Dienstendpunkts für Azure Database for MySQL finden Sie unter [Konfigurieren des Zugriffs über virtuelle Netzwerke](https://docs.microsoft.com/azure/mysql/concepts-data-access-and-security-vnet).

* Eine Übersicht über die Konnektivität für Azure Database for MySQL finden Sie unter [Verbindungsarchitektur in Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/concepts-connectivity-architecture).
