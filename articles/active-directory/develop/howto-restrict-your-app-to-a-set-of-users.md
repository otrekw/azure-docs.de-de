---
title: Einschränken von Azure AD-Apps auf eine Gruppe von Benutzern | Azure
titleSuffix: Microsoft identity platform
description: Es wird beschrieben, wie Sie den Zugriff auf Ihre Apps, die für Azure AD registriert sind, auf eine Gruppe von ausgewählten Benutzern beschränken.
services: active-directory
author: kalyankrishna1
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: how-to
ms.date: 09/24/2018
ms.author: kkrishna
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: a9b0090fa13a6d2f7a837a4f5ffee37d70893318
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88116902"
---
# <a name="how-to-restrict-your-azure-ad-app-to-a-set-of-users-in-an-azure-ad-tenant"></a>Gewusst wie: Einschränken Ihrer Azure AD-App auf eine Gruppe von Benutzern in einem Azure AD-Mandanten

Anwendungen, die für einen Azure AD-Mandanten (Azure Active Directory) registriert sind, sind standardmäßig für alle Benutzer des Mandanten verfügbar, deren Authentifizierung erfolgreich war.

Ähnlich ist es für eine [mehrinstanzenfähige](howto-convert-app-to-be-multi-tenant.md) App: Alle Benutzer des Azure AD-Mandanten, für den die App bereitgestellt wurde, können auf diese Anwendung zugreifen, nachdem sie sich erfolgreich am jeweiligen Mandanten angemeldet haben.

Für Mandantenadministratoren und Entwickler ist es häufig erforderlich, dass eine App auf eine bestimmte Gruppe von Benutzern beschränkt werden muss. Entwickler können dies erreichen, indem sie beliebte Autorisierungsmuster nutzen, z. B. die rollenbasierte Zugriffssteuerung von Azure (Azure RBAC). Dieser Ansatz ist für Entwickler aber mit einem erheblichen Arbeitsaufwand verbunden.

Mandantenadministratoren und Entwickler können auch durch Verwendung dieser integrierten Funktion von Azure AD eine App auf eine bestimmte Gruppe von Benutzern oder Sicherheitsgruppen im Mandanten beschränken.

## <a name="supported-app-configurations"></a>Unterstützte App-Konfigurationen

Die Option zum Beschränken einer App auf eine bestimmte Gruppe von Benutzern oder Sicherheitsgruppen in einem Mandanten funktioniert für die folgenden Arten von Anwendungen:

- Anwendungen, die für die einmalige Verbundanmeldung mit SAML-basierter Authentifizierung konfiguriert sind
- Anwendungsproxyanwendungen mit Verwendung von Azure AD-Vorauthentifizierung
- Anwendungen, die direkt auf der Azure AD-Anwendungsplattform mit Verwendung der OAuth 2.0/OpenID Connect-Authentifizierung erstellt werden (nachdem ein Benutzer oder Administrator seine Zustimmung für die Anwendung erteilt hat).

     > [!NOTE]
     > Dieses Feature ist nur für Web-App/Web-API und Unternehmensanwendungen verfügbar. Apps, die als [nativ](./quickstart-register-app.md) registriert wurden, können im Mandanten nicht auf eine Gruppe von Benutzern oder Sicherheitsgruppen beschränkt werden.

## <a name="update-the-app-to-enable-user-assignment"></a>Aktualisieren der App zum Aktivieren der Benutzerzuweisung

Es gibt zwei Möglichkeiten zum Erstellen einer Anwendung mit aktivierter Benutzerzuweisung. Die erste Möglichkeit erfordert die Rolle **Globaler Administrator**, die zweite nicht.

### <a name="enterprise-applications-requires-the-global-administrator-role"></a>Unternehmensanwendungen (erfordert die Rolle „Globaler Administrator“)

