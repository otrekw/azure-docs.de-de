---
title: Hinzufügen und Entfernen von Knotentypen eines verwalteten Service Fabric-Clusters (Vorschau)
description: In diesem Tutorial erfahren Sie, wie Sie Knotentypen eines verwalteten Service Fabric-Clusters hinzufügen und entfernen.
ms.topic: tutorial
ms.date: 09/28/2020
ms.openlocfilehash: 860345f5b297edaeea9d099ac392243176dfaca7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91410224"
---
# <a name="tutorial-add-and-remove-node-types-from-a-service-fabric-managed-cluster-preview"></a>Tutorial: Hinzufügen und Entfernen von Knotentypen in einem verwalteten Service Fabric-Cluster (Vorschau)

In dieser Tutorialreihe wird Folgendes behandelt:

> [!div class="checklist"]
> * [Bereitstellen eines verwalteten Service Fabric-Clusters](tutorial-managed-cluster-deploy.md)
> * [Aufskalieren eines verwalteten Service Fabric-Clusters](tutorial-managed-cluster-scale.md)
> * Hinzufügen und Entfernen von Knoten in einem verwalteten Service Fabric-Cluster
> * [Bereitstellen einer Anwendung in einem verwalteten Service Fabric-Cluster](tutorial-managed-cluster-deploy-app.md)

In diesem Teil der Reihe wird Folgendes behandelt:

> [!div class="checklist"]
> * Hinzufügen eines Knotentyps zu einem verwalteten Service Fabric-Cluster
> * Löschen eines Knotentyps aus einem verwalteten Service Fabric-Cluster

## <a name="prerequisites"></a>Voraussetzungen

* Ein verwalteter Service Fabric-Cluster (siehe [*Bereitstellen eines verwalteten Clusters*](tutorial-managed-cluster-deploy.md)).
* [Azure PowerShell 4.7.0](https://docs.microsoft.com/powershell/azure/release-notes-azureps?view=azps-4.7.0&preserve-view=true#azservicefabric) oder höher (siehe [*Installieren von Azure PowerShell*](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-4.7.0&preserve-view=true)).

## <a name="add-a-node-type-to-a-service-fabric-managed-cluster"></a>Hinzufügen eines Knotentyps zu einem verwalteten Service Fabric-Cluster

Sie können einem verwalteten Service Fabric-Cluster mit einer Azure Resource Manager-Vorlage, mit PowerShell oder mit der CLI einen Knotentyp hinzufügen. In diesem Tutorial fügen wir mit Azure PowerShell einen Knotentyp hinzu.

Um einen neuen Knotentyp zu erstellen, müssen Sie drei Eigenschaften definieren:

* **Name des Knotentyps**: Der Name, der für alle vorhandenen Knotentypen im Cluster eindeutig ist.
* **Instanzenanzahl**: Die anfängliche Anzahl von Knoten des neuen Knotentyps.
* **VM-Größe**: Die VM-SKU für die Knoten. Wenn Sie hier nichts angeben, wird der Standardwert *Standard_D2* verwendet.

> [!NOTE]
> Wenn der hinzugefügte Knotentyp der erste oder einzige Knotentyp im Cluster ist, muss die Eigenschaft „Primary“ verwendet werden.

```powershell
$resourceGroup = "myResourceGroup"
$clusterName = "mysfcluster"
$nodeTypeName = "nt2"
$vmSize = "Standard_D2_v2"

New-AzServiceFabricManagedNodeType -ResourceGroupName $resourceGroup -ClusterName $clusterName -Name $nodeTypeName -InstanceCount 3 -vmSize $vmSize
```

## <a name="remove-a-node-type-from-a-service-fabric-managed-cluster"></a>Entfernen eines Knotentyps aus einem verwalteten Service Fabric-Cluster

Wenn Sie einen Knotentyp aus einem verwalteten Service Fabric-Cluster entfernen möchten, müssen Sie PowerShell oder die CLI verwenden. In diesem Tutorial entfernen wir mit Azure PowerShell einen Knotentyp.

> [!NOTE]
> Es ist nicht möglich, einen primären Knotentyp zu entfernen, wenn er der einzige primäre Knotentyp im Cluster ist.  

So entfernen Sie einen Knotentyp:

```powershell
$resourceGroup = "myResourceGroup"
$clusterName = "myCluster"
$nodeTypeName = "nt2"

Remove-AzServiceFabricManagedNodeType -ResourceGroupName $resourceGroup -ClusterName $clusterName  -Name $nodeTypeName
```

## <a name="next-steps"></a>Nächste Schritte

 In diesem Abschnitt haben wir Knotentypen hinzugefügt und gelöscht. Informationen zum Bereitstellen einer Anwendung in einem verwalteten Service Fabric-Cluster finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Bereitstellen einer App in einem verwalteten Service Fabric-Cluster](tutorial-managed-cluster-deploy-app.md)
