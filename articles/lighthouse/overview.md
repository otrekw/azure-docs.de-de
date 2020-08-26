---
title: Was ist Azure Lighthouse?
description: Mit Azure Lighthouse können Dienstanbieter verwaltete Dienste für Ihre Kunden bereitstellen, bei denen bedarfsorientiert eine höhere Automatisierung und Effizienz erzielt werden können.
ms.date: 08/19/2020
ms.topic: overview
ms.openlocfilehash: d21d11d1316b177f38b6bce197dec4f2c3d3fac8
ms.sourcegitcommit: d18a59b2efff67934650f6ad3a2e1fe9f8269f21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88660732"
---
# <a name="what-is-azure-lighthouse"></a>Was ist Azure Lighthouse?

Azure Lighthouse ermöglicht die mandantenübergreifende Verwaltung sowie die Verwaltung mehrerer Mandanten mit stärkerer Automatisierung, besserer Skalierbarkeit und verbesserter Governance für alle Ressourcen und Mandanten. Mit Azure Lighthouse können Dienstanbieter verwaltete Dienste mithilfe umfassender und stabiler Verwaltungstools bereitstellen, die in die Azure-Plattform integriert sind. Dieses Angebot kann auch IT-Abteilungen von Unternehmen zugutekommen, die Ressourcen in mehreren Mandanten verwalten.

![Übersichtsdiagramm von Azure Lighthouse](media/azure-lighthouse-overview.jpg)

## <a name="benefits"></a>Vorteile

Azure Lighthouse hilft Ihnen bei der profitablen und effizienten Erstellung und Bereitstellung verwalteter Dienste. Es ergeben sich folgende Vorteile:

- **Bedarfsorientierte Verwaltung**: Kundenbindungs- und Lebenszyklusvorgänge zur Verwaltung von Kundenressourcen sind einfacher und besser skalierbar. Vorhandene APIs, Verwaltungstools und Workflows können mit delegierten Ressourcen verwendet werden, unabhängig von den Regionen, in denen sie sich befinden.
- **Besserer Einblick und höhere Genauigkeit für Kunden**: Kunden haben einen besseren Einblick in Ihre Aktionen und eine genauere Kontrolle über den Bereich, den sie zur Verwaltung delegieren, und können beispielsweise den Zugriff vollständig entfernen. Ihre IP-Adresse bleibt dabei erhalten.
- **Umfassende und einheitliche Plattformtools**: Unsere Toolerfahrung ist auf wichtige Dienstanbieterszenarien ausgelegt, einschließlich mehrerer Lizenzierungsmodelle wie EA (Enterprise Agreement), CSP (Cloud-Lösungsanbieter) und nutzungsbasierte Bezahlung. Die neuen Funktionen arbeiten mit vorhandenen Tools und APIs, Lizenzierungsmodellen und Partnerprogrammen wie dem [Programm für Cloud-Lösungsanbieter](/partner-center/csp-overview). Azure Lighthouse lässt sich in Ihre vorhandenen Workflows und Anwendungen integrieren, und Sie können Ihre Wirksamkeit auf die Kundenbindung nachverfolgen, indem Sie [Ihre Partner-ID verknüpfen](../cost-management-billing/manage/link-partner-id.md).

Es fallen keine zusätzlichen Kosten für die Verwendung von Azure Lighthouse zur Verwaltung von Azure-Ressourcen an. Jeder Azure-Kunde oder -Partner kann Azure Lighthouse verwenden.

## <a name="capabilities"></a>Funktionen

Azure Lighthouse umfasst mehrere Möglichkeiten zur Optimierung von Kundenbindung und Verwaltung:

- **Delegierte Azure-Ressourcenverwaltung**: Verwalten Sie die Azure-Ressourcen Ihrer Kunden sicher aus Ihrem eigenen Mandanten heraus, ohne den Kontext oder Steuerungsebenen wechseln zu müssen. Abonnements und Ressourcengruppen können an bestimmte Benutzer und Rollen im Verwaltungsmandanten delegiert werden, und es besteht die Möglichkeit, den Zugriff bei Bedarf zu entfernen. Weitere Informationen finden Sie unter [Delegierte Azure-Ressourcenverwaltung](concepts/azure-delegated-resource-management.md).
- **Neue Azure-Portalerfahrung**: Zeigen Sie mandantenübergreifende Informationen auf der [Seite **Meine Kunden** ](./how-to/view-manage-customers.md) im Azure-Portal an. Auf einer entsprechenden [Seite **Dienstanbieter**](how-to/view-manage-service-providers.md) können Kunden den Dienstanbieterzugriff anzeigen und verwalten.
- **Azure Resource Manager-Vorlagen**: Unsere Vorlagen helfen Ihnen, mandantenübergreifende Verwaltungsaufgaben auszuführen und delegierte Kundenressourcen zu integrieren. Weitere Informationen finden Sie in unserem [Beispielrepository](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates) und unter [Onboarding eines Kunden in Azure Lighthouse durchführen](how-to/onboard-customer.md).
- **Angebote für verwaltete Dienste im Azure Marketplace:** Bieten Sie Ihre Dienste Kunden durch private oder öffentliche Angebote an, und lassen Sie sie automatisch in Azure Lighthouse integrieren. Weitere Informationen finden Sie unter [Angebote für verwaltete Dienste in Azure Marketplace](concepts/managed-services-offers.md).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur [delegierten Azure-Ressourcenverwaltung](concepts/azure-delegated-resource-management.md)
- Erfahren Sie mehr über [mandantenübergreifende Verwaltungsmöglichkeiten](concepts/cross-tenant-management-experience.md).
- Informieren Sie sich über die [Verwendung von Azure Lighthouse in einem Unternehmen](concepts/enterprise.md).
