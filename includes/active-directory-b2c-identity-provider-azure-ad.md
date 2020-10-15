---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 04/07/2020
ms.author: mimart
ms.openlocfilehash: 818f33abfdb0655d96c0a8873a43903ee972b3bf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "82925463"
---
## <a name="register-an-azure-ad-app"></a>Registrieren einer Azure AD-App

Um die Anmeldung für Benutzer von einer bestimmten Azure AD-Organisation zu aktivieren, müssen Sie eine Anwendung im Azure AD-Mandanten der Organisation registrieren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD-Organisationsmandanten (z. B. „contoso.com“) enthält. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD-Mandanten enthält.
1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **App-Registrierungen**, und wählen Sie dann diese Option aus.
1. Wählen Sie **Neue Registrierung** aus.
1. Geben Sie einen **Namen** für Ihre Anwendung ein. Beispiel: `Azure AD B2C App`.
1. Übernehmen Sie für diese Anwendung die Standardauswahl von **Nur Konten in diesem Organisationsverzeichnis**.
1. Übernehmen Sie den Wert **Web** als **Umleitungs-URI**, geben Sie die folgende URL in Kleinbuchstaben ein, und ersetzen Sie dabei `your-B2C-tenant-name` durch den Namen Ihres Azure AD B2C-Mandanten.

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Beispiel: `https://fabrikam.b2clogin.com/fabrikam.onmicrosoft.com/oauth2/authresp`.

1. Wählen Sie **Registrieren**. Notieren Sie sich die **Anwendungs-ID (Client)** zur Verwendung in einem späteren Schritt.
1. Wählen Sie **Zertifikate & Geheimnisse** und dann **Neuer geheimer Clientschlüssel** aus.
1. Geben Sie eine **Beschreibung** für das Geheimnis ein, wählen Sie ein Ablaufdatum aus, und wählen Sie dann **Hinzufügen** aus. Notieren Sie sich den **Wert** des Geheimnisses zur Verwendung in einem späteren Schritt.

### <a name="configuring-optional-claims"></a>Konfigurieren optionaler Ansprüche

Wenn Sie die Ansprüche `family_name` und `given_name` von Azure AD erhalten möchten, können Sie optionale Ansprüche für Ihre Anwendung im Azure-Portal oder im Anwendungsmanifest konfigurieren. Weitere Informationen finden Sie unter [Bereitstellen optionaler Ansprüche für Ihre Azure AD-App](/azure/active-directory/develop/active-directory-optional-claims).

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Suchen Sie nach **Azure Active Directory**, und wählen Sie diese Option aus.
1. Wählen Sie im Abschnitt **Verwalten** die Option **App-Registrierungen** aus.
1. Wählen Sie in der Liste die Anwendung aus, für die Sie optionale Ansprüche konfigurieren möchten.
1. Wählen Sie im Abschnitt **Verwalten** die Option **Tokenkonfiguration** aus.
1. Wählen Sie **Optionalen Anspruch hinzufügen** aus.
1. Wählen Sie als **Tokentyp** die Option **ID** aus.
1. Wählen Sie die hinzuzufügenden optionalen Ansprüche (`family_name` und `given_name`) aus.
1. Klicken Sie auf **Hinzufügen**.