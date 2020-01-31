---
title: Auflisten von Rollenzuweisungen mithilfe von Azure RBAC und dem Azure-Portal
description: Erfahren Sie, wie Sie mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) und dem Azure-Portal ermitteln können, auf welche Ressourcen Benutzer, Gruppen, Dienstprinzipale und verwaltete Identitäten Zugriff besitzen.
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
ms.date: 01/23/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 099cf74dd27f39a4289397d5178511125d9ebf6f
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76720710"
---
# <a name="list-role-assignments-using-azure-rbac-and-the-azure-portal"></a>Auflisten von Rollenzuweisungen mithilfe von Azure RBAC und dem Azure-Portal

[!INCLUDE [Azure RBAC definition list access](../../includes/role-based-access-control-definition-list.md)] In diesem Artikel wird beschrieben, wie Sie mit dem Azure-Portal Rollenzuweisungen auflisten.

> [!NOTE]
> Wenn Ihre Organisation Verwaltungsfunktionen an einen Dienstanbieter ausgelagert hat, der die [delegierte Azure-Ressourcenverwaltung](../lighthouse/concepts/azure-delegated-resource-management.md) verwendet, werden die von diesem Dienstanbieter autorisierten Rollenzuweisungen hier nicht angezeigt.

## <a name="list-role-assignments-for-a-user-or-group"></a>Auflisten von Rollenzuweisungen für einen Benutzer oder eine Gruppe

Die einfachste Möglichkeit, die einem Benutzer oder einer Gruppe in einem Abonnement zugewiesenen Rollen anzuzeigen, ist die Verwendung des Bereichs **Azure-Ressourcen**.

1. Klicken Sie im Azure-Portal auf **Alle Dienste**, und wählen Sie dann **Benutzer** oder **Gruppen** aus.

1. Klicken Sie auf den Benutzer oder die Gruppe, für den bzw. die Sie Rollenzuweisungen auflisten möchten.

1. Klicken Sie auf **Azure-Ressourcen**.

    Es wird eine Liste der Rollen angezeigt, die dem ausgewählten Benutzer bzw. der ausgewählten Gruppe in verschiedenen Bereichen (etwa Verwaltungsgruppe, Abonnement, Ressourcengruppe oder Ressource) zugewiesen sind. Diese Liste enthält alle Rollenzuweisungen, für die Sie über Leseberechtigungen verfügen.

    ![Rollenzuweisungen für einen Benutzer](./media/role-assignments-list-portal/azure-resources-user.png)    

1. Um das Abonnement zu ändern, klicken Sie auf die Liste **Abonnements**.

## <a name="list-owners-of-a-subscription"></a>Auflisten der Besitzer eines Abonnements

