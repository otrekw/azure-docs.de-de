---
title: Was ist Azure Lighthouse?
description: Mit Azure Lighthouse können Dienstanbieter verwaltete Dienste für Ihre Kunden bereitstellen, bei denen bedarfsorientiert eine höhere Automatisierung und Effizienz erzielt werden können.
ms.date: 05/28/2020
ms.topic: overview
ms.openlocfilehash: 119f1465c9fdf03779f28a46967c30d899ae37d5
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86131375"
---
# <a name="what-is-azure-lighthouse"></a>Was ist Azure Lighthouse?

Azure Lighthouse bietet Dienstanbietern eine zentrale Steuerungsebene, um Azure für alle ihre Kunden mit höherer Automatisierung, Skalierbarkeit und verbesserter Governance anzuzeigen und zu verwalten. Mit Azure Lighthouse können Dienstanbieter verwaltete Dienste mithilfe umfassender und stabiler Verwaltungstools bereitstellen, die in die Azure-Plattform integriert sind. Dieses Angebot kann auch IT-Abteilungen von Unternehmen zugutekommen, die Ressourcen in mehreren Mandanten verwalten.

![Übersichtsdiagramm von Azure Lighthouse](media/azure-lighthouse-overview.jpg)

## <a name="benefits"></a>Vorteile

Azure Lighthouse hilft Ihnen bei der profitablen und effizienten Erstellung und Bereitstellung verwalteter Dienste für Ihre Kunden. Es ergeben sich folgende Vorteile:

- **Bedarfsorientierte Verwaltung**: Kundenbindungs- und Lebenszyklusvorgänge zur Verwaltung von Kundenressourcen sind einfacher und besser skalierbar. Vorhandene APIs, Verwaltungstools und Workflows können mit delegierten Kundenressourcen verwendet werden, unabhängig von den Regionen, in denen sie sich befinden.
- **Besserer Einblick und höhere Genauigkeit für Kunden**: Kunden haben einen besseren Einblick in Ihre Aktionen und eine genauere Kontrolle über den Bereich, den sie zur Verwaltung delegieren, und können beispielsweise den Zugriff vollständig entfernen. Ihre IP-Adresse bleibt dabei erhalten.
- **Umfassende und einheitliche Plattformtools**: Unsere Toolerfahrung ist auf wichtige Dienstanbieterszenarien ausgelegt, einschließlich mehrerer Lizenzierungsmodelle wie EA (Enterprise Agreement), CSP (Cloud-Lösungsanbieter) und nutzungsbasierte Bezahlung. Die neuen Funktionen arbeiten mit vorhandenen Tools und APIs, Lizenzierungsmodellen und Partnerprogrammen wie dem [Programm für Cloud-Lösungsanbieter](/partner-center/csp-overview). Azure Lighthouse lässt sich in Ihre vorhandenen Workflows und Anwendungen integrieren, und Sie können Ihre Wirksamkeit auf die Kundenbindung nachverfolgen, indem Sie [Ihre Partner-ID verknüpfen](../cost-management-billing/manage/link-partner-id.md).

Es fallen keine zusätzlichen Kosten für die Verwendung von Azure Lighthouse an, um die Azure-Ressourcen ihrer Kunden zu verwalten. Jeder Azure-Kunde oder -Partner kann Azure Lighthouse verwenden.

## <a name="capabilities"></a>Funktionen

Azure Lighthouse umfasst mehrere Möglichkeiten zur Optimierung von Kundenbindung und -verwaltung:

- **Delegierte Azure-Ressourcenverwaltung**: Verwalten Sie die Azure-Ressourcen Ihrer Kunden sicher aus Ihrem eigenen Mandanten heraus, ohne den Kontext oder Steuerungsebenen wechseln zu müssen. Abonnements und Ressourcengruppen können an bestimmte Benutzer und Rollen im Verwaltungsmandanten delegiert werden, und es besteht die Möglichkeit, den Zugriff bei Bedarf zu entfernen. Weitere Informationen finden Sie unter [Delegierte Azure-Ressourcenverwaltung](concepts/azure-delegated-resource-management.md).
- **Neue Azure-Portalerfahrung**: Zeigen Sie mandantenübergreifende Informationen auf der neuen Seite **Meine Kunden** im [Azure-Portal](https://portal.azure.com) an. Auf einem entsprechenden Blatt **Dienstanbieter** können Ihre Kunden den Dienstanbieterzugriff anzeigen und verwalten. Weitere Informationen finden Sie unter [Anzeigen und Verwalten von Kunden](./how-to/view-manage-customers.md) und [Anzeigen und Verwalten von Dienstanbietern](how-to/view-manage-service-providers.md).
- **Azure Resource Manager-Vorlagen**: Führen Sie Verwaltungsaufgaben leichter durch, einschließlich Onboarding von Kunden für die delegierte Azure-Ressourcenverwaltung. Weitere Informationen finden Sie in unserem [Beispielrepository](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates) und unter [Onboarding eines Kunden für die delegierte Azure-Ressourcenverwaltung](how-to/onboard-customer.md).
- **Angebote für verwaltete Dienste im Azure Marketplace:** Bieten Sie Ihre Dienste Kunden durch private oder öffentliche Angebote an, und lassen Sie diese automatisch in die delegierte Azure-Ressourcenverwaltung integrieren, als Alternative zum Onboarding mithilfe von Azure Resource Manager-Vorlagen. Weitere Informationen finden Sie unter [Angebote für verwaltete Dienste in Azure Marketplace](concepts/managed-services-offers.md).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur [delegierten Azure-Ressourcenverwaltung](concepts/azure-delegated-resource-management.md)
- Erfahren Sie über [Mandantenübergreifende Verwaltungsmöglichkeiten](concepts/cross-tenant-management-experience.md).
