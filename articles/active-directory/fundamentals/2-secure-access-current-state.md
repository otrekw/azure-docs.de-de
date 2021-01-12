---
title: Ermitteln des aktuellen Status externer Zusammenarbeit mit Azure Active Directory
description: Hier lernen Sie Methoden kennen, mit denen Sie den aktuellen Status Ihrer Zusammenarbeit ermitteln können.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 12/18/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: ff656887081681a804285e9a96352feef15fc675
ms.sourcegitcommit: 44844a49afe8ed824a6812346f5bad8bc5455030
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/23/2020
ms.locfileid: "97743735"
---
# <a name="discover-the-current-state-of-external-collaboration-in-your-organization"></a>Ermitteln des aktuellen Status der Zusammenarbeit mit externen Benutzern in Ihrer Organisation 

Vor dem Ermitteln des aktuellen Zustands der Zusammenarbeit mit externen Benutzern sollten Sie [den gewünschten Sicherheitsstatus](1-secure-access-posture.md) ermitteln. Sie berücksichtigen die Anforderungen Ihres Unternehmens an zentralisierte und delegierte Steuerung sowie relevante Ziele in Bezug auf Governance, gesetzliche Bestimmungen und Compliance. 

Personen in Ihrer Organisation arbeiten wahrscheinlich bereits mit Benutzern aus anderen Organisationen zusammen. Die Zusammenarbeit kann über Features in Produktivitätsanwendungen (z. B. Microsoft 365), per E-Mail oder mithilfe sonstiger Ressourcen für die Zusammenarbeit mit externen Benutzern erfolgen. Die Säulen Ihres Governanceplans werden deutlich, wenn ... 
*   Benutzer die Zusammenarbeit mit externen Benutzern initiieren.
*   Sie die externen Benutzer und Organisationen, mit denen Sie zusammenarbeiten, besser kennenlernen.
*   Sie den für externe Benutzer gewährten Zugriff festlegen.


## <a name="users-initiating-external-collaboration"></a>Benutzer, die Zusammenarbeit mit externen Benutzern initiieren

Die Benutzer, die die Zusammenarbeit mit externen Benutzern initiieren, wissen am besten, welche Anwendungen für diese Zusammenarbeit am relevantesten sind und welche Zugriffsberechtigungen gewährt werden sollten. Wenn Sie sich in die Lage dieser Benutzer versetzen, finden Sie schnell heraus, wer über delegierte Berechtigungen zum Einladen externer Benutzer, Erstellen von Zugriffspaketen und Durchführen von Zugriffsüberprüfungen verfügen sollte.

