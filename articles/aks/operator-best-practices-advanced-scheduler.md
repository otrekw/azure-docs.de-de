---
title: Bewährte Methoden für Schedulerfunktionen
titleSuffix: Azure Kubernetes Service
description: Lernen Sie die Best Practices des Clusteroperators für die Verwendung erweiterter Schedulerfunktionen kennen, wie z.B. Taints und Toleranzen, Knotenselektoren und Affinität oder Pod-interne Affinität und Antiaffinität in Azure Kubernetes Service (AKS) kennen.
services: container-service
ms.topic: conceptual
ms.date: 03/09/2021
ms.openlocfilehash: 27b32d7d10b691ed806e4d7aa31a095630d2bfc9
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107103622"
---
# <a name="best-practices-for-advanced-scheduler-features-in-azure-kubernetes-service-aks"></a>Best Practices für erweiterte Schedulerfunktionen in Azure Kubernetes Service (AKS)

Beim Verwalten von Clustern in Azure Kubernetes Service (AKS) müssen Sie oft Teams oder Workloads isolieren. Mit erweiterten Funktionen, die vom Kubernetes-Scheduler bereitgestellt werden, können Sie Folgendes steuern:
* Welche Pods auf bestimmten Knoten geplant werden können.
* Wie Anwendungen mit mehreren Pods angemessen über den Cluster verteilt werden können. 

Dieser Artikel zu Best Practices konzentriert sich auf erweiterten Kubernetes-Planungsfunktionen für Clusteroperatoren. In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Verwenden von Taints und Toleranzen, um einzuschränken welche Pods auf Knoten geplant werden können
> * Festlegen, dass Pods auf bestimmten Knoten ausgeführt werden, mit Knotenselektoren oder Knotenaffinität
> * Trennen oder Gruppieren von Pods mit Pod-interner Affinität oder Antiaffinität

## <a name="provide-dedicated-nodes-using-taints-and-tolerations"></a>Bereitstellen dedizierter Knoten mit Taints und Toleranzen

> **Best Practices-Leitfaden:** 
>
> Beschränken Sie den Zugriff für ressourcenintensive Anwendungen, z. B. Eingangscontroller, auf bestimmte Knoten. Halten Sie Knotenressourcen für Workloads verfügbar, die sie benötigen, und erlauben Sie keine Planung anderer Workloads auf den Knoten.

Wenn Sie Ihren AKS-Cluster erstellen, können Sie Knoten mit GPU-Unterstützung oder einer großen Anzahl leistungsstarker CPUs bereitstellen. Sie können diese Knoten für große Datenverarbeitungsworkloads wie Machine Learning (ML) oder Künstliche Intelligenz (KI) verwenden. 

Da diese Knotenressourcen-Hardware in der Regel teuer in der Bereitstellung ist, begrenzen Sie die Workloads, die auf diesen Knoten geplant werden können. Stattdessen verwenden Sie einige Knoten im Cluster zum Ausführen von Eingangsdiensten und Verhindern anderer Workloads.

Diese Unterstützung für verschiedene Knoten erhalten Sie durch die Verwendung von mehrerer Knotenpools. Ein AKS-Cluster bietet einen oder mehrere Knotenpools.

Der Kubernetes-Scheduler verwendet Taints und Toleranzen, um einzuschränken, welche Workloads auf Knoten ausgeführt werden können.

* Wenden Sie ein **Taint** auf einen Knoten an, um anzugeben, dass nur bestimmte Pods auf ihm geplant werden können.
* Dann wenden Sie eine **Toleranz** auf einen Pod an, sodass dieser den Taint des Knotens *tolerieren* kann.

