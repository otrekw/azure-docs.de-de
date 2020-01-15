---
title: Bewährte Methoden für Azure Service Fabric-Netzwerke
description: Bewährte Methoden und Entwurfsüberlegungen für die Verwaltung der Netzwerkkonnektivität mithilfe von Azure Service Fabric.
author: peterpogorski
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: de2a74ad2d61de18d2150b72be3251e5b5583f2e
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2019
ms.locfileid: "75551793"
---
# <a name="networking"></a>Netzwerk

Wenn Sie Azure Service Fabric-Cluster erstellen und verwalten, stellen Sie die Netzwerkverbindung für Ihre Knoten und Anwendungen bereit. Zu den Netzwerkressourcen gehören IP-Adressbereiche, virtuelle Netzwerke, Lastenausgleich und Netzwerksicherheitsgruppen. In diesem Artikel lernen Sie bewährte Methoden für diese Ressourcen kennen.

Lesen Sie Azure [Service Fabric-Netzwerkmuster](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking), um zu erfahren, wie Sie Cluster erstellen, die die folgenden Features verwenden: Vorhandenes virtuelles Netzwerk oder Subnetz, statische öffentliche IP-Adresse, reiner interner Lastausgleich oder interner und externer Lastausgleich.

## <a name="infrastructure-networking"></a>Infrastrukturnetzwerke
Maximieren Sie die Leistung Ihres virtuellen Computers durch beschleunigten Netzwerkbetrieb, indem Sie die enableAcceleratedNetworking-Eigenschaft in Ihrer Resource Manager-Vorlage deklarieren. Der folgende Codeausschnitt stammt aus NetworkInterfaceConfigurations einer VM-Skalierungsgruppe, die beschleunigten Netzwerkbetrieb ermöglicht:

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
Der Service Fabric Cluster kann unter [Linux mit beschleunigtem Netzwerkbetrieb](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli) und unter [Windows mit beschleunigtem Netzwerkbetrieb](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell) bereitgestellt werden.

Der beschleunigte Netzwerkbetrieb wird für SKUs der Azure Virtual Machine-Serie unterstützt: D/DSv2, D/DSv3, E/ESv3, F/FS, FSv2 und Ms/Mms. Der beschleunigte Netzwerkbetrieb wurde am 23.01.2019 erfolgreich mit der SKU Standard_DS8_v3 für einen Service Fabric Windows-Cluster und am 29.01.2019 mit Standard_DS12_v2 für einen Service Fabric Linux-Cluster getestet.

Um beschleunigten Netzwerkbetrieb für einen vorhandenen Service Fabric-Cluster zu aktivieren, müssen Sie zunächst [einen Service Fabric-Cluster erweitern, indem Sie eine VM-Skalierungsgruppe](https://docs.microsoft.com/azure/service-fabric/virtual-machine-scale-set-scale-node-type-scale-out) hinzufügen, um Folgendes auszuführen:
1. Bereitstellen eines NodeType mit aktiviertem beschleunigten Netzwerkbetrieb
2. Migrieren Ihrer Dienste und deren Status zum bereitgestellten NodeType mit aktiviertem beschleunigten Netzwerkbetrieb

Die Erweiterung der Infrastruktur ist erforderlich, um beschleunigten Netzwerkbetrieb für einen vorhandenen Cluster zu ermöglichen, da die Aktivierung von beschleunigtem Netzwerkbetrieb zu Ausfallzeiten führen würde, da alle virtuellen Computer in einer Verfügbarkeitsgruppe [beendet werden müssen und ihre Zuordnung aufgehoben werden muss, bevor beschleunigter Netzwerkbetrieb für eine vorhandene NIC](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli#enable-accelerated-networking-on-existing-vms) aktiviert werden kann.

## <a name="cluster-networking"></a>Clusternetzwerke

* Service Fabric-Cluster können in einem vorhandenen virtuellen Netzwerk bereitgestellt werden, indem die unter [Service Fabric-Netzwerkmuster](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking) beschriebenen Schritte ausgeführt werden.

* Netzwerksicherheitsgruppen (NSGs) werden für Knotentypen empfohlen, die eingehenden und ausgehenden Datenverkehr auf ihrem Cluster beschränken. Stellen Sie sicher, dass die erforderlichen Ports in der NSG geöffnet sind. Beispiel:  ![Service Fabric-NSG-Regeln][NSGSetup]

* Der primäre Knotentyp, der die Service Fabric-Systemdienste enthält, muss nicht über den externen Lastausgleich bereitgestellt werden. Er kann über einen [internen Lastausgleich](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking#internal-only-load-balancer) bereitgestellt werden.

* Verwenden Sie eine [statische öffentliche IP-Adresse](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking#static-public-ip-address-1) für Ihren Cluster.

## <a name="application-networking"></a>Anwendungsnetzwerke

* Verwenden Sie zum Ausführen von Windows-Containerworkloads den [offenen Netzwerkmodus](https://docs.microsoft.com/azure/service-fabric/service-fabric-networking-modes#set-up-open-networking-mode), um die Kommunikation zwischen Diensten zu erleichtern.

* Verwenden Sie einen Reverseproxy wie [Traefik](https://docs.traefik.io/v1.6/configuration/backends/servicefabric/) oder den [Service Fabric-Reverseproxy](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy), um allgemeine Anwendungsports wie 80 oder 443 bereitzustellen.

* Setzen Sie bei Windows-Containern, die auf Air-Gap-Computern gehostet werden, die keine Basisebenen aus dem Azure-Cloudspeicher pullen können, das Verhalten der fremden Ebene außer Kraft. Verwenden Sie dazu das Flag [--allow-nondistributable-artifacts](https://docs.microsoft.com/virtualization/windowscontainers/about/faq#how-do-i-make-my-container-images-available-on-air-gapped-machines) im Docker-Daemon.

## <a name="next-steps"></a>Nächste Schritte

* Erstellen eines Clusters auf virtuellen Computern oder Computern mit Windows Server: [Erstellen eines Service Fabric-Clusters für Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Erstellen eines Clusters auf virtuellen Computern oder Computern mit Linux: [Erstellen eines Linux-Clusters](service-fabric-cluster-creation-via-portal.md)
* Informieren Sie sich über [Service Fabric-Supportoptionen](service-fabric-support.md).

[NSGSetup]: ./media/service-fabric-best-practices/service-fabric-nsg-rules.png
