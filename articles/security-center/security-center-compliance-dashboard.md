---
title: 'Tutorial: Überprüfungen der Einhaltung gesetzlicher Bestimmungen: Azure Security Center'
description: 'Tutorial: Es wird beschrieben, wie Sie die Einhaltung gesetzlicher Bestimmungen verbessern, indem Sie Azure Security Center verwenden.'
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 5f50c4dc-ea42-418d-9ea8-158ffeb93706
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/28/2021
ms.author: memildin
ms.openlocfilehash: 523b081b59bd2f4c45c1ceeb9f39c58f4e3b02b1
ms.sourcegitcommit: 04297f0706b200af15d6d97bc6fc47788785950f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98986903"
---
# <a name="tutorial-improve-your-regulatory-compliance"></a>Tutorial: Verbessern der Einhaltung gesetzlicher Vorschriften

Mit Azure Security Center kann der Prozess zur Einhaltung von Anforderungen gesetzlicher Bestimmungen optimiert werden, indem das **Dashboard zur Einhaltung gesetzlicher Bestimmungen** verwendet wird. 

Security Center bewertet kontinuierlich Ihre Hybrid Cloud-Umgebung, um die Risikofaktoren gemäß den Kontrollen und bewährten Methoden in den Standards zu analysieren, die auf Ihre Abonnements angewendet wurden. Das Dashboard gibt den Status Ihrer Einhaltung dieser Standards wieder. 

