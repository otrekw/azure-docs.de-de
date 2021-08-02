---
title: Zuweisen von Azure AD-Rollen zu Benutzern – Azure Active Directory
description: Erfahren Sie, wie Sie Benutzern in Azure Active Directory Zugriff gewähren, indem Sie Azure AD-Rollen zuweisen.
services: active-directory
author: rolyon
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: how-to
ms.date: 05/14/2021
ms.author: rolyon
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1d62e723f42838343798603517ca430d727ff517
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "110787833"
---
# <a name="assign-azure-ad-roles-to-users"></a>Zuweisen von Azure AD-Rollen zu Benutzern

Sie können jetzt im Azure AD Admin Center sämtliche Mitglieder der Administratorrollen anzeigen und verwalten. Wenn Sie häufig Rollenzuweisungen verwalten, bevorzugen Sie diese Funktionalität wahrscheinlich. In diesem Artikel wird beschrieben, wie Azure AD-Rollen über das Azure AD Admin Center zugewiesen werden.

## <a name="prerequisites"></a>Voraussetzungen

- „Administrator für privilegierte Rollen“ oder „Globaler Administrator“
- Azure AD Premium P2-Lizenz bei Verwendung von Privileged Identity Management (PIM)

## <a name="assign-a-role"></a>Zuweisen einer Rolle

1. Melden Sie sich beim [Azure AD Admin Center](https://aad.portal.azure.com) an.

1. Wählen Sie **Azure Active Directory** aus.

1. Wählen Sie **Rollen und Administratoren** aus, um die Liste mit allen verfügbaren Rollen anzuzeigen.

    ![Screenshot der Seite „Rollen und Administratoren“](./media/manage-roles-portal/roles-and-administrators.png)

1. Wählen Sie eine Rolle aus, um ihre Zuweisungen anzuzeigen.

    Um Ihnen die Suche nach der benötigten Rolle zu vereinfachen, können Sie in Azure AD Teilmengen der Rollen anhand der jeweiligen Rollenkategorien anzeigen. Sehen Sie sich den Filter **Typ** an, mit dem Sie nur die Rollen mit dem ausgewählten Typ anzeigen können.

1. Wählen Sie **Zuweisungen hinzufügen** und dann die Benutzer aus, die Sie dieser Rolle zuweisen möchten.

    Wenn Ihre Darstellung von der folgenden Abbildung abweicht, lesen Sie den Hinweis unter [Privileged Identity Management (PIM)](#privileged-identity-management-pim), um zu überprüfen, ob Sie PIM verwenden.

    ![Liste der Berechtigungen für eine Administratorrolle](./media/manage-roles-portal/add-assignments.png)

1. Wählen Sie **Hinzufügen** aus, um die Rolle zuzuweisen.

## <a name="privileged-identity-management-pim"></a>Privileged Identity Management (PIM)

Sie können **Im PIM verwalten** auswählen, um zusätzliche Verwaltungsfunktionen mit [Azure AD Privileged Identity Management (PIM)](../privileged-identity-management/pim-configure.md) zu verwenden. Administratoren für privilegierte Rollen können Zuweisungen vom Typ „Permanent“ (in der Rolle stets aktiv) in „Berechtigt“ (nur bei Erweiterung in die Rolle) ändern. Wenn Sie nicht über Privileged Identity Management verfügen, können Sie trotzdem **Manage in PIM** (In PIM verwalten) auswählen, um sich für eine Testversion zu registrieren. Für Privileged Identity Management ist eine [Azure AD Premium P2-Lizenz](../privileged-identity-management/subscription-requirements.md) erforderlich.

![Screenshot: Seite „Benutzeradministrator – Zuweisungen“ mit ausgewählter Aktion „Im PIM verwalten“](./media/manage-roles-portal/member-list-pim.png)

Wenn Sie globaler Administrator oder Administrator für privilegierte Rollen sind, können Sie ganz einfach Mitglieder hinzufügen oder entfernen, die Liste filtern oder ein Mitglied auswählen, um dessen aktive zugewiesene Rollen anzuzeigen.

> [!Note]
> Wenn Sie über eine Azure AD Premium P2-Lizenz verfügen und Privileged Identity Management bereits nutzen, werden alle Rollenverwaltungsaufgaben in Privileged Identity Management und nicht in Azure AD ausgeführt.
>
> ![In PIM verwaltete Azure AD-Rollen für Benutzer, die PIM bereits nutzen und über eine Premium P2-Lizenz verfügen](./media/manage-roles-portal/pim-manages-roles-for-p2.png)

## <a name="next-steps"></a>Nächste Schritte

* Im [Forum für Azure AD-Administratorrollen](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032) können Sie sich gerne mit uns in Verbindung setzen.
* Weitere Informationen zu Rollen finden Sie unter [Integrierte Rollen in Azure AD](permissions-reference.md).
* Informationen zu Standardbenutzerberechtigungen finden Sie unter [Vergleich von Standardbenutzerberechtigungen für Gäste und Mitglieder](../fundamentals/users-default-permissions.md).
