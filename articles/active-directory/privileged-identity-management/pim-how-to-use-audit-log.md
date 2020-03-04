---
title: Anzeigen des Überwachungsberichts für Azure AD-Rollen in PIM – Azure AD | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Überwachungsverlauf für Azure AD-Rollen in Azure AD Privileged Identity Management (PIM) anzeigen.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 02/07/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: a4a2eccc02d13bf5a2dfc8bf3ceb7887e4962489
ms.sourcegitcommit: 0a9419aeba64170c302f7201acdd513bb4b346c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77498497"
---
# <a name="view-audit-history-for-azure-ad-roles-in-pim"></a>Anzeigen des Überwachungsverlaufs für Azure AD-Rollen in PIM

Im Überwachungsverlauf von Privileged Identity Management (PIM) können Sie alle Rollenzuweisungen und -aktivierungen für alle privilegierten Rollen in den letzten 30 Tagen anzeigen. Wenn Sie den vollständigen Überwachungsverlauf von Aktivitäten in Ihrer Azure Active Directory-Organisation (Azure AD-Organisation) einschließlich der Aktivitäten von Administratoren, Endbenutzern und Synchronisierungen anzeigen möchten, können Sie die [Azure Active Directory-Sicherheits- und Aktivitätsberichte](../reports-monitoring/overview-reports.md) verwenden.

## <a name="determine-your-version-of-pim"></a>Ermitteln der PIM-Version

Ab November 2019 werden die Azure AD Rollen von Privileged Identity Management auf eine neue Version aktualisiert, die dem Verhalten von Azure-Ressourcenrollen entspricht. Dadurch werden zusätzliche Features verfügbar, und es kommt zu [Änderungen an der vorhandenen API](azure-ad-roles-features.md#api-changes). Welche Verfahren Sie in diesem Artikel während der Einführung der neuen Version befolgen, hängt von Ihrer aktuellen Version von Privileged Identity Management ab. Führen Sie die Schritte in diesem Abschnitt aus, um Ihre Version von Privileged Identity Management zu ermitteln. Nachdem Sie Ihre Version von Privileged Identity Management kennen, können Sie die Verfahren in diesem Artikel auswählen, die dieser Version entsprechen.

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com/) als Benutzer mit der Rolle [Administrator für privilegierte Rollen](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) an.
1. Öffnen Sie **Azure AD Privileged Identity Management**. Wenn sich oben auf der Übersichtsseite ein Banner befindet, befolgen Sie die Anweisungen auf der Registerkarte **Neue Version** dieses Artikels. Andernfalls befolgen Sie die Anweisungen auf der Registerkarte **Vorherige Version**.

    [![](media/pim-how-to-add-role-to-user/pim-new-version.png "Select Azure AD > Privileged Identity Management")](media/pim-how-to-add-role-to-user/pim-new-version.png#lightbox)

# <a name="new-version"></a>[Neue Version](#tab/new)

Führen Sie die folgenden Schritte aus, um den Überwachungsverlauf für Azure AD-Rollen anzuzeigen.

## <a name="view-resource-audit-history"></a>Anzeigen des Ressourcenüberwachungsverlaufs

Mit der Ressourcenüberwachung erhalten Sie eine Übersicht über alle Aktivitäten, die Ihren Azure AD Rollen zugeordnet sind.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Wählen Sie **Azure AD-Rollen** aus.

1. Wählen Sie **Ressourcenüberwachung** aus.

1. Filtern Sie den Verlauf nach einem vordefinierten Datum oder nach einem benutzerdefinierten Bereich.

    ![Ressourcenüberwachungsliste mit Filtern](media/pim-how-to-use-audit-log/resource-audit.png)

## <a name="view-my-audit"></a>Anzeigen der eigenen Überwachung

In der eigenen Überwachung können Sie Ihre persönliche Rollenaktivität anzeigen.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Wählen Sie **Azure AD-Rollen** aus.

1. Wählen Sie die Ressource aus, für die Sie den Überwachungsverlauf anzeigen möchten.

1. Wählen Sie **Meine Überwachung** aus.

1. Filtern Sie den Verlauf nach einem vordefinierten Datum oder nach einem benutzerdefinierten Bereich.

    ![Überwachungsliste für den aktuellen Benutzer](media/pim-how-to-use-audit-log/audit-time-span.png)

# <a name="previous-version"></a>[Vorherige Version](#tab/previous)

## <a name="view-audit-history"></a>Anzeigen des Überwachungsverlaufs

Führen Sie die folgenden Schritte aus, um den Überwachungsverlauf für Azure AD-Rollen anzuzeigen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) mit einem Benutzer an, der ein Mitglied der Rolle [Administrator für privilegierte Rollen](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) ist.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Wählen Sie **Azure AD-Rollen** aus.

1. Wählen Sie **Verlauf der Verzeichnisrollenüberwachung** aus.

    Abhängig von Ihrem Überwachungsverlauf wird ein Säulendiagramm zusammen mit der Gesamtzahl der Aktivierungen, der maximalen Aktivierungen pro Tag und der durchschnittlichen Aktivierungen pro Tag angezeigt.

    ![Verlauf der Verzeichnisrollenüberwachung](media/pim-how-to-use-audit-log/directory-roles-audit-history.png)

    Am unteren Rand der Seite wird eine Tabelle mit Informationen zu jeder Aktion im verfügbaren Überwachungsverlauf angezeigt. Die Spalten haben die folgende Bedeutung:

    | Column | BESCHREIBUNG |
    | --- | --- |
    | Time | Der Zeitpunkt, zu dem die Aktion erfolgt ist. |
    | Anforderer | Der Benutzer, der die Rollenaktivierung oder -änderung angefordert hat. Wenn der Wert **Azure System**lautet, suchen Sie im Azure-Überwachungsverlauf nach weiteren Informationen. |
    | Aktion | Die vom Anforderer ausgeführten Aktionen. Aktionen können „Assign“, „Unassign“, „Activate“, „Deactivate“ oder „AddedOutsidePIM“ umfassen. |
    | Member | Der Benutzer, der eine Rolle aktiviert oder einer Rolle zugewiesen ist. |
    | Role | Die Rolle, die dem Benutzer zugewiesen ist oder die durch den Benutzer aktiviert wurde. |
    | Erläuterung | Text, der während der Aktivierung in das Feld „Grund“ eingegeben wurde. |
    | Ablauf | Zeitpunkt, an dem die aktivierte Rolle abläuft. Gilt nur für berechtigte Rollenzuweisungen. |

1. Um den Überwachungsverlauf zu sortieren, klicken Sie auf **Zeit**, **Aktion** und **Rolle**.

## <a name="filter-audit-history"></a>Filtern des Überwachungsverlaufs

1. Klicken Sie am oberen Rand der Seite mit dem Überwachungsverlauf auf die Schaltfläche **Filter**.

    Der Bereich **Diagrammparameter aktualisieren** wird angezeigt.

1. Wählen Sie in **Zeitbereich** einen Zeitraum aus.

1. Aktivieren Sie unter **Rollen** die Kontrollkästchen für die Rollen, die Sie anzeigen möchten.

    ![Bereich „Diagrammparameter aktualisieren“](media/pim-how-to-use-audit-log/update-chart-parameters.png)

1. Wählen Sie **Fertig** aus, um den gefilterten Überwachungsverlauf anzuzeigen.

---

## <a name="next-steps"></a>Nächste Schritte

- [Anzeigen von Aktivitäten und des Überwachungsverlaufs für Azure-Ressourcenrollen in Privileged Identity Management](azure-pim-resource-rbac.md)
