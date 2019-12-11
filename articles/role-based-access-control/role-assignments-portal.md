---
title: Hinzufügen oder Entfernen von Rollenzuweisungen mithilfe von Azure RBAC und dem Azure-Portal
description: Erfahren Sie, wie Sie den Zugriff auf Azure-Ressourcen für Benutzer, Gruppen, Dienstprinzipale und verwaltete Identitäten mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) und dem Azure-Portal verwalten.
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
ms.date: 11/25/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 17a325e96e9709b60da2f23d1dc68e3300fde80c
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74707864"
---
# <a name="add-or-remove-role-assignments-using-azure-rbac-and-the-azure-portal"></a>Hinzufügen oder Entfernen von Rollenzuweisungen mithilfe von Azure RBAC und dem Azure-Portal

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)] In diesem Artikel wird die Zuweisung von Rollen über das Azure-Portal beschrieben.

Anweisungen zum Zuweisen von Administratorrollen zu einem Benutzer in Azure Active Directory finden Sie unter [Anzeigen und Zuweisen von Administratorrollen in Azure Active Directory](../active-directory/users-groups-roles/directory-manage-roles-portal.md).

## <a name="prerequisites"></a>Voraussetzungen

Um Rollenzuweisungen hinzufügen oder entfernen zu können, benötigen Sie Folgendes:

