---
title: Hinzufügen eines Knotentyps zu einem Azure Service Fabric-Cluster
description: In diesem Artikel erfahren Sie, wie ein Service Fabric-Cluster durch Hinzufügen einer VM-Skalierungsgruppe skaliert wird.
ms.topic: article
ms.date: 02/13/2019
ms.openlocfilehash: 1e7bae89561d43d717eb4d15e95183761b077443
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75463975"
---
# <a name="scale-a-service-fabric-cluster-out-by-adding-a-virtual-machine-scale-set"></a>Skalieren eines Service Fabric-Clusters durch Hinzufügen einer VM-Skalierungsgruppe
In diesem Artikel wird beschrieben, wie Sie einen Azure Service Fabric-Cluster skalieren, indem Sie einem vorhandenen Cluster einen neuen Knotentyp hinzufügen. Ein Service Fabric-Cluster enthält eine per Netzwerk verbundene Gruppe von virtuellen oder physischen Computern, auf denen Ihre Microservices bereitgestellt und verwaltet werden. Ein physischer oder virtueller Computer, der Teil eines Clusters ist, wird als Knoten bezeichnet. VM-Skalierungsgruppen sind eine Azure-Computeressource, mit der Sie eine Sammlung von virtuellen Computern als Gruppe bereitstellen und verwalten können. Jeder Knotentyp, der in einem Azure-Cluster definiert ist, wird [als separate Skalierungsgruppe eingerichtet](service-fabric-cluster-nodetypes.md). Jeder Knotentyp kann dann separat verwaltet werden. Nachdem Sie einen Service Fabric-Cluster erstellt haben, können Sie einen Cluster horizontal skalieren, indem Sie einem vorhandenen Cluster einen neuen Knotentyp (VM-Skalierungsgruppe) hinzufügen.  Sie können die Skalierung für den Cluster jederzeit durchführen – auch bei Ausführung von Workloads im Cluster.  Wenn der Cluster skaliert wird, werden Ihre Anwendungen ebenfalls automatisch skaliert.

## <a name="add-an-additional-scale-set-to-an-existing-cluster"></a>Hinzufügen einer weiteren Skalierungsgruppe zu einem vorhandenen Cluster
Das Hinzufügen eines neuen Knotentyps (durch eine VM-Skalierungsgruppe gesichert) zu einem vorhandenen Cluster lässt sich mit der [Durchführung eines Upgrades für den primären Knotentyp](service-fabric-scale-up-node-type.md) vergleichen. Der Unterschied ist lediglich, dass Sie nicht die gleiche NodeTypeRef-Eigenschaft verwenden, aktiv verwendete VM-Skalierungsgruppen natürlich nicht deaktivieren und dass die Clusterverfügbarkeit nicht verloren geht, wenn der primäre Knotentyp nicht aktualisiert wird. 

Die NodeTypeRef-Eigenschaft wird in den Eigenschaften der Service Fabric-Erweiterung der VM-Skalierungsgruppe deklariert:
```json
<snip>
"publisher": "Microsoft.Azure.ServiceFabric",
     "settings": {
     "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
     "nodeTypeRef": "[parameters('vmNodeType2Name')]",
     "dataPath": "D:\\\\SvcFab",
     "durabilityLevel": "Silver",
<snip>
```

Außerdem müssen Sie diesen neuen Knotentyp der Service Fabric-Clusterressource hinzufügen:

```json
<snip>
"nodeTypes": [
      {
      "name": "[parameters('vmNodeType2Name')]",
      "applicationPorts": {
                "endPort": "[parameters('nt2applicationEndPort')]",
                "startPort": "[parameters('nt2applicationStartPort')]"
      },
      "clientConnectionEndpointPort": "[parameters('nt2fabricTcpGatewayPort')]",
      "durabilityLevel": "Silver",
       "ephemeralPorts": {
                "endPort": "[parameters('nt2ephemeralEndPort')]",
                "startPort": "[parameters('nt2ephemeralStartPort')]"
      },
      "httpGatewayEndpointPort": "[parameters('nt2fabricHttpGatewayPort')]",
      "isPrimary": false,
      "vmInstanceCount": "[parameters('nt2InstanceCount')]"
},
<snip>
```

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie, wie der [primäre Knotentyp hochskaliert wird](service-fabric-scale-up-node-type.md).
* Machen Sie sich mit der [Skalierbarkeit von Anwendungen](service-fabric-concepts-scalability.md) vertraut.
* [Skalieren eines Service Fabric-Clusters](service-fabric-tutorial-scale-cluster.md) (horizontal hoch oder herunter)
* [Programmgesteuertes Skalieren eines Service Fabric-Clusters](service-fabric-cluster-programmatic-scaling.md) (per Azure Fluent-Compute-SDK)
* [Horizontales Herunter- oder Hochskalieren eines eigenständigen Clusters](service-fabric-cluster-windows-server-add-remove-nodes.md)

