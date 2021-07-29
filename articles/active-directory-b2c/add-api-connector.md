---
title: Hinzufügen von API-Connectors zu Benutzerflows (Vorschauversion)
description: Konfigurieren Sie einen API-Connector, der in einem Benutzerflow verwendet werden soll.
services: active-directory-b2c
ms.service: active-directory
ms.subservice: B2C
ms.topic: how-to
ms.date: 05/03/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 1913b6cf14aaf31d610adcf446dbe91326e02ff1
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108742911"
---
# <a name="add-an-api-connector-to-a-sign-up-user-flow"></a>Hinzufügen eines API-Connectors zu einem Benutzerflow für die Registrierung

Als Entwickler oder IT-Administrator können Sie API-Connectors verwenden, um Ihre Flows zur Benutzerregistrierung mit REST-APIs zu integrieren, um die Registrierungsumgebung anzupassen und in externe Systemen zu integrieren. Am Ende dieser exemplarischen Vorgehensweise sind Sie in der Lage, einen Azure AD B2C-Benutzerflow zu erstellen, der mit [REST-API-Diensten](api-connectors-overview.md) interagiert. 

::: zone pivot="b2c-user-flow"

In diesem Szenario überprüft die REST-API, ob die Domäne der E-Mail-Adresse „fabrikam.com“ oder „fabricam.com“ lautet. Der vom Benutzer angegebene Anzeigename umfasst mehr als fünf Zeichen. Anschließend wird die Auftragsbezeichnung mit einem statischen Wert zurückgegeben. 

> [!IMPORTANT]
> API-Connectors für die Registrierung sind ein Feature der öffentlichen Vorschauversion von Azure AD B2C. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

::: zone-end

::: zone pivot="b2c-custom-policy"

In diesem Szenario fügen wir die Möglichkeit für Benutzer hinzu, auf der Azure AD B2C-Registrierungsseite eine Treuebonusnummer einzugeben. Die REST-API überprüft, ob die Kombination aus E-Mail-Adresse und Treuebonusnummer einem Aktionscode zugeordnet ist. Wenn die REST-API einen Aktionscode für diesen Benutzer findet, wird dieser an Azure AD B2C zurückgegeben. Schließlich wird der Aktionscode in die Tokenansprüche so eingefügt, dass er von der Anwendung genutzt werden kann.

Sie können die Interaktion auch als Orchestrierungsschritt entwerfen. Dies bietet sich an, wenn die REST-API die Daten auf dem Bildschirm nicht überprüft und stets Ansprüche zurückgibt. Weitere Informationen finden Sie unter [Exemplarische Vorgehensweise: Integrieren von REST-API-Anspruchsaustausch-Vorgängen in Ihre Azure AD B2C User Journey als Orchestrierungsschritt](custom-policy-rest-api-claims-exchange.md)

::: zone-end

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

::: zone pivot="b2c-user-flow"

## <a name="create-an-api-connector"></a>Erstellen eines API-Connectors

Um einen [API-Connector](api-connectors-overview.md) zu verwenden, erstellen Sie zunächst den API-Connector und aktivieren ihn anschließend in einem Benutzerflow.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie unter **Azure-Dienste** die Option **Azure AD B2C** aus.
4. Wählen Sie **API-Connectors (Vorschau)** und dann **Neuer API-Connector** aus.

   ![Hinzufügen eines neuen API-Connectors](./media/add-api-connector/api-connector-new.png)

5. Geben Sie einen Anzeigenamen für den Aufruf an, beispielsweise **Benutzerinformationen überprüfen**.
6. Geben Sie die **Endpunkt-URL** für den API-Aufruf an.
7. Wählen Sie den **Authentifizierungstyp** aus, und konfigurieren Sie die Authentifizierungsinformationen zum Aufrufen Ihrer API. Weitere Informationen zum Schützen Ihrer API finden Sie weiter unten in diesem Artikel.

    ![Konfigurieren eines API-Connectors](./media/add-api-connector/api-connector-config.png)

8. Wählen Sie **Speichern** aus.

## <a name="securing-the-api-endpoint"></a>Schützen des API-Endpunkts
Sie können Ihren API-Endpunkt entweder mithilfe der HTTP-Standardauthentifizierung oder mithilfe der HTTPS-Clientzertifikatauthentifizierung (Vorschau) schützen. In beiden Fällen müssen Sie die Anmeldeinformationen angeben, die Azure AD B2C beim Aufrufen Ihres API-Endpunkts verwenden wird. Der API-Endpunkt überprüft dann die Anmeldeinformationen und führt Autorisierungsentscheidungen aus.

