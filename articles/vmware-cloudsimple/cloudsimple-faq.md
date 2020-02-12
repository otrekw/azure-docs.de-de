---
title: 'Häufig gestellte Fragen: Azure VMware Solutions (AVS)'
description: Häufig gestellte Fragen zu Azure VMware Solution (AVS)
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: c3808491c84f6c76a51c914aac6ee5e5ee370970
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025060"
---
# <a name="frequently-asked-questions-about-vmware-solution-by-avs"></a>Häufig gestellte Fragen zu VMware Solution by AVS

## <a name="avs-service"></a>AVS-Dienst

**Was ist Azure VMware Solutions (AVS)?**

Azure VMware Solutions (AVS) transformiert und erweitert VMware-Workloads in Minuten in private, dedizierte Clouds in Azure. AVS kümmert sich um die Bereitstellung, Verwaltung der Infrastruktur und Orchestrierung der Workloads zwischen lokalen Standorten und Azure. Da Ihre Apps sowohl lokal als auch in Azure exakt gleich ausgeführt werden, profitieren Sie von der Flexibilität und den Diensten der Cloud, ohne die Komplexität eines Neuentwurfs Ihrer App-Architektur. AVS verringert Ihre Gesamtkosten mit einem Cloudverbrauchsmodell, das bedarfsgesteuerte Bereitstellung, nutzungsbasierte Bezahlung und Kapazitätsoptimierung bereitstellt. Informationen zu Funktionen, Vorteilen und Szenarien finden Sie unter [Was ist die Azure-VMware-Lösung von AVS?](cloudsimple-vmware-solutions-overview.md).

**Worum handelt es sich bei einer privaten AVS-Cloud?**

Eine private AVS-Cloud ist eine private, dedizierte Cloud, die aus einer Hochleistungsumgebung für Compute, Speicher und Netzwerk besteht, die in der Microsoft Azure-Infrastruktur (Hardware und Speicherplatz im Rechenzentrum) an Azure-Standorten bereitgestellt wird. Eine private AVS-Cloud bietet eine native VMware-Plattform als Dienst (Platform-as-a-Service). Formuliert in VMware-Begriffen: Jede private AVS-Cloud enthält genau eine Instanz von vCenter Server. vCenter Server verwaltet mehrere ESXi-Knoten, die sich in einem oder mehreren vSphere-Clustern befinden, zusammen mit dem entsprechenden virtuellen SAN-Speicher (vSAN). Ein AVS-Dienst kann mehrere private AVS-Clouds in Ihrem Azure-Abonnement enthalten. Weitere Informationen finden Sie unter [Übersicht über private AVS-Clouds](cloudsimple-private-cloud.md).

**Wo ist der AVS-Dienst verfügbar?**

AVS ist in den Regionen „USA, Osten“ „USA, Westen“ und „Europa, Westen“ verfügbar. Weitere Regionen sind in naher Zukunft geplant.

**Wie aktiviere ich mein Abonnement für AVS?**

Sie können sich an Ihren Microsoft-Kundenbetreuer unter [azurevmwaresales@microsoft.com](mailto:azurevmwaresales@microsoft.com) wenden, um Ihr Abonnement für den AVS-Dienst zu aktivieren. Geben Sie in der E-Mail Ihre Abonnement-ID an, für die der AVS-Dienst aktiviert werden soll. 

**Wie greife ich auf das AVS-Portal zu?**

Sie können über das Azure-Portal auf das AVS-Portal zugreifen. Ausführliche Informationen finden Sie unter [Zugreifen auf das Portal von VMware Solutions (AVS) über das Azure-Portal](access-cloudsimple-portal.md).

**Wie erhöhe ich die Kapazität für eine private AVS-Cloud?**

