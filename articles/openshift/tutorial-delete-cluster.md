---
title: 'Tutorial: Löschen eines Azure Red Hat OpenShift-Clusters'
description: In diesem Tutorial erfahren Sie, wie Sie einen Azure Red Hat OpenShift-Cluster mithilfe der Azure-Befehlszeilenschnittstelle löschen.
author: sakthi-vetrivel
ms.custom: fasttrack-edit
ms.author: suvetriv
ms.topic: tutorial
ms.service: azure-redhat-openshift
ms.date: 04/24/2020
ms.openlocfilehash: 3830942d63c3db9646b84d43d65f7c68cb5fcf9e
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102209789"
---
# <a name="tutorial-delete-an-azure-red-hat-openshift-4-cluster"></a>Tutorial: Löschen eines Azure Red Hat OpenShift 4-Clusters

In diesem Tutorial (Teil 3 von 3) erfahren Sie, wie Sie ein Azure Red Hat OpenShift-Cluster mit OpenShift 4 löschen. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Löschen eines Azure Red Hat OpenShift-Clusters


## <a name="before-you-begin"></a>Vorbereitung

In den vorherigen Tutorials wurde erläutert, wie Sie ein Azure Red Hat OpenShift-Cluster erstellen und mithilfe der OpenShift-Webkonsole eine Verbindung herstellen. Falls Sie diese Schritte noch nicht ausgeführt haben und dies jetzt nachholen möchten, sollten Sie mit [Tutorial 1: Erstellen eines Azure Red Hat OpenShift 4-Clusters](tutorial-create-cluster.md) beginnen.

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial Version 2.6.0 oder höher der Azure CLI ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Wenn Sie die Azure CLI lokal ausführen, führen Sie `az login` aus, um sich bei Azure anzumelden.

```bash
az login
```

Wenn Sie Zugriff auf mehrere Abonnements haben, führen Sie `az account set -s {subscription ID}` aus, und ersetzen Sie `{subscription ID}` durch das zu verwendende Abonnement.

## <a name="delete-the-cluster"></a>Löschen des Clusters

In den vorherigen Tutorials wurden die folgenden Variablen festgelegt.

```bash
CLUSTER=yourclustername
RESOURCEGROUP=yourresourcegroup
```

Löschen Sie mithilfe dieser Werte Ihren Cluster:

```bash
az aro delete --resource-group $RESOURCEGROUP --name $CLUSTER
```

Anschließend werden Sie aufgefordert, zu bestätigen, dass Sie den Cluster löschen möchten. Nachdem Sie mit `y` bestätigt haben, nimmt das Löschen des Clusters einige Minuten in Anspruch. Wenn der Befehl abgeschlossen ist, werden die gesamte Ressourcengruppe und alle darin enthaltenen Ressourcen einschließlich des Clusters gelöscht.

## <a name="next-steps"></a>Nächste Schritte

In diesem Teil des Tutorials haben Sie Folgendes gelernt:
> [!div class="checklist"]
> * Löschen eines Azure Red Hat OpenShift 4-Clusters

Weitere Informationen zur Verwendung von OpenShift finden Sie in der offiziellen [Red Hat OpenShift-Dokumentation](https://docs.openshift.com/container-platform/4.6/welcome/index.html)