Wenn Sie einen Pod in einem AKS-Cluster bereitstellen, plant Kubernetes nur Pods auf Knoten, deren Taint der Toleranz entspricht. Angenommen, Sie verfügen über einen Knotenpool in Ihrem AKS-Cluster für Knoten mit GPU-Unterstützung. Sie definieren einen Namen, z.B. *gpu* und dann einen Wert für die Planung. Wenn Sie diesen Wert auf *NoSchedule* festlegen, kann der Kubernetes-Scheduler keine Pods mit nicht definierter Toleranz auf dem Knoten planen.

```console
kubectl taint node aks-nodepool1 sku=gpu:NoSchedule
```

Wenn für Knoten ein Taint angewendet wird, definieren Sie in den Podspezifikationen eine Toleranz, die eine Planung auf den Knoten ermöglicht. Im folgenden Beispiel werden `sku: gpu` und `effect: NoSchedule` so definiert, dass der im vorherigen Schritt für den Knoten angewendete Taint toleriert wird.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: tf-mnist
spec:
  containers:
  - name: tf-mnist
    image: mcr.microsoft.com/azuredocs/samples-tf-mnist-demo:gpu
    resources:
      requests:
        cpu: 0.5
        memory: 2Gi
      limits:
        cpu: 4.0
        memory: 16Gi
  tolerations:
  - key: "sku"
    operator: "Equal"
    value: "gpu"
    effect: "NoSchedule"
```

Wenn dieser Pod mit `kubectl apply -f gpu-toleration.yaml` bereitgestellt wird, kann Kubernetes den Pod erfolgreich auf den Knoten mit dem angewandten Taint planen. Mit dieser logischen Isolation können Sie den Zugriff auf Ressourcen innerhalb eines Clusters steuern.

Wenn Sie Taints anwenden, arbeiten Sie mit Ihren Anwendungsentwicklern und -besitzern zusammen, damit sie die erforderlichen Toleranzen in ihren Bereitstellungen definieren können.

Weitere Informationen zur Verwendung mehrerer Knotenpools in AKS finden Sie unter [Erstellen und Verwalten mehrerer Knotenpools für einen Cluster in AKS][use-multiple-node-pools].

### <a name="behavior-of-taints-and-tolerations-in-aks"></a>Verhalten von Taints und Toleranzen in AKS

Wenn Sie ein Upgrade für einen Knotenpool in AKS durchführen, folgen Taints und Toleranzen einem festen Muster, da sie für neue Knoten angewendet werden:

#### <a name="default-clusters-that-use-vm-scale-sets"></a>Standardcluster, die VM-Skalierungsgruppen verwenden
Sie können über die AKS-API [einen Knotenpool mit Taints versehen][taint-node-pool], damit neu aufskalierte Knoten die von der API angegebenen Knotentaints erhalten.

Gehen Sie von folgender Annahme aus:
1. Sie beginnen mit einem Cluster mit zwei Knoten: *node1* und *node2*. 
1. Sie führen ein Upgrade für den Knotenpool durch.
1. Zwei zusätzliche Knoten werden erstellt: *node3* und *node4*. 
1. Die Taints werden entsprechend übergeben.
1. Die ursprünglichen *node1* und *node2* werden gelöscht.

#### <a name="clusters-without-vm-scale-set-support"></a>Cluster ohne Unterstützung für VM-Skalierungsgruppen

Gehen Sie noch einmal von folgender Annahme aus:
1. Sie haben einen Cluster mit zwei Knoten: *node1* und *node2*. 
1. Dann führen Sie ein Upgrade für den Knotenpool durch.
1. Ein zusätzlicher Knoten wird erstellt: *node3*.
1. Die Taints aus *node1* werden auf *node3* angewendet.
1. *node1* wird gelöscht.
1. Ein neuer *node1* wird erstellt, um den ursprünglichen *node1* zu ersetzen.
1. Die Taints aus *node2* werden auf den neuen *node1* angewendet. 
1. *node2* wird gelöscht.

Zusammengefasst wird *node1* zu *node3*, und *node2* wird zum neuen *node1*.

Wenn Sie einen Knotenpool in AKS skalieren, werden Taints und Toleranzen absichtlich nicht übertragen.

## <a name="control-pod-scheduling-using-node-selectors-and-affinity"></a>Steuern der Podplanung mit Knotenselektoren und Affinität

> **Best Practices-Leitfaden** 
> 
> Steuern Sie die Planung von Pods auf Knoten mithilfe von Knotenelektoren, Knotenaffinität oder Pod-interner Affinität. Diese Einstellungen ermöglichen es dem Kubernetes-Scheduler, Workloads logisch zu isolieren, z.B. durch Hardware im Knoten.

Mit Taints und Toleranzen werden Ressourcen durch eine harte Trennung logisch isoliert. Wenn der Pod den Taint eines Knotens nicht toleriert, wird er nicht auf dem Knoten geplant. 

Alternativ können Sie Knotenselektoren verwenden. Beispielsweise bezeichnen Sie Knoten, um einen lokal angefügten SSD-Speicher oder eine große Menge an Speicher anzuzeigen, und definieren dann in der Podspezifikation einen Knotenselektor. Kubernetes plant diese Pods auf einem übereinstimmenden Knoten. 

Im Gegensatz zu Toleranzen können Pods ohne einen übereinstimmenden Knotenselektor dennoch auf bezeichneten Knoten geplant werden. Dieses Verhalten ermöglicht es, ungenutzte Ressourcen auf den Knoten zu verwenden, priorisiert aber Pods, die den passenden Knotenselektor definieren.

Sehen wir uns ein Beispiel für Knoten mit einer hohe Menge an Arbeitsspeicher an. Diese Knoten priorisieren Pods, für die viel Speicher erforderlich ist. Um sicherzustellen, dass die Ressourcen nicht ungenutzt bleiben, können sie auch andere Pods ausführen.

```console
kubectl label node aks-nodepool1 hardware=highmem
```

Eine Podspezifikation fügt dann die Eigenschaft `nodeSelector` hinzu, um einen Knotenselektor zu definieren, der mit der auf einem Knoten festgelegten Bezeichnung übereinstimmt:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: tf-mnist
spec:
  containers:
  - name: tf-mnist
    image: mcr.microsoft.com/azuredocs/samples-tf-mnist-demo:gpu
    resources:
      requests:
        cpu: 0.5
        memory: 2Gi
      limits:
        cpu: 4.0
        memory: 16Gi
  nodeSelector:
      hardware: highmem
```