### <a name="http-basic-authentication"></a>HTTP-Standardauthentifizierung
Die HTTP-Standardauthentifizierung ist in [RFC 2617](https://tools.ietf.org/html/rfc2617) definiert. Azure AD B2C sendet eine HTTP-Anforderung mit den Clientanmeldeinformationen (`username` und `password`) im Header `Authorization`. Die Anmeldeinformationen werden als Base64-codierte Zeichenfolge im Format `username:password` angegeben. Die API überprüft dann diese Werte, um zu bestimmen, ob ein API-Aufruf abgelehnt werden soll oder nicht.

### <a name="https-client-certificate-authentication-preview"></a>HTTPS-Clientzertifikatauthentifizierung (Vorschau)

> [!IMPORTANT]
> Diese Funktion befindet sich in der Vorschauhase und wird ohne Vereinbarung zum Servicelevel bereitgestellt. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bei der Client-Zertifikat-Authentifizierung handelt es sich um eine auf gegenseitigen Zertifikaten basierende Authentifizierungsmethode, bei welcher der Client dem Server ein Client-Zertifikat zur Verfügung stellt, um seine Identität zu beweisen. In diesem Fall verwendet Azure AD B2C das Zertifikat, das Sie als Teil der API-Connectorkonfiguration hochladen. Dies erfolgt im Rahmen des TLS-/SSL-Handshakes. Ihr API-Dienst kann dann den Zugriff nur auf Dienste beschränken, die über ordnungsgemäße Zertifikate verfügen. Bei dem Clientzertifikat handelt es sich um ein digitales X.509-Zertifikat im Format PKCS12 (PFX). In Produktionsumgebungen muss es von einer Zertifizierungsstelle signiert werden. 

Zum Erstellen eines Zertifikats können Sie den [Azure Key Vault](../key-vault/certificates/create-certificate.md) verwenden, der über Optionen für selbstsignierte Zertifikate und Integrationen mit Zertifikatausstelleranbietern für signierte Zertifikate verfügt. Empfohlene Einstellungen umfassen:
- **Betreff**: `CN=<yourapiname>.<tenantname>.onmicrosoft.com`
- **Inhaltstyp**: `PKCS #12`
- **Gültigkeitsdauer Aktionstyp**: `Email all contacts at a given percentage lifetime` oder `Email all contacts a given number of days before expiry`
- **Schlüsseltyp**: `RSA`
- **Schlüsselgröße**: `2048`
- **Exportierbarer privater Schlüssel**: `Yes` (um die pfx-Datei exportieren zu können)

Sie können dann [das Zertifikat exportieren](../key-vault/certificates/how-to-export-certificate.md). Sie können alternativ das Cmdlet [New-SelfSignedCertificate](../active-directory-b2c/secure-rest-api.md#prepare-a-self-signed-certificate-optional) von PowerShell verwenden, um ein selbstsigniertes Zertifikat zu generieren.

Nachdem Sie ein Zertifikat haben, können Sie es als Teil der Konfiguration des API-Connectors hochladen. Beachten Sie, dass das Kennwort nur für kennwortgeschützte Zertifikatdateien erforderlich ist.

Ihre API muss die Autorisierung basierend auf gesendeten Client-Zertifikaten implementieren, um die API-Endpunkte zu schützen. Für den Azure App Service und die Azure Funktionen finden Sie unter [Konfigurieren der gegenseitigen TLS-Authentifizierung](../app-service/app-service-web-configure-tls-mutual-auth.md) Informationen zum Aktivieren und *Validieren des Zertifikats über Ihren API-Code*.  Sie können auch das Azure API Management verwenden, um die [Eigenschaften](
../api-management/api-management-howto-mutual-certificates-for-clients.md) von Client-Zertifikaten mithilfe von Richtlinienbegriffen auf die gewünschten Werte zu prüfen.

Es wird empfohlen, Erinnerungswarnungen für den Zeitpunkt festzulegen, an dem das Zertifikat abläuft. Sie müssen ein neues Zertifikat erstellen und die obigen Schritte wiederholen. Ihr API-Dienst kann vorübergehend weiterhin alte und neue Zertifikate akzeptieren, während das neue Zertifikat bereitgestellt wird. Um ein neues Zertifikat in einen bestehenden API-Anschluss hochzuladen, wählen Sie den API-Anschluss unter **API-Anschlüsse** aus und klicken Sie auf **Neues Zertifikat hochladen**. Azure Active Directory verwendet automatisch das zuletzt hochgeladene Zertifikat, das nicht abgelaufen ist und nach dem Startdatum erstellt wurde.

### <a name="api-key"></a>API-Schlüssel

Einige Dienste verwenden einen API-Schlüssel-Mechanismus, um den Zugriff auf die HTTP-Endpunkte während der Entwicklung zu verschleiern. Sie können diesen für [Azure Functions](../azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys) einrichten, indem Sie `code` als Abfrageparameter in die **Endpunkt-URL** einschließen. Beispiel: `https://contoso.azurewebsites.net/api/endpoint`<b>`?code=0123456789`</b>. 

Hierbei handelt es sich nicht um einen Mechanismus, der allein in der Produktion verwendet werden sollte. Daher ist die Konfiguration für die Standard- oder Zertifikatauthentifizierung immer erforderlich. Wenn Sie für Entwicklungszwecke keine Authentifizierungsmethode implementieren möchten (nicht empfohlen), können Sie die Standardauthentifizierung auswählen und temporäre Werte für `username` und `password` verwenden, die von der API ignoriert werden können, während Sie die Autorisierung in Ihrer API implementieren.

## <a name="the-request-sent-to-your-api"></a>An die API gesendete Anforderung
Ein API-Connector wird als **HTTP POST**-Anforderung dargestellt und sendet Benutzerattribute („Ansprüche“) als Schlüssel-Wert-Paare in einem JSON-Text. Attribute werden ähnlich wie [Microsoft Graph](/graph/api/resources/user#properties)-Benutzereigenschaften serialisiert. 

**Beispielanforderung**
```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "givenName":"John",
 "surname":"Smith",
 "jobTitle":"Supplier",
 "streetAddress":"1000 Microsoft Way",
 "city":"Seattle",
 "postalCode": "12345",
 "state":"Washington",
 "country":"United States",
 "extension_<extensions-app-id>_CustomAttribute1": "custom attribute value",
 "extension_<extensions-app-id>_CustomAttribute2": "custom attribute value",
 "ui_locales":"en-US"
}
```

Nur Benutzereigenschaften und benutzerdefinierte Attribute, die unter **Azure AD B2C** > **Benutzerattribute** aufgeführt sind, können in der Anforderung gesendet werden.

Benutzerdefinierte Attribute sind im Verzeichnis im Format **extension_\<extensions-app-id>_CustomAttribute** vorhanden. Die API sollte den Empfang von Ansprüchen in diesem serialisierten Format erwarten. Weitere Informationen zu benutzerdefinierten Attributen finden Sie unter [Definieren benutzerdefinierter Attribute in Azure AD B2C](user-flow-custom-attributes.md).

Außerdem wird der Anspruch **Gebietsschema der Benutzeroberfläche („ui_locales“)** standardmäßig in allen Anforderungen gesendet. Er gibt das Gebietsschema (oder auch mehrere Gebietsschemas) eines Benutzers an, wie es auf seinem Gerät konfiguriert ist. Dieses kann von der API verwendet werden, um internationalisierte Antworten zurückzugeben.

> [!IMPORTANT]
> Wenn ein Anspruch zum Zeitpunkt des Aufrufs des API-Endpunkts keinen Wert enthält, wird der Anspruch nicht an die API gesendet. Ihre API sollte so entworfen sein, dass explizit geprüft wird, ob ein Anspruch in der Anforderung enthalten ist. Fehlt der Anspruch, sollte eine entsprechende Verarbeitung erfolgen.

> [!TIP] 
> Mit den Ansprüchen [**Identitäten („identities“)**](/graph/api/resources/objectidentity) und **E-Mail-Adresse („email“)** kann Ihre API einen Benutzer identifizieren, bevor er über ein Konto in Ihrem Mandanten verfügt. 

## <a name="enable-the-api-connector-in-a-user-flow"></a>Aktivieren des API-Connectors in einem Benutzerflow

Führen Sie die folgenden Schritte aus, um einem Benutzerflow für die Registrierung einen API-Connector hinzuzufügen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie unter **Azure-Dienste** die Option **Azure AD B2C** aus.
4. Wählen Sie **Benutzerflows** und dann den Benutzerflow aus, dem Sie den API-Connector hinzufügen möchten.
5. Wählen Sie **API-Connectors** und dann die API-Endpunkte aus, die bei den folgenden Schritten im Benutzerflow aufgerufen werden sollen:

   - **Nach Anmeldung bei einem Identitätsanbieter**
   - **Vor dem Erstellen des Benutzers**

   ![Hinzufügen von APIs zum Benutzerflow](./media/add-api-connector/api-connectors-user-flow-select.png)

6. Wählen Sie **Speichern** aus.

## <a name="after-signing-in-with-an-identity-provider"></a>Nach Anmeldung bei einem Identitätsanbieter

Ein API-Connector in diesem Schritt des Registrierungsprozesses wird unmittelbar nach der Authentifizierung des Benutzers bei einem Identitätsanbieter (etwa Google, Facebook und Azure AD) aufgerufen. Dieser Schritt geht der ***Seite zur Attributsammlung***  voraus, die dem Benutzer zum Sammeln von Benutzerattributen angezeigt wird. Dieser Schritt wird nicht aufgerufen, wenn sich ein Benutzer mit einem lokalen Konto registriert.

### <a name="example-request-sent-to-the-api-at-this-step"></a>In diesem Schritt an die API gesendete Beispielanforderung
```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [ 
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "givenName":"John",
 "lastName":"Smith",
 "ui_locales":"en-US"
}
```

Die genauen Ansprüche, die an die API gesendet werden, hängen von den vom Identitätsanbieter bereitgestellten Informationen ab. Der Anspruch „email“ wird immer gesendet.

### <a name="expected-response-types-from-the-web-api-at-this-step"></a>Von der Web-API in diesem Schritt erwartete Antworttypen

Wenn die Web-API während eines Benutzerflows eine HTTP-Anforderung von Azure AD empfängt, kann sie folgende Antworten zurückgeben:

- Fortsetzungsantwort
- Blockierungsantwort

#### <a name="continuation-response"></a>Fortsetzungsantwort

Eine Fortsetzungsantwort gibt an, dass der Benutzerflow mit dem nächsten Schritt fortgesetzt werden soll: Attributerfassungsseite.

In einer Fortsetzungsantwort kann die API Ansprüche zurückgeben. Wenn ein Anspruch von der API zurückgegeben wird, passiert mit dem Anspruch Folgendes:

- Das Eingabefeld auf der Attributerfassungsseite wird vorab aufgefüllt.

Sehen Sie sich ein Beispiel für eine [Fortsetzungsantwort](#example-of-a-continuation-response) an.

#### <a name="blocking-response"></a>Blockierungsantwort

Eine Blockierungsantwort beendet den Benutzerflow. Sie kann von der API absichtlich ausgegeben werden, um die Fortsetzung des Benutzerflows zu beenden, indem für den Benutzer eine Blockierungsseite angezeigt wird. Auf der Blockierungsseite wird die von der API angegebene `userMessage` angezeigt.

Sehen Sie sich ein Beispiel für eine [Blockierungsantwort](#example-of-a-blocking-response) an.

## <a name="before-creating-the-user"></a>Vor dem Erstellen des Benutzers

Ein API-Connector in diesem Schritt des Registrierungsprozesses wird nach der Seite zur Attributsammlung aufgerufen, sofern vorhanden. Dieser Schritt wird immer aufgerufen, bevor ein Benutzerkonto erstellt wird.

### <a name="example-request-sent-to-the-api-at-this-step"></a>In diesem Schritt an die API gesendete Beispielanforderung

```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "givenName":"John",
 "surname":"Smith",
 "jobTitle":"Supplier",
 "streetAddress":"1000 Microsoft Way",
 "city":"Seattle",
 "postalCode": "12345",
 "state":"Washington",
 "country":"United States",
 "extension_<extensions-app-id>_CustomAttribute1": "custom attribute value",
 "extension_<extensions-app-id>_CustomAttribute2": "custom attribute value",
 "ui_locales":"en-US"
}
```

Die Ansprüche, die an die API gesendet werden, hängen von den Informationen ab, die vom Benutzer gesammelt oder vom Identitätsanbieter bereitgestellt werden.

### <a name="expected-response-types-from-the-web-api-at-this-step"></a>Von der Web-API in diesem Schritt erwartete Antworttypen

Wenn die Web-API während eines Benutzerflows eine HTTP-Anforderung von Azure AD empfängt, kann sie folgende Antworten zurückgeben:

- Fortsetzungsantwort
- Blockierungsantwort
- Überprüfungsantwort

#### <a name="continuation-response"></a>Fortsetzungsantwort

Eine Fortsetzungsantwort gibt an, dass der Benutzerflow mit dem nächsten Schritt fortgesetzt werden soll: Erstellen des Benutzers im Verzeichnis.

In einer Fortsetzungsantwort kann die API Ansprüche zurückgeben. Wenn ein Anspruch von der API zurückgegeben wird, passiert mit dem Anspruch Folgendes:

- Alle Werte, die dem Anspruch über die Attributerfassungsseite bereits zugewiesen wurden, werden überschrieben.

Sehen Sie sich ein Beispiel für eine [Fortsetzungsantwort](#example-of-a-continuation-response) an.

#### <a name="blocking-response"></a>Blockierungsantwort
Eine Blockierungsantwort beendet den Benutzerflow. Sie kann von der API absichtlich ausgegeben werden, um die Fortsetzung des Benutzerflows zu beenden, indem für den Benutzer eine Blockierungsseite angezeigt wird. Auf der Blockierungsseite wird die von der API angegebene `userMessage` angezeigt.

Sehen Sie sich ein Beispiel für eine [Blockierungsantwort](#example-of-a-blocking-response) an.

### <a name="validation-error-response"></a>Validierungsfehlerantwort
 Wenn die API eine Validierungsfehlerantwort ausgibt, wird der Benutzerflow auf der Attributsammlungsseite angehalten, und dem Benutzer wird eine `userMessage` angezeigt. Der Benutzer kann dann das Formular bearbeiten und erneut senden. Diese Antwort kann für die Eingabevalidierung verwendet werden.

Sehen Sie sich ein Beispiel für eine [Validierungsfehlerantwort](#example-of-a-validation-error-response) an.

## <a name="example-responses"></a>Beispielantworten

### <a name="example-of-a-continuation-response"></a>Beispiel für eine Fortsetzungsantwort

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "Continue",
    "postalCode": "12349", // return claim
    "extension_<extensions-app-id>_CustomAttribute": "value" // return claim
}
```

| Parameter                                          | type              | Erforderlich | BESCHREIBUNG                                                                                                                                                                                                                                                                            |
| -------------------------------------------------- | ----------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| action                                             | String            | Ja      | Der Wert muss `Continue` sein.                                                                                                                                                                                                                                                              |
| \<builtInUserAttribute>                            | \<attribute-type> | Nein       | Zurückgegebene Werte können Werte überschreiben, die von einem Benutzer gesammelt wurden. Diese können auch im Token zurückgegeben werden, wenn sie als **Anwendungsanspruch** ausgewählt sind.                                              |
| \<extension\_{extensions-app-id}\_CustomAttribute> | \<attribute-type> | Nein       | Der Anspruch muss `_<extensions-app-id>_` nicht enthalten. Zurückgegebene Werte können Werte überschreiben, die von einem Benutzer gesammelt wurden. Diese können auch im Token zurückgegeben werden, wenn sie als **Anwendungsanspruch** ausgewählt sind.  |

### <a name="example-of-a-blocking-response"></a>Beispiel für eine Blockierungsantwort

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "ShowBlockPage",
    "userMessage": "There was a problem with your request. You are not able to sign up at this time.",
}

```

| Parameter   | type   | Erforderlich | BESCHREIBUNG                                                                |
| ----------- | ------ | -------- | -------------------------------------------------------------------------- |
| version     | String | Ja      | Die Version der API.                                                    |
| action      | String | Ja      | Der Wert muss `ShowBlockPage` sein.                                              |
| userMessage | String | Ja      | Meldung, die für den Benutzer angezeigt wird.                                            |

**Anzeige einer Blockierungsantwort für den Endbenutzer**

![Beispiel für eine Blockierungsseite](./media/add-api-connector/blocking-page-response.png)

### <a name="example-of-a-validation-error-response"></a>Beispiel für eine Validierungsfehlerantwort

```http
HTTP/1.1 400 Bad Request
Content-type: application/json

{
    "version": "1.0.0",
    "status": 400,
    "action": "ValidationError",
    "userMessage": "Please enter a valid Postal Code."
}
```

| Parameter   | type    | Erforderlich | BESCHREIBUNG                                                                |
| ----------- | ------- | -------- | -------------------------------------------------------------------------- |
| version     | String  | Ja      | Die Version Ihrer API                                                    |
| action      | String  | Ja      | Der Wert muss `ValidationError` sein.                                           |
| status      | Integer / Zeichenkette | Ja      | Für eine Validierungsfehlerantwort muss der Wert `400` oder `"400"` sein.  |
| userMessage | String  | Ja      | Meldung, die für den Benutzer angezeigt wird.                                            |

> [!NOTE]
> Der HTTP-Statuscode muss „400“ lauten, und der Antworttext muss den Wert „status“ enthalten.

**Anzeige einer Validierungsfehlerantwort für den Endbenutzer**

![Beispiel für eine Validierungsseite](./media/add-api-connector/validation-error-postal-code.png)


::: zone-end

::: zone pivot="b2c-custom-policy"


## <a name="prepare-a-rest-api-endpoint"></a>Vorbereiten eines REST-API-Endpunkts

Für diese exemplarische Vorgehensweise benötigen Sie eine REST-API, die überprüft, ob eine E-Mail-Adresse in Ihrem Back-End-System mit einer Treuepunkt-ID registriert ist. Falls ja, muss die REST-API einen Aktionscode für die Registrierung zurückgeben, den der Kunde zum Kauf von Waren in Ihrer Anwendung angeben kann. Andernfalls sollte die REST-API die HTTP-Fehlermeldung 409 zurückgeben: „Die Treuepunkt-ID {loyalty ID} ist nicht mit der E-Mail-Adresse {Email} verknüpft.“

Der folgende JSON-Code veranschaulicht die Daten, die Azure AD B2C an Ihren REST-API-Endpunkt sendet. 

```json
{
    "email": "User email address",
    "language": "Current UI language",
    "loyaltyId": "User loyalty ID"
}
```

Sobald Ihre REST-API die Daten überprüft, muss sie HTTP 200 (OK) mit den folgenden JSON-Daten zurückgeben:

```json
{
    "promoCode": "24534"
}
```

Wenn die Prüfung fehlgeschlagen ist, muss die REST-API den HTTP-Fehler 409 (Konflikt) mit dem JSON-Element `userMessage` zurückgeben. Das IEF erwartet den Anspruch `userMessage`, den die REST-API zurückgibt. Dieser Anspruch wird dem Benutzer als Zeichenfolge dargestellt, wenn die Überprüfung fehlschlägt.

```json
{
    "version": "1.0.1",
    "status": 409,
    "userMessage": "LoyaltyId ID '1234' is not associated with 'david@contoso.com' email address."
}
```

Die Einrichtung des REST-API-Endpunkts wird in diesem Artikel nicht behandelt. Wir haben ein [Azure Functions](../azure-functions/functions-reference.md)-Beispiel erstellt. Sie können auf [GitHub](https://github.com/azure-ad-b2c/rest-api/tree/master/source-code/azure-function) auf den gesamten Azure-Funktionscode zugreifen.

## <a name="define-claims"></a>Definieren von Ansprüchen

Ein Anspruch bietet eine temporäre Speicherung von Daten während der Ausführung einer Azure AD B2C-Richtlinie. Sie können Ansprüche im Abschnitt [Anspruchsschema](claimsschema.md) deklarieren. 

1. Öffnen Sie die Erweiterungsdatei Ihrer Richtlinie. Beispiel: <em>`SocialAndLocalAccounts/`**`TrustFrameworkExtensions.xml`**</em>.
1. Suchen Sie nach dem Element [BuildingBlocks](buildingblocks.md). Wenn das Element nicht vorhanden ist, fügen Sie es hinzu.
1. Suchen Sie nach dem Element [ClaimsSchema](claimsschema.md). Wenn das Element nicht vorhanden ist, fügen Sie es hinzu.
1. Fügen Sie dem Element **ClaimsSchema** die folgenden Ansprüche hinzu.  

```xml
<ClaimType Id="loyaltyId">
  <DisplayName>Your loyalty ID</DisplayName>
  <DataType>string</DataType>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
<ClaimType Id="promoCode">
  <DisplayName>Your promo code</DisplayName>
  <DataType>string</DataType>
  <UserInputType>Paragraph</UserInputType>
</ClaimType>
  <ClaimType Id="userLanguage">
  <DisplayName>User UI language (used by REST API to return localized error messages)</DisplayName>
  <DataType>string</DataType>
</ClaimType>
```

## <a name="add-the-restful-api-technical-profile"></a>Hinzufügen des technischen Profils für die RESTful-API 

Ein [technisches RESTful-Profil](restful-technical-profile.md) bietet Unterstützung bei der Anbindung an Ihren eigenen RESTful-Dienst. Azure AD B2C sendet Daten an den RESTful-Dienst in einer `InputClaims`-Sammlung und erhält Daten in einer `OutputClaims`-Sammlung zurück. Suchen Sie das Element **ClaimsProviders**, und fügen Sie wie folgt einen neuen Anspruchsanbieter hinzu:

```xml
<ClaimsProvider>
  <DisplayName>REST APIs</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="REST-ValidateProfile">
      <DisplayName>Check loyaltyId Azure Function web hook</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <!-- Set the ServiceUrl with your own REST API endpoint -->
        <Item Key="ServiceUrl">https://your-account.azurewebsites.net/api/ValidateProfile?code=your-code</Item>
        <Item Key="SendClaimsIn">Body</Item>
        <!-- Set AuthenticationType to Basic or ClientCertificate in production environments -->
        <Item Key="AuthenticationType">None</Item>
        <!-- REMOVE the following line in production environments -->
        <Item Key="AllowInsecureAuthInProduction">true</Item>
      </Metadata>
      <InputClaims>
        <!-- Claims sent to your REST API -->
        <InputClaim ClaimTypeReferenceId="loyaltyId" />
        <InputClaim ClaimTypeReferenceId="email" />
        <InputClaim ClaimTypeReferenceId="userLanguage" PartnerClaimType="lang" DefaultValue="{Culture:LCID}" AlwaysUseDefaultValue="true" />
      </InputClaims>
      <OutputClaims>
        <!-- Claims parsed from your REST API -->
        <OutputClaim ClaimTypeReferenceId="promoCode" />
      </OutputClaims>
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

In diesem Beispiel wird `userLanguage` als `lang` innerhalb der JSON-Nutzlast an den REST-Dienst gesendet. Der Wert des Anspruchs `userLanguage` enthält die ID der aktuellen Benutzersprache. Weitere Informationen finden Sie unter [Anspruchskonfliktlöser](claim-resolver-overview.md).

### <a name="configure-the-restful-api-technical-profile"></a>Konfigurieren des technischen Profils für die RESTful-API 

Nachdem Sie Ihre REST-API bereitgestellt haben, legen Sie die Metadaten des technischen Profils `REST-ValidateProfile` so fest, dass sie Ihre eigene REST-API wiedergeben, darunter:

- **ServiceUrl**. Legt die URL des REST-API-Endpunkts fest.
- **SendClaimsIn**. Gibt an, wie die Eingabeansprüche an den RESTful-Anspruchsanbieter gesendet werden.
- **AuthenticationType**. Legt den Typ der Authentifizierung fest, die vom RESTful-Anspruchsanbieter ausgeführt wird. 
- **AllowInsecureAuthInProduction**. Sorgen Sie in einer Produktionsumgebung dafür, dass diese Metadaten auf `true` festgelegt werden.
    
Weitere Konfigurationen finden Sie in den [Metadaten für das technische RESTful-Profil](restful-technical-profile.md#metadata).

Die Kommentare `AuthenticationType` und `AllowInsecureAuthInProduction` oben geben Änderungen an, die Sie beim Wechsel zu einer Produktionsumgebung vornehmen sollten. Informationen zum Schützen Ihrer RESTful-APIs für die Produktionsumgebung finden Sie unter [Schützen von RESTful-APIs](secure-rest-api.md).

## <a name="validate-the-user-input"></a>Überprüfen der Benutzereingaben

Um die Treuebonusnummer des Benutzers bei der Registrierung abzurufen, müssen Sie dem Benutzer erlauben, diese Daten auf dem Bildschirm einzugeben. Fügen Sie den Ausgabeanspruch **loyaltyId** zur Anmeldeseite hinzu, indem Sie ihn dem Element `OutputClaims` des bestehenden technischen Registrierungsprofilabschnitts hinzufügen. Geben Sie die gesamte Liste von Ausgabeansprüchen an, um die Reihenfolge zu steuern, in der die Ansprüche auf dem Bildschirm angezeigt werden.  

Fügen Sie den Verweis auf das technische Überprüfungsprofil dem technischen Profil für die Registrierung hinzu, das `REST-ValidateProfile` aufruft. Das neue technische Überprüfungsprofil wird der in der Basisrichtlinie definierten Sammlung `<ValidationTechnicalProfiles>` oben hinzugefügt. Dieses Verhalten bedeutet, dass Azure AD B2C erst nach erfolgreicher Überprüfung mit der Erstellung des Kontos im Verzeichnis fortfahren kann.   

1. Suchen Sie nach dem Element **ClaimsProviders**. Fügen Sie einen neuen Anspruchsanbieter wie folgt hinzu:

    ```xml
    <ClaimsProvider>
      <DisplayName>Local Account</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true"/>
            <OutputClaim ClaimTypeReferenceId="newPassword" Required="true"/>
            <OutputClaim ClaimTypeReferenceId="reenterPassword" Required="true"/>
            <OutputClaim ClaimTypeReferenceId="displayName"/>
            <OutputClaim ClaimTypeReferenceId="givenName"/>
            <OutputClaim ClaimTypeReferenceId="surName"/>
            <!-- Required to present the text box to collect the data from the user -->
            <OutputClaim ClaimTypeReferenceId="loyaltyId"/>
            <!-- Required to pass the promoCode returned from "REST-ValidateProfile" 
            to subsequent orchestration steps and token issuance-->
            <OutputClaim ClaimTypeReferenceId="promoCode" />
          </OutputClaims>
          <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="REST-ValidateProfile" />
          </ValidationTechnicalProfiles>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    <ClaimsProvider>
      <DisplayName>Self Asserted</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="SelfAsserted-Social">
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="email" />
          </InputClaims>
            <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="email" />
            <OutputClaim ClaimTypeReferenceId="displayName"/>
            <OutputClaim ClaimTypeReferenceId="givenName"/>
            <OutputClaim ClaimTypeReferenceId="surname"/>
            <!-- Required to present the text box to collect the data from the user -->
            <OutputClaim ClaimTypeReferenceId="loyaltyId"/>
            <!-- Required to pass the promoCode returned from "REST-ValidateProfile" 
            to subsequent orchestration steps and token issuance-->
            <OutputClaim ClaimTypeReferenceId="promoCode" />
          </OutputClaims>
          <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="REST-ValidateProfile"/>
          </ValidationTechnicalProfiles>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

## <a name="include-a-claim-in-the-token"></a>Einschließen eines Anspruchs in das Token 

Um den Anspruch „Aktionscode“ an die Anwendung der vertrauenden Seite zurückzugeben, fügen Sie der Datei <em>`SocialAndLocalAccounts/`**`SignUpOrSignIn.xml`**</em> einen Ausgabeanspruch hinzu. Der Ausgabeanspruch ermöglicht es, den Anspruch nach einer erfolgreichen User Journey in das Token einzufügen, der dann an die Anwendung gesendet wird. Ändern Sie im Abschnitt für die vertrauende Seite das Element des technischen Profils, um `promoCode` als Ausgabeanspruch hinzuzufügen.
 
```xml
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <TechnicalProfile Id="PolicyProfile">
    <DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="OpenIdConnect" />
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="displayName" />
      <OutputClaim ClaimTypeReferenceId="givenName" />
      <OutputClaim ClaimTypeReferenceId="surname" />
      <OutputClaim ClaimTypeReferenceId="email" />
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      <OutputClaim ClaimTypeReferenceId="identityProvider" />
      <OutputClaim ClaimTypeReferenceId="tenantId" AlwaysUseDefaultValue="true" DefaultValue="{Policy:TenantObjectId}" />
      <OutputClaim ClaimTypeReferenceId="promoCode" DefaultValue="" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
  </TechnicalProfile>
</RelyingParty>
```

## <a name="test-the-custom-policy"></a>Testen der benutzerdefinierten Richtlinie

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD-Mandanten enthält, indem Sie im oberen Menü den **Verzeichnis- und Abonnementfilter** und dann das Verzeichnis auswählen, das Ihren Azure AD-Mandanten enthält.
1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **App-Registrierungen**, und wählen Sie dann diese Option aus.
1. Wählen Sie **Framework für die Identitätsfunktion** aus.
1. Wählen Sie **Benutzerdefinierte Richtlinie hochladen** aus, und laden Sie dann die geänderten Richtliniendateien hoch: *TrustFrameworkExtensions.xml*, und *SignUpOrSignin.xml*. 
1. Wählen Sie die hochgeladene Registrierungs- oder Anmelderichtlinie aus, und klicken Sie auf die Schaltfläche **Jetzt ausführen**.
1. Sie sollten sich mit einer E-Mail-Adresse registrieren können.
1. Klicken Sie auf den Link **Jetzt registrieren**.
1. Geben Sie 1234 in **Ihre Treuebonus-ID** ein, und klicken Sie auf **Weiter**. An dieser Stelle sollten Sie eine Prüfungsfehlermeldung erhalten.
1. Ändern Sie den Wert, und klicken Sie auf **Weiter**.
1. Das Token, das an die Anwendung zurückgesendet wird, enthält den Anspruch `promoCode`.

```json
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "X5eXk4xyojNFum1kl2Ytv8dlNP4-c57dO6QGTVBwaNk"
}.{
  "exp": 1584295703,
  "nbf": 1584292103,
  "ver": "1.0",
  "iss": "https://contoso.b2clogin.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "aud": "e1d2612f-c2bc-4599-8e7b-d874eaca1ee1",
  "acr": "b2c_1a_signup_signin",
  "nonce": "defaultNonce",
  "iat": 1584292103,
  "auth_time": 1584292103,
  "name": "Emily Smith",
  "email": "emily@outlook.com",
  "given_name": "Emily",
  "family_name": "Smith",
  "promoCode": "84362"
  ...
}
```

::: zone-end

## <a name="best-practices-and-how-to-troubleshoot"></a>Bewährte Methoden und Problembehandlungen

### <a name="using-serverless-cloud-functions"></a>Verwenden von serverlosen Cloudfunktionen

Serverlose Funktionen (z. B. HTTP-Trigger in Azure Functions) bieten eine Möglichkeit zum Erstellen von API-Endpunkten, die mit dem API-Connector verwendet werden. Sie können die serverlosen Cloudfunktionen [beispielsweise](code-samples.md#api-connectors) verwenden, um Validierungslogik auszuführen und Registrierungen auf bestimmte E-Mail-Domänen zu beschränken. Mit den serverlosen Cloudfunktionen können auch andere Web-APIs, Benutzerspeicher und weitere Clouddienste für komplexere Szenarien aufgerufen werden.

### <a name="best-practices"></a>Bewährte Methoden
Stellen Sie Folgendes sicher:
* Ihre API entspricht den API-Anforderungs- und -Antwortverträgen, wie dies weiter oben beschrieben ist. 
* Die **Endpunkt-URL** des API-Connectors verweist auf den richtigen API-Endpunkt.
* In Ihrer API wird explizit auf NULL-Werte der empfangenen Ansprüche geprüft.
* Ihre API antwortet so schnell wie möglich, um eine flüssige Darstellung für den Benutzer zu gewährleisten.
    * Wenn Sie eine serverlose Funktion oder einen skalierbaren Webdienst verwenden, nutzen Sie einen Hostingplan, mit dem die API im aktiven Zustand („wach“ oder „warm“) gehalten wird. Für Azure Functions wird die Verwendung des [Premium-Plans](../azure-functions/functions-scale.md) empfohlen.
 
### <a name="use-logging"></a>Verwenden von Protokollierung

Üblicherweise ist es nützlich, die von Ihrem Web-API-Dienst aktivierten Protokollierungstools, etwa [Application Insights](../azure-functions/functions-monitoring.md), zu verwenden, um Ihre API auf unerwartete Fehlercodes, Ausnahmen und schlechte Leistung zu überwachen.
* Überwachen Sie auf HTTP-Statuscodes, die weder HTTP 200 noch 400 sind.
* Der HTTP-Statuscode 401 oder 403 kennzeichnet in der Regel, dass ein Problem mit Ihrer Authentifizierung vorliegt. Überprüfen Sie die Authentifizierungsschicht Ihrer API und die entsprechende Konfiguration im API-Connector.
* Verwenden Sie in der Entwicklung ggf. höhere Protokolliergrade (z. B. „trace“ oder „debug“).
* Überwachen Sie Ihre API hinsichtlich langer Antwortzeiten.

## <a name="next-steps"></a>Nächste Schritte

::: zone pivot="b2c-user-flow"

- Erste Schritte mit den [Beispielen](code-samples.md#api-connectors)
- [Schützen Ihres API-Connectors](secure-rest-api.md)

::: zone-end

::: zone pivot="b2c-custom-policy"

- [Exemplarische Vorgehensweise: Integrieren von REST-API-Anspruchsaustausch-Vorgängen in Ihre Azure AD B2C User Journey als Orchestrierungsschritt](custom-policy-rest-api-claims-exchange.md)
- [Schützen Ihres API-Connectors](secure-rest-api.md)
- [Referenz: Technisches Profil „RESTful“](restful-technical-profile.md)

::: zone-end
