---
title: 'Self-Service-Registrierungsportal für B2B-Zusammenarbeit: Azure AD'
description: Die Azure Active Directory B2B-Zusammenarbeit ermöglicht Geschäftspartnern den gezielten Zugriff auf Ihre Unternehmensanwendungen und unterstützt so Ihre unternehmensübergreifenden Beziehungen.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: ff18d3d9cae6e887abbd9fb1845de62386062b2b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85386706"
---
# <a name="self-service-for-azure-ad-b2b-collaboration-sign-up"></a>Self-Service-Registrierung für Azure AD B2B Collaboration

Kunden können mit den integrierten Features, die über das [Azure-Portal](https://portal.azure.com) und den [Anwendungszugriffsbereich](https://myapps.microsoft.com) für Endbenutzer verfügbar gemacht werden, viele Aufgaben erledigen. Unter Umständen müssen Sie jedoch den Onboarding-Workflow für B2B-Benutzer an die Bedürfnisse ihrer Organisation anpassen.

## <a name="azure-ad-entitlement-management-for-b2b-guest-user-sign-up"></a>Azure AD-Berechtigungsverwaltung für die B2B-Gastbenutzerregistrierung

Als einladende Organisation wissen Sie möglicherweise im Vorfeld nicht, welche externen Mitarbeiter Zugriff auf Ihre Ressourcen benötigen. Benutzer von Partnerunternehmen müssen sich selbstständig registrieren können, und die Registrierung muss Richtlinien unterliegen, die von Ihnen gesteuert werden. Konfigurieren Sie mithilfe der [Azure AD-Berechtigungsverwaltung](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview) Richtlinien zum [Verwalten des Zugriffs für externe Benutzer](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-external-users#how-access-works-for-external-users), damit Benutzer aus anderen Organisationen Zugriff anfordern und nach erfolgter Genehmigung Gastkonten erhalten und Gruppen, Apps und SharePoint Online-Websites zugewiesen werden können.

## <a name="azure-active-directory-b2b-invitation-api"></a>Einladungs-API von Azure Active Directory B2B

Organisationen können die [Einladungs-Manager-API von Microsoft Graph](https://docs.microsoft.com/graph/api/resources/invitation?view=graph-rest-1.0) verwenden, um ihre eigenen Onboardingumgebungen für B2B-Gastbenutzer zu erstellen. Wenn Sie eine Self-Service-B2B-Gastbenutzerregistrierung anbieten möchten, empfehlen wir die Verwendung der [Azure AD-Berechtigungsverwaltung](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview). Wenn Sie dagegen eine eigene Umgebung erstellen möchten, können Sie die [API zum Erstellen von Einladungen](https://docs.microsoft.com/graph/api/invitation-post?view=graph-rest-1.0&tabs=http) verwenden, um beispielsweise Ihre angepasste Einladungs-E-Mail automatisch direkt an den B2B-Benutzer zu senden. Alternativ kann Ihre App die in der Erstellungsantwort zurückgegebene URL vom Typ „inviteRedeemUrl“ verwenden, um Ihre eigene Einladung (über den von Ihnen bevorzugten Kommunikationsweg) für den eingeladenen Benutzer zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

* [Was ist die Azure AD B2B-Zusammenarbeit?](what-is-b2b.md)
* [Lizenzierung der Azure AD B2B-Zusammenarbeit](licensing-guidance.md)
* [Häufig gestellte Fragen zur Azure Active Directory B2B-Zusammenarbeit](faq.md)
