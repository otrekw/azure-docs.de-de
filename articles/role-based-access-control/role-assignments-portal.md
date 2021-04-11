---
title: Zuweisen von Azure-Rollen mit dem Azure-Portal – Azure RBAC
description: Erfahren Sie, wie Sie den Zugriff auf Azure-Ressourcen für Benutzer, Gruppen, Dienstprinzipale und verwaltete Identitäten über das Azure-Portal mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) verwalten.
services: active-directory
author: rolyon
manager: daveba
ms.service: role-based-access-control
ms.topic: how-to
ms.workload: identity
ms.date: 02/15/2021
ms.author: rolyon
ms.custom: contperf-fy21q3-portal
ms.openlocfilehash: 6b159d9ca7d8d7739d623bb3a48752b4d03e24bc
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2021
ms.locfileid: "106385961"
---
# <a name="assign-azure-roles-using-the-azure-portal"></a>Zuweisen von Azure-Rollen über das Azure-Portal

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control/definition-grant.md)] In diesem Artikel wird die Zuweisung von Rollen über das Azure-Portal beschrieben.

Anweisungen zum Zuweisen von Administratorrollen zu einem Benutzer in Azure Active Directory finden Sie unter [Anzeigen und Zuweisen von Administratorrollen in Azure Active Directory](../active-directory/roles/manage-roles-portal.md).

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [Azure role assignment prerequisites](../../includes/role-based-access-control/prerequisites-role-assignments.md)]

## <a name="step-1-identify-the-needed-scope"></a>Schritt 1: Ermitteln des erforderlichen Bereichs

[!INCLUDE [Scope for Azure RBAC introduction](../../includes/role-based-access-control/scope-intro.md)]

[!INCLUDE [Scope for Azure RBAC least privilege](../../includes/role-based-access-control/scope-least.md)]Weitere Informationen zum Begriff „Bereich“ finden Sie unter [Grundlegendes zum Begriff „Bereich“](scope-overview.md).

![Bereichsebenen für Azure RBAC](../../includes/role-based-access-control/media/scope-levels.png)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Suchen Sie im oberen Suchfeld nach dem Bereich, für den Sie Zugriff gewähren möchten. Suchen Sie beispielsweise nach **Verwaltungsgruppen**, **Abonnements**, **Ressourcengruppen** oder eine bestimmten Ressource.

    ![Suche nach Ressourcengruppe im Azure-Portal](./media/shared/rg-portal-search.png)

1. Klicken Sie auf die gewünschte Ressource für diesen Bereich.

    Nachfolgend sehen Sie ein Beispiel für eine Ressourcengruppe.

    ![Übersicht über Ressourcengruppen](./media/shared/rg-overview.png)

## <a name="step-2-open-the-add-role-assignment-pane"></a>Schritt 2: Öffnen des Bereichs „Rollenzuweisung hinzufügen“

Die Zuweisung von Rollen zum Gewähren von Zugriff auf Azure-Ressourcen erfolgt in der Regel über die Seite **Zugriffssteuerung (IAM)** . Diese wird auch als Identity & Access Management (IAM) bezeichnet und wird an mehreren Stellen im Azure-Portal angezeigt.

1. Klicken Sie auf **Zugriffssteuerung (IAM)**.

    Die folgende Abbildung zeigt ein Beispiel der Seite „Zugriffssteuerung (IAM)“ für eine Ressourcengruppe.

    ![Seite „Zugriffssteuerung (IAM)“ für eine Ressourcengruppe](./media/shared/rg-access-control.png)

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen**, um die Rollenzuweisungen für diesen Bereich anzuzeigen.

1. Klicken Sie auf **Hinzufügen** > **Rollenzuweisung hinzufügen**.
   Wenn Sie keine Berechtigungen zum Zuweisen von Rollen haben, ist die Option „Rollenzuweisung hinzufügen“ deaktiviert.

   ![Menü „Rollenzuweisung hinzufügen“](./media/shared/add-role-assignment-menu.png)

    Der Bereich „Rollenzuweisung hinzufügen“ wird geöffnet.

   ![Seite „Rollenzuweisung hinzufügen“](../../includes/role-based-access-control/media/add-role-assignment-page.png)

