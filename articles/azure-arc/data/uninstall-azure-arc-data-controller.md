---
title: Löschen des Azure Arc-Datencontrollers
description: Löschen des Azure Arc-Datencontrollers
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: a040200c5746defcaee84a951521d5919c0c4d28
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91660675"
---
# <a name="delete-azure-arc-data-controller"></a>Löschen des Azure Arc-Datencontrollers

Im folgenden Artikel wird beschrieben, wie Sie einen Azure Arc-Datencontroller löschen.

Bevor Sie fortfahren, sollten Sie sicherstellen, dass alle Datendienste, die auf dem Datencontroller erstellt wurden, wie folgt entfernt wurden:

## <a name="log-in-to-the-data-controller"></a>Anmelden beim Datencontroller

Melden Sie sich beim Datencontroller an, den Sie löschen möchten:

```
azdata login
```

## <a name="list--delete-existing-data-services"></a>Auflisten und Löschen vorhandener Datendienste

Führen Sie den folgenden Befehl aus, um zu überprüfen, ob SQL Managed Instance-Instanzen erstellt wurden:

```
azdata arc sql mi list
```

Führen Sie für jede verwaltete SQL-Instanz aus der obigen Liste den Löschbefehl wie folgt aus:

```
azdata arc sql mi delete -n <name>
# for example: azdata arc sql mi delete -n sqlinstance1
```

Führen Sie ebenso für die Suche nach PostgreSQL Hyperscale-Instanzen Folgendes aus:

```
azdata arc postgres server list
```

Führen Sie zudem für jede PostgreSQL Hyperscale-Instanz den Löschbefehl wie folgt aus:
```
azdata arc postgres server delete -n <name>
# for example: azdata arc postgres server delete -n pg1
```

## <a name="delete-controller"></a>Löschen des Controllers

Nachdem alle SQL Managed Instance-Instanzen und PostgreSQL Hyperscale-Instanzen entfernt wurden, kann der Datencontroller wie folgt gelöscht werden:

```
azdata arc dc delete -n <name> -ns <namespace>
# for example: azdata arc dc delete -ns arc -n arcdc
```

### <a name="remove-sccs-red-hat-openshift-only"></a>Entfernen Sie die SCCs (nur in Red Hat OpenShift).

```console
oc adm policy remove-scc-from-user privileged -z default -n arc
oc adm policy remove-scc-from-user anyuid     -z default -n arc
```

### <a name="delete-cluster-level-objects"></a>Löschen Sie Objekte auf Clusterebene.

Zusätzlich zu den im Namespace gültigen Objekten führen Sie, wenn Sie auch die Objekte auf Clusterebene löschen möchten, z. B. die CRDs `clusterroles` und `clusterrolebindings`, die folgenden Befehle aus:

```console
# Cleanup azure arc data service artifacts
#Delete CRDs
kubectl delete crd datacontrollers.arcdata.microsoft.com 
kubectl delete crd sqlmanagedinstances.sql.arcdata.microsoft.com 
kubectl delete crd postgresql-11s.arcdata.microsoft.com 
kubectl delete crd postgresql-12s.arcdata.microsoft.com
#Delete clusterrole
kubectl delete clusterrole <namespace>:cr-arc-metricsdc-reader
#For example: kubectl delete clusterrole arc:cr-arc-metricsdc-reader
#Delete rolebinding
kubectl delete clusterrolebinding <namespace>:crb-arc-metricsdc-reader
#For example: kubectl delete clusterrolebinding arc:crb-arc-metricsdc-reader
```

### <a name="optionally-delete-the-azure-arc-data-controller-namespace"></a>Löschen Sie optional den Namespace des Azure Arc Datacontrollers.


```console
kubectl delete ns <nameSpecifiedDuringCreation>
# for example: kubectl delete ns arc
```

## <a name="next-steps"></a>Nächste Schritte

[Was sind Azure Arc-fähige Datendienste?](overview.md)
