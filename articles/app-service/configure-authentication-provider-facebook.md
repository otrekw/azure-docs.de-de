---
title: Konfigurieren der Facebook-Authentifizierung
description: Erfahren Sie, wie Sie die Facebook-Authentifizierung als Identitätsanbieter für Ihre App Services- oder Azure Functions-App konfigurieren.
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.topic: article
ms.date: 03/29/2021
ms.custom:
- seodec18
- fasttrack-edit
ms.openlocfilehash: bc639eaea76b3309d6ed047e73c726040da19639
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106078008"
---
# <a name="configure-your-app-service-or-azure-functions-app-to-use-facebook-login"></a>Konfigurieren Ihrer App Service- oder Azure Functions-App zur Verwendung der Facebook-Anmeldung

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Artikel wird veranschaulicht, wie Sie Azure App Service oder Azure Functions zur Verwendung von Facebook als Authentifizierungsanbieter konfigurieren.

Sie benötigen ein Facebook-Konto mit verifizierter E-Mail-Adresse und einer Mobiltelefonnummer, um den in diesem Artikel beschriebenen Vorgang abzuschließen. Rufen Sie die Seite [facebook.com]auf, um ein neues Facebook-Konto zu erstellen.

## <a name="register-your-application-with-facebook"></a><a name="register"> </a>Registrieren Ihrer Anwendung für Facebook

1. Wechseln Sie zur Website für [Facebook-Entwickler], und melden Sie sich mit den Anmeldeinformationen für Ihr Facebook-Konto an.

   Wenn Sie kein Facebook für Entwickler-Konto besitzen, wählen Sie **Get Started** (Erste Schritte) aus, und befolgen Sie die Registrierungsschritte.
1. Wählen Sie **My Apps** (Meine Apps) > **Add New App** (Neue App hinzufügen) aus.
1. Im Feld **Anzeigename**:
   1. Geben Sie einen eindeutigen Namen für Ihre App ein.
   1. Geben Sie Ihre **Kontakt-E-Mail** an.
   1. Wählen Sie **App-ID erstellen** aus.
   1. Schließen Sie die Sicherheitsüberprüfung ab.

   Das Entwickler-Dashboard für Ihre neue Facebook-App wird geöffnet.
1. Wählen Sie **Dashboard** > **Facebook Login** (Facebook-Anmeldung) > **Setup** (Einrichten) > **Web** aus.
1. Wählen Sie im linken Navigationsbereich utner **Facebook Login** (Facebook-Anmeldung) **Settings** (Einstellungen) aus.
1. Geben Sie in das Feld **Valid OAuth redirect URIs** (Gültige OAuth-Umleitungs-URIs) `https://<app-name>.azurewebsites.net/.auth/login/facebook/callback` ein. Denken Sie daran, `<app-name>` durch den Namen Ihrer Azure App Service-App zu ersetzen.
1. Klicken Sie auf **Save changes** (Änderungen speichern).
1. Wählen Sie im linken Bereich **Settings** > **Basic** (Einstellungen > Allgemein) aus. 
1. Wählen Sie im Feld **App Secret** (App-Geheimnis) den Eintrag **Show** (Anzeigen) aus. Kopieren Sie die Werte von **App ID** und **App Secret** (App-Geheimnis). Diese verwenden Sie später zum Konfigurieren Ihrer App Service-App in Azure.

   > [!IMPORTANT]
   > Der App-Schlüssel ist eine wichtige Anmeldeinformation. Teilen Sie diesen Schlüssel mit niemandem, und geben Sie ihn nicht über Ihre Anwendung weiter.
   >

1. Das zum Registrieren der Anwendung verwendete Facebook-Konto fungiert als Administrator der App. Zu diesem Zeitpunkt können sich nur Administratoren bei der Anwendung anmelden.

   Um andere Facebook-Konten zu authentifizieren, klicken Sie auf **App Review** (App-Überprüfung), und aktivieren Sie **Make \<your-app-name> public** (<Name Ihrer App> öffentlich machen), um den allgemeinen öffentlichen Zugriff auf die App mithilfe der Facebook-Authentifizierung zu aktivieren.

## <a name="add-facebook-information-to-your-application"></a><a name="secrets"> </a>Hinzufügen von Facebook-Informationen zu Ihrer Anwendung

1. Melden Sie sich am [Azure-Portal] an und navigieren Sie zu Ihrer App.
1. Wählen Sie im Menü auf der linken Seite die Option **Authentifizierung** aus. Klicken Sie auf **Identitätsanbieter hinzufügen**.
1. Wählen Sie **Facebook** in der Dropdown-Liste der Identitätsanbieter aus. Fügen Sie die Werte für App-ID und den geheimen App-Schlüssel ein, die Sie zuvor abgerufen haben.

    Der geheime Schlüssel wird als Slot-persistente [Anwendungs-Einstellung](./configure-common.md#configure-app-settings) mit dem Namen `FACEBOOK_PROVIDER_AUTHENTICATION_SECRET` gespeichert . Sie können diese Einstellung später aktualisieren, um [Key Vault Verweise](./app-service-key-vault-references.md) zu verwenden, wenn Sie den geheimen Schlüssel in Azure Key Vault verwalten möchten.

1. Wenn dies der erste Identitätsanbieter ist, der für die Anwendung konfiguriert wurde, wird auch ein Abschnitt mit den **Einstellungen für die App-Dienst-Authentifizierung** angezeigt. Andernfalls können Sie mit dem nächsten Schritt fortfahren.
    
    Diese Optionen bestimmen, wie Ihre Anwendung auf nicht authentifizierte Anfragen reagiert, und die Standardeinstellungen leiten alle Anfragen zur Anmeldung mit diesem neuen Anbieter um. Sie können dieses Verhalten jetzt anpassen oder diese Einstellungen später über den Hauptbildschirm der **Authentifizierung** anpassen, indem Sie neben **Authentifizierungs-Einstellungen** die Option **Bearbeiten** auswählen. Weitere Informationen zu diesen Optionen finden Sie unter [Authentifizierungs-Fluss](overview-authentication-authorization.md#authentication-flow).

1. (Optional) Klicken Sie auf **Fortfahren: Umfänge** und fügen Sie alle von der Anwendung benötigten Umfänge hinzu. Diese werden zum Zeitpunkt der Anmeldung für browserbasierte Flows angefordert.
1. Klicken Sie auf **Hinzufügen**.

Sie können nun Facebook für die Authentifizierung in Ihrer App verwenden. Der Anbieter wird auf dem Bildschirm **Authentifizierung** aufgeführt. Von dort aus können Sie diese Anbieterkonfiguration bearbeiten oder löschen.

## <a name="next-steps"></a><a name="related-content"> </a>Nächste Schritte

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- URLs. -->
[Facebook-Entwickler]: https://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: https://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure portal]: https://portal.azure.com/
