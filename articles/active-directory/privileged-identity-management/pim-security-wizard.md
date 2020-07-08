---
title: Sicherheitsassistent für Azure AD-Rollen in PIM – Azure Active Directory | Microsoft-Dokumentation
description: Beschreibt den Sicherheitsassistenten, mit dem Sie mit Azure AD Privileged Identity Management (PIM) permanent privilegierte Azure AD-Rollenzuweisungen in berechtigte Rollen konvertieren können.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: pim
ms.date: 04/21/2020
ms.author: curtand
ms.custom: pim ; H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1cc7aed1cc79a8c08a7ff11382a1c7a51455d5c3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84743659"
---
# <a name="azure-ad-roles-security-wizard-in-privileged-identity-management"></a>Sicherheitsassistent für Azure AD-Rollen in Privileged Identity Management

Wenn Sie die erste Person sind, die Privileged Identity Management (PIM) in Ihrer Azure Active Directory (Azure AD)-Organisation ausführt, wird Ihnen ein Assistent für die ersten Schritte angezeigt. Der Assistent bietet Einblick in die Sicherheitsrisiken privilegierter Identitäten und informiert darüber, wie Sie diese Risiken mithilfe von Privileged Identity Management reduzieren können. Sie müssen die vorhandenen Rollenzuweisungen im Assistenten nicht ändern, wenn Sie dies lieber später machen möchten.

> [!Important]
> Der Sicherheits-Assistent ist vorübergehend nicht verfügbar. Vielen Dank für Ihre Geduld.

## <a name="wizard-overview"></a>Übersicht über den Assistenten

Bevor Ihre Organisation mit der Verwendung von Privileged Identity Management beginnt, sind alle Rollenzuweisungen permanent, d. h. die Benutzer besitzen diese Rollen ständig, auch wenn sie die zugehörigen Berechtigungen derzeit nicht benötigen. Der erste Schritt des Assistenten bietet eine Liste der Rollen mit hohen Berechtigungen und zeigt an, wie viele Benutzer derzeit in diesen Rollen vorhanden sind. Sie können Details zu einer bestimmten Rolle anzeigen, um weitere Informationen zu den Benutzern zu erhalten und zu ermitteln, ob ein oder mehrere Benutzer unbekannt sind.

Im zweiten Schritt des Assistenten haben Sie die Möglichkeit, die Rollenzuweisungen des Administrators zu ändern.  

> [!WARNING]
> Es müssen unbedingt mindestens ein globaler Administrator sowie mehrere Administratoren für privilegierte Rollen mit einem Geschäfts-, Schul- oder Unikonto (keinem Microsoft-Konto) vorhanden sein. Wenn nur ein Administrator für privilegierte Rollen vorhanden ist, kann die Organisation Privileged Identity Management nicht mehr verwalten, sollte dieses Konto gelöscht werden.
> Verwenden Sie außerdem permanente Rollenzuweisungen, wenn ein Benutzer über ein Microsoft-Konto verfügt (also ein Konto zum Anmelden bei Microsoft-Diensten wie Skype und Outlook.com). Wenn Sie planen, zur Aktivierung dieser Rolle die mehrstufige Authentifizierung als erforderlich festzulegen, kann der Benutzer sie nicht übernehmen.

## <a name="run-the-wizard"></a>Ausführen des Assistenten

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Wählen Sie **Azure AD-Rollen** aus und dann **Assistenten**.

    ![Azure AD-Rollen – Seite des Assistenten mit den 3 Schritten zum Ausführen des Assistenten](./media/pim-security-wizard/wizard-start.png)

1. Wählen Sie **1 Ermitteln von privilegierten Rollen** aus.

1. Überprüfen Sie die Liste der Rollen, um festzustellen, welche Benutzer permanente oder berechtigte Zuweisungen haben.

    ![Privilegierte Rollen ermitteln – Bereich „Rollen“ mit permanenten und berechtigten Mitgliedern](./media/pim-security-wizard/discover-privileged-roles-users.png)

1. Wählen Sie **Weiter** aus, um die Benutzer oder Gruppen auszuwählen, die Sie in berechtige Mitglieder umwandeln möchten.

    ![Seite „Mitglieder in berechtigte Mitglieder konvertieren“ mit Optionen zum Auswählen von Mitgliedern, die in berechtigte Mitglieder für Rollen umgewandelt werden sollen](./media/pim-security-wizard/convert-members-eligible.png)

1. Wählen Sie nach dem Auswählen der Benutzer oder Gruppen die **Weiter** aus.

    ![Seite „Änderungen überprüfen“ mit Mitgliedern mit permanenten Rollenzuweisungen, die umgewandelt werden](./media/pim-security-wizard/review-changes.png)

1. Wählen Sie **OK** aus, um die permanenten in berechtigte Zuweisungen zu konvertieren.

    Nach Abschluss der Konvertierung wird eine Benachrichtigung angezeigt.

    ![Benachrichtigung mit dem Status einer Konvertierung](./media/pim-security-wizard/notification-completion.png)

Wenn Sie weitere privilegierte Rollenzuweisungen in berechtigte Zuweisungen konvertieren müssen, führen Sie den Assistenten erneut aus. Wenn Sie die Privileged Identity Management-Schnittstelle anstelle des Assistenten verwenden möchten, lesen Sie [Zuweisen von Azure AD-Rollen in Privileged Identity Management](pim-how-to-add-role-to-user.md).

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen von Azure AD-Rollen in PIM](pim-how-to-add-role-to-user.md)
- [Anderen Administratoren Zugriff für die Verwaltung von Privileged Identity Management gewähren](pim-how-to-give-access-to-pim.md).
