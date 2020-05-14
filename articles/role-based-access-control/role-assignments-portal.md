---
title: Hinzufügen oder Entfernen von Azure-Rollenzuweisungen über das Azure-Portal – Azure RBAC
description: Erfahren Sie, wie Sie den Zugriff auf Azure-Ressourcen für Benutzer, Gruppen, Dienstprinzipale und verwaltete Identitäten über das Azure-Portal mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) verwalten.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/25/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 88cb1e9893e0ca058e57ce12f00747a59248fb19
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2020
ms.locfileid: "82735690"
---
# <a name="add-or-remove-azure-role-assignments-using-the-azure-portal"></a>Hinzufügen oder Entfernen von Azure-Rollenzuweisungen über das Azure-Portal

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)] In diesem Artikel wird die Zuweisung von Rollen über das Azure-Portal beschrieben.

Anweisungen zum Zuweisen von Administratorrollen zu einem Benutzer in Azure Active Directory finden Sie unter [Anzeigen und Zuweisen von Administratorrollen in Azure Active Directory](../active-directory/users-groups-roles/directory-manage-roles-portal.md).

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um Rollenzuweisungen hinzufügen oder entfernen zu können:

- `Microsoft.Authorization/roleAssignments/write`- und `Microsoft.Authorization/roleAssignments/delete`-Berechtigungen, wie z.B. [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator) oder [Besitzer](built-in-roles.md#owner)

## <a name="access-control-iam"></a>Zugriffssteuerung (IAM)

Über das Blatt **Zugriffssteuerung (IAM)** weisen Sie Rollen zum Gewähren des Zugriffs auf Azure-Ressourcen zu. Es wird auch als Identitäts- und Zugriffsverwaltung bezeichnet und erscheint an mehreren Stellen im Azure-Portal. Nachfolgend wird ein Beispiel für das Blatt „Zugriffssteuerung (IAM)“ für ein Abonnement gezeigt.

![Blatt „Zugriffssteuerung (IAM)“ für ein Abonnement](./media/role-assignments-portal/access-control-subscription.png)

Um das Blatt „Zugriffssteuerung (IAM)“ am effektivsten zu verwenden, ist es nützlich, beim Versuch, eine Rolle zuzuweisen, die folgenden drei Fragen zu beantworten:

1. **Wer benötigt Zugriff?**

    „Wer“ bezieht sich auf einen Benutzer, eine Gruppe, einen Dienstprinzipal oder eine verwaltete Identität. Dies wird auch als *Sicherheitsprinzipal* bezeichnet.

1. **Welche Rolle benötigen die Personen?**

    Berechtigungen sind in Rollen zusammengefasst. Sie können aus einer Liste von verschiedenen [integrierten Rollen](built-in-roles.md) auswählen oder eigene benutzerdefinierte Rollen verwenden.

1. **Wo wird der Zugriff benötigt?**

    „Wo“ bezieht sich auf einen Ressourcensatz, für den der Zugriff gilt. „Wo“ kann eine Verwaltungsgruppe, ein Abonnement, eine Ressourcengruppe oder eine einzelne Ressource, z.B. ein Speicherkonto, sein. Dies wird als der *Bereich* bezeichnet.

## <a name="add-a-role-assignment"></a>Hinzufügen einer Rollenzuweisung

Wenn Sie in Azure RBAC Zugriff auf eine Azure-Ressource gewähren möchten, fügen Sie eine Rollenzuweisung hinzu. Gehen Sie wie folgt vor, um eine Rolle zuzuweisen:

1. Klicken Sie im Azure-Portal auf **Alle Dienste**, und wählen Sie dann den Bereich aus, auf den Sie Zugriff gewähren möchten. Sie können beispielsweise **Verwaltungsgruppen**, **Abonnements**, **Ressourcengruppen** oder eine Ressource auswählen.

1. Klicken Sie auf die gewünschte Ressource für diesen Bereich.

1. Klicken Sie auf **Zugriffssteuerung (IAM)** .

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen**, um die Rollenzuweisungen für diesen Bereich anzuzeigen.

    ![Zugriffssteuerung (IAM) und Registerkarte „Rollenzuweisungen“](./media/role-assignments-portal/role-assignments.png)

1. Klicken Sie auf **Hinzufügen** > **Rollenzuweisung hinzufügen**.

   Wenn Sie keine Berechtigungen zum Zuweisen von Rollen haben, ist die Option „Rollenzuweisung hinzufügen“ deaktiviert.

   ![Menü „Hinzufügen“](./media/role-assignments-portal/add-menu.png)

    Der Bereich „Rollenzuweisung hinzufügen“ wird geöffnet.

   ![Bereich „Rollenzuweisung hinzufügen“](./media/role-assignments-portal/add-role-assignment.png)

1. Wählen Sie in der Dropdownliste **Rolle** eine Rolle aus, etwa **Mitwirkender für virtuelle Computer**.

1. Wählen Sie in der Liste **Auswählen** einen Benutzer, eine Gruppe, einen Dienstprinzipal oder eine verwaltete Identität aus. Wird der Sicherheitsprinzipal in der Liste nicht angezeigt, können Sie im Feld **Auswählen** einen Begriff eingeben, um das Verzeichnis nach Anzeigenamen, E-Mail-Adressen und Objektbezeichner zu durchsuchen.

1. Klicken Sie auf **Speichern**, um die Rolle zuzuweisen.

   Nach einigen Augenblicken wird dem Sicherheitsprinzipal die Rolle für den Bereich zugewiesen.

    ![Hinzugefügte Rollenzuweisung gespeichert](./media/role-assignments-portal/add-role-assignment-save.png)

## <a name="assign-a-user-as-an-administrator-of-a-subscription"></a>Zuweisen eines Benutzers als Administrator eines Abonnements

Um einen Benutzer zum Administrator eines Azure-Abonnements zu machen, weisen Sie ihm die Rolle [Besitzer](built-in-roles.md#owner) im Abonnementbereich zu. Durch die Rolle „Besitzer“ erhält der Benutzer Vollzugriff auf alle Ressourcen im Abonnement, einschließlich der Berechtigung, anderen Personen den Zugriff zu gewähren. Diese Schritte sind identisch mit allen anderen Rollenzuweisungen.

1. Klicken Sie im Azure-Portal auf **Alle Dienste** und anschließend auf **Abonnements**.

1. Klicken Sie auf das Abonnement, für das Sie Zugriff erteilen möchten.

1. Klicken Sie auf **Zugriffssteuerung (IAM)** .

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen**, um die Rollenzuweisungen für dieses Abonnement anzuzeigen.

    ![Zugriffssteuerung (IAM) und Registerkarte „Rollenzuweisungen“](./media/role-assignments-portal/role-assignments.png)

1. Klicken Sie auf **Hinzufügen** > **Rollenzuweisung hinzufügen**.

   Wenn Sie keine Berechtigungen zum Zuweisen von Rollen haben, ist die Option „Rollenzuweisung hinzufügen“ deaktiviert.

   ![Menü „Hinzufügen“](./media/role-assignments-portal/add-menu.png)

    Der Bereich „Rollenzuweisung hinzufügen“ wird geöffnet.

   ![Bereich „Rollenzuweisung hinzufügen“](./media/role-assignments-portal/add-role-assignment.png)

1. Wählen Sie in der Dropdownliste **Rolle** die Option **Besitzer** aus.

1. Wählen Sie in der Liste **Auswählen** einen Benutzer aus. Wird der Benutzer in der Liste nicht angezeigt, können Sie im Feld **Auswählen** einen Suchbegriff eingeben, um das Verzeichnis nach Anzeigenamen und E-Mail-Adressen zu durchsuchen.

1. Klicken Sie auf **Speichern**, um die Rolle zuzuweisen.

   Nach einigen Augenblicken wird dem Benutzer im Abonnementbereich die Rolle „Besitzer“ zugewiesen.

## <a name="remove-a-role-assignment"></a>Entfernen einer Rollenzuweisung

Wenn Sie in Azure RBAC Zugriff auf eine Azure-Ressource entfernen möchten, entfernen Sie eine Rollenzuweisung. Führen Sie die folgenden Schritte aus, um eine Rollenzuweisung zu entfernen.

1. Öffnen Sie das Blatt **Zugriffssteuerung (IAM)** für einen Bereich, z.B. für eine Verwaltungsgruppe, ein Abonnement, eine Ressourcengruppe oder eine Ressource, für das bzw. die Sie den Zugriff entfernen möchten.

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen**, um alle Rollenzuweisungen für dieses Abonnement anzuzeigen.

1. Aktivieren Sie in der Liste der Rollenzuweisungen den Sicherheitsprinzipal mit der zu entfernenden Rollenzuweisung.

   ![Nachricht zum Entfernen der Rollenzuweisung](./media/role-assignments-portal/remove-role-assignment-select.png)

1. Klicken Sie auf **Entfernen**.

   ![Nachricht zum Entfernen der Rollenzuweisung](./media/role-assignments-portal/remove-role-assignment.png)

1. Klicken Sie in der angezeigten Meldung zum Entfernen der Rollenzuweisung auf **Ja**.

    Wenn eine Meldung angezeigt wird, dass geerbte Rollenzuweisungen nicht entfernt werden können, versuchen Sie, eine Rollenzuweisung in einem untergeordneten Bereich zu entfernen. Öffnen Sie die Zugriffssteuerung (IAM) in dem Bereich, in dem die Rolle zugewiesen wurde, und versuchen Sie es noch mal. Eine Möglichkeit zum schnellen Öffnen der Zugriffssteuerung (IAM) im richtigen Bereich besteht darin, in der Spalte **Bereich** auf den Link neben **(Geerbt)** zu klicken.

   ![Nachricht zum Entfernen der Rollenzuweisung](./media/role-assignments-portal/remove-role-assignment-inherited.png)

## <a name="next-steps"></a>Nächste Schritte

- [Auflisten von Azure-Rollenzuweisungen über das Azure-Portal](role-assignments-list-portal.md)
- [Tutorial: Gewähren des Zugriffs auf Azure-Ressourcen für einen Benutzer über das Azure-Portal](quickstart-assign-role-user-portal.md)
- [Behandeln von Problemen bei Azure RBAC](troubleshooting.md)
- [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../governance/management-groups/overview.md)
