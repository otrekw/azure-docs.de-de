---
title: Hinzufügen oder Entfernen von Azure-Rollenzuweisungen über das Azure-Portal – Azure RBAC
description: Erfahren Sie, wie Sie den Zugriff auf Azure-Ressourcen für Benutzer, Gruppen, Dienstprinzipale und verwaltete Identitäten über das Azure-Portal mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) verwalten.
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: how-to
ms.workload: identity
ms.date: 09/30/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 1aebb80fcba6c21ccce63772195af80b94092bde
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2020
ms.locfileid: "92368317"
---
# <a name="add-or-remove-azure-role-assignments-using-the-azure-portal"></a>Hinzufügen oder Entfernen von Azure-Rollenzuweisungen über das Azure-Portal

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)] In diesem Artikel wird die Zuweisung von Rollen über das Azure-Portal beschrieben.

Anweisungen zum Zuweisen von Administratorrollen zu einem Benutzer in Azure Active Directory finden Sie unter [Anzeigen und Zuweisen von Administratorrollen in Azure Active Directory](../active-directory/roles/manage-roles-portal.md).

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um Rollenzuweisungen hinzufügen oder entfernen zu können:

- `Microsoft.Authorization/roleAssignments/write`- und `Microsoft.Authorization/roleAssignments/delete`-Berechtigungen, wie z.B. [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator) oder [Besitzer](built-in-roles.md#owner)

## <a name="access-control-iam"></a>Zugriffssteuerung (IAM)

Die Zuweisung von Rollen zum Gewähren von Zugriff auf Azure-Ressourcen erfolgt in der Regel über die Seite **Zugriffssteuerung (IAM)** . Es wird auch als Identitäts- und Zugriffsverwaltung bezeichnet und erscheint an mehreren Stellen im Azure-Portal. Die folgende Abbildung zeigt ein Beispiel der Seite „Zugriffssteuerung (IAM)“ für ein Abonnement:

![Die Seite „Zugriffssteuerung (IAM)“ für ein Abonnement](./media/role-assignments-portal/access-control-subscription.png)

Um die Seite „Zugriffssteuerung (IAM)“ möglichst effizient zu nutzen, befolgen Sie beim Zuweisen einer Rolle die folgenden Schritte.

1. Ermitteln Sie, wer Zugriff benötigt. Sie können eine Rolle einem Benutzer, einer Gruppe, einem Dienstprinzipal oder einer verwalteten Identität zuweisen.

1. Suchen Sie die gewünschte Rolle. Berechtigungen sind in Rollen zusammengefasst. Sie können aus einer Liste von verschiedenen [in Azure integrierten Rollen](built-in-roles.md) auswählen oder eigene benutzerdefinierte Rollen verwenden.

1. Ermitteln Sie den erforderlichen Bereich. Azure bietet vier Bereichsebenen: [Verwaltungsgruppe](../governance/management-groups/overview.md), Abonnement, [Ressourcengruppe](../azure-resource-manager/management/overview.md#resource-groups) und Ressource. Weitere Informationen zum Begriff „Bereich“ finden Sie unter [Grundlegendes zum Begriff „Bereich“](scope-overview.md).

1. Führen Sie die Schritte in einem der folgenden Abschnitte aus, um eine Rolle zuzuweisen.

## <a name="add-a-role-assignment"></a>Hinzufügen einer Rollenzuweisung

Wenn Sie in Azure RBAC Zugriff auf eine Azure-Ressource gewähren möchten, fügen Sie eine Rollenzuweisung hinzu. Gehen Sie wie folgt vor, um eine Rolle zuzuweisen:

1. Klicken Sie im Azure-Portal auf **Alle Dienste** , und wählen Sie dann den Bereich aus, auf den Sie Zugriff gewähren möchten. Sie können beispielsweise **Verwaltungsgruppen** , **Abonnements** , **Ressourcengruppen** oder eine Ressource auswählen.

1. Klicken Sie auf die gewünschte Ressource für diesen Bereich.

1. Klicken Sie auf **Zugriffssteuerung (IAM)** .

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen** , um die Rollenzuweisungen für diesen Bereich anzuzeigen.

    ![Zugriffssteuerung (IAM) und Registerkarte „Rollenzuweisungen“](./media/role-assignments-portal/role-assignments.png)

1. Klicken Sie auf **Hinzufügen** > **Rollenzuweisung hinzufügen** .

   Wenn Sie keine Berechtigungen zum Zuweisen von Rollen haben, ist die Option „Rollenzuweisung hinzufügen“ deaktiviert.

   ![Menü „Rollenzuweisung hinzufügen“](./media/shared/add-role-assignment-menu.png)

    Der Bereich „Rollenzuweisung hinzufügen“ wird geöffnet.

   ![Bereich „Rollenzuweisung hinzufügen“](./media/role-assignments-portal/add-role-assignment.png)

1. Wählen Sie in der Dropdownliste **Rolle** eine Rolle aus, etwa **Mitwirkender für virtuelle Computer** .

1. Wählen Sie in der Liste **Auswählen** einen Benutzer, eine Gruppe, einen Dienstprinzipal oder eine verwaltete Identität aus. Wird der Sicherheitsprinzipal in der Liste nicht angezeigt, können Sie im Feld **Auswählen** einen Begriff eingeben, um das Verzeichnis nach Anzeigenamen, E-Mail-Adressen und Objektbezeichner zu durchsuchen.

1. Klicken Sie auf **Speichern** , um die Rolle zuzuweisen.

   Nach einigen Augenblicken wird dem Sicherheitsprinzipal die Rolle für den Bereich zugewiesen.

    ![Hinzugefügte Rollenzuweisung gespeichert](./media/role-assignments-portal/add-role-assignment-save.png)

## <a name="assign-a-user-as-an-administrator-of-a-subscription"></a>Zuweisen eines Benutzers als Administrator eines Abonnements

Um einen Benutzer zum Administrator eines Azure-Abonnements zu machen, weisen Sie ihm die Rolle [Besitzer](built-in-roles.md#owner) im Abonnementbereich zu. Durch die Rolle „Besitzer“ erhält der Benutzer Vollzugriff auf alle Ressourcen im Abonnement, einschließlich der Berechtigung, anderen Personen den Zugriff zu gewähren. Diese Schritte sind identisch mit allen anderen Rollenzuweisungen.

1. Klicken Sie im Azure-Portal auf **Alle Dienste** und anschließend auf **Abonnements** .

1. Klicken Sie auf das Abonnement, für das Sie Zugriff erteilen möchten.

1. Klicken Sie auf **Zugriffssteuerung (IAM)** .

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen** , um die Rollenzuweisungen für dieses Abonnement anzuzeigen.

    ![Zugriffssteuerung (IAM) und Registerkarte „Rollenzuweisungen“](./media/role-assignments-portal/role-assignments.png)

1. Klicken Sie auf **Hinzufügen** > **Rollenzuweisung hinzufügen** .

   Wenn Sie keine Berechtigungen zum Zuweisen von Rollen haben, ist die Option „Rollenzuweisung hinzufügen“ deaktiviert.

   ![Menü „Rollenzuweisung hinzufügen“ für ein Abonnement](./media/shared/add-role-assignment-menu.png)

    Der Bereich „Rollenzuweisung hinzufügen“ wird geöffnet.

   ![Bereich „Rollenzuweisung hinzufügen“ für ein Abonnement](./media/role-assignments-portal/add-role-assignment.png)

1. Wählen Sie in der Dropdownliste **Rolle** die Option **Besitzer** aus.

1. Wählen Sie in der Liste **Auswählen** einen Benutzer aus. Wird der Benutzer in der Liste nicht angezeigt, können Sie im Feld **Auswählen** einen Suchbegriff eingeben, um das Verzeichnis nach Anzeigenamen und E-Mail-Adressen zu durchsuchen.

1. Klicken Sie auf **Speichern** , um die Rolle zuzuweisen.

   Nach einigen Augenblicken wird dem Benutzer im Abonnementbereich die Rolle „Besitzer“ zugewiesen.

## <a name="add-a-role-assignment-for-a-managed-identity-preview"></a>Hinzufügen einer Rollenzuweisung für eine verwaltete Identität (Vorschau)

Sie können Rollenzuweisungen für eine verwaltete Identität über die zuvor beschriebene Seite **Zugriffssteuerung (IAM)** hinzufügen. Dabei beginnen Sie auf der Seite „Zugriffssteuerung (IAM)“ zunächst mit dem Bereich und wählen anschließend die verwaltete Identität und die Rolle aus. In diesem Abschnitt wird eine alternative Vorgehensweise zum Hinzufügen von Rollenzuweisungen für eine verwaltete Identität beschrieben. Dabei beginnen Sie mit der verwalteten Identität und wählen anschließend den Bereich und die Rolle aus.

> [!IMPORTANT]
> Diese alternative Vorgehensweise zum Hinzufügen einer Rollenzuweisung für eine verwaltete Identität befindet sich derzeit in der Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

### <a name="system-assigned-managed-identity"></a>Systemseitig zugewiesene verwaltete Identität

Führen Sie die folgenden Schritte aus, um einer systemseitig zugewiesenen verwalteten Identität eine Rolle zuzuweisen und dabei mit der verwalteten Identität zu beginnen:

1. Öffnen Sie im Azure-Portal eine systemseitig zugewiesene verwaltete Identität.

1. Klicken Sie im linken Menü auf **Identität** .

    ![Systemseitig zugewiesene verwaltete Identität](./media/shared/identity-system-assigned.png)

1. Klicken Sie unter **Berechtigungen** auf **Azure-Rollenzuweisungen** .

    Wurden der ausgewählten systemseitig zugewiesenen verwalteten Identität bereits Rollen zugewiesen, wird die Liste der Rollenzuweisungen angezeigt. Diese Liste enthält alle Rollenzuweisungen, für die Sie über Leseberechtigungen verfügen.

    ![Rollenzuweisungen für eine systemseitig zugewiesene verwaltete Identität](./media/shared/role-assignments-system-assigned.png)

1. Klicken Sie auf die Liste **Abonnement** , um das Abonnement zu ändern.

1. Klicken Sie auf **Rollenzuweisung hinzufügen (Vorschau)** .

1. Wählen Sie in den Dropdownlisten die Ressourcen aus, für die die Rollenzuweisung gelten soll, z. B. **Abonnement** , **Ressourcengruppe** oder Ressource.

    Wenn Sie für den ausgewählten Bereich nicht über Schreibberechtigungen für die Rollenzuweisung verfügen, wird eine Inlinemeldung angezeigt. 

1. Wählen Sie in der Dropdownliste **Rolle** eine Rolle aus, etwa **Mitwirkender für virtuelle Computer** .

   ![Bereich „Rollenzuweisung hinzufügen“ für eine systemseitig zugewiesene verwaltete Identität](./media/role-assignments-portal/add-role-assignment-with-scope.png)

1. Klicken Sie auf **Speichern** , um die Rolle zuzuweisen.

   Nach einigen Augenblicken wird der verwalteten Identität die Rolle für den ausgewählten Bereich zugewiesen.

### <a name="user-assigned-managed-identity"></a>Benutzerseitig zugewiesene verwaltete Identität

Führen Sie die folgenden Schritte aus, um einer benutzerseitig zugewiesenen verwalteten Identität eine Rolle zuzuweisen und dabei mit der verwalteten Identität zu beginnen:

1. Öffnen Sie im Azure-Portal eine benutzerseitig zugewiesene verwaltete Identität.

1. Klicken Sie im Menü auf der linken Seite auf **Azure-Rollenzuweisungen** .

    Wurden der ausgewählten benutzerseitig zugewiesenen verwalteten Identität bereits Rollen zugewiesen, wird die Liste der Rollenzuweisungen angezeigt. Diese Liste enthält alle Rollenzuweisungen, für die Sie über Leseberechtigungen verfügen.

    ![Rollenzuweisungen für eine benutzerseitig zugewiesene verwaltete Identität](./media/shared/role-assignments-user-assigned.png)

1. Klicken Sie auf die Liste **Abonnement** , um das Abonnement zu ändern.

1. Klicken Sie auf **Rollenzuweisung hinzufügen (Vorschau)** .

1. Wählen Sie in den Dropdownlisten die Ressourcen aus, für die die Rollenzuweisung gelten soll, z. B. **Abonnement** , **Ressourcengruppe** oder Ressource.

    Wenn Sie für den ausgewählten Bereich nicht über Schreibberechtigungen für die Rollenzuweisung verfügen, wird eine Inlinemeldung angezeigt. 

1. Wählen Sie in der Dropdownliste **Rolle** eine Rolle aus, etwa **Mitwirkender für virtuelle Computer** .

   ![Bereich „Rollenzuweisung hinzufügen“ für eine benutzerseitig zugewiesene verwaltete Identität](./media/role-assignments-portal/add-role-assignment-with-scope.png)

1. Klicken Sie auf **Speichern** , um die Rolle zuzuweisen.

   Nach einigen Augenblicken wird der verwalteten Identität die Rolle für den ausgewählten Bereich zugewiesen.

## <a name="remove-a-role-assignment"></a>Entfernen einer Rollenzuweisung

Wenn Sie in Azure RBAC Zugriff auf eine Azure-Ressource entfernen möchten, entfernen Sie eine Rollenzuweisung. Führen Sie die folgenden Schritte aus, um eine Rollenzuweisung zu entfernen.

1. Öffnen Sie das Blatt **Zugriffssteuerung (IAM)** für einen Bereich, z.B. für eine Verwaltungsgruppe, ein Abonnement, eine Ressourcengruppe oder eine Ressource, für das bzw. die Sie den Zugriff entfernen möchten.

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen** , um alle Rollenzuweisungen für dieses Abonnement anzuzeigen.

1. Aktivieren Sie in der Liste der Rollenzuweisungen den Sicherheitsprinzipal mit der zu entfernenden Rollenzuweisung.

   ![Zum Entfernen ausgewählte Rollenzuweisung](./media/role-assignments-portal/remove-role-assignment-select.png)

1. Klicken Sie auf **Entfernen** .

   ![Nachricht zum Entfernen der Rollenzuweisung](./media/role-assignments-portal/remove-role-assignment.png)

1. Klicken Sie in der angezeigten Meldung zum Entfernen der Rollenzuweisung auf **Ja** .

    Wenn eine Meldung angezeigt wird, dass geerbte Rollenzuweisungen nicht entfernt werden können, versuchen Sie, eine Rollenzuweisung in einem untergeordneten Bereich zu entfernen. Öffnen Sie die Zugriffssteuerung (IAM) in dem Bereich, in dem die Rolle zugewiesen wurde, und versuchen Sie es noch mal. Eine Möglichkeit zum schnellen Öffnen der Zugriffssteuerung (IAM) im richtigen Bereich besteht darin, in der Spalte **Bereich** auf den Link neben **(Geerbt)** zu klicken.

   ![Nachricht zum Entfernen der Rollenzuweisung für geerbte Rollenzuweisungen](./media/role-assignments-portal/remove-role-assignment-inherited.png)

## <a name="next-steps"></a>Nächste Schritte

- [Auflisten von Azure-Rollenzuweisungen über das Azure-Portal](role-assignments-list-portal.md)
- [Tutorial: Gewähren des Zugriffs auf Azure-Ressourcen für einen Benutzer über das Azure-Portal](quickstart-assign-role-user-portal.md)
- [Behandeln von Problemen bei Azure RBAC](troubleshooting.md)
- [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../governance/management-groups/overview.md)
