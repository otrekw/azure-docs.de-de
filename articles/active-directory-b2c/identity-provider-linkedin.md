---
title: Einrichten der Registrierung und Anmeldung mit einem LinkedIn-Konto
titleSuffix: Azure AD B2C
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit LinkedIn-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 09/22/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 10c460775bcb63028f03d0e8d0b1f7ed1507cdb4
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91259457"
---
# <a name="set-up-sign-up-and-sign-in-with-a-linkedin-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem LinkedIn-Konto mithilfe von Azure Active Directory B2C

## <a name="create-a-linkedin-application"></a>Erstellen einer LinkedIn-Anwendung

Um ein LinkedIn-Konto als [Identitätsanbieter](authorization-code-flow.md) in Azure Active Directory B2C (Azure AD B2C) verwenden zu können, müssen Sie in Ihrem Mandanten eine Anwendung erstellen, die es darstellt. Wenn Sie noch über kein LinkedIn-Konto verfügen, können Sie sich unter [https://www.linkedin.com/](https://www.linkedin.com/) registrieren.

1. Melden Sie sich auf der [LinkedIn-Entwickler-Website](https://www.developer.linkedin.com/) mit den Anmeldeinformationen für Ihr LinkedIn-Konto an.
1. Wählen Sie **My Apps** (Meine Apps) aus, und klicken Sie dann auf **Create App** (App erstellen).
1. Geben Sie **App-Name**, **LinkedIn-Seite**, **URL der Datenschutzrichtlinie** und **App-Logo** ein.
1. Akzeptieren Sie die **API-Nutzungsbedingungen** von LinkedIn, und klicken Sie auf **Create app** (App erstellen).
1. Wählen Sie die Registerkarte **Authentifizierung** aus. Kopieren Sie unter **Authentication Keys** (Authentifizierungsschlüssel) die Werte für **Client ID** (Client-ID) und **Client Secret** (Geheimer Clientschlüssel). Sie benötigen beide Angaben, um LinkedIn als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. **geheime Clientschlüssel** ist eine wichtige Sicherheitsanmeldeinformation.
1. Wählen Sie neben **Authorized redirect URLs for your app** (Autorisierte Umleitungs-URLs für Ihre App) den Bearbeitungsstift und anschließend **Add redirect URL** (Umleitungs-URL hinzufügen) aus. Geben Sie `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` ein, und ersetzen Sie dabei `your-tenant-name` durch den Namen Ihres Mandanten. Bei der Eingabe Ihres Mandantennamens dürfen Sie nur Kleinbuchstaben verwenden, auch wenn der Mandant in Azure AD B2C Großbuchstaben enthält. Wählen Sie **Update** aus.
2. Standardmäßig wird Ihre LinkedIn-App nicht für Bereiche genehmigt, die mit der Anmeldung zusammenhängen. Wählen Sie zum Anfordern einer Überprüfung die Registerkarte **Products** (Produkte) und anschließend **Sign In with LinkedIn** (Mit LinkedIn anmelden) aus. Nach Abschluss der Überprüfung werden Ihrer Anwendung die erforderlichen Bereiche hinzugefügt.
   > [!NOTE]
   > Die derzeit für Ihre App zulässigen Bereiche werden auf der Registerkarte **Auth** (Authentifizierung) im Abschnitt **OAuth 2.0 scopes** (OAuth 2.0-Bereiche) angezeigt.

## <a name="configure-a-linkedin-account-as-an-identity-provider"></a>Konfigurieren eines LinkedIn-Kontos als Identitätsanbieter

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
1. Wählen Sie **Identitätsanbieter** und dann **LinkedIn** aus.
1. Geben Sie einen **Namen** ein. Beispiel: *LinkedIn*.
1. Geben Sie für die **Client-ID** die Client-ID der LinkedIn-Anwendung ein, die Sie zuvor erstellt haben.
1. Geben Sie für **Geheimer Clientschlüssel** den zuvor notierten geheimen Clientschlüssel ein.
1. Wählen Sie **Speichern** aus.

## <a name="migration-from-v10-to-v20"></a>Migration von Version 1.0 zu 2.0

LinkedIn hat kürzlich [seine APIs von Version 1.0 auf 2.0 aktualisiert](https://engineering.linkedin.com/blog/2018/12/developer-program-updates). Im Rahmen der Migration kann Azure AD B2C während der Registrierung nur den vollständigen Namen des LinkedIn-Benutzers abrufen. Wenn eine E-Mail-Adresse zu den Attributen gehört, die bei der Registrierung gesammelt werden, muss der Benutzer die E-Mail-Adresse manuell eingeben und validieren.
