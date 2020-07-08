---
title: Nachverfolgen des Benutzerverhaltens mit Application Insights
titleSuffix: Azure AD B2C
description: Erfahren Sie, wie Sie Ereignisprotokolle in Application Insights von Azure AD B2C-User Journeys mit benutzerdefinierten Richtlinien aktivieren.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.date: 04/05/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 67ea7324419d86fa5b5c23a2f0aa5f8c057495d1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85385976"
---
# <a name="track-user-behavior-in-azure-active-directory-b2c-using-application-insights"></a>Nachverfolgen des Benutzerverhaltens in Azure Active Directory B2C mithilfe von Application Insights

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure Active Directory B2C (Azure AD B2C) unterstützt das direkte Senden von Ereignisdaten an [Application Insights](../azure-monitor/app/app-insights-overview.md) unter Verwendung des für Azure AD B2C bereitgestellten Instrumentierungsschlüssels.  Mit einem technischen Application Insights-Profil können Sie ausführliche und angepasste Ereignisprotokolle für Ihre User Journeys erhalten. Diese ermöglichen Folgendes:

* Gewinnen von Einblicken in Benutzerverhalten.
* Beheben von Problemen mit eigenen Richtlinien in der Entwicklung oder in der Produktion.
* Messen der Leistung.
* Erstellen von Benachrichtigungen von Application Insights.

## <a name="how-it-works"></a>Funktionsweise

Das technische Profil [Application Insights](application-insights-technical-profile.md) definiert ein Ereignis in Azure AD B2C. Das Profil gibt den Namen des Ereignisses, die aufzuzeichnenden Ansprüche und den Instrumentierungsschlüssel an. Um ein Ereignis zu senden, wird das technische Profil dann als Orchestrierungsschritt in einer [User Journey](userjourneys.md) hinzugefügt.

Application Insights kann die Ereignisse mithilfe einer Korrelations-ID zum Aufzeichnen einer Benutzersitzung vereinheitlichen. Application Insights macht das Ereignis und die Sitzung innerhalb von Sekunden verfügbar und stellt viele Visualisierungs-, Export- und Analysetools bereit.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte unter [Erste Schritte mit benutzerdefinierten Richtlinien in Azure Active Directory B2C](custom-policy-get-started.md) aus. Sie sollten eine funktionierende benutzerdefinierte Richtlinie für die Registrierung und Anmeldung bei lokalen Konten besitzen.

## <a name="create-an-application-insights-resource"></a>Erstellen einer Application Insights-Ressource

Wenn Sie Application Insights mit Azure AD B2C verwenden, müssen Sie lediglich eine Ressource erstellen und den Instrumentierungsschlüssel abrufen. Informationen finden Sie unter [Erstellen einer Application Insights-Ressource](../azure-monitor/app/create-new-resource.md).

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihr Azure-Abonnement enthält, indem Sie im oberen Menü den Filter **Verzeichnis und Abonnement** auswählen und dann das Verzeichnis auswählen, das Ihr Abonnement enthält. Bei diesem Mandanten handelt es sich nicht um Ihren Azure AD B2C-Mandanten.
3. Wählen Sie links oben im Azure-Portal **Ressource erstellen** aus, und suchen Sie dann nach **Application Insights**, und wählen Sie dann diese Option aus.
4. Klicken Sie auf **Erstellen**.
5. Geben Sie einen **Namen** für die Ressource ein.
6. Wählen Sie als **Anwendungstyp** die Option **ASP.NET-Webanwendung** aus.
7. Wählen Sie als **Ressourcengruppe** eine vorhandene Gruppe aus, oder geben Sie den Namen für eine neue Gruppe ein.
8. Klicken Sie auf **Erstellen**.
4. Nachdem Sie die Application Insights-Ressource erstellt haben, öffnen Sie sie, erweitern Sie **Zusammenfassung**, und kopieren Sie den Instrumentierungsschlüssel.

![Application Insights – Übersicht und Instrumentierungsschlüssel](./media/analytics-with-application-insights/app-insights.png)

## <a name="define-claims"></a>Definieren von Ansprüchen

Ein Anspruch stellt eine temporäre Speicherung von Daten während der Ausführung einer Azure AD B2C-Richtlinie bereit. Im [Anspruchsschema](claimsschema.md) deklarieren Sie Ihre Ansprüche.

1. Öffnen Sie die Erweiterungsdatei Ihrer Richtlinie. Beispiel: <em>`SocialAndLocalAccounts/`**`TrustFrameworkExtensions.xml`**</em>.
1. Suchen Sie nach dem Element [BuildingBlocks](buildingblocks.md). Wenn das Element nicht vorhanden ist, fügen Sie es hinzu.
1. Suchen Sie nach dem Element [ClaimsSchema](claimsschema.md). Wenn das Element nicht vorhanden ist, fügen Sie es hinzu.
1. Fügen Sie dem Element **ClaimsSchema** die folgenden Ansprüche hinzu. 

