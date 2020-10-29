---
title: Benutzer und Gruppen in Richtlinien für bedingten Zugriff – Azure Active Directory
description: Bedeutung von Benutzern und Gruppen in einer Richtlinie für bedingten Zugriff in Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 08/03/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 710122ed4e5a02453f6f0c19a145af64f2c69178
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2020
ms.locfileid: "92145425"
---
# <a name="conditional-access-users-and-groups"></a>Bedingter Zugriff: Benutzer und Gruppen

Eine Richtlinie für bedingten Zugriff muss eine Benutzerzuweisung als eines der Signale im Entscheidungsprozess einschließen. Benutzer können in Richtlinien für bedingten Zugriff eingeschlossen oder davon ausgeschlossen werden. Von Azure Active Directory werden alle Richtlinien ausgewertet, und es wird sichergestellt, dass alle Anforderungen erfüllt sind, bevor dem Benutzer der Zugriff gewährt wird.

![Benutzer als Signal in den Entscheidungen des bedingten Zugriffs](./media/concept-conditional-access-users-groups/conditional-access-users-and-groups.png)

## <a name="include-users"></a>Einschließen von Benutzern

Diese Liste von Benutzern umfasst in der Regel alle Benutzer, die eine Organisation in einer Richtlinie für bedingten Zugriff berücksichtigt. 

Beim Erstellen einer Richtlinie für bedingten Zugriff sind die folgenden Optionen zum Einschließen verfügbar.

- Keine
   - Keine Benutzer ausgewählt
- Alle Benutzer
   - Alle Benutzer, die im Verzeichnis vorhanden sind, einschließlich der B2B-Gäste.
- Auswählen von „Benutzer und Gruppen“
   - Alle Gastbenutzer und externen Benutzer
      - Diese Auswahl umfasst B2B-Gastbenutzer und externe Benutzer, einschließlich aller Benutzer, deren `user type`-Attribut auf `guest` festgelegt ist. Diese Auswahl gilt auch für externe Benutzer, die sich von einer anderen Organisation wie einem Cloudlösungsanbieter (Cloud Solution Provider, CSP) angemeldet haben. 
   - Verzeichnisrollen
      - Diese Option ermöglicht Administratoren das Auswählen bestimmter Azure AD-Verzeichnisrollen, die zum Bestimmen der Zuweisung verwendet werden. Beispielsweise können Organisationen eine restriktivere Richtlinie für Benutzer erstellen, denen die Rolle „Globaler Administrator“ zugewiesen ist.
   - Benutzer und Gruppen
      - Ermöglicht das Einbeziehen bestimmter Gruppen von Benutzern. Beispielsweise können Organisationen eine Gruppe auswählen, die alle Mitglieder der Personalabteilung enthält, wenn eine HR-App als Cloud-App ausgewählt wurde. Als Gruppe gilt eine beliebige Gruppe in Azure AD, einschließlich dynamischer oder zugewiesener Sicherheits- und Verteilungsgruppen. Die Richtlinie wird auf geschachtelte Benutzer und Gruppen angewendet.

> [!WARNING]
> Wenn Benutzer oder Gruppen zu mehr als 2048 Gruppen gehören, kann ihr Zugriff blockiert werden. Diese Begrenzung gilt sowohl für die direkte als auch für die geschachtelte Gruppenmitgliedschaft.

> [!WARNING]
> Richtlinien für bedingten Zugriff unterstützen keine Benutzer, die einer [auf eine Verwaltungseinheit beschränkten](../users-groups-roles/roles-admin-units-assign-roles.md) Verzeichnisrolle zugewiesen sind. Das gleiche gilt für Benutzer, die Verzeichnisrollen zugewiesen sind, die sich direkt auf ein Objekt beziehen (z.B. über [benutzerdefinierte Rollen](../users-groups-roles/roles-create-custom.md)).

## <a name="exclude-users"></a>Ausschließen von Benutzern

Wenn Organisationen einen Benutzer oder eine Gruppe sowohl ein- als auch ausschließen, wird der Benutzer oder die Gruppe von der Richtlinie ausgeschlossen, da eine Ausschlussaktion das Einschließen in die Richtlinie außer Kraft setzt. Ausschlüsse werden häufig für Notfallzugriffskonten verwendet. Weitere Informationen zu Notfallzugriffskonten und warum sie wichtig sind, finden Sie in den folgenden Artikeln: 

* [Verwalten von Konten für den Notfallzugriff in Azure AD](../users-groups-roles/directory-emergency-access.md)
* [Erstellen einer robusten Verwaltungsstrategie für die Zugriffssteuerung in Azure Active Directory](../authentication/concept-resilient-controls.md)

Beim Erstellen einer Richtlinie für bedingten Zugriff sind die folgenden Optionen zum Ausschließen verfügbar.

- Alle Gastbenutzer und externen Benutzer
   - Diese Auswahl umfasst B2B-Gastbenutzer und externe Benutzer, einschließlich aller Benutzer, deren `user type`-Attribut auf `guest` festgelegt ist. Diese Auswahl gilt auch für externe Benutzer, die sich von einer anderen Organisation wie einem Cloudlösungsanbieter (Cloud Solution Provider, CSP) angemeldet haben. 
- Verzeichnisrollen
   - Diese Option ermöglicht Administratoren das Auswählen bestimmter Azure AD-Verzeichnisrollen, die zum Bestimmen der Zuweisung verwendet werden. Beispielsweise können Organisationen eine restriktivere Richtlinie für Benutzer erstellen, denen die Rolle „Globaler Administrator“ zugewiesen ist.
- Benutzer und Gruppen
   - Ermöglicht das Einbeziehen bestimmter Gruppen von Benutzern. Beispielsweise können Organisationen eine Gruppe auswählen, die alle Mitglieder der Personalabteilung enthält, wenn eine HR-App als Cloud-App ausgewählt wurde. Als Gruppe gilt eine beliebige Gruppe in Azure AD, einschließlich dynamischer oder zugewiesener Sicherheits- und Verteilungsgruppen.

### <a name="preventing-administrator-lockout"></a>Verhindern der Administratorsperre

Um zu verhindern, dass ein Administrator sich bei der Erstellung einer Richtlinie, die für **Alle Benutzer** und **Alle Apps** gilt, aus seinem Verzeichnis aussperrt, wird die folgende Warnung angezeigt:

> Sperren Sie sich nicht aus! Wir empfehlen, die Richtlinie zuerst auf eine kleine Gruppe von Benutzern anzuwenden und zu überprüfen, ob sie sich erwartungsgemäß verhält. Wir empfehlen auch, mindestens einen Administrator von dieser Richtlinie auszuschließen. So wird sichergestellt, dass Sie weiterhin Zugriff haben und die Richtlinie bei Bedarf aktualisieren können. Überprüfen Sie die betroffenen Benutzer und Apps.

Standardmäßig bietet die Richtlinie eine Option, um den aktuellen Benutzer von der Richtlinie auszuschließen. Diese Standardeinstellung kann jedoch vom Administrator außer Kraft gesetzt werden, wie in der folgenden Abbildung dargestellt. 

![Warnung: Sperren Sie sich nicht aus!](./media/concept-conditional-access-users-groups/conditional-access-users-and-groups-lockout-warning.png)

[Was ist zu tun, wenn Ihr Zugriff auf das Azure-Portal gesperrt ist?](troubleshoot-conditional-access.md#what-to-do-if-you-are-locked-out-of-the-azure-portal)

## <a name="next-steps"></a>Nächste Schritte

- [Bedingter Zugriff: Cloud-Apps oder -aktionen](concept-conditional-access-cloud-apps.md)

- [Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)