Benutzer, denen die Rolle [Besitzer](built-in-roles.md#owner) für ein Abonnement zugewiesen wurde, können alles in dem Abonnement verwalten. Befolgen Sie diese Schritte, um die Besitzer eines Abonnements aufzulisten.

1. Klicken Sie im Azure-Portal auf **Alle Dienste** und anschließend auf **Abonnements**.

1. Klicken Sie auf das Abonnement, dessen Besitzer Sie auflisten möchten.

1. Klicken Sie auf **Zugriffssteuerung (IAM)** .

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen**, um alle Rollenzuweisungen für dieses Abonnement anzuzeigen.

1. Blättern Sie zum Abschnitt **Besitzer**, um alle Benutzer anzuzeigen, denen die Rolle des Besitzers für dieses Abonnement zugewiesen wurde.

   ![Zugriffssteuerung für Abonnements: Registerkarte „Rollenzuweisungen“](./media/role-assignments-list-portal/access-control-role-assignments-subscription.png)

## <a name="list-role-assignments-at-a-scope"></a>Auflisten der Rollenzuweisungen für einen Bereich

1. Klicken Sie im Azure-Portal auf **Alle Dienste**, und wählen Sie anschließend den Bereich aus. Sie können beispielsweise **Verwaltungsgruppen**, **Abonnements**, **Ressourcengruppen** oder eine Ressource auswählen.

1. Klicken Sie auf die gewünschte Ressource.

1. Klicken Sie auf **Zugriffssteuerung (IAM)** .

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen**, um alle Rollenzuweisungen für diesen Bereich anzuzeigen.

   ![Zugriffssteuerung: Registerkarte „Rollenzuweisungen“](./media/role-assignments-list-portal/access-control-role-assignments.png)

   Auf der Registerkarte „Rollenzuweisungen“ können Sie sehen, wer Zugriff auf diesen Bereich hat. Beachten Sie, dass einige Rollen auf **Diese Ressource** begrenzt sind, während andere von einem anderen Bereich **geerbt** werden. Der Zugriff wird entweder speziell dieser Ressource zugewiesen oder von einer Zuweisung des übergeordneten Bereichs geerbt.

## <a name="list-role-assignments-for-a-user-at-a-scope"></a>Auflisten von Rollenzuweisungen für einen Benutzer für einen Bereich

Um den Zugriff für einen Benutzer, eine Gruppe, einen Dienstprinzipal oder eine verwaltete Identität aufzulisten, listen Sie die Rollenzuweisungen auf. Führen Sie diese Schritte aus, um die Rollenzuweisungen für einen einzelnen Benutzer, eine Gruppe, einen Dienstprinzipal oder eine verwaltete Identität für einen bestimmten Bereich aufzulisten.

1. Klicken Sie im Azure-Portal auf **Alle Dienste**, und wählen Sie anschließend den Bereich aus. Sie können beispielsweise **Verwaltungsgruppen**, **Abonnements**, **Ressourcengruppen** oder eine Ressource auswählen.

1. Klicken Sie auf die gewünschte Ressource.

1. Klicken Sie auf **Zugriffssteuerung (IAM)** .

1. Klicken Sie auf die Registerkarte **Zugriff überprüfen**.

    ![Zugriffssteuerung: Registerkarte „Zugriff überprüfen“](./media/role-assignments-list-portal/access-control-check-access.png)

1. Wählen Sie in der Liste **Finden** den Typ des Sicherheitsprinzipals aus, für den Sie den Zugriff überprüfen möchten.

1. Geben Sie in das Suchfeld eine Zeichenfolge ein, nach der das Verzeichnis durchsucht werden soll. Sie können nach Anzeigenamen, E-Mail-Adressen oder Objektbezeichnern suchen.

    ![Auswahlliste für Zugriffsüberprüfung](./media/role-assignments-list-portal/check-access-select.png)

1. Klicken Sie auf den Sicherheitsprinzipal, um den Bereich **Zuweisungen** zu öffnen.

    ![Bereich „Zuweisungen“](./media/role-assignments-list-portal/check-access-assignments.png)

    In diesem Bereich werden die Rollen angezeigt, die dem ausgewählten Sicherheitsprinzipal für den Bereich zugewiesen sind. Wenn Ablehnungszuweisungen für diesen Bereich oder an diesen Bereich vererbte Ablehnungszuweisungen vorhanden sind, werden diese angezeigt.

## <a name="list-role-assignments-for-a-system-assigned-managed-identity"></a>Auflisten von Rollenzuweisungen für eine systemseitig zugewiesene verwaltete Identität

1. Öffnen Sie im Azure-Portal eine systemseitig zugewiesene verwaltete Identität.

1. Klicken Sie im linken Menü auf **Identität**.

    ![Systemseitig zugewiesene verwaltete Identität](./media/role-assignments-list-portal/identity-system-assigned.png)

1. Klicken Sie unter **Rollenzuweisungen** auf **Die dieser verwalteten Identität zugewiesenen Azure RBAC-Rollen anzeigen**.

    Es wird eine Liste der Rollen angezeigt, die der ausgewählten systemseitig zugewiesenen verwalteten Identität in verschiedenen Bereichen (etwa Verwaltungsgruppe, Abonnement, Ressourcengruppe oder Ressource) zugewiesen sind. Diese Liste enthält alle Rollenzuweisungen, für die Sie über Leseberechtigungen verfügen.

    ![Rollenzuweisungen für eine systemseitig zugewiesene verwaltete Identität](./media/role-assignments-list-portal/azure-resources-system-assigned.png)

## <a name="list-role-assignments-for-a-user-assigned-managed-identity"></a>Auflisten der Rollenzuweisungen für eine benutzerseitig zugewiesene verwaltete Identität

1. Öffnen Sie im Azure-Portal eine systemseitig zugewiesene verwaltete Identität.

1. Klicken Sie auf **Azure-Ressourcen**.

    Es wird eine Liste der Rollen angezeigt, die der ausgewählten benutzerseitig zugewiesenen verwalteten Identität in verschiedenen Bereichen (etwa Verwaltungsgruppe, Abonnement, Ressourcengruppe oder Ressource) zugewiesen sind. Diese Liste enthält alle Rollenzuweisungen, für die Sie über Leseberechtigungen verfügen.

    ![Rollenzuweisungen für eine systemseitig zugewiesene verwaltete Identität](./media/role-assignments-list-portal/azure-resources-user-assigned.png)

1. Um das Abonnement zu ändern, klicken Sie auf die Liste **Abonnements**.

## <a name="list-number-of-role-assignments"></a>Auflisten der Anzahl von Rollenzuweisungen

Sie können in jedem Abonnement bis zu **2000** Rollenzuweisungen haben. Damit Sie diese Grenzwerte nachverfolgen können, enthält die Registerkarte **Rollenzuweisungen** ein Diagramm, in dem die aktuelle Anzahl der Rollenzuweisungen aufgeführt ist.

![Zugriffssteuerung: Diagramm „Anzahl der Rollenzuweisungen“](./media/role-assignments-list-portal/access-control-role-assignments-chart.png)

Wenn Sie sich der maximalen Anzahl nähern und versuchen, weitere Rollenzuweisungen hinzuzufügen, wird im Bereich **Rollenzuweisung hinzufügen** eine Warnung angezeigt. Sie können die Anzahl von Rollenzuweisungen verringern, indem Sie Rollenzuweisungen löschen, die nicht mehr benötigt werden, oder indem Sie Rollen, anstelle von einzelnen Benutzern, Gruppen zuweisen.

![Zugriffssteuerung: Warnung „Rollenzuweisung hinzufügen“](./media/role-assignments-list-portal/add-role-assignment-warning.png)

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen oder Entfernen von Rollenzuweisungen mithilfe von Azure RBAC und dem Azure-Portal](role-assignments-portal.md)
- [Problembehandlung von RBAC für Azure-Ressourcen](troubleshooting.md)
