---
title: Bewährte Methoden für Azure Service Fabric-Netzwerke
description: Regeln und Entwurfsüberlegungen für die Verwaltung der Netzwerkkonnektivität mithilfe von Azure Service Fabric
author: chrpap
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: chrpap
ms.openlocfilehash: caba864e77822ccab649f694df7e63e0ee5d6e51
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101732563"
---
# <a name="networking"></a>Netzwerk

Wenn Sie Azure Service Fabric-Cluster erstellen und verwalten, stellen Sie die Netzwerkverbindung für Ihre Knoten und Anwendungen bereit. Zu den Netzwerkressourcen gehören IP-Adressbereiche, virtuelle Netzwerke, Lastenausgleich und Netzwerksicherheitsgruppen. In diesem Artikel lernen Sie bewährte Methoden für diese Ressourcen kennen.

Lesen Sie Azure [Service Fabric-Netzwerkmuster](./service-fabric-patterns-networking.md), um zu erfahren, wie Sie Cluster erstellen, die die folgenden Features verwenden: Vorhandenes virtuelles Netzwerk oder Subnetz, statische öffentliche IP-Adresse, reiner interner Lastausgleich oder interner und externer Lastausgleich.

## <a name="infrastructure-networking"></a>Infrastrukturnetzwerke
Maximieren Sie die Leistung Ihres virtuellen Computers durch beschleunigten Netzwerkbetrieb, indem Sie die *enableAcceleratedNetworking*-Eigenschaft in Ihrer Resource Manager-Vorlage deklarieren. Der folgende Codeausschnitt stammt aus NetworkInterfaceConfigurations einer VM-Skalierungsgruppe, die beschleunigten Netzwerkbetrieb ermöglicht:

```json
"networkInterfaceConfigurations": [
  {
    "name": "[concat(variables('nicName'), '-0')]",
    "properties": {
      "enableAcceleratedNetworking": true,
      "ipConfigurations": [
        {
        <snip>
        }
      ],
      "primary": true
    }
  }
]
```
Der Service Fabric Cluster kann unter [Linux mit beschleunigtem Netzwerkbetrieb](../virtual-network/create-vm-accelerated-networking-cli.md) und unter [Windows mit beschleunigtem Netzwerkbetrieb](../virtual-network/create-vm-accelerated-networking-powershell.md) bereitgestellt werden.

Der beschleunigte Netzwerkbetrieb wird für SKUs der Azure Virtual Machine-Serie unterstützt: D/DSv2, D/DSv3, E/ESv3, F/FS, FSv2 und Ms/Mms. Der beschleunigte Netzwerkbetrieb wurde am 23.01.2019 erfolgreich mit der SKU Standard_DS8_v3 für einen Service Fabric Windows-Cluster und am 29.01.2019 mit Standard_DS12_v2 für einen Service Fabric Linux-Cluster getestet. Beachten Sie, dass der beschleunigte Netzwerkbetrieb mindestens 4 vCPUs erfordert. 

Um beschleunigten Netzwerkbetrieb für einen vorhandenen Service Fabric-Cluster zu aktivieren, müssen Sie zunächst [einen Service Fabric-Cluster erweitern, indem Sie eine VM-Skalierungsgruppe](./virtual-machine-scale-set-scale-node-type-scale-out.md) hinzufügen, um Folgendes auszuführen:
1. Bereitstellen eines NodeType mit aktiviertem beschleunigten Netzwerkbetrieb
2. Migrieren Ihrer Dienste und deren Status zum bereitgestellten NodeType mit aktiviertem beschleunigten Netzwerkbetrieb

