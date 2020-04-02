---
title: Anpassen der Benutzeroberfläche Ihrer App mit einer benutzerdefinierten Richtlinie
titleSuffix: Azure AD B2C
description: Erfahren Sie, wie eine Benutzeroberfläche mithilfe einer benutzerdefinierten Richtlinie in Azure Active Directory B2C angepasst werden kann.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: e50d6d0623e87dfa68a7cc9744c3f595ff0179c6
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2020
ms.locfileid: "80396385"
---
# <a name="customize-the-user-interface-of-your-application-using-a-custom-policy-in-azure-active-directory-b2c"></a>Anpassen der Benutzeroberfläche einer Anwendung mithilfe einer benutzerdefinierten Richtlinie in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Mit den Schritten in diesem Artikel erstellen Sie eine benutzerdefinierte Registrierungs- oder Anmelderichtlinie mit Ihrer Marke und Darstellung. Mit Azure Active Directory B2C (Azure AD B2C) erlangen Sie nahezu vollständige Kontrolle über den HTML- und CSS-Inhalt, der Benutzern angezeigt wird. Bei Verwendung einer benutzerdefinierten Richtlinie konfigurieren Sie die Anpassung der Benutzeroberfläche in XML, anstatt über Steuerelemente im Azure-Portal.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte unter [Erste Schritte mit benutzerdefinierten Richtlinien in Azure Active Directory B2C](custom-policy-get-started.md) aus. Sie sollten eine funktionierende benutzerdefinierte Richtlinie für die Registrierung und Anmeldung bei lokalen Konten besitzen.

[!INCLUDE [active-directory-b2c-html-how-to](../../includes/active-directory-b2c-html-how-to.md)]

### <a name="4-modify-the-extensions-file"></a>4. Ändern der Erweiterungsdatei

Um die Anpassung der Benutzeroberfläche zu konfigurieren, kopieren Sie die **ContentDefinition** und ihre untergeordneten Elemente in der Basisdatei, und fügen Sie sie in die Erweiterungsdatei ein.

1. Öffnen Sie die Basisdatei Ihrer Richtlinie. Beispiel: <em>`SocialAndLocalAccounts/` **`TrustFrameworkBase.xml`** </em>. Die Basisdatei ist eine der Richtliniendateien im Starter Pack für benutzerdefinierte Richtlinien, das Sie im unter „Voraussetzungen“ genannten Artikel [Erste Schritte mit benutzerdefinierten Richtlinien](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-get-started-custom) abgerufen haben sollten.
1. Suchen Sie nach dem Element **ContentDefinitions**, und kopieren Sie den gesamten Inhalt.
1. Öffnen Sie die Erweiterungsdatei. Beispiel: *TrustFrameworkExtensions.xml*. Suchen Sie nach dem Element **BuildingBlocks**. Wenn das Element nicht vorhanden ist, fügen Sie es hinzu.
1. Fügen Sie den gesamten Inhalt des **ContentDefinitions**-Elements ein, das Sie als untergeordnetes Element des **BuildingBlocks**-Elements kopiert haben.
1. Suchen Sie dem **ContentDefinition**-Element, das `Id="api.signuporsignin"` in der kopierten XML-Datei enthält.
1. Ändern Sie den Wert von **LoadUri** in die URL der HTML-Datei, die Sie in den Speicher hochgeladen haben. Beispiel: `https://your-storage-account.blob.core.windows.net/your-container/customize-ui.html`.

    Ihre benutzerdefinierte Richtlinie sollte wie der folgende Codeausschnitt aussehen:

    ```xml
    <BuildingBlocks>
      <ContentDefinitions>
        <ContentDefinition Id="api.signuporsignin">
          <LoadUri>https://your-storage-account.blob.core.windows.net/your-container/customize-ui.html</LoadUri>
          <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
          <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0</DataUri>
          <Metadata>
            <Item Key="DisplayName">Signin and Signup</Item>
          </Metadata>
        </ContentDefinition>
      </ContentDefinitions>
    </BuildingBlocks>
    ```

1. Speichern Sie die Erweiterungsdatei.

### <a name="5-upload-and-test-your-updated-custom-policy"></a>5. Hochladen und Testen Ihrer aktualisierten benutzerdefinierten Richtlinie

#### <a name="51-upload-the-custom-policy"></a>5.1 Hochladen der benutzerdefinierten Richtlinie

