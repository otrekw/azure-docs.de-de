---
title: Was ist Azure Lighthouse?
description: Mit Azure Lighthouse können Dienstanbieter verwaltete Dienste für Ihre Kunden bereitstellen, bei denen bedarfsorientiert eine höhere Automatisierung und Effizienz erzielt werden können.
ms.date: 05/11/2021
ms.topic: overview
ms.openlocfilehash: 3f8b09584a5f025e22543a65eefd50dbfa7bb894
ms.sourcegitcommit: 32ee8da1440a2d81c49ff25c5922f786e85109b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2021
ms.locfileid: "109785905"
---
# <a name="what-is-azure-lighthouse"></a>Was ist Azure Lighthouse?

Azure Lighthouse ermöglicht die mandantenübergreifende Verwaltung sowie die Verwaltung mehrerer Mandanten mit stärkerer Automatisierung, besserer Skalierbarkeit und verbesserter Governance für alle Ressourcen und Mandanten.

Mit Azure Lighthouse können Dienstanbieter verwaltete Dienste mithilfe umfassender und stabiler Verwaltungstools bereitstellen, die in die Azure-Plattform integriert sind. Kunden behalten die Kontrolle darüber, wer auf ihren Mandanten zugreifen kann, auf welche Ressourcen sie Zugriff haben und welche Aktionen durchgeführt werden können. Von diesem Angebot können auch [IT-Abteilungen von Unternehmen](concepts/enterprise.md) profitieren, die Ressourcen auf mehreren Mandanten verwalten.

Mithilfe der [mandantenübergreifenden Verwaltungsmöglichkeiten](concepts/cross-tenant-management-experience.md) können Sie effizienter mit Azure-Verwaltungsdiensten wie Azure Policy, Azure Security Center und vielem mehr arbeiten. Alle Dienstanbieteraktivitäten werden im Aktivitätsprotokoll nachverfolgt, das im Mandanten des Dienstanbieters gespeichert wird (und von Benutzern im verwaltenden Mandanten angezeigt werden kann). Benutzer können sehen, welche Änderungen von wem vorgenommen wurden.

![Übersichtsdiagramm von Azure Lighthouse](media/azure-lighthouse-overview.jpg)

## <a name="benefits"></a>Vorteile

Azure Lighthouse dient Dienstanbietern beim effizienten Erstellen und Bereitstellen verwalteter Dienste als Hilfe. Dies hat unter anderem folgende Vorteile:

- **Bedarfsorientierte Verwaltung**: Kundenbindungs- und Lebenszyklusvorgänge zur Verwaltung von Kundenressourcen sind einfacher und besser skalierbar. Vorhandene APIs, Verwaltungstools und Workflows können mit delegierten Ressourcen, z. B. auch außerhalb von Azure gehosteten Computern, verwendet werden. Dies gilt unabhängig von den Regionen, in denen sie sich befinden.
- **Bessere Transparenz und Kontrolle für Kunden**: Kunden können präzise steuern, welche Bereiche sie für die Verwaltung delegieren und welche Berechtigungen zulässig sind. Sie können Aktionen von Dienstanbietern überwachen und die Zugriffsmöglichkeit bei Bedarf vollständig entfernen.
- **Umfassende und einheitliche Plattformtools**: Unsere Toolerfahrung ist auf wichtige Dienstanbieterszenarien ausgelegt, einschließlich mehrerer Lizenzierungsmodelle wie EA (Enterprise Agreement), CSP (Cloud-Lösungsanbieter) und nutzungsbasierte Bezahlung. Für Azure Lighthouse können vorhandene Tools und APIs, Lizenzierungsmodelle, [von Azure verwaltete Anwendungen](concepts/managed-applications.md) und Partnerprogramme, z. B. das [Cloud Solution Provider-Programm (CSP)](/partner-center/csp-overview), genutzt werden. Sie können Azure Lighthouse in Ihre vorhandenen Workflows und Anwendungen integrieren und nachverfolgen, welchen Einfluss Sie auf die Kundenbindung haben, indem Sie [Ihre Partner-ID verknüpfen](./how-to/partner-earned-credit.md).

## <a name="capabilities"></a>Funktionen

