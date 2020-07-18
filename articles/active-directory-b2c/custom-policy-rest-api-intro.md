---
title: REST-API-Anspruchsaustausch in benutzerdefinierter B2C-Richtlinie
titleSuffix: Azure AD B2C
description: Eine Einführung zum Erstellen einer Azure AD B2C User Journey, die mit RESTful-Diensten interagiert.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 05/18/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: fe328de9460efb743037f697c7f564e2c628278d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85388934"
---
# <a name="integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-custom-policy"></a>Integrieren von REST-API-Anspruchsaustauschvorgängen in Ihre benutzerdefinierte Azure AD B2C-Richtlinie

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Das Identity Experience Framework (IEF), das Azure Active Directory B2C (Azure AD B2C) zugrunde liegt, kann im Rahmen einer User Journey mit RESTful-APIs integriert werden. In diesem Artikel wird gezeigt, wie Sie eine User Journey erstellen, die mithilfe eines [technischen RESTful-Profils](https://identitydivision.visualstudio.com/defaultcollection/Identity%20CXP/_git/GTP?path=%2Fyoelh%2Fdocs%2Frest-api%2Frestful-technical-profile.md&version=GBmaster) mit einem RESTful-Dienst interagiert.

Mit Azure AD B2C können Sie einer User Journey Ihre eigene Geschäftslogik hinzufügen, indem Sie Ihren eigenen RESTful-Dienst aufrufen. Das Identity Experience Framework kann Daten an Ihren RESTful-Dienst senden und von ihm empfangen, um Ansprüche auszutauschen. Beispielsweise können Sie folgende Aktionen ausführen:

- **Überprüfen von Benutzereingabedaten**. Sie können beispielsweise überprüfen, ob die vom Benutzer angegebene E-Mail-Adresse in Ihrer Kundendatenbank vorhanden ist. Und wenn dies nicht der Fall ist, können Sie einen Fehler ausgeben.
- **Verarbeiten von Ansprüchen**. Wenn ein Benutzer seinen Vornamen vollständig in Kleinbuchstaben oder Großbuchstaben eingibt, kann die REST-API den Namen so formatieren, dass nur der erste Buchstabe groß geschrieben wird, und den Namen in dieser Form an Azure AD B2C zurückgeben.
- **Anreichern von Benutzerdaten durch eine stärkere Integration in die Branchenanwendungen des Unternehmens**. Ihr RESTful-Dienst kann die E-Mail-Adresse des Benutzers empfangen, die Kundendatenbank abfragen und die Treuenummer des Benutzers an Azure AD B2C zurückgeben. Die Rückgabeansprüche können dann im Azure AD-Konto des Benutzers gespeichert, in den nächsten Orchestrierungsschritten ausgewertet oder in das Zugriffstoken eingebunden werden.
- **Ausführen von benutzerdefinierter Geschäftslogik**. Sie können Pushbenachrichtigungen senden, Unternehmensdatenbanken aktualisieren, einen Benutzermigrationsprozess ausführen, Berechtigungen verwalten, Datenbanken überwachen und andere Workflows ausführen.

![Diagramm eines Anspruchsaustauschs über den RESTful-Dienst](media/custom-policy-rest-api-intro/restful-service-claims-exchange.png)

> [!NOTE]
> Wenn der RESTful-Dienst nur langsam reagiert oder gar keine Antwort an Azure AD B2C sendet, beträgt der Timeoutwert 30 Sekunden, und die Anzahl der Wiederholungsversuche ist auf „2“ festgelegt (d. h. es gibt insgesamt 3 Versuche). Die Einstellungen für das Timeout und die Anzahl der Wiederholungsversuche können derzeit nicht konfiguriert werden.

## <a name="calling-a-restful-service"></a>Aufrufen eines RESTful-Diensts

Die Interaktion umfasst einen Anspruchsaustausch von Informationen zwischen den REST-API-Ansprüchen und Azure AD B2C. Sie können die Integration in die RESTful-Dienste auf folgende Weise entwerfen:

- **Technisches Validierungsprofil**. Der Anruf des RESTful-Diensts erfolgt innerhalb eines [technischen Validierungsprofils](validation-technical-profile.md) des angegebenen [selbstbestätigten technischen Profils](self-asserted-technical-profile.md) oder über ein [Anzeigesteuerelement zur Überprüfung](display-control-verification.md) einer [Anzeigesteuerung](display-controls.md). Mit dem technischen Validierungsprofil werden die vom Benutzer bereitgestellten Daten überprüft, bevor die User Journey fortgesetzt wird. Beim technischen Validierungsprofil haben Sie folgende Möglichkeiten:

  - Senden von Ansprüchen an Ihre REST-API
  - Überprüfen von Ansprüchen und Auslösen von benutzerdefinierten Fehlermeldungen, die dem Benutzer angezeigt werden
  - Zurücksenden von Ansprüchen von der REST-API an nachfolgende Orchestrierungsschritte

- **Anspruchsaustausch**. Ein direkter Anspruchsaustausch kann durch Aufrufen eines technischen REST-API-Profils direkt aus einem Orchestrierungsschritt in einer [User Journey](userjourneys.md) konfiguriert werden. Diese Definition ist auf folgende Vorgänge beschränkt:

  - Senden von Ansprüchen an Ihre REST-API
  - Überprüfen von Ansprüchen und Auslösen von benutzerdefinierten Fehlermeldungen, die an die Anwendung zurückgegeben werden
  - Zurücksenden von Ansprüchen von der REST-API an nachfolgende Orchestrierungsschritte

Sie können einen Rest-API-Aufruf in jedem Schritt der User Journey hinzufügen, die durch eine benutzerdefinierte Richtlinie definiert ist. Beispielsweise können Sie in folgenden Schritten eine Rest-API aufrufen:

- Während der Anmeldung, kurz bevor Azure AD B2C die Anmeldeinformationen überprüft
- Unmittelbar nach der Anmeldung
- Bevor Azure AD B2C ein neues Konto im Verzeichnis erstellt
- Nachdem Azure AD B2C ein neues Konto im Verzeichnis erstellt hat
- Bevor Azure AD B2C ein Zugriffstoken ausgibt

![Erfassung über das technische Validierungsprofil](media/custom-policy-rest-api-intro/validation-technical-profile.png)

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

Die Ausgabeansprüche sollten wie folgt aussehen:

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


Die Ausgabeansprüche sollten wie folgt aussehen:

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="contacts.0.person.name" />
  <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="contacts.0.person.emails.0.email" />
  <OutputClaim ClaimTypeReferenceId="loyaltyNumber" PartnerClaimType="contacts.0.person.loyaltyNumber" />
</OutputClaims>
```

## <a name="security-considerations"></a>Sicherheitshinweise

Sie müssen ihren REST-API-Endpunkt schützen, damit nur authentifizierte Clients mit ihm kommunizieren können. Die REST-API muss einen HTTPS-Endpunkt verwenden. Legen Sie die Metadaten „AuthenticationType“ auf eine der folgenden Authentifizierungsmethoden fest:

- **Clientzertifikat**: Durch die Authentifizierung mit Clientzertifikat wird der Zugriff eingeschränkt. Nur Dienste mit den richtigen Zertifikaten erhalten Zugriff auf Ihre API. Das Clientzertifikat wird in einem Azure AD B2C-Richtlinienschlüssel gespeichert. Erfahren Sie mehr über das [Schützen Ihres RESTful-Diensts mit Clientzertifikaten](secure-rest-api.md#https-client-certificate-authentication).
- **Standard**: Die REST-API wird durch die HTTP-Standardauthentifizierung gesichert. Nur verifizierte Benutzer, einschließlich Azure AD B2C, haben Zugriff auf Ihre API. Der Benutzername und das Kennwort werden in Azure AD B2C-Richtlinienschlüsseln gespeichert. Erfahren Sie mehr über das [Schützen Ihrer RESTful-Dienste unter Verwendung der HTTP-Standardauthentifizierung](secure-rest-api.md#http-basic-authentication).
- **Bearer**: Der Zugriff wird mithilfe eines OAuth2-Clientzugriffstokens eingeschränkt. Das Zugriffstoken wird in einem Azure AD B2C-Richtlinienschlüssel gespeichert. Erfahren Sie mehr über das [Schützen Ihres RESTful-Diensts mit Bearertoken](secure-rest-api.md#oauth2-bearer-authentication).

## <a name="rest-api-platform"></a>REST-API-Plattform
Ihre REST-API kann auf jeder beliebigen Plattform aufsetzen und in einer beliebigen Programmiersprache geschrieben werden, solange sie sicher ist und Ansprüche senden und empfangen kann, wie im [technischen RESTful-Profil](restful-technical-profile.md) angegeben.

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

Dies kann nur durch Aufrufen eines technischen REST-API-Profils aus einem technischen Validierungsprofil erreicht werden. Dadurch kann der Benutzer die Daten auf der Seite korrigieren und die Validierung bei der Seitenübermittlung erneut ausführen.

Die HTTP-Antwort 409 ist erforderlich, um in diesem Orchestrierungsschritt die Verarbeitung aller nachfolgenden technischen Validierungsprofile zu verhindern.

Wenn Sie direkt aus einer User Journey auf ein technisches REST-API-Profil verweisen, wird der Benutzer mit der entsprechenden Fehlermeldung wieder an die Anwendung der vertrauenden Seite zurückgeleitet.

## <a name="publishing-your-rest-api"></a>Veröffentlichen Ihrer REST-API

Die Anforderung an Ihren REST-API-Dienst kommt von Azure AD B2C-Servern. Der REST-API-Dienst muss auf einem öffentlich zugänglichen HTTPS-Endpunkt veröffentlicht werden. Die REST-API-Aufrufe gehen von der IP-Adresse eines Azure-Rechenzentrums ein.

Achten Sie beim Entwurf Ihres REST-API-Diensts und der zugrundeliegenden Komponenten (z. B. Datenbank und Dateisystem) auf Hochverfügbarkeit.

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln finden Sie Beispiele für die Verwendung eines technischen RESTful-Profils:

- [Exemplarische Vorgehensweise: Integrieren von REST-API-Anspruchsaustauschvorgängen in Ihre Azure AD B2C-User Journey zur Validierung der Benutzereingabe](custom-policy-rest-api-claims-validation.md)
- [Exemplarische Vorgehensweise: Hinzufügen von REST-API-Anspruchsaustauschvorgängen zu benutzerdefinierten Richtlinien in Azure Active Directory B2C](custom-policy-rest-api-claims-validation.md)
- [Sichern von REST-API-Diensten](secure-rest-api.md)
- [Referenz: Technisches Profil „RESTful“](restful-technical-profile.md)
