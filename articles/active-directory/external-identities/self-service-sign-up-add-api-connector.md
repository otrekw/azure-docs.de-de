---
title: Hinzufügen von API-Connectors zu Self-Service-Registrierungsflows – Azure AD
description: Konfigurieren Sie eine Web-API zur Verwendung in einem Benutzerflow.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: article
ms.date: 03/02/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0c9bbdb831df9c51c6d80e6c441ac7bdd2778428
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105044548"
---
# <a name="add-an-api-connector-to-a-user-flow"></a>Hinzufügen eines API-Connectors zu einem Benutzerflow

Um einen [API-Connector](api-connectors-overview.md) zu verwenden, erstellen Sie zunächst den API-Connector und aktivieren ihn anschließend in einem Benutzerflow.

> [!IMPORTANT]
>**Am 4. Januar 2021** wird Google [die Unterstützung für die WebView-Anmeldung einstellen](https://developers.googleblog.com/2020/08/guidance-for-our-effort-to-block-less-secure-browser-and-apps.html). Wenn Sie einen Google-Verbund oder die Self-Service-Registrierung mit Gmail verwenden, sollten Sie [Ihre nativen Branchenanwendungen auf Kompatibilität testen](google-federation.md#deprecation-of-webview-sign-in-support).

## <a name="create-an-api-connector"></a>Erstellen eines API-Connectors

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie unter **Azure-Dienste** die Option **Azure Active Directory** aus.
3. Wählen Sie im Menü auf der linken Seite **Externe Identitäten** aus.
4. Wählen Sie **Alle API-Connectors** und dann **Neuer API-Connector** aus.

   ![Hinzufügen eines neuen API-Connectors](./media/self-service-sign-up-add-api-connector/api-connector-new.png)

5. Geben Sie einen Anzeigenamen für den Aufruf an, z. B. **Genehmigungsstatus überprüfen**.
6. Geben Sie die **Endpunkt-URL** für den API-Aufruf an.
7. Wählen Sie den **Authentifizierungstyp** aus, und konfigurieren Sie die Authentifizierungsinformationen zum Aufrufen Ihrer API. Weitere Informationen zum Schützen Ihrer API finden Sie weiter unten in diesem Artikel.

    ![Konfigurieren eines API-Connectors](./media/self-service-sign-up-add-api-connector/api-connector-config.png)

8. Wählen Sie **Speichern** aus.

## <a name="securing-the-api-endpoint"></a>Schützen des API-Endpunkts
Sie können Ihren API-Endpunkt entweder mithilfe der HTTP-Standardauthentifizierung oder mithilfe der HTTPS-Clientzertifikatauthentifizierung (Vorschau) schützen. In beiden Fällen müssen Sie die Anmeldeinformationen angeben, die Azure Active Directory beim Aufrufen Ihres API-Endpunkts verwenden wird. Der API-Endpunkt überprüft dann die Anmeldeinformationen und führt Autorisierungsentscheidungen aus.

### <a name="http-basic-authentication"></a>HTTP-Standardauthentifizierung
Die HTTP-Standardauthentifizierung ist in [RFC 2617](https://tools.ietf.org/html/rfc2617) definiert. Azure Active Directory sendet eine HTTP-Anforderung mit den Clientanmeldeinformationen (`username` und `password`) im Header `Authorization`. Die Anmeldeinformationen werden als Base64-codierte Zeichenfolge im Format `username:password` angegeben. Die API überprüft dann diese Werte, um zu bestimmen, ob ein API-Aufruf abgelehnt werden soll oder nicht.

### <a name="https-client-certificate-authentication-preview"></a>HTTPS-Clientzertifikatauthentifizierung (Vorschau)

> [!IMPORTANT]
> Diese Funktion befindet sich in der Vorschauhase und wird ohne Vereinbarung zum Servicelevel bereitgestellt. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bei der Client-Zertifikat-Authentifizierung handelt es sich um eine auf gegenseitigen Zertifikaten basierende Authentifizierungsmethode, bei welcher der Client dem Server ein Client-Zertifikat zur Verfügung stellt, um seine Identität nachzuweisen. In diesem Fall verwendet Azure Active Directory das Zertifikat, das Sie als Teil der API-Connectorkonfiguration hochladen. Dies erfolgt im Rahmen des SSL-Handshakes. Ihr API-Dienst kann dann den Zugriff nur auf Dienste beschränken, die über ordnungsgemäße Zertifikate verfügen. Beim Client-Zertifikat handelt es sich um ein digitales PKCS12 (PFX) X.509-Zertifikat. In Produktionsumgebungen muss es von einer Zertifizierungsstelle signiert werden. 

Zum Erstellen eines Zertifikats können Sie den [Azure Key Vault](../../key-vault/certificates/create-certificate.md) verwenden, der über Optionen für selbstsignierte Zertifikate und Integrationen mit Zertifikatausstelleranbietern für signierte Zertifikate verfügt. Empfohlene Einstellungen umfassen:
- **Betreff**: `CN=<yourapiname>.<tenantname>.onmicrosoft.com`
- **Inhaltstyp**: `PKCS #12`
- **Gültigkeitsdauer Aktionstyp**: `Email all contacts at a given percentage lifetime` oder `Email all contacts a given number of days before expiry`
- **Schlüsseltyp**: `RSA`
- **Schlüsselgröße**: `2048`
- **Exportierbarer privater Schlüssel**: `Yes` (um die pfx-Datei exportieren zu können)

Sie können dann [das Zertifikat exportieren](../../key-vault/certificates/how-to-export-certificate.md). Sie können alternativ das Cmdlet [New-SelfSignedCertificate](../../active-directory-b2c/secure-rest-api.md#prepare-a-self-signed-certificate-optional) von PowerShell verwenden, um ein selbstsigniertes Zertifikat zu generieren.

Nachdem Sie ein Zertifikat haben, können Sie es als Teil der Konfiguration des API-Connectors hochladen. Hinweis: Das Kennwort ist nur für kennwortgeschützte Zertifikatdateien erforderlich.

Ihre API muss die Autorisierung basierend auf gesendeten Client-Zertifikaten implementieren, um die API-Endpunkte zu schützen. Für den Azure App Service und die Azure Funktionen finden Sie unter [Konfigurieren der gegenseitigen TLS-Authentifizierung](../../app-service/app-service-web-configure-tls-mutual-auth.md) Informationen zum Aktivieren und *Validieren des Zertifikats über Ihren API-Code*.  Sie können auch das Azure API Management verwenden, um die [Eigenschaften](
../../api-management/api-management-howto-mutual-certificates-for-clients.md) von Client-Zertifikaten mithilfe von Richtlinienbegriffen auf die gewünschten Werte zu prüfen.
 
Es wird empfohlen, Erinnerungswarnungen für den Zeitpunkt festzulegen, an dem das Zertifikat abläuft. Sie müssen ein neues Zertifikat erstellen und die obigen Schritte wiederholen. Ihr API-Dienst kann vorübergehend weiterhin alte und neue Zertifikate akzeptieren, während das neue Zertifikat bereitgestellt wird. Wenn Sie ein neues Zertifikat in einen vorhandenen API-Connector hochladen möchten, wählen Sie diesen API-Connector unter **All API connectors** (Alle API-Connectors) aus, und klicken Sie auf **Upload new certificate** (Neues Zertifikat hochladen). Das zuletzt hochgeladene Zertifikat, das noch nicht abgelaufen ist und das Startdatum überschritten hat, wird automatisch vom Azure Active Directory verwendet.

### <a name="api-key"></a>API-Schlüssel
Einige Dienste verwenden einen API-Schlüssel-Mechanismus, um den Zugriff auf die HTTP-Endpunkte während der Entwicklung zu verschleiern. Sie können diesen für [Azure Functions](../../azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys) einrichten, indem Sie `code` als Abfrageparameter in die **Endpunkt-URL** einschließen. Beispiel: `https://contoso.azurewebsites.net/api/endpoint`<b>`?code=0123456789`</b>. 

Hierbei handelt es sich nicht um einen Mechanismus, der allein in der Produktion verwendet werden sollte. Daher ist die Konfiguration für die Standard- oder Zertifikatauthentifizierung immer erforderlich. Wenn Sie für Entwicklungszwecke keine Authentifizierungsmethode implementieren möchten (nicht empfohlen), können Sie die Standardauthentifizierung auswählen und temporäre Werte für `username` und `password` verwenden, die von der API ignoriert werden können, während Sie die Autorisierung in Ihrer API implementieren.

## <a name="the-request-sent-to-your-api"></a>An die API gesendete Anforderung
Ein API-Connector wird als **HTTP POST**-Anforderung dargestellt und sendet Benutzerattribute („Ansprüche“) als Schlüssel-Wert-Paare in einem JSON-Text. Attribute werden ähnlich wie [Microsoft Graph](/graph/api/resources/user#properties)-Benutzereigenschaften serialisiert. 

**Beispielanforderung**
```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [ // Sent for Google, Facebook, and Email One Time Passcode identity providers 
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

Nur Benutzereigenschaften und benutzerdefinierte Attribute, die unter **Azure Active Directory** > **Externe Identitäten** > **Benutzerdefinierte Benutzerattribute** aufgeführt sind, können in der Anforderung gesendet werden.

Benutzerdefinierte Attribute sind im Verzeichnis im Format **extension_\<extensions-app-id>_AttributName** vorhanden. Die API sollte den Empfang von Ansprüchen in diesem serialisierten Format erwarten. Weitere Informationen zu benutzerdefinierten Attributen finden Sie unter [Definieren von benutzerdefinierten Attributen für Self-Service-Registrierungsflows](user-flow-add-custom-attributes.md).

Außerdem wird der Anspruch **Gebietsschema der Benutzeroberfläche („ui_locales“)** standardmäßig in allen Anforderungen gesendet. Er gibt das Gebietsschema (oder auch mehrere Gebietsschemas) eines Benutzers an, wie es auf seinem Gerät konfiguriert ist. Dieses kann von der API verwendet werden, um internationalisierte Antworten zurückzugeben.

> [!IMPORTANT]
> Wenn ein Anspruch zum Zeitpunkt des Aufrufs des API-Endpunkts keinen Wert enthält, wird der Anspruch nicht an die API gesendet. Ihre API sollte so entworfen sein, dass explizit geprüft wird, ob ein Anspruch in der Anforderung enthalten ist. Fehlt der Anspruch, sollte eine entsprechende Verarbeitung erfolgen.

> [!TIP] 
> Mit den Ansprüchen [**Identitäten („identities“)**](/graph/api/resources/objectidentity) und **E-Mail-Adresse („email“)** kann Ihre API einen Benutzer identifizieren, bevor er über ein Konto in Ihrem Mandanten verfügt. Der Anspruch „identities“ wird gesendet, wenn sich ein Benutzer mit einem Identitätsanbieter (z. B. Google oder Facebook) authentifiziert. Der Anspruch „email“ wird immer gesendet.

## <a name="enable-the-api-connector-in-a-user-flow"></a>Aktivieren des API-Connectors in einem Benutzerflow

Führen Sie die folgenden Schritte aus, um einem Benutzerflow für die Self-Service-Registrierung einen API-Connector hinzuzufügen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als Azure AD-Administrator an.
2. Wählen Sie unter **Azure-Dienste** die Option **Azure Active Directory** aus.
3. Wählen Sie im Menü auf der linken Seite **Externe Identitäten** aus.
4. Wählen Sie **Benutzerflows** und dann den Benutzerflow aus, dem Sie den API-Connector hinzufügen möchten.
5. Wählen Sie **API-Connectors** und dann die API-Endpunkte aus, die bei den folgenden Schritten im Benutzerflow aufgerufen werden sollen:

   - **Nach Anmeldung bei einem Identitätsanbieter**
   - **Vor dem Erstellen des Benutzers**

   ![Hinzufügen von APIs zum Benutzerflow](./media/self-service-sign-up-add-api-connector/api-connectors-user-flow-select.png)

6. Wählen Sie **Speichern** aus.

## <a name="after-signing-in-with-an-identity-provider"></a>Nach Anmeldung bei einem Identitätsanbieter

Ein API-Connector in diesem Schritt des Registrierungsprozesses wird unmittelbar nach der Authentifizierung des Benutzers bei einem Identitätsanbieter (etwa Google, Facebook und Azure AD) aufgerufen. Dieser Schritt geht der ***Seite zur Attributsammlung***  voraus, die dem Benutzer zum Sammeln von Benutzerattributen angezeigt wird. Dieser Schritt wird nicht aufgerufen, wenn sich ein Benutzer mit einem lokalen Konto registriert.

### <a name="example-request-sent-to-the-api-at-this-step"></a>In diesem Schritt an die API gesendete Beispielanforderung
```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [ // Sent for Google, Facebook, and Email One Time Passcode identity providers 
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

Die genauen Ansprüche, die an die API gesendet werden, hängen von den Informationen ab, die vom Identitätsanbieter bereitgestellt werden. Der Anspruch „email“ wird immer gesendet.

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

Ein API-Connector in diesem Schritt des Registrierungsprozesses wird nach der Seite zur Attributsammlung aufgerufen, sofern vorhanden. Dieser Schritt wird stets aufgerufen, ehe ein Benutzerkonto in Azure AD erstellt wird. 

### <a name="example-request-sent-to-the-api-at-this-step"></a>In diesem Schritt an die API gesendete Beispielanforderung

```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [ // Sent for Google, Facebook, and Email One Time Passcode identity providers 
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
Die genauen Ansprüche, die an die API gesendet werden, hängen von den Informationen ab, die vom Benutzer gesammelt oder vom Identitätsanbieter bereitgestellt werden.

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

| Parameter                                          | Typ              | Erforderlich | BESCHREIBUNG                                                                                                                                                                                                                                                                            |
| -------------------------------------------------- | ----------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| version                                            | String            | Ja      | Die Version der API.                                                                                                                                                                                                                                                                |
| action                                             | String            | Ja      | Der Wert muss `Continue` sein.                                                                                                                                                                                                                                                              |
| \<builtInUserAttribute>                            | \<attribute-type> | Nein       | Werte können im Verzeichnis gespeichert werden, wenn sie als **zu empfangender Anspruch** in der API-Connector-Konfiguration und als **Benutzerattribute** für einen Benutzerflow ausgewählt sind. Werte können im Token zurückgegeben werden, wenn sie als **Anwendungsanspruch** ausgewählt sind.                                              |
| \<extension\_{extensions-app-id}\_CustomAttribute> | \<attribute-type> | Nein       | Der zurückgegebene Anspruch muss `_<extensions-app-id>_` nicht enthalten. Zurückgegebene Werte können Werte überschreiben, die von einem Benutzer gesammelt wurden. Sie können auch im Token zurückgegeben werden, wenn dieses als Teil der Anwendung konfiguriert ist.  |

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

| Parameter   | Typ   | Erforderlich | BESCHREIBUNG                                                                |
| ----------- | ------ | -------- | -------------------------------------------------------------------------- |
| version     | String | Ja      | Die Version der API.                                                    |
| action      | String | Ja      | Der Wert muss `ShowBlockPage` sein.                                              |
| userMessage | String | Ja      | Meldung, die für den Benutzer angezeigt wird.                                            |

**Anzeige einer Blockierungsantwort für den Endbenutzer**

![Beispiel für eine Blockierungsseite](./media/api-connectors-overview/blocking-page-response.png)

### <a name="example-of-a-validation-error-response"></a>Beispiel für eine Validierungsfehlerantwort

```http
HTTP/1.1 400 Bad Request
Content-type: application/json

{
    "version": "1.0.0",
    "status": 400,
    "action": "ValidationError",
    "userMessage": "Please enter a valid Postal Code.",
}
```

| Parameter   | Typ    | Erforderlich | BESCHREIBUNG                                                                |
| ----------- | ------- | -------- | -------------------------------------------------------------------------- |
| version     | String  | Ja      | Die Version Ihrer API                                                    |
| action      | String  | Ja      | Der Wert muss `ValidationError` sein.                                           |
| status      | Integer | Ja      | Für eine Validierungsfehlerantwort muss der Wert `400` sein.                        |
| userMessage | String  | Ja      | Meldung, die für den Benutzer angezeigt wird.                                            |

> [!NOTE]
> Der HTTP-Statuscode muss „400“ lauten, und der Antworttext muss den Wert „status“ enthalten.

**Anzeige einer Validierungsfehlerantwort für den Endbenutzer**

![Beispiel für eine Validierungsseite](./media/api-connectors-overview/validation-error-postal-code.png)


## <a name="best-practices-and-how-to-troubleshoot"></a>Bewährte Methoden und Problembehandlungen

### <a name="using-serverless-cloud-functions"></a>Verwenden von serverlosen Cloudfunktionen
Serverlose Funktionen, z. B. HTTP-Trigger in Azure Functions, bieten eine einfache Möglichkeit zum Erstellen von API-Endpunkten, die mit dem API-Connector verwendet werden. Sie können die serverlosen Cloudfunktionen [beispielsweise](code-samples-self-service-sign-up.md#api-connector-azure-function-quickstarts) verwenden, um Validierungslogik auszuführen und Registrierungen auf bestimmte E-Mail-Domänen zu beschränken. Mit den serverlosen Cloudfunktionen können auch andere Web-APIs, Benutzerspeicher und weitere Clouddienste für komplexere Szenarien aufgerufen werden.

### <a name="best-practices"></a>Bewährte Methoden
Stellen Sie Folgendes sicher:
* Ihre API entspricht den API-Anforderungs- und -Antwortverträgen, wie dies weiter oben beschrieben ist. 
* Die **Endpunkt-URL** des API-Connectors verweist auf den richtigen API-Endpunkt.
* In Ihrer API wird explizit auf NULL-Werte der empfangenen Ansprüche geprüft.
* Ihre API antwortet so schnell wie möglich, um eine flüssige Darstellung für den Benutzer zu gewährleisten.
    * Wenn Sie eine serverlose Funktion oder einen skalierbaren Webdienst verwenden, nutzen Sie einen Hostingplan, mit dem die API im aktiven Zustand („wach“ oder „warm“) gehalten wird. Für Azure Functions wird empfohlen, den [Premium-Plan](../../azure-functions/functions-scale.md) zu verwenden.

### <a name="use-logging"></a>Verwenden von Protokollierung
Üblicherweise ist es nützlich, die von Ihrem Web-API-Dienst aktivierten Protokollierungstools, etwa [Application Insights](../../azure-functions/functions-monitoring.md), zu verwenden, um Ihre API auf unerwartete Fehlercodes, Ausnahmen und schlechte Leistung zu überwachen.
* Überwachen Sie auf HTTP-Statuscodes, die weder HTTP 200 noch 400 sind.
* Der HTTP-Statuscode 401 oder 403 kennzeichnet in der Regel, dass ein Problem mit Ihrer Authentifizierung vorliegt. Überprüfen Sie die Authentifizierungsschicht Ihrer API und die entsprechende Konfiguration im API-Connector.
* Verwenden Sie in der Entwicklung ggf. eine aggressivere Protokollierungsebene (z. B. „trace“ oder „debug“).
* Überwachen Sie Ihre API hinsichtlich langer Antwortzeiten.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie [der Self-Service-Registrierung einen benutzerdefinierten Genehmigungsworkflow hinzufügen](self-service-sign-up-add-approvals.md).
- Verwenden Sie unsere [Schnellstartbeispiele](code-samples-self-service-sign-up.md#api-connector-azure-function-quickstarts), um direkt loszulegen.