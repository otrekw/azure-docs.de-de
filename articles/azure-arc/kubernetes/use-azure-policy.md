---
title: Verwenden von Azure Policy zum Anwenden von skalierbaren Clusterkonfigurationen (Vorschauversion)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Verwenden von Azure Policy zum Anwenden skalierbarer Clusterkonfigurationen
keywords: Kubernetes, Arc, Azure, K8s, Container
ms.openlocfilehash: e4279f3d89376320116067bf191e3196271918ce
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87050042"
---
# <a name="use-azure-policy-to-apply-cluster-configurations-at-scale-preview"></a>Verwenden von Azure Policy zum Anwenden von skalierbaren Clusterkonfigurationen (Vorschauversion)

## <a name="overview"></a>Übersicht

Verwenden Sie Azure Policy, um zu erzwingen, dass auf jede `Microsoft.Kubernetes/connectedclusters`-Ressource oder Git-Ops-fähige `Microsoft.ContainerService/managedClusters`-Ressource bestimmte `Microsoft.KubernetesConfiguration/sourceControlConfigurations` angewendet werden. Um Azure Policy zu verwenden, wählen Sie eine vorhandene Richtliniendefinition aus, und erstellen Sie eine Richtlinienzuweisung. Beim Erstellen der Richtlinienzuweisung legen Sie den Bereich für die Zuweisung fest: Hierbei handelt es sich um eine Azure-Ressourcengruppe oder ein Azure-Abonnement. Außerdem legen Sie die Parameter für die `sourceControlConfiguration` fest, die erstellt wird. Nachdem die Zuweisung erstellt wurde, identifiziert das Richtlinienmodul alle `connectedCluster`- oder `managedCluster`-Ressourcen, die sich innerhalb des Bereichs befinden, und wendet die `sourceControlConfiguration` jeweils auf diese an.

Wenn Sie mehrere Git-Repositorys als autoritative Quellen für jeden Cluster verwenden (z. B. ein Repository für den zentralen IT-/Clusteroperator und andere Repositorys für Anwendungsteams), können Sie dies mithilfe mehrerer Richtlinienzuweisungen aktivieren, wobei jede Richtlinienzuweisung für die Verwendung eines anderen Git-Repositorys konfiguriert ist.

## <a name="prerequisite"></a>Voraussetzung

Stellen Sie sicher, dass Sie über Berechtigungen für `Microsoft.Authorization/policyAssignments/write` für den Reservierungsumfang (Abonnement oder Ressourcengruppe) verfügen, in dem Sie diese Richtlinienzuweisung erstellen möchten.

## <a name="create-a-policy-assignment"></a>Erstellen einer Richtlinienzuweisung

1. Navigieren Sie im Azure-Portal zu „Richtlinie“, und wählen Sie auf der Randleiste im Abschnitt **Erstellen** die Option **Definitionen** aus.
2. Wählen Sie in der Kategorie „Kubernetes“ die integrierte Richtlinie „GitOps für Kubernetes-Cluster bereitstellen“ aus, und klicken Sie auf **Zuweisen**.
3. Legen Sie den **Bereich** auf die Verwaltungsgruppe, das Abonnement oder die Ressourcengruppe fest, in dem die Richtlinienzuweisung angewendet wird.
4. Wenn Sie Ressourcen aus dem Richtlinienbereich ausschließen möchten, legen Sie **Ausschlüsse** fest.
5. Weisen Sie der Richtlinienzuweisung einen **Namen** und eine **Beschreibung** zu, die Sie zur einfachen Identifizierung verwenden können.
6. Stellen Sie sicher, dass **Richtlinienerzwingung** auf *Aktiviert* festgelegt ist.
7. Wählen Sie **Weiter** aus.
8. Legen Sie Parameterwerte fest, die während der Erstellung der `sourceControlConfiguration` verwendet werden.
9. Wählen Sie **Weiter** aus.
10. Aktivieren Sie **Korrekturtask erstellen**.
11. Stellen Sie sicher, dass **Verwaltete Identität erstellen** aktiviert ist und dass die Identität **Mitwirkender**-Berechtigungen besitzt. Weitere Informationen zu den erforderlichen Berechtigungen finden Sie in [diesem Dokument](../../governance/policy/assign-policy-portal.md) und im [Kommentar in diesem Dokument](../../governance/policy/how-to/remediate-resources.md).
12. Klicken Sie auf **Überprüfen + erstellen**.

Nachdem die Richtlinienzuweisung erstellt wurde, wird für jede neue `connectedCluster`-Ressource (oder `managedCluster`-Ressource bei installierten GitOps-Agents) die `sourceControlConfiguration` angewendet. Bei vorhandenen Clustern müssen Sie manuell einen Wartungstask ausführen. Es dauert in der Regel 10-20 Minuten, bis die Richtlinienzuweisung wirksam wird.

## <a name="verify-a-policy-assignment"></a>Überprüfen einer Richtlinienzuweisung

1. Navigieren Sie im Azure-Portal zu einer der `connectedCluster`-Ressourcen, und wählen Sie im Abschnitt **Einstellungen** auf der Randleiste **Richtlinien** aus. (Die Benutzeroberfläche für den AKS-Cluster ist noch nicht implementiert.)
2. In der Liste sollte die oben erstellte Richtlinienzuweisung angezeigt werden, und der **Konformitätszustand** sollte *Konform* lauten.
3. Wählen Sie im Abschnitt **Einstellungen** auf der Randleiste **Konfigurationen** aus.
4. In der Liste sollte die `sourceControlConfiguration` angezeigt werden, die von der Richtlinienzuweisung erstellt wurde.
5. Verwenden Sie **kubectl**, um den Cluster abzufragen: Sie sollten den Namespace und die Artefakte sehen, die von der `sourceControlConfiguration` erstellt wurden.
6. Innerhalb von 5 Minuten sollten im Cluster die Artefakte angezeigt werden, die in den Manifesten im konfigurierten Git-Repository beschrieben werden.

## <a name="next-steps"></a>Nächste Schritte

* [Einrichten von Azure Monitor für Container mit Arc-fähigen Kubernetes-Clustern](../../azure-monitor/insights/container-insights-enable-arc-enabled-clusters.md)