Wenn Sie diese Scheduleroptionen verwenden, arbeiten Sie mit Ihren Anwendungsentwicklern und -besitzern zusammen, damit sie ihre Podspezifikationen korrekt definieren können.

Weitere Informationen zur Verwendung von Knotenselektoren finden Sie unter [Zuweisen von Pods zu Knoten][k8s-node-selector].

### <a name="node-affinity"></a>Knotenaffinität

Ein Knotenselektor ist eine grundlegende Methode zum Zuweisen von Pods zu einem bestimmten Knoten. Die *Knotenaffinität* bietet mehr Flexibilität. Hiermit können Sie definieren, was geschieht, wenn für den Pod kein übereinstimmender Knoten vorhanden ist. Sie können: 
* *Anfordern*, dass der Kubernetes-Scheduler einem Pod mit einem bezeichneten Host entspricht. Oder:
* *Eine Übereinstimmung bevorzugen*, aber zulassen, dass der Pod auf einem anderen Host geplant wird, wenn keine Übereinstimmung vorhanden ist.

Im folgenden Beispiel wird die Knotenaffinität auf *RequiredDuringSchedulingIgnoredDuringExecution* festgelegt. Aufgrund dieser Affinität muss der Kubernetes-Scheduler einen Knoten mit einer übereinstimmenden Bezeichnung verwenden. Wenn kein Knoten verfügbar ist, muss der Pod warten, bis die Planung fortgesetzt wird. Damit der Pod auf einem anderen Knoten geplant werden kann, können Sie den Wert stattdessen auf ***preferred** DuringSchedulingIgnoreDuringExecution* festlegen:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: tf-mnist
spec:
  containers:
  - name: tf-mnist
    image: mcr.microsoft.com/azuredocs/samples-tf-mnist-demo:gpu
    resources:
      requests:
        cpu: 0.5
        memory: 2Gi
      limits:
        cpu: 4.0
        memory: 16Gi
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: hardware
            operator: In
            values: highmem