Wenn Sie Security Center für ein Azure-Abonnement aktivieren, wird ihm automatisch der [Azure-Sicherheitsvergleichstest](../security/benchmarks/introduction.md) zugewiesen. Diese weit verbreitete Benchmark basiert auf den Kontrollen des [Center for Internet Security (CIS)](https://www.cisecurity.org/benchmark/azure/) und des [National Institute of Standards and Technology (NIST)](https://www.nist.gov/) und konzentriert sich auf cloudzentrierte Sicherheit.

Im Dashboard für die Einhaltung gesetzlicher Bestimmungen wird der Status aller Bewertungen in Ihrer Umgebung im Kontext eines bestimmten Standards oder einer Bestimmung angezeigt. Wenn Sie aufgrund der Empfehlungen Maßnahmen ergreifen und die Risikofaktoren Ihrer Umgebung reduzieren, verbessert sich Ihr Konformitätsstatus.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Evaluieren der Einhaltung gesetzlicher Bestimmungen im entsprechenden Dashboard
> * Verbessern Ihres Konformitätsstatus durch das Ergreifen von Maßnahmen aufgrund von Empfehlungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchlaufen der in diesem Tutorial behandelten Features müssen folgende Voraussetzungen erfüllt sein:

- [Azure Defender](azure-defender.md) muss aktiviert sein. Azure Defender kann 30 Tage lang kostenlos getestet werden.
- Sie müssen mit einem Konto angemeldet sein, das über Leserzugriff auf die Daten zur Richtlinienkonformität verfügt. (**Sicherheitsleseberechtigter** reicht nicht aus.) Hierzu kann die Rolle **Globaler Leser** für das Abonnement verwendet werden. Ihnen müssen mindestens die Rollen **Mitwirkender bei Ressourcenrichtlinien** und **Sicherheitsadministrator** zugewiesenen sein.

##  <a name="assess-your-regulatory-compliance"></a>Bewerten der Einhaltung gesetzlicher Bestimmungen

Auf dem Dashboard zur Einhaltung gesetzlicher Bestimmungen werden Ihre ausgewählten Konformitätsstandards mit allen zugehörigen Anforderungen angezeigt. Die unterstützten Anforderungen sind hierbei jeweils den entsprechenden Sicherheitsbewertungen zugeordnet. Der Status dieser Bewertungen spiegelt Ihre Einhaltung des Standards wider.

Verwenden Sie das Dashboard zur Einhaltung gesetzlicher Bestimmungen, um sich auf Konformitätslücken im Zusammenhang mit Standards und Bestimmung zu konzentrieren, die für Sie relevant sind. Diese fokussierte Ansicht ermöglicht auch eine kontinuierliche Überwachung Ihrer Compliance in dynamischen Cloud- und Hybridumgebungen.

1. Wählen Sie im Security Center-Menü die Option **Einhaltung gesetzlicher Bestimmungen** aus.

    Im oberen Bildschirmbereich befindet sich ein Dashboard mit einer Übersicht über Ihren Konformitätsstatus mit den unterstützten Bestimmungen. Es sind Ihr Gesamtstatus der Konformität und die Anzahl von bestandenen und nicht bestandenen Bewertungen der einzelnen Standards angegeben.

    :::image type="content" source="./media/security-center-compliance-dashboard/compliance-dashboard.png" alt-text="Dashboard für die Einhaltung gesetzlicher Bestimmungen" lightbox="./media/security-center-compliance-dashboard/compliance-dashboard.png":::

1. Wählen Sie eine Registerkarte für einen Konformitätsstandard aus, der für Sie relevant ist (1). Sie sehen, auf welche Abonnements der Standard angewendet wird (2), und die Liste mit allen Kontrollen für den Standard (3) wird angezeigt. Sie können für eine Kontrolle jeweils die Details zu den bestandenen und nicht bestandenen Bewertungen (4) sowie die Anzahl von betroffenen Ressourcen (5) anzeigen. Einige Kontrollen sind abgeblendet. Diesen Kontrollen sind keine Security Center-Bewertungen zugeordnet. Überprüfen Sie die Anforderungen hierfür, und führen Sie die Bewertung in Ihrer Umgebung selbst durch. Einige sind möglicherweise prozessbezogen und nicht technischer Art.

    :::image type="content" source="./media/security-center-compliance-dashboard/compliance-drilldown.png" alt-text="Erkunden der Compliancedetails für einen bestimmten Standard":::

1. Wählen Sie **Bericht herunterladen** aus, um einen PDF-Bericht mit einer Zusammenfassung Ihres aktuellen Compliancestatus für einen bestimmten Standard zu generieren.

    Der Bericht enthält eine allgemeine Zusammenfassung des Compliancestatus für den ausgewählten Standard auf der Grundlage von Security Center-Bewertungsdaten. Der Bericht ist nach den Kontrollen dieses bestimmten Standards gegliedert. Der Bericht kann an Stakeholder weitergegeben und als Nachweis bei internen und externen Prüfungen genutzt werden.

    :::image type="content" source="./media/security-center-compliance-dashboard/download-report.png" alt-text="Herunterladen des Berichts zur Compliance":::

## <a name="improve-your-compliance-posture"></a>Verbessern Ihres Konformitätsstatus

Mit den Informationen im Dashboard für die Einhaltung gesetzlicher Bestimmungen können Sie Ihren Konformitätsstatus verbessern, indem Sie direkt im Dashboard Lösungen für Empfehlungen finden.

1.  Klicken Sie durch die nicht bestandenen Bewertungen, die im Dashboard angezeigt werden, um jeweils die Details zu einer Empfehlung anzuzeigen. Jede Empfehlung enthält verschiedene Lösungsschritte, die ausgeführt werden müssen, um das Problem zu beheben.

1.  Sie können eine bestimmte Ressource auswählen, um weitere Details anzuzeigen und die Empfehlung für diese Ressource zu lösen. <br>Im Standard **Azure CIS 1.1.0** können Sie beispielsweise die Empfehlung **Datenträgerverschlüsselung sollte auf virtuelle Computer angewendet werden** auswählen.

    :::image type="content" source="./media/security-center-compliance-dashboard/sample-recommendation.png" alt-text="Auswahl einer Empfehlung aus einem Standard führt direkt zur Seite mit den Details der Empfehlung":::

1. Wenn Sie in diesem Beispiel auf der Seite mit den Details zur Empfehlung die Option **Aktion ausführen** auswählen, gelangen Sie zu den Azure Virtual Machine-Seiten im Azure-Portal, wo Sie die Registerkarte **Sicherheit** öffnen und die Verschlüsselung aktivieren können:

    :::image type="content" source="./media/security-center-compliance-dashboard/encrypting-vm-disks.png" alt-text="Schaltfläche „Aktion ausführen“ auf der Seite mit den Details zur Empfehlung führt zu den Lösungsoptionen":::

    Weitere Informationen zur Anwendung der Empfehlungen finden Sie unter [Implementieren von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md).

1.  Nachdem Sie Lösungsmaßnahmen für die Empfehlungen ergriffen haben, sind die Auswirkungen im Bericht im Dashboard zur Einhaltung gesetzlicher Bestimmungen sichtbar, weil sich Ihre Compliancebewertung verbessert hat.

    > [!NOTE]
    > Da die Bewertungen ungefähr alle zwölf Stunden durchgeführt werden, sehen die Auswirkungen auf Ihre Konformitätsdaten jeweils erst nach der nächsten Ausführung der relevanten Bewertung.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde beschrieben, wie Sie das Security Center-Dashboard zur Einhaltung gesetzlicher Bestimmungen für folgende Zwecke verwenden:

-   Anzeigen und Überwachen Ihres Konformitätsstatus basierend auf den Standards und Bestimmungen, die für Sie wichtig sind
-   Verbessern Ihres Konformitätsstatus durch das Lösen relevanter Empfehlungen und Verfolgen der verbesserten Compliancebewertung

Mit dem Dashboard zur Einhaltung gesetzlicher Bestimmungen kann der Konformitätsprozess stark vereinfacht werden. Darüber hinaus kann eine erhebliche Verkürzung des Zeitraums erzielt werden, der für das Sammeln von Konformitätsnachweisen für Ihre Azure- und Hybridumgebung erforderlich ist.

Weitere Informationen finden Sie in den folgenden verwandten Artikeln:

-   [Aktualisieren auf dynamische Compliancepakete in Ihrem Dashboard für die Einhaltung gesetzlicher Bestimmungen (Vorschau)](update-regulatory-compliance-packages.md): Informieren Sie sich über diese neue Previewfunktion, mit der Sie die Standards, die in Ihrem Dashboard für die Einhaltung gesetzlicher Bestimmungen angezeigt werden, auf die neuen *dynamischen* Pakete aktualisieren können. Dieselbe Previewfunktion können Sie auch nutzen, um neue Compliancepakete hinzuzufügen und Ihre Compliance mit zusätzlichen Standards zu überwachen. 
-   [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
-   [Verwalten von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md): Es wird beschrieben, wie Sie Empfehlungen in Azure Security Center nutzen, um Ihre Azure-Ressourcen zu schützen.
-   [Verbessern des Secure Score in Azure Security Center](secure-score-security-controls.md): Es wird beschrieben, wie Sie Sicherheitsrisiken und -empfehlungen priorisieren, um Ihren Sicherheitsstatus bestmöglich zu verbessern.