```xml
<ClaimType Id="EventType">
  <DisplayName>Event type</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="EventTimestamp">
  <DisplayName>Event timestamp</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="PolicyId">
  <DisplayName>Policy Id</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="Culture">
  <DisplayName>Culture ID</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="CorrelationId">
  <DisplayName>Correlation Id</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="federatedUser">
  <DisplayName>Federated user</DisplayName>
  <DataType>boolean</DataType>
</ClaimType>
<ClaimType Id="parsedDomain">
  <DisplayName>Domain name</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>The domain portion of the email address.</UserHelpText>
</ClaimType>
<ClaimType Id="userInLocalDirectory">
  <DisplayName>userInLocalDirectory</DisplayName>
  <DataType>boolean</DataType>
</ClaimType>
```

## <a name="add-new-technical-profiles"></a>Hinzufügen neuer technischer Profile

Technische Profile können im Azure AD B2C-Framework für die Identitätsfunktion als Funktionen angesehen werden. In dieser Tabelle werden die technischen Profile definiert, die zum Öffnen einer Sitzung und Senden von Ereignissen verwendet werden.

| Technisches Profil | Aufgabe |
| ----------------- | -----|
| AppInsights-Common | Gemeinsamer Satz von Parametern, die in allen technischen Profilen von Azure Insights enthalten sein sollen. |
| AppInsights-SignInRequest | Zeichnet ein `SignInRequest`-Ereignis mit einem Satz von Ansprüchen auf, wenn eine Anmeldeanforderung empfangen wird. |
| AppInsights-UserSignUp | Zeichnet ein `UserSignUp`-Ereignis auf, wenn der Benutzer die Registrierungsoption in einer Registrierungs- oder Anmeldungsjourney auslöst. |
| AppInsights-SignInComplete | Zeichnet ein `SignInComplete`-Ereignis für den erfolgreichen Abschluss einer Authentifizierung auf, wenn ein Token an die Anwendung der vertrauenden Seite gesendet wurde. |

Fügen Sie die Profile zur Datei *TrustFrameworkExtensions.xml* aus dem Starter Pack hinzu. Fügen Sie diese Elemente dem Element **ClaimsProviders** hinzu:

```xml
<ClaimsProvider>
  <DisplayName>Application Insights</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="AppInsights-Common">
      <DisplayName>Application Insights</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.Insights.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <!-- The ApplicationInsights instrumentation key which will be used for logging the events -->
        <Item Key="InstrumentationKey">xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</Item>
        <Item Key="DeveloperMode">false</Item>
        <Item Key="DisableTelemetry ">false</Item>
      </Metadata>
      <InputClaims>
        <!-- Properties of an event are added through the syntax {property:NAME}, where NAME is property being added to the event. DefaultValue can be either a static value or a value that's resolved by one of the supported DefaultClaimResolvers. -->
        <InputClaim ClaimTypeReferenceId="EventTimestamp" PartnerClaimType="{property:EventTimestamp}" DefaultValue="{Context:DateTimeInUtc}" />
        <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
        <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:CorrelationId}" DefaultValue="{Context:CorrelationId}" />
        <InputClaim ClaimTypeReferenceId="Culture" PartnerClaimType="{property:Culture}" DefaultValue="{Culture:RFC5646}" />
      </InputClaims>
    </TechnicalProfile>

    <TechnicalProfile Id="AppInsights-SignInRequest">
      <InputClaims>
        <!-- An input claim with a PartnerClaimType="eventName" is required. This is used by the AzureApplicationInsightsProvider to create an event with the specified value. -->
        <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="SignInRequest" />
      </InputClaims>
      <IncludeTechnicalProfile ReferenceId="AppInsights-Common" />
    </TechnicalProfile>

    <TechnicalProfile Id="AppInsights-UserSignUp">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="UserSignUp" />
      </InputClaims>
      <IncludeTechnicalProfile ReferenceId="AppInsights-Common" />
    </TechnicalProfile>
    
    <TechnicalProfile Id="AppInsights-SignInComplete">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="SignInComplete" />
        <InputClaim ClaimTypeReferenceId="federatedUser" PartnerClaimType="{property:FederatedUser}" DefaultValue="false" />
        <InputClaim ClaimTypeReferenceId="parsedDomain" PartnerClaimType="{property:FederationPartner}" DefaultValue="Not Applicable" />
      </InputClaims>
      <IncludeTechnicalProfile ReferenceId="AppInsights-Common" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

> [!IMPORTANT]
> Ändern Sie den Instrumentierungsschlüssel im technischen Profil `AppInsights-Common` in die GUID, die von Ihrer Application Insights-Ressource bereitgestellt wurde.

## <a name="add-the-technical-profiles-as-orchestration-steps"></a>Hinzufügen der technischen Profil als Orchestrierungsschritte

Rufen Sie `AppInsights-SignInRequest` als Orchestrierungsschritt 2 auf, um nachzuverfolgen, ob eine Anmeldungs-/Registrierungsanforderung empfangen wurde:

```xml
<!-- Track that we have received a sign in request -->
<OrchestrationStep Order="2" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInRequest" TechnicalProfileReferenceId="AppInsights-SignInRequest" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Fügen Sie direkt *vor* dem Orchestrierungsschritt `SendClaims` einen neuen Schritt ein, der `AppInsights-UserSignup` aufruft. Es wird ausgelöst, wenn der Benutzer die Registrierungsschaltfläche in einer Registrierungs-/Anmeldungsjourney auswählt.

