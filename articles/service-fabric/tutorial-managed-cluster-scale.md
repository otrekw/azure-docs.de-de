---
title: Aufskalieren eines verwalteten Service Fabric-Clusters (Vorschau)
description: In diesem Tutorial erfahren Sie, wie Sie Knotentypen eines verwalteten Service Fabric-Clusters aufskalieren.
ms.topic: tutorial
ms.date: 09/28/2020
ms.openlocfilehash: a6777743932f3cc73db973c4d9935d4b5827700b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91410208"
---
# <a name="tutorial-scale-out-a-service-fabric-managed-cluster-preview"></a>Tutorial: Aufskalieren eines verwalteten Service Fabric-Clusters (Vorschau)

In dieser Tutorialreihe wird Folgendes behandelt:

> [!div class="checklist"]
> * [Bereitstellen eines verwalteten Service Fabric-Clusters.](tutorial-managed-cluster-deploy.md)
> * Aufskalieren eines verwalteten Service Fabric-Clusters
> * [Hinzufügen und Entfernen von Knoten in einem verwalteten Service Fabric-Cluster](tutorial-managed-cluster-add-remove-node-type.md)
> * [Bereitstellen einer Anwendung in einem verwalteten Service Fabric-Cluster](tutorial-managed-cluster-deploy-app.md)

In diesem Teil der Reihe wird Folgendes behandelt:

> [!div class="checklist"]
> * Skalieren eines verwalteten Service Fabric-Clusterknotens

## <a name="prerequisites"></a>Voraussetzungen

* Ein verwalteter Service Fabric-Cluster (siehe [*Bereitstellen eines verwalteten Clusters*](tutorial-managed-cluster-deploy.md)).
* [Azure PowerShell 4.7.0](https://docs.microsoft.com/powershell/azure/release-notes-azureps?view=azps-4.7.0&preserve-view=true#azservicefabric) oder höher (siehe [*Installieren von Azure PowerShell*](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-4.7.0&preserve-view=true)).

## <a name="scale-a-service-fabric-managed-cluster"></a>Skalieren eines verwalteten Service Fabric-Clusters
Ändern Sie die Instanzanzahl, um die Anzahl der Knoten für den Knotentyp zu erhöhen oder zu verringern, die Sie skalieren möchten. Sie finden Knotentypnamen in der ARM-Vorlage (Azure Resource Manager) aus der Clusterbereitstellung oder in Service Fabric Explorer.  

> [!NOTE]
> Wenn der Knotentyp auf einen primären Typ festgelegt ist, sind Sie nicht in der Lage, unter 3 Knoten für einen Cluster der SKU „Basic“ und unter 5 Knoten für einen Cluster der SKU „Standard“ zu skalieren.

```powershell
$resourceGroup = "myResourceGroup"
$clusterName = "mysfcluster"
$nodeTypeName = "FE"
$instanceCount = "7"

Set-AzServiceFabricManagedNodeType -ResourceGroupName $resourceGroup -ClusterName $clusterName -name $nodeTypeName -InstanceCount $instanceCount -Verbose
```

Das Upgrade des Clusters wird automatisch ausgeführt, und nach einigen Minuten werden die zusätzlichen Knoten angezeigt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schritt haben wir einen Knotentyp auf einem verwalteten Service Fabric-Cluster skaliert. Weitere Informationen zum Hinzufügen und Entfernen von Knotentypen finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Hinzufügen und Entfernen von Knoten in einem verwalteten Service Fabric-Cluster](tutorial-managed-cluster-add-remove-node-type.md)
