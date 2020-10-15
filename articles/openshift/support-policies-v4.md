---
title: Richtlinien für die Clusterunterstützung in Azure Red Hat OpenShift 4
description: In diesem Artikel erhalten Sie Informationen zu den Unterstützungsrichtlinienanforderungen für Red Hat OpenShift 4.
author: sakthi-vetrivel
ms.author: suvetriv
ms.service: container-service
ms.topic: conceptual
ms.date: 04/24/2020
ms.openlocfilehash: f496d0f38452fa7cf64bc9eef370bd0b2116cfa5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89049960"
---
# <a name="azure-red-hat-openshift-support-policy"></a>Unterstützungsrichtlinien in Azure Red Hat OpenShift

Bestimmte Konfigurationen Ihrer Cluster in Azure Red Hat OpenShift 4 können sich auf die Unterstützbarkeit Ihres Clusters auswirken. In Azure Red Hat OpenShift 4 können Clusteradministratoren Änderungen an internen Clusterkomponenten vornehmen. Es werden jedoch nicht alle Änderungen unterstützt. Die Unterstützungsrichtlinien unten fassen zusammen, welche Änderungen die Richtlinien verletzen und damit die Unterstützung durch Microsoft und Red Hat aufheben.

> [!NOTE]
> Features, die auf der OpenShift-Plattform für Container als Technologievorschau gekennzeichnet sind, werden in Azure Red Hat OpenShift nicht unterstützt.

## <a name="cluster-configuration-requirements"></a>Clusterkonfigurationsanforderungen

* Alle Clusteroperatoren in OpenShift müssen in einem verwalteten Zustand verbleiben. Die Liste der Clusteroperatoren kann zurückgegeben gegeben werden, indem `oc get clusteroperators` ausgeführt wird.
* Entfernen oder ändern Sie die Prometheus- und Alertmanager-Clusterdienste nicht.
* Entfernen Sie die Alertmanager-Dienstregeln nicht.
* Entfernen oder ändern Sie die Protokollierung im Azure Red Hat OpenShift-Dienst nicht (MDSD-Pods).
* Entfernen oder ändern Sie das Clusterpullgeheimnis „arosvc.azurecr.io“ nicht.
* Alle Cluster-VMs benötigen direkten ausgehenden Internetzugriff, und zwar mindestens auf die Endpunkte für Azure Resource Manager (ARM) und die Dienstprotokollierung (Geneva).  Es wird keine Form von HTTPS-Proxys unterstützt.
* Ändern Sie die DNS-Konfiguration des virtuellen Netzwerks des Clusters nicht. Der Azure DNS-Standardkonfliktlöser muss verwendet werden.
* Setzen Sie keines der MachineConfig-Objekte des Clusters (z. B. die Kubelet-Konfiguration) auf irgendeine Weise außer Kraft.
* Der Azure Red Hat OpenShift-Dienst greift auf Ihre Cluster über den Private Link-Dienst zu.  Ändern oder entfernen Sie den Dienstzugriff nicht.
* Nicht-RHCOS-Serverknoten werden nicht unterstützt. Sie können z. B. keinen RHEL-Serverknoten verwenden.

## <a name="supported-virtual-machine-sizes"></a>Unterstützte VM-Größen

Azure Red Hat OpenShift 4 unterstützt Workerknoteninstanzen auf den folgenden VM-Größen:

### <a name="general-purpose"></a>Allgemeiner Zweck

|Reihen|Size|vCPU|Memory: GiB|
|-|-|-|-|
|Dasv4|Standard_D4as_v4|4|16|
|Dasv4|Standard_D8as_v4|8|32|
|Dasv4|Standard_D16as_v4|16|64|
|Dasv4|Standard_D32as_v4|32|128|
|Dsv3|Standard_D4s_v3|4|16|
|Dsv3|Standard_D8s_v3|8|32|
|Dsv3|Standard_D16s_v3|16|64|
|Dsv3|Standard_D32s_v3|32|128|

### <a name="memory-optimized"></a>Arbeitsspeicheroptimiert

|Reihen|Size|vCPU|Memory: GiB|
|-|-|-|-|
|Esv3|Standard_E4s_v3|4|32|
|Esv3|Standard_E8s_v3|8|64|
|Esv3|Standard_E16s_v3|16|128|
|Esv3|Standard_E32s_v3|32|256|

### <a name="compute-optimized"></a>Computeoptimiert

|Reihen|Size|vCPU|Memory: GiB|
|-|-|-|-|
|Fsv2|Standard_F4s_v2|4|8|
|Fsv2|Standard_F8s_v2|8|16|
|Fsv2|Standard_F16s_v2|16|32|
|Fsv2|Standard_F32s_v2|32|64|

### <a name="master-nodes"></a>Masterknoten

|Reihen|Size|vCPU|Memory: GiB|
|-|-|-|-|
|Dsv3|Standard_D8s_v3|8|32|
|Dsv3|Standard_D16s_v3|16|64|
|Dsv3|Standard_D32s_v3|32|128|
