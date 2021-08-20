---
title: Automatisches Reparieren von AKS-Knoten (Azure Kubernetes Service)
description: Erfahren Sie mehr über die Funktion zur automatischen Reparatur von Knoten und darüber, wie AKS fehlerhafte Workerknoten korrigiert.
services: container-service
ms.topic: conceptual
ms.date: 03/11/2021
ms.openlocfilehash: 6ca726445e7593898609c39e0a503405852098fc
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355911"
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

Sollte die automatische Reparatur nicht erfolgreich sein, werden von AKS-Technikern alternative Abhilfemaßnahmen geprüft. 

Sind bei einer Integritätsüberprüfung mehrere Knoten fehlerhaft, werden die Knoten einzeln nacheinander repariert.


## <a name="limitations"></a>Einschränkungen

In vielen Fällen kann AKS ermitteln, ob ein Knoten fehlerhaft ist, und das Problem zu beheben versuchen. Es gibt jedoch Fälle, in denen AKS das Problem entweder nicht beheben kann oder nicht erkennt, dass ein Problem vorliegt. AKS kann beispielsweise keine Probleme erkennen, wenn aufgrund eines Fehlers in der Netzwerkkonfiguration kein Knotenstatus gemeldet wird.

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie [Verfügbarkeitszonen][availability-zones], um die Hochverfügbarkeit Ihrer AKS-Clusterworkloads zu erhöhen.

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[availability-zones]: ./availability-zones.md
[vm-updates]: ../virtual-machines/maintenance-and-updates.md
