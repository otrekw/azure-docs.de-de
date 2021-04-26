---
title: Übersicht über Netzwerkkonzepte – Azure Database for MySQL Flexible Server
description: In diesem Artikel erfahren Sie mehr über die Konnektivitäts- und Netzwerkoptionen in der Bereitstellungsoption „Flexible Server“ für Azure Database for MySQL.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: conceptual
ms.date: 9/23/2020
ms.openlocfilehash: c83f36216e7488df94c372234d0541a4ee9f99b5
ms.sourcegitcommit: bfa7d6ac93afe5f039d68c0ac389f06257223b42
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106492221"
---
# <a name="connectivity-and-networking-concepts-for-azure-database-for-mysql---flexible-server-preview"></a>Konnektivitäts- und Netzwerkkonzepte für Azure Database for MySQL – Flexible Server (Vorschau)

In diesem Artikel werden die öffentlichen und privaten Konnektivitätsoptionen für Ihren Server beschrieben. Außerdem werden die Netzwerkkonzepte für Azure Database for MySQL Flexible Server ausführlich erläutert, die für die sichere Erstellung eines Servers in Azure erforderlich sind.

> [!IMPORTANT]
> Azure Database for MySQL – Flexible Server befindet sich in der Vorschau.

## <a name="choosing-a-networking-option"></a>Auswählen einer Netzwerkoption
Für Ihre Azure Database for MySQL Flexible Server-Bereitstellung haben Sie zwei Netzwerkoptionen: den **privaten Zugriff** (VNET-Integration) und den **öffentlichen Zugriff (zugelassene IP-Adressen)** . Bei der Servererstellung müssen Sie eine dieser Optionen auswählen. 

> [!NOTE]
> Nach der Servererstellung kann diese Netzwerkoption nicht mehr geändert werden. 

* **Privater Zugriff (VNET-Integration):** Sie können Ihren flexiblen Server in Ihrer [Azure Virtual Network](../../virtual-network/virtual-networks-overview.md)-Instanz bereitstellen. Virtuelle Azure-Netzwerke ermöglichen eine private und sichere Netzwerkkommunikation. Ressourcen in einem virtuellen Netzwerk können über private IP-Adressen kommunizieren.

   Die VNET-Integration bietet sich an, wenn Sie die folgenden Funktionen benötigen:
   * Verbinden von Azure-Ressourcen im gleichen virtuellen Netzwerk oder [Peered virtuellen Netzwerk](../../virtual-network/virtual-network-peering-overview.md) mit Ihrem flexiblen Server
   * Herstellen einer Verbindung von Azure-externen Ressourcen mit Ihrem flexiblen Server über ein VPN oder eine ExpressRoute-Verbindung
   * Kein öffentlicher Endpunkt

* **Öffentlicher Zugriff (zugelassene IP-Adressen):** Auf Ihren flexiblen Server wird über einen öffentlichen Endpunkt zugegriffen. Der öffentliche Endpunkt ist eine öffentlich auflösbare DNS-Adresse. Der Ausdruck „zugelassene IP-Adressen“ bezieht sich auf einen Bereich von IP-Adressen, denen Sie die Berechtigung erteilen, auf den Server zuzugreifen. Diese Berechtigungen heißen **Firewallregeln**. 

   Sie sollten sich für einen öffentlichen Zugriff entscheiden, wenn Sie die folgenden Funktionen benötigen:
   * Herstellen einer Verbindung von Azure-Ressourcen aus, die keine virtuellen Netzwerke unterstützen
   * Herstellen einer Verbindung von Azure-externen Ressourcen aus, die weder ein VPN noch ExpressRoute unterstützen 
   * Der flexible Server hat keinen öffentlichen Endpunkt.

Die folgenden Eigenschaften gelten unabhängig davon, ob Sie den privaten oder den öffentlichen Zugriff wählen:
* Verbindungen von zugelassenen IP-Adressen aus müssen sich beim MySQL-Server mit gültigen Anmeldeinformationen authentifizieren.
* Für den Netzwerkdatenverkehr ist die [Verbindungsverschlüsselung](#tls-and-ssl) verfügbar.
* Der Server verfügt über einen vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN). Für die Hostnamenseigenschaft in Verbindungszeichenfolgen wird empfohlen, den FQDN anstelle einer IP-Adresse zu verwenden.
* Über beide Optionen wird der Zugriff auf Serverebene, nicht auf Datenbank- oder auf Tabellenebene gesteuert. Der Zugriff auf Datenbanken, Tabellen und andere Objekte wird mithilfe der Rolleneigenschaften in MySQL gesteuert.


## <a name="private-access-vnet-integration"></a>Privater Zugriff (VNET-Integration)
Der private Zugriff mit der Integration über ein virtuelles Netzwerk (VNET) gewährleistet eine private und sichere Kommunikation für Ihre MySQL Flexible Server-Instanz.

### <a name="virtual-network-concepts"></a>Virtuelle Netzwerke: Konzepte
Nachstehend werden einige Konzepte erläutert, die Sie kennen sollten, wenn Sie virtuelle Netzwerke mit MySQL Flexible Server-Instanzen verwenden.

* **Virtuelles Netzwerk:** Ein virtuelles Azure-Netzwerk (Virtual Network, VNET) enthält einen privaten IP-Adressraum, der für Ihre Verwendung konfiguriert ist. Weitere Informationen zu virtuellen Azure-Netzwerken finden Sie unter [Überblick über Azure Virtual Network](../../virtual-network/virtual-networks-overview.md).

    Ihr virtuelles Netzwerk muss in derselben Azure-Region wie Ihr flexibler Server bereitgestellt werden.

* **Delegiertes Subnetz:** Ein virtuelles Netzwerk enthält Subnetze. Subnetze bieten Ihnen die Möglichkeit, Ihr virtuelles Netzwerk in kleinere Adressräume einzuteilen. Azure-Ressourcen werden in bestimmten Subnetzen innerhalb eines virtuellen Netzwerks bereitgestellt. 

   Ihre MySQL Flexible Server-Instanz muss sich in einem Subnetz befinden, das eigens für MySQL Flexible Server-Instanzen eingeteilt (**delegiert**) wurde. Diese Delegierung bedeutet, dass dieses Subnetz nur von Azure Database for MySQL Flexible Server-Instanzen genutzt werden kann. Im delegierten Subnetz können sich keine anderen Azure-Ressourcentypen befinden. Sie können ein Subnetz delegieren, indem Sie „Microsoft.DBforMySQL/flexibleServers“ als Delegierungseigenschaft festlegen.

* **Netzwerksicherheitsgruppen (NSG):** Mit Sicherheitsregeln in Netzwerksicherheitsgruppen können Sie den Typ des ein- und ausgehenden Netzwerkdatenverkehrs von Subnetzen virtueller Netzwerke und Netzwerkschnittstellen filtern. Weitere Informationen finden Sie unter [Netzwerksicherheitsgruppen](../../virtual-network/network-security-groups-overview.md).

* **Peering virtueller Netzwerke:** Durch das Peering virtueller Netzwerke können mehrere virtuelle Netzwerke in Azure nahtlos verbunden werden. Die peered virtuellen Netzwerke werden für Verbindungszwecke als einzelnes Element angezeigt. Der Datenverkehr zwischen virtuellen Computern in virtuellen Netzwerken mit Peering erfolgt über die Microsoft-Backboneinfrastruktur. Der Datenverkehr zwischen der Clientanwendung und dem flexiblem Server mittels peered VNets wird nur über das private Netzwerk von Microsoft weitergeleitet und ist nur in diesem Netzwerk isoliert.

Flexibler Server unterstützt das Peering virtueller Netzwerke in derselben Azure-Region. VNET-Peering wird **nicht unterstützt**. Weitere Informationen finden Sie in den [Konzepten zum Peering virtueller Netzwerke](../../virtual-network/virtual-network-peering-overview.md).

### <a name="connecting-from-peered-vnets-in-same-azure-region"></a>Herstellen einer Verbindung von peered VNets in derselben Azure-Region
Wenn sich die Clientanwendung, die versucht, eine Verbindung mit flexiblem Server herzustellen, im Peering fähigen virtuellen Netzwerk befindet, ist sie möglicherweise nicht in der Lage, eine Verbindung mit dem flexiblen Server-Servernamen herzustellen, da sie den DNS-Namen für den flexiblen Server nicht aus dem peered VNet auflösen kann. Es gibt zwei Optionen, um dieses Problem zu beheben:
* Private IP-Adresse verwenden (empfohlen für DEV/Test-Szenario): diese Option kann für Entwicklungs- oder Testzwecke verwendet werden. Mit „nslookup“ können Sie die private IP-Adresse für Ihren flexiblen Servernamen (voll qualifizierter Domänenname) umkehren und eine private IP-Adresse verwenden, um eine Verbindung mit der Clientanwendung herzustellen. Die Verwendung der privaten IP-Adresse für die Verbindung mit flexiblem Server wird für die Verwendung in der Produktion nicht empfohlen, da Sie sich während geplanter oder ungeplanter Ereignisse ändern.
* Verwenden Sie die private DNS-Zone (empfohlen für die Produktion): diese Option ist für Produktionszwecke geeignet. Sie stellen eine [private DNS-Zone](../../dns/private-dns-getstarted-portal.md) bereit und verknüpfen Sie sie mit Ihrem virtuellen Clientnetzwerk. In der privaten DNS-Zone fügen Sie einen [A-Datensatz](../../dns/dns-zones-records.md#record-types) für Ihren flexiblen Server hinzu, indem Sie die zugehörige private IP-Adresse verwenden. Anschließend können Sie den A-Datensatz verwenden, um eine Verbindung zwischen der Clientanwendung in einem mit einem peered virtuellen Netzwerk und einem flexiblen Server herzustellen.

### <a name="connecting-from-on-premises-to-flexible-server-in-virtual-network-using-expressroute-or-vpn"></a>Herstellen einer Verbindung zwischen einem lokalen Standort und einem flexiblen Server in Virtual Network mithilfe von ExpressRoute oder VPN
Für Workloads, die Zugriff auf einen flexiblen Server in einem virtuellen Netzwerk über das lokale Netzwerk benötigen, benötigen Sie [ExpressRoute](/azure/architecture/reference-architectures/hybrid-networking/expressroute/) oder [VPN](/azure/architecture/reference-architectures/hybrid-networking/vpn/) und ein virtuelles Netzwerk[, das mit dem lokalen Standort ](/azure/architecture/reference-architectures/hybrid-networking/) verbunden ist. Wenn Sie diese Einrichtung durchgeführt haben, benötigen Sie eine DNS-Weiterleitung, um den flexiblen Servernamen aufzulösen, wenn Sie eine Verbindung von einer Clientanwendung (z. B. MySQL Workbench) herstellen möchten, die im lokalen virtuellen Netzwerk ausgeführt wird. Diese DNS-Weiterleitung ist dafür zuständig, alle DNS-Abfragen über eine Weiterleitung auf Serverebene zum von Azure bereitgestellten DNS-Dienst [168.63.129.16](../../virtual-network/what-is-ip-address-168-63-129-16.md) aufzulösen.

Für eine korrekte Konfiguration benötigen Sie die folgenden Ressourcen:

- Lokales Netzwerk
- Flexibler MySQL-Server mit privatem Zugriff (VNet-Integration)
- Virtuelles Netzwerk [mit Verbindung mit lokalem Standort](/azure/architecture/reference-architectures/hybrid-networking/)
- Nutzen Sie die in Azure bereitgestellte DNS-Weiterleitung [168.63.129.16](../../virtual-network/what-is-ip-address-168-63-129-16.md)

Anschließend können Sie den flexiblen Servernamen (FQDN) verwenden, um eine Verbindung zwischen der Clientanwendung in einem mit einem peered virtuellen Netzwerk oder einem lokalen Netzwerk und einem flexiblen Server herzustellen.

### <a name="unsupported-virtual-network-scenarios"></a>Nicht unterstützte virtuelle Netzwerkszenarios
* Öffentlicher Endpunkt (oder öffentliche IP-Adresse oder DNS): Ein flexibler Server, der in einem virtuellen Netzwerk bereitgestellt wird, kann keinen öffentlichen Endpunkt haben.
* Nachdem der flexible Server in einem virtuellen Netzwerk und Subnetz bereitgestellt wurde, können Sie ihn in kein anderes virtuelles Netzwerk oder Subnetz verschieben. Auch das virtuelle Netzwerk kann in keine andere Ressourcengruppe oder kein anderes Abonnement verschoben werden.
* Die Subnetzgröße (Adressräume) kann nicht erhöht werden, sobald Ressourcen im Subnetz vorhanden sind.
* VNET-Peering wird nicht unterstützt.

Unter den folgenden Links erfahren Sie, wie Sie den privaten Zugriff (VNET-Integration) über das [Azure-Portal](how-to-manage-virtual-network-portal.md) oder die [Azure CLI](how-to-manage-virtual-network-cli.md) aktivieren.

> [!NOTE]
> Wenn Sie den benutzerdefinierten DNS-Server verwenden, müssen Sie eine DNS-Weiterleitung verwenden, um den FQDN von Azure Database for MySQL: Flexible Server aufzulösen. Weitere Informationen finden Sie unter [Namensauflösung mithilfe eines eigenen DNS-Servers](../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).

## <a name="public-access-allowed-ip-addresses"></a>Öffentlicher Zugriff (zugelassene IP-Adressen)
Die öffentliche Zugriffsmethode weist u. a. folgende Eigenschaften auf:
* Nur die IP-Adressen, die Sie zulassen, dürfen auf Ihre MySQL Flexible Server-Instanz zugreifen. Standardmäßig sind keine IP-Adressen zugelassen. Sie können IP-Adressen während der Servererstellung oder später hinzufügen.
* Ihr MySQL-Server verfügt über einen öffentlich auflösbaren DNS-Namen.
* Ihr flexibler Server befindet sich nicht in einem Ihrer virtuellen Azure-Netzwerke.
* Der Netzwerkdatenverkehr zu und von Ihrem Server erfolgt nicht über ein privates Netzwerk. Der Datenverkehr verwendet die allgemeinen Internetpfade.

### <a name="firewall-rules"></a>Firewallregeln
Eine Berechtigung, die einer IP-Adresse erteilt wird, wird als Firewallregel bezeichnet. Wenn ein Verbindungsversuch von einer IP-Adresse stammt, die Sie nicht zugelassen haben, wird dem Ursprungsclient ein Fehler zurückgegeben.


### <a name="allowing-all-azure-ip-addresses"></a>Zulassen aller Azure-IP-Adressen
Wenn eine feste IP-Adresse für ausgehenden Datenverkehr für Ihren Azure-Dienst nicht verfügbar ist, können Sie die Aktivierung von Verbindungen von allen IP-Adressen im Azure-Rechenzentrum in Erwägung ziehen.

> [!IMPORTANT]
> Die Option **Allow public access from Azure services and resources within Azure** (Öffentlichen Zugriff von Azure-Diensten und -Ressourcen aus gestatten) konfiguriert die Firewall so, dass alle von Azure ausgehenden Verbindungen zugelassen werden, einschließlich der Verbindungen aus Abonnements anderer Kunden. Wenn Sie diese Option auswählen, stellen Sie sicher, dass die Anmelde- und die Benutzerberechtigungen den Zugriff nur auf autorisierte Benutzer beschränken.

Unter den folgenden Links erfahren Sie, wie Sie den öffentlichen Zugriff (zugelassene IP-Adressen) über das [Azure-Portal](how-to-manage-firewall-portal.md) oder die [Azure CLI](how-to-manage-firewall-cli.md) aktivieren und verwalten.


### <a name="troubleshooting-public-access-issues"></a>Behandeln von Problemen mit dem öffentlichen Zugriff
Wenn der Zugriff auf den Serverdienst „Microsoft Azure Database for MySQL“ nicht das erwartete Verhalten aufweist, sind folgende Punkte zu beachten:

* **Änderungen an der Zulassungsliste sind noch nicht wirksam:** Änderungen an der Firewallkonfiguration für Azure-Datenbank für MySQL-Server werden möglicherweise erst nach fünf Minuten wirksam.

* **Fehler bei der Authentifizierung:** Wenn ein Benutzer nicht über die Berechtigungen für den Azure Database for MySQL-Server verfügt oder das verwendete Kennwort falsch ist, wird die Verbindung mit dem Azure Database for MySQL-Server verweigert. Durch das Erstellen einer Firewalleinstellung wird Clients lediglich die Möglichkeit gegeben, einen Verbindungsversuch zum Server zu unternehmen. Jeder Client muss weiterhin die erforderlichen Sicherheitsanmeldeinformationen bereitstellen.

* **Dynamische Client-IP-Adresse:** Wenn Sie über eine Internetverbindung mit dynamischer IP-Adressierung verfügen und Probleme beim Passieren der Firewall auftreten, können Sie eine der folgenden Lösungen ausprobieren:

   * Fragen Sie Ihren Internetdienstanbieter nach dem IP-Adressbereich, der Ihren Clientcomputern zugewiesen ist, die auf den Azure Database for MySQL-Server zugreifen, und fügen Sie dann den IP-Adressbereich als Firewallregel hinzu.
   * Verwenden Sie stattdessen die statische IP-Adressierung für die Clientcomputer, und fügen Sie dann die statische IP-Adresse als Firewallregel hinzu.
  
* **Die Firewallregel ist für das IPv6-Format nicht verfügbar:** Firewallregeln müssen das IPv4-Format aufweisen. Wenn Sie Firewallregeln im IPv6-Format angeben, wird ein Validierungsfehler angezeigt.


## <a name="hostname"></a>Hostname
Unabhängig von Ihrer gewählten Netzwerkoption empfiehlt es sich, beim Herstellen einer Verbindung mit Ihrem flexiblen Server immer einen FQDN als Hostnamen zu verwenden. Es ist nicht gewährleistet, dass die IP-Adresse des Servers statisch bleibt. Mithilfe des FQDN können Sie verhindern, dass Änderungen an der Verbindungszeichenfolge vorgenommen werden. 

Beispiel
* Empfohlen: `hostname = servername.mysql.database.azure.com`
* Vermeiden Sie nach Möglichkeit die Verwendung von `hostname = 10.0.0.4` (eine private Adresse) oder `hostname = 40.2.45.67` (eine öffentliche IP-Adresse).


## <a name="tls-and-ssl"></a>TLS und SSL
Azure Database for MySQL Flexible Server unterstützt das Herstellen einer Verbindung zwischen Ihren Clientanwendungen und dem MySQL-Server über Secure Sockets Layer (SSL) mit TLS-Verschlüsselung (Transport Layer Security). TLS ist ein Standardprotokoll der Branche, das verschlüsselte Netzwerkverbindungen zwischen dem Datenbankserver und Clientanwendungen gewährleistet, sodass Sie Konformitätsanforderungen einhalten können.

Azure Database for MySQL Flexible Server unterstützt standardmäßig verschlüsselte Verbindungen mit Transport Layer Security (TLS 1.2), und alle eingehenden Verbindungen mit TLS 1.0 und TLS 1.1 werden standardmäßig verweigert. Die Erzwingung einer verschlüsselten Verbindung oder die TLS-Versionskonfiguration bei Ihrem flexiblen Server kann konfiguriert und geändert werden. 

Im Folgenden sind die verschiedenen Konfigurationen von SSL- und TLS-Einstellungen aufgeführt, die Sie bei Ihrem flexiblen Server verwenden können:

| Szenario   | Einstellungen für Serverparameter      | BESCHREIBUNG                                    |
|------------|--------------------------------|------------------------------------------------|
|SSL deaktivieren (verschlüsselte Verbindungen) | require_secure_transport = OFF |Wenn Ihre Legacy-Anwendung keine verschlüsselten Verbindungen mit dem MySQL-Server unterstützt, können Sie die Erzwingung von verschlüsselten Verbindungen mit Ihrem flexiblen Server deaktivieren, indem Sie „require_secure_transport=OFF“ festlegen.|
|Erzwingen von SSL bei der TLS-Version < 1.2 | require_secure_transport = ON und tls_version = TLSV1 oder TLSV1.1| Wenn Ihre Legacy-Anwendung verschlüsselte Verbindungen unterstützt, aber TLS-Version < 1.2 erfordert, können Sie verschlüsselte Verbindungen aktivieren, müssen aber Ihren flexiblen Server so konfigurieren, dass Verbindungen mit der von Ihrer Anwendung unterstützten TLS-Version (v1.0 oder v1.1) zulässig sind.|
|SSL mit TLS-Version = 1.2 erzwingen (Standardkonfiguration)|require_secure_transport = ON und tls_version = TLSV1.2| Dies ist die empfohlene Standardkonfiguration für einen flexiblen Server.|
|SSL mit TLS-Version = 1.3 erzwingen (unterstützt bei MySQL v8.0 und höher)| require_secure_transport = ON und tls_version = TLSV1.3| Dies ist nützlich und wird für die Entwicklung neuer Anwendungen empfohlen.|

> [!Note]
> Änderungen am SSL-Verschlüsselungsverfahren bei einem flexiblen Server werden nicht unterstützt. FIPS-Verschlüsselungssammlungen werden standardmäßig erzwungen, wenn „tls_version“ auf „TLS-Version 1.2“ festgelegt wird. Bei anderen TLS-Versionen als Version 1.2 wird das SSL-Verschlüsselungsverfahren auf die Standardeinstellungen festgelegt, die in der MySQL-Communityinstallation enthalten sind.

Weitere Informationen finden Sie unter [Herstellen einer Verbindung mithilfe von SSL/TLS](how-to-connect-tls-ssl.md). 


## <a name="next-steps"></a>Nächste Schritte
* Unter den folgenden Links erfahren Sie, wie Sie den privaten Zugriff (VNET-Integration) über das [Azure-Portal](how-to-manage-virtual-network-portal.md) oder die [Azure CLI](how-to-manage-virtual-network-cli.md) aktivieren.
* Unter den folgenden Links erfahren Sie, wie Sie den öffentlichen Zugriff (zugelassene IP-Adressen) über das [Azure-Portal](how-to-manage-firewall-portal.md) oder die [Azure CLI](how-to-manage-firewall-cli.md) aktivieren.
* Weitere Informationen zur [Verwendung der TLS](how-to-connect-tls-ssl.md)
