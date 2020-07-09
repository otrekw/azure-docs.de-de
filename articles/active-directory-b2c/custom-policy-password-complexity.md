---
title: Konfigurieren der Kennwortkomplexität mithilfe von benutzerdefinierten Richtlinien
titleSuffix: Azure AD B2C
description: Erfahren Sie, wie Komplexitätsanforderungen für Kennwörter mit einer benutzerdefinierten Richtlinie in Azure Active Directory B2C konfiguriert werden.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 03/10/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 4ab196e894fc53b1243ac363f9863d5c7d4e328f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85389002"
---
# <a name="configure-password-complexity-using-custom-policies-in-azure-active-directory-b2c"></a>Konfigurieren der Kennwortkomplexität mithilfe von benutzerdefinierten Richtlinien in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In Azure Active Directory B2C (Azure AD B2C) können Sie die Komplexitätsanforderungen für Kennwörter konfigurieren, die von einem Benutzer beim Erstellen eines Kontos angegeben werden. Standardmäßig verwendet Azure AD B2C **starke** Kennwörter. In diesem Artikel erfahren Sie, wie Sie Kennwortkomplexität in [benutzerdefinierten Richtlinien](custom-policy-overview.md) konfigurieren. Es ist auch möglich, die Kennwortkomplexität in [Benutzerflows](user-flow-password-complexity.md) zu konfigurieren.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte unter [Erste Schritte mit benutzerdefinierten Richtlinien in Azure Active Directory B2C](custom-policy-get-started.md) aus. Sie sollten eine funktionierende benutzerdefinierte Richtlinie für die Registrierung und Anmeldung bei lokalen Konten besitzen.


## <a name="add-the-elements"></a>Hinzufügen der Elemente

