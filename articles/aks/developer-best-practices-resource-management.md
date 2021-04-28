---
title: Bewährte Methoden für die Ressourcenverwaltung
titleSuffix: Azure Kubernetes Service
description: Lernen Sie bewährte Anwendungsentwicklermethoden für die Ressourcenverwaltung in Azure Kubernetes Service (AKS) kennen.
services: container-service
author: zr-msft
ms.topic: conceptual
ms.date: 03/15/2021
ms.author: zarhoads
ms.openlocfilehash: 2cd2bab05346f66b933512e677f1d38f4514796c
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107105271"
---
# <a name="best-practices-for-application-developers-to-manage-resources-in-azure-kubernetes-service-aks"></a>Bewährte Anwendungsentwicklermethoden zum Verwalten von Ressourcen in Azure Kubernetes Service (AKS)

Beim Entwickeln und Ausführen von Anwendungen in Azure Kubernetes Service (AKS) sind jedoch einige wichtige Aspekte zu berücksichtigen. Die Art, in der Sie Ihre Anwendungsbereitstellungen verwalten, kann die Benutzerfreundlichkeit der Dienste beeinträchtigen, die Sie bereitstellen. Um erfolgreich zu sein, beachten Sie beim Entwickeln und Ausführen von Anwendungen in AKS einige bewährte Methoden.

Dieser Artikel konzentriert sich auf das Ausführen Ihrer Cluster und Workloads aus der Perspektive des Anwendungsentwicklers. Weitere Informationen zu bewährten Verwaltungsmethoden finden Sie unter [Bewährte Methoden der Clusterisolierung in Azure Kubernetes Service (AKS)][operator-best-practices-isolation]. In diesem Artikel wird Folgendes behandelt:

> [!div class="checklist"]
> * Podressourcenanforderungen und -grenzwerte
> * Methoden zum Entwickeln und Bereitstellen von Anwendungen mit Bridge to Kubernetes und Visual Studio Code
> * Verwenden des `kube-advisor`-Tools für die Suche nach Problemen mit Bereitstellungen

## <a name="define-pod-resource-requests-and-limits"></a>Definieren von Podressourcenanforderungen und -grenzwerten

> **Best Practices-Leitfaden**
> 
> Legen Sie Podanforderungen und -grenzwerte für alle Pods in Ihren YAML-Manifesten fest. Wenn der AKS-Cluster *Ressourcenkontingente* verwendet und Sie diese Werte nicht definieren, wird Ihre Bereitstellung möglicherweise abgelehnt.

Verwenden Sie Podanforderungen und -grenzwerte, um die Computeressourcen in einem AKS-Cluster zu verwalten. Podanforderungen und -grenzwerte informieren den Kubernetes-Scheduler darüber, welche Computeressourcen einem Pod zugewiesen werden sollen.

### <a name="pod-cpumemory-requests"></a>Podanforderungen für CPU/Arbeitsspeicher
*Podanforderungen* definieren eine vom Pod regelmäßig benötigte festgelegte CPU-Leistung und Arbeitsspeichermenge.

Es ist eine **bewährte Methode und sehr wichtig**, diese Anforderungen und Grenzwerte in Ihren Podspezifikationen basierend auf den oben genannten Informationen zu definieren. Wenn Sie diese Werte nicht angeben, kann der Kubernetes-Planer nicht die Ressourcen berücksichtigen, die Ihre Anwendungen für die Planung von Entscheidungen benötigen.

Überwachen Sie die Leistung Ihrer Anwendung, um Podanforderungen anzupassen. 
* Wenn Sie Podanforderungen unterschätzen, kann die Leistung der Anwendung aufgrund der Überplanung eines Knotens beeinträchtigt werden. 
* Wenn Anforderungen überschätzt werden, kann es zu erhöhten Schwierigkeiten bei der Planung Ihrer Anwendung kommen.

### <a name="pod-cpumemory-limits"></a>Podgrenzwerte für CPU/Arbeitsspeicher** 
*Podgrenzwerte* legen das Maximum an CPU-Leistung und Arbeitsspeichermenge fest, das ein Pod verwenden kann. 

* *Speichergrenzwerte* definieren, welche Pods bei instabilen Knoten aufgrund unzureichender Ressourcen beendet werden sollen. Ohne geeignete Grenzwerte werden Pods beendet, bis der Ressourcendruck sich erhöht. 
* Obwohl ein Pod den *CPU-Grenzwert* für einen bestimmten Zeitraum überschreiten kann, wird er nicht wegen Überschreitung des CPU-Grenzwerts beendet. 

