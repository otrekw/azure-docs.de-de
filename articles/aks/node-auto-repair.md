---
title: Automatisches Reparieren von AKS-Knoten (Azure Kubernetes Service)
description: Erfahren Sie mehr über die Funktion zur automatischen Reparatur von Knoten und darüber, wie AKS fehlerhafte Workerknoten korrigiert.
services: container-service
ms.topic: conceptual
ms.date: 08/24/2020
ms.openlocfilehash: 781a1ffebb40b0cce9f18699d308db90633e8626
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "89490104"
---
# <a name="azure-kubernetes-service-aks-node-auto-repair"></a>Automatisches Reparieren von AKS-Knoten (Azure Kubernetes Service)

AKS überprüft den Integritätszustand von Workerknoten kontinuierlich und führt eine automatische Reparatur der Knoten durch, wenn sie fehlerhaft sind. Dieses Dokument richtet sich an Operatoren und enthält Informationen zum Verhalten der automatischen Knotenreparatur für Windows- und Linux-Knoten. Neben AKS-Reparaturen werden von der Azure-VM-Plattform auch [Wartungsmaßnahmen für virtuelle Computer][vm-updates] durchgeführt, auf denen Probleme auftreten. AKS und virtuelle Azure-Computer arbeiten zusammen, um Dienstunterbrechungen für Cluster zu minimieren.

## <a name="how-aks-checks-for-unhealthy-nodes"></a>Überprüfung auf fehlerhafte Knoten durch AKS

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

Wenn ein Knoten gemäß den obigen Regeln fehlerhaft ist und zehn aufeinanderfolgende Minuten lang fehlerhaft bleibt, werden folgende Aktionen ausgeführt:

1. Neustarten des Knotens
1. Durchführen eines Reimagings, falls der Neustart nicht erfolgreich war
1. Erstellen eines neuen Knotens, und Durchführen eines Reimagings, falls das Reimaging nicht erfolgreich war

Sollte keine der Aktionen erfolgreich sein, werden von AKS-Technikern weitere Maßnahmen untersucht. Sollten bei einer Integritätsüberprüfung mehrere Knoten fehlerhaft sein, werden die Knoten jeweils einzeln repariert, bevor mit der nächsten Reparatur begonnen wird.

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie [Verfügbarkeitszonen][availability-zones], um die Hochverfügbarkeit Ihrer AKS-Clusterworkloads zu erhöhen.

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[availability-zones]: ./availability-zones.md
[vm-updates]: ../virtual-machines/maintenance-and-updates.md
