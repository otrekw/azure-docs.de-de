---
title: Konfigurieren der Twitter-Authentifizierung
description: Erfahren Sie, wie Sie die Twitter-Authentifizierung als Identitätsanbieter für Ihre App Services- oder Azure Functions-App konfigurieren.
ms.assetid: c6dc91d7-30f6-448c-9f2d-8e91104cde73
ms.topic: article
ms.date: 03/29/2021
ms.custom:
- seodec18
- fasttrack-edit
ms.openlocfilehash: 6ecce954991d9f3901c54a6f87fc803b32469862
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106077974"
---
# <a name="configure-your-app-service-or-azure-functions-app-to-use-twitter-login"></a>Konfigurieren Ihrer App Service- oder Azure Functions-App zur Verwendung der Twitter-Anmeldung

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Artikel wird veranschaulicht, wie Sie Azure App Service oder Azure Functions zur Verwendung von Twitter als Authentifizierungsanbieter konfigurieren.

Sie benötigen ein Twitter-Konto mit verifizierter E-Mail-Adresse und Telefonnummer, um den in diesem Artikel beschriebenen Vorgang abzuschließen. Rufen Sie die Seite [twitter.com] auf, um ein neues Twitter-Konto zu erstellen.

## <a name="register-your-application-with-twitter"></a><a name="register"> </a>Registrieren Ihrer Anwendung für Twitter

1. Melden Sie sich beim [Azure portal] an, und wechseln Sie zu Ihrer Anwendung. Kopieren Sie die **URL**. Sie verwenden sie zur Konfiguration Ihrer Twitter-App.
1. Wechseln Sie zur [Twitter Developers]-Website, melden Sie sich mit den Anmeldeinformationen für Ihr Twitter-Konto an, und wählen Sie dann **App erstellen** aus.
1. Geben Sie den **App-Namen** und die **Anwendungsbeschreibung** für Ihre neue App ein. Fügen Sie die **URL** Ihrer Anwendung in das Feld **Website-URL** ein. Geben Sie im Abschnitt **Rückruf-URLs** die HTTPS-URL Ihrer App Service-App ein, und fügen Sie den Pfad `/.auth/login/twitter/callback` an. Beispiel: `https://contoso.azurewebsites.net/.auth/login/twitter/callback`.
1. Geben Sie unten auf der Seite mindestens 100 Zeichen in **Mitteilen, wie diese App verwendet wird** ein, und wählen Sie dann **Erstellen** aus. Klicken Sie im Popupfenster erneut auf **Erstellen**. Die Anwendungsdetails werden angezeigt.
1. Wählen Sie die Registerkarte **Keys and Access Tokens** .

   Notieren Sie sich diese Werte:
   - API-Schlüssel
   - Geheimer API-Schlüssel

   > [!IMPORTANT]
   > Der geheime API-Schlüssel ist ein wichtiges Sicherheitsmerkmal. Teilen Sie diesen Schlüssel mit niemandem, und geben Sie ihn nicht über Ihre App frei.

## <a name="add-twitter-information-to-your-application"></a><a name="secrets"> </a>Hinzufügen von Twitter-Informationen zu Ihrer Anwendung

1. Melden Sie sich am [Azure Pportal] an und navigieren Sie zu Ihrer App.
1. Wählen Sie **Authentifizierung** im Menü auf der linken Seite. Klicken Sie auf **Identitätsanbieter hinzufügen**.
1. Wählen Sie **Twitter** in der Dropdown-Liste der Identitätsanbieter aus. Fügen Sie die Werte von `API key` und `API secret key` ein, die Sie zuvor abgerufen haben.

    Der geheime Schlüssel wird als Slot-persistente [Anwendungs-Einstellung](./configure-common.md#configure-app-settings) mit dem Namen `TWITTER_PROVIDER_AUTHENTICATION_SECRET` gespeichert. Sie können diese Einstellung später aktualisieren, um [Schlüsseltresor-Verweise](./app-service-key-vault-references.md) zu verwenden, wenn Sie den geheimen Schlüssel im Azure Schlüsseltresor verwalten möchten.

1. Wenn dies der erste Identitätsanbieter ist, der für die Anwendung konfiguriert wurde, wird auch ein Abschnitt mit den **Einstellungen für die App-Dienst-Authentifizierung** angezeigt. Andernfalls können Sie mit dem nächsten Schritt fortfahren.
    
    Diese Optionen bestimmen, wie Ihre Anwendung auf nicht authentifizierte Anfragen reagiert, und die Standardeinstellungen leiten alle Anfragen zur Anmeldung mit diesem neuen Anbieter um. Sie können dieses Verhalten jetzt anpassen oder diese Einstellungen später über den Hauptbildschirm der **Authentifizierung** anpassen, indem Sie neben den **Authentifizierungs-Einstellungen** die Option **Bearbeiten** auswählen. Weitere Informationen zu diesen Optionen finden Sie unter [Authentifizierungs-Verlauf](overview-authentication-authorization.md#authentication-flow).

1. Klicken Sie auf **Hinzufügen**.

Sie können nun Twitter für die Authentifizierung in Ihrer App verwenden. Der Anbieter wird auf der **Authentifizierungs-** Anzeige aufgeführt. Von dort aus können Sie diese Anbieter-Konfiguration bearbeiten oder löschen.

## <a name="next-steps"></a><a name="related-content"> </a>Nächste Schritte

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- URLs. -->

[Twitter Developers]: https://go.microsoft.com/fwlink/p/?LinkId=268300
[twitter.com]: https://go.microsoft.com/fwlink/p/?LinkID=268287
[Azure portal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
