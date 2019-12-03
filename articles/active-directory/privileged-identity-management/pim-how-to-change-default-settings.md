---
title: Konfigurieren von Einstellungen für Azure AD-Rollen in PIM – Azure AD | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Einstellungen für Azure AD-Rollen in Azure AD Privileged Identity Management (PIM) konfigurieren.
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
ms.openlocfilehash: ee99a8e75fe8da85b1cf82623ed110991db24b66
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74021799"
---
# <a name="configure-azure-ad-role-settings-in-privileged-identity-management"></a>Konfigurieren von Einstellungen für Azure AD-Rollen in PIM

Ein Administrator für privilegierte Rollen kann Privileged Identity Management (PIM) in der Azure AD-Organisation (Azure Active Directory) anpassen und dabei auch die Art und Weise ändern, in der ein Benutzer eine berechtigte Rollenzuweisung aktiviert.

## <a name="open-role-settings"></a>Öffnen von Rolleneinstellungen

Führen Sie die folgenden Schritte aus, um die Einstellungen für eine Azure AD-Rolle zu öffnen.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Klicken Sie auf **Azure AD-Rollen**.

1. Klicken Sie auf **Einstellungen**.

    ![Azure AD-Rollen – Einstellungen](./media/pim-how-to-change-default-settings/pim-directory-roles-settings.png)

1. Klicken Sie auf **Rollen**.

1. Klicken Sie auf die Rolle, deren Einstellungen Sie konfigurieren möchten.

    ![Azure AD-Rollen – Einstellungen für Rollen](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-role.png)

    Auf der Einstellungsseite für jede Rolle können Sie verschiedene Einstellungen konfigurieren. Diese Einstellungen wirken sich nur auf Benutzer aus, die über **berechtigte** Zuweisungen verfügen, nicht über **permanente** Zuweisungen.

## <a name="activations"></a>Aktivierungen

Legen Sie mit dem Schieberegler **Aktivierungen** die maximale Zeit in Stunden fest, die eine Rolle aktiv bleibt, bevor sie abläuft. Dieser Wert kann zwischen 1 und 72 Stunden liegen.

## <a name="notifications"></a>Benachrichtigungen

Mit dem Schalter **Benachrichtigungen** können Sie angeben, ob Administratoren E-Mail-Benachrichtigungen empfangen, wenn Rollen aktiviert werden. Diese Benachrichtigungen können für die Erkennung von nicht autorisierten oder unzulässigem Aktivierungen nützlich sein.

Wenn diese Option auf **Aktivieren** festgelegt ist, werden Benachrichtigungen an folgende Administratoren gesendet:

- Administrator für privilegierte Rollen
- Sicherheitsadministrator
- Globaler Administrator

Weitere Informationen finden Sie unter [E-Mail-Benachrichtigungen in Privileged Identity Management](pim-email-notifications.md).

## <a name="incidentrequest-ticket"></a>Incident/Ticket anfordern

Mit dem Schalter **Incident/Ticket anfordern** können Sie festlegen, dass berechtigte Administratoren eine Ticketnummer angeben müssen, wenn sie ihre Rolle aktivieren. Dies kann die Überprüfung des Rollenzugriffs effektiver machen.

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Mit dem Schalter **Multi-Factor Authentication** können Sie angeben, ob Benutzer ihre Identität mit MFA verifizieren müssen, bevor sie ihre Rollen aktivieren können. Sie müssen ihre Identität nur einmal pro Sitzung verifizieren, nicht jedes Mal, wenn sie eine Rolle aktivieren. Beachten Sie zwei Tipps, wenn Sie die MFA aktivieren:

- Benutzer, die Microsoft-Konten als E-Mail-Adressen verwenden (in der Regel @outlook.com, aber nicht immer), können sich nicht für Azure Multi-Factor Authentication registrieren. Wenn Sie Benutzern mit Microsoft-Konten Rollen zuweisen möchten, sollten Sie sie zu permanenten Administratoren machen oder Multi-Factor Authentication für diese Rolle deaktivieren.
- Sie können Multi-Factor Authentication für sehr privilegierte Rollen für Azure AD und Office 365 nicht deaktivieren. Dieses Sicherheitsfeature trägt zum Schutz der folgenden Rollen bei:  
  
  - Azure Information Protection-Administrator
  - Rechnungsadministrator
  - Cloudanwendungsadministrator
  - Complianceadministrator
  - Administrator für bedingten Zugriff
  - Dynamics 365-Administrator
  - Genehmigende Person für den LockBox-Kundenzugriff
  - Verzeichnis schreiben
  - Exchange-Administrator
  - Globaler Administrator
  - Intune-Administrator
  - Power BI-Administrator
  - Administrator für privilegierte Rollen
  - Sicherheitsadministrator
  - SharePoint-Administrator
  - Skype for Business-Administrator
  - Benutzeradministrator

Weitere Informationen finden Sie unter [Multi-Factor Authentication und Privileged Identity Management](pim-how-to-require-mfa.md).

## <a name="require-approval"></a>Genehmigung anfordern

Wenn Sie die angeforderte Genehmigung zum Aktivieren einer Rolle delegieren möchten, gehen Sie wie folgt vor.

1. Legen Sie den Schalter **Genehmigung anfordern** auf **Aktiviert** fest. Der Bereich wird erweitert und zeigt Optionen zum Auswählen von genehmigenden Personen an.

    ![Azure AD-Rollen – Einstellungen: Genehmigung anfordern](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval.png)

    Wenn Sie keine genehmigenden Personen angeben, wird der Administrator für privilegierte Rollen zum Standardgenehmiger und muss dann alle Aktivierungsanforderungen für diese Rolle genehmigen.

1. Um genehmigende Personen anzugeben, klicken Sie auf **Genehmigende Personen auswählen**.

    ![Azure AD-Rollen – Einstellungen: Genehmigung anfordern](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval-select-approvers.png)

1. Wählen Sie zusätzlich zum Administrator für privilegierten Rollen eine oder mehrere genehmigende Personen aus, und klicken Sie dann auf **Auswählen**. Sie können Benutzer oder Gruppen auswählen. Es sollten mindestens zwei genehmigende Personen vorhanden sein. Auch wenn Sie sich selbst als genehmigende Person hinzufügen, können Sie eine Rollenaktivierung nicht für sich selbst genehmigen. Ihre Auswahl wird in der Liste der ausgewählten genehmigenden Personen angezeigt.

1. Wenn Sie alle gewünschten Rolleneinstellungen angegeben haben, wählen Sie **Speichern** aus, um die Änderungen zu speichern.

<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen von Azure AD-Rollen in PIM](pim-how-to-add-role-to-user.md)
- [Konfigurieren von Sicherheitswarnungen für Azure-Ressourcenrollen in Privileged Identity Management (PIM)](pim-how-to-configure-security-alerts.md)
