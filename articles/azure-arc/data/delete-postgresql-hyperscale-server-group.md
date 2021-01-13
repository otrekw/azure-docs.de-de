---
title: Löschen einer Azure Arc-fähigen PostgreSQL Hyperscale-Servergruppe
description: Löschen einer Azure Arc-fähigen Postgres Hyperscale-Servergruppe
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: ac620909996b03a97a311e5f06c31d6dab8f1a60
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92218645"
---
# <a name="delete-an-azure-arc-enabled-postgresql-hyperscale-server-group"></a>Löschen einer Azure Arc-fähigen PostgreSQL Hyperscale-Servergruppe

In diesem Dokument werden die Schritte zum Löschen einer Servergruppe aus Ihrem Azure Arc-Setup beschrieben.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="delete-the-server-group"></a>Löschen der Servergruppe

Beispielsweise möchten wir die Instanz _postgres01_ aus dem folgenden Setup löschen:

```console
azdata arc postgres server list
Name        State    Workers
----------  -------  ---------
postgres01  Ready    3
```

Das allgemeine Format des Löschbefehls lautet:
```console
azdata arc postgres server delete -n <server group name>
```
Um weitere Informationen zum Löschbefehl zu erhalten, führen Sie Folgendes aus:
```console
azdata arc postgres server delete --help
```

### <a name="delete-the-server-group-used-in-this-example"></a>Löschen der in diesem Beispiel verwendeten Servergruppe

```console
azdata arc postgres server delete -n postgres01
```

## <a name="reclaim-the-kubernetes-persistent-volume-claims-pvcs"></a>Freigeben von Ansprüchen für persistente Kubernetes-Volumes

Durch das Löschen einer Servergruppe werden nicht deren zugeordnete [PVCs](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) (Persistent Volume Claims, Ansprüche für persistente Volumes) entfernt. Dies ist beabsichtigt. Auf diese Weise kann der Benutzer einfacher auf die Datenbankdateien zugreifen, falls eine Instanz versehentlich gelöscht wurde. Das Löschen von PVCs ist nicht obligatorisch, wird aber empfohlen. Wenn Sie diese PVCs nicht freigeben, treten letztendlich Fehler auf, da der Kubernetes-Cluster davon ausgeht, dass nicht mehr über genügend Speicherplatz verfügbar ist. Führen Sie die folgenden Schritte aus, um die PVCs freizugeben:

### <a name="1-list-the-pvcs-for-the-server-group-you-deleted"></a>1. Listen Sie die PVCs für die Servergruppe auf, die Sie gelöscht haben.

Führen Sie den folgenden Befehl aus, um die PVCs aufzulisten:

```console
kubectl get pvc [-n <namespace name>]
```

Mit dem Befehl wird die Liste der PVCs zurückgegeben – insbesondere der PVCs für die von Ihnen gelöschte Servergruppe. Zum Beispiel:

```output
kubectl get pvc
NAME                                         STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
data-few7hh0k4npx9phsiobdc3hq-postgres01-0   Bound    pvc-72ccc225-dad0-4dee-8eae-ed352be847aa   5Gi        RWO            default        2d18h
data-few7hh0k4npx9phsiobdc3hq-postgres01-1   Bound    pvc-ce6f0c51-faed-45ae-9472-8cdf390deb0d   5Gi        RWO            default        2d18h
data-few7hh0k4npx9phsiobdc3hq-postgres01-2   Bound    pvc-5a863ab9-522a-45f3-889b-8084c48c32f8   5Gi        RWO            default        2d18h
data-few7hh0k4npx9phsiobdc3hq-postgres01-3   Bound    pvc-00e1ace3-1452-434f-8445-767ec39c23f2   5Gi        RWO            default        2d15h
logs-few7hh0k4npx9phsiobdc3hq-postgres01-0   Bound    pvc-8b810f4c-d72a-474a-a5d7-64ec26fa32de   5Gi        RWO            default        2d18h
logs-few7hh0k4npx9phsiobdc3hq-postgres01-1   Bound    pvc-51d1e91b-08a9-4b6b-858d-38e8e06e60f9   5Gi        RWO            default        2d18h
logs-few7hh0k4npx9phsiobdc3hq-postgres01-2   Bound    pvc-8e5ad55e-300d-4353-92d8-2e383b3fe96e   5Gi        RWO            default        2d18h
logs-few7hh0k4npx9phsiobdc3hq-postgres01-3   Bound    pvc-f9e4cb98-c943-45b0-aa07-dd5cff7ea585   5Gi        RWO            default        2d15h
```
Für diese Servergruppe gibt es 8 PVCs.

### <a name="2-delete-each-of-the-pvcs"></a>2. Löschen Sie alle PVCs.

Löschen Sie die Daten, und protokollieren Sie die PVCs für alle PostgreSQL Hyperscale-Knoten (Koordinator und Worker) der von Ihnen gelöschten Servergruppe.

Das allgemeine Format des Befehls lautet: 

```console
kubectl delete pvc <name of pvc>  [-n <namespace name>]
```

Zum Beispiel:

```console
kubectl delete pvc data-few7hh0k4npx9phsiobdc3hq-postgres01-0
kubectl delete pvc data-few7hh0k4npx9phsiobdc3hq-postgres01-1
kubectl delete pvc data-few7hh0k4npx9phsiobdc3hq-postgres01-2
kubectl delete pvc data-few7hh0k4npx9phsiobdc3hq-postgres01-3
kubectl delete pvc logs-few7hh0k4npx9phsiobdc3hq-postgres01-0
kubectl delete pvc logs-few7hh0k4npx9phsiobdc3hq-postgres01-1
kubectl delete pvc logs-few7hh0k4npx9phsiobdc3hq-postgres01-2
kubectl delete pvc logs-few7hh0k4npx9phsiobdc3hq-postgres01-3
```

Mit jedem dieser kubectl-Befehle wird das erfolgreiche Löschen des PVCs bestätigt. Zum Beispiel:

```output
persistentvolumeclaim "data-postgres01-0" deleted
```
  

>[!NOTE]
> Wie bereits erwähnt, kann das Nichtlöschen der PVCs dazu führen, dass Ihr Kubernetes-Cluster letztlich in eine Situation gerät, in der er Fehler verursacht. Diese Fehler können u. U. bewirken, dass Sie sich nicht mit azdata bei Ihrem Kubernetes-Cluster anmelden können, da die Pods aufgrund dieses Speicherproblems ggf. aus dem Cluster entfernt werden (normales Kubernetes-Verhalten).
>
> Beispielsweise können in den Protokollen Meldungen wie die folgende angezeigt werden:  
> ```output
> Annotations:    microsoft.com/ignore-pod-health: true  
> Status:         Failed  
> Reason:         Evicted  
> Message:        The node was low on resource: ephemeral-storage. Container controller was using 16372Ki, which exceeds its request of 0.
> ```
    
## <a name="next-step"></a>Nächster Schritt
Erstellen von [Azure Arc-fähigem PostgreSQL Hyperscale](create-postgresql-hyperscale-server-group.md)
