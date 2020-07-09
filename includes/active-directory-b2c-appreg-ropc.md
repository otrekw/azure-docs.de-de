---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: f941f67105227b3973386b015344d5fa787c3485
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2020
ms.locfileid: "84317437"
---
Zum Registrieren einer Anwendung in Ihrem Azure AD B2C-Mandanten können Sie die unsere neue einheitliche Umgebung **App-Registrierungen** oder die alte Umgebung **Anwendungen (Legacy)** verwenden. [Weitere Informationen zur neuen Oberfläche](https://aka.ms/b2cappregtraining)

#### <a name="app-registrations"></a>[App-Registrierungen](#tab/app-reg-ga/)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie im linken Menü die Option **Azure AD B2C** aus. Oder wählen Sie **Alle Dienste** aus, suchen Sie nach dem Eintrag **Azure AD B2C**, und wählen Sie ihn aus.
1. Wählen Sie **App-Registrierungen** aus, und wählen Sie dann **Registrierung einer neuen Anwendung** aus.
1. Geben Sie unter **Name** einen Namen für die Anwendung ein. Zum Beispiel *ROPC_Auth_app*.
1. Übernehmen Sie die anderen Werte unverändert, und wählen Sie dann **Registrieren** aus.
1. Notieren Sie sich die **Anwendungs-ID (Client)** zur Verwendung in einem späteren Schritt.
1. Wählen Sie unter **Verwalten** die Option **Authentifizierung** aus.
1. Wählen Sie **Neue Benutzeroberfläche ausprobieren** aus (sofern die Option angezeigt wird).
1. Wählen Sie unter **Standardclienttyp** die Option **Ja** aus, um die Anwendung als öffentlichen Client einzustufen. Diese Einstellung ist für den ROPC-Flow erforderlich.
1. Wählen Sie **Speichern** aus.
1. Wählen Sie im linken Menü **Manifest** aus, um den Manifest-Editor zu öffnen. 
1. Legen Sie das Attribut **oauth2AllowImplicitFlow** auf *true* fest:
    ```json
    "oauth2AllowImplicitFlow": true,
    ```
1. Wählen Sie **Speichern** aus.

#### <a name="applications-legacy"></a>[Anwendungen (Legacy)](#tab/applications-legacy/)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie im linken Menü die Option **Azure AD B2C** aus. Oder wählen Sie **Alle Dienste** aus, suchen Sie nach dem Eintrag **Azure AD B2C**, und wählen Sie ihn aus.
1. Wählen Sie **Anwendungen (Legacy)** und dann **Hinzufügen** aus.
1. Geben Sie einen Namen für die Anwendung ein. Zum Beispiel *ROPC_Auth_app*.
1. Wählen Sie unter **Nativer Client** die Option **Ja** aus.
1. Übernehmen Sie die anderen Werte unverändert, und wählen Sie dann **Erstellen** aus.
1. Notieren Sie sich die **ANWENDUNGS-ID** zur Verwendung in einem späteren Schritt.