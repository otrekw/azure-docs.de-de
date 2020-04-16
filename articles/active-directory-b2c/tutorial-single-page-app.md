---
title: 'Tutorial: Aktivieren der Authentifizierung in einer Single-Page-Webanwendung'
titleSuffix: Azure AD B2C
description: In diesem Tutorial erfahren Sie, wie Sie Azure Active Directory B2C zum Bereitstellen von Benutzeranmeldungen für eine JavaScript-basierte Single-Page-Webanwendung verwenden.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 04/04/2020
ms.custom: mvc, seo-javascript-september2019
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: d7cd437f597fc34fe83904715fc2e459dfe4550f
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2020
ms.locfileid: "80875555"
---
# <a name="tutorial-enable-authentication-in-a-single-page-application-with-azure-ad-b2c"></a>Tutorial: Aktivieren der Authentifizierung in einer Single-Page-Webanwendung mit Azure AD B2C

In diesem Tutorial erfahren Sie, wie Sie Azure Active Directory B2C (Azure AD B2C) für die Registrierung und Anmeldung von Benutzern in einer Single-Page-Webanwendung (Single-Page Application, SPA) verwenden.

Dieses Tutorial ist der erste Teil einer zweiteiligen Reihe und beinhaltet Folgendes:

> [!div class="checklist"]
> * Hinzufügen einer Antwort-URL zu einer in Ihrem Azure AD B2C-Mandanten registrierten Anwendung
> * Herunterladen eines Codebeispiels von GitHub
> * Ändern des Codes der Beispielanwendung zur Verwendung mit Ihrem Mandanten
> * Registrieren mithilfe Ihres Benutzerablaufs für die Registrierung/Anmeldung

Im [nächsten Tutorial](tutorial-single-page-app-webapi.md) dieser Reihe wird der Web-API-Teil des Codebeispiels aktiviert.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen die folgenden Azure AD B2C-Ressourcen, bevor Sie mit den Schritten dieses Tutorials fortfahren können:

* [Azure AD B2C-Mandant](tutorial-create-tenant.md)
* In Ihrem Mandanten [registrierte Anwendung](tutorial-register-applications.md)
* Im Mandanten [erstellte Benutzerabläufe](tutorial-create-user-flows.md)

Darüber hinaus benötigen Sie in Ihrer lokalen Entwicklungsumgebung Folgendes:

* [Visual Studio Code](https://code.visualstudio.com/) oder ein anderer Code-Editor
* [Node.js](https://nodejs.org/en/download/)

## <a name="update-the-application"></a>Aktualisieren der Anwendung

Im zweiten Tutorial, das Sie zur Vorbereitung absolviert haben, wurde eine Webanwendung unter Azure AD B2C registriert. Um die Kommunikation mit dem Codebeispiel in diesem Tutorial zu ermöglichen, muss der Anwendungsregistrierung eine Antwort-URL (auch Umleitungs-URI genannt) hinzugefügt werden.

Sie können die aktuelle Benutzeroberfläche für **Anwendungen** oder unsere neue einheitliche Benutzeroberfläche **App-Registrierungen (Vorschau)** verwenden, um die Anwendung zu aktualisieren. [Weitere Informationen zur neuen Oberfläche](https://aka.ms/b2cappregintro)

#### <a name="applications"></a>[Anwendungen](#tab/applications/)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
1. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
1. Wählen Sie **Anwendungen** und anschließend die Anwendung *webapp1* aus.
1. Fügen Sie unter **Antwort-URL** Folgendes hinzu: `http://localhost:6420`.
1. Wählen Sie **Speichern** aus.
1. Notieren Sie sich auf der Eigenschaftenseite die **Anwendungs-ID**. Sie verwenden die App-ID in einem späteren Schritt, wenn Sie den Code in der Single-Page-Webanwendung aktualisieren.

#### <a name="app-registrations-preview"></a>[App-Registrierungen (Vorschau)](#tab/app-reg-preview/)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie im linken Menü die Option **Azure AD B2C** aus. Oder wählen Sie **Alle Dienste** aus, suchen Sie nach dem Eintrag **Azure AD B2C**, und wählen Sie ihn aus.
1. Wählen Sie **App-Registrierungen (Vorschau)** , die Registerkarte **Anwendungen mit Besitzer** und dann die Anwendung *webapp1* aus.
1. Wählen Sie **Authentifizierung** und dann **Neue Benutzeroberfläche ausprobieren** aus (sofern die Option angezeigt wird).
1. Wählen Sie unter **Web** den Link **URI hinzufügen** aus, geben Sie `http://localhost:6420` ein, und wählen Sie dann **Speichern** aus.
1. Wählen Sie **Übersicht**.
1. Notieren Sie sich die **Anwendungs-ID (Client)** zur Verwendung in einem späteren Schritt, wenn Sie den Code in der Single-Page-Webanwendung aktualisieren.

* * *

## <a name="get-the-sample-code"></a>Laden Sie den Beispielcode herunter

In diesem Tutorial wird ein von GitHub heruntergeladenes Codebeispiel für die Verwendung mit Ihrem B2C-Mandanten konfiguriert. Das Beispiel veranschaulicht, wie eine Single-Page-Webanwendung Azure AD B2C für die Benutzerregistrierung und -anmeldung sowie zum Aufrufen einer geschützten Web-API verwenden kann. (Die Web-API wird im nächsten Tutorial der Reihe aktiviert.)

[Laden Sie eine ZIP-Datei herunter](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip), oder klonen Sie das Beispiel aus GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

## <a name="update-the-sample"></a>Aktualisieren des Beispiels

Nachdem Sie nun das Beispiel vorliegen haben, können Sie den Code mit dem Namen Ihres Azure AD B2C-Mandanten und der Anwendungs-ID aus dem vorherigen Schritt aktualisieren.

1. Öffnen Sie die Datei *authConfig.js* im Ordner *JavaScriptSPA*.
1. Aktualisieren Sie im Objekt `msalConfig` Folgendes:
    * `clientId` mit der **Anwendungs-ID (Client)** , die Sie sich in einem früheren Schritt notiert haben
    * Den URI `authority` mit dem Namen Ihres Azure AD B2C-Mandanten und dem Namen des Benutzerablaufs für die Registrierung/Anmeldung, den Sie im Rahmen der Voraussetzungen erstellt haben (z. B. *B2C_1_signupsignin1*)

    ```javascript
    const msalConfig = {
        auth: {
            clientId: "00000000-0000-0000-0000-000000000000", // Replace this value with your Application (client) ID
            authority: "https://your-b2c-tenant.b2clogin.com/your-b2c-tenant.onmicrosoft.com/B2C_1_signupsignin1", // Update with your tenant and user flow names
            validateAuthority: false
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };
    ```

## <a name="run-the-sample"></a>Ausführen des Beispiels

1. Öffnen Sie ein Konsolenfenster, und wechseln Sie in das Verzeichnis mit dem Beispiel. Beispiel:

    ```console
    cd active-directory-b2c-javascript-msal-singlepageapp
    ```
1. Führen Sie die folgenden Befehle aus:

    ```console
    npm install && npm update
    npm start
    ```

    Im Konsolenfenster wird die Portnummer des lokal ausgeführten Node.js-Servers angezeigt:

    ```console
    Listening on port 6420...
    ```
1. Navigieren Sie zu `http://localhost:6420`, um die auf Ihrem lokalen Computer ausgeführte Webanwendung anzuzeigen.

    :::image type="content" source="media/tutorial-single-page-app/web-app-spa-01-not-logged-in.png" alt-text="Webbrowser mit lokal ausgeführter Single-Page-Webanwendung":::

### <a name="sign-up-using-an-email-address"></a>Registrieren mit einer E-Mail-Adresse

Diese Beispielanwendung unterstützt Registrierung, Anmeldung und Kennwortzurücksetzung. In diesem Tutorial registrieren Sie sich mit einer E-Mail-Adresse.

1. Wählen Sie **Anmelden** aus, um den in einem vorherigen Schritt angegebenen Benutzerablauf *B2C_1_signupsignin1* zu initiieren.
1. Von Azure AD B2C wird eine Anmeldeseite mit einem Registrierungslink angezeigt. Da Sie noch nicht über ein Konto verfügen, klicken Sie auf den Link **Jetzt registrieren**.
1. Der Registrierungsworkflow zeigt eine Seite an, über die die Identität des Benutzers anhand einer E-Mail-Adresse erfasst und überprüft wird. Darüber hinaus werden im Rahmen des Registrierungsworkflows auch das Kennwort des Benutzers sowie die angeforderten Attribute erfasst, die im Benutzerablauf definiert wurden.

    Verwenden Sie eine gültige E-Mail-Adresse, und bestätigen Sie sie mithilfe des Prüfcodes. Legen Sie ein Kennwort fest. Geben Sie Werte für die angeforderten Attribute ein.

    :::image type="content" source="media/tutorial-single-page-app/user-flow-sign-up-workflow-01.png" alt-text="Vom Azure AD B2C-Benutzerablauf angezeigte Registrierungsseite":::

1. Wählen Sie **Erstellen** aus, um im Azure AD B2C-Verzeichnis ein lokales Konto zu erstellen.

Wenn Sie **Erstellen** auswählen, wird von der Anwendung der Name des angemeldeten Benutzers angezeigt.

:::image type="content" source="media/tutorial-single-page-app/web-app-spa-02-logged-in.png" alt-text="Webbrowser mit Single-Page-Webanwendung und angemeldetem Benutzer":::

Wenn Sie die Anmeldung testen möchten, klicken Sie auf die Schaltfläche **Abmelden**, wählen Sie **Anmelden** aus, und melden Sie sich mit der bei der Registrierung eingegebenen E-Mail-Adresse und dem entsprechenden Kennwort an.

### <a name="what-about-calling-the-api"></a>Was ist mit dem Aufrufen der API?

Wenn Sie nach der Anmeldung die Schaltfläche **API aufrufen** auswählen, wird anstelle der Ergebnisse des API-Aufrufs die Seite des Benutzerablaufs für die Registrierung/Anmeldung angezeigt. Dies ist zu erwarten, da der API-Teil der Anwendung noch nicht für die Kommunikation mit einer Web-API-Anwendung konfiguriert wurde, die in *Ihrem* Azure AD B2C-Mandanten registriert wurde.

An diesem Punkt versucht die Anwendung immer noch, mit der im Demomandanten (fabrikamb2c.onmicrosoft.com) registrierten API zu kommunizieren, und da Sie nicht mit diesem Mandanten authentifiziert sind, wird die Registrierungs-/Anmeldeseite angezeigt.

Fahren Sie mit dem nächsten Tutorial dieser Reihe fort, um die geschützte API zu aktivieren. (Weitere Informationen finden Sie im Abschnitt [Nächste Schritte](#next-steps).)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Single-Page-Webanwendung so konfiguriert, dass sie mit einem Benutzerablauf in Ihrem Azure AD B2C-Mandanten verwendet werden kann, um eine Registrierungs- und Anmeldefunktion bereitzustellen. Dazu haben Sie folgende Schritte ausgeführt:

> [!div class="checklist"]
> * Hinzufügen einer Antwort-URL zu einer in Ihrem Azure AD B2C-Mandanten registrierten Anwendung
> * Herunterladen eines Codebeispiels von GitHub
> * Ändern des Codes der Beispielanwendung zur Verwendung mit Ihrem Mandanten
> * Registrieren mithilfe Ihres Benutzerablaufs für die Registrierung/Anmeldung

Fahren Sie jetzt mit dem nächsten Tutorial der Reihe fort, um über die Single-Page-Webanwendung Zugriff auf eine geschützte Web-API zu gewähren:

> [!div class="nextstepaction"]
> [Tutorial: Schützen einer Node.js-Web-API und Gewähren von Zugriff auf eine Node.js-Web-API über eine Single-Page-Webanwendung mithilfe von Azure AD B2C >](tutorial-single-page-app-webapi.md)
