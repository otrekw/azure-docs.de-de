---
title: 'Hinzufügen von Facebook als Identitätsanbieter: Azure AD'
description: Richten Sie einen Verbund mit Facebook ein, damit sich externe Benutzer (Gäste) mit ihren eigenen Facebook-Konten bei Ihren Azure AD-Apps anmelden können.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: a92fbd254f223e2c7eb70a4e86bb7e904294395e
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83595079"
---
# <a name="add-facebook-as-an-identity-provider-for-external-identities"></a>Hinzufügen von Facebook als Identitätsanbieter für externe Identitäten

Sie können Facebook zu Ihren Benutzerflows für die Self-Service-Registrierung (Vorschau) hinzufügen, damit sich Benutzer mit ihren eigenen Facebook-Konten bei Ihren Anwendungen anmelden können. Damit sich Benutzer mit Facebook anmelden können, müssen Sie zunächst die [Self-Service-Registrierungs](self-service-sign-up-user-flow.md) für Ihren Mandanten aktivieren. Nachdem Sie Facebook als Identitätsanbieter hinzugefügt haben, richten Sie einen Benutzerflow für die Anwendung ein und wählen Facebook als eine der Anmeldeoptionen aus.

## <a name="create-an-app-in-the-facebook-developers-console"></a>Erstellen einer App in der Facebook-Entwicklerkonsole

Um ein Facebook-Konto als [Identitätsanbieter](identity-providers.md) verwenden zu können, müssen Sie eine Anwendung in der Facebook-Entwicklerkonsole erstellen. Wenn Sie noch über kein Facebook-Konto verfügen, können Sie sich unter [https://www.facebook.com/](https://www.facebook.com) registrieren.

> [!NOTE]  
> Verwenden Sie die folgenden URLs in den unten angegebenen Schritten 9 und 16.
> - Geben Sie `https://login.microsoftonline.com` als **Website-URL** ein.
> - Geben Sie `https://login.microsoftonline.com/te/<tenant-id>/oauth2/authresp` unter **Valid OAuth redirect URIs** (Gültige OAuth-Umleitungs-URIs) ein. Ihre Verzeichnis-ID (`<tenant-ID>`) finden Sie auf dem Azure Active Directory-Übersichtsblatt.


1. Melden Sie sich auf der [Facebook-Entwickler-Website](https://developers.facebook.com/) mit den Anmeldeinformationen für Ihr Facebook-Konto an.
2. Wenn Sie dies noch nicht getan haben, müssen Sie sich als Facebook-Entwickler registrieren. Dazu wählen Sie oben rechts auf der Seite die Option **Get Started** (Erste Schritte) aus, akzeptieren die Facebook-Richtlinien und führen die Registrierungsschritte aus.
3. Wählen Sie **My Apps** (Meine Apps) und dann **Create App** (App erstellen) aus.
4. Geben Sie unter **Display Name** einen Anzeigenamen und unter **Contact Email** (Kontakt-E-Mail) eine gültige E-Mail-Adresse ein.
5. Wählen Sie **App-ID erstellen** aus. Hierzu müssen Sie möglicherweise die Richtlinien für die Facebook-Plattform akzeptieren und eine Online-Sicherheitsüberprüfung durchführen.
6. Wählen Sie **Settings** > **Basic** (Einstellungen > Allgemeines) aus.
7. Wählen Sie eine **Kategorie** aus, etwa „Business and Pages“ (Unternehmen und Seiten). Dieser Wert ist für Facebook erforderlich, wird aber nicht für Azure AD verwendet.
8. Wählen Sie unten auf der Seite die Option **Add Platform** (Plattform hinzufügen) und dann **Website** aus.
9. Geben Sie unter **Site URL** (Website-URL) die entsprechende URL ein (siehe oben).
10. Geben Sie unter **Privacy Policy URL** (Datenschutzrichtlinien-URL) die URL zur Seite mit den Datenschutzinformationen für Ihre Anwendung ein, z. B. `http://www.contoso.com`.
11. Klicken Sie auf **Save changes** (Änderungen speichern).
12. Kopieren Sie im oberen Bereich der Seite den Wert von **App ID** (App-ID).
13. Wählen Sie **Show** (Anzeigen) aus, und kopieren Sie den Wert unter **App Secret** (App-Geheimnis). Sie benötigen beide Angaben, um Facebook als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. **App Secret** handelt es sich um eine wichtige Sicherheitsinformation für die Anmeldung.
14. Wählen Sie das Pluszeichen neben **Products** (Produkte) und dann unter **Facebook Login** (Facebook-Anmeldung) die Option **Set up** (Einrichten) aus.
15. Wählen Sie unter **Facebook Login** (Facebook-Anmeldung) die Option **Settings** (Einstellungen) aus.
16. Geben Sie unter **Valid OAuth redirect URIs** (Gültige OAuth-Umleitungs-URIs) die entsprechende URL sein (siehe oben).
17. Wählen Sie unten auf der Seite die Option **Save Changes** (Änderungen speichern) aus.
18. Wählen Sie rechts oben auf der Seite den Statusselektor aus, und legen Sie ihn auf **On** (Ein) fest, um die Anwendung öffentlich und Ihre Facebook-Anwendung für Azure AD verfügbar zu machen. Wählen Sie anschließend **Switch Mode** (Modus wechseln) aus. An diesem Punkt sollte sich der Status von **Entwicklung** in **Live** ändern.
    
## <a name="configure-a-facebook-account-as-an-identity-provider"></a>Konfigurieren eines Facebook-Kontos als Identitätsanbieter

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als globaler Administrator Ihres Azure AD-Mandanten an.
2. Wählen Sie unter **Azure-Dienste** die Option **Azure Active Directory** aus.
3. Wählen Sie im Menü auf der linken Seite **Externe Identitäten** aus.
4. Wählen Sie **Alle Identitätsanbieter** und dann **Facebook** aus.
5. Geben Sie unter **Client-ID** die **App-ID** der Facebook-Anwendung ein, die Sie zuvor erstellt haben.
6. Geben Sie das zuvor notierte **App-Geheimnis** unter **Geheimer Clientschlüssel** ein.

   ![Screenshot der Seite „Soziales Netzwerk als Identitätsanbieter hinzufügen“](media/facebook-federation/add-social-identity-provider-page.png)

7. Wählen Sie **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

- [Einladen externer Benutzer für die Zusammenarbeit](add-users-administrator.md)
- [Hinzufügen eines Benutzerflows für die Self-Service-Registrierung zu einer App (Vorschau)](self-service-sign-up-user-flow.md)