```

Der *IgnoredDuringExecution*-Teil der Einstellung gibt an, dass der Pod nicht vom Knoten entfernt werden soll, wenn sich die Bezeichnung des Knotens ändert. Der Kubernetes-Scheduler verwendet nur die aktualisierten Knotenbezeichnungen für neue Pods, die geplant werden, nicht für bereits geplante Pods auf den Knoten.

Weitere Informationen finden Sie unter [Affinität und Antiaffinität][k8s-affinity].

### <a name="inter-pod-affinity-and-anti-affinity"></a>Pod-interne Affinität und Antiaffinität

Ein letzter Ansatz für den Kubernetes-Scheduler zur logischen Isolation von Workloads ist die Verwendung von Pod-interner Affinität oder Antiaffinität. Diese Einstellungen definieren, dass Pods auf einem Knoten mit einem übereinstimmenden Pod entweder *geplant* oder *nicht geplant* werden sollten. Standardmäßig versucht der Kubernetes-Scheduler mehrere Pods knotenübergreifend in einer Replikatgruppe zu planen. Sie können für dieses Verhalten spezifischer Regeln definieren.

Angenommen, Sie haben eine Webanwendung, die auch eine Azure Cache for Redis-Instanz verwendet. 
1. Sie verwenden Regeln für die Pod-Antiaffinität, um anzufordern, dass der Kubernetes-Scheduler Replikate über Knoten verteilt. 
1. Sie verwenden Affinitätsregeln, um sicherzustellen, dass jede Komponente der Webanwendung auf dem gleichen Host wie ein entsprechender Cache geplant ist. 

Die Verteilung der Pods auf die Knoten sieht beispielsweise wie folgt aus:

| **Knoten 1** | **Knoten 2** | **Knoten 3** |
|------------|------------|------------|
| webapp-1   | webapp-2   | webapp-3   |
| cache-1    | cache-2    | cache-3    |

Pod-interne Affinität und Antiaffinität ermöglichen eine komplexere Bereitstellung als Knotenselektoren oder Knotenaffinität. Mit der Bereitstellung nehmen Sie eine logische Isolierung von Ressourcen vor und steuern, wie Kubernetes Pods auf Knoten plant. 

Ein vollständiges Beispiel dieser Webanwendung mit Azure Cache for Redis-Beispiel finden Sie unter [Zusammenstellen von Pods auf demselben Knoten][k8s-pod-affinity].

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel konzentriert sich auf erweiterte Funktionen des Kubernetes-Schedulers. Weitere Informationen zu Clustervorgängen in AKS finden Sie in den folgenden Best Practices:

* [Best Practices für Clusterisolierung in Azure Kubernetes Service (AKS)][aks-best-practices-scheduler]
* [Best Practices für grundlegende Schedulerfunktionen in Azure Kubernetes Service (AKS)][aks-best-practices-scheduler]
* [Authentifizierung und Autorisierung][aks-best-practices-identity]

<!-- EXTERNAL LINKS -->
[k8s-taints-tolerations]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[k8s-node-selector]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[k8s-affinity]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity
[k8s-pod-affinity]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#always-co-located-in-the-same-node

<!-- INTERNAL LINKS -->
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md
[aks-best-practices-identity]: operator-best-practices-identity.md
[use-multiple-node-pools]: use-multiple-node-pools.md
[taint-node-pool]: use-multiple-node-pools.md#specify-a-taint-label-or-tag-for-a-node-pool