## <a name="step-3-select-the-appropriate-role"></a>Schritt 3: Auswählen der geeigneten Rolle

1. Suchen oder scrollen Sie in der Liste **Rollen**, um die Rolle zu finden, die Sie zuweisen möchten.

    Um Ihnen bei der Ermittlung der entsprechenden Rolle zu helfen, können Sie mit dem Mauszeiger auf das Info-Symbol zeigen, um eine Beschreibung der Rolle anzuzeigen. Weitere Informationen finden Sie im Artikel [Integrierte Azure-Rollen](built-in-roles.md).

   ![Rolle auswählen in „Rollenzuweisung hinzufügen“](./media/role-assignments-portal/add-role-assignment-role.png)

1. Klicken Sie, um die Rolle auszuwählen.

## <a name="step-4-select-who-needs-access"></a>Schritt 4: Auswahl zugriffsberechtigter Benutzer

1. Wählen Sie in der Liste **Zugriff zuweisen zu** den Typ des Sicherheitsprinzipals aus, dem Sie den Zugriff zuweisen möchten.

    | type | BESCHREIBUNG |
    | --- | --- |
    | **Benutzer, Gruppe oder Dienstprinzipal** | Wenn Sie die Rolle einem Benutzer, einer Gruppe oder einem Dienstprinzipal (Anwendung) zuweisen möchten, wählen Sie diesen Typ aus. |
    | **Benutzerseitig zugewiesene verwaltete Identität** | Wenn Sie die Rolle einer [benutzerseitig zugewiesenen verwalteten Identität](../active-directory/managed-identities-azure-resources/overview.md) zuweisen möchten, wählen Sie diesen Typ aus. |
    | *Systemseitig zugewiesene verwaltete Identität* | Wenn Sie die Rolle einer [systemseitig zugewiesenen verwalteten Identität](../active-directory/managed-identities-azure-resources/overview.md) zuweisen möchten, wählen Sie die Azure-Dienstinstanz aus, in der sich die verwaltete Identität befindet. |

   ![Auswählen des Sicherheitsprinzipaltyps in „Rollenzuweisung hinzufügen“](./media/role-assignments-portal/add-role-assignment-type.png)

1. Wenn Sie eine benutzerseitig zugewiesene verwaltete Identität oder eine systemseitig zugewiesene verwaltete Identität ausgewählt haben, wählen Sie das **Abonnement** aus, in dem sich die verwaltete Identität befindet.

1. Suchen Sie im Abschnitt **Auswählen** nach dem Sicherheitsprinzipal, indem Sie eine Zeichenfolge eingeben oder durch die Liste scrollen.

   ![Auswählen von Benutzern unter „Rollenzuweisung hinzufügen“](./media/role-assignments-portal/add-role-assignment-user.png)

1. Nachdem Sie den Dienstprinzipal gefunden haben, wählen Sie ihn durch Mausklick aus.

## <a name="step-5-assign-role"></a>Schritt 5: Zuweisen einer Rolle

1. Klicken Sie auf **Speichern**, um die Rolle zuzuweisen.

   Nach einigen Augenblicken wird dem Sicherheitsprinzipal die Rolle für den Bereich zugewiesen.

1. Überprüfen Sie auf der Registerkarte **Rollenzuweisungen**, ob die Rollenzuweisung in der Liste angezeigt wird.

    ![Hinzugefügte Rollenzuweisung gespeichert](./media/role-assignments-portal/rg-role-assignments.png)

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen von Administratorzugriff für ein Azure-Abonnement](role-assignments-portal-subscription-admin.md)
- [Entfernen von Azure-Rollenzuweisungen](role-assignments-remove.md)
- [Behandeln von Problemen bei Azure RBAC](troubleshooting.md)
