---
title: 'Schnellstart: Überprüfen des Zugriffs auf Azure-Ressourcen für einen Benutzer – Azure RBAC'
description: In diesem Schnellstart erfahren Sie, wie Sie den Zugriff für sich selbst oder einen anderen Benutzer auf Azure-Ressourcen mithilfe des Azure-Portals und der rollenbasierten Zugriffssteuerung in Azure (Azure RBAC) überprüfen können.
services: role-based-access-control
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: quickstart
ms.workload: identity
ms.date: 12/09/2020
ms.author: rolyon
ms.custom: contperf-fy21q2
ms.openlocfilehash: 7cf4020ad38224b25ea8bb7dc7f0fdea7dd6f3b1
ms.sourcegitcommit: 3ea45bbda81be0a869274353e7f6a99e4b83afe2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2020
ms.locfileid: "97034002"
---
# <a name="quickstart-check-access-for-a-user-to-azure-resources"></a>Schnellstart: Überprüfen des Zugriffs auf Azure-Ressourcen für einen Benutzer

Manchmal müssen Sie überprüfen, welchen Zugriff ein Benutzer auf einen Satz von Azure-Ressourcen besitzt. Sie überprüfen seinen Zugriff, indem Sie seine Zuweisungen auflisten. Eine schnelle Möglichkeit, den Zugriff für einen einzelnen Benutzer zu überprüfen, ist die Verwendung des Features **Zugriff überprüfen** auf der Seite **Zugriffssteuerung (IAM)** .

## <a name="step-1-open-the-azure-resources"></a>Schritt 1: Öffnen von Azure-Ressourcen

Um den Zugriff für einen Benutzer zu überprüfen, müssen Sie zunächst die Azure-Ressourcen öffnen, für die Sie den Zugriff überprüfen möchten. Azure-Ressourcen werden in Ebenen organisiert, die in der Regel als *Bereich* bezeichnet werden. In Azure können Sie einen Bereich auf vier Ebenen von weit bis eng angeben: Verwaltungsgruppe, Abonnement, Ressourcengruppe und Ressource.

![Bereichsebenen für Azure RBAC](../../includes/role-based-access-control/media/scope-levels.png)

Führen Sie die folgenden Schritte aus, um den Satz der Azure-Ressourcen zu öffnen, für die Sie den Zugriff überprüfen möchten.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).

1. Öffnen Sie den Satz von Azure-Ressourcen, z. B. **Verwaltungsgruppen**, **Abonnements**, **Ressourcengruppen** oder eine bestimmte Ressource.

1. Klicken Sie auf die gewünschte Ressource in diesem Bereich.

    Nachfolgend sehen Sie ein Beispiel für eine Ressourcengruppe.

    ![Übersicht über Ressourcengruppen](./media/check-access/rg-overview.png)

## <a name="step-2-check-access-for-a-user"></a>Schritt 2: Überprüfen des Zugriffs für einen Benutzer

Führen Sie die folgenden Schritte aus, um den Zugriff für einen einzelnen Benutzer, eine Gruppe, einen Dienstprinzipal oder eine verwaltete Identität auf die zuvor ausgewählten Azure-Ressourcen zu überprüfen.

1. Klicken Sie auf **Zugriffssteuerung (IAM)**.

    Die folgende Abbildung zeigt ein Beispiel der Seite „Zugriffssteuerung (IAM)“ für eine Ressourcengruppe.

    ![Ressourcengruppen-Zugriffssteuerung: Registerkarte „Zugriff überprüfen“](./media/check-access/rg-access-control.png)

1. Wählen Sie auf der Registerkarte **Zugriff überprüfen** in der Liste **Suchen** Benutzer, Gruppen, Dienstprinzipale oder verwaltete Identitäten aus, für die Sie den Zugriff überprüfen möchten.

1. Geben Sie in das Suchfeld eine Zeichenfolge ein, nach der das Verzeichnis durchsucht werden soll. Sie können nach Anzeigenamen, E-Mail-Adressen oder Objektbezeichnern suchen.

    ![Auswahlliste für Zugriffsüberprüfung](./media/shared/rg-check-access-select.png)

1. Klicken Sie auf den Sicherheitsprinzipal, um den Bereich **Zuweisungen** zu öffnen.

    Hier können Sie den in diesem Bereich zugewiesenen und geerbten Zugriff für den ausgewählten Sicherheitsprinzipal anzeigen. Zuweisungen in untergeordneten Bereichen werden nicht aufgeführt. Folgende Zuweisungen werden angezeigt:

    - Mit Azure RBAC hinzugefügte Rollenzuweisungen.
    - Mithilfe von Azure Blueprints oder verwalteten Azure-Apps hinzugefügte Zuweisungen werden abgelehnt.
    - Klassische Dienstadministrator- oder Co-Administratorzuweisungen für klassische Bereitstellungen. 

    ![Rollen und Ablehnungsbereich für Zuweisungen für einen Benutzer](./media/shared/rg-check-access-assignments-user.png)

## <a name="step-3-check-your-access"></a>Schritt 3: Überprüfen des Zugriffs

Führen Sie die folgenden Schritte aus, um den Zugriff auf die zuvor ausgewählten Azure-Ressourcen zu überprüfen.

1. Klicken Sie auf **Zugriffssteuerung (IAM)**.

1. Klicken Sie auf der Registerkarte **Zugriff überprüfen** auf die Schaltfläche **Eigenen Zugriff anzeigen**.

    Ein Zuweisungsbereich wird angezeigt, der Ihren Zugriff in diesem Bereich auflistet sowie den an diesen Bereich vererbten Zugriff. Zuweisungen in untergeordneten Bereichen werden nicht aufgeführt.

    ![Rollen und Ablehnungsbereich für Zuweisungen](./media/check-access/rg-check-access-assignments.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Auflisten von Azure-Rollenzuweisungen über das Azure-Portal](role-assignments-list-portal.md)
