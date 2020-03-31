---
title: Gewähren von Zugriff zum Verwalten von PIM – Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie anderen Administratoren Zugriff zum Verwalten von Azure AD Privileged Identity Management (PIM) gewähren.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7424e92f8520d13137b6ac8787523095058a005f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "74022108"
---
# <a name="grant-access-to-other-administrators-to-manage-privileged-identity-management"></a>Gewähren von Zugriff für andere Administratoren zum Verwalten von Privileged Identity Management

Der globale Administrator, der Privileged Identity Management (PIM) für eine Organisation aktiviert, erhält automatisch Rollenzuweisungen und Zugriff auf Privileged Identity Management. Standardmäßig wird jedoch keiner anderen Person in Ihrer Azure Active Directory-Organisation (Azure AD), einschließlich globaler Administratoren, Schreibzugriff gewährt. Andere globale Administratoren, Sicherheitsadministratoren und Sicherheitsleseberechtigte erhalten schreibgeschützten Zugriff auf Privileged Identity Management. Um Zugriff auf Privileged Identity Management zu gewähren, kann der erste Benutzer anderen die Rolle **Administrator für privilegierte Rollen** zuweisen.

> [!NOTE]
> Für das Verwalten von Privileged Identity Management ist Multi-Factor Authentication erforderlich. Da Microsoft-Konten nicht für Azure Multi-Factor Authentication registriert werden können, kann ein Benutzer, der sich mit einem Microsoft-Konto anmeldet, nicht auf Privileged Identity Management zugreifen.

Stellen Sie sicher, dass immer mindestens zwei Benutzer mit der Rolle „Administrator für privilegierte Rollen“ vorhanden sind, falls ein Benutzer gesperrt oder sein Konto gelöscht wird.

## <a name="grant-access-to-manage-pim"></a>Gewähren des Zugriffs zur Verwaltung von PIM

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Öffnen Sie in Azure AD **Privileged Identity Management**.

1. Wählen Sie **Azure AD-Rollen** aus.

1. Wählen Sie **Rollen** aus.

    ![„Azure AD-Rollen“ in Privileged Identity Management – Rollen](./media/pim-how-to-give-access-to-pim/pim-directory-roles-roles.png)

1. Wählen Sie die Rolle **Administrator für privilegierte Rollen** aus, um die Seite „Mitglieder“ zu öffnen.

    ![Administrator für privilegierte Rollen: Mitglieder](./media/pim-how-to-give-access-to-pim/pim-pra-members.png)

1. Wählen Sie **Mitglied hinzufügen** aus, um den Bereich „Verwaltete Mitglieder hinzufügen“ zu öffnen.

1. Wählen Sie **Mitglieder auswählen** aus, um den Bereich „Mitglieder auswählen“ zu öffnen.

    ![Administrator für privilegierte Rollen: Mitglieder auswählen](./media/pim-how-to-give-access-to-pim/pim-pra-select-members.png)

1. Wählen Sie ein Mitglied aus, und klicken Sie dann auf **Auswählen**.

1. Klicken Sie auf **OK**, damit das Mitglied für die Rolle **Administrator für privilegierte Rollen** berechtigt ist.

    Wenn Sie einem Benutzer in Privileged Identity Management eine neue Rolle zuweisen, wird er automatisch als **Berechtigt** zum Aktivieren der Rolle konfiguriert.

1. Um das Mitglied dauerhaft zu machen, wählen Sie in der Mitgliederliste „Administrator für privilegierte Rollen“ den Benutzer aus.

1. Wählen Sie **Mehr** und dann auf **Als permanent festlegen** aus, um die Zuweisung dauerhaft einzurichten.

    ![Administrator für privilegierte Rollen: Als permanent festlegen](./media/pim-how-to-give-access-to-pim/pim-pra-make-permanent.png)

1. Senden Sie an den Benutzer einen Link zu [Einstieg in Privileged Identity Management](pim-getting-started.md).

## <a name="remove-access-to-manage-pim"></a>Aufheben des Zugriffs zur Verwaltung von PIM

Bevor Sie einen Benutzer von der Rolle „Administrator für privilegierte Rollen“ entfernen, stellen Sie sicher, dass dieser Rolle danach noch mindestens zwei Benutzer zugewiesen sind.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Wählen Sie **Azure AD-Rollen** aus.

1. Wählen Sie **Rollen** aus.

1. Wählen Sie die Rolle **Administrator für privilegierte Rollen** aus, um die Seite „Mitglieder“ zu öffnen.

1. Aktivieren Sie das Kontrollkästchen neben dem Benutzer, den Sie entfernen möchten, und wählen Sie dann **Mitglied entfernen** aus.

    ![Administrator für privilegierte Rollen: Mitglied entfernen](./media/pim-how-to-give-access-to-pim/pim-pra-remove-member.png)

1. Wenn Sie aufgefordert werden zu bestätigen, dass Sie das Mitglied aus der Rolle entfernen möchten, wählen Sie **Ja** aus.

## <a name="next-steps"></a>Nächste Schritte

- [Einstieg in Privileged Identity Management](pim-getting-started.md)
