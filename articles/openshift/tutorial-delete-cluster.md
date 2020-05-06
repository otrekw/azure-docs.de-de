---
title: 'Tutorial: Löschen eines Azure Red Hat OpenShift-Clusters'
description: In diesem Tutorial erfahren Sie, wie Sie einen Azure Red Hat OpenShift-Cluster mithilfe der Azure-Befehlszeilenschnittstelle löschen.
author: sakthi-vetrivel
ms.author: suvetriv
ms.topic: tutorial
ms.service: container-service
ms.date: 04/24/2020
ms.openlocfilehash: 2de60b90eb6fb75ef013a2fd8785f1b8b616fba6
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82232137"
---
# <a name="tutorial-delete-an-azure-red-hat-openshift-4-cluster"></a>Tutorial: Löschen eines Azure Red Hat OpenShift 4-Clusters

In diesem Tutorial (Teil 3 von 3) erfahren Sie, wie Sie ein Azure Red Hat OpenShift-Cluster mit OpenShift 4 löschen. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Löschen eines Azure Red Hat OpenShift-Clusters


## <a name="before-you-begin"></a>Voraussetzungen

In den vorherigen Tutorials wurde erläutert, wie Sie ein Azure Red Hat OpenShift-Cluster erstellen und mithilfe der OpenShift-Webkonsole eine Verbindung herstellen. Falls Sie diese Schritte noch nicht ausgeführt haben und dies jetzt nachholen möchten, sollten Sie mit [Tutorial 1: Erstellen eines Azure Red Hat OpenShift 4-Clusters](tutorial-create-cluster.md) beginnen.

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Version 2.0.75 oder höher der Azure CLI ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

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
RESOURCE_GROUP=yourresourcegroup
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

Weitere Informationen zur Verwendung von OpenShift finden Sie in der offiziellen [Red Hat OpenShift-Dokumentation](https://www.openshift.com/try)
