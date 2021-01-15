---
title: Hinzufügen oder Entfernen von Azure-Rollenzuweisungen über das Azure-Portal – Azure RBAC
description: Erfahren Sie, wie Sie den Zugriff auf Azure-Ressourcen für Benutzer, Gruppen, Dienstprinzipale und verwaltete Identitäten über das Azure-Portal mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) verwalten.
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: how-to
ms.workload: identity
ms.date: 01/11/2021
ms.author: rolyon
ms.openlocfilehash: f1753e7bc50fa9ff2c5512696a37dae7578f23b4
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2021
ms.locfileid: "98117443"
---
# <a name="add-or-remove-azure-role-assignments-using-the-azure-portal"></a>Hinzufügen oder Entfernen von Azure-Rollenzuweisungen über das Azure-Portal

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control/definition-grant.md)] In diesem Artikel wird die Zuweisung von Rollen über das Azure-Portal beschrieben.

Anweisungen zum Zuweisen von Administratorrollen zu einem Benutzer in Azure Active Directory finden Sie unter [Anzeigen und Zuweisen von Administratorrollen in Azure Active Directory](../active-directory/roles/manage-roles-portal.md).

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [Azure role assignment prerequisites](../../includes/role-based-access-control/prerequisites-role-assignments.md)]

## <a name="add-a-role-assignment"></a>Hinzufügen einer Rollenzuweisung

Wenn Sie in Azure RBAC Zugriff auf eine Azure-Ressource gewähren möchten, fügen Sie eine Rollenzuweisung hinzu. Gehen Sie wie folgt vor, um eine Rolle zuzuweisen: Eine grobe Übersicht der Schritte finden Sie unter [Schritte zum Hinzufügen einer Rollenzuweisung](role-assignments-steps.md).

### <a name="step-1-identify-the-needed-scope"></a>Schritt 1: Ermitteln des erforderlichen Bereichs

[!INCLUDE [Scope for Azure RBAC introduction](../../includes/role-based-access-control/scope-intro.md)]

[!INCLUDE [Scope for Azure RBAC least privilege](../../includes/role-based-access-control/scope-least.md)]Weitere Informationen zum Begriff „Bereich“ finden Sie unter [Grundlegendes zum Begriff „Bereich“](scope-overview.md).

![Bereichsebenen für Azure RBAC](../../includes/role-based-access-control/media/scope-levels.png)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Suchen Sie im oberen Suchfeld nach dem Bereich, für den Sie Zugriff gewähren möchten. Suchen Sie beispielsweise nach **Verwaltungsgruppen**, **Abonnements**, **Ressourcengruppen** oder eine bestimmten Ressource.

    ![Suche nach Ressourcengruppe im Azure-Portal](./media/shared/rg-portal-search.png)

1. Klicken Sie auf die gewünschte Ressource für diesen Bereich.

    Nachfolgend sehen Sie ein Beispiel für eine Ressourcengruppe.

    ![Übersicht über Ressourcengruppen](./media/shared/rg-overview.png)

### <a name="step-2-open-the-add-role-assignment-pane"></a>Schritt 2: Öffnen des Bereichs „Rollenzuweisung hinzufügen“

Die Zuweisung von Rollen zum Gewähren von Zugriff auf Azure-Ressourcen erfolgt in der Regel über die Seite **Zugriffssteuerung (IAM)** . Diese wird auch als Identity & Access Management (IAM) bezeichnet und wird an mehreren Stellen im Azure-Portal angezeigt.

1. Klicken Sie auf **Zugriffssteuerung (IAM)**.

    Die folgende Abbildung zeigt ein Beispiel der Seite „Zugriffssteuerung (IAM)“ für eine Ressourcengruppe.

    ![Seite „Zugriffssteuerung (IAM)“ für eine Ressourcengruppe](./media/shared/rg-access-control.png)

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen**, um die Rollenzuweisungen für diesen Bereich anzuzeigen.

