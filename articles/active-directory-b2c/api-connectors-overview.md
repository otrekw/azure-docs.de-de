---
title: Informationen zu API-Connectors in Azure AD B2C
description: Mit Azure Active Directory (Azure AD) API-Konnektoren können Sie Ihre Benutzerabläufe mithilfe von REST-APIs anpassen und erweitern.
services: active-directory-b2c
ms.service: active-directory
ms.subservice: B2C
ms.topic: how-to
ms.date: 04/27/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: a9eeb7ed664f67e1273a7dfff701a18970cd91fd
ms.sourcegitcommit: 516eb79d62b8dbb2c324dff2048d01ea50715aa1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108174522"
---
# <a name="use-api-connectors-to-customize-and-extend-sign-up-user-flows"></a>Verwenden von API-Connectors zum Anpassen und Erweitern von Flows zur Benutzerregistrierung

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

::: zone pivot="b2c-user-flow"

> [!IMPORTANT]
> API-Connectors für die Registrierung sind ein Feature der öffentlichen Vorschauversion von Azure AD B2C. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="overview"></a>Übersicht 

Mithilfe von API-Konnektoren können Sie als Entwickler oder IT-Administrator Ihre Anmelde-Benutzerströme mit REST-APIs integrieren, um das Anmeldeverfahren anzupassen und mit externen Systemen zu integrieren. API-Connectors bieten beispielsweise folgende Möglichkeiten:

- **Überprüfen von Benutzereingabedaten**. Überprüfen Sie auf fehlerhafte oder ungültige Benutzerdaten. Beispielsweise können Sie vom Benutzer bereitgestellte Daten im Vergleich mit vorhandenen Daten in einem externen Datenspeicher oder einer Liste zulässiger Werte überprüfen. Falls ungültig, können Sie einen Benutzer auffordern, gültige Daten anzugeben, oder den Benutzer daran hindern, den Registrierungsflow fortzusetzen.
- **Integrieren in einen benutzerdefinierten Genehmigungsworkflow**. Verbinden mit einem benutzerdefinierten Genehmigungssystem zum Verwalten und Einschränken der Kontoerstellung.
- **Überschreiben von Benutzerattributen**. Sie können ein vom Benutzer erfasstes Attribut neu formatieren oder ihm einen Wert zuweisen. Wenn z.B. ein Benutzer den Vornamen vollständig in Kleinbuchstaben oder Großbuchstaben eingibt, können Sie den Namen so formatieren, dass nur der erste Buchstabe groß geschrieben wird. 
- **Benutzeridentität verifizieren**. Verwenden Sie einen Identitätsüberprüfungsdienst, um Entscheidungen bei der Kontoerstellung eine zusätzliche Sicherheitsstufe hinzuzufügen.
- **Ausführen von benutzerdefinierter Geschäftslogik**. Sie können nachgelagerte Ereignisse in Ihren Cloudsystemen auslösen, um Pushbenachrichtigungen zu senden, Unternehmensdatenbanken zu aktualisieren, Berechtigungen zu verwalten, Datenbanken zu überwachen und andere benutzerdefinierte Aktionen auszuführen.

Ein API-Konnektor versorgt Azure AD B2C mit den Informationen, die für den Aufruf des API-Endpunkts erforderlich sind, indem er die HTTP-Endpunkt-URL und die Authentifizierung für den API-Aufruf definiert. Sobald Sie einen API-Connector konfiguriert haben, können Sie ihn für einen bestimmten Schritt in einem Benutzerflow aktivieren. Wenn ein Benutzer diesen Schritt im Registrierungsflow erreicht, wird der API-Connector aufgerufen und als HTTP-POST-Anforderung an Ihre API materialisiert, wobei Benutzerinformationen („Ansprüche“) als Schlüssel-Wert-Paare in einem JSON-Text gesendet werden. Die API-Antwort kann die Ausführung des Benutzerflows beeinträchtigen. Beispielsweise kann die API-Antwort einen Benutzer für die Anmeldung sperren, den Benutzer auffordern, Informationen erneut einzugeben, oder Benutzerattribute überschreiben und anhängen.