Podgrenzwerte definieren, wann ein Pod die Kontrolle über den Ressourcenverbrauch verloren hat. Bei Überschreiten des Grenzwerts wird der Pod zum Beenden markiert. Durch dieses Verhalten werden die Knotenintegrität bewahrt und die Auswirkungen auf Pods minimiert, die den Knoten gemeinsam nutzen. Wenn Sie keinen Podgrenzwert festlegen, wird standardmäßig der höchste verfügbare Wert auf einem bestimmten Knoten festgelegt.

Vermeiden Sie es, einen höheren Podgrenzwert festzulegen, als Ihre Knoten unterstützen können. Jeder AKS-Knoten reserviert eine bestimmte Menge von CPU-Leistung und Arbeitsspeichermenge für die Kubernetes-Kernkomponenten. Ihre Anwendung versucht möglicherweise, zu viele Ressourcen auf dem Knoten für die erfolgreiche Ausführung anderer Pods zu beanspruchen.

Überwachen Sie die Leistung Ihrer Anwendung zu verschiedenen Zeitpunkten während des Tags oder der Woche. Bestimmen Sie die Spitzenzeiten der Nachfrage, und passen Sie die Podgrenzwerte den Ressourcen an, die zur Deckung des maximalen Bedarfs erforderlich sind.

> [!IMPORTANT]
>
> Definieren Sie diese Anforderungen und Grenzwerte in Ihren Podspezifikationen basierend auf den oben genannten Informationen. Wenn Sie diese Werte nicht angeben, kann der Kubernetes-Scheduler nicht die Ressourcen berücksichtigen, die Ihre Anwendungen für die Planung von Entscheidungen benötigen.

Wenn der Planer einen Pod auf einem Knoten mit unzureichenden Ressourcen platziert, wird die Anwendungsleistung beeinträchtigt. Clusteradministratoren **müssen** *Ressourcenkontingente* für einen Namespace festlegen, der von Ihnen das Festlegen von Ressourcenanforderungen und -grenzwerten erfordert. Weitere Informationen finden Sie unter [Ressourcenkontingente auf AKS-Clustern][resource-quotas].

Beim Definieren einer CPU-Anforderung oder eines Grenzwerts wird der Wert in CPU-Einheiten gemessen. 
* *1.0* Die CPU entspricht einem zugrunde liegenden virtuellen CPU-Kern auf dem Knoten. 
    * Dieselbe Maßeinheit wird für GPUs verwendet.
* Sie können Anteile, gemessen in Millicore, definieren. Beispiel: *100m* ist *0,1* eines zugrunde liegenden vCPU-Kerns.

