---
title: Konfigurieren Ihres verwalteten Service Fabric-Clusters
description: Erfahren Sie, wie Sie Ihren verwalteten Service Fabric-Cluster für automatische Betriebssystemupgrades, NSG-Regeln und mehr konfigurieren.
ms.topic: how-to
ms.date: 5/10/2021
ms.openlocfilehash: 5fd2736917517f20d3d093492c6011d6fcf716cf
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110067436"
---
# <a name="service-fabric-managed-cluster-configuration-options"></a>Konfigurationsoptionen für verwaltete Service Fabric-Cluster

Zusätzlich zur Auswahl der [SKUs für verwaltete Service Fabric-Cluster](overview-managed-cluster.md#service-fabric-managed-cluster-skus) beim Erstellen des Clusters gibt es eine Reihe weiterer Möglichkeiten, sie zu konfigurieren:

* Hinzufügen einer [VM-Skalierungsgruppenerweiterung](how-to-managed-cluster-vmss-extension.md) zu einem verwalteten Knotentyp
* Konfigurieren eines [verfügbarkeitszonenübergreifenden](how-to-managed-cluster-availability-zones.md) Clusters
* Konfigurieren von [NSG-Regeln und anderen Netzwerkoptionen](how-to-managed-cluster-networking.md) für einen Cluster
* Konfigurieren der [verwalteten Identität](how-to-managed-identity-managed-cluster-virtual-machine-scale-sets.md) für Clusterknotentypen
* Aktiveren [automatischer Betriebssystemupgrades](how-to-managed-cluster-configuration.md#enable-automatic-os-image-upgrades) für Clusterknoten
* Aktivieren der [Betriebssystem- und Datenträgerverschlüsselung](how-to-enable-managed-cluster-disk-encryption.md) für Clusterknoten
* Auswählen der [SKU für den verwalteten Datenträger](how-to-managed-cluster-managed-disk.md) des Clusters
* Konfigurieren von [Upgradeoptionen für die Runtime](how-to-managed-cluster-upgrades.md) im Cluster

## <a name="enable-automatic-os-image-upgrades"></a>Aktivieren automatischer Betriebssystemupgrades

Sie können automatische Betriebssystemimage-Upgrades für die virtuellen Computer aktivieren, auf denen Ihre verwalteten Clusterknoten ausgeführt werden. Obwohl die VM-Skalierungsgruppenressourcen in Ihrem Auftrag mit verwalteten Service Fabric-Clustern verwaltet werden, können Sie automatische Betriebssystemimage-Upgrades für Ihre Clusterknoten aktivieren. Wie [klassische Service Fabric](service-fabric-best-practices-infrastructure-as-code.md#virtual-machine-os-automatic-upgrade-configuration)-Cluster werden verwaltete Clusterknoten nicht standardmäßig aktualisiert, um unbeabsichtigte Störungen Ihres Clusters zu verhindern.

So aktivieren Sie automatische Betriebssystemupgrades:

* Verwenden Sie die Version `2021-05-01` (oder höher) von *Microsoft.ServiceFabric/managedclusters*- und *Microsoft.ServiceFabric/managedclusters/nodetypes*-Ressourcen.
* Setzen Sie die Eigenschaft `enableAutoOSUpgrade` des Clusters auf *TRUE*.
* Setzen Sie die nodeTypes-Ressourceneigenschaft `vmImageVersion` des Clusters auf *latest*.

Beispiel:

```json
    {
      "apiVersion": "2021-05-01",
      "type": "Microsoft.ServiceFabric/managedclusters",
      ...
      "properties": {
        ...
        "enableAutoOSUpgrade": true
      },
    },
    {
      "apiVersion": "2021-05-01",
      "type": "Microsoft.ServiceFabric/managedclusters/nodetypes",
       ...
      "properties": {
        ...
        "vmImageVersion": "latest",
        ...
      }
    }
}

```

Nach der Aktivierung beginnt Service Fabric mit der Abfrage und Nachverfolgung von Betriebssystemimage-Versionen im verwalteten Cluster. Wenn eine neue Betriebssystemversion verfügbar ist, werden die Clusterknotentypen (VM-Skalierungsgruppen) nacheinander aktualisiert. Service Fabric-Runtimeupgrades werden erst ausgeführt, nachdem feststeht, dass derzeit keine Betriebssystemimage-Upgrades für Clusterknoten ausgeführt werden.

Wenn bei einem Upgrade ein Fehler auftritt, versucht Service Fabric es nach 24 Stunden erneut. Maximal drei erneute Versuche werden unternommen. Ähnlich wie bei klassischen (nicht verwalteten) Service Fabric-Upgrades können fehlerhafte Apps oder Knoten das Betriebssystemimage-Upgrade blockieren.

Weitere Informationen zu Imageupgrades finden Sie unter [Automatische Betriebssystemimageupgrades mit Azure-VM-Skalierungsgruppen](../virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade.md).

## <a name="next-steps"></a>Nächste Schritte

[Verwaltete Service Fabric-Cluster (Übersicht)](overview-managed-cluster.md)

[Service Fabric cluster templates](https://github.com/Azure-Samples/service-fabric-cluster-templates) (Service Fabric-Clustervorlagen)