Die Erweiterung der Infrastruktur ist erforderlich, um beschleunigten Netzwerkbetrieb für einen vorhandenen Cluster zu ermöglichen, da die Aktivierung von beschleunigtem Netzwerkbetrieb zu Ausfallzeiten führen würde, da alle virtuellen Computer in einer Verfügbarkeitsgruppe [beendet werden müssen und ihre Zuordnung aufgehoben werden muss, bevor beschleunigter Netzwerkbetrieb für eine vorhandene NIC](../virtual-network/create-vm-accelerated-networking-cli.md#enable-accelerated-networking-on-existing-vms) aktiviert werden kann.

## <a name="cluster-networking"></a>Clusternetzwerke

* Service Fabric-Cluster können in einem vorhandenen virtuellen Netzwerk bereitgestellt werden, indem die unter [Service Fabric-Netzwerkmuster](./service-fabric-patterns-networking.md) beschriebenen Schritte ausgeführt werden.

* Netzwerksicherheitsgruppen (NSGs) werden für Knotentypen empfohlen, um den eingehenden und ausgehenden Datenverkehr auf ihren Cluster zu beschränken. Stellen Sie sicher, dass die erforderlichen Ports in der NSG geöffnet sind. 

* Der primäre Knotentyp, der die Service Fabric-Systemdienste enthält, muss nicht über den externen Lastausgleich bereitgestellt werden. Er kann über einen [internen Lastausgleich](./service-fabric-patterns-networking.md#internal-only-load-balancer) bereitgestellt werden.

* Verwenden Sie eine [statische öffentliche IP-Adresse](./service-fabric-patterns-networking.md#static-public-ip-address-1) für Ihren Cluster.

## <a name="network-security-rules"></a>Netzwerksicherheitsregeln

Die hier angegebenen grundlegenden Regeln stellen die Mindestanforderungen für eine Sicherheitssperrung eines verwalteten Azure Service Fabric-Clusters dar. Wenn die folgenden Ports nicht geöffnet sind oder die IP-Adresse/URL nicht genehmigt wurde, kann der Cluster nicht ordnungsgemäß betrieben werden und wird möglicherweise nicht unterstützt. Bei diesem Regelsatz ist es zwingend erforderlich, [automatische Upgrades von Betriebssystemimages](../virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade.md) zu verwenden. Andernfalls müssen zusätzliche Ports geöffnet werden.

### <a name="inbound"></a>Eingehende Verbindungen 
|Priority   |Name               |Port        |Protocol  |`Source`             |Destination       |Aktion   
|---        |---                |---         |---       |---                |---               |---
|3900       |Azure              |19080       |TCP       |Internet           |VirtualNetwork    |Allow
|3910       |Client             |19000       |TCP       |Internet           |VirtualNetwork    |Allow
|3920       |Cluster            |1025-1027   |TCP       |VirtualNetwork     |VirtualNetwork    |Allow
|3930       |Kurzlebig          |49152-65534 |TCP       |VirtualNetwork     |VirtualNetwork    |Allow
|3940       |Application        |20000-30000 |TCP       |VirtualNetwork     |VirtualNetwork    |Allow
|3950       |SMB                |445         |TCP       |VirtualNetwork     |VirtualNetwork    |Allow
|3960       |RDP                |3389-3488   |TCP       |Internet           |VirtualNetwork    |Verweigern
|3970       |SSH                |22          |TCP       |Internet           |VirtualNetwork    |Verweigern
|3980       |Benutzerdefinierter Endpunkt    |80          |TCP       |Internet           |VirtualNetwork    |Allow
|4100       |Eingangssperre      |443         |Any       |Any                |Any               |Allow

Weitere Informationen zu den Eingangssicherheitsregeln:

* **Azure**. Über diesen Port durchsucht und verwaltet Service Fabric Explorer den Cluster. Außerdem fragt der Service Fabric-Ressourcenanbieter über ihn Informationen zu Ihrem Cluster ab, um sie im Azure-Verwaltungsportal anzuzeigen. Wenn der Service Fabric-Ressourcenanbieter nicht auf diesen Port zugreifen kann, wird im Azure-Portal eine Meldung wie „Knoten nicht gefunden“ oder „UpgradeServiceNotReachable“ angezeigt, und die Liste der Knoten und Anwendungen wird leer angezeigt. Wenn Sie Ihren Cluster im Azure-Portal anzeigen möchten, muss Ihr Lastenausgleich somit eine öffentliche IP-Adresse bereitstellen, und die NSG muss über Port 19080 eingehenden Datenverkehr zulassen.  

* **Client**. Der Clientverbindungs-Endpunkt für APIs wie REST/PowerShell/Befehlszeilenschnittstelle. 

* **Cluster**. Wird für die Kommunikation zwischen Knoten verwendet, sollte nie blockiert werden.

* **Kurzlebig**. Service Fabric nutzt einen Teil dieser Ports als Anwendungsports, und die restlichen Ports sind für das Betriebssystem verfügbar. Außerdem wird dieser Bereich dem vorhandenen Bereich des Betriebssystems zugeordnet, und Sie können die in den Beispielen hier angegebenen Bereiche für alle Zwecke verwenden. Stellen Sie sicher, dass der Unterschied zwischen den Start- und Endports mindestens 255 beträgt. Wenn dieser Unterschied zu gering ist, kommt es womöglich zu Konflikten, da dieser Bereich für das Betriebssystem freigegeben ist. Um den konfigurierten dynamischen Portbereich anzuzeigen, führen Sie *netsh int ipv4 show dynamic port tcp* aus. Diese Ports sind für Linux-Cluster nicht erforderlich.

* **Anwendung**. Der Portbereich der Anwendung sollte ausreichend sein, um die Endpunktanforderung Ihrer Anwendungen abzudecken. Dieser Bereich sollte vom dynamischen Portbereich auf dem Computer (dem „ephemeralPorts“-Bereich in der Konfiguration) getrennt sein. Service Fabric greift auf diese Ports zurück, wenn neue Ports benötigt werden, und öffnet die Firewall für diese Ports auf den Knoten.

* **SMB**. Das SMB-Protokoll wird vom Imagespeicherdienst für zwei Szenarien verwendet. Dieser Port wird zum Herunterladen der Pakete aus dem Imagespeicher von den Knoten sowie zum Replizieren zwischen den Replikaten benötigt. 

* **RDP**. Optional, wenn RDP aus dem Internet oder dem virtuellen Netzwerk für Jumpboxszenarien erforderlich ist. 

* **SSH**. Optional, wenn SSH aus dem Internet oder dem virtuellen Netzwerk für Jumpboxszenarien erforderlich ist.

* **Benutzerdefinierter Endpunkt**. Ein Beispiel für Ihre Anwendung zum Aktivieren eines aus dem Internet zugänglichen Endpunkts.

### <a name="outbound"></a>Ausgehend

|Priority   |Name               |Port        |Protocol  |`Source`             |Destination       |Aktion   
|---        |---                |---         |---       |---                |---               |---
|3900       |Netzwerk            |Any         |TCP       |VirtualNetwork     |VirtualNetwork    |Allow
|3910       |Ressourcenanbieter  |443         |TCP       |VirtualNetwork     |ServiceFabric     |Allow
|3920       |Aktualisieren            |443         |TCP       |VirtualNetwork     |Internet          |Allow
|3950       |Ausgangssperre     |Any         |Any       |Any                |Any               |Verweigern

Weitere Informationen zu den Ausgangssicherheitsregeln:

* **Netzwerk**. Der Kommunikationskanal für Subnetze und für andere virtuelle Netzwerke.

* **Ressourcenanbieter**. Verbindung durch den Aktualisierungsdienst zum Ausführen aller ARM-Bereitstellungen durch den Service Fabric-Ressourcenanbieter.

* **Aktualisierung**. Der Aktualisierungsdienst verwendet die Adresse download.microsoft.com, um die Bits abzurufen. Dies ist für die Einrichtung, die Neuerstellung von Images und Laufzeitupgrades erforderlich. Der Dienst arbeitet mit dynamischen IP-Adressen. Im Szenario mit einem nur internen Lastenausgleich muss der Vorlage ein zusätzlicher externer Lastenausgleich mit einer Regel hinzugefügt werden, die ausgehenden Datenverkehr für Port 443 zulässt. Optional kann dieser Port nach einer erfolgreichen Einrichtung blockiert werden. In diesem Fall muss das Upgradepaket jedoch an die Knoten verteilt werden, oder der Port muss für einen kurzen Zeitraum geöffnet werden. Anschließend ist ein manuelles Upgrade erforderlich.

Verwenden Sie Azure Firewall mit dem [NSG-Datenflussprotokoll](../network-watcher/network-watcher-nsg-flow-logging-overview.md) und der [Datenverkehrsanalyse](../network-watcher/traffic-analytics.md), um Probleme mit der Sicherheitssperrung zu verfolgen. Die ARM-Vorlage [Service Fabric mit NSG](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure-NSG) ist ein gutes Beispiel für den Einstieg. 


## <a name="application-networking"></a>Anwendungsnetzwerke

* Verwenden Sie zum Ausführen von Windows-Containerworkloads den [offenen Netzwerkmodus](./service-fabric-networking-modes.md#set-up-open-networking-mode), um die Kommunikation zwischen Diensten zu erleichtern.

* Verwenden Sie einen Reverseproxy wie [Traefik](https://docs.traefik.io/v1.6/configuration/backends/servicefabric/) oder den [Service Fabric-Reverseproxy](./service-fabric-reverseproxy.md), um allgemeine Anwendungsports wie 80 oder 443 bereitzustellen.

* Setzen Sie bei Windows-Containern, die auf Air-Gap-Computern gehostet werden, die keine Basisebenen aus dem Azure-Cloudspeicher pullen können, das Verhalten der fremden Ebene außer Kraft. Verwenden Sie dazu das Flag [--allow-nondistributable-artifacts](/virtualization/windowscontainers/about/faq#how-do-i-make-my-container-images-available-on-air-gapped-machines) im Docker-Daemon.

## <a name="next-steps"></a>Nächste Schritte

* Erstellen eines Clusters auf virtuellen Computern oder Computern mit Windows Server: [Erstellen eines Service Fabric-Clusters für Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Erstellen eines Clusters auf virtuellen Computern oder Computern mit Linux: [Erstellen eines Linux-Clusters](service-fabric-cluster-creation-via-portal.md)
* Informieren Sie sich über [Service Fabric-Supportoptionen](service-fabric-support.md).

[NSGSetup]: ./media/service-fabric-best-practices/service-fabric-nsg-rules.png
