---
title: Häufig gestellte Fragen zum Azure Arc-fähigen Kubernetes
services: azure-arc
ms.service: azure-arc
ms.date: 02/15/2021
ms.topic: conceptual
author: shashankbarsin
ms.author: shasb
description: Dieser Artikel enthält eine Liste mit häufig gestellten Fragen zu Azure Arc-fähigem Kubernetes.
keywords: Kubernetes, Arc, Azure, Container, Konfiguration, GitOps, FAQ, Häufig gestellte Fragen
ms.openlocfilehash: 237b2629b833a63552b172636f46a1ac92e321c0
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100561243"
---
# <a name="frequently-asked-questions---azure-arc-enabled-kubernetes"></a>Häufig gestellte Fragen: Azure Arc-fähiges Kubernetes

In diesem Artikel werden häufig gestellte Fragen zu Azure Arc-fähigem Kubernetes behandelt.

## <a name="what-is-the-difference-between-azure-arc-enabled-kubernetes-and-azure-kubernetes-service-aks"></a>Worin besteht der Unterschied zwischen Azure Arc-fähigem Kubernetes und Azure Kubernetes Service (AKS)?

AKS ist das Managed Kubernetes-Angebot von Azure. AKS vereinfacht die Bereitstellung eines Managed Kubernetes-Clusters in Azure, indem ein Großteil der Komplexität und des betrieblichen Aufwands nach Azure ausgelagert wird. Da die Kubernetes-Mastereinheiten von Azure verwaltet werden, müssen Sie sich nur um die Verwaltung und Wartung der Agent-Knoten kümmern.

Azure Arc-fähiges Kubernetes ermöglicht es Ihnen, die Verwaltungsfunktionen von Azure (wie Azure Monitor und Azure Policy) zu erweitern, indem Kubernetes-Cluster mit Azure verbunden werden. Den zugrunde liegenden Kubernetes-Cluster pflegen Sie selber.

## <a name="do-i-need-to-connect-my-aks-clusters-running-on-azure-to-azure-arc"></a>Muss ich meine AKS-Cluster, die in Azure ausgeführt werden, mit Azure Arc verbinden?

Nein. Alle Azure Arc-fähigen Kubernetes-Funktionen, einschließlich Azure Monitor und Azure Policy (Gatekeeper), sind auf AKS verfügbar (eine native Ressource in Azure Resource Manager).
    
## <a name="should-i-connect-my-aks-hci-cluster-and-kubernetes-clusters-on-azure-stack-hub-and-azure-stack-edge-to-azure-arc"></a>Sollte ich meinen AKS-HCI-Cluster und meine Kubernetes-Cluster auf Azure Stack Hub und Azure Stack Edge mit Azure Arc verbinden?

Ja, wenn Sie Ihren AKS-HCI-Cluster oder Ihre Kubernetes-Cluster auf Azure Stack Edge oder Azure Stack Hub mit Azure Arc verbinden, wird Clustern die Ressourcendarstellung in Azure Resource Manager bereitgestellt. Diese Ressourcendarstellung erweitert Funktionen wie Clusterkonfiguration, Azure Monitor und Azure Policy (Gatekeeper) auf die Kubernetes-Cluster, die Sie verbinden.

## <a name="how-to-address-expired-azure-arc-enabled-kubernetes-resources"></a>Wie geht man mit abgelaufenen Azure Arc-fähigen Kubernetes-Ressourcen um?

Das Zertifikat der verwalteten Dienstidentität (Managed Service Identity, MSI), das Ihrem Azure Arc-fähigen Kuberenetes zugeordnet ist, verfügt über ein Ablauffenster von 90 Tagen. Nach Ablauf dieses Zertifikats wird die Ressource als `Expired` betrachtet, und alle Features wie Konfiguration, Überwachung und Richtlinie stellen in diesem Cluster die Funktion ein. Führen Sie folgende Schritte aus, damit Ihr Kubernetes-Cluster wieder mit Azure Arc zusammenarbeitet:

1. Löschen die Azure Arc-fähige Kubernetes-Ressource und Agents auf dem Cluster 

    ```console
    az connectedk8s delete -n <name> -g <resource-group>
    ```

1. Erstellen Sie die Azure Arc-fähige Kubernetes-Ressource erneut, indem Sie wieder Agents auf dem Cluster bereitstellen.
    
    ```console
    az connectedk8s connect -n <name> -g <resource-group>
    ```

> [!NOTE]
> `az connectedk8s delete` löscht außerdem zusätzlich zum Cluster noch Konfigurationen. Nachdem Sie `az connectedk8s connect` ausgeführt haben, erstellen Sie die Konfigurationen auf dem Cluster erneut, entweder manuell oder mithilfe von Azure Policy.

## <a name="if-i-am-already-using-cicd-pipelines-can-i-still-use-azure-arc-enabled-kubernetes-and-configurations"></a>Kann ich, wenn ich bereits CI/CD-Pipelines verwende, trotzdem Azure Arc-aktiviertes Kubernetes und Konfigurationen verwenden?

Ja, Sie können immer noch Konfigurationen auf einem Cluster verwenden, der Bereitstellungen über eine CI/CD-Pipeline empfängt. Im Vergleich zu herkömmlichen CI/CD-Pipelines bietet die Funktion „Konfigurationen“ zwei zusätzliche Vorteile:

**Driftausgleich**

Die CI/CD-Pipeline wendet Änderungen während der Pipelineausführung nur einmal an. Der GitOps-Operator im Cluster fragt jedoch kontinuierlich das Git-Repository ab, um den gewünschten Zustand der Kubernetes-Ressourcen im Cluster abzurufen. Wenn der GitOps-Operator feststellt, dass der gewünschte Zustand von Ressourcen vom tatsächlichen Zustand der Ressourcen im Cluster abweicht, wird diese Abweichung (Drift) ausgeglichen.

**Anwenden von GitOps im großen Stil**

CI/CD-Pipelines eignen sich gut für ereignisgestützte Bereitstellungen in Ihrem Kubernetes-Cluster, wobei das Ereignis ein Push in ein Git-Repository sein könnte. Allerdings muss bei einer Bereitstellung derselben Konfiguration auf allen Ihren Kubernetes-Clustern die CI/CD-Pipeline manuell mit Anmeldeinformationen jedes einzelnen dieser Kubernetes-Cluster konfiguriert werden. Andererseits können Sie im Falle von Azure Arc-fähigem Kubernetes, da Azure Resource Manager Ihre Konfigurationen verwaltet, Azure Policy verwenden, um die Anwendung der gewünschten Konfiguration auf alle Kubernetes-Cluster in einem Abonnement- oder Ressourcengruppenumfang in einem Schritt zu automatisieren. Diese Funktion kann sogar auf Azure Arc-fähige Kubernetes-Ressourcen angewendet werden, die nach der Richtlinienzuweisung erstellt wurden.

Die Funktion „Konfigurationen“ wird verwendet, um Baselinekonfigurationen wie Netzwerkrichtlinien, Rollenbindungen und Podsicherheitsrichtlinien im gesamten Bestand von Kubernetes-Clustern auf Compliance- und Governanceanforderungen anzuwenden.

## <a name="next-steps"></a>Nächste Schritte

* [Verbinden eines Clusters mit Azure Arc](./connect-cluster.md)
* [Erstellen von Konfigurationen auf Ihrem Arc-fähigen Kubernetes-Cluster](./use-gitops-connected-cluster.md)
* [Verwenden von Azure Policy zum Anwenden von Konfigurationen im großen Stil](./use-azure-policy.md)