## <a name="where-you-can-enable-an-api-connector-in-a-user-flow"></a>Wann ein API-Connector in einem Benutzerflow aktiviert werden kann

Es gibt zwei Punkte in einem Benutzerflow, an denen Sie einen API-Connector aktivieren können:

- Nach Anmeldung bei einem Identitätsanbieter
- Vor dem Erstellen des Benutzers

> [!IMPORTANT]
> In beiden Fällen werden die API-Connectors bei der **Registrierung** und nicht bei der Anmeldung eines Benutzer aufgerufen.

### <a name="after-signing-in-with-an-identity-provider"></a>Nach Anmeldung bei einem Identitätsanbieter

Ein API-Connector in diesem Schritt des Registrierungsprozesses wird unmittelbar nach der Authentifizierung des Benutzers bei einem Identitätsanbieter (etwa Google, Facebook und Azure AD) aufgerufen. Dieser Schritt geht der ***Seite zur Attributsammlung***  voraus, die dem Benutzer zum Sammeln von Benutzerattributen angezeigt wird. Dieser Schritt wird nicht aufgerufen, wenn sich ein Benutzer mit einem lokalen Konto registriert. Im Folgenden finden Sie Beispiele für Szenarien mit API-Connectors, die Sie in diesem Schritt aktivieren können:

- Verwenden Sie die E-Mail- oder Verbundidentität, die der Benutzer angegeben hat, um Ansprüche in einem bestehenden System nachzuschlagen. Geben Sie diese Ansprüche aus dem bestehenden System zurück, füllen Sie die Seite zur Attributsammlung vorab aus, und stellen Sie sie zur Rückgabe im Token zur Verfügung.
- Implementieren Sie eine Zulassungs- oder Sperrliste basierend auf der sozialen Identität.

### <a name="before-creating-the-user"></a>Vor dem Erstellen des Benutzers

Ein API-Connector in diesem Schritt des Registrierungsprozesses wird nach der Seite zur Attributsammlung aufgerufen, sofern vorhanden. Dieser Schritt wird immer aufgerufen, bevor ein Benutzerkonto erstellt wird. Es folgen Beispiele für Szenarien, die Sie an dieser Stelle während der Registrierung aktivieren könnten:

- Überprüfen eingegebener Benutzerdaten und Aufforderung an einen Benutzer, Daten erneut zu übermitteln
- Sperren einer Benutzerregistrierung auf Grundlage der vom Benutzer eingegebenen Daten
- Verifizieren Sie die Benutzeridentität.
- Abfragen externer Systeme nach vorhandenen Daten zum Benutzer, um diese im Anwendungstoken zurückzugeben oder in Azure AD zu speichern

::: zone-end

::: zone pivot="b2c-custom-policy"

Das Identity Experience Framework (IEF), das Azure Active Directory B2C (Azure AD B2C) zugrunde liegt, kann im Rahmen einer User Journey mit RESTful-APIs integriert werden. In diesem Artikel wird gezeigt, wie Sie eine User Journey erstellen, die mithilfe eines [technischen RESTful-Profils](restful-technical-profile.md) mit einem RESTful-Dienst interagiert.

Mit Azure AD B2C können Sie einer User Journey Ihre eigene Geschäftslogik hinzufügen, indem Sie Ihren eigenen RESTful-Dienst aufrufen. Das Identity Experience Framework kann Daten an Ihren RESTful-Dienst senden und von ihm empfangen, um Ansprüche auszutauschen. Beispielsweise können Sie folgende Aktionen ausführen:

- **Überprüfen von Benutzereingabedaten**. Sie können beispielsweise überprüfen, ob die vom Benutzer angegebene E-Mail-Adresse in Ihrer Kundendatenbank vorhanden ist. Und wenn dies nicht der Fall ist, können Sie einen Fehler ausgeben.
- **Verarbeiten von Ansprüchen**. Wenn ein Benutzer seinen Vornamen vollständig in Kleinbuchstaben oder Großbuchstaben eingibt, kann die REST-API den Namen so formatieren, dass nur der erste Buchstabe groß geschrieben wird, und den Namen in dieser Form an Azure AD B2C zurückgeben.
- **Anreichern von Benutzerdaten durch eine stärkere Integration in die Branchenanwendungen des Unternehmens**. Ihr RESTful-Dienst kann die E-Mail-Adresse des Benutzers empfangen, die Kundendatenbank abfragen und die Treuenummer des Benutzers an Azure AD B2C zurückgeben. Die Rückgabeansprüche können dann im Azure AD-Konto des Benutzers gespeichert, in den nächsten Orchestrierungsschritten ausgewertet oder in das Zugriffstoken eingebunden werden.
- **Ausführen von benutzerdefinierter Geschäftslogik**. Sie können Pushbenachrichtigungen senden, Unternehmensdatenbanken aktualisieren, einen Benutzermigrationsprozess ausführen, Berechtigungen verwalten, Datenbanken überwachen und andere Workflows ausführen.

![Diagramm eines Anspruchsaustauschs über den RESTful-Dienst](media/api-connectors-overview/restful-service-claims-exchange.png)

> [!NOTE]
> Erfolgt eine langsame oder keine Antwort vom RESTful-Dienst an Azure AD B2C, so beträgt der Timeout 30 Sekunden und die Anzahl der Wiederholungsversuche zwei (d. h. es gibt insgesamt 3 Versuche). Die Einstellungen für das Timeout und die Anzahl der Wiederholungsversuche können derzeit nicht konfiguriert werden.

## <a name="calling-a-restful-service"></a>Aufrufen eines RESTful-Diensts

Die Interaktion umfasst einen Anspruchsaustausch von Informationen zwischen den REST-API-Ansprüchen und Azure AD B2C. Sie können die Integration in die RESTful-Dienste auf folgende Weise entwerfen:

- **Technisches Validierungsprofil**. Der Anruf des RESTful-Diensts erfolgt innerhalb eines [technischen Validierungsprofils](validation-technical-profile.md) des angegebenen [selbstbestätigten technischen Profils](self-asserted-technical-profile.md) oder über ein [Anzeigesteuerelement zur Überprüfung](display-control-verification.md) einer [Anzeigesteuerung](display-controls.md). Mit dem technischen Validierungsprofil werden die vom Benutzer bereitgestellten Daten überprüft, bevor die User Journey fortgesetzt wird. Beim technischen Validierungsprofil haben Sie folgende Möglichkeiten:

  - Senden von Ansprüchen an Ihre REST-API
  - Überprüfen von Ansprüchen und Auslösen von benutzerdefinierten Fehlermeldungen, die dem Benutzer angezeigt werden
  - Senden Sie Forderungen von der REST-API an die nächsten Orchestrierungs Schritte zurück.

- **Anspruchsaustausch**. Ein direkter Anspruchsaustausch kann durch Aufrufen eines technischen REST-API-Profils direkt aus einem Orchestrierungsschritt in einer [User Journey](userjourneys.md) konfiguriert werden. Diese Definition ist auf folgende Vorgänge beschränkt:

  - Senden von Ansprüchen an Ihre REST-API
  - Überprüfen von Ansprüchen und Auslösen von benutzerdefinierten Fehlermeldungen, die an die Anwendung zurückgegeben werden
  - Senden Sie Ansprüche von der REST-API an die nächsten Orchestrierungs Schritte zurück.

Sie können einen Rest-API-Aufruf in jedem Schritt der User Journey hinzufügen, die durch eine benutzerdefinierte Richtlinie definiert ist. Beispielsweise können Sie in folgenden Schritten eine Rest-API aufrufen:

- Während der Anmeldung, kurz bevor Azure AD B2C die Anmeldeinformationen überprüft
- Unmittelbar nach der Anmeldung
- Bevor Azure AD B2C ein neues Konto im Verzeichnis erstellt
- Nachdem Azure AD B2C ein neues Konto im Verzeichnis erstellt hat
- Bevor Azure AD B2C ein Zugriffstoken ausgibt

![Erfassung über das technische Validierungsprofil](media/api-connectors-overview/validation-technical-profile.png)

## <a name="sending-data"></a>Senden von Daten

Das `InputClaims`-Element im [technischen RESTful-Profil](restful-technical-profile.md) enthält eine Liste der Ansprüche, die an den RESTful-Dienst gesendet werden sollen. Sie können den Namen Ihres Anspruchs dem im RESTful-Dienst definierten Namen zuordnen, einen Standardwert festlegen und [Anspruchskonfliktlöser](claim-resolver-overview.md) verwenden.

Mit dem Attribut „SendClaimsIn“ können Sie konfigurieren, wie die Eingabeansprüche an den RESTful-Anspruchsanbieter gesendet werden. Mögliche Werte:

- **Body**: Wird im JSON-Format im HTTP POST-Anforderungstext gesendet.
- **Form**: Wird im HTTP POST-Anforderungstext in einem durch kaufmännische Und-Zeichen (&) getrennten Schlüsselwertformat gesendet.
- **Header**: Wird im HTTP GET-Anforderungsheader gesendet.
- **QueryString**: Wird in der Abfragezeichenfolge der HTTP GET-Anforderung gesendet.

