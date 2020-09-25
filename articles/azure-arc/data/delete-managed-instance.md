---
title: Löschen von Azure Arc-fähigen SQL Managed Instance-Instanzen
description: Löschen von Azure Arc-fähigen SQL Managed Instance-Instanzen
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: vin-yu
ms.author: vinsonyu
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: e531349e8f404380d9f0601caa3b66557c297062
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931809"
---
# <a name="delete-azure-arc-enabled-sql-managed-instance"></a>Löschen von Azure Arc-fähigen SQL Managed Instance-Instanzen
In diesem Artikel wird beschrieben, wie Sie eine Azure Arc-fähige SQL Managed Instance-Instanz löschen.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="view-existing-azure-arc-enabled-sql-managed-instances"></a>Anzeigen vorhandener Azure Arc-fähigen SQL Managed Instance-Instanzen
Führen Sie den folgenden Befehl aus, um SQL Managed Instance-Instanzen anzuzeigen:

```console
azdata arc sql mi list
```

Die Ausgabe sollte ungefähr wie folgt aussehen:

```console
Name    Replicas    ServerEndpoint    State
------  ----------  ----------------  -------
demo-mi 1/1         10.240.0.4:32023  Ready
```

## <a name="delete-a-azure-arc-enabled-sql-managed-instance"></a>Löschen einer Azure Arc-fähigen SQL Managed Instance-Instanz
Führen Sie zum Löschen einer SQL Managed Instance-Instanz den folgenden Befehl aus:

```console
azdata arc sql mi delete -n <NAME_OF_INSTANCE>
```

Die Ausgabe sollte ungefähr wie folgt aussehen:

```console
# azdata arc sql mi delete -n demo-mi
Deleted demo-mi from namespace arc
```

## <a name="reclaim-the-kubernetes-persistent-volume-claims-pvcs"></a>Freigeben von Ansprüchen für persistente Kubernetes-Volumes (PVCs)

Durch das Löschen einer SQL Managed Instance-Instanz werden nicht deren zugeordnete [PVCs](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) (Persistent Volume Claims, Ansprüche für persistente Volumes) entfernt. Dies ist beabsichtigt. Auf diese Weise kann der Benutzer einfacher auf die Datenbankdateien zugreifen, falls eine Instanz versehentlich gelöscht wurde. Das Löschen von PVCs ist nicht obligatorisch. Es wird aber empfohlen. Wenn Sie diese PVCs nicht freigeben, treten letztendlich Fehler auf, da dem Kubernetes-Cluster der Speicherplatz ausgeht. Führen Sie die folgenden Schritte aus, um die PVCs freizugeben:

### <a name="1-list-the-pvcs-for-the-server-group-you-deleted"></a>1. Auflisten der PVCs für die Servergruppe, die Sie gelöscht haben
Führen Sie den folgenden Befehl aus, um die PVCs aufzulisten:
```console
kubectl get pvc
```

Beachten Sie im folgenden Beispiel die PVCs für die von Ihnen gelöschten SQL Managed Instance-Instanzen.
```console
# kubectl get pvc -n arc

NAME                    STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS      AGE
data-demo-mi-0        Bound     pvc-1030df34-4b0d-4148-8986-4e4c20660cc4   5Gi        RWO            managed-premium   13h
logs-demo-mi-0        Bound     pvc-11836e5e-63e5-4620-a6ba-d74f7a916db4   5Gi        RWO            managed-premium   13h
```

### <a name="2-delete-each-of-the-pvcs"></a>2. Löschen aller PVCs
Löschen Sie die Daten, und protokollieren Sie die PVCs für jede der von Ihnen gelöschten SQL Managed Instance-Instanzen.
Das allgemeine Format des Befehls lautet folgendermaßen: 
```console
kubectl delete pvc <name of pvc>
```

Zum Beispiel:
```console
kubectl delete pvc data-demo-mi-0 -n arc
kubectl delete pvc logs-demo-mi-0 -n arc
```

Mit jedem dieser kubectl-Befehle wird das erfolgreiche Löschen des PVCs bestätigt. Zum Beispiel:
```console
persistentvolumeclaim "data-demo-mi-0" deleted
persistentvolumeclaim "logs-demo-mi-0" deleted
```
  

> [!NOTE]
> Wie bereits erwähnt, kann das Nichtlöschen der PVCs dazu führen, dass Ihr Kubernetes-Cluster letztlich in eine Situation gerät, in der er Fehler verursacht. Zu diesen Fehlern kann es gehören, dass Sie sich nicht mit azdata bei Ihrem Kubernetes-Cluster anmelden können, da die Pods aufgrund dieses Speicherproblems ggf. aus dem Cluster entfernt werden (normales Kubernetes-Verhalten).
>
> Beispielsweise können Meldungen in den Protokollen ähnlich den folgenden angezeigt werden:  
> - Annotations: microsoft.com/ignore-pod-health: true  
> - Status:         Fehler  
> - Ursache:         Evicted (Entfernt)  
> - Meldung:        The node was low on resource: ephemeral-storage (Zu wenig Speicher des Knotens für die Ressource: ephemeral-storage). Container controller was using 16372Ki, which exceeds its request of 0 (Der Containercontroller hat 16372Ki verwendet, was die Anforderung von 0 überschreitet).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Features und Funktionen von Azure Arc-aktivierten SQL Managed Instance-Instanzen](managed-instance-features.md)

[Beginnen Sie mit der Erstellung eines Datencontrollers](create-data-controller.md)

Wurde bereits ein Datencontroller erstellt? [Erstellen von Azure Arc-fähigen SQL Managed Instance-Instanzen](create-sql-managed-instance.md)