1. Navigieren Sie zum [**Azure-Portal**](https://portal.azure.com/), und melden Sie sich als **globaler Administrator** an.
1. Wählen Sie in der obersten Leiste das angemeldete Konto aus. 
1. Wählen Sie unter **Verzeichnis** den Azure AD-Mandanten aus, für den die App registriert werden soll.
1. Wählen Sie im Navigationsmenü auf der linken Seite die Option **Azure Active Directory**. Führen Sie die folgenden Schritte aus, wenn Azure Active Directory im Navigationsbereich nicht verfügbar ist:

    1. Wählen Sie oben im Hauptnavigationsmenü auf der linken Seite die Option **Alle Dienste**.
    1. Geben Sie im Filtersuchfeld den Namen **Azure Active Directory** ein, und wählen Sie dann in den Ergebnissen das Element **Azure Active Directory** aus.

1. Wählen Sie im Bereich **Azure Active Directory** im Navigationsmenü **Azure Active Directory** auf der linken Seite die Option **Unternehmensanwendungen**.
1. Wählen Sie **Alle Anwendungen**, um eine Liste mit Ihren Anwendungen anzuzeigen.

     Falls die gewünschte Anwendung hier nicht angezeigt wird, können Sie oben in der Liste **Alle Anwendungen** die verschiedenen Filter verwenden, um den Inhalt der Liste einzugrenzen. Sie können auch in der Liste nach unten scrollen, um nach Ihrer Anwendung zu suchen.

1. Wählen Sie in der Liste die Anwendung aus, der Sie einen Benutzer oder eine Sicherheitsgruppe zuweisen möchten.
1. Wählen Sie auf der Seite **Übersicht** der Anwendung im linken Navigationsmenü der Anwendung die Option **Eigenschaften** aus.
1. Suchen Sie nach der Einstellung **Benutzerzuweisung erforderlich?** , und legen Sie sie auf **Ja** fest. Wenn diese Option auf **Ja**festgelegt ist, müssen Benutzer im Mandanten zuerst dieser Anwendung zugewiesen werden. Andernfalls können sie sich nicht bei dieser Anwendung anmelden.
1. Wählen Sie **Speichern**, um diese Konfigurationsänderung zu speichern.

### <a name="app-registration"></a>App-Registrierung

1. Navigieren Sie zum [**Azure-Portal**](https://portal.azure.com/).
1. Wählen Sie in der obersten Leiste das angemeldete Konto aus. 
1. Wählen Sie unter **Verzeichnis** den Azure AD-Mandanten aus, für den die App registriert werden soll.
1. Wählen Sie im Navigationsmenü auf der linken Seite die Option **Azure Active Directory**.
1. Wählen Sie im Bereich **Azure Active Directory** im linken Navigationsmenü **Azure Active Directory** die Option **App-Registrierungen** aus.
1. Erstellen Sie die App, die Sie verwalten möchten, oder wählen Sie die App aus. Sie müssen der **Besitzer** dieser App-Registrierung sein.
1. Folgen Sie auf der Seite **Übersicht** der Anwendung dem Link **Verwaltete Anwendung in lokalem Verzeichnis** unter der Zusammenfassung oben auf der Seite. Dadurch gelangen Sie zur _verwalteten Unternehmensanwendung_ Ihrer App-Registrierung.
1. Wählen Sie auf dem Navigationsblatt auf der linken Seite die Option **Eigenschaften** aus.
1. Suchen Sie nach der Einstellung **Benutzerzuweisung erforderlich?** , und legen Sie sie auf **Ja** fest. Wenn diese Option auf **Ja**festgelegt ist, müssen Benutzer im Mandanten zuerst dieser Anwendung zugewiesen werden. Andernfalls können sie sich nicht bei dieser Anwendung anmelden.
1. Wählen Sie **Speichern**, um diese Konfigurationsänderung zu speichern.

## <a name="assign-users-and-groups-to-the-app"></a>Zuweisen von Benutzern und Gruppen zur App

Nachdem Sie Ihre App für die Aktivierung der Benutzerzuweisung konfiguriert haben, können Sie damit beginnen, der App Benutzer und Gruppen zuzuweisen.

1. Wählen Sie in der Anwendung im Navigationsmenü auf der linken Seite den Bereich **Benutzer und Gruppen**.
1. Wählen Sie oben in der Liste **Benutzer und Gruppen** die Schaltfläche **Benutzer hinzufügen** aus, um den Bereich **Zuweisung hinzufügen** zu öffnen.
1. Wählen Sie im Bereich **Zuweisung hinzufügen** den Selektor **Benutzer**. 

     Eine Liste mit Benutzern und Sicherheitsgruppen wird mit einem Textfeld angezeigt, mit dem bestimmte Benutzer oder Gruppen gesucht werden können. Auf diesem Bildschirm können Sie mehrere Benutzer und Gruppen auf einmal auswählen.

1. Nachdem Sie mit dem Auswählen der Benutzer und Gruppen fertig sind, können Sie unten die Schaltfläche **Auswählen** verwenden, um mit dem nächsten Teil fortzufahren.
1. (Optional) Wenn Sie App-Rollen in Ihrer Anwendung definiert haben, können mit der Option **Rolle auswählen** die ausgewählten Benutzer und Gruppen einer der Anwendungsrollen zuweisen. 
1. Wählen Sie unten die Schaltfläche **Zuweisen** aus, um die Zuweisungen von Benutzern und Gruppen zur App abzuschließen. 
1. Vergewissern Sie sich, dass die hinzugefügten Benutzer und Gruppen in der aktualisierten Liste **Benutzer und Gruppen** angezeigt werden.

## <a name="more-information"></a>Weitere Informationen

- [Vorgehensweise: Hinzufügen von App-Rollen in Ihrer Anwendung](./howto-add-app-roles-in-azure-ad-apps.md)
- [Hinzufügen von Autorisierung zu einer ASP.NET Core-Web-App mit App-Rollen und Rollenansprüchen](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/5-WebApp-AuthZ/5-1-Roles)
- [Using Security Groups and Application Roles in your apps (Video)](https://www.youtube.com/watch?v=V8VUPixLSiM) (Verwenden von Sicherheitsgruppen und Anwendungsrollen in Ihren Apps (Video))
- [Azure Active Directory, now with Group Claims and Application Roles](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-now-with-Group-Claims-and-Application/ba-p/243862) (Azure Active Directory jetzt mit Gruppenansprüchen und Anwendungsrollen)
- [Azure Active Directory-App-Manifest](./reference-app-manifest.md)