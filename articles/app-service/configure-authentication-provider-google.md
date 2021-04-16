---
title: Konfigurieren von Google-Authentifizierung
description: Erfahren Sie, wie Sie die Google-Authentifizierung als Identitätsanbieter für Ihre App Services- oder Azure Functions-App konfigurieren.
ms.assetid: 2b2f9abf-9120-4aac-ac5b-4a268d9b6e2b
ms.topic: article
ms.date: 03/29/2021
ms.custom:
- seodec18
- fasttrack-edit
ms.openlocfilehash: f6bec32fa928e840569ed95c35a056db91ea9737
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106077991"
---
# <a name="configure-your-app-service-or-azure-functions-app-to-use-google-login"></a>Konfigurieren Ihrer App Service- oder Azure Functions-App zur Verwendung der Google-Anmeldung

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Thema wird veranschaulicht, wie Sie Azure App Service oder Azure Functions zur Verwendung von Google als Authentifizierungsanbieter konfigurieren.

Sie benötigen ein Google-Konto mit verifizierter E-Mail-Adresse, um den in diesem Thema beschriebenen Vorgang abzuschließen. Besuchen Sie die Seite [accounts.google.com](https://go.microsoft.com/fwlink/p/?LinkId=268302), um ein neues Google-Konto zu erstellen.

## <a name="register-your-application-with-google"></a><a name="register"> </a>Registrieren Ihrer Anwendung für Google

1. Gehen Sie entsprechend der Google-Dokumentation unter [Google Sign-In for server-side apps](https://developers.google.com/identity/sign-in/web/server-side-flow) vor, um eine Client-ID und einen geheimen Client Schlüssel zu erstellen: Es sind keine Codeänderungen erforderlich. Verwenden Sie lediglich die folgenden Informationen:
    - Verwenden Sie für **Authorized JavaScript Origins** den Wert `https://<app-name>.azurewebsites.net`, wobei Sie den Namen Ihrer App in *\<app-name>* angeben.
    - Verwenden Sie für **Authorized Redirect URI** den Wert `https://<app-name>.azurewebsites.net/.auth/login/google/callback`.
1. Kopieren Sie die App-ID- und App-Geheimnis-Werte.

    > [!IMPORTANT]
    > Das App-Geheimnis ist eine wichtige Sicherheitsanmeldeinformation. Teilen Sie diesen Schlüssel mit niemandem, und geben Sie ihn nicht über Ihre Anwendung weiter.

## <a name="add-google-information-to-your-application"></a><a name="secrets"> </a>Hinzufügen von Google-Informationen zu Ihrer Anwendung

1. Melden Sie sich am [Azure-Portal] an und navigieren Sie zu Ihrer App.
1. Wählen Sie im Menü auf der linken Seite die Option **Authentifizierung** aus. Klicken Sie auf **Identitätsanbieter hinzufügen**.
1. Wählen Sie **Google** in der Dropdown-Liste der Identitätsanbieter aus. Fügen Sie die Werte für App-ID und den geheimen App-Schlüssel ein, die Sie zuvor abgerufen haben.

    Der geheime Schlüssel wird als Slot-persistente [Anwendungs-Einstellung](./configure-common.md#configure-app-settings) mit dem Namen `GOOGLE_PROVIDER_AUTHENTICATION_SECRET` gespeichert. Sie können diese Einstellung später aktualisieren, um [Key Vault Verweise](./app-service-key-vault-references.md) zu verwenden, wenn Sie den geheimen Schlüssel in Azure Key Vault verwalten möchten.

1. Wenn dies der erste Identitätsanbieter ist, der für die Anwendung konfiguriert wurde, wird auch ein Abschnitt mit den **Einstellungen für die App-Dienst-Authentifizierung** angezeigt. Andernfalls können Sie mit dem nächsten Schritt fortfahren.
    
    Diese Optionen bestimmen, wie Ihre Anwendung auf nicht authentifizierte Anfragen reagiert, und die Standardeinstellungen leiten alle Anfragen zur Anmeldung mit diesem neuen Anbieter um. Sie können dieses Verhalten jetzt anpassen oder diese Einstellungen später über den Hauptbildschirm der **Authentifizierung** anpassen, indem Sie neben **Authentifizierungs-Einstellungen** die Option **Bearbeiten** auswählen. Weitere Informationen zu diesen Optionen finden Sie unter [Authentifizierungs-Fluss](overview-authentication-authorization.md#authentication-flow).

1. (Optional) Klicken Sie auf **Fortfahren: Umfänge** und fügen Sie alle von der Anwendung benötigten Umfänge hinzu. Diese werden zum Zeitpunkt der Anmeldung für browserbasierte Flows angefordert.
1. Klicken Sie auf **Hinzufügen**.

Sie können nun Google für die Authentifizierung in Ihrer App verwenden. Der Anbieter wird auf dem Bildschirm **Authentifizierung** aufgeführt. Von dort aus können Sie diese Anbieterkonfiguration bearbeiten oder löschen.

## <a name="next-steps"></a><a name="related-content"> </a>Nächste Schritte

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: https://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure portal]: https://portal.azure.com/

