---
title: Benutzer und Gruppen in Richtlinien für bedingten Zugriff – Azure Active Directory
description: Bedeutung von Benutzern und Gruppen in einer Richtlinie für bedingten Zugriff in Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 02/11/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 36898e75680771a9cb084fa142bb635ddbf51c70
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2020
ms.locfileid: "77192047"
---
# <a name="conditional-access-users-and-groups"></a>Bedingter Zugriff: Benutzer und Gruppen

Eine Richtlinie für bedingten Zugriff muss eine Benutzerzuweisung als eines der Signale im Entscheidungsprozess einschließen. Benutzer können in Richtlinien für bedingten Zugriff eingeschlossen oder davon ausgeschlossen werden. 

![Benutzer als Signal in den Entscheidungen des bedingten Zugriffs](./media/concept-conditional-access-users-groups/conditional-access-users-and-groups.png)

## <a name="include-users"></a>Einschließen von Benutzern

Diese Liste von Benutzern umfasst in der Regel alle Benutzer, die eine Organisation in einer Richtlinie für bedingten Zugriff berücksichtigt. 

Beim Erstellen einer Richtlinie für bedingten Zugriff sind die folgenden Optionen zum Einschließen verfügbar.

- Keine
   - Keine Benutzer ausgewählt
- Alle Benutzer
   - Alle Benutzer, die im Verzeichnis vorhanden sind, einschließlich der B2B-Gäste.
- Auswählen von „Benutzer und Gruppen“
   - Alle Gast- und externen Benutzer (Vorschau)
      - Diese Auswahl umfasst B2B-Gastbenutzer und externe Benutzer, einschließlich aller Benutzer, deren `user type`-Attribut auf `guest` festgelegt ist. Diese Auswahl gilt auch für externe Benutzer, die sich von einer anderen Organisation wie einem Cloudlösungsanbieter (Cloud Solution Provider, CSP) angemeldet haben. 
   - Verzeichnisrollen (Vorschau)
      - Diese Option ermöglicht Administratoren das Auswählen bestimmter Azure AD-Verzeichnisrollen, die zum Bestimmen der Zuweisung verwendet werden. Beispielsweise können Organisationen eine restriktivere Richtlinie für Benutzer erstellen, denen die Rolle „Globaler Administrator“ zugewiesen ist.
   - Benutzer und Gruppen
      - Ermöglicht das Einbeziehen bestimmter Gruppen von Benutzern. Beispielsweise können Organisationen eine Gruppe auswählen, die alle Mitglieder der Personalabteilung enthält, wenn eine HR-App als Cloud-App ausgewählt wurde. Als Gruppe gilt eine beliebige Gruppe in Azure AD, einschließlich dynamischer oder zugewiesener Sicherheits- und Verteilungsgruppen.

## <a name="exclude-users"></a>Ausschließen von Benutzern

Ausschlüsse werden häufig für Notfallzugriffskonten verwendet. Weitere Informationen zu Notfallzugriffskonten und warum sie wichtig sind, finden Sie in den folgenden Artikeln: 

* [Verwalten von Konten für den Notfallzugriff in Azure AD](../users-groups-roles/directory-emergency-access.md)
* [Erstellen einer robusten Verwaltungsstrategie für die Zugriffssteuerung in Azure Active Directory](../authentication/concept-resilient-controls.md)

Beim Erstellen einer Richtlinie für bedingten Zugriff sind die folgenden Optionen zum Ausschließen verfügbar.

- Alle Gast- und externen Benutzer (Vorschau)
   - Diese Auswahl umfasst B2B-Gastbenutzer und externe Benutzer, einschließlich aller Benutzer, deren `user type`-Attribut auf `guest` festgelegt ist. Diese Auswahl gilt auch für externe Benutzer, die sich von einer anderen Organisation wie einem Cloudlösungsanbieter (Cloud Solution Provider, CSP) angemeldet haben. 
- Verzeichnisrollen (Vorschau)
   - Diese Option ermöglicht Administratoren das Auswählen bestimmter Azure AD-Verzeichnisrollen, die zum Bestimmen der Zuweisung verwendet werden. Beispielsweise können Organisationen eine restriktivere Richtlinie für Benutzer erstellen, denen die Rolle „Globaler Administrator“ zugewiesen ist.
- Benutzer und Gruppen
   - Ermöglicht das Einbeziehen bestimmter Gruppen von Benutzern. Beispielsweise können Organisationen eine Gruppe auswählen, die alle Mitglieder der Personalabteilung enthält, wenn eine HR-App als Cloud-App ausgewählt wurde. Als Gruppe gilt eine beliebige Gruppe in Azure AD, einschließlich dynamischer oder zugewiesener Sicherheits- und Verteilungsgruppen.

## <a name="next-steps"></a>Nächste Schritte

- [Bedingter Zugriff: Cloud-Apps oder -aktionen](concept-conditional-access-cloud-apps.md)

- [Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)