1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
1. Suchen Sie nach **Azure AD B2C**, und wählen Sie diese Option aus.
1. Wählen Sie unter **Richtlinien** die Option **Identity Experience Framework** aus.
1. Wählen Sie **Benutzerdefinierte Richtlinie hochladen** aus.
1. Laden Sie die zuvor geänderte Erweiterungsdatei hoch.

#### <a name="52-test-the-custom-policy-by-using-run-now"></a>5.2 Testen der benutzerdefinierten Richtlinie mit **Jetzt ausführen**

1. Wählen Sie die hochgeladene Richtlinie aus, und wählen Sie dann **Jetzt ausführen** aus.
1. Sie sollten sich mit einer E-Mail-Adresse registrieren können.

[!INCLUDE [active-directory-b2c-html-templates](../../includes/active-directory-b2c-html-templates.md)]

## <a name="configure-dynamic-custom-page-content-uri"></a>Konfigurieren eines URI für den dynamischen benutzerdefinierten Seiteninhalt

Mithilfe von benutzerdefinierten Azure AD B2C-Richtlinien können Sie einen Parameter im URL-Pfad oder eine Abfragezeichenfolge senden. Durch Übergeben des Parameters an Ihren HTML-Endpunkt können Sie den Seiteninhalt dynamisch ändern. Sie können z.B. das Hintergrundbild auf der Azure AD B2C-Registrierungs- oder Anmeldeseite auf der Basis eines Parameters ändern, den Sie von der Web- oder Mobilanwendung übergeben. Der Parameter kann ein beliebiger [Anspruchskonfliktlöser](claim-resolver-overview.md) sein, z. B. Anwendungs-ID, Sprach-ID oder ein benutzerdefinierter Abfragezeichenfolgeparameter wie `campaignId`.

### <a name="sending-query-string-parameters"></a>Senden von Abfragezeichenfolgeparametern

Um Abfragezeichenfolgeparameter zu senden, fügen Sie in der [Richtlinie für die vertrauende Seite](relyingparty.md) ein `ContentDefinitionParameters`-Element hinzu, wie unten gezeigt.

```XML
<RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <UserJourneyBehaviors>
    <ContentDefinitionParameters>
        <Parameter Name="campaignId">{OAUTH-KV:campaignId}</Parameter>
        <Parameter Name="lang">{Culture:LanguageName}</Parameter>
        <Parameter Name="appId">{OIDC:ClientId}</Parameter>
    </ContentDefinitionParameters>
    </UserJourneyBehaviors>
    ...
</RelyingParty>
```

Ändern Sie in der Inhaltsdefinition den Wert `LoadUri` in `https://<app_name>.azurewebsites.net/home/unified`. Ihre benutzerdefinierte Richtlinie `ContentDefinition` sollte wie der folgende Codeausschnitt aussehen:

```XML
<ContentDefinition Id="api.signuporsignin">
  <LoadUri>https://<app_name>.azurewebsites.net/home/unified</LoadUri>
  ...
</ContentDefinition>
```

Wenn Azure AD B2C die Seite lädt, wird der Endpunkt Ihres Webservers aufgerufen:

```http
https://<app_name>.azurewebsites.net/home/unified?campaignId=123&lang=fr&appId=f893d6d3-3b6d-480d-a330-1707bf80ebea
```

### <a name="dynamic-page-content-uri"></a>URI für dynamischen Seiteninhalt

Inhalt kann anhand der verwendeten Parameter von unterschiedlichen Quellen abgerufen werden. Richten Sie in Ihrem CORS-fähigen Endpunkt eine Ordnerstruktur zum Hosten von Inhalten ein. Sie können den Inhalt beispielsweise in der folgenden Struktur organisieren. Stammverzeichnis *Ordner/Ordner pro Sprache/Ihre HTML-Dateien*. Beispielsweise könnte Ihr benutzerdefinierter Seiten-URI wie folgt aussehen:

```XML
<ContentDefinition Id="api.signuporsignin">
  <LoadUri>https://contoso.blob.core.windows.net/{Culture:LanguageName}/myHTML/unified.html</LoadUri>
  ...
</ContentDefinition>
```

Azure AD B2C sendet den aus zwei Buchstaben bestehenden ISO-Code, z. B. `fr` für Französisch:

```http
https://contoso.blob.core.windows.net/fr/myHTML/unified.html
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, welche Elemente der Benutzeroberfläche angepasst werden können, finden Sie unter [Anpassen der Benutzeroberfläche in Azure Active Directory B2C](customize-ui-overview.md).