Überprüfen Sie das [Microsoft 365-Überwachungsprotokoll für Freigabe- und Zugriffsanforderungsaktivitäten](https://docs.microsoft.com/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance?view=o365-worldwide#sharing-and-access-request-activities), um nach Benutzern zu suchen, die derzeit mit anderen zusammenarbeiten. Sie können sich auch das [Azure AD-Überwachungsprotokoll ansehen, um herauszufinden, wer B2B-Benutzer](../external-identities/auditing-and-reporting.md) in Ihr Verzeichnis eingeladen hat.

## <a name="find-current-collaboration-partners"></a>Suchen nach aktuellen Kollaborationspartnern

Externe Benutzer sind möglicherweise [Azure AD B2B-Benutzer](../external-identities/what-is-b2b.md) (bevorzugt) mit von Partnern verwalteten Anmeldeinformationen oder externe Benutzer mit lokal bereitgestellten Anmeldeinformationen. Diese Benutzer sind in der Regel (aber nicht immer) mit dem UserType „Gast“ gekennzeichnet. Gastbenutzer können über die [Microsoft Graph-API](https://docs.microsoft.com/graph/api/user-list?view=graph-rest-1.0&tabs=http), [PowerShell](https://docs.microsoft.com/graph/api/user-list?view=graph-rest-1.0&tabs=http) oder das [Azure-Portal](../enterprise-users/users-bulk-download.md) aufgelistet werden.

### <a name="use-email-domains-and-companyname-property"></a>Verwenden von E-Mail-Domänen und der companyName-Eigenschaft

Externe Organisationen können durch die Domänennamen der E-Mail-Adressen externer Benutzer bestimmt werden. Wenn Consumeridentitätsanbieter wie Google unterstützt werden, ist dies unter Umständen nicht möglich. In diesem Fall empfiehlt es sich, das companyName-Attribut zu schreiben, um die externe Organisation des Benutzers eindeutig zu identifizieren.

### <a name="use-allow-or-deny-lists"></a>Verwenden von Zulassungs- oder Verweigerungslisten

Eine andere Möglichkeit zum Ermitteln von Personen, mit denen Sie derzeit zusammenarbeiten bzw. für die Sie die Zusammenarbeit blockiert haben, besteht darin, zu überprüfen, ob Sie Ihren [Zulassungs- oder Verweigerungslisten](../external-identities/allow-deny-list.md) eine Organisation hinzugefügt haben.

Angenommen, Ihre Organisation möchte nur die Zusammenarbeit mit bestimmten Organisationen erlauben. Außerdem soll die Kollaboration mit bestimmten Organisationen blockiert werden. Diese Einstellungen können für die gesamte B2B-Einlösung oder nur für ein bestimmtes Zugriffspaket angewendet werden.

![Screenshot: Zulassungs- bzw. Verweigerungsliste beim Erstellen eines neuen Zugriffspakets](media/secure-external-access/2-new-access-package.png)


## <a name="find-access-being-granted-to-external-users"></a>Ermitteln der Zugriffsberechtigungen, die externen Benutzern gewährt werden

Wenn Sie mit mehreren externen Benutzern und Organisationen zusammenarbeiten, können Sie die Zugriffsberechtigungen für diese Benutzer mithilfe der Microsoft Graph-API festlegen, um [Azure AD-Gruppenmitgliedschaften](https://docs.microsoft.com/graph/api/resources/groups-overview?view=graph-rest-1.0) oder[direkte Anwendungszuweisungen](https://docs.microsoft.com/graph/api/resources/approleassignment?view=graph-rest-1.0) in Azure AD zu verwalten.


### <a name="enumerate-application-specific-permissions"></a>Aufzählen anwendungsspezifischer Berechtigungen

Möglicherweise können Sie auch anwendungsspezifische Berechtigungsaufzählungen durchführen. Sie können beispielsweise mithilfe [dieses Skripts](https://gallery.technet.microsoft.com/office/SharePoint-Online-c9ec4f64) programmgesteuert einen Berechtigungsbericht für SharePoint Online erstellen.

Untersuchen Sie insbesondere den Zugriff auf alle geschäfts- und unternehmenskritischen Apps, damit Sie über jeglichen externen Zugriff vollständig Bescheid wissen.

### <a name="detect-ad-hoc-sharing"></a>Erkennen von Ad-hoc-Freigaben
Wenn Ihre E-Mail-Infrastruktur und Ihre Netzwerkpläne es zulassen, können Sie per E-Mail oder über nicht autorisierte SaaS-Apps (Software-as-a-Service) freigegebene Inhalte untersuchen. Die [Microsoft 365-Mechanismen zur Verhinderung von Datenverlust (Data Loss Protection, DLP)](https://docs.microsoft.com/microsoft-365/compliance/data-loss-prevention-policies?view=o365-worldwide) unterstützen Sie beim Identifizieren, Verhindern und Überwachen von versehentlichen Freigaben vertraulicher Informationen in Ihrer gesamten Microsoft 365-Infrastruktur. Mit [Microsoft Cloud App Security](https://www.microsoft.com/microsoft-365/enterprise-mobility-security/cloud-app-security) können Sie die Verwendung nicht autorisierter SaaS-Apps in Ihrer Umgebung identifizieren.

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln finden Sie Informationen zum Sichern des externen Zugriffs auf Ressourcen. Es wird empfohlen, die Aktionen in der angegebenen Reihenfolge auszuführen.

1. [Ermitteln des Sicherheitsstatus für externen Zugriff](1-secure-access-posture.md)

2. [Ermitteln des aktuellen Status](2-secure-access-current-state.md) (in diesem Artikel erläuterter Schritt)

3. [Erstellen eines Governanceplans für den externen Zugriff](3-secure-access-plan.md)

4. [Verwenden von Gruppen für die Sicherheit](4-secure-access-groups.md)

5. [Umstellung auf Azure AD B2B](5-secure-access-b2b.md)

6. [Schützen des Zugriffs mit der Berechtigungsverwaltung](6-secure-access-entitlement-managment.md)

7. [Schützen des Zugriffs mit Richtlinien für bedingten Zugriff](7-secure-access-conditional-access.md)

8. [Schützen des Zugriffs mit Vertraulichkeitsbezeichnungen](8-secure-access-sensitivity-labels.md)

9. [Schützen des Zugriffs auf Microsoft Teams, OneDrive und SharePoint](9-secure-access-teams-sharepoint.md)