```xml
<!-- Handles the user clicking the sign up link in the local account sign in page -->
<OrchestrationStep Order="8" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
      <Value>newUser</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
    <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
      <Value>newUser</Value>
      <Value>false</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackUserSignUp" TechnicalProfileReferenceId="AppInsights-UserSignup" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Rufen Sie direkt nach dem Orchestrierungsschritt `SendClaims` den Befehl `AppInsights-SignInComplete` auf. Dieser Schritt zeigt eine erfolgreich abgeschlossene Journey an.

```xml
<!-- Track that we have successfully sent a token -->
<OrchestrationStep Order="10" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInComplete" TechnicalProfileReferenceId="AppInsights-SignInComplete" />
  </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> Nummerieren Sie nach dem Hinzufügen der neuen Orchestrierungsschritte die Schritte nacheinander von 1 bis N neu, ohne einen Integer zu überspringen.


## <a name="upload-your-file-run-the-policy-and-view-events"></a>Hochladen Ihrer Datei, Ausführen der Richtlinie und Anzeigen von Ereignissen

Speichern Sie die Datei *TrustFrameworkExtensions.xml*, und laden Sie sie hoch. Rufen Sie dann die Richtlinie für die vertrauende Seite Ihrer Anwendung ab, oder verwenden Sie **Jetzt ausführen** im Azure-Portal. In wenigen Sekunden sind die Ereignisse in Application Insights verfügbar.

1. Öffnen Sie die **Application Insights**-Ressource in Ihrem Azure Active Directory-Mandanten.
2. Wählen Sie **Verwendung** > **Ereignisse** aus.
3. Legen Sie **Während** auf **letzte Stunde** und **Für** auf **3 Minuten** fest.  Sie müssen möglicherweise **Aktualisieren** auswählen, um Ihre Ergebnisse anzuzeigen.

![Application Insights-Nutzungsereignisbasis](./media/analytics-with-application-insights/app-ins-graphic.png)

## <a name="optional-collect-more-data"></a>[Optional:] Erfassen von weiteren Daten

Fügen Sie Ihrer User Journey entsprechend Ihren Anforderungen Anspruchstypen und Ereignisse hinzu. Sie können [Anspruchskonfliktlöser](claim-resolver-overview.md) oder einen beliebigen Zeichenfolgenanspruchstyp verwenden, die Ansprüche hinzufügen, indem Sie dem Application Insights-Ereignis oder dem technischen Profil „AppInsights-Common“ ein **Eingabeanspruch**-Element hinzufügen.

- **ClaimTypeReferenceId** ist der Verweis auf einen Anspruchstyp.
- **PartnerClaimType** ist der Name der Eigenschaft, die in Azure Insights angezeigt wird. Verwenden Sie die Syntax von `{property:NAME}`, wobei `NAME` die Eigenschaft ist, die dem Ereignis hinzugefügt wird.
- **DefaultValue** verwendet einen beliebigen Zeichenfolgenwert oder den Anspruchskonfliktlöser.

```xml
<InputClaim ClaimTypeReferenceId="app_session" PartnerClaimType="{property:app_session}" DefaultValue="{OAUTH-KV:app_session}" />
<InputClaim ClaimTypeReferenceId="loyalty_number" PartnerClaimType="{property:loyalty_number}" DefaultValue="{OAUTH-KV:loyalty_number}" />
<InputClaim ClaimTypeReferenceId="language" PartnerClaimType="{property:language}" DefaultValue="{Culture:RFC5646}" />
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum technischen Profil [Application Insights](application-insights-technical-profile.md) finden Sie in der IEF-Referenz. 
