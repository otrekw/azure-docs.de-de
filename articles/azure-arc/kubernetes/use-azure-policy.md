---
title: Verwenden von Azure Policy zum Anwenden skalierbarer Clusterkonfigurationen
services: azure-arc
ms.service: azure-arc
ms.date: 03/02/2021
ms.topic: article
author: mlearned
ms.author: mlearned
description: Verwenden von Azure Policy zum Anwenden skalierbarer Clusterkonfigurationen
keywords: Kubernetes, Arc, Azure, K8s, Container
ms.openlocfilehash: 7f85050666c383ba49730bd88ce1f26d55607e7a
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101652146"
---
# <a name="use-azure-policy-to-apply-cluster-configurations-at-scale"></a>Verwenden von Azure Policy zum Anwenden skalierbarer Clusterkonfigurationen

## <a name="overview"></a>Übersicht

Sie können mit Azure Policy entsprechende Konfigurationen (`Microsoft.KubernetesConfiguration/sourceControlConfigurations`-Ressourcentyp) im großen Stil auf Kubernetes-Cluster mit Azure Arc-Aktivierung (`Microsoft.Kubernetes/connectedclusters`) anwenden.

Um Azure Policy zu verwenden, wählen Sie eine vorhandene Richtliniendefinition aus, und erstellen Sie eine Richtlinienzuweisung. Beim Erstellen der Richtlinienzuweisung:
1. Legen Sie den Bereich für die Zuweisung fest.
    * Der Bereich ist eine Azure-Ressourcengruppe oder ein Azure-Abonnement. 
2. Legen Sie die Parameter für die Konfiguration fest, die erstellt wird. 

Sobald die Zuweisung erstellt ist, identifiziert die Azure Policy-Engine alle Kubernetes-Cluster mit Azure Arc-Aktivierung, die sich innerhalb des Bereichs befinden, und wendet die Konfiguration auf jeden Cluster an.

Sie können mehrere Konfigurationen erstellen, die jeweils auf ein anderes Git-Repository verweisen, indem Sie mehrere Richtlinienzuweisungen verwenden. Beispiel: Ein Repository für den zentralen IT-/Clusteroperator und weitere Repositorys für Anwendungsteams.

## <a name="prerequisite"></a>Voraussetzung

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

Nach dem Erstellen der Richtlinienzuweisung wird die Konfiguration auf neue Kubernetes-Cluster mit Azure Arc-Aktivierung angewendet, die im Bereich der Richtlinienzuweisung erstellt wurden.

Bei vorhandenen Clustern müssen Sie manuell einen Wartungstask ausführen. Es dauert in der Regel 10 bis 20 Minuten, bis die Richtlinienzuweisung wirksam wird.

## <a name="verify-a-policy-assignment"></a>Überprüfen einer Richtlinienzuweisung

1. Navigieren Sie im Azure-Portal zu einem Ihrer Kubernetes-Cluster mit Azure Arc-Aktivierung.
1. Wählen Sie im Abschnitt **Einstellungen** auf der Randleiste **Richtlinien** aus. 
    * In der Liste der Richtlinien sollte die oben erstellte Richtlinienzuweisung angezeigt werden, und der **Konformitätszustand** sollte *Konform* lauten.
1. Wählen Sie im Abschnitt **Einstellungen** auf der Randleiste **Konfigurationen** aus.
    * In der Liste der Konfigurationen sollte die durch die Richtlinienzuweisung erstellte Konfiguration angezeigt werden.
1. Verwenden Sie `kubectl`, um den Cluster abzufragen. 
    * Sie sollten den Namespace und die Artefakte sehen, die von den Konfigurationsressourcen erstellt wurden.
    * Innerhalb von fünf Minuten (vorausgesetzt, der Cluster verfügt über Netzwerkkonnektivität mit Azure) sollten Sie sehen, dass die Objekte, die durch die Manifeste im Git-Repository beschrieben werden, auf dem Cluster erstellt werden.

## <a name="next-steps"></a>Nächste Schritte

* [Einrichten von Azure Monitor für Container mit Arc-fähigen Kubernetes-Clustern](../../azure-monitor/containers/container-insights-enable-arc-enabled-clusters.md)