Wenn die Option **Body** konfiguriert wird, können Sie mit dem technischen REST-API-Profil eine komplexe JSON-Nutzlast an einen Endpunkt senden. Weitere Informationen finden Sie unter [Senden einer JSON-Nutzlast](restful-technical-profile.md#send-a-json-payload).

## <a name="receiving-data"></a>Empfangen von Daten

Das `OutputClaims`-Element des [technischen RESTful-Profils](restful-technical-profile.md) enthält eine Liste von Ansprüchen, die von der REST-API zurückgegeben werden. Sie müssen den Namen des Anspruchs, der in Ihrer Richtlinie definiert ist, dem Namen, der in der REST-API definiert wurde, zuordnen. Sie können auch Ansprüche einfügen, die nicht vom REST-API-Identitätsanbieter zurückgegeben werden, wenn Sie das Attribut „DefaultValue“ festlegen.

Bei den Ausgabeansprüchen, die vom RESTful-Anspruchsanbieter analysiert werden, wird immer die Analyse einer einfachen JSON-Textantwort erwartet. Beispiel:

```json
{
  "name": "Emily Smith",
  "email": "emily@outlook.com",
  "loyaltyNumber":  1234
}
```

Die ausgegebenen Ansprüche sollten wie das folgende xml-Snippet aussehen:

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
  <OutputClaim ClaimTypeReferenceId="email" />
  <OutputClaim ClaimTypeReferenceId="loyaltyNumber" />
</OutputClaims>
```

Zum Analysieren einer geschachtelten JSON-Textantwort müssen Sie die Metadaten „ResolveJsonPathsInJsonTokens“ auf „true“ festlegen. Legen Sie im Ausgabeanspruch den Wert für „PartnerClaimType“ auf das auszugebende JSON-Pfadelement fest.

```json
"contacts": [
  {
    "id": "MAINCONTACT_1",
    "person": {
      "name": "Emily Smith",
      "loyaltyNumber":  1234,
      "emails": [
        {
          "id": "EMAIL_1",
          "type": "WORK",
          "email": "email@domain.com"
        }
      ]
    }
  }
],
```


Die ausgegebenen Ansprüche sollten wie das folgende xml-Snippet aussehen:

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="contacts[0].person.name" />
  <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="contacts[0].person.emails[0].email" />
  <OutputClaim ClaimTypeReferenceId="loyaltyNumber" PartnerClaimType="contacts[0].person.loyaltyNumber" />
</OutputClaims>
```

## <a name="localize-the-rest-api"></a>Lokalisieren der REST-API

In einem technischen RESTful-Profil empfiehlt es sich, die Sprache bzw. das Gebietsschema der aktuellen Sitzung zu senden und bei Bedarf eine lokalisierte Fehlermeldung auszugeben. Mit dem [Anspruchskonfliktlöser](claim-resolver-overview.md) können Sie einen kontextbezogenen Anspruch (z. B. die Benutzersprache) senden. Das folgende Beispiel zeigt ein technisches RESTful-Profil mit diesem Szenario.

```xml
<TechnicalProfile Id="REST-ValidateUserData">
  <DisplayName>Validate user input data</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app.azurewebsites.net/api/identity</Item>
    <Item Key="AuthenticationType">None</Item>
    <Item Key="SendClaimsIn">Body</Item>
    <Item Key="IncludeClaimResolvingInClaimsHandling">true</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userLanguage" DefaultValue="{Culture:LCID}" AlwaysUseDefaultValue="true" />
    <InputClaim ClaimTypeReferenceId="email" PartnerClaimType="emailAddress" />
  </InputClaims>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
</TechnicalProfile>
```

## <a name="handling-error-messages"></a>Verarbeiten von Fehlermeldungen

Ihre REST-API muss möglicherweise eine Fehlermeldung zurückgeben (z. B. „Der Benutzer wurde nicht im CRM-System gefunden“). Wenn ein Fehler auftritt, sollte die REST-API die HTTP-Fehlermeldung 409 (Antwortstatuscode „Konflikt“) zurückgeben. Weitere Informationen finden Sie unter [Technisches RESTful-Profil](restful-technical-profile.md#returning-validation-error-message).

Ein solches Verhalten kann nur durch den Aufruf eines technischen REST-API-Profils aus einem technischen Validierungsprofil erreicht werden. Dadurch kann der Benutzer die Daten auf der Seite korrigieren und die Validierung bei der Seitenübermittlung erneut ausführen.

Wenn Sie direkt aus einer User Journey auf ein technisches REST-API-Profil verweisen, wird der Benutzer mit der entsprechenden Fehlermeldung wieder an die Anwendung der vertrauenden Seite zurückgeleitet.

::: zone-end

## <a name="security-considerations"></a>Sicherheitshinweise

Sie müssen ihren REST-API-Endpunkt schützen, damit nur authentifizierte Clients mit ihm kommunizieren können. Die REST-API muss einen HTTPS-Endpunkt verwenden. Bestimmen Sie den Authentifizierungstyp auf eine der folgenden Authentifizierungsmethoden.

### <a name="api-key"></a>API-Schlüssel

Ein API-Schlüssel ist ein eindeutiger Bezeichner, der zum Authentifizieren eines Benutzers für den Zugriff auf einen Rest-API-Endpunkt verwendet wird. Beispielsweise enthält [Azure Functions HTTP-Trigger](../azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys) als `code` Abfrageparameter in der Endpunkt-URL.

::: zone pivot="b2c-user-flow"

```http
https://contoso.azurewebsites.net/api/endpoint?code=0123456789 
```

Die API-Schlüsselauthentifizierung sollte nicht allein in der Produktion verwendet werden. Daher ist die Konfiguration für die Standard- oder Zertifikatauthentifizierung immer erforderlich. Wenn Sie für Entwicklungszwecke keine Authentifizierungsmethode implementieren möchten (nicht empfohlen), können Sie die Standardauthentifizierung auswählen und temporäre Werte für `username` und `password` verwenden, die von der API ignoriert werden können, während Sie die Autorisierung in Ihrer API implementieren.

::: zone-end

::: zone pivot="b2c-custom-policy"

Der API-Schlüssel kann als benutzerdefinierter HTTP-Header gesendet werden. Der [HTTP-Trigger in Azure Functions](../azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys) beispielsweise verwendet `x-functions-key` im HTTP-Header, um den Anforderer zu identifizieren.  

::: zone-end

### <a name="client-certificate"></a>Clientzertifikat

Die Client-Zertifikat-Authentifizierung ist ein Verfahren zur gegenseitigen zertifikatsbasierten Authentifizierung, bei dem der Client dem Server ein Client-Zertifikat zur Verfügung stellt, um seine Identität nachzuweisen. In diesem Fall verwendet Azure AD B2C das Zertifikat, das Sie als Teil der API-Connectorkonfiguration hochladen.  Dies erfolgt im Rahmen des SSL-Handshakes. 

Ihr API-Dienst kann dann den Zugriff nur auf Dienste beschränken, die über ordnungsgemäße Zertifikate verfügen. Bei dem Clientzertifikat handelt es sich um ein digitales X.509-Zertifikat im Format PKCS12 (PFX). In Produktionsumgebungen muss es von einer Zertifizierungsstelle signiert werden.

### <a name="http-basic-authentication"></a>HTTP-Standardauthentifizierung

Die HTTP-Standardauthentifizierung ist in [RFC 2617](https://tools.ietf.org/html/rfc2617) definiert. Azure AD B2C sendet eine HTTP-Anforderung mit den Clientanmeldeinformationen (`username` und `password`) im Header `Authorization`. Die Anmeldeinformationen werden als Base64-codierte Zeichenfolge im Format `username:password` angegeben. Die API überprüft dann diese Werte, um zu bestimmen, ob ein API-Aufruf abgelehnt werden soll oder nicht.

::: zone pivot="b2c-custom-policy"

### <a name="bearer-token"></a>Bearertoken

Die Definition der Bearertokenauthentifizierung finden Sie unter [OAuth2.0 Authorization Framework: Verwendung von Bearertoken (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt). Bei der Bearertokenauthentifizierung sendet Azure AD B2C eine HTTP-Anforderung mit einem Token im Autorisierungsheader.

```http
Authorization: Bearer <token>
```

Ein Bearertoken ist eine nicht transparente Zeichenfolge. Dabei kann es sich um ein JWT-Zugriffstoken oder eine beliebige Zeichenfolge handeln, von dem/der die REST-API erwartet, dass es/sie von Azure AD B2C im Autorisierungsheader gesendet wird. 
 
::: zone-end

## <a name="rest-api-platform"></a>REST-API-Plattform

Ihre REST-API kann auf beliebigen Plattformen basieren und in beliebigen Programmiersprachen geschrieben sein, solange sie sicher ist und Forderungen im JSON-Format senden und empfangen kann.

Die Anforderung an Ihren REST-API-Dienst kommt von Azure AD B2C-Servern. Der REST-API-Dienst muss auf einem öffentlich zugänglichen HTTPS-Endpunkt veröffentlicht werden. Die REST-API-Aufrufe gehen von der IP-Adresse eines Azure-Rechenzentrums ein.

Achten Sie beim Entwurf Ihres REST-API-Diensts und der zugrundeliegenden Komponenten (z. B. Datenbank und Dateisystem) auf Hochverfügbarkeit.


## <a name="next-steps"></a>Nächste Schritte



::: zone pivot="b2c-user-flow"

- Erfahren Sie, wie Sie [einen API-Connector zu einem Benutzerflow hinzufügen](add-api-connector.md).
- Erste Schritte mit den [Beispielen](code-samples.md#api-connectors)

::: zone-end

::: zone pivot="b2c-custom-policy"

In den folgenden Artikeln finden Sie Beispiele für die Verwendung eines technischen RESTful-Profils:

- [Exemplarische Vorgehensweise: Hinzufügen eines API-Connectors zu einem Benutzerflow für die Registrierung](add-api-connector.md)
- [Exemplarische Vorgehensweise: Hinzufügen von REST-API-Anspruchsaustauschvorgängen zu benutzerdefinierten Richtlinien in Azure Active Directory B2C](custom-policy-rest-api-claims-exchange.md)
- [Sichern von REST-API-Diensten](secure-rest-api.md)
- [Referenz: Technisches Profil „RESTful“](restful-technical-profile.md)

::: zone-end