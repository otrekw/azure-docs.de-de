---
title: Vergleichen von Externe Identitäten – Azure Active Directory | Microsoft-Dokumentation
description: Mit Externe Azure AD-Identitäten können Personen außerhalb Ihrer Organisation auf Ihre Apps und Ressourcen unter Verwendung ihrer eigenen Identität zugreifen. Vergleichen Sie Lösungen für Externe Identitäten, einschließlich Azure Active Directory B2B-Zusammenarbeit und Azure AD B2C.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: overview
ms.date: 08/05/2020
ms.custom: project-no-code
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 08623e6d69ac4db1790c1e9b46089f0c72c0526d
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87910503"
---
# <a name="what-are-external-identities-in-azure-active-directory"></a>Worum handelt es sich bei „Externe Identitäten“ in Azure Active Directory?

Mit „Externe Identitäten“ in Azure AD können Sie Personen außerhalb Ihrer Organisation den Zugriff auf Ihre Apps und Ressourcen erlauben. Gleichzeitig können sie sich mit der von ihnen bevorzugten Identität anmelden. Ihre Partner, Vertriebspartner, Lieferanten, Anbieter und andere Gastbenutzer können „ihre eigenen Identitäten einbringen“. Unabhängig davon, ob sie zu Azure AD oder einem anderen von der IT verwalteten System gehören oder aber über eine nicht verwaltete soziale Identität wie Google oder Facebook verfügen, können sie ihre eigenen Anmeldeinformationen für die Anmeldung verwenden. Der Identitätsanbieter verwaltet die Identität des externen Benutzers, und Sie verwalten den Zugriff auf Ihre Apps mit Azure AD, um Ihre Ressourcen zu schützen. 

## <a name="external-identities-scenarios"></a>Szenarien für Externe Identitäten

Externe Azure AD-Identitäten konzentriert sich weniger auf die Beziehung eines Benutzers zu Ihrer Organisation als auf die Art und Weise, wie sich eine Person bei ihren Apps und Ressourcen anmelden möchte. Innerhalb dieses Frameworks unterstützt Azure AD eine Vielzahl von Szenarien – von der B2B-Zusammenarbeit (Business-to-Business) bis hin zur App-Entwicklung für Kunden und Consumer (Business-to-Consumer oder B2C).

- **Freigeben von Apps für externe Benutzer (B2B-Zusammenarbeit)** . Laden Sie externe Benutzer in Ihren eigenen Mandanten als „Gastbenutzer“ ein, denen Sie Berechtigungen (zur Autorisierung) zuweisen können. Gleichzeitig erlauben Sie ihnen, ihre vorhandenen Anmeldeinformationen (zur Authentifizierung) zu verwenden. Benutzer melden sich über einen einfachen Einladungs- und Einlösungsprozess mit ihrem Geschäfts-, Schul- oder Unikonto oder aber einem beliebigen E-Mail-Konto bei den freigegebenen Ressourcen an. Mit der Verfügbarkeit von Benutzerflows für die Self-Service-Registrierung (Vorschau) können Sie jetzt auch einen Anmeldevorgang für Ihre externen Benutzer über die Anwendung bereitstellen, die Sie freigeben möchten. Sie können Benutzerfloweinstellungen konfigurieren, um zu steuern, wie sich Benutzer für die Anwendung registrieren. Dies ermöglicht es ihnen, ihr Geschäfts-, Schul- oder Unikonto oder aber eine beliebige soziale Identität (wie Google oder Facebook) ihrer Wahl zu nutzen.  Weitere Informationen finden Sie in der [Azure AD B2B-Dokumentation](index.yml).

- **Entwickeln Sie Apps, die für andere Azure AD-Mandanten (einzelner Mandant oder mehrere Mandanten) vorgesehen sind**. Wenn Sie Anwendungen für Azure AD entwickeln, können Sie Benutzer aus einer einzelnen Organisation (einzelner Mandant) oder Benutzer aus einer Organisation, die bereits über einen Azure AD Mandanten verfügt (die so genannten „mehrinstanzenfähigen Anwendungen“), als Ziel festlegen. Diese mehrinstanzenfähigen Anwendungen werden einmal von Ihnen selbst in Ihrem eigenen Azure AD registriert, können aber von jedem beliebigen Azure AD Benutzer aus jeder beliebigen Organisation eingesetzt werden, ohne dass eine zusätzliche Aktion Ihrerseits erforderlich ist.

