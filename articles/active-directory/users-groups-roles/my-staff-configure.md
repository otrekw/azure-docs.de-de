---
title: 'Verwenden von „Meine Mitarbeiter“ zum Delegieren der Benutzerverwaltung (Vorschau): Azure AD | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie die Benutzerverwaltung mit „Meine Mitarbeiter“ und mit Verwaltungseinheiten delegieren.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.topic: article
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.date: 04/14/2020
ms.author: curtand
ms.reviewer: sahenry
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 3f7c12612dbe37de6b08cb05a64af460296ade93
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393904"
---
# <a name="manage-your-users-with-my-staff-preview"></a>Verwalten Ihrer Benutzer mit „Meine Mitarbeiter“ (Vorschau)

„Meine Mitarbeiter“ ermöglicht Ihnen das Delegieren von Berechtigungen an Führungspersonen, z. B. Store Manager oder Teamleiter. Diese können dann sicherstellen, dass ihre Mitarbeiter auf die benötigten Azure AD-Konten zugreifen können. Anstatt einen zentralen Helpdesk zu verwenden, können Organisationen häufige Aufgaben an Teamleiter delegieren, z. B. das Zurücksetzen von Kennwörtern oder das Ändern von Telefonnummern. Bei Verwendung von „Meine Mitarbeiter“ kann ein Benutzer, der nicht auf sein Konto zugreifen kann, ohne die Hilfe des Helpdesks oder der IT-Abteilung mit wenigen Klicks den Zugriff wiedererlangen.

Vor dem Konfigurieren von „Meine Mitarbeiter“ für Ihre Organisation empfehlen wir Ihnen, diese Dokumentation und die [Benutzerdokumentation](../user-help/my-staff-team-manager.md) zu lesen. So können Sie sicherstellen, dass Sie mit den Funktionen und Auswirkungen dieses Features für Ihre Benutzer vertraut sind. Sie können die Benutzerdokumentation verwenden, um Ihre Benutzer zu schulen und auf die neue Oberfläche vorzubereiten. So sorgen Sie für einen erfolgreichen Rollout.

## <a name="how-my-staff-works"></a>Funktionsweise von „Meine Mitarbeiter“

„Meine Mitarbeiter“ basiert auf Verwaltungseinheiten. Hierbei handelt es sich um Container mit Ressourcen, die verwendet werden können, um den Umfang der administrativen Kontrolle einer Rollenzuweisung einzuschränken. Bei „Meine Mitarbeiter“ werden Verwaltungseinheiten genutzt, um eine Teilmenge der Benutzer einer Organisation zu definieren, z. B. nach einem Geschäft oder einer Abteilung. Anschließend kann einem Teammanager beispielsweise eine Rolle zugewiesen werden, die eine oder mehrere Verwaltungseinheiten umfasst. Im Beispiel unten wurde dem Benutzer die Administratorrolle „Authentifizierung“ gewährt, die die drei Verwaltungseinheiten umfasst. Weitere Informationen zu Verwaltungseinheiten finden Sie unter [Verwalten von Verwaltungseinheiten in Azure Active Directory](directory-administrative-units.md).

## <a name="how-to-enable-my-staff"></a>Aktivieren von „Meine Mitarbeiter“

Nachdem Sie die Verwaltungseinheiten konfiguriert haben, können Sie diesen Bereich auf Ihre Benutzer anwenden, die auf „Meine Mitarbeiter“ zugreifen. Nur Benutzer, denen eine Administratorrolle zugewiesen ist, können auf „Meine Mitarbeiter“ zugreifen. Führen Sie die folgenden Schritte aus, um „Meine Mitarbeiter“ zu aktivieren:

1. Melden Sie sich als Benutzeradministrator beim Azure-Portal an.
2. Navigieren Sie zu **Azure Active Directory** > **Benutzereinstellungen** > **Vorschauen für Benutzerfeatures** > **Vorschaueinstellungen für Benutzerfeatures verwalten**.
3. Unter **Administratoren können auf „Meine Mitarbeiter“ zugreifen** können Sie dies für alle Benutzer, ausgewählte Benutzer oder keine Benutzer aktivieren.

> [!Note]
> Nur Benutzer, denen eine Administratorrolle zugewiesen wurde, können auf „Meine Mitarbeiter“ zugreifen. Wenn Sie „Meine Mitarbeiter“ für einen Benutzer aktivieren, dem keine Administratorrolle zugewiesen ist, kann er nicht auf „Meine Mitarbeiter“ zugreifen.

## <a name="using-my-staff"></a>Verwenden von „Meine Mitarbeiter“