Um die Kapazität zu erhöhen, erwerben Sie zusätzliche Knoten über das Azure-Portal, und erweitern Sie dann Ihre private AVS-Cloud über das AVS-Portal. Sie können einem vorhandenen vSphere-Cluster weitere Knoten hinzufügen oder diese einem neuen vSphere-Cluster hinzufügen. Weitere Informationen finden Sie unter [Erweitern einer privaten AVS-Cloud](expand-private-cloud.md).

**Was geschieht mit meiner privaten AVS-Cloud während einer Wartung?**

AVS sendet mehrere Tage vor einem geplanten Wartungsintervall eine Benachrichtigung. Die Wartung erfolgt in einer unterbrechungsfreien Weise, um die Verfügbarkeit Ihrer privaten AVS-Cloud sicherzustellen. Die Wartung kann von einem der folgenden Typen sein:

* **AVS-Infrastruktur:** Die AVS-Infrastruktur ist auf Hochverfügbarkeit ausgelegt. Bei dieser Art von Wartungsintervall werden redundante Komponenten nacheinander aktualisiert, um Dienstunterbrechungen zu vermeiden. Sie behalten Zugriff auf die vCenter Server-Instanz Ihrer privaten AVS-Cloud, auf alle virtuellen Computer, auf die Internetverbindung aus Ihrer privaten AVS-Cloud und auf die Verbindungen mit lokalen Standorten oder Azure.
* **AVS-Portal:** Bei dieser Art von Wartungsintervall sind einige Features im AVS-Portal möglicherweise deaktiviert oder nicht zugänglich. Die Benachrichtigung vor dem Wartungsintervall enthält Details zu Funktionseinschränkungen, die während der Wartung auftreten können.

## <a name="connectivity"></a>Konnektivität

**Welche Konnektivitätsoptionen habe ich mit dem AVS-Regionsnetzwerk?**

AVS stellt die folgenden Konnektivitätsoptionen für die Verbindung mit Ihrem AVS-Regionsnetzwerk bereit. Es können mehrere Optionen gleichzeitig verwendet werden.

* **ExpressRoute-Verbindung von Ihrem lokalen Rechenzentrum mit dem AVS-Regionsnetzwerk.** Dies ist eine sichere private Verbindung mit hoher Geschwindigkeit und geringer Latenz, die mithilfe von Global Reach Ihre lokale ExpressRoute-Leitung mit Ihrer AVS-ExpressRoute-Leitung verbindet. Informationen zum Einrichten der Verbindung finden Sie unter [Herstellen einer Verbindung von lokalen Standorten mit AVS mithilfe von ExpressRoute](on-premises-connection.md).
* **ExpressRoute-Verbindung von Ihrem virtuellen Azure-Netzwerk mit Ihrem AVS-Regionsnetzwerk.** Dies ist eine sichere private Verbindung mit hoher Geschwindigkeit und geringer Latenz, die mithilfe von Gateways für virtuelle Netzwerke Ihr virtuelles Netzwerk in Azure mit Ihrer AVS-ExpressRoute-Leitung verbindet. Informationen zum Einrichten der Verbindung finden Sie unter [Herstellen einer Verbindung aus Ihrer privaten AVS-Cloudumgebung mit dem virtuellen Azure-Netzwerk mithilfe von ExpressRoute](azure-expressroute-connection.md).
* **Site-to-Site-VPN-Verbindung von Ihrem lokalen Rechenzentrum mit Ihrem AVS-Regionsnetzwerk.** Dies ist eine sichere private Netzwerkverbindung von Ihrem lokalen VPN-Gerät mit Ihrer privaten AVS-Cloudregion. Ausführliche Informationen finden Sie unter [Einrichten von VPN-Gateways im AVS-Netzwerk](vpn-gateway.md).

**Wie stelle ich eine Verbindung mit einer privaten AVS-Cloud her?**

