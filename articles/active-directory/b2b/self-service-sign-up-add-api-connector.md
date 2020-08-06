---
title: Hinzufügen von API-Connectors zu Self-Service-Registrierungsflows – Azure AD
description: Konfigurieren Sie eine Web-API zur Verwendung in einem Benutzerflow.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 06/16/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: e6238e89b3941668f831f3128bb0e723a4097e48
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87027511"
---
# <a name="add-an-api-connector-to-a-user-flow"></a>Hinzufügen eines API-Connectors zu einem Benutzerflow

Um einen [API-Connector](api-connectors-overview.md) zu verwenden, erstellen Sie zunächst den API-Connector und aktivieren ihn anschließend in einem Benutzerflow.

## <a name="create-an-api-connector"></a>Erstellen eines API-Connectors

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als Azure AD-Administrator an.
2. Wählen Sie unter **Azure-Dienste** die Option **Azure Active Directory** aus.
3. Wählen Sie im Menü auf der linken Seite **Externe Identitäten** aus.
4. Wählen Sie **Alle API-Connectors (Vorschau)** und dann **Neuer API-Connector** aus.

   ![Hinzufügen eines neuen API-Connectors](./media/self-service-sign-up-add-api-connector/api-connector-new.png)

5. Geben Sie einen Anzeigenamen für den Aufruf an, z. B. **Genehmigungsstatus überprüfen**.
6. Geben Sie die **Endpunkt-URL** für den API-Aufruf an.
7. Geben Sie die Authentifizierungsinformationen für die API an.

   - Derzeit wird nur die Standardauthentifizierung unterstützt. Wenn Sie eine API ohne Standardauthentifizierung für Entwicklungszwecke verwenden möchten, geben Sie einfach Dummywerte für **Benutzername** und **Kennwort** ein, die in der API ignoriert werden können. Zur Verwendung mit einer Azure-Funktion mit einem API-Schlüssel können Sie den Code als Abfrageparameter in der **Endpunkt-URL** einfügen (z. B. https[]()://contoso.azurewebsites.net/api/endpoint<b>?code=0123456789</b>).

   ![Hinzufügen eines neuen API-Connectors](./media/self-service-sign-up-add-api-connector/api-connector-config.png)

8. Wählen Sie die Ansprüche aus, die an die API gesendet werden sollen.
9. Wählen Sie die Ansprüche aus, die die API zurücksenden soll.

   ![Festlegen der Ansprüche für den API-Connector](./media/self-service-sign-up-add-api-connector/api-connector-claims.png)

10. Wählen Sie **Speichern** aus.

## <a name="enable-the-api-connector-in-a-user-flow"></a>Aktivieren des API-Connectors in einem Benutzerflow

Führen Sie die folgenden Schritte aus, um einem Benutzerflow für die Self-Service-Registrierung einen API-Connector hinzuzufügen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als Azure AD-Administrator an.
2. Wählen Sie unter **Azure-Dienste** die Option **Azure Active Directory** aus.
3. Wählen Sie im Menü auf der linken Seite **Externe Identitäten** aus.
4. Wählen Sie **Benutzerflows (Vorschau)** und dann den Benutzerflow aus, dem Sie den API-Connector hinzufügen möchten.
5. Wählen Sie **API-Connectors** und dann die API-Endpunkte aus, die bei den folgenden Schritten im Benutzerflow aufgerufen werden sollen:

   - **Nach Anmeldung bei einem Identitätsanbieter**
   - **Vor dem Erstellen des Benutzers**

   ![Hinzufügen von APIs zum Benutzerflow](./media/self-service-sign-up-add-api-connector/api-connectors-user-flow-select.png)

6. Wählen Sie **Speichern** aus.

Informationen dazu, wann ein API-Connector in einem Benutzerflow aktiviert werden kann, erhalten Sie [hier](api-connectors-overview.md#where-you-can-enable-an-api-connector-in-a-user-flow).

## <a name="request-sent-to-the-api"></a>An die API gesendete Anforderung

Ein API-Connector wird als HTTP POST-Anforderung dargestellt und sendet ausgewählte Ansprüche als Schlüssel-Wert-Paare in einem JSON-Text. Die Antwort sollte auch den HTTP-Header `Content-Type: application/json` enthalten. Attribute werden ähnlich wie Microsoft Graph-Benutzerattribute serialisiert. <!--# TODO: Add link to MS Graph or create separate reference.-->

### <a name="example-request"></a>Beispielanforderung

```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [ //Sent for Google and Facebook identity providers
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "postalCode": "33971",
 "extension_<extensions-app-id>_CustomAttribute1": "custom attribute value",
 "extension_<extensions-app-id>_CustomAttribute2": "custom attribute value",
 "ui_locales":"en-US"
}
```

Der Anspruch **Gebietsschemas der Benutzeroberfläche („ui_locales“)** wird standardmäßig in allen Anforderungen gesendet. Er gibt die Gebietsschemas eines Benutzers an und kann von der API verwendet werden, um internationalisierte Antworten zurückzugeben. Er wird im API-Konfigurationsbereich nicht angezeigt.

Wenn ein zu sendender Anspruch zum Zeitpunkt des Aufrufs des API-Endpunkts keinen Wert enthält, wird er nicht an die API gesendet.

Für den Benutzer können benutzerdefinierte Attribute im Format **extension_\<extensions-app-id>_AttributeName** erstellt werden. Die API sollte den Empfang von Ansprüchen in diesem serialisierten Format erwarten. Die API kann Ansprüche mit oder ohne `<extensions-app-id>` zurückgeben. Weitere Informationen zu benutzerdefinierten Attributen finden Sie unter [Definieren von benutzerdefinierten Attributen für Self-Service-Registrierungsflows](user-flow-add-custom-attributes.md).

> [!TIP] 
> Mit den Ansprüchen [**Identitäten („identities“)** ](https://docs.microsoft.com/graph/api/resources/objectidentity?view=graph-rest-1.0) und **E-Mail-Adresse („email“)** können Sie einen Benutzer identifizieren, bevor er über ein Konto in Ihrem Mandanten verfügt. Der Anspruch „identities“ wird gesendet, wenn sich ein Benutzer mit einem Google- oder Facebook-Konto authentifiziert. Der Anspruch „email“ wird immer gesendet.

## <a name="expected-response-types-from-the-web-api"></a>Erwartete Antworttypen von der Web-API

Wenn die Web-API während eines Benutzerflows eine HTTP-Anforderung von Azure AD empfängt, kann sie folgende Antworten zurückgeben:

- [Fortsetzungsantwort](#continuation-response)
- [Blockierungsantwort](#blocking-response)
- [Validierungsfehlerantwort](#validation-error-response)

### <a name="continuation-response"></a>Fortsetzungsantwort

Eine Fortsetzungsantwort gibt an, dass der Benutzerflow mit dem nächsten Schritt fortgesetzt werden soll. In einer Fortsetzungsantwort kann die API Ansprüche zurückgeben.

Wenn ein Anspruch von der API zurückgegeben wird und als **zu empfangender Anspruch** ausgewählt ist, wird Folgendes durchgeführt:

- Die Eingabefelder in der Attributesammlungsseite werden vorab ausgefüllt, wenn der API-Connector vor der Anzeige der Seite aufgerufen wird.
- Alle Werte, die dem Anspruch bereits zugewiesen sind, werden überschrieben.
- Dem Wert wird ein Wert zugewiesen, wenn dieser zuvor NULL war.

#### <a name="example-of-a-continuation-response"></a>Beispiel für eine Fortsetzungsantwort

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
| version                                            | String            | Ja      | Die Version der API.                                                                                                                                                                                                                                                                |
| action                                             | String            | Ja      | Der Wert muss `Continue` sein.                                                                                                                                                                                                                                                              |
| \<builtInUserAttribute>                            | \<attribute-type> | Nein       | Werte können im Verzeichnis gespeichert werden, wenn sie als **zu empfangender Anspruch** in der API-Connector-Konfiguration und als **Benutzerattribute** für einen Benutzerflow ausgewählt sind. Werte können im Token zurückgegeben werden, wenn sie als **Anwendungsanspruch** ausgewählt sind.                                              |
| \<extension\_{extensions-app-id}\_CustomAttribute> | \<attribute-type> | Nein       | Der zurückgegebene Anspruch muss `_<extensions-app-id>_` nicht enthalten. Werte werden im Verzeichnis gespeichert, wenn sie als **zu empfangender Anspruch** in der API-Connector-Konfiguration und als **Benutzerattribute** für einen Benutzerflow ausgewählt sind. Benutzerdefinierte Attribute können im Token nicht zurückgesendet werden. |

### <a name="blocking-response"></a>Blockierungsantwort

Eine Blockierungsantwort beendet den Benutzerflow. Sie kann von der API absichtlich ausgegeben werden, um die Fortsetzung des Benutzerflows zu beenden, indem für den Benutzer eine Blockierungsseite angezeigt wird. Auf der Blockierungsseite wird die von der API angegebene `userMessage` angezeigt.

Beispiel für eine Blockierungsanwort:

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "ShowBlockPage",
    "userMessage": "There was a problem with your request. You are not able to sign up at this time.",
    "code": "CONTOSO-BLOCK-00"
}

```

| Parameter   | type   | Erforderlich | BESCHREIBUNG                                                                |
| ----------- | ------ | -------- | -------------------------------------------------------------------------- |
| version     | String | Ja      | Die Version der API.                                                    |
| action      | String | Ja      | Der Wert muss `ShowBlockPage` sein.                                              |
| userMessage | String | Ja      | Meldung, die für den Benutzer angezeigt wird.                                            |
| code        | String | Nein       | Fehlercode Kann zum Debuggen verwendet werden. Wird nicht für den Benutzer angezeigt. |

#### <a name="end-user-experience-with-a-blocking-response"></a>Anzeige einer Blockierungsantwort für den Endbenutzer

![Beispiel für eine Blockierungsseite](./media/api-connectors-overview/blocking-page-response.png)

### <a name="validation-error-response"></a>Validierungsfehlerantwort

Bei einem API-Aufruf, der nach einer Attributesammlungsseite aufgerufen wird, kann eine Validierungsfehlerantwort zurückgegeben werden. Wenn dies der Fall ist, verbleibt der Benutzerflow auf der Attributesammlungsseite. Für den Benutzer wird `userMessage` angezeigt. Der Benutzer kann dann das Formular bearbeiten und erneut senden. Diese Antwort kann für die Eingabevalidierung verwendet werden.

#### <a name="example-of-a-validation-error-response"></a>Beispiel für eine Validierungsfehlerantwort

```http
HTTP/1.1 400 Bad Request
Content-type: application/json

{
    "version": "1.0.0",
    "status": 400,
    "action": "ValidationError",
    "userMessage": "Please enter a valid Postal Code.",
    "code": "CONTOSO-VALIDATION-00"
}
```

| Parameter   | type    | Erforderlich | BESCHREIBUNG                                                                |
| ----------- | ------- | -------- | -------------------------------------------------------------------------- |
| version     | String  | Ja      | Die Version der API.                                                    |
| action      | String  | Ja      | Der Wert muss `ValidationError` sein.                                           |
| status      | Integer | Ja      | Für eine Validierungsfehlerantwort muss der Wert `400` sein.                        |
| userMessage | String  | Ja      | Meldung, die für den Benutzer angezeigt wird.                                            |
| code        | String  | Nein       | Fehlercode Kann zum Debuggen verwendet werden. Wird nicht für den Benutzer angezeigt. |

#### <a name="end-user-experience-with-a-validation-error-response"></a>Anzeige einer Validierungsfehlerantwort für den Endbenutzer

![Beispiel für eine Validierungsseite](./media/api-connectors-overview/validation-error-postal-code.png)

### <a name="integration-with-azure-functions"></a>Integration in Azure Functions
Sie können einen HTTP-Trigger in Azure Functions verwenden, um auf einfache Weise eine API zur Verwendung mit dem API-Connector zu erstellen. [Beispielsweise](code-samples-self-service-sign-up.md#api-connector-azure-function-quickstarts) können Sie die Azure-Funktion verwenden, um Validierungslogik auszuführen und Registrierungen auf bestimmte Domänen zu beschränken. Sie können auch andere Web-APIs, Benutzerspeicher und Clouddienste aufrufen.

## <a name="next-steps"></a>Nächste Schritte

<!-- - Learn [where you can enable an API connector](api-connectors-overview.md#where-you-can-enable-an-api-connector-in-a-user-flow) -->
- Erfahren Sie, wie Sie [der Self-Service-Registrierung einen benutzerdefinierten Genehmigungsworkflow hinzufügen](self-service-sign-up-add-approvals.md).
- Erste Schritte mit den [Azure Functions-Schnellstartbeispielen](code-samples-self-service-sign-up.md#api-connector-azure-function-quickstarts).
<!-- - Learn how to [use API connectors to verify a user identity](code-samples-self-service-sign-up.md#identity-verification) -->
