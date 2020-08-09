---
title: Automatisches Reparieren von AKS-Knoten (Azure Kubernetes Service)
description: Erfahren Sie mehr über die Funktion zur automatischen Reparatur von Knoten und darüber, wie AKS fehlerhafte Workerknoten korrigiert.
services: container-service
ms.topic: conceptual
ms.date: 06/02/2020
ms.openlocfilehash: 7fcb7b380f3694aaf34328019c3e09f5157c9e64
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2020
ms.locfileid: "87542041"
---
# <a name="azure-kubernetes-service-aks-node-auto-repair"></a>Automatisches Reparieren von AKS-Knoten (Azure Kubernetes Service)

AKS überprüft den Integritätszustand von Workerknoten kontinuierlich und führt eine automatische Reparatur der Knoten durch, wenn sie fehlerhaft sind. Dieses Dokument richtet sich an Operatoren und enthält Informationen zum Verhalten der automatischen Knotenreparatur. Neben AKS-Reparaturen werden von der Azure-VM-Plattform auch [Wartungsmaßnahmen für virtuelle Computer][vm-updates] durchgeführt, auf denen Probleme auftreten. AKS und virtuelle Azure-Computer arbeiten zusammen, um Dienstunterbrechungen für Cluster zu minimieren.

## <a name="limitations"></a>Einschränkungen

* Windows-Knotenpools werden heute nicht unterstützt.

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

Wenn ein Knoten auf Grundlage der obigen Regeln als fehlerhaft eingestuft wird und 10 aufeinanderfolgende Minuten lang fehlerhaft bleibt, startet AKS den Knoten neu. Bei Knoten, die nach dem ersten Reparaturvorgang weiterhin fehlerhaft sind, werden von AKS-Technikern weitere Reparaturmöglichkeiten untersucht.
  
Sollten bei einer Integritätsüberprüfung mehrere Knoten fehlerhaft sein, werden die Knoten jeweils einzeln repariert, bevor mit der nächsten Reparatur begonnen wird.

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie [Verfügbarkeitszonen][availability-zones], um die Hochverfügbarkeit Ihrer AKS-Clusterworkloads zu erhöhen.

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[availability-zones]: ./availability-zones.md
[vm-updates]: ../virtual-machines/maintenance-and-updates.md
