---
title: Automatisches Reparieren von AKS-Knoten (Azure Kubernetes Service)
description: Erfahren Sie mehr über die Funktion zur automatischen Reparatur von Knoten und darüber, wie AKS fehlerhafte Workerknoten korrigiert.
services: container-service
ms.topic: conceptual
ms.date: 03/11/2021
ms.openlocfilehash: 341aef394a3784edbc0acd91dad396c9794da3d0
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107105203"
---
# <a name="azure-kubernetes-service-aks-node-auto-repair"></a>Automatisches Reparieren von AKS-Knoten (Azure Kubernetes Service)

AKS überwacht den Integritätszustand von Workerknoten kontinuierlich und führt im Falle eines fehlerhaften Zustands automatisch eine Knotenreparatur durch. Von der Azure-VM-Plattform werden [Wartungsmaßnahmen für virtuelle Computer][vm-updates] durchgeführt, auf denen Probleme auftreten. 

AKS und virtuelle Azure-Computer arbeiten zusammen, um Dienstunterbrechungen für Cluster zu minimieren.

Dieses Dokument enthält Informationen zum Verhalten der automatischen Knotenreparatur für Windows- und Linux-Knoten. 

## <a name="how-aks-checks-for-unhealthy-nodes"></a>Überprüfung auf fehlerhafte Knoten durch AKS

Von AKS wird anhand der folgenden Regeln ermittelt, ob ein Knoten fehlerhaft ist und repariert werden muss: 
* Der Knoten meldet bei aufeinanderfolgenden Überprüfungen innerhalb eines Zeitraums von zehn Minuten den Status **NotReady**.
* Der Knoten meldet innerhalb von zehn Minuten keinen Status.

Sie können den Integritätszustand Ihrer Knoten mit kubectl manuell überprüfen.

```
kubectl get nodes
```

## <a name="how-automatic-repair-works"></a>Funktionsweise der automatischen Reparatur

> [!Note]
> AKS initiiert Reparaturvorgänge mit dem Benutzerkonto **aks-remediator**.

Wenn von AKS ein fehlerhafter Knoten identifiziert wird und dieser Knoten zehn Minuten lang fehlerhaft ist, werden folgende Aktionen ausgeführt:

1. Neustarten des Knotens
1. Durchführen eines Reimagings, falls der Neustart nicht erfolgreich war
1. Erstellen eines neuen Knotens, und Durchführen eines Reimagings, falls das Reimaging nicht erfolgreich war

Sollte die automatische Reparatur nicht erfolgreich sein, werden von AKS-Technikern alternative Abhilfemaßnahmen geprüft. 

Sind bei einer Integritätsüberprüfung mehrere Knoten fehlerhaft, werden die Knoten einzeln nacheinander repariert.

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie [Verfügbarkeitszonen][availability-zones], um die Hochverfügbarkeit Ihrer AKS-Clusterworkloads zu erhöhen.

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[availability-zones]: ./availability-zones.md
[vm-updates]: ../virtual-machines/maintenance-and-updates.md