Im folgenden grundlegenden Beispiel für einen einzelnen NGINX-Pod fordert der Pod *100m* der CPU-Zeit und *128Mi* des Arbeitsspeichers an. Die Ressourcengrenzwerte für den Pod sind auf *250m* CPU und *256Mi* Arbeitsspeicher festgelegt:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
  - name: mypod
    image: mcr.microsoft.com/oss/nginx/nginx:1.15.5-alpine
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
```

Weitere Informationen zu Ressourcenverwaltung und Zuweisungen finden Sie unter [Verwalten von Computeressourcen für Container][k8s-resource-limits].

## <a name="develop-and-debug-applications-against-an-aks-cluster"></a>Entwickeln und Debuggen von Anwendungen für einen AKS-Cluster

> **Best Practices-Leitfaden** 
>
> Entwicklungsteams sollten einen AKS-Cluster mithilfe von Bridge to Kubernetes bereitstellen und debuggen.

Mit Bridge to Kubernetes können Sie Anwendungen direkt für einen AKS-Cluster entwickeln, debuggen und testen. Entwickler in einem Team arbeiten zusammen, um während des gesamten Anwendungslebenszyklus diese zu erstellen und zu testen. Vorhandene Tools wie Visual Studio oder Visual Studio Code können mit der Bridge to Kubernetes-Erweiterung weiterhin verwendet werden. 

Durch Verwendung des integrierten Entwicklungs- und Testprozesses mit Bridge to Kubernetes wird der Bedarf an lokalen Testumgebungen wie [minikube][minikube] reduziert. Stattdessen entwickeln und testen Sie anhand eines AKS-Clusters, auch anhand gesicherter und isolierter Cluster. 

> [!NOTE]
> Bridge to Kubernetes ist für die Verwendung mit auf Linux-Pods und -Knoten ausgeführten Anwendungen vorgesehen.

## <a name="use-the-visual-studio-code-vs-code-extension-for-kubernetes"></a>Verwenden der Visual Studio Code-Erweiterung (VS Code) für Kubernetes

> **Best Practices-Leitfaden** 
>
> Installieren und verwenden Sie die VS Code-Erweiterung für Kubernetes beim Schreiben von YAML-Manifesten. Sie können die Erweiterung auch für integrierte Bereitstellungslösungen verwenden, was Besitzern von Anwendungen helfen kann, die nur selten mit dem AKS-Cluster interagieren.

Die [Visual Studio Code-Erweiterung für Kubernetes][vscode-kubernetes] hilft Ihnen, Anwendungen in AKS zu entwickeln und bereitzustellen. Die Erweiterung bietet Folgendes:
* Intellisense für Kubernetes-Ressourcen, Helm-Diagramme und Vorlagen 
* Funktionen zum Durchsuchen, Bereitstellen und Bearbeiten von Kubernetes-Ressourcen in VS Code 
* Intellisense-Überprüfung für Ressourcenanforderungen oder -grenzwerte, die in den Podspezifikationen festgelegt werden:

    ![VS Code-Erweiterung für Kubernetes-Warnung zu fehlenden Arbeitsspeicherlimits](media/developer-best-practices-resource-management/vs-code-kubernetes-extension.png)

## <a name="regularly-check-for-application-issues-with-kube-advisor"></a>Regelmäßiges Überprüfen auf Anwendungsprobleme mit dem Kube-Advisor

> **Best Practices-Leitfaden** 
> 
> Führen Sie regelmäßig die neueste Version des Open-Source-Tools `kube-advisor` aus, um Probleme in Ihrem Cluster zu erkennen. Führen Sie `kube-advisor` aus, bevor Sie Ressourcenkontingente auf einen bestehenden AKS-Cluster anwenden, um Pods zu finden, für die keine Ressourcenanforderungen und -grenzwerte definiert sind.

Das [kube-advisor][kube-advisor]-Tool ist ein verwandtes Open-Source-Projekt für AKS, das einen Kubernetes-Cluster scannt und erkannte Probleme meldet. Eine nützliche Überprüfung ist die Identifizierung von Pods, für die keine Ressourcenanforderungen und -grenzwerte angegeben sind.

Während das `kube-advisor`-Tool Berichte zu Ressourcenanforderungen und -grenzwerten erstellen kann, die in PodSpecs für Windows- und Linux-Anwendungen fehlen, muss `kube-advisor` selbst auf einem Linux-Pod geplant werden. Verwenden Sie einen [Knotenselektor][k8s-node-selector] in der Konfiguration eines Pods, um ihn für die Ausführung in einem Knotenpool mit einem bestimmten Betriebssystem zu planen.

In einem AKS-Cluster, der viele Entwicklungsteams und Anwendungen hostet, kann es einfacher sein, Pods anhand von Ressourcenanforderungen und -grenzwerten zu verfolgen. Führen Sie als Best Practice regelmäßig `kube-advisor` in Ihren AKS-Clustern aus.

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel konzentriert sich darauf, wie Sie Ihre Cluster und Workloads aus der Perspektive des Clusteroperators ausführen. Weitere Informationen zu bewährten Verwaltungsmethoden finden Sie unter [Bewährte Methoden der Clusterisolierung in Azure Kubernetes Service (AKS)][operator-best-practices-isolation].

Um einige dieser Best Practices zu implementieren, lesen Sie folgende Artikel:

* [Entwickeln mit Bridge to Kubernetes][btk]
* [Überprüfen auf Probleme mit dem Kube-Advisor][aks-kubeadvisor]

<!-- EXTERNAL LINKS -->
[k8s-resource-limits]: https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/
[vscode-kubernetes]: https://github.com/Azure/vscode-kubernetes-tools
[kube-advisor]: https://github.com/Azure/kube-advisor
[minikube]: https://kubernetes.io/docs/setup/minikube/

<!-- INTERNAL LINKS -->
[aks-kubeadvisor]: kube-advisor-tool.md
[btk]: /visualstudio/containers/overview-bridge-to-kubernetes
[operator-best-practices-isolation]: operator-best-practices-cluster-isolation.md
[resource-quotas]: operator-best-practices-scheduler.md#enforce-resource-quotas
[k8s-node-selector]: concepts-clusters-workloads.md#node-selectors
