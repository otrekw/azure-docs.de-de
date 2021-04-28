---
title: Bewährte Methoden für die Clusterisolierung
titleSuffix: Azure Kubernetes Service
description: Lernen Sie die Best Practices für die Isolierung in Azure Kubernetes Service (AKS) für Clusteroperator kennen.
services: container-service
ms.topic: conceptual
ms.date: 03/09/2021
ms.openlocfilehash: e51689d33711f127f775c63c9d7fc8ad4c901604
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107105169"
---
# <a name="best-practices-for-cluster-isolation-in-azure-kubernetes-service-aks"></a>Best Practices für Clusterisolierung in Azure Kubernetes Service (AKS)

Beim Verwalten von Clustern in Azure Kubernetes Service (AKS) müssen Sie oft Teams oder Workloads isolieren. AKS bietet Flexibilität bei der Art, in der Sie mehrinstanzenfähige Cluster ausführen und Ressourcen isolieren können. Um den Nutzen Ihrer Investition in Kubernetes zu maximieren, sollten Sie zunächst die AKS-Funktionen für Mehrinstanzenfähigkeit und Isolation verstehen und implementieren.

Dieser Best Practices-Artikel konzentriert sich auf die Isolierung für Clusteroperator. In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Planen für mehrinstanzenfähige Cluster und Trennung von Ressourcen
> * Verwenden der logischen oder physischen Isolierung in Ihren AKS-Clustern

## <a name="design-clusters-for-multi-tenancy"></a>Entwerfen von Clustern für die Mehrinstanzenfähigkeit

Mit Kubernetes können Sie Teams und Workloads im selben Cluster logisch isolieren. Das Ziel ist, die geringste Anzahl von Berechtigungen bereitzustellen, die auf die von den einzelnen Teams benötigten Ressourcen beschränkt sind. Ein Kubernetes-[Namespace][k8s-namespaces] erstellt eine logische Isolationsgrenze. Zusätzliche Kubernetes-Funktionen und Überlegungen zu Isolierung und Mehrinstanzenfähigkeit umfassen die folgenden Bereiche:

### <a name="scheduling"></a>Scheduling

Bei der *Planung* werden grundlegende Funktionen verwendet, z. B. Ressourcenkontingente und Budgets für die Unterbrechung von Pods. Weitere Informationen zu diesen Features finden Sie unter [Best Practices für grundlegende Schedulerfunktionen in Azure Kubernetes Service (AKS)][aks-best-practices-scheduler].

Zu den erweiterten Schedulerfunktionen gehören:
* Taints und Toleranzen
* Knotenselektoren
* Knoten- und Podaffinität oder Antiaffinität 

Weitere Informationen zu diesen Features finden Sie unter [Best Practices für erweiterte Schedulerfunktionen in Azure Kubernetes Service (AKS)][aks-best-practices-advanced-scheduler].

### <a name="networking"></a>Netzwerk

Für das *Netzwerk* werden Netzwerkrichtlinien verwendet, um den ein- und ausgehenden Datenverkehr des Pods zu steuern.

### <a name="authentication-and-authorization"></a>Authentifizierung und Autorisierung

Zur *Authentifizierung und Autorisierung* wird Folgendes verwendet:
* Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)
* Azure Active Directory-Integration (AD)
* Podidentitäten
* Geheimnisse in Azure Key Vault 

Weitere Informationen zu diesen Features finden Sie unter [Best Practices für die Authentifizierung und Autorisierung in Azure Kubernetes Service (AKS)][aks-best-practices-identity].

### <a name="containers"></a>Container
*Container* umfassen Folgendes:
* Azure Policy-Add-On für AKS zum Erzwingen der Podsicherheit
* Verwendung von Podsicherheitskontexten
* Überprüfung von Images und Runtimes auf Sicherheitsrisiken 
* Verwendung von App Armor oder Seccomp (Secure Computing, sicheres Computing) zum Einschränken des Containerzugriffs auf den zugrunde liegenden Knoten

## <a name="logically-isolate-clusters"></a>Logisches Isolieren von Clustern

> **Best Practices-Leitfaden**
>
> Trennen Sie Teams und Projekte mithilfe einer *logischen Isolation*. Minimieren Sie die Anzahl der physischen AKS-Cluster, die Sie bereitstellen, um Teams oder Anwendungen zu isolieren.

