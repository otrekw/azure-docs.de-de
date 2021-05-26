---
title: Best Practices für Operator – Grundlegende Schedulerfunktionen in Azure Kubernetes Service (AKS)
description: Lernen Sie die Best Practices des Clusteroperators für die Verwendung grundlegender Schedulerfunktionen wie Ressourcenkontingente und Budgets für die Unterbrechung von Pods im Azure Kubernetes Service (AKS) kennen.
services: container-service
ms.topic: conceptual
ms.date: 03/09/2021
ms.openlocfilehash: 034b7e18d3114804f846d77aa4feb001492c8883
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110467062"
---
# <a name="best-practices-for-basic-scheduler-features-in-azure-kubernetes-service-aks"></a>Best Practices für grundlegende Schedulerfunktionen in Azure Kubernetes Service (AKS)

Beim Verwalten von Clustern in Azure Kubernetes Service (AKS) müssen Sie oft Teams oder Workloads isolieren. Mit dem Kubernetes-Scheduler können Sie die Verteilung von Computeressourcen steuern oder die Auswirkungen von Wartungsereignissen begrenzen.

Dieser Artikel zu Best Practices konzentriert sich auf grundlegende Kubernetes-Planungsfunktionen für Clusteroperatoren. In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Verwenden von Ressourcenkontingenten, um eine feste Menge an Ressourcen für Teams oder Workloads bereitzustellen
> * Beschränken der Auswirkungen der geplanten Wartung mithilfe von Budgets für die Unterbrechung von Pods

## <a name="enforce-resource-quotas"></a>Durchsetzen von Ressourcenkontingenten

> **Best Practices-Leitfaden** 
> 
> Planen Sie Ressourcenkontingente auf Namespaceebene, und wenden Sie sie an. Wenn Pods keine Ressourcenanforderungen und -grenzwerte definieren, lehnen Sie die Bereitstellung ab. Überwachen Sie die Ressourcennutzung, und passen Sie Kontingente nach Bedarf an.

Ressourcenanforderungen und -grenzwerte werden in der Podspezifikation hinterlegt. Grenzwerte werden vom Kubernetes-Scheduler zum Zeitpunkt der Bereitstellung verwendet, um einen verfügbaren Knoten im Cluster zu finden. Grenzwerte und Anforderungen gelten jeweils für den einzelnen Pod. Weitere Informationen zum Definieren dieser Werte finden Sie unter [Definieren von Ressourcenanforderungen und -grenzwerten][resource-limits].

Um eine Möglichkeit zu bieten, Ressourcen innerhalb eines Entwicklungsteams oder Projekts zu reservieren und zu begrenzen, sollten Sie *Ressourcenkontingente* verwenden. Diese Kontingente werden in einem Namespace definiert und können verwendet werden, um Kontingente auf der folgenden Grundlage festzulegen:

* **Computeressourcen**, z.B. CPU und Arbeitsspeicher oder GPUs.
* **Speicherressourcen**, einschließlich der Gesamtanzahl der Volumes oder des Speicherplatzes für eine angegebene Speicherklasse.
* **Objektanzahl**, z.B die maximale Anzahl von Geheimnissen, Diensten oder der Aufträge, die erstellt werden können.

Kubernetes weist nicht zu viele Ressourcen zu. Sobald die kumulierte Summe der Ressourcenanforderungen das zugewiesene Kontingent überschreitet, können weitere Bereitstellungen nicht erfolgreich ausgeführt werden.

Wenn Sie Ressourcenkontingente definieren, müssen alle im Namespace erstellten Pods in ihren Podspezifikationen Grenzwerte oder Anforderungen enthalten. Wenn diese Werte nicht angeben werden, können Sie die Bereitstellung ablehnen. Stattdessen können Sie [Standardanforderungen und Grenzwerte für einen Namespace definieren][configure-default-quotas].

Das folgende Beispiel eines YAML-Manifests mit dem Namen *dev-app-team-quotas.yaml* legt einen festen Grenzwert von insgesamt *10* CPUs, *20Gi* Arbeitsspeicher und *10* Pods fest:

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: dev-app-team
spec:
  hard:
    cpu: "10"
    memory: 20Gi
    pods: "10"