- `Microsoft.Authorization/roleAssignments/write`- und `Microsoft.Authorization/roleAssignments/delete`-Berechtigungen, wie z.B. [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator) oder [Besitzer](built-in-roles.md#owner)

## <a name="overview-of-access-control-iam"></a>Übersicht der Zugriffssteuerung (IAM)

Auf dem Blatt **Zugriffssteuerung (IAM)** weisen Sie Rollen zu. Es wird auch als Identitäts- und Zugriffsverwaltung bezeichnet und erscheint an mehreren Stellen im Azure-Portal. Nachfolgend wird ein Beispiel für das Blatt „Zugriffssteuerung (IAM)“ für ein Abonnement gezeigt.

![Blatt „Zugriffssteuerung (IAM)“ für ein Abonnement](./media/role-assignments-portal/access-control-subscription.png)

Um das Blatt „Zugriffssteuerung (IAM)“ am effektivsten zu verwenden, ist es nützlich, beim Versuch, eine Rolle zuzuweisen, die folgenden drei Fragen zu beantworten:

1. **Wer benötigt Zugriff?**

    „Wer“ bezieht sich auf einen Benutzer, eine Gruppe, einen Dienstprinzipal oder eine verwaltete Identität. Dies wird auch als *Sicherheitsprinzipal* bezeichnet.

1. **Welche Rolle benötigen die Personen?**

    Berechtigungen sind in Rollen zusammengefasst. Sie können in einer Liste von verschiedenen [integrierten Rollen](built-in-roles.md) auswählen oder eigene benutzerdefinierte Rollen verwenden.

1. **Wo wird der Zugriff benötigt?**

    „Wo“ bezieht sich auf einen Ressourcensatz, für den der Zugriff gilt. „Wo“ kann eine Verwaltungsgruppe, ein Abonnement, eine Ressourcengruppe oder eine einzelne Ressource, z.B. ein Speicherkonto, sein. Dies wird als der *Bereich* bezeichnet.

## <a name="add-a-role-assignment"></a>Hinzufügen einer Rollenzuweisung

Gehen Sie wie folgt vor, um eine Rolle für verschiedene Bereiche zuzuweisen.

1. Klicken Sie im Azure-Portal auf **Alle Dienste**, und wählen Sie anschließend den Bereich aus. Sie können beispielsweise **Verwaltungsgruppen**, **Abonnements**, **Ressourcengruppen** oder eine Ressource auswählen.

1. Klicken Sie auf die gewünschte Ressource.

1. Klicken Sie auf **Zugriffssteuerung (IAM)** .

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen**, um alle Rollenzuweisungen für diesen Bereich anzuzeigen.

1. Klicken Sie auf **Hinzufügen** > **Rollenzuweisung hinzufügen**, um den Bereich „Rollenzuweisung hinzufügen“ zu öffnen.

   Wenn Sie keine Berechtigungen zum Zuweisen von Rollen haben, ist die Option „Rollenzuweisung hinzufügen“ deaktiviert.

   ![Menü „Hinzufügen“](./media/role-assignments-portal/add-menu.png)

   ![Bereich „Rollenzuweisung hinzufügen“](./media/role-assignments-portal/add-role-assignment.png)

1. Wählen Sie in der Dropdownliste **Rolle** eine Rolle aus, etwa **Mitwirkender für virtuelle Computer**.

1. Wählen Sie in der Liste **Auswählen** einen Benutzer, eine Gruppe, einen Dienstprinzipal oder eine verwaltete Identität aus. Wird der Sicherheitsprinzipal in der Liste nicht angezeigt, können Sie im Feld **Auswählen** einen Begriff eingeben, um das Verzeichnis nach Anzeigenamen, E-Mail-Adressen und Objektbezeichner zu durchsuchen.

1. Klicken Sie auf **Speichern**, um die Rolle zuzuweisen.

   Nach einigen Augenblicken wird dem Sicherheitsprinzipal die Rolle für den Bereich zugewiesen.

## <a name="assign-a-user-as-an-administrator-of-a-subscription"></a>Zuweisen eines Benutzers als Administrator eines Abonnements

Um einen Benutzer zum Administrator eines Azure-Abonnements zu machen, weisen Sie ihm die Rolle [Besitzer](built-in-roles.md#owner) im Abonnementbereich zu. Durch die Rolle „Besitzer“ erhält der Benutzer Vollzugriff auf alle Ressourcen im Abonnement, einschließlich der Berechtigung, anderen Personen den Zugriff zu gewähren. Diese Schritte sind identisch mit allen anderen Rollenzuweisungen.

1. Klicken Sie im Azure-Portal auf **Alle Dienste** und anschließend auf **Abonnements**.

1. Klicken Sie auf das Abonnement, in dem Sie eine Rollenzuweisung hinzufügen möchten.

1. Klicken Sie auf **Zugriffssteuerung (IAM)** .

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen**, um alle Rollenzuweisungen für dieses Abonnement anzuzeigen.

1. Klicken Sie auf **Hinzufügen** > **Rollenzuweisung hinzufügen**, um den Bereich „Rollenzuweisung hinzufügen“ zu öffnen.

   Wenn Sie keine Berechtigungen zum Zuweisen von Rollen haben, ist die Option „Rollenzuweisung hinzufügen“ deaktiviert.

   ![Menü „Hinzufügen“](./media/role-assignments-portal/add-menu.png)

   ![Bereich „Rollenzuweisung hinzufügen“](./media/role-assignments-portal/add-role-assignment.png)

1. Wählen Sie in der Dropdownliste **Rolle** die Option **Besitzer** aus.

1. Wählen Sie in der Liste **Auswählen** einen Benutzer aus. Wird der Benutzer in der Liste nicht angezeigt, können Sie im Feld **Auswählen** einen Suchbegriff eingeben, um das Verzeichnis nach Anzeigenamen und E-Mail-Adressen zu durchsuchen.

1. Klicken Sie auf **Speichern**, um die Rolle zuzuweisen.

   Nach einigen Augenblicken wird dem Benutzer im Abonnementbereich die Rolle „Besitzer“ zugewiesen.

## <a name="remove-a-role-assignment"></a>Entfernen einer Rollenzuweisung

In RBAC entfernen Sie eine Rollenzuweisung, um den Zugriff zu entfernen. Führen Sie die folgenden Schritte aus, um eine Rollenzuweisung zu entfernen.

1. Öffnen Sie das Blatt **Zugriffssteuerung (IAM)** für einen Bereich, z.B. für eine Verwaltungsgruppe, ein Abonnement, eine Ressourcengruppe oder eine Ressource, für das bzw. die Sie den Zugriff entfernen möchten.

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen**, um alle Rollenzuweisungen für dieses Abonnement anzuzeigen.

1. Aktivieren Sie in der Liste der Rollenzuweisungen den Sicherheitsprinzipal mit der zu entfernenden Rollenzuweisung.

   ![Nachricht zum Entfernen der Rollenzuweisung](./media/role-assignments-portal/remove-role-assignment-select.png)

1. Klicken Sie auf **Entfernen**.

   ![Nachricht zum Entfernen der Rollenzuweisung](./media/role-assignments-portal/remove-role-assignment.png)

1. Klicken Sie in der angezeigten Meldung zum Entfernen der Rollenzuweisung auf **Ja**.

    Geerbte Rollenzuweisungen können nicht entfernt werden. Wenn Sie eine geerbte Rollenzuweisung entfernen möchten, müssen Sie dies für den Bereich durchführen, in dem die Rollenzuweisung erstellt wurde. Die Spalte **Bereich** neben **(Geerbt)** enthält einen Link, mit dem Sie zum Bereich gelangen, in dem diese Rolle zugewiesen wurde. Sie können die Rollenzuweisung entfernen, indem Sie zu dem dort aufgelisteten Bereich gehen.

   ![Nachricht zum Entfernen der Rollenzuweisung](./media/role-assignments-portal/remove-role-assignment-inherited.png)

## <a name="next-steps"></a>Nächste Schritte

- [Auflisten von Rollenzuweisungen mithilfe von Azure RBAC und dem Azure-Portal](role-assignments-list-portal.md)
- [Tutorial: Gewähren des Zugriffs auf Azure-Ressourcen für einen Benutzer mit RBAC und dem Azure-Portal](quickstart-assign-role-user-portal.md)
- [Problembehandlung von RBAC für Azure-Ressourcen](troubleshooting.md)
- [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../governance/management-groups/overview.md)