Mit logischer Isolation kann ein einzelner AKS-Cluster für mehrere Workloads, Teams oder Umgebungen verwendet werden. Kubernetes-[Namespaces][k8s-namespaces] bilden die logische Isolationsgrenze für Workloads und Ressourcen.

![Logische Isolierung eines Kubernetes-Clusters in AKS](media/operator-best-practices-cluster-isolation/logical-isolation.png)

Die logische Trennung von Clustern bietet in der Regel eine höhere Poddichte als physisch isolierte Cluster, wodurch weniger überschüssige Computekapazität im Cluster ungenutzt bleibt. In Kombination mit der automatischen Clusterskalierung in Kubernetes können Sie die Anzahl von Knoten gemäß der Anforderungen nach oben oder unten skalieren. Dieser Best Practice-Ansatz zur automatischen Skalierung minimiert die Kosten, da nur die erforderliche Anzahl von Knoten ausgeführt wird.

Derzeit sind Kubernetes-Umgebungen nicht vollständig vor einer feindlichen Verwendung mit mehreren Mandanten sicher. In einer Umgebung mit mehreren Mandanten arbeiten mehrere Mandanten in einer gemeinsamen, freigegebenen Infrastruktur. Wenn alle Mandanten nicht vertrauenswürdig sind, benötigen Sie eine zusätzliche Planung, um zu verhindern, dass Mandanten die Sicherheit und den Dienst anderer beeinträchtigen.

Mithilfe zusätzlicher Sicherheitsfeatures wie *Podsicherheitsrichtlinien* oder Kubernetes RBAC für Knoten können Exploits effizient blockiert werden. Um echte Sicherheit bei der Ausführung feindlicher Workloads mit mehreren Mandanten zu erzielen, sollten Sie nur einem Hypervisor vertrauen. Die Sicherheitsdomäne für Kubernetes wird zum gesamten Cluster und nicht zu einem einzelnen Knoten. 

Für diese Art von feindlichen Workloads mit mehreren Mandanten sollten Sie physisch isolierte Cluster verwenden.

## <a name="physically-isolate-clusters"></a>Physisches Isolieren von Clustern

> **Best Practices-Leitfaden** 
>
> Minimieren Sie die Verwendung der physischen Isolation für jedes einzelne Team oder jede einzelne Anwendungsbereitstellung. Verwenden Sie stattdessen die *logische* Isolierung, wie im vorherigen Abschnitt erläutert.

Die physische Trennung von AKS-Clustern ist eine gängige Methode der Clusterisolation. In diesem Isolationsmodell werden Teams oder Workloads ihre eigenen AKS-Cluster zugewiesen. Obwohl die physische Isolation als die einfachste Möglichkeit zum Isolieren von Workloads oder Teams erscheint, erhöht sie den Verwaltungs- und Finanzaufwand. Sie müssen jetzt diese vielen Cluster verwalten und einzeln Zugriff gewähren und Berechtigungen zuweisen. Außerdem werden Ihnen die einzelnen Knoten in Rechnung gestellt.

![Physische Isolierung einzelner Kubernetes-Cluster in AKS](media/operator-best-practices-cluster-isolation/physical-isolation.png)

Physisch separate Cluster verfügen in der Regel über eine geringe Poddichte. Da jedes Team oder jede Workload einen eigenen AKS-Cluster hat, wird dem Cluster häufig ein Übermaß an Computeressourcen bereitgestellt. Häufig ist eine kleine Anzahl von Pods auf diesen Knoten eingeplant. Nicht beanspruchte Knotenkapazität kann nicht für Anwendungen oder Dienste verwendet werden, die von anderen Teams entwickelt werden. Diese überschüssigen Ressourcen tragen zu den zusätzlichen Kosten in physisch separaten Clustern bei.

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel konzentriert sich auf Clusterisolierung. Weitere Informationen zu Clustervorgängen in AKS finden Sie in den folgenden Best Practices:

* [Best Practices für grundlegende Schedulerfunktionen in Azure Kubernetes Service (AKS)][aks-best-practices-scheduler]
* [Best Practices für erweiterte Schedulerfunktionen in Azure Kubernetes Service (AKS)][aks-best-practices-advanced-scheduler]
* [Authentifizierung und Autorisierung][aks-best-practices-identity]

<!-- EXTERNAL LINKS -->

<!-- INTERNAL LINKS -->
[k8s-namespaces]: concepts-clusters-workloads.md#namespaces
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[aks-best-practices-identity]: operator-best-practices-identity.md