```

Dieses Ressourcenkontingent kann durch das Angeben des Namespaces angewendet werden, z.B. *dev-apps*:

```console
kubectl apply -f dev-app-team-quotas.yaml --namespace dev-apps
```

Arbeiten Sie mit Ihren Anwendungsentwicklern und -besitzern zusammen, um deren Anforderungen zu verstehen und die entsprechenden Ressourcenkontingente anzuwenden.

Weitere Informationen zu verfügbaren Ressourcenobjekten, Bereichen und Prioritäten finden Sie unter [Ressourcenkontingente in Kubernetes][k8s-resource-quotas].

## <a name="plan-for-availability-using-pod-disruption-budgets"></a>Planen der Verfügbarkeit mit Budgets für die Unterbrechung von Pods

> **Best Practices-Leitfaden** 
>
> Um die Verfügbarkeit von Anwendungen aufrechtzuerhalten, definieren Sie Budgets für die Unterbrechung von Pods (Pod Disruption Budgets, PDBs), um sicherzustellen, dass eine Mindestanzahl von Pods im Cluster verfügbar ist.

Es gibt zwei unterbrechende Ereignisse, durch die Pods entfernt werden:

### <a name="involuntary-disruptions"></a>Unfreiwillige Unterbrechungen

*Unfreiwillige Unterbrechungen* sind Ereignisse, die außerhalb der typischen Kontrolle des Clusteroperators oder Anwendungsbesitzers liegen. Einschließen:
* Hardwarefehler auf dem physischen Computer
* Kernelstörung
* Löschen einer Knoten-VM

Die Auswirkungen unfreiwilliger Unterbrechungen können auf folgende Weise verringert werden:
* Verwenden mehrerer Replikate Ihrer Pods in einer Bereitstellung 
* Ausführen mehrerer Knoten im AKS-Cluster 

### <a name="voluntary-disruptions"></a>Freiwillige Unterbrechungen

*Freiwillige Unterbrechungen* sind Ereignisse, die vom Clusteroperator oder Anwendungsbesitzer angefordert werden. Einschließen:
* Clusterupgrades
* Aktualisierte Bereitstellungsvorlage
* Versehentliches Löschen eines Pods

Kubernetes bietet *Budgets für die Unterbrechung von Pods* für freiwillige Unterbrechungen, mit denen Sie planen können, wie Bereitstellungen oder Replikatgruppen bei einem freiwilligen Unterbrechungsereignis reagieren. Mithilfe von Budgets für die Unterbrechung von Pods können Clusteroperatoren eine minimal verfügbare oder maximal nicht verfügbare Ressourcenanzahl festlegen. 

Wenn Sie ein Upgrade für einen Cluster durchführen oder eine Bereitstellungsvorlage aktualisieren, werden vom Kubernetes-Scheduler zusätzliche Pods auf anderen Knoten geplant, bevor freiwillige Unterbrechungsereignisse fortgesetzt werden können. Der Scheduler wartet mit dem Neustart eines Knotens, bis die definierte Anzahl von Pods auf anderen Knoten im Cluster erfolgreich geplant ist.

Schauen wir uns ein Beispiel für eine Replikatgruppe mit fünf Pods an, auf denen NGINX ausgeführt wird. Den Pods in der Replikatgruppe ist die Bezeichnung `app: nginx-frontend` zugewiesen. Während eines freiwilligen Unterbrechungsereignisses, wie z.B. eines Clusterupgrades, möchten Sie sicherstellen, dass mindestens drei Pods weiterhin laufen. Das folgende YAML-Manifest für ein *PodDisruptionBudget*-Objekt definiert diese Anforderungen:

```yaml
apiVersion: policy/v1beta1
kind: PodDisruptionBudget
metadata:
   name: nginx-pdb
spec:
   minAvailable: 3
   selector:
    matchLabels:
      app: nginx-frontend
```

Sie können auch einen Prozentsatz definieren, wie z.B. *60 %*, mit dem Sie automatisch die Erhöhung der Anzahl der Pods durch die Replikatgruppe kompensieren können.

Sie können eine maximale Anzahl von nicht verfügbaren Instanzen in einer Replikatgruppe definieren. Darüber hinaus kann ein Prozentsatz für die maximale Anzahl der nicht verfügbaren Pods definiert werden. Das folgende YAML-Manifest für das Budget für die Unterbrechung von Pods definiert, dass nicht mehr als zwei Pods in der Replikatgruppe nicht verfügbar sein dürfen:

```yaml
apiVersion: policy/v1beta1
kind: PodDisruptionBudget
metadata:
   name: nginx-pdb
spec:
   maxUnavailable: 2
   selector:
    matchLabels:
      app: nginx-frontend
```

Sobald Sie Ihr Budget definiert haben, können Sie es genauso wie bei jedem anderen Kubernetes-Objekt in Ihrem AKS-Cluster erstellen:

```console
kubectl apply -f nginx-pdb.yaml
```

Arbeiten Sie mit Ihren Anwendungsentwicklern und -besitzern zusammen, um deren Anforderungen zu verstehen und das entsprechende Budget für die Unterbrechung von Pods anzuwenden.

Weitere Informationen zur Verwendung von Budgets für die Unterbrechung von Pods finden Sie unter [Spezifizieren von Budgets für die Unterbrechung für Ihre Anwendung][k8s-pdbs].

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel konzentriert sich auf grundlegende Funktionen des Kubernetes-Schedulers. Weitere Informationen zu Clustervorgängen in AKS finden Sie in den folgenden Best Practices:

* [Best Practices für Clusterisolierung in Azure Kubernetes Service (AKS)][aks-best-practices-cluster-isolation]
* [Best Practices für erweiterte Schedulerfunktionen in Azure Kubernetes Service (AKS)][aks-best-practices-advanced-scheduler]
* [Authentifizierung und Autorisierung][aks-best-practices-identity]

<!-- EXTERNAL LINKS -->
[k8s-resource-quotas]: https://kubernetes.io/docs/concepts/policy/resource-quotas/
[configure-default-quotas]: https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/
[k8s-pdbs]: https://kubernetes.io/docs/tasks/run-application/configure-pdb/

<!-- INTERNAL LINKS -->
[resource-limits]: developer-best-practices-resource-management.md#define-pod-resource-requests-and-limits
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md
[aks-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[aks-best-practices-identity]: operator-best-practices-identity.md
[k8s-node-selector]: concepts-clusters-workloads.md#node-selectors
