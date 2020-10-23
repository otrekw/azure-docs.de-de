---
title: SubJourneys in Azure Active Directory B2C | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie das SubJourneys-Element einer benutzerdefinierten Richtlinie in Azure Active Directory B2C angeben.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/15/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 6609dabe9bd507751bd131a4effe24295e2aac04
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2020
ms.locfileid: "91952418"
---
# <a name="subjourneys"></a>SubJourneys

Mit SubJourneys kann der Flow von Orchestrierungsschritten innerhalb einer User Journey organisiert und vereinfacht werden. [User Journeys](userjourneys.md) geben explizite Pfade an, über die eine Richtlinie einer Anwendung der vertrauenden Seite die gewünschten Ansprüche für einen Benutzer abrufen kann. Der Benutzer wird über diese Pfade weitergeleitet, um die Ansprüche abzurufen, die an die vertrauenden Seite übermittelt werden sollen. Das heißt, User Journeys definieren die Geschäftslogik, die ein Endbenutzer durchläuft, während das Framework für die Identitätsfunktion von Azure AD B2C die Anforderung verarbeitet. Eine User Journey wird als Orchestrierungssequenz dargestellt, die für eine erfolgreiche Transaktion durchlaufen werden muss. Das [ClaimsExchange](userjourneys.md#claimsexchanges)-Element eines Orchestrierungsschritts ist an ein einzelnes [technisches Profil](technical-profiles-overview.md) gebunden, das ausgeführt wird.

Bei einer SubJourney handelt es sich um eine Gruppierung von Orchestrierungsschritten, die zu jedem beliebigen Zeitpunkt innerhalb einer User Journey aufgerufen werden können. Mit SubJourneys können Sie wiederverwendbare Schrittsequenzen erstellen oder Verzweigungen implementieren, um die Geschäftslogik besser darzustellen.

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="user-journey-branching"></a>Verzweigung von User Journeys

SubJourneys verhalten sich wie [User Journeys](userjourneys.md), da beide als Orchestrierungssequenz dargestellt werden, die für eine erfolgreiche Transaktion durchlaufen werden muss. User Journeys können eigenständig aufgerufen werden und erfordern zur Ausführung einen SendClaims-Schritt. SubJourneys sind Komponenten von User Journeys und können nicht unabhängig aufgerufen werden. Sie werden immer von einer User Journey aufgerufen.

Die Hauptkomponente der Verzweigung dient dazu, eine bessere Verarbeitung der Geschäftslogik in einer User Journey zu ermöglichen. Allgemeine Orchestrierungsschritte werden in einzelne Teile gruppiert, die separat aufgerufen werden können. Eine SubJourney kann eine Journey vereinfachen, bei der mehrere Orchestrierungsschritte miteinander verknüpft sind (mit denselben Vorbedingungen). Eine SubJourney wird nur von einer User Journey aufgerufen. Sie sollte keine andere SubJourney aufrufen.

Es gibt zwei Typen von SubJourneys:

- **Call**: Gibt die Steuerung an den Aufrufer zurück. Die SubJourney wird ausgeführt, und dann wird die Steuerung an den Orchestrierungsschritt zurückgegeben, der aktuell innerhalb der User Journey ausgeführt wird.
- **Transfer**: Überträgt die Steuerung an die SubJourney (irreversible Verzweigung). Die SubJourney muss einen SendClaims-Schritt enthalten, um die Ansprüche an die Anwendung der vertrauenden Seite zurückzugeben.

## <a name="example-scenarios"></a>Beispielszenarien

### <a name="call-subjourney"></a>Call-SubJourney

Eine Call-SubJourney ist in den folgenden Szenarien nützlich:

- Altersbeschränkung: Für die Altersbeschränkung gibt es viele gemeinsam genutzte Komponenten zwischen den User Journeys. Die Verzweigung ermöglicht die Kompilierung der gemeinsamen Elemente in gemeinsam nutzbare Komponenten.  
- Zustimmung der Eltern: Die Verzweigung ermöglicht eine benutzerfreundliche elterliche Zustimmung, indem wir auf Ansprüche aus der User Journey zugreifen können, die der Minderjährige durchgeführt hat, und in der Lage sind, in eine User Journey mit elterlicher Zustimmung zu verzweigen, nachdem festgestellt wurde, dass der Benutzer die Zustimmung benötigt. 
- Registrierung für die Anmeldung: Stellen Sie sich ein Szenario vor, in dem ein Benutzer bereits im Verzeichnis vorhanden ist, aber vielleicht vergessen hat, dass er tatsächlich ein Konto erstellt hat. In einem solchen Fall kann es wünschenswert sein, über eine Richtlinie zu verfügen, die einen Wechsel von einem Registrierungsflow zu einem Anmeldeflow für diesen Benutzer vornehmen kann, statt dem Benutzer mitzuteilen, dass die von ihm eingegebenen Anmeldeinformationen bereits vorhanden sind, und den Benutzer zu zwingen, die Journey neu zu starten.  

### <a name="transfer-subjourney"></a>Transfer-SubJourney

Eine Transfer-SubJourney ist in den folgenden Szenarien nützlich:

- Anzeigen einer Blockseite
- A/B-Tests durch Weiterleiten der Anforderung an eine SubJourney zur Ausführung und zum Ausstellen eines Tokens

## <a name="adding-a-subjourney-element"></a>Hinzufügen eines SubJourney-Elements

Nachfolgend finden Sie ein Beispiel für ein `SubJourney`-Element vom Typ `Call`, das die Steuerung wieder an die User Journey zurückgibt.

```xml
<SubJourneys>
  <SubJourney Id="ConditionalAccess_Evaluation" Type="Call">
    <OrchestrationSteps>
      <OrchestrationStep Order="1" Type="ClaimsExchange">
       <ClaimsExchanges>
        <ClaimsExchange Id="ConditionalAccessEvaluation" TechnicalProfileReferenceId="ConditionalAccessEvaluation" />
       </ClaimsExchanges>
      </OrchestrationStep>
      <OrchestrationStep Order="2" Type="ClaimsExchange">
        <Preconditions>
          <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
            <Value>conditionalAccessClaimCollection</Value>
            <Action>SkipThisOrchestrationStep</Action>
          </Precondition>
        </Preconditions>
        <ClaimsExchanges>
          <ClaimsExchange Id="GenerateCAClaimFlags" TechnicalProfileReferenceId="GenerateCAClaimFlags" />
        </ClaimsExchanges>
      </OrchestrationStep>
    </OrchestrationSteps>
  </SubJourney>
</SubJourneys>
```

Nachfolgend finden Sie ein Beispiel für ein `SubJourney`-Element vom Typ `Transfer`, das ein Token an die Anwendung der vertrauenden Seite zurückgibt.

```xml
<SubJourneys>
  <SubJourney Id="B" Type="Transfer">
    <OrchestrationSteps>
      ...
      <OrchestrationStep Order="5" Type="SendClaims">
    </OrchestrationSteps>
  </SubJourney>
</SubJourneys>
```

### <a name="invoke-a-subjourney-step"></a>Aufrufen eines SubJourney-Schritts

Ein neuer Orchestrierungsschritt vom Typ `InvokeSubJourney` wird verwendet, um eine SubJourney auszuführen. Im folgenden Beispiel werden alle Ausführungselemente dieses Orchestrierungsschritts angezeigt.

```xml
<OrchestrationStep Order="5" Type="InvokeSubJourney">
  <JourneyList>
    <Candidate SubJourneyReferenceId="ConditionalAccess_Evaluation" />
  </JourneyList>
</OrchestrationStep>
```

> [!NOTE]
> Eine SubJourney sollte keine andere SubJourney aufrufen.

## <a name="components"></a>Komponenten

Fügen Sie ein **SubJourneys**-Element unter dem Element der obersten Ebene der Richtliniendatei hinzu, um die von der Richtlinie unterstützten SubJourneys zu definieren.

Das **SubJourneys**-Element enthält das folgende Element:

| Element | Vorkommen | BESCHREIBUNG |
| ------- | ----------- | ----------- |
| SubJourney | 1:n | Eine SubJourney, die alle für einen vollständigen Benutzerflow erforderlichen Konstrukte definiert. |

Das **SubJourneys**-Element enthält die folgenden Attribute:

| attribute | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| Id | Ja | Der SubJourney-Bezeichner, der von der User Journey verwendet werden kann, um auf die SubJourney in der Richtlinie zu verweisen. Das **SubJourneyReferenceId**-Element des [Candidate](userjourneys.md#journeylist)-Elements verweist auf dieses Attribut. |
| type | Ja | Mögliche Werte sind `Call` oder `Transfer`. Weitere Informationen finden Sie unter [Verzweigung von User Journeys](#user-journey-branching).|

Das **SubJourney**-Element enthält das folgende Element:

| Element | Vorkommen | BESCHREIBUNG |
| ------- | ----------- | ----------- |
| OrchestrationSteps | 1:n | Eine Orchestrierungssequenz, die für eine erfolgreiche Transaktion durchlaufen werden muss. Jede User Journey besteht aus einer geordneten List von Orchestrierungsschritten, die nacheinander ausgeführt werden. Wenn ein Schritt fehlschlägt, schlägt die Transaktion fehl. |

## <a name="orchestrationsteps"></a>OrchestrationSteps

Die vollständige Liste der Elemente der Orchestrierungsschritte finden Sie unter [UserJourneys](userjourneys.md).

## <a name="next-steps"></a>Nächste Schritte

[UserJourneys](userjourneys.md)