Details zu Ihrer privaten AVS-Cloud können Sie im AVS-Portal anzeigen. Um eine Verbindung mit der vCenter-Instanz herzustellen, die Ihrer privaten AVS-Cloud entspricht, stellen Sie zunächst sich, dass eine Netzwerkverbindung über eine Site-to-Site- oder Point-to-Site-VPN-Verbindung oder eine ExpressRoute-Verbindung hergestellt wurde. Starten Sie dann das AVS-Portal über das Azure-Portal, und klicken Sie auf der Startseite oder auf der Seite mit den Details der privaten AVS-Cloud auf **vSphere-Client starten**.

**Worin besteht der Vorteil von ExpressRoute-Leitungen?**

Eine Azure ExpressRoute-Leitung bietet eine sichere Verbindung mit hoher Geschwindigkeit und niedriger Latenz. AVS bietet eine dedizierte ExpressRoute-Leitung pro Region und Kunde. Durch die Verwendung dieser Leitung können Sie eine sichere Verbindung von einem lokalen Standort und/oder Ihrem Azure-Abonnement aus herstellen.

**Welche Netzwerkkosten fallen für die Verbindung mit AVS an? Fallen Gebühren für ausgehenden Datenverkehr zwischen AVS und Azure oder für regionsübergreifende Verbindungen an?**

Es gibt keine AVS-Gebühren für ausgehenden Netzwerkdatenverkehr. Für ausgehenden Datenverkehr aus Ihrem virtuellen Netzwerk oder von einer lokalen ExpressRoute-Leitung gelten die Standardtarife von Azure.

## <a name="networking"></a>Netzwerk

**Welche Netzwerkfunktionen sind für meine private AVS-Cloud verfügbar?**

Sie können VLANs (und deren Subnetze) und Firewalltabellen bereitstellen sowie öffentliche IP-Adressen zuweisen und einem virtuellen Computer zuordnen, der in Ihrer privaten AVS-Cloud ausgeführt wird. Weitere Informationen zu den Netzwerkfunktionen finden Sie unter [Übersicht über VLANs und Subnetze](cloudsimple-vlans-subnets.md), [Übersicht über Firewalltabellen](cloudsimple-firewall-tables.md) und [Öffentliche CloudSimple-IP-Adresse – Übersicht](cloudsimple-public-ip-address.md).

**Wie richte ich verschiedene Subnetze für meine Anwendungen in meiner privaten AVS-Cloud ein?**

Sie erstellen VLANs in Ihrer privaten AVS-Cloud über das AVS-Portal. Nachdem Sie ein VLAN erstellt haben, können Sie über das VLAN eine verteilte Portgruppe in der vCenter-Instanz Ihrer privaten AVS-Cloud und virtuelle Computer erstellen, die mit der verteilten Portgruppe verbunden sind. Sie können eine Firewalltabelle für das VLAN/Subnetz aktivieren und Firewallregeln definieren, um den Netzwerkdatenverkehr zu sichern.

**Welche Firewalleinstellungen sind für meine privaten AVS-Clouds verfügbar?**

Sie können Regeln für den Datenverkehr in Nord-Süd- und in Ost-West-Richtung konfigurieren. Die Regeln werden in einer Firewalltabelle definiert. Die Firewalltabelle kann an VLANs in Ihrer privaten AVS-Cloud angefügt werden. Weitere Informationen finden Sie unter [Einrichten von Firewalltabellen und -regeln für private AVS-Clouds](firewall.md).

**Kann ich öffentliche IP-Adressen für virtuelle Computer in meiner privaten AVS-Cloudumgebung zuweisen?**

Im AVS-Portal können Sie eine neue öffentliche IP-Adresse reservieren und sie der privaten IP-Adresse eines virtuellen Computers oder einer Appliance zuordnen. Sie können auch neue Firewallregeln erstellen oder vorhandene Firewallregeln anwenden, um den Datenverkehr von bestimmten Ports und IP-Adressen im Portal zuzulassen. Weitere Informationen finden Sie unter [Zuordnen von öffentlichen IP-Adressen für die private AVS-Cloudumgebung](public-ips.md).

