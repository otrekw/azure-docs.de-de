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
ms.date: 11/25/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 25e11ae1311df9d0392340b32e0691298f78ee1c
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74709890"
---
# <a name="list-role-assignments-using-azure-rbac-and-the-azure-portal"></a>Auflisten von Rollenzuweisungen mithilfe von Azure RBAC und dem Azure-Portal

[!INCLUDE [Azure RBAC definition list access](../../includes/role-based-access-control-definition-list.md)] In diesem Artikel wird beschrieben, wie Sie mit dem Azure-Portal Rollenzuweisungen auflisten.

## <a name="list-role-assignments-for-a-user-or-group"></a>Auflisten von Rollenzuweisungen für einen Benutzer oder eine Gruppe

Die einfachste Möglichkeit, die einem Benutzer oder einer Gruppe in einem Abonnement zugewiesenen Rollen anzuzeigen, ist die Verwendung des Bereichs **Azure-Ressourcen**.

1. Klicken Sie im Azure-Portal auf **Alle Dienste**, und wählen Sie dann **Benutzer** oder **Gruppen** aus.

1. Klicken Sie auf den Benutzer oder die Gruppe, für den bzw. die Sie Rollenzuweisungen auflisten möchten.

1. Klicken Sie auf **Azure-Ressourcen**.

    Es wird eine Liste der Rollen angezeigt, die dem ausgewählten Benutzer bzw. der ausgewählten Gruppe in verschiedenen Bereichen (etwa Verwaltungsgruppe, Abonnement, Ressourcengruppe oder Ressource) zugewiesen sind. Diese Liste enthält alle Rollenzuweisungen, für die Sie über Leseberechtigungen verfügen.

    ![Rollenzuweisungen für einen Benutzer](./media/role-assignments-list-portal/azure-resources-user.png)    

1. Um das Abonnement zu ändern, klicken Sie auf die Liste **Abonnements**.

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

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen oder Entfernen von Rollenzuweisungen mithilfe von Azure RBAC und dem Azure-Portal](role-assignments-portal.md)
- [Problembehandlung von RBAC für Azure-Ressourcen](troubleshooting.md)