1. Klicken Sie auf **Hinzufügen** > **Rollenzuweisung hinzufügen**.
   Wenn Sie keine Berechtigungen zum Zuweisen von Rollen haben, ist die Option „Rollenzuweisung hinzufügen“ deaktiviert.

   ![Menü „Rollenzuweisung hinzufügen“](./media/shared/add-role-assignment-menu.png)

    Der Bereich „Rollenzuweisung hinzufügen“ wird geöffnet.

   ![Bereich „Rollenzuweisung hinzufügen“](./media/shared/add-role-assignment.png)

### <a name="step-3-select-the-appropriate-role"></a>Schritt 3: Auswählen der geeigneten Rolle

1. Suchen oder scrollen Sie in der Liste **Rollen**, um die Rolle zu finden, die Sie zuweisen möchten.

    Um Ihnen bei der Ermittlung der entsprechenden Rolle zu helfen, können Sie mit dem Mauszeiger auf das Info-Symbol zeigen, um eine Beschreibung der Rolle anzuzeigen. Weitere Informationen finden Sie im Artikel [Integrierte Azure-Rollen](built-in-roles.md).

   ![Rolle auswählen in „Rollenzuweisung hinzufügen“](./media/role-assignments-portal/add-role-assignment-role.png)

1. Klicken Sie, um die Rolle auszuwählen.

### <a name="step-4-select-who-needs-access"></a>Schritt 4: Auswahl zugriffsberechtigter Benutzer

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

### <a name="step-5-assign-role"></a>Schritt 5: Zuweisen einer Rolle

1. Klicken Sie auf **Speichern**, um die Rolle zuzuweisen.

   Nach einigen Augenblicken wird dem Sicherheitsprinzipal die Rolle für den Bereich zugewiesen.

1. Überprüfen Sie auf der Registerkarte **Rollenzuweisungen**, ob die Rollenzuweisung in der Liste angezeigt wird.

    ![Hinzugefügte Rollenzuweisung gespeichert](./media/role-assignments-portal/rg-role-assignments.png)

## <a name="remove-a-role-assignment"></a>Entfernen einer Rollenzuweisung

Wenn Sie in Azure RBAC Zugriff auf eine Azure-Ressource entfernen möchten, entfernen Sie eine Rollenzuweisung. Führen Sie die folgenden Schritte aus, um eine Rollenzuweisung zu entfernen.

1. Öffnen Sie das Blatt **Zugriffssteuerung (IAM)** für einen Bereich, z.B. für eine Verwaltungsgruppe, ein Abonnement, eine Ressourcengruppe oder eine Ressource, für das bzw. die Sie den Zugriff entfernen möchten.

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen**, um alle Rollenzuweisungen für diesen Bereich anzuzeigen.

1. Aktivieren Sie in der Liste der Rollenzuweisungen den Sicherheitsprinzipal mit der zu entfernenden Rollenzuweisung.

   ![Zum Entfernen ausgewählte Rollenzuweisung](./media/role-assignments-portal/rg-role-assignments-select.png)

1. Klicken Sie auf **Entfernen**.

   ![Nachricht zum Entfernen der Rollenzuweisung](./media/role-assignments-portal/remove-role-assignment.png)

1. Klicken Sie in der angezeigten Meldung zum Entfernen der Rollenzuweisung auf **Ja**.

    Wenn eine Meldung angezeigt wird, dass geerbte Rollenzuweisungen nicht entfernt werden können, versuchen Sie, eine Rollenzuweisung in einem untergeordneten Bereich zu entfernen. Öffnen Sie die Zugriffssteuerung (IAM) in dem Bereich, in dem die Rolle zugewiesen wurde, und versuchen Sie es noch mal. Eine Möglichkeit zum schnellen Öffnen der Zugriffssteuerung (IAM) im richtigen Bereich besteht darin, in der Spalte **Bereich** auf den Link neben **(Geerbt)** zu klicken.

   ![Nachricht zum Entfernen der Rollenzuweisung für geerbte Rollenzuweisungen](./media/role-assignments-portal/remove-role-assignment-inherited.png)

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen von Administratorzugriff für ein Azure-Abonnement](role-assignments-portal-subscription-admin.md)
- [Hinzufügen einer Rollenzuweisung für eine verwaltete Identität](role-assignments-portal-managed-identity.md)
- [Behandeln von Problemen bei Azure RBAC](troubleshooting.md)
