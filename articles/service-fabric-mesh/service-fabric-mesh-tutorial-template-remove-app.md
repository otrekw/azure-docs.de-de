---
title: 'Tutorial: Entfernen einer in Azure Service Fabric Mesh ausgeführten App'
description: In diesem Tutorial erfahren Sie, wie Sie eine in Service Fabric Mesh ausgeführte Anwendung entfernen und Ressourcen löschen.
author: dkkapur
ms.topic: tutorial
ms.date: 01/11/2019
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: 4780f81d23f0183837d2aafb9a8e5e2c41faa1cf
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75351796"
---
# <a name="tutorial-remove-an-application-and-resources"></a>Tutorial: Entfernen einer Anwendung und der Ressourcen

Dieses Tutorial ist der vierte Teil einer Serie. Hier erfahren Sie, wie Sie eine ausgeführte Anwendung entfernen, die [zuvor in Service Fabric Mesh bereitgestellt wurde](service-fabric-mesh-tutorial-template-deploy-app.md). 

Im vierten Teil der Serie lernen Sie Folgendes:

> [!div class="checklist"]
> * Löschen einer in Service Fabric Mesh ausgeführten App
> * Löschen der Anwendungsressourcen

In dieser Tutorialserie lernen Sie Folgendes:
> [!div class="checklist"]
> * [Bereitstellen einer Anwendung in Service Fabric Mesh mithilfe einer Vorlage](service-fabric-mesh-tutorial-template-deploy-app.md)
> * [Skalieren einer in Service Fabric Mesh ausgeführten Anwendung](service-fabric-mesh-tutorial-template-scale-services.md)
> * [Aktualisieren einer in Service Fabric Mesh ausgeführten Anwendung](service-fabric-mesh-tutorial-template-upgrade-app.md)
> * Entfernen einer Anwendung

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:

* Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

* Öffnen Sie [Azure Cloud Shell](service-fabric-mesh-howto-setup-cli.md), oder [installieren Sie die Azure- und die Service Fabric Mesh-Befehlszeilenschnittstelle lokal](service-fabric-mesh-howto-setup-cli.md#install-the-azure-service-fabric-mesh-cli).

## <a name="delete-the-resource-group-and-all-the-resources"></a>Löschen der Ressourcengruppe und aller Ressourcen

Löschen Sie alle erstellten Ressourcen, wenn Sie sie nicht mehr benötigen. Zuvor haben Sie [eine neue Ressourcengruppe erstellt](service-fabric-mesh-tutorial-template-deploy-app.md#create-a-container-registry), um die Azure Container Registry-Instanz und die Service Fabric Mesh-Anwendungsressourcen zu hosten.  Sie können diese Ressourcengruppe löschen. Dabei werden auch alle zugehörigen Ressourcen gelöscht.

```azurecli
az group delete --resource-group myResourceGroup
```

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="individually-delete-the-resources"></a>Einzelnes Löschen der Ressourcen
Sie können die ACR-Instanz, die Service Fabric Mesh-Anwendung und die Netzwerkressourcen auch einzeln löschen.

So löschen Sie die ACR-Instanz:

```azurecli
az acr delete --resource-group myResourceGroup --name myContainerRegistry
```

So löschen Sie die Service Fabric Mesh-Anwendung:

```azurecli
az mesh app delete --resource-group myResourceGroup --name todolistapp
```

So löschen Sie das Netzwerk:
```azurecli
az mesh network delete --resource-group myResourceGroup --name todolistappNetwork
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Teil des Tutorials haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Löschen einer in Service Fabric Mesh ausgeführten App
> * Löschen der Anwendungsressourcen