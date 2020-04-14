---
title: Konfigurieren der Twitter-Authentifizierung
description: Erfahren Sie, wie Sie die Twitter-Authentifizierung als Identitätsanbieter für Ihre App Services- oder Azure Functions-App konfigurieren.
ms.assetid: c6dc91d7-30f6-448c-9f2d-8e91104cde73
ms.topic: article
ms.date: 02/28/2020
ms.custom:
- seodec18
- fasttrack-edit
ms.openlocfilehash: 11c913b12b4dcb7d2a5ffa532064b347b82904ef
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2020
ms.locfileid: "80519912"
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

   > [!NOTE]
   > Der geheime API-Schlüssel ist ein wichtiges Sicherheitsmerkmal. Teilen Sie diesen Schlüssel mit niemandem, und geben Sie ihn nicht über Ihre App frei.

## <a name="add-twitter-information-to-your-application"></a><a name="secrets"> </a>Hinzufügen von Twitter-Informationen zu Ihrer Anwendung

1. Wechseln Sie im [Azure portal] zu Ihrer Anwendung.
1. Wählen Sie **Einstellungen** > **Authentifizierung/Autorisierung,** aus, und stellen Sie sicher, dass für die **App Service-Authentifizierung** die Option **Ein** festgelegt ist.
1. Wählen Sie **Twitter** aus.
1. Fügen Sie die Werte von `API key` und `API secret key` ein, die Sie zuvor abgerufen haben.
1. Klicken Sie auf **OK**.

   ![Screenshot der Einstellungen für die mobile Twitter-App][1]

   Standardmäßig erfolgt die Authentifizierung über App Service, wobei jedoch der Zugriff auf die Inhalte Ihrer Website und APIs nicht autorisiert wird. Sie müssen die Benutzer in Ihrem App-Code autorisieren.

1. (Optional) Um den Zugriff auf Ihre Website ausschließlich auf Benutzer zu beschränken, die von Twitter authentifiziert wurden, legen Sie **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** auf **Twitter** fest. Wenn Sie diese Funktion festlegen, erfordert Ihre App, dass alle Anforderungen authentifiziert werden. Sie leitet außerdem alle nicht authentifizierten Anforderungen zur Authentifizierung an Twitter um.

   > [!CAUTION]
   > Das Einschränken des Zugriffs auf diese Weise gilt für alle Aufrufe Ihrer App, was für Apps, die eine öffentlich verfügbare Startseite haben, eventuell nicht wünschenswert ist, wie bei vielen Single-Page-Anwendungen. Bei solchen Anwendungen ist möglicherweise die Einstellung **Anonyme Anforderungen zulassen (keine Aktion)** vorzuziehen, sodass die App die Authentifizierung selbst manuell startet. Weitere Informationen finden Sie unter [Authentifizierungsflow](overview-authentication-authorization.md#authentication-flow).

1. Wählen Sie **Speichern** aus.

Sie können nun Twitter für die Authentifizierung in Ihrer App verwenden.

## <a name="next-steps"></a><a name="related-content"> </a>Nächste Schritte

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter Developers]: https://go.microsoft.com/fwlink/p/?LinkId=268300
[twitter.com]: https://go.microsoft.com/fwlink/p/?LinkID=268287
[Azure portal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
