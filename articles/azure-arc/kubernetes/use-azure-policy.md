---
title: Verwenden von Azure Policy zum Anwenden von skalierbaren Clusterkonfigurationen (Vorschauversion)
services: azure-arc
ms.service: azure-arc
ms.date: 02/15/2021
ms.topic: article
author: mlearned
ms.author: mlearned
description: Verwenden von Azure Policy zum Anwenden skalierbarer Clusterkonfigurationen
keywords: Kubernetes, Arc, Azure, K8s, Container
ms.openlocfilehash: b80e50cb4823632f054de3b7f9da71392f8578d7
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100560192"
---
# <a name="use-azure-policy-to-apply-cluster-configurations-at-scale-preview"></a>Verwenden von Azure Policy zum Anwenden von skalierbaren Clusterkonfigurationen (Vorschauversion)

## <a name="overview"></a>Übersicht

Sie können Azure Policy verwenden, um die Anwendung spezieller `Microsoft.KubernetesConfiguration/sourceControlConfigurations` auf eine der folgenden Ressourcen zu erzwingen:
*  `Microsoft.Kubernetes/connectedclusters`-Ressource
* GitOps-fähige `Microsoft.ContainerService/managedClusters`-Ressource 

Um Azure Policy zu verwenden, wählen Sie eine vorhandene Richtliniendefinition aus, und erstellen Sie eine Richtlinienzuweisung. Beim Erstellen der Richtlinienzuweisung:
1. Legen Sie den Bereich für die Zuweisung fest.
    * Der Bereich ist eine Azure-Ressourcengruppe oder ein Azure-Abonnement. 
2. Legen Sie die Parameter für die `sourceControlConfiguration` fest, die erstellt wird. 

Nachdem die Zuweisung erstellt wurde, identifiziert die Azure Policy-Engine alle `connectedCluster`- oder `managedCluster`-Ressourcen, die sich innerhalb des Bereichs befinden, und wendet die `sourceControlConfiguration` jeweils auf diese an.

Sie können für jeden Cluster mehrere Git-Repositorys als autoritative Quellen aktiveren, indem Sie mehrere Richtlinienzuweisungen verwenden. Jede Richtlinienzuweisung wird dann für die Verwendung eines anderen Git-Repositorys konfiguriert, z. B. ein Repository für den zentralen IT-/Clusteroperator und andere Repositorys für die Anwendungsteams.

## <a name="prerequisite"></a>Voraussetzungen

Überprüfen Sie, ob Sie über `Microsoft.Authorization/policyAssignments/write`-Berechtigungen für den Bereich (Abonnement oder Ressourcengruppe) verfügen, in dem Sie diese Richtlinienzuweisung erstellen möchten.

## <a name="create-a-policy-assignment"></a>Erstellen einer Richtlinienzuweisung

1. Navigieren Sie im Azure-Portal zu **Policy**.
1. Wählen Sie im Abschnitt **Erstellung** auf der Randleiste die Option **Definitionen** aus.
1. Wählen Sie in der Kategorie „Kubernetes“ die integrierte Richtlinie „GitOps für Kubernetes-Cluster bereitstellen“ aus. 
1. Klicken Sie auf **Zuweisen**.
1. Legen Sie den **Bereich** auf die Verwaltungsgruppe, das Abonnement oder die Ressourcengruppe fest, in der oder dem die Richtlinienzuweisung angewandt wird.
    * Wenn Sie Ressourcen aus dem Richtlinienbereich ausschließen möchten, legen Sie **Ausschlüsse** fest.
1. Legen Sie für die Richtlinienzuweisung einen einfach erkennbaren **Namen** und eine **Beschreibung** fest.
1. Stellen Sie sicher, dass **Richtlinienerzwingung** auf **Aktiviert** festgelegt ist.
1. Wählen Sie **Weiter** aus.
1. Legen Sie die Parameterwerte für die Erstellung der `sourceControlConfiguration` fest.
1. Wählen Sie **Weiter** aus.
1. Aktivieren Sie **Korrekturtask erstellen**.
1. Stellen Sie sicher, dass **Verwaltete Identität erstellen** aktiviert ist und dass die Identität **Mitwirkender**-Berechtigungen besitzt. 
    * Weitere Informationen finden Sie im [Schnellstart zum Erstellen einer Richtlinienzuweisung](../../governance/policy/assign-policy-portal.md) und im Artikel [Korrigieren nicht konformer Ressourcen mit Azure Policy](../../governance/policy/how-to/remediate-resources.md).
1. Klicken Sie auf **Überprüfen + erstellen**.

Nachdem Sie die Richtlinienzuweisung erstellt haben, wird die `sourceControlConfiguration` auf alle folgenden Ressourcen angewandt, die sich innerhalb des Bereichs der Zuweisung befinden:
* Neue `connectedCluster`-Ressourcen
* Neue `managedCluster`-Ressourcen mit installierten GitOps-Agents 

Bei vorhandenen Clustern müssen Sie manuell einen Wartungstask ausführen. Es dauert in der Regel 10 bis 20 Minuten, bis die Richtlinienzuweisung wirksam wird.

## <a name="verify-a-policy-assignment"></a>Überprüfen einer Richtlinienzuweisung

1. Navigieren Sie im Azure-Portal zu einer Ihrer `connectedCluster`-Ressourcen.
1. Wählen Sie im Abschnitt **Einstellungen** auf der Randleiste **Richtlinien** aus. 
    * Die Benutzeroberfläche für AKS-Cluster ist noch nicht implementiert.
    * In der Liste der Richtlinien sollte die oben erstellte Richtlinienzuweisung angezeigt werden, und der **Konformitätszustand** sollte *Konform* lauten.
1. Wählen Sie im Abschnitt **Einstellungen** auf der Randleiste **Konfigurationen** aus.
    * In der Liste der Konfigurationen sollte die `sourceControlConfiguration` angezeigt werden, die von der Richtlinienzuweisung erstellt wurde.
1. Verwenden Sie `kubectl`, um den Cluster abzufragen. 
    * Sie sollten den Namespace und die Artefakte sehen, die von der `sourceControlConfiguration` erstellt wurden.
    * Innerhalb von 5 Minuten sollten im Cluster die Artefakte angezeigt werden, die in den Manifesten im konfigurierten Git-Repository beschrieben werden.

## <a name="next-steps"></a>Nächste Schritte

* [Einrichten von Azure Monitor für Container mit Arc-fähigen Kubernetes-Clustern](../../azure-monitor/insights/container-insights-enable-arc-enabled-clusters.md)