Azure Lighthouse umfasst mehrere Möglichkeiten zur Optimierung von Kundenbindung und Verwaltung:

- **Delegierte Azure-Ressourcenverwaltung**: [Verwalten Sie die Azure-Ressourcen Ihrer Kunden sicher von Ihrem eigenen Mandanten aus](concepts/architecture.md), ohne den Kontext oder Steuerungsebenen wechseln zu müssen. Kundenabonnements und Ressourcengruppen können an bestimmte Benutzer und Rollen im Verwaltungsmandanten delegiert werden, und es besteht die Möglichkeit, den Zugriff bei Bedarf zu entfernen.
- **Neue Azure-Portalerfahrung**: Zeigen Sie mandantenübergreifende Informationen auf der [Seite **Meine Kunden**](how-to/view-manage-customers.md) im Azure-Portal an. Auf einer entsprechenden [Seite **Dienstanbieter**](how-to/view-manage-service-providers.md) können Kunden den Dienstanbieterzugriff anzeigen und verwalten.
- **Azure Resource Manager-Vorlagen**: Verwenden Sie ARM-Vorlagen zum [Durchführen des Onboardings für delegierte Kundenressourcen](how-to/onboard-customer.md) und [Durchführen von mandantenübergreifenden Verwaltungsaufgaben](samples/index.md).
- **Angebote für verwaltete Dienste im Azure Marketplace:** [Bieten Sie Ihre Dienste Kunden als private oder öffentliche Angebote an](concepts/managed-services-offers.md), und nutzen Sie das automatische Onboarding für Azure Lighthouse.

> [!TIP]
> Ein ähnliches Angebot, [Microsoft 365 Lighthouse](https://techcommunity.microsoft.com/t5/small-and-medium-business-blog/announcing-microsoft-365-lighthouse-for-managed-service/ba-p/1698181), hilft IT-Partnern dabei, Ihre Microsoft 365 Kunden skaliert zu integrieren, zu überwachen und zu verwalten. Microsoft 365 Lighthouse befindet sich derzeit in der privaten Vorschau.

## <a name="pricing-and-availability"></a>Preise und Verfügbarkeit

Es fallen keine zusätzlichen Kosten für die Verwendung von Azure Lighthouse zur Verwaltung von Azure-Ressourcen an. Jeder Azure-Kunde oder -Partner kann Azure Lighthouse verwenden.

## <a name="cross-region-and-cloud-considerations"></a>Überlegungen zu regionsübergreifenden und cloudübergreifenden Szenarien

Azure Lighthouse ist ein nicht regionaler Dienst. Sie können delegierte Ressourcen verwalten, die sich in unterschiedlichen [Regionen](../availability-zones/az-overview.md#regions) befinden. Die Delegierung von Abonnements auf eine [nationale Cloud](../active-directory/develop/authentication-national-cloud.md) und die öffentliche Azure-Cloud oder zwei separate nationale Clouds wird nicht unterstützt.

## <a name="support-for-azure-lighthouse"></a>Unterstützung für Azure Lighthouse

Öffnen Sie eine Supportanfrage im Azure-Portal, um Unterstützung bei der Verwendung von Azure Lighthouse zu erhalten. Wählen Sie als **Problemtyp** **Technisch** aus. Wählen Sie ein Abonnement aus und dann **Lighthouse** (unter **Überwachung und Verwaltung**).

## <a name="next-steps"></a>Nächste Schritte

- Machen Sie sich mit den [technischen Aspekten von Azure Lighthouse](concepts/architecture.md) vertraut.
- Erfahren Sie mehr über [mandantenübergreifende Verwaltungsmöglichkeiten](concepts/cross-tenant-management-experience.md).
- Informieren Sie sich über die [Verwendung von Azure Lighthouse in einem Unternehmen](concepts/enterprise.md).
- Zeigen Sie Details zu [Verfügbarkeit](https://azure.microsoft.com/global-infrastructure/services/?products=azure-lighthouse&regions=all) und [FedRAMP- und DoD CC SRG-Prüfumfang](../azure-government/compliance/azure-services-in-fedramp-auditscope.md) für Azure Lighthouse an.