## <a name="security"></a>Sicherheit

**Welche Sicherheitsoptionen habe ich bei AVS?**

AVS bietet die folgenden Sicherheitsfunktionen zur Sicherung Ihrer privaten AVS-Cloudumgebung:

* **Verschlüsselung für ruhende Daten**. Sie können ruhende Daten verschlüsseln, die sich im vSAN-Speicher in Ihrer privaten AVS-Cloud befinden. vSAN unterstützt externe Schlüsselverwaltungsserver (KMS), die in Ihrer Azure vNet- oder der lokalen Umgebung bereitgestellt werden können. Weitere Informationen finden Sie unter [Konfigurieren der vSAN-Verschlüsselung für Ihre private AVS-Cloud](vsan-encryption.md).
* **Netzwerksicherheit**. Steuern Sie den Netzwerkdatenverkehr mit Firewallregeln, die zwischen Ihrer privaten AVS-Cloud und dem Internet, Ihrer privaten AVS-Cloud und der lokalen Umgebung oder innerhalb von Subnetzen Ihrer privaten AVS-Cloud gelten.
* **Sichere, private Verbindung**. Eine sichere private Verbindung wird zwischen Ihrem lokalen Netzwerk und Ihrem Azure-Abonnement eingerichtet.

## <a name="compute"></a>Compute

**Welche Arten von Hosts sind verfügbar?**

AVS bietet die folgenden Hosttypen:

* **CS28-Knoten:** CPU: 2 x 2,2 GHz, 28 Kerne insgesamt, 48 HT.  RAM: 256 GB.  Speicher: 1.600 GB NVMe-Cache, 5.760 GB Daten (All-Flash). Netzwerk: 4x25Gbe NIC
* **CS36-Knoten:** CPU: 2 x 2,3 GHz, 36 Kerne insgesamt, 72 HT.  RAM: 512 GB.  Speicher: 3.200 GB NVMe-Cache 11.520 GB Daten (All-Flash).  Netzwerk: 4x25Gbe NIC
* **CS36m-Knoten:** CPU: 2 x 2,3 GHz, 36 Kerne insgesamt, 72 HT.  RAM: 576 GB.  Speicher: 3200 GB NVMe-Cache 13360 GB Daten (All-Flash).  Netzwerk: 4x25Gbe NIC

**Wie werden Hardwarefehler behandelt?**

Die gesamte AVS-Infrastruktur wird fortwährend von der AVS-Plattform und unseren Service Operations-Teams überwacht. Wenn ein Hardwarefehler erkannt wird, wird Ihrer privaten AVS-Cloud ein neuer Knoten hinzugefügt, und der fehlerhafte Knoten wird entfernt.

## <a name="storage"></a>Storage

**Welche Arten von Speicher werden in einer privaten AVS-Cloud unterstützt?**

