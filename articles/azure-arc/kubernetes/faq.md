---
title: Häufig gestellte Fragen zum Azure Arc-fähigen Kubernetes
services: azure-arc
ms.service: azure-arc
ms.date: 02/19/2021
ms.topic: conceptual
author: shashankbarsin
ms.author: shasb
description: Dieser Artikel enthält eine Liste mit häufig gestellten Fragen zu Azure Arc-fähigem Kubernetes.
keywords: Kubernetes, Arc, Azure, Container, Konfiguration, GitOps, FAQ, Häufig gestellte Fragen
ms.openlocfilehash: dc12294b5d53372be5f2e1dd71436973fefbb194
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101647862"
---
# <a name="frequently-asked-questions---azure-arc-enabled-kubernetes"></a>Häufig gestellte Fragen: Azure Arc-fähiges Kubernetes

In diesem Artikel werden häufig gestellte Fragen zu Azure Arc-fähigem Kubernetes behandelt.

## <a name="what-is-the-difference-between-azure-arc-enabled-kubernetes-and-azure-kubernetes-service-aks"></a>Worin besteht der Unterschied zwischen Azure Arc-fähigem Kubernetes und Azure Kubernetes Service (AKS)?

AKS ist das Managed Kubernetes-Angebot von Azure. AKS vereinfacht die Bereitstellung eines Managed Kubernetes-Clusters in Azure, indem ein Großteil der Komplexität und des betrieblichen Aufwands nach Azure ausgelagert wird. Da die Kubernetes-Mastereinheiten von Azure verwaltet werden, müssen Sie sich nur um die Verwaltung und Wartung der Agent-Knoten kümmern.

Azure Arc-fähiges Kubernetes ermöglicht es Ihnen, die Verwaltungsfunktionen von Azure (wie Azure Monitor und Azure Policy) zu erweitern, indem Kubernetes-Cluster mit Azure verbunden werden. Den zugrunde liegenden Kubernetes-Cluster pflegen Sie selber.

## <a name="do-i-need-to-connect-my-aks-clusters-running-on-azure-to-azure-arc"></a>Muss ich meine AKS-Cluster, die in Azure ausgeführt werden, mit Azure Arc verbinden?

Nein. Alle Azure Arc-fähigen Kubernetes-Funktionen, einschließlich Azure Monitor und Azure Policy (Gatekeeper), sind auf AKS verfügbar (eine native Ressource in Azure Resource Manager).
    
## <a name="should-i-connect-my-aks-hci-cluster-and-kubernetes-clusters-on-azure-stack-hub-and-azure-stack-edge-to-azure-arc"></a>Sollte ich meinen AKS-HCI-Cluster und meine Kubernetes-Cluster auf Azure Stack Hub und Azure Stack Edge mit Azure Arc verbinden?

Ja, wenn Sie Ihren AKS-HCI-Cluster oder Ihre Kubernetes-Cluster auf Azure Stack Edge oder Azure Stack Hub mit Azure Arc verbinden, wird Clustern die Ressourcendarstellung in Azure Resource Manager bereitgestellt. Diese Ressourcendarstellung erweitert Funktionen wie Clusterkonfiguration, Azure Monitor und Azure Policy (Gatekeeper) auf die verbundenen Kubernetes-Cluster.

Wenn sich der Kubernetes-Cluster mit Azure Arc-Aktivierung unter Azure Stack Edge, AKS unter Azure Stack HCI (>= Update von April 2021) oder AKS unter Windows Server 2019 Datacenter (>= Update von April 2021) befindet, dann ist die Kubernetes-Konfiguration kostenlos enthalten.

## <a name="how-to-address-expired-azure-arc-enabled-kubernetes-resources"></a>Wie geht man mit abgelaufenen Azure Arc-fähigen Kubernetes-Ressourcen um?

Das Zertifikat der verwalteten Dienstidentität (Managed Service Identity, MSI), das Ihrem Kubernetes mit Azure Arc-Aktivierung zugeordnet ist, verfügt über ein Ablauffenster von 90 Tagen. Nach Ablauf dieses Zertifikats wird die Ressource als `Expired` betrachtet, und alle Features (wie Konfiguration, Überwachung und Richtlinie) stellen in diesem Cluster die Funktion ein. Führen Sie folgende Schritte aus, damit Ihr Kubernetes-Cluster wieder mit Azure Arc zusammenarbeitet:

1. Löschen Sie die Kubernetes-Ressource mit Azure Arc-Aktivierung sowie die Agents auf dem Cluster. 

    ```console
    az connectedk8s delete -n <name> -g <resource-group>
    ```

1. Erstellen Sie die Azure Arc-fähige Kubernetes-Ressource erneut, indem Sie Agents auf dem Cluster bereitstellen.
    
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

CI/CD-Pipelines sind hilfreich für ereignisgesteuerte Bereitstellungen in Ihrem Kubernetes-Cluster (Beispiel: Push in ein Git-Repository). Wenn Sie jedoch dieselbe Konfiguration für alle Ihre Kubernetes-Cluster bereitstellen möchten, müssen Sie die Anmeldeinformationen jedes Kubernetes-Clusters manuell für die CI/CD-Pipeline konfigurieren. 

Da Azure Resource Manager Ihre Konfigurationen für Kubernetes mit Azure Arc-Aktivierung verwaltet, können Sie mithilfe von Azure Policy im Rahmen eines Abonnements oder einer Ressourcengruppe automatisch dieselbe Konfiguration für alle Kubernetes-Ressourcen mit Azure Arc-Aktivierung erstellen. Diese Funktion kann sogar auf Azure Arc-fähige Kubernetes-Ressourcen angewendet werden, die nach der Richtlinienzuweisung erstellt wurden.

Dieses Feature wendet Baselinekonfigurationen (wie Netzwerkrichtlinien, Rollenbindungen und Podsicherheitsrichtlinien) auf den gesamten Bestand der Kubernetes-Cluster an, um Compliance- und Governanceanforderungen zu entsprechen.

## <a name="next-steps"></a>Nächste Schritte

* [Verbinden eines Clusters mit Azure Arc](./quickstart-connect-cluster.md)
* [Erstellen von Konfigurationen auf Ihrem Arc-fähigen Kubernetes-Cluster](./use-gitops-connected-cluster.md)
* [Verwenden von Azure Policy zum Anwenden von Konfigurationen im großen Stil](./use-azure-policy.md)
