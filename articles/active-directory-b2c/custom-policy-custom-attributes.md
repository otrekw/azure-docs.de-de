---
title: Hinzufügen eigener Attribute zu benutzerdefinierten Richtlinien
titleSuffix: Azure AD B2C
description: Eine exemplarische Vorgehensweise zur Verwendung von Erweiterungseigenschaften und benutzerdefinierten Attributen sowie zu ihrer Einbindung in die Benutzeroberfläche.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 03/17/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: dbeb2540084fad2cfab3ce360dd15b60a75e5e59
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85389325"
---
# <a name="azure-active-directory-b2c-enable-custom-attributes-in-a-custom-profile-policy"></a>Azure Active Directory B2C: Aktivieren benutzerdefinierter Attribute in einer benutzerdefinierten Profilrichtlinie

Im Artikel [Hinzufügen von Ansprüchen und Anpassen von Benutzereingaben mithilfe benutzerdefinierter Richtlinien](custom-policy-configure-user-input.md) erfahren Sie, wie Sie integrierte [Benutzerprofilattribute](user-profile-attributes.md) verwenden können. In diesem Artikel aktivieren Sie in Ihrem Azure Active Directory B2C-Verzeichnis (Azure AD B2C) ein benutzerdefiniertes Attribut. Später können Sie das neue Attribut als benutzerdefinierten Anspruch gleichzeitig in [Benutzerflows](user-flow-overview.md) oder [benutzerdefinierten Richtlinien](custom-policy-get-started.md) verwenden.

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

## <a name="prerequisites"></a>Voraussetzungen

Folgen Sie den Schritten im Artikel [Azure Active Directory B2C: Erste Schritte mit benutzerdefinierten Richtlinien](custom-policy-get-started.md).

## <a name="use-custom-attributes-to-collect-information-about-your-customers"></a>Verwenden benutzerdefinierter Attribute zum Erfassen von Informationen zu Ihren Kunden 

Ihr Azure AD B2C-Verzeichnis verfügt über [mehrere integrierte Attribute](user-profile-attributes.md). Sie müssen jedoch oftmals für die Verwaltung Ihres spezifischen Szenarios eigene Attribute erstellen, z. B. in den folgenden Fällen:

* Für eine Kundenanwendung muss ein Attribut wie **LoyaltyNumber** persistent gespeichert werden.
* Ein Identitätsanbieter hat eine eindeutige Benutzer-ID, die persistent gespeichert werden muss, z. B. **uniqueUserGUID**.
* Für eine benutzerdefinierte User Journey muss der Status eines Benutzers persistent gespeichert werden, z. B. **migrationStatus**.

Azure AD B2C ermöglicht Ihnen, die für die einzelnen Benutzerkonten gespeicherte Gruppe von Attributen zu erweitern. Außerdem können Sie diese Attribute mit der [Microsoft Graph-API](manage-user-accounts-graph-api.md) lesen und schreiben.

## <a name="azure-ad-b2c-extensions-app"></a>Erweiterungs-App für Azure AD B2C

Erweiterungsattribute können nur für ein Anwendungsobjekt registriert werden, obwohl sie Daten für einen Benutzer enthalten können. Das Erweiterungsattribut wird an die Anwendung namens b2c-extensions-app angefügt. Ändern Sie diese Anwendung nicht, da sie von Azure AD B2C zum Speichern von Benutzerdaten genutzt wird. Sie finden diese Anwendung unter Azure AD B2C, App-Registrierungen.

Die Begriffe *Erweiterungseigenschaft*, *benutzerdefiniertes Attribut* und *benutzerdefinierter Anspruch* beziehen sich im Kontext dieses Artikels auf die gleiche Sache. Der Name variiert je nach Kontext (Anwendung, Objekt, Richtlinie).

## <a name="get-the-application-properties"></a>Abrufen der Anwendungseigenschaften

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie im linken Menü die Option **Azure AD B2C** aus. Oder wählen Sie **Alle Dienste** aus, suchen Sie nach dem Eintrag **Azure AD B2C**, und wählen Sie ihn aus.
1. Wählen Sie **App-Registrierungen** und dann **Alle Anwendungen** aus.
1. Wählen Sie die Anwendung `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` aus.
1. Kopieren Sie die folgenden Bezeichner in die Zwischenablage, und speichern Sie sie:
    * **Anwendungs-ID**. Beispiel: `11111111-1111-1111-1111-111111111111`.
    * **Objekt-ID** Beispiel: `22222222-2222-2222-2222-222222222222`.

## <a name="modify-your-custom-policy"></a>Ändern der benutzerdefinierten Richtlinie

Um benutzerdefinierte Attribute in Ihrer Richtlinie zu aktivieren, geben Sie die **Anwendungs-ID** und **Objekt-ID** der Anwendung in den Metadaten des technischen Profils AAD-Common an. Das technische Profil *AAD-Common* befindet sich im technischen Basisprofil [Azure Active Directory](active-directory-technical-profile.md) und bietet Unterstützung für die Benutzerverwaltung in Azure AD. Andere technische Profile von Azure AD enthalten AAD-Common, um dessen Konfiguration zu nutzen. Überschreiben Sie das technische Profil AAD-Common in der Erweiterungsdatei.

1. Öffnen Sie die Erweiterungsdatei Ihrer Richtlinie. Beispiel: <em>`SocialAndLocalAccounts/`**`TrustFrameworkExtensions.xml`**</em>.
1. Wechseln Sie zum ClaimsProviders-Element. Fügen Sie dem ClaimsProviders-Element ein neues ClaimsProvider-Element hinzu.
1. Ersetzen Sie `ApplicationObjectId` durch die Objekt-ID, die Sie zuvor aufgezeichnet haben. Ersetzen Sie dann `ClientId` durch die Anwendungs-ID, die Sie zuvor im folgenden Codeausschnitt aufgezeichnet haben.

    ```xml
    <ClaimsProvider>
      <DisplayName>Azure Active Directory</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="AAD-Common">
          <Metadata>
            <!--Insert b2c-extensions-app application ID here, for example: 11111111-1111-1111-1111-111111111111-->  
            <Item Key="ClientId"></Item>
            <!--Insert b2c-extensions-app application ObjectId here, for example: 22222222-2222-2222-2222-222222222222-->
            <Item Key="ApplicationObjectId"></Item>
          </Metadata>
        </TechnicalProfile>
      </TechnicalProfiles> 
    </ClaimsProvider>
    ```

## <a name="upload-your-custom-policy"></a>Hochladen der benutzerdefinierten Richtlinie

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD-Mandanten enthält, indem Sie im oberen Menü den Filter **Verzeichnis und Abonnement** und dann das Verzeichnis auswählen, das Ihren Azure AD B2C-Mandanten enthält.
3. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **App-Registrierungen**, und wählen Sie dann diese Option aus.
4. Wählen Sie **Framework für die Identitätsfunktion** aus.
5. Wählen Sie **Benutzerdefinierte Richtlinie hochladen** aus, und laden Sie dann die geänderten Richtliniendateien des Typs „TrustFrameworkExtensions.xml“ hoch.

> [!NOTE]
> Wenn das technische Azure AD-Profil zum ersten Mal den Anspruch auf das Verzeichnis persistent speichert, prüft es, ob das benutzerdefinierte Attribut existiert. Falls nicht, wird das benutzerdefinierte Attribut erstellt.  

## <a name="create-a-custom-attribute-through-azure-portal"></a>Erstellen eines benutzerdefinierten Attributs im Azure-Portal

Von integrierten und benutzerdefinierten Profilen werden dieselben Erweiterungsattribute genutzt. Beim Hinzufügen benutzerdefinierter Attribute auf der Portaloberfläche werden diese Attribute mithilfe von der in jedem B2C-Mandanten vorhandenen **b2c-extensions-app** registriert.

Sie können diese Attribute auf der Portalbenutzeroberfläche erstellen, bevor Sie sie in Ihren benutzerdefinierten Richtlinien einsetzen. Befolgen Sie die Anleitungen zum [Definieren benutzerdefinierter Attribute in Azure Active Directory B2C](user-flow-custom-attributes.md). Wenn Sie im Portal das Attribut **loyaltyId** erstellen, müssen Sie wie folgt darauf verweisen:

|Name     |Verwendung in |
|---------|---------|
|`extension_loyaltyId`  | Benutzerdefinierte Richtlinie|
|`extension_<b2c-extensions-app-guid>_loyaltyId`  | [Microsoft Graph-API](manage-user-accounts-graph-api.md)|

Das folgende Beispiel veranschaulicht die Verwendung benutzerdefinierter Attribute in einer benutzerdefinierten Azure AD B2C-Anspruchsdefinition für eine Richtlinie.

```xml
<BuildingBlocks>
  <ClaimsSchema>
    <ClaimType Id="extension_loyaltyId">
      <DisplayName>Loyalty Identification</DisplayName>
      <DataType>string</DataType>
      <UserHelpText>Your loyalty number from your membership card</UserHelpText>
      <UserInputType>TextBox</UserInputType>
    </ClaimType>
  </ClaimsSchema>
</BuildingBlocks>
```

Das folgende Beispiel veranschaulicht die Verwendung eines benutzerdefinierten Attributs in der benutzerdefinierten Azure AD B2C-Richtlinie in einem technischen Profil, einer Eingabe, einer Ausgabe und persistent gespeicherten Ansprüchen.

```xml
<InputClaims>
  <InputClaim ClaimTypeReferenceId="extension_loyaltyId"  />
</InputClaims>
<PersistedClaims>
  <PersistedClaim ClaimTypeReferenceId="extension_loyaltyId" />
</PersistedClaims>
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
</OutputClaims>
```

## <a name="use-a-custom-attribute-in-a-policy"></a>Verwenden eines benutzerdefinierten Attributs in einer Richtlinie

Befolgen Sie die Anleitungen zum [Hinzufügen von Ansprüchen und Anpassen von Benutzereingaben mit benutzerdefinierten Richtlinien in Azure Active Directory B2C](custom-policy-configure-user-input.md). In diesem Beispiel wird der integrierte Anspruch „city“ verwendet. Um ein benutzerdefiniertes Attribut zu verwenden, ersetzen Sie „city“ durch Ihre eigenen benutzerdefinierten Attribute.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen:

- [Azure AD B2C-Benutzerprofilattribute](user-profile-attributes.md)
- [Definition von Erweiterungsattributen](user-profile-attributes.md#extension-attributes)
- [Verwalten von Azure AD B2C-Benutzerkonten mit Microsoft Graph](manage-user-accounts-graph-api.md)
