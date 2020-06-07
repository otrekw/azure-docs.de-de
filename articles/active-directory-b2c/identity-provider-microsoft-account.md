---
title: Einrichten der Registrierung und Anmeldung mit einem Microsoft-Konto
titleSuffix: Azure AD B2C
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit Microsoft-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/12/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 375c83445bb559efe5c797e583129cb1b0c2fb65
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83636907"
---
# <a name="set-up-sign-up-and-sign-in-with-a-microsoft-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem Microsoft-Konto mithilfe von Azure Active Directory B2C

## <a name="create-a-microsoft-account-application"></a>Erstellen einer Microsoft-Kontoanwendung

Um ein Microsoft-Konto als [Identitätsanbieter](openid-connect.md) in Azure Active Directory B2C (Azure AD B2C) verwenden zu können, müssen Sie im Azure AD-Mandanten eine Anwendung erstellen. Der Azure AD-Mandant ist nicht mit Ihrem Azure AD B2C-Mandanten identisch. Wenn Sie noch kein Microsoft-Konto haben, können Sie unter [https://www.live.com/](https://www.live.com/) eins erstellen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD-Mandanten enthält, indem Sie im oberen Menü den **Verzeichnis- und Abonnementfilter** und dann das Verzeichnis auswählen, das Ihren Azure AD-Mandanten enthält.
1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **App-Registrierungen**, und wählen Sie dann diese Option aus.
1. Wählen Sie **Neue Registrierung** aus.
1. Geben Sie einen **Namen** für Ihre Anwendung ein. Zum Beispiel *MSAapp1*.
1. Wählen Sie unter **Unterstützte Kontotypen** die Option **Accounts in any organizational directory (Any Azure AD directory - Multitenant) and personal Microsoft accounts (e.g. Skype, Xbox)** (Konten in allen Organisationsverzeichnissen (beliebiges Azure AD-Verzeichnis: mehrere Mandanten) und persönliche Microsoft-Konten (z. B. Skype, Xbox)) aus.

   Weitere Informationen zur Auswahl anderer Kontotypen finden Sie unter [Schnellstart: Registrieren einer Anwendung bei der Microsoft Identity Platform](../active-directory/develop/quickstart-register-app.md).
1. Klicken Sie unter **Umleitungs-URI (optional)** auf **Web**, und geben Sie `https://<tenant-name>.b2clogin.com/<tenant-name>.onmicrosoft.com/oauth2/authresp` in das Textfeld ein. Ersetzen Sie `<tenant-name>` durch den Azure AD B2C-Mandantennamen.
1. Wählen Sie **Registrieren** aus.
1. Notieren Sie die **Anwendungs-ID (Client)** , die auf der Seite „Übersicht“ der Anwendung angezeigt wird. Sie benötigen diesen Wert beim Konfigurieren des Identitätsanbieters im nächsten Abschnitt.
1. Wählen Sie **Zertifikate und Geheimnisse** aus.
1. Klicken Sie auf **Neuer geheimer Clientschlüssel**.
1. Geben Sie eine **Beschreibung** für das Geheimnis ein (z.B. *Anwendungskennwort 1*), und klicken Sie dann auf **Hinzufügen**.
1. Notieren Sie das in der Spalte **Wert** angezeigte Anwendungskennwort. Sie benötigen diesen Wert beim Konfigurieren des Identitätsanbieters im nächsten Abschnitt.

## <a name="configure-a-microsoft-account-as-an-identity-provider"></a>Konfigurieren eines Microsoft-Kontos als Identitätsanbieter

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
1. Wählen Sie **Identitätsanbieter** und dann **Microsoft-Konto** aus.
1. Geben Sie einen **Namen** ein. Beispiel: *MSA*.
1. Geben Sie für die **Client-ID** die Anwendungs-ID (Client) der Azure AD-Anwendung ein, die Sie zuvor erstellt haben.
1. Geben Sie für **Geheimer Clientschlüssel** den zuvor notierten geheimen Clientschlüssel ein.
1. Wählen Sie **Speichern** aus.