AVS bietet All-Flash VMware vSAN-Speicher für jede private AVS-Cloud. Jeder vSphere wird mit seinem eigenen vSAN-Datenspeicher erstellt. Weitere Informationen finden Sie unter [VMware-Komponenten für die private AVS-Cloud – vSAN-Speicher](vmware-components.md#vsan-storage).

**Wird die Verschlüsselung von Daten unterstützt?**
Ja. Sie können den vSAN-Speicher für Ihre private AVS-Cloud so einrichten, dass er einen Schlüsselverwaltungsserver (Key Management Server, KMS) verwendet, der lokal oder in Azure bereitgestellt wird, um die in der vSAN-Instanz gespeicherten Daten zu verschlüsseln.

**Wie werden fehlerhafte Datenträger behandelt?**

AVS überwacht kontinuierlich alle Hardwarekomponenten der privaten AVS-Cloud. Wenn ein Datenträgerfehler erkannt oder ein Datenträger anhand einer heuristischen Methode als fehlerhaft identifiziert wird, wird der privaten AVS-Cloud automatisch ein neuer Knoten hinzugefügt. Der Knoten mit dem fehlerhaften oder aktuell ausfallenden Datenträger wird aus der privaten AVS-Cloud entfernt.

## <a name="vmware"></a>VMware

**Wie führe ich umfangreiche Uploads oder Migrationsvorgänge von Anwendungen und Daten aus einer lokalen Umgebung aus?**

AVS stellt eine native VMware vSphere-Lösung bereit. Alle VMware-Tools für die Massenmigration von Daten können mit Ihrer privaten AVS-Cloud verwendet werden. Beispiele für Optionen:

* VMware HCX für die Massenmigration von Daten.
* „Kalte“ Migration von Daten mithilfe von Storage vMotion aus einem lokalen Standort zu AVS.

**Kann ich alle VMware-Tools installieren?**

AVS stellt eine native VMware vSphere-Lösung bereit. Alle VMware-Tools, die für die Verwaltung Ihrer lokalen vSphere-Umgebung verwendet werden, können für AVS verwendet werden. AVS unterstützt das BYOL-Modell (Bring-Your-Own-License) für die Installation von VMware-Tools.

**Wie werden Updated und Upgrades verwaltet?**

AVS verwaltet und aktualisiert alle Komponenten der Infrastruktur Ihrer privaten AVS-Cloud auf nahtlose und unterbrechungsfreie Art. Alle von VMware oder Infrastrukturanbietern freigegebenen Updates und Sicherheitspatches werden für die Aktualisierung eingeplant, sobald sie von AVS als qualifiziert eingestuft werden.

AVS führt keine Upgrades oder Updates für installierte Anwendungen in der privaten AVS-Cloud durch.

## <a name="azure-integration"></a>Azure-Integration

**Welche Azure-Dienste werden unterstützt?**

AVS stellt eine Azure ExpressRoute-Leitung für Ihr Abonnement in Azure bereit. Alle Dienste, die in Ihrem Abonnement ausgeführt werden, können eine Verbindung mit Ihrer privaten AVS-Cloud herstellen. Beispiele:

* **Azure Active Directory** als Identitätsquelle für Ihre AVS-vCenter-Instanz
* **Azure Storage** zum Speichern von Sicherungen, Images und anderen Daten aus Ihrer privaten AVS-Cloud
* **Hybridanwendungen** mit einer Anwendungsarchitektur, die öffentliche und private AVS-Clouds umfasst. Beispielsweise können Sie Webserver in Azure erstellen, die auf Anwendungs- und Datenbankserver in Ihrer privaten AVS-Cloud zugreifen.
* **Azure Monitor** und **Azure SecurityCenter** für Workloads, die unter VMware ausgeführt werden, unterstützen Protokollierung, Leistungsmetriken und Sicherheitsverwaltung.

**Wie ordne ich meine VMware-Mandanten in Azure zu?**

AVS bietet die einzigartige Möglichkeit, Ihre virtuellen VMware-Computer in einer privaten AVS-Cloud über das Azure-Portal verwalten zu können. Ein vCenter-Ressourcenpool (konfiguriert mit den gewünschten Ressourceneinschränkungen) kann vom globalen Administrator Ihrem Abonnement zugeordnet werden. 

**Welche Lizenzierungsvorteile erhalte ich mit Azure?**

Mit AVS können Sie den Azure-Vorteil bei Hybridnutzung nutzen und bis zu 90 Prozent bei Lizenzen sparen. Dieser Vorteil bewahrt Ihre Investitionen in Microsoft-Lizenzen und senkt Ihre Gesamtkosten im Vergleich zu anderen Cloudlösungen. Außerdem erhalten Sie verlängerte Sicherheitsupdates für Windows Server 2008 und Microsoft SQL Server 2008. Das BYOL-Modell (Bring Your Own License) hilft Ihnen dabei, die Kosten für gängige Apps wie Veeam und Zerto niedrig zu halten. 