Wenn ein Benutzer „Meine Mitarbeiter“ aufruft, werden die Namen der [Verwaltungseinheiten](directory-administrative-units.md) angezeigt, für die er über Administratorrechte verfügt. In der [Benutzerdokumentation zu „Meine Mitarbeiter“](../user-help/my-staff-team-manager.md) verwenden wir den Begriff „Standort“ für Verwaltungseinheiten. Wenn für die Berechtigungen eines Administrators kein Umfang bzw. Bereich für Verwaltungseinheiten festgelegt ist, gelten die Berechtigungen für die gesamte Organisation. Nach dem Aktivieren von „Meine Mitarbeiter“ können die Benutzer, die zugeordnet wurden und denen eine Administratorrolle zugewiesen wurde, unter [https://mystaff.microsoft.com](https://mystaff.microsoft.com) darauf zugreifen. Sie können eine Verwaltungseinheit auswählen, um die darin enthaltenen Benutzer anzuzeigen, und dann einen Benutzer auswählen, um sein Profil zu öffnen.

## <a name="licenses"></a>Lizenzen

Jeder in „Meine Mitarbeiter“ aktivierte Benutzer muss auch dann über eine Lizenz verfügen, wenn er das Portal „Meine Mitarbeiter“ nicht verwendet. Jeder aktivierte Benutzer muss über eine der folgenden Azure AD- oder Microsoft 365-Lizenzen verfügen:

- Azure AD Premium P1 oder P2
- Microsoft 365 F1 oder F3

## <a name="reset-a-users-password"></a>Zurücksetzen des Kennworts für einen Benutzer

Die folgenden Rollen verfügen über die Berechtigung zum Zurücksetzen des Kennworts eines Benutzers:

- [Authentifizierungsadministrator](directory-assign-admin-roles.md#authentication-administrator)
- [Privilegierter Authentifizierungsadministrator](directory-assign-admin-roles.md#privileged-authentication-administrator)
- [Globaler Administrator](directory-assign-admin-roles.md#global-administrator--company-administrator)
- [Helpdeskadministrator](directory-assign-admin-roles.md#helpdesk-administrator)
- [Benutzeradministrator](directory-assign-admin-roles.md#user-administrator)
- [Kennwortadministrator](directory-assign-admin-roles.md#password-administrator)

Öffnen Sie in **Meine Mitarbeiter** das Profil eines Benutzers. Wählen Sie **Kennwort zurücksetzen** aus.

- Falls es sich um einen reinen Cloudbenutzer handelt, wird ein temporäres Kennwort angezeigt, das Sie an den Benutzer weitergeben können.
- Wenn der Benutzer über eine lokale Active Directory-Instanz synchronisiert wird, können Sie ein Kennwort eingeben, das Ihre lokalen AD-Richtlinien erfüllt. Anschließend können Sie das Kennwort an den Benutzer weitergeben.

    ![Statusanzeige und Erfolgsmeldung für Kennwortzurücksetzung](media/my-staff-configure/reset-password.png)

Der Benutzer muss sein Kennwort bei der nächsten Anmeldung ändern.

## <a name="manage-a-phone-number"></a>Verwalten einer Telefonnummer

Öffnen Sie in **Meine Mitarbeiter** das Profil eines Benutzers.

- Wählen Sie den Abschnitt **Telefonnummer hinzufügen** aus, um eine Telefonnummer für den Benutzer hinzuzufügen.
- Wählen Sie **Telefonnummer bearbeiten** aus, um die Telefonnummer zu ändern.
- Wählen Sie **Telefonnummer entfernen** aus, um die Telefonnummer für den Benutzer zu entfernen.

Je nach Ihren Einstellungen kann der Benutzer dann die von Ihnen eingerichtete Telefonnummer verwenden, um sich per SMS anzumelden, die mehrstufige Authentifizierung zu nutzen oder die Self-Service-Kennwortzurücksetzung durchzuführen.

Zum Verwalten der Telefonnummer eines Benutzers muss Ihnen eine der folgenden Rollen zugewiesen sein:

- [Authentifizierungsadministrator](directory-assign-admin-roles.md#authentication-administrator)
- [Privilegierter Authentifizierungsadministrator](directory-assign-admin-roles.md#privileged-authentication-administrator)
- [Globaler Administrator](directory-assign-admin-roles.md#global-administrator--company-administrator)

## <a name="search"></a>Suchen,

Sie können nach Verwaltungseinheiten und Benutzern in Ihrer Organisation suchen, indem Sie die Suchleiste in „Meine Mitarbeiter“ verwenden. Sie können alle Verwaltungseinheiten und Benutzer Ihrer Organisation durchsuchen, aber Sie können Änderungen für Benutzer nur in einer Verwaltungseinheit vornehmen, für die Sie über Administratorrechte verfügen.

Sie können auch innerhalb einer Verwaltungseinheit nach einem Benutzer suchen. Verwenden Sie hierzu die Suchleiste am oberen Rand der Benutzerliste.

## <a name="audit-logs"></a>Überwachungsprotokolle

Sie können Überwachungsprotokolle für Aktionen, die in „Meine Mitarbeiter“ durchgeführt werden, im Azure Active Directory-Portal anzeigen. Falls ein Überwachungsprotokoll aufgrund einer Aktion in „Meine Mitarbeiter“ generiert wurde, wird dies im Überwachungsereignis unter „ZUSÄTZLICHE DETAILS“ angegeben.

## <a name="next-steps"></a>Nächste Schritte

[Benutzerdokumentation zu „Meine Mitarbeiter“](../user-help/my-staff-team-manager.md)
[Dokumentation zu Verwaltungseinheiten](directory-administrative-units.md)
