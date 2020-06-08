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
ms.openlocfilehash: c017e9422733069ffd93f6dff72ecb884da057c4
ms.sourcegitcommit: a9784a3fd208f19c8814fe22da9e70fcf1da9c93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/22/2020
ms.locfileid: "83779952"
---
# <a name="use-azure-policy-to-apply-cluster-configurations-at-scale-preview"></a>Verwenden von Azure Policy zum Anwenden von skalierbaren Clusterkonfigurationen (Vorschauversion)

## <a name="overview"></a>Übersicht

Verwenden Sie Azure Policy, um zu erzwingen, dass auf jede `Microsoft.Kubernetes/connectedclusters`- oder Git-Ops-fähige `Microsoft.ContainerService/managedClusters`-Ressource bestimmte `Microsoft.KubernetesConfiguration/sourceControlConfigurations` angewendet werden.  Um Azure Policy zu verwenden, wählen Sie eine vorhandene Richtliniendefinition aus, und erstellen Sie eine Richtlinienzuweisung.  Beim Erstellen der Richtlinienzuweisung legen Sie den Bereich für die Zuweisung fest: Hierbei handelt es sich um eine Azure-Ressourcengruppe oder ein Azure-Abonnement.  Außerdem legen Sie die Parameter für die `sourceControlConfiguration` fest, die erstellt wird.  Nachdem die Zuweisung erstellt wurde, identifiziert das Richtlinienmodul alle `connectedCluster`- oder `managedCluster`-Ressourcen, die sich innerhalb des Bereichs befinden, und wendet die `sourceControlConfiguration` jeweils auf diese an.

Wenn Sie mehrere Git-Repositorys als autoritative Quellen für jeden Cluster verwenden (z. B. ein Repository für den zentralen IT-/Clusteroperator und andere Repositorys für Anwendungsteams), können Sie dies mithilfe mehrerer Richtlinienzuweisungen aktivieren, wobei jede Richtlinienzuweisung für die Verwendung eines anderen Git-Repositorys konfiguriert ist.

## <a name="create-a-custom-policy-definition"></a>Erstellen einer benutzerdefinierten Richtliniendefinition

1. Navigieren Sie im Azure-Portal zu „Richtlinie“, und wählen Sie auf der Randleiste im Abschnitt **Erstellen** die Option **Definitionen** aus.
2. Klicken Sie auf **+ Richtliniendefinition**.
3. Legen Sie Ihr Abonnement oder Ihre Verwaltungsgruppe als **Definitionsspeicherort** fest.  Dadurch wird der größte Bereich festgelegt, in dem die Richtliniendefinition verwendet werden kann.
4. Legen Sie **Name** und **Beschreibung** für die Richtlinie fest.
5. Wählen Sie unter „Kategorie“ die Option **Neu erstellen**, und schreiben Sie *Kubernetes-Cluster – Azure Arc*.
6. Kopieren Sie den Inhalt dieser [Beispielrichtliniendefinition](https://raw.githubusercontent.com/Azure/arc-k8s-demo/master/policy/Ensure-GitOps-configuration-for-Kubernetes-cluster.json), und fügen Sie sie im Bearbeitungsfeld **Richtlinienregel** ein.
7. **Speichern** Sie sie.

Dieser Schritt zum Erstellen einer benutzerdefinierten Richtliniendefinition ist nicht erforderlich, nachdem die Schritte ausgeführt wurden, um diese als integrierte Richtlinie festzulegen.

## <a name="create-a-policy-assignment"></a>Erstellen einer Richtlinienzuweisung

1. Navigieren Sie im Azure-Portal zu „Richtlinie“, und wählen Sie auf der Randleiste im Abschnitt **Erstellen** die Option **Definitionen** aus.
2. Suchen Sie die soeben erstellte Definition, und wählen Sie sie aus.
3. Wählen Sie in den Seitenaktionen den Befehl **Zuweisen** aus.
4. Legen Sie den **Bereich** auf die Verwaltungsgruppe, das Abonnement oder die Ressourcengruppe fest, in dem die Richtlinienzuweisung angewendet wird.
5. Wenn Sie Ressourcen aus dem Richtlinienbereich ausschließen möchten, legen Sie **Ausschlüsse** fest.
6. Weisen Sie der Richtlinienzuweisung einen **Namen** und eine **Beschreibung** zu, die Sie zur einfachen Identifizierung verwenden können.
7. Stellen Sie sicher, dass **Richtlinienerzwingung** auf *Aktiviert* festgelegt ist.
8. Wählen Sie **Weiter** aus.
9. Legen Sie Parameterwerte fest, die während der Erstellung der `sourceControlConfiguration` verwendet werden.
10. Wählen Sie **Weiter** aus.
11. Aktivieren Sie **Korrekturtask erstellen**.
12. Stellen Sie sicher, dass **Verwaltete Identität erstellen** aktiviert ist und dass die Identität **Mitwirkender**-Berechtigungen besitzt.  Weitere Informationen zu den erforderlichen Berechtigungen finden Sie in [diesem Dokument](https://docs.microsoft.com/azure/governance/policy/assign-policy-portal) und im [Kommentar in diesem Dokument](https://docs.microsoft.com/azure/governance/policy/how-to/remediate-resources).
13. Klicken Sie auf **Überprüfen + erstellen**.

Nachdem die Richtlinienzuweisung erstellt wurde, wird für jede neue `connectedCluster`-Ressource (oder `managedCluster`-Ressource bei installierten GitOps-Agents) die `sourceControlConfiguration` angewendet.  Bei vorhandenen Clustern müssen Sie manuell einen Wartungstask ausführen.  Es dauert in der Regel 10-20 Minuten, bis die Richtlinienzuweisung wirksam wird.

## <a name="verify-a-policy-assignment"></a>Überprüfen einer Richtlinienzuweisung

1. Navigieren Sie im Azure-Portal zu einer der `connectedCluster`-Ressourcen, und wählen Sie im Abschnitt **Einstellungen** auf der Randleiste **Richtlinien** aus. (Die Benutzeroberfläche für den verwalteten AKS-Cluster ist noch nicht implementiert.)
2. In der Liste sollte die oben erstellte Richtlinienzuweisung angezeigt werden, und der **Konformitätszustand** sollte *Konform* lauten.
3. Wählen Sie im Abschnitt **Einstellungen** auf der Randleiste **Konfigurationen** aus.
4. In der Liste sollte die `sourceControlConfiguration` angezeigt werden, die von der Richtlinienzuweisung erstellt wurde.
5. Verwenden Sie **kubectl**, um den Cluster abzufragen: Sie sollten den Namespace und die Artefakte sehen, die von der `sourceControlConfiguration` erstellt wurden.
6. Innerhalb von 5 Minuten sollten im Cluster die Artefakte angezeigt werden, die in den Manifesten im konfigurierten Git-Repository beschrieben werden.

## <a name="next-steps"></a>Nächste Schritte

* [Einrichten von Azure Monitor für Container mit Arc-fähigen Kubernetes-Clustern](./deploy-azure-monitor-for-containers.md)
