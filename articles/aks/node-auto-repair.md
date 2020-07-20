---
title: Automatisches Reparieren von AKS-Knoten (Azure Kubernetes Service)
description: Erfahren Sie mehr über die Funktion zur automatischen Reparatur von Knoten und darüber, wie AKS fehlerhafte Workerknoten korrigiert.
services: container-service
ms.topic: conceptual
ms.date: 06/02/2020
ms.openlocfilehash: 91384461567634faabaaa1dd588d6e7ec6ece60e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84735624"
---
# <a name="azure-kubernetes-service-aks-node-auto-repair"></a>Automatisches Reparieren von AKS-Knoten (Azure Kubernetes Service)

AKS überprüft den Integritätszustand von Workerknoten kontinuierlich und führt eine automatische Reparatur der Knoten durch, wenn sie fehlerhaft sind. Dieses Dokument richtet sich an Operatoren und enthält Informationen zum Verhalten der automatischen Knotenreparatur. Neben AKS-Reparaturen werden von der Azure-VM-Plattform auch [Wartungsmaßnahmen für virtuelle Computer][vm-updates] durchgeführt, auf denen Probleme auftreten. AKS und virtuelle Azure-Computer arbeiten zusammen, um Dienstunterbrechungen für Cluster zu minimieren.

> [!Important]
> Für Windows Server-Knotenpools wird die Funktion zur automatischen Knotenreparatur derzeit nicht unterstützt.

## <a name="how-aks-checks-for-unhealthy-nodes"></a>Überprüfung auf fehlerhafte Knoten durch AKS

> [!Note]
> AKS führt Reparaturaktionen für Knoten mit dem Benutzerkonto **aks-remediator** durch.

Dabei ermittelt AKS mithilfe von Regeln, ob ein Knoten fehlerhaft ist und repariert werden muss. Anhand der folgenden Regeln bestimmt AKS, ob eine automatische Reparatur erforderlich ist.

* Der Knoten meldet bei aufeinanderfolgenden Überprüfungen innerhalb eines Zeitraums von 10 Minuten den Status **NotReady**.
* Der Knoten meldet innerhalb von 10 Minuten keinen Status.

Sie können den Integritätszustand Ihrer Knoten mit kubectl manuell überprüfen. 

```
kubectl get nodes
```

## <a name="how-automatic-repair-works"></a>Funktionsweise der automatischen Reparatur

> [!Note]
> AKS initiiert Reparaturvorgänge mit dem Benutzerkonto **aks-remediator**.

Die automatische Reparatur wird standardmäßig für Cluster mit dem VM-Gruppentyp **VM-Skalierungsgruppen** unterstützt. Wird ein Knoten auf der Grundlage der obigen Regeln als fehlerhaft eingestuft, wird er von AKS neu gestartet, nachdem er sich zehn Minuten lang in einem fehlerhaften Zustand befunden hat. Bei Knoten, die nach dem ersten Reparaturvorgang weiterhin fehlerhaft sind, werden von AKS-Technikern weitere Reparaturmöglichkeiten untersucht.
  
Sollten bei einer Integritätsüberprüfung mehrere Knoten fehlerhaft sein, werden die Knoten jeweils einzeln repariert, bevor mit der nächsten Reparatur begonnen wird.

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie [Verfügbarkeitszonen][availability-zones], um die Hochverfügbarkeit Ihrer AKS-Clusterworkloads zu erhöhen.

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[availability-zones]: ./availability-zones.md
[vm-updates]: ../virtual-machines/maintenance-and-updates.md
