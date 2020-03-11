---
title: Dynamische Gruppen und B2B-Zusammenarbeit – Azure Active Directory | Microsoft-Dokumentation
description: Hier wird gezeigt, wie Sie dynamische Azure AD-Gruppen mit Azure Active Directory B2B-Zusammenarbeit verwenden.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 41e8b81bc3594c6a378757636f70058510a38cc7
ms.sourcegitcommit: 390cfe85629171241e9e81869c926fc6768940a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2020
ms.locfileid: "78226911"
---
# <a name="dynamic-groups-and-azure-active-directory-b2b-collaboration"></a>Dynamische Gruppen und Azure Active Directory B2B-Zusammenarbeit

## <a name="what-are-dynamic-groups"></a>Was sind dynamische Gruppen?
Die dynamische Konfiguration der Mitgliedschaft in Sicherheitsgruppen für Azure Active Directory (Azure AD) ist im [Azure-Portal](https://portal.azure.com) verfügbar. Administratoren können Regeln einrichten, um in Azure AD erstellte Gruppen anhand von Benutzerattributen (z.B. Benutzertyp, Abteilung oder Land/Region) aufzufüllen. Mitglieder können auf der Grundlage ihrer Attribute automatisch zu einer Sicherheitsgruppe hinzugefügt oder daraus entfernt werden. Diese Gruppen können Zugriff auf Anwendungen oder Cloudressourcen (SharePoint-Websites, Dokumente) gewähren oder den Mitgliedern Lizenzen zuweisen. Weitere Informationen zu dynamischen Gruppen erhalten Sie unter [Dedizierte Gruppen in Azure Active Directory](../active-directory-accessmanagement-dedicated-groups.md).

Für die Erstellung und Verwendung dynamischer Gruppen wird die passende [Azure AD-Lizenzierung (Premium P1 oder Premium P2)](https://azure.microsoft.com/pricing/details/active-directory/) benötigt. Weitere Informationen finden Sie im Artikel [Erstellen attributbasierter Regeln für dynamische Gruppenmitgliedschaft in Azure Active Directory](../users-groups-roles/groups-dynamic-membership.md).

## <a name="creating-an-all-users-dynamic-group"></a>Erstellen einer dynamischen Gruppe „Alle Benutzer“
Sie können mithilfe einer Mitgliedschaftsregel eine Gruppe mit allen Benutzern innerhalb eines Mandanten erstellen. Wenn Benutzer in Zukunft zum Mandanten hinzugefügt oder daraus entfernt werden, wird die Mitgliedschaft in der Gruppe automatisch angepasst.

1. Melden Sie sich im [Azure-Portal](https://portal.azure.com) mit einem Konto an, dem im Mandanten die Rolle „Globaler Administrator“ oder „Benutzeradministrator“ zugewiesen ist.
1. Wählen Sie **Azure Active Directory** aus.
2. Wählen Sie unter **Verwalten** die Option **Gruppen** und anschließend **Neue Gruppe** aus.
1. Wählen Sie auf der Seite **Neue Gruppe** unter **Gruppentyp** die Option **Sicherheit** aus. Geben Sie einen **Gruppennamen** und eine **Gruppenbeschreibung** für die neue Gruppe ein. 
2. Wählen Sie unter **Mitgliedschaftstyp** die Option **Dynamischer Benutzer** und dann **Dynamische Abfrage hinzufügen** aus. 
4. Wählen Sie über dem Textfeld **Regelsyntax** den Befehl **Bearbeiten** aus. Geben Sie auf der Seite **Regelsyntax bearbeiten** den folgenden Ausdruck im Textfeld ein:

   ```
   user.objectId -ne null
   ```
1. Klicken Sie auf **OK**. Die Regel wird im Feld „Regelsyntax“ angezeigt:

   ![Regelsyntax für dynamische Gruppe „Alle Benutzer“](media/use-dynamic-groups/all-user-rule-syntax.png)

1.  Wählen Sie **Speichern** aus. Die neue dynamische Gruppe enthält jetzt sowohl B2B-Gastbenutzer als auch Mitgliederbenutzer.


1. Wählen Sie auf der Seite **Neue Gruppe** die Option **Erstellen** aus, um die Gruppe zu erstellen.

## <a name="creating-a-group-of-members-only"></a>Erstellen einer Gruppe nur für Mitglieder

Wenn in einer Gruppe Gastbenutzer ausgeschlossen und nur Mitglieder des Mandanten enthalten sein sollen, erstellen Sie eine dynamische Gruppe (wie zuvor beschrieben), geben aber im Feld **Regelsyntax** den folgenden Ausdruck ein:

```
(user.objectId -ne null) and (user.userType -eq "Member")
```

Die folgende Abbildung zeigt die Regelsyntax für eine modifizierte dynamische Gruppe, die nur Mitglieder enthält und Gastbenutzer ausschließt.

![Zeigt die Regel, bei der der Benutzertyp gleich „Mitglied“ ist](media/use-dynamic-groups/all-member-user-rule-syntax.png)

## <a name="creating-a-group-of-guests-only"></a>Erstellen einer Gruppe nur für Gäste

Unter Umständen empfiehlt es sich auch, eine neue dynamische Gruppe zu erstellen, die nur Gastbenutzer enthält, um Richtlinien auf sie anwenden zu können (etwa Azure AD-Richtlinien für den bedingten Zugriff). Erstellen Sie eine dynamische Gruppe (wie zuvor beschrieben), geben aber im Feld **Regelsyntax** den folgenden Ausdruck ein:

```
(user.objectId -ne null) and (user.userType -eq "Guest")
```

Die folgende Abbildung zeigt die Regelsyntax für eine modifizierte dynamische Gruppe, die nur Gäste enthält und Mitgliederbenutzer ausschließt.

![Zeigt die Regel, bei der der Benutzertyp gleich „Gast“ ist](media/use-dynamic-groups/all-guest-user-rule-syntax.png)

## <a name="next-steps"></a>Nächste Schritte

- [Eigenschaften von B2B-Zusammenarbeitsbenutzern](user-properties.md)
- [Hinzufügen eines B2B-Zusammenarbeitsbenutzers zu einer Rolle](add-guest-to-role.md)
- [Bedingter Zugriff für Benutzer von B2B-Zusammenarbeit](conditional-access.md)