- **Entwickeln Sie White-Label-Apps für Consumer und Kunden (Azure AD B2C)** . Wenn Sie ein Unternehmen oder Entwickler sind, das bzw. der kundenorientierte Apps erstellt, können Sie sie mithilfe einer Azure AD B2C-App auf Consumer, Kunden oder Bürger skalieren. Entwickler können Azure AD als voll funktionsfähiges Identitätssystem für ihre Anwendung nutzen. Gleichzeitig können sich Kunden mit einer bereits vorhandenen Identität (wie Facebook oder Gmail) anmelden. Mit Azure AD B2C können Sie vollständig anpassen und steuern, wie sich Kunden bei Nutzung Ihrer Anwendungen registrieren, anmelden und ihre Profile verwalten. Weitere Informationen finden Sie in der [Azure AD B2C-Dokumentation](https://docs.microsoft.com/azure/active-directory-b2c/).

## <a name="compare-external-identities-solutions"></a>Vergleichen von Lösungen für externe Identitäten

Die nachstehende Tabelle enthält einen detaillierten Vergleich der verschiedenen Szenarien, die Sie mit Externe Azure AD-Identitäten aktivieren können.

| Mehrinstanzenfähige Anwendungen  | Zusammenarbeit mit externem Benutzer (B2B) | Apps für Consumer oder Kunden (B2C)  |
| ---- | --- | --- |
| Primäres Szenario: Enterprise Software-as-a-Service (SaaS) | Primäres Szenario: Zusammenarbeit mithilfe von Microsoft-Anwendungen (Microsoft 365, Teams usw.) oder Ihrer eigenen Software für die Zusammenarbeit.  | Primäres Szenario: Transaktionsanwendungen mit benutzerdefiniert entwickelten Anwendungen.   |
| Vorgesehen für: Organisationen, die Software für viele Unternehmenskunden bereitstellen möchten.    | Vorgesehen für: Organisationen, die Benutzer einer Partnerorganisation unabhängig vom Identitätsanbieter authentifizieren möchten.    | Vorgesehen für: Einladung von Kunden Ihrer mobilen Apps und Web-Apps (Einzelpersonen, Institutionen oder Organisationen) in ein Azure AD Verzeichnis, das vom Verzeichnis Ihrer Organisation getrennt ist. |
| Unterstützte Identitäten: Mitarbeiter mit Azure AD-Konten. | Unterstützte Identitäten: Mitarbeiter mit Geschäfts-, Schul- oder Unikonto, Partner mit Geschäfts-, Schul- oder Unikonto oder beliebige E-Mail-Adressen. Direkter Verbund wird in Kürze unterstützt.      | Unterstützte Identitäten: Endbenutzer mit lokalem Anwendungskonto (E-Mail-Adresse oder Benutzername) oder eine beliebige unterstützte soziale Identität mit direktem Verbund.       |
| Externe Benutzer werden in einem eigenen Verzeichnis verwaltet, das isoliert ist von dem Verzeichnis, in dem die Anwendung registriert wurde.    | Externe Benutzer werden im gleichen Verzeichnis verwaltet wie Mitarbeiter, aber speziell gekennzeichnet. Sie können auf dieselbe Weise wie Mitarbeiter verwaltet, denselben Gruppen hinzugefügt werden usw.    | Externe Benutzer werden im Anwendungsverzeichnis verwaltet. Sie werden getrennt vom Verzeichnis für Mitarbeiter und Partner (sofern vorhanden) der Organisation verwaltet.  |
| Einmaliges Anmelden: Einmaliges Anmelden (Single Sign-On, SSO) bei allen mit Azure AD verbundenen Apps wird unterstützt.          | Einmaliges Anmelden: Einmaliges Anmelden (Single Sign-On, SSO) bei allen mit Azure AD verbundenen Apps wird unterstützt. Beispielsweise können Sie den Zugriff auf Microsoft 365 oder lokale Apps und auf andere SaaS-Apps wie z. B. Salesforce oder Workday bereitstellen.    | Einmaliges Anmelden: Einmaliges Anmelden bei Kunden-Apps innerhalb der Azure AD B2C-Mandanten wird unterstützt. Einmaliges Anmelden bei Microsoft 365 oder anderen SaaS-Apps von Microsoft wird nicht unterstützt.    |
| Kundenlebenszyklus: Wird von der eigenen Organisation des Benutzers verwaltet.      | Partnerlebenszyklus: Wird von der Hostorganisation/einladenden Organisation verwaltet.    | Kundenlebenszyklus: Self-Service oder von der Anwendung verwaltet.      |
| Sicherheitsrichtlinie und Compliance: Wird von der Hostorganisation/einladenden Organisation verwaltet (beispielsweise mit [Richtlinien für den bedingten Zugriff](https://docs.microsoft.com/azure/active-directory/b2b/conditional-access)).           | Sicherheitsrichtlinie und Compliance: Wird von der Hostorganisation/einladenden Organisation verwaltet (beispielsweise mit [Richtlinien für den bedingten Zugriff](https://docs.microsoft.com/azure/active-directory/b2b/conditional-access)). | Sicherheitsrichtlinie und Compliance: Von der Anwendung verwaltet.        |
| Branding: Das Branding der Hostorganisation/einladenden Organisation wird verwendet.   | Branding: Das Branding der Hostorganisation/einladenden Organisation wird verwendet.    | Branding: Von der Anwendung verwaltet. Normalerweise wird eher das Produktbranding verwendet, und die Organisation tritt in den Hintergrund.   |
| Weitere Informationen: [Verwalten der Identität in mehrinstanzenfähigen Anwendungen](https://docs.microsoft.com/azure/architecture/multitenant-identity/) [Schrittanleitung](https://docs.microsoft.com/azure/active-directory/develop/howto-convert-app-to-be-multi-tenant) | Weitere Informationen: [Blogbeitrag](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [Dokumentation](what-is-b2b.md)                   | Weitere Informationen: [Produktseite](https://azure.microsoft.com/services/active-directory-b2c/), [Dokumentation](https://docs.microsoft.com/azure/active-directory-b2c/)       |

Sichern und verwalten Sie Kunden und Partner über die Grenzen Ihrer Organisation hinaus mit Externe Azure AD-Identitäten.

## <a name="next-steps"></a>Nächste Schritte

- [Was ist die Azure AD B2B-Zusammenarbeit?](what-is-b2b.md)
- [Informationen zu Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/overview)
