---
title: Veröffentlichen eines Angebots für verwaltete Dienste im Azure Marketplace
description: Erfahren Sie, wie Sie ein Angebot für verwaltete Dienste veröffentlichen, das das Onboarding von Kunden in Azure Lighthouse durchführt.
ms.date: 05/04/2020
ms.topic: how-to
ms.openlocfilehash: 19364164617a32a561140e985c8723f8deafe1a7
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86133317"
---
# <a name="publish-a-managed-service-offer-to-azure-marketplace"></a>Veröffentlichen eines Angebots für verwaltete Dienste im Azure Marketplace

In diesem Artikel erfahren Sie, wie Sie ein öffentliches oder privates Angebot für verwaltete Dienste in [Azure Marketplace](https://azuremarketplace.microsoft.com) unter Verwendung des Programms [Kommerzieller Marktplatz](../../marketplace/partner-center-portal/commercial-marketplace-overview.md) in Partner Center veröffentlichen können. Kunden, die das Angebot erwerben, delegieren dann Abonnements oder Ressourcengruppen, die Sie dann über [Azure Lighthouse](../overview.md) verwalten können.

## <a name="publishing-requirements"></a>Veröffentlichungsbedingungen

Sie benötigen ein gültiges [Konto in Partner Center](../../marketplace/partner-center-portal/create-account.md), um Angebote zu erstellen und zu veröffentlichen. Wenn Sie noch nicht über ein Konto verfügen, führt Sie der [Registrierungsvorgang](https://aka.ms/joinmarketplace) durch die Schritte zum Erstellen eines Kontos in Partner Center und die Registrierung beim kommerziellen Marketplace-Programm.

Gemäß den [Zertifizierungsanforderungen für Angebote für verwaltete Dienste](/legal/marketplace/certification-policies#7004-business-requirements) müssen Sie über die [Cloudplattform-Kompetenzebene Silver oder Gold](/partner-center/learn-about-competencies) verfügen oder ein [Azure Expert MSP](https://partner.microsoft.com/membership/azure-expert-msp) sein, um ein Angebot für verwaltete Dienste zu veröffentlichen.

Ihre MPN-ID (Microsoft Partner Network) den von Ihnen veröffentlichten Angeboten [automatisch zugeordnet](../../cost-management-billing/manage/link-partner-id.md), um ihre Wirksamkeit hinsichtlich der Kundenbindung zu verfolgen.

> [!NOTE]
> Wenn Sie ein Angebot nicht im Azure Marketplace veröffentlichen möchten, können Sie das Onboarding von Kunden mithilfe von Azure Resource Manager-Vorlagen manuell durchführen. Weitere Informationen finden Sie unter [Onboarding eines Kunden in Azure Lighthouse durchführen](onboard-customer.md).

## <a name="create-your-offer"></a>Erstellen Ihres Angebots

Ausführliche Anweisungen zum Erstellen Ihres Angebots, einschließlich aller Informationen und Ressourcen, die Sie bereitstellen müssen, finden Sie unter [Erstellen eines Angebots für einen verwalteten Dienst](../../marketplace/partner-center-portal/create-new-managed-service-offer.md).

Informationen zum allgemeinen Veröffentlichungsprozess finden Sie im [Veröffentlichungsleitfaden für Azure Marketplace und AppSource](../../marketplace/marketplace-publishers-guide.md). Sehen Sie sich darüber hinaus die Informationen zu [Zertifizierungsrichtlinien für den kommerziellen Marketplace](/legal/marketplace/certification-policies) (insbesondere den Abschnitt [Managed Services](/legal/marketplace/certification-policies#700-managed-services) (Verwaltete Dienste)) an.

Wenn ein Kunde Ihr Angebot hinzufügt, kann er ein oder mehrere Abonnements oder Ressourcengruppen delegieren, für die dann [das Onboarding in Azure Lighthouse durchgeführt wird](#the-customer-onboarding-process).

> [!IMPORTANT]
> Jeder Plan in einem Angebot für verwaltete Dienste enthält einen Abschnitt mit **Manifestdetails**, in dem Sie die Azure Active Directory-Entitäten (Azure AD) in Ihrem Mandanten definieren, die Zugriff auf die delegierten Ressourcengruppen und/oder Abonnements für Kunden erhalten sollen, die diesen Plan kaufen. Es ist wichtig, zu bedenken, dass jede Gruppe (bzw. jeder Benutzer oder Dienstprinzipal), die Sie einschließen, über dieselben Berechtigungen für jeden Kunden verfügt, der den Plan kauft. Damit Sie verschiedene Gruppen für die Arbeit mit den einzelnen Kunden zuweisen können, müssen Sie einen gesonderten [privaten Plan](../../marketplace/private-offers.md) veröffentlichen, der jeweils exklusiv für die einzelnen Kunden ist.

## <a name="publish-your-offer"></a>Veröffentlichen Ihres Angebots

Nach der Vervollständigung aller Abschnitte besteht Ihr nächster Schritt darin, das Angebot im Azure Marketplace zu veröffentlichen. Wählen Sie die Schaltfläche **Veröffentlichen** aus, um den Vorgang zu starten, mit dem Ihr Angebot live geschaltet wird. Weitere Informationen zu diesem Prozess finden Sie [hier](../../marketplace/partner-center-portal/create-new-managed-service-offer.md#publish). 

Sie können jederzeit [eine aktualisierte Version Ihres Angebots veröffentlichen](../..//marketplace/partner-center-portal/update-existing-offer.md). Beispielsweise können Sie einem zuvor veröffentlichten Angebot eine neue Rollendefinition hinzufügen. Kunden, die das Angebot bereits hinzugefügt haben, wird dann auf der Seite [**Dienstanbieter**](view-manage-service-providers.md) im Azure-Portal ein Symbol angezeigt, das darauf hinweist, dass eine Aktualisierung verfügbar ist. Jeder Kunde kann die [Änderungen überprüfen](view-manage-service-providers.md#update-service-provider-offers) und entscheiden, ob er auf die neue Version aktualisieren möchte. 

## <a name="the-customer-onboarding-process"></a>Kundenonboarding

Sobald ein Kunde Ihr Angebot hinzufügt hat, kann er [ein oder mehrere spezifische Abonnements oder Ressourcengruppen delegieren](view-manage-service-providers.md#delegate-resources), für die dann das Onboarding in Azure Lighthouse durchgeführt wird. Wenn ein Kunde ein Angebot angenommen, aber noch keine Ressourcen delegiert hat, wird im Azure-Portal auf der Seite [**Dienstanbieter**](view-manage-service-providers.md) am oberen Rand des Abschnitts **Anbieterangebote** ein Hinweis angezeigt.

> [!IMPORTANT]
> Die Delegierung muss von einem Konto im Mandanten des Kunden durchgeführt werden, bei dem es sich nicht um ein Gastkonto handelt und das für das Abonnement, für das das Onboarding durchgeführt wird (oder das die Ressourcengruppen enthält, für die das Onboarding durchgeführt wird), über die [integrierte Rolle „Besitzer“](../../role-based-access-control/built-in-roles.md#owner) verfügt. Um alle Benutzer anzuzeigen, die das Abonnement delegieren können, kann ein Benutzer im Mandanten des Kunden das Abonnement im Azure-Portal auswählen, **Zugriffssteuerung (IAM)** öffnen und [alle Benutzer mit der Rolle „Besitzer“ anzeigen](../../role-based-access-control/role-assignments-list-portal.md#list-owners-of-a-subscription).

Wenn der Kunde ein Abonnement (oder eine oder mehrere Ressourcengruppen innerhalb eines Abonnements) delegiert, wird der Ressourcenanbieter **Microsoft.ManagedServices** für dieses Abonnement registriert, und Benutzer in Ihrem Mandanten können gemäß den Autorisierungen in Ihrem Angebot auf die delegierten Ressourcen zugreifen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über den [kommerziellen Marketplace](../../marketplace/partner-center-portal/commercial-marketplace-overview.md).
- Erfahren Sie über [Mandantenübergreifende Verwaltungsmöglichkeiten](../concepts/cross-tenant-management-experience.md).
- [Anzeigen und Verwalten von Kunden](view-manage-customers.md), indem sie im Azure-Portal zu **Meine Kunden** navigieren.
