---
title: Einstieg in die Verwendung von PIM – Azure Active Directory | Microsoft-Dokumentation
description: Hier erhalten Sie Informationen zum Aktivieren von und zu den ersten Schritten mit Azure AD Privileged Identity Management (PIM) im Azure-Portal.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: pim
ms.topic: conceptual
ms.workload: identity
ms.date: 03/13/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5cbb9b4340a7cdb9be5039722a8f75e09288ec48
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79472859"
---
# <a name="start-using-privileged-identity-management"></a>Einstieg in Privileged Identity Management

Mit Privileged Identity Management (PIM) können Sie den Zugriff innerhalb Ihrer Azure Active Directory-Organisation (Azure AD-Organisation) verwalten, steuern und überwachen. Dazu zählt der Zugriff auf Azure-Ressourcen, Azure AD und andere Microsoft-Onlinedienste wie Office 365 oder Microsoft Intune.

In diesem Artikel werden das Aktivieren von und die ersten Schritte mit Privileged Identity Management beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

Um Privileged Identity Management verwenden zu können, müssen Sie über eine der folgenden Lizenzen verfügen:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5

Weitere Informationen finden Sie unter [Lizenzanforderungen für die Verwendung von PIM](subscription-requirements.md).

## <a name="sign-up-pim-for-azure-ad-roles"></a>Registrieren von PIM für Azure AD-Rollen

Nachdem Sie Privileged Identity Management für Ihr Verzeichnis aktiviert haben, müssen Sie Privileged Identity Management für die Verwaltung von Azure AD-Rollen registrieren.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Wählen Sie **Azure AD-Rollen** aus.

    ![Privileged Identity Management für Azure AD-Rollen registrieren](./media/pim-getting-started/sign-up-pim-azure-ad-roles.png)

1. Wählen Sie **Registrieren** aus.

1. Klicken Sie in der daraufhin angezeigten Meldung auf **Ja**, um Privileged Identity Management für die Verwaltung von Azure AD-Rollen zu registrieren.

    ![Meldung für die Registrierung von Privileged Identity Management für Azure AD-Rollen](./media/pim-getting-started/sign-up-pim-message.png)

    Nach Abschluss der Registrierung sind die Azure AD-Optionen aktiviert. Möglicherweise müssen Sie das Portal aktualisieren.

    Informationen zum Ermitteln und Auswählen der mit Privileged Identity Management zu schützenden Azure-Ressourcen finden Sie unter [Ermitteln von Azure-Ressourcen zur Verwaltung in PIM](pim-resource-roles-discover-resources.md).

## <a name="navigate-to-your-tasks"></a>Navigieren zu Ihren Aufgaben

Nach der Einrichtung von Privileged Identity Management können Sie mit dem Ausführen Ihrer Identitätsverwaltungsaufgaben beginnen.

![Navigationsfenster in Privileged Identity Management mit den Optionen „Aufgaben“ und „Verwalten“](./media/pim-getting-started/pim-quickstart-tasks.png)

| „Aufgaben“ und „Verwalten“ | BESCHREIBUNG |
| --- | --- |
| **Meine Rollen**  | Zeigt eine Liste mit berechtigten und aktiven Rollen an, die Ihnen zugewiesen sind. Hier können Sie zugewiesene berechtigte Rollen aktivieren. |
| **Meine Anforderungen** | Zeigt Ihre ausstehenden Anforderungen zum Aktivieren berechtigter Rollenzuweisungen an. |
| **Genehmigen von Anforderungen** | Zeigt eine Liste mit Anforderungen zum Aktivieren berechtigter Rollen nach Benutzern in Ihrem Verzeichnis an, für deren Genehmigung Sie zuständig sind. |
| **Zugriff überprüfen** | Zeigt eine Liste mit aktiven Zugriffsüberprüfungen an, die Ihnen zugewiesen sind. Dabei spielt es keine Rolle, ob Sie den Zugriff für sich selbst oder für eine andere Person überprüfen. |
| **Azure AD-Rollen** | Zeigt für Administratoren privilegierter Rollen ein Dashboard und Einstellungen zum Verwalten von Azure AD-Rollenzuweisungen an. Für Benutzer, die kein Administrator für privilegierte Rollen sind, ist dieses Dashboard deaktiviert. Diese Benutzer haben Zugriff auf ein spezielles Dashboard namens „Meine Ansicht“. Auf dem Dashboard „Meine Ansicht“ werden nur Informationen zu dem Benutzer angezeigt, der auf das Dashboard zugreift (also keine Informationen für den gesamten Mandanten). |
| **Azure-Ressourcen** | Zeigt für Administratoren privilegierter Rollen ein Dashboard und Einstellungen zum Verwalten von Azure-Ressourcenrollenzuweisungen an. Für Benutzer, die kein Administrator für privilegierte Rollen sind, ist dieses Dashboard deaktiviert. Diese Benutzer haben Zugriff auf ein spezielles Dashboard namens „Meine Ansicht“. Auf dem Dashboard „Meine Ansicht“ werden nur Informationen zu dem Benutzer angezeigt, der auf das Dashboard zugreift (also keine Informationen für den gesamten Mandanten). |

## <a name="add-a-pim-tile-to-the-dashboard"></a>Hinzufügen einer PIM-Kachel zum Dashboard

Fügen Sie Ihrem Azure-Portaldashboard eine Privileged Identity Management-Kachel hinzu, um Privileged Identity Management einfacher öffnen zu können.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Wählen Sie **Alle Dienste** aus, und suchen Sie nach dem Dienst **Azure AD Privileged Identity Management**.

    ![Azure AD Privileged Identity Management in „Alle Dienste“](./media/pim-getting-started/pim-all-services-find.png)

1. Wählen Sie den Privileged Identity Management-Schnellstart aus.

1. Aktivieren Sie **Blatt an Dashboard anheften**, um das Blatt für den Schnellstart von Privileged Identity Management an das Dashboard anzuheften.

    ![Reißzweckensymbol für das Anheften des Privileged Identity Management-Blatts an das Dashboard](./media/pim-getting-started/pim-quickstart-pin-to-dashboard.png)

    Im Azure-Dashboard wird eine Kachel wie diese angezeigt:

    ![Kachel für den Schnellstart von Privileged Identity Management-Schnellstart auf dem Dashboard](./media/pim-getting-started/pim-quickstart-dashboard-tile.png)

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen von Azure AD-Rollen in PIM](pim-how-to-add-role-to-user.md)
- [Ermitteln von Azure-Ressourcen zur Verwaltung in PIM](pim-resource-roles-discover-resources.md)
