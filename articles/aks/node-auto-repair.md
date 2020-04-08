---
title: Automatisches Reparieren von AKS-Knoten (Azure Kubernetes Service)
description: Erfahren Sie mehr über die Funktion zur automatischen Reparatur von Knoten und darüber, wie AKS fehlerhafte Workerknoten korrigiert.
services: container-service
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 9bf9df69a0a6bfa4d9f4029278d2a146811980c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80284839"
---
# <a name="azure-kubernetes-service-aks-node-auto-repair"></a>Automatisches Reparieren von AKS-Knoten (Azure Kubernetes Service)

AKS überprüft den Integritätszustand von Workerknoten kontinuierlich und führt eine automatische Reparatur der Knoten durch, wenn sie fehlerhaft sind. In dieser Dokumentation wird beschrieben, wie Azure Kubernetes Service (AKS) Workerknoten überwacht und fehlerhafte Workerknoten repariert.  Mit dieser Dokumentation sollen AKS-Bediener über das Verhalten der Knotenreparaturfunktionen informiert werden. Es ist auch wichtig zu beachten, dass die Azure-Plattform [die Wartung auf den virtuellen Computern durchführt][vm-updates], auf denen diese Probleme auftreten. AKS und Azure arbeiten zusammen, um Dienstunterbrechungen bei Ihren Clustern zu minimieren.

> [!Important]
> Die Funktion zur automatischen Knotenreparatur wird derzeit für Windows Server-Knotenpools nicht unterstützt.

## <a name="how-aks-checks-for-unhealthy-nodes"></a>Überprüfung auf fehlerhafte Knoten durch AKS

> [!Note]
> AKS führt Reparaturaktionen für Knoten mit dem Benutzerkonto **aks-remediator** durch.

Dabei nutzt AKS Regeln, um zu ermitteln, ob ein Knoten einen fehlerhaften Zustand aufweist und repariert werden muss. Anhand der folgenden Regeln bestimmt AKS, ob eine automatische Reparatur erforderlich ist.

* Der Knoten meldet bei aufeinanderfolgenden Überprüfungen innerhalb eines Zeitraums von 10 Minuten den Status **NotReady**.
* Der Knoten meldet innerhalb von 10 Minuten keinen Status.

Sie können den Integritätszustand Ihrer Knoten mit kubectl manuell überprüfen. 

```
kubectl get nodes
```

## <a name="how-automatic-repair-works"></a>Funktionsweise der automatischen Reparatur

> [!Note]
> AKS führt Reparaturaktionen für Knoten mit dem Benutzerkonto **aks-remediator** durch.

Dieses Verhalten gilt für **Virtual Machine Scale Sets**.  Die automatische Reparatur umfasst mehrere Schritte zum Reparieren eines fehlerhaften Knotens.  Wenn ein Knoten als fehlerhaft eingestuft wird, unternimmt AKS mehrere Schritte zur Wartung.  Die Schritte werden in dieser Reihenfolge ausgeführt:

1. Wenn die Containerruntime 10 Minuten lang nicht mehr reagiert, werden die fehlerhaften Runtimedienste auf dem Knoten neu gestartet.
2. Wenn der Knoten nicht innerhalb von 10 Minuten bereit ist, wird der Knoten neu gestartet.
3. Wenn der Knoten nicht innerhalb von 30 Minuten bereit ist, wird ein neues Image des Knotens erstellt.

> [!Note]
> Wenn mehrere Knoten fehlerhaft sind, werden sie einzeln repariert.

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie [Verfügbarkeitszonen][availability-zones], um die Hochverfügbarkeit Ihrer AKS-Clusterworkloads zu erhöhen.

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[availability-zones]: ./availability-zones.md
[vm-updates]: ../virtual-machines/maintenance-and-updates.md
