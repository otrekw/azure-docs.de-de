---
title: Definieren eines technischen Application Insights-Profils in einer benutzerdefinierten Richtlinie
titleSuffix: Azure AD B2C
description: Hier erfahren Sie, wie Sie ein technisches Application Insights-Profil in einer benutzerdefinierten Richtlinie in Azure Active Directory B2C definieren.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/20/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 77bb53e2605913fcee6999284acb04616efc53af
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85201411"
---
# <a name="define-an-application-insights-technical-profile-in-an-azure-ad-b2c-custom-policy"></a>Definieren eines technischen Application Insights-Profils in einer benutzerdefinierten Azure AD B2C-Richtlinie

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) unterstützt das direkte Senden von Ereignisdaten an [Application Insights](../azure-monitor/app/app-insights-overview.md) unter Verwendung des für Azure AD B2C bereitgestellten Instrumentierungsschlüssels.  Mit einem technischen Application Insights-Profil können Sie ausführliche und angepasste Ereignisprotokolle für Ihre User Journeys erhalten. Diese ermöglichen Folgendes:

* Gewinnen von Einblicken in Benutzerverhalten.
* Beheben von Problemen mit eigenen Richtlinien in der Entwicklung oder in der Produktion.
* Messen der Leistung.
* Erstellen von Benachrichtigungen von Application Insights.


## <a name="protocol"></a>Protocol

Das **Name**-Attribut des **Protocol**-Elements muss auf `Proprietary` festgelegt werden. Das **handler**-Attribut muss den vollqualifizierten Namen der Protokollhandlerassembly enthalten, die von Azure AD B2C für Application Insights verwendet wird: `Web.TPEngine.Providers.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`

Das folgende Beispiel zeigt das allgemeine technische Application Insights-Profil. Andere technische Application Insights-Profile enthalten „AAD-Common“, um die zugehörige Konfiguration zu nutzen.  

```xml
<TechnicalProfile Id="AzureInsights-Common">
  <DisplayName>Azure Insights Common</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.Insights.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

## <a name="input-claims"></a>Eingabeansprüche

Das Element **InputClaims** enthält eine Liste mit Ansprüchen, die an Application Insights gesendet werden sollen. Der Name Ihres Anspruchs kann auch einem von Ihnen bevorzugten Namen für die Anzeige in Application Insights zugeordnet werden. Im folgenden Beispiel wird das Senden von Telemetriedaten an Application Insights gezeigt. Die Eigenschaften eines Ereignisses werden mit der Syntax `{property:NAME}` hinzugefügt, wobei „NAME“ die Eigenschaft ist, die dem Ereignis hinzugefügt wird. „DefaultValue“ kann ein statischer Wert oder ein Wert sein, der von einem der unterstützten [Anspruchskonfliktlöser](claim-resolver-overview.md) aufgelöst wird.

```xml
<InputClaims>
  <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
  <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:JourneyId}" DefaultValue="{Context:CorrelationId}" />
  <InputClaim ClaimTypeReferenceId="Culture" PartnerClaimType="{property:Culture}" DefaultValue="{Culture:RFC5646}" />
  <InputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="{property:objectId}"  />
</InputClaims>
```

Das Element **InputClaimsTransformations** kann eine Sammlung von Elementen vom Typ **InputClaimsTransformation** enthalten, die vor dem Senden an Application Insights zum Ändern der Eingabeansprüche oder zum Generieren neuer Eingabeansprüche verwendet werden.

## <a name="persist-claims"></a>Beibehalten von Ansprüchen

Das Element „PersistedClaims“ wird nicht verwendet.

## <a name="output-claims"></a>Ausgabeansprüche

Die Elemente „OutputClaims“ und „OutputClaimsTransformations“ werden nicht verwendet.

## <a name="cryptographic-keys"></a>Kryptografische Schlüssel

Das Element „CryptographicKeys“ wird nicht verwendet.


## <a name="metadata"></a>Metadaten

| attribute | Erforderlich | Beschreibung |
| --------- | -------- | ----------- |
| InstrumentationKey| Ja | Der [Instrumentierungsschlüssel](../azure-monitor/app/create-new-resource.md#copy-the-instrumentation-key) von Application Insights, der zum Protokollieren der Ereignisse verwendet wird. | 
| DeveloperMode| Nein | Ein boolescher Wert, der angibt, ob der Entwicklermodus aktiviert ist. Mögliche Werte: `true` und `false` (Standardwert). Diese Metadaten steuern die Pufferung von Ereignissen. In einer Entwicklungsumgebung mit minimalem Ereignisaufkommen führt die Aktivierung des Entwicklermodus dazu, dass Ereignisse sofort an Application Insights gesendet werden.|  
|DisableTelemetry |Nein |Ein boolescher Wert, der angibt, ob die Telemetrie aktiviert werden soll. Mögliche Werte: `true` und `false` (Standardwert).| 


## <a name="next-steps"></a>Nächste Schritte

- [Erstellen Sie eine Application Insights-Ressource](../azure-monitor/app/create-new-resource.md).
- Machen Sie sich mit dem [Nachverfolgen des Benutzerverhaltens in Azure Active Directory B2C mithilfe von Application Insights](analytics-with-application-insights.md) vertraut.
