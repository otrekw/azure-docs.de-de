---
title: Mandantenübergreifende Verwaltung in Azure Security Center | Microsoft-Dokumentation
description: Erfahren Sie, wie mandantenübergreifende Verwaltung für den Sicherheitsstatus mehrerer Mandanten in Security Center unter Verwendung delegierter Azure-Ressourcenverwaltung eingerichtet wird.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 7d51291a-4b00-4e68-b872-0808b60e6d9c
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2019
ms.author: memildin
ms.openlocfilehash: 5c654610df8c00e1e80382687bd30dca0c02b69f
ms.sourcegitcommit: b5d59c6710046cf105236a6bb88954033bd9111b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2019
ms.locfileid: "74558952"
---
# <a name="cross-tenant-management-in-security-center"></a>Mandantenübergreifende Verwaltung in Security Center

Mithilfe der mandantenübergreifenden Verwaltung können Sie den Sicherheitsstatus mehrerer Mandanten in Security Center unter Verwendung der [delegierten Azure-Ressourcenverwaltung](../lighthouse/concepts/azure-delegated-resource-management.md) anzeigen und verwalten. Verwalten Sie effizient mehrere Mandanten aus einer einzigen Ansicht heraus, ohne sich bei den einzelnen Mandantenverzeichnissen anmelden zu müssen.

- Dienstanbieter können den Sicherheitsstatus von Ressourcen für mehrere Kunden innerhalb ihres eigenen Mandanten verwalten.

- Sicherheitsteams von Organisationen mit mehreren Mandanten können ihren Sicherheitsstatus von einem einzigen Standort aus anzeigen und verwalten.

## <a name="set-up-cross-tenant-management"></a>Einrichten der mandantenübergreifenden Verwaltung

Richten Sie die mandantenübergreifende Verwaltung ein, indem Sie den Zugriff auf Ressourcen verwalteter Mandanten mithilfe der [delegierten Azure-Ressourcenverwaltung](../lighthouse/concepts/azure-delegated-resource-management.md) an Ihren eigenen Mandanten delegieren.

> [!NOTE]
> Die delegierte Azure-Ressourcenverwaltung ist eine der Hauptkomponenten von Azure Lighthouse.

## <a name="how-does-cross-tenant-management-work-in-security-center"></a>Funktionsweise der mandantenübergreifenden Verwaltung in Azure Security Center

Sie können Abonnements für mehrere Mandanten auf die gleiche Weise überprüfen und verwalten, wie Sie mehrere Abonnements auf einem einzelnen Mandanten verwalten.

Klicken Sie in der oberen Menüleiste auf das Filtersymbol, und wählen Sie aus den Verzeichnissen der einzelnen Mandanten die Abonnements aus, die Sie anzeigen möchten.

  ![Filtern von Mandanten](./media/security-center-cross-tenant-management/cross-tenant-filter.png)

Die Ansichten und Aktionen sind im Grunde identisch. Hier einige Beispiele:

- **Verwalten von Sicherheitsrichtlinien**: Verwalten Sie in einer einzigen Ansicht den Sicherheitsstatus vieler Ressourcen mit [Richtlinien](tutorial-security-policy.md), ergreifen Sie Maßnahmen mit Sicherheitsempfehlungen, und erfassen und verwalten Sie sicherheitsrelevante Daten.
- **Verbessern von Secure Score und Konformitätsstatus**: Mithilfe der mandantenübergreifenden Sichtbarkeit können Sie den gesamten Sicherheitsstatus all Ihrer Mandanten anzeigen und ermitteln, wo und wie Sie die [Sicherheitsbewertung](security-center-secure-score.md) und den [Compliancestatus](security-center-compliance-dashboard.md) für jeden einzelnen Mandanten optimieren können.
- **Umsetzen von Empfehlungen**: Sie können eine [Empfehlung](security-center-recommendations.md) für viele Ressourcen von verschiedenen Mandanten gleichzeitig überwachen und umsetzen. Anschließend können Sie die Sicherheitslücken, die das höchste Risiko darstellen, sofort für alle Mandanten beseitigen.
- **Verwalten von Warnungen**: Ermitteln Sie [Warnungen](security-center-alerts-overview.md) auf den verschiedenen Mandanten. Ergreifen Sie Maßnahmen für Ressourcen, die nicht mit handlungsrelevanten [Schritten zur Bereinigung](security-center-managing-and-responding-alerts.md) konform sind.

- **Verwalten erweiterter Cloudschutzfeatures und mehr**: Verwalten Sie die verschiedenen Dienste zur Erkennung von und zum Schutz vor Bedrohungen, z.B. [Just-In-Time (JIT)-VM-Zugriff](security-center-just-in-time.md), [adaptive Netzwerkhärtung](security-center-adaptive-network-hardening.md), [adaptive Anwendungssteuerung](security-center-adaptive-application.md) und mehr.
 
## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wird erläutert, wie die mandantenübergreifende Verwaltung in Azure Security Center funktioniert. Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Erhöhen Ihres Sicherheitsstatus in Azure Security Center](security-center-monitoring.md): Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Weitere Informationen zu Azure Lighthouse in Unternehmensszenarien](https://docs.microsoft.com/azure/lighthouse/concepts/enterprise): Erfahren Sie, wie Azure Lighthouse die mandantenübergreifenden Verwaltung in einem Unternehmen, in dem mehrere Azure AD-Mandanten vorhanden sind, vereinfachen kann.