Um die Kennwortkomplexität zu konfigurieren, überschreiben Sie die [Anspruchstypen](claimsschema.md) `newPassword` und `reenterPassword` mit einem Verweis auf [Prädikatüberprüfungen](predicates.md#predicatevalidations). Das Element „PredicateValidations“ gruppiert eine Reihe von Prädikaten mit dem Ziel, eine Überprüfung der Benutzereingabe zu erstellen, die auf einen Anspruchstyp angewendet werden kann. Öffnen Sie die Erweiterungsdatei Ihrer Richtlinie. Beispiel: <em>`SocialAndLocalAccounts/` **`TrustFrameworkExtensions.xml`** </em>.

1. Suchen Sie nach dem Element [BuildingBlocks](buildingblocks.md). Wenn das Element nicht vorhanden ist, fügen Sie es hinzu.
1. Suchen Sie nach dem Element [ClaimsSchema](claimsschema.md). Wenn das Element nicht vorhanden ist, fügen Sie es hinzu.
1. Fügen Sie dem Element **ClaimsSchema** die Ansprüche `newPassword` und `reenterPassword` hinzu.

    ```xml
    <ClaimType Id="newPassword">
      <PredicateValidationReference Id="CustomPassword" />
    </ClaimType>
    <ClaimType Id="reenterPassword">
      <PredicateValidationReference Id="CustomPassword" />
    </ClaimType>
    ```

1. Das Element [Predicates](predicates.md) definiert eine grundlegende Überprüfung zum Prüfen des Werts eines Anspruchstyps und gibt „true“ oder „false“ zurück. Die Überprüfung erfolgt mit einem bestimmten Methodenelement und einer Reihe von Parametern, die für die Methode relevant sind. Fügen Sie dem Element **BuildingBlocks** unmittelbar nach dem Schließen des Elements `</ClaimsSchema>` die folgenden Prädikate hinzu:

    ```xml
    <Predicates>
      <Predicate Id="LengthRange" Method="IsLengthRange">
        <UserHelpText>The password must be between 6 and 64 characters.</UserHelpText>
        <Parameters>
          <Parameter Id="Minimum">6</Parameter>
          <Parameter Id="Maximum">64</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Lowercase" Method="IncludesCharacters">
        <UserHelpText>a lowercase letter</UserHelpText>
        <Parameters>
          <Parameter Id="CharacterSet">a-z</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Uppercase" Method="IncludesCharacters">
        <UserHelpText>an uppercase letter</UserHelpText>
        <Parameters>
          <Parameter Id="CharacterSet">A-Z</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Number" Method="IncludesCharacters">
        <UserHelpText>a digit</UserHelpText>
        <Parameters>
          <Parameter Id="CharacterSet">0-9</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Symbol" Method="IncludesCharacters">
        <UserHelpText>a symbol</UserHelpText>
        <Parameters>
          <Parameter Id="CharacterSet">@#$%^&amp;*\-_+=[]{}|\\:',.?/`~"();!</Parameter>
        </Parameters>
      </Predicate>
    </Predicates>
    ```

1. Fügen Sie dem Element **BuildingBlocks** unmittelbar nach dem Schließen des Elements `</Predicates>` die folgenden Prädikatüberprüfungen hinzu:

    ```xml
    <PredicateValidations>
      <PredicateValidation Id="CustomPassword">
        <PredicateGroups>
          <PredicateGroup Id="LengthGroup">
            <PredicateReferences MatchAtLeast="1">
              <PredicateReference Id="LengthRange" />
            </PredicateReferences>
          </PredicateGroup>
          <PredicateGroup Id="CharacterClasses">
            <UserHelpText>The password must have at least 3 of the following:</UserHelpText>
            <PredicateReferences MatchAtLeast="3">
              <PredicateReference Id="Lowercase" />
              <PredicateReference Id="Uppercase" />
              <PredicateReference Id="Number" />
              <PredicateReference Id="Symbol" />
            </PredicateReferences>
          </PredicateGroup>
        </PredicateGroups>
      </PredicateValidation>
    </PredicateValidations>
    ```

1. Die folgenden technischen Profile sind [technische Active Directory-Profile](active-directory-technical-profile.md), die Daten in Azure Active Directory schreiben und daraus lesen. Überschreiben Sie diese technischen Profile in der Erweiterungsdatei. Verwenden Sie `PersistedClaims`, um die sichere Kennwortrichtlinie zu deaktivieren. Suchen Sie nach dem Element **ClaimsProviders**.  Fügen Sie wie folgt die folgenden Anspruchsanbieter hinzu:

    ```xml
    <ClaimsProvider>
      <DisplayName>Azure Active Directory</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="AAD-UserWriteUsingLogonEmail">
          <PersistedClaims>
            <PersistedClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration, DisableStrongPassword"/>
          </PersistedClaims>
        </TechnicalProfile>
        <TechnicalProfile Id="AAD-UserWritePasswordUsingObjectId">
          <PersistedClaims>
            <PersistedClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration, DisableStrongPassword"/>
          </PersistedClaims>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. Speichern Sie die Richtliniendatei.

## <a name="test-your-policy"></a>Testen Ihrer Richtlinie

### <a name="upload-the-files"></a>Hochladen der Dateien

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
3. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
4. Wählen Sie **Framework für die Identitätsfunktion** aus.
5. Klicken Sie auf der Seite „Benutzerdefinierte Richtlinien“ auf **Richtlinie hochladen**.
6. Aktivieren Sie **Richtlinie überschreiben, sofern vorhanden**, suchen Sie nach der Datei *TrustFrameworkExtensions.xml*, und wählen Sie die Datei aus.
7. Klicken Sie auf **Hochladen**.

### <a name="run-the-policy"></a>Ausführen der Richtlinie

1. Öffnen Sie die Registrierungs- oder Anmelderichtlinie. Beispiel: *B2C_1A_signup_signin*.
2. Wählen Sie für **Anwendung** Ihre Anwendung aus, die Sie zuvor registriert haben. Um das Token anzuzeigen, muss als **Antwort-URL** der Wert `https://jwt.ms` angegeben werden.
3. Klicken Sie auf **Jetzt ausführen**.
4. Wählen Sie **Jetzt registrieren** aus, geben Sie eine E-Mail-Adresse ein, und geben Sie ein neues Kennwort ein. Es werden Anweisungen zu Kennworteinschränkungen angezeigt. Schließen Sie die Eingabe der Benutzerinformationen ab, und klicken Sie dann auf **Erstellen**. Es sollten Ihnen jetzt die Inhalte des zurückgegebenen Tokens angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Konfigurieren der Kennwortänderung in benutzerdefinierten Richtlinien in Azure Active Directory B2C](custom-policy-password-change.md).
- Weitere Informationen finden Sie unter den Elementen [Predicates](predicates.md) und [PredicateValidations](predicates.md#predicatevalidations) in der IEF-Referenz.
