---
title: FHIR-Server in Azure mit Postman – Azure API for FHIR
description: In diesem Tutorial werden die Schritte erläutert, die zum Verwenden von Postman für den Zugriff auf einen fhir-Server erforderlich sind. Postman ist beim Debuggen von Anwendungen nützlich, die auf APIs zugreifen.
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: dseven
ms.author: matjazl
author: matjazl
ms.date: 03/26/2021
ms.openlocfilehash: 59847f745037acec47415489cdf61d119a7807af
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105936273"
---
# <a name="access-azure-api-for-fhir-with-postman"></a>Zugreifen auf Azure API for FHIR mit Postman

Eine Client Anwendung kann über eine [Rest-API](https://www.hl7.org/fhir/http.html)auf die Azure-API für die Azure-API zugreifen. Verwenden Sie zum Senden von Anforderungen, zum Anzeigen von Antworten und zum Debuggen Ihrer Anwendung während der Erstellung ein API-Testtool Ihrer Wahl. In diesem Tutorial werden Sie durch die Schritte zum Zugreifen auf den-Server mit [Postman](https://www.getpostman.com/)geführt. 

## <a name="prerequisites"></a>Voraussetzungen

- Ein FHIR-Endpunkt in Azure. 

  Um die Azure-API für fhir (einen verwalteten Dienst) bereitzustellen, können Sie die [Azure-Portal](fhir-paas-portal-quickstart.md), [PowerShell](fhir-paas-powershell-quickstart.md)oder [Azure CLI](fhir-paas-cli-quickstart.md)verwenden.

- Eine registrierte [vertrauliche Client Anwendung](register-confidential-azure-ad-client-app.md) für den Zugriff auf den fhir-Dienst.
- Sie haben der vertraulichen Client Anwendung Berechtigungen erteilt, z. b. "fhir-Daten Mitwirkender", um auf den fhir-Dienst zuzugreifen. Weitere Informationen finden Sie unter Konfigurieren der Azure-rollenbasierten Zugriffs Steuerung ( [RBAC](./configure-azure-rbac.md)).
- Postman ist installiert. 
    
  Weitere Informationen zu postman finden Sie unter [Get Started with Postman](https://www.getpostman.com).

## <a name="fhir-server-and-authentication-details"></a>FHIR-Server und Authentifizierungsdetails

Zum Verwenden von Postman sind die folgenden Authentifizierungs Parameter erforderlich:

- Ihre URL für den URL-Server, z. b. `https://MYACCOUNT.azurehealthcareapis.com`

- Der Identitätsanbieter `Authority` für Ihren FHIR-Server, z. B. `https://login.microsoftonline.com/{TENANT-ID}`.

- Der konfigurierte `audience` , der normalerweise die URL des-Datei-Servers ist, `https://<FHIR-SERVER-NAME>.azurehealthcareapis.com` z `https://azurehealthcareapis.com` . b. oder.

- Die- `client_id` oder-Anwendungs-ID der [vertraulichen Client Anwendung](register-confidential-azure-ad-client-app.md) , die für den Zugriff auf den-Dienst verwendet wird.

- Der `client_secret` geheime Anwendungs Schlüssel oder der geheime Anwendungs Schlüssel der vertraulichen Client Anwendung.

Zum Schluss sollten Sie überprüfen, ob `https://www.getpostman.com/oauth2/callback` eine registrierte Antwort-URL für Ihre Clientanwendung ist.

## <a name="connect-to-fhir-server"></a>Verbinden mit dem FHIR-Server

Öffnen Sie Postman, und wählen Sie dann **Get** aus, um eine Anforderung an zu senden `https://fhir-server-url/metadata` .

![Metadaten für Funktionsbestätigung in Postman](media/tutorial-postman/postman-metadata.png)

Die Metadaten-URL für Azure API for FHIR lautet `https://MYACCOUNT.azurehealthcareapis.com/metadata`. 

In diesem Beispiel ist die URL des Shir `https://fhirdocsmsft.azurewebsites.net` -Servers, und die Funktions Anweisung des Servers ist unter verfügbar `https://fhirdocsmsft.azurewebsites.net/metadata` . Auf diesen Endpunkt kann ohne Authentifizierung zugegriffen werden.

Wenn Sie versuchen, auf eingeschränkte Ressourcen zuzugreifen, wird die Antwort "Authentifizierung fehlgeschlagen" angezeigt.

![Fehler bei der Authentifizierung](media/tutorial-postman/postman-authentication-failed.png)

## <a name="obtaining-an-access-token"></a>Abrufen eines Zugriffstokens
Wählen Sie **Get New Access Token** (Neues Zugriffstoken abrufen) aus.

Um ein gültiges Zugriffs Token zu erhalten, wählen Sie **Autorisierung** aus, und wählen Sie im Dropdown Menü **Typ** den Befehl **OAuth 2,0** aus.

![Festlegen von OAuth 2.0](media/tutorial-postman/postman-select-oauth2.png)

Wählen Sie **Get New Access Token** (Neues Zugriffstoken abrufen) aus.

![Anfordern eines neuen Zugriffstokens](media/tutorial-postman/postman-request-token.png)

Geben Sie im Dialogfeld **Get New Access Token (neues Zugriffs Token abrufen** ) die folgenden Details ein:

| Feld                 | Beispielwert                                                                                                   | Kommentar                    |
|-----------------------|-----------------------------------------------------------------------------------------------------------------|----------------------------|
| Tokenname            | MYTOKEN                                                                                                         | Ein von Ihnen gewählter Name          |
| Gewährungstyp            | Autorisierungscode                                                                                              |                            |
| Rückruf-URL          | `https://www.getpostman.com/oauth2/callback`                                                                    |                            |
| Authentifizierungs-URL              | `https://login.microsoftonline.com/{TENANT-ID}/oauth2/authorize?resource=<audience>` | `audience` ist `https://MYACCOUNT.azurehealthcareapis.com` für Azure API for FHIR |
| Zugriffstoken-URL      | `https://login.microsoftonline.com/{TENANT ID}/oauth2/token`                                                    |                            |
| Client-ID             | `XXXXXXXX-XXX-XXXX-XXXX-XXXXXXXXXXXX`                                                                           | Anwendungs-ID             |
| Geheimer Clientschlüssel         | `XXXXXXXX`                                                                                                      | Geheimer Clientschlüssel          |
| Bereich | `<Leave Blank>` | Der Bereich wird nicht verwendet. Daher kann er leer gelassen werden.  
| State                 | `1234`     | Der [Status](https://learning.postman.com/docs/sending-requests/authorization/) ist ein nicht transparenter Wert, um eine Website übergreifende Anforderungs Fälschung zu verhindern. Er ist optional und kann einen beliebigen Wert, z. b. "1234", annehmen.                           |
| Clientauthentifizierung | Clientanmeldeinformationen im Text senden                                                                                 |                 

Wählen Sie das **Anforderungs Token** aus, das durch den Azure Active Directory Authentifizierungs Fluss geleitet werden soll, und an Postman wird ein Token zurückgegeben. Wenn ein Authentifizierungsfehler auftritt, finden Sie in der Postman-Konsole Weitere Informationen. **Hinweis**: Wählen Sie im Menüband die Option **Ansicht** aus, und wählen Sie dann die Option **Postman Console anzeigen** aus. Die Tastenkombination für die Postman-Konsole ist **ALT + STRG + C**.

Scrollen Sie nach unten, um den Bildschirm zurückgegebene Token anzuzeigen, und wählen Sie dann **Token verwenden** aus.

![Token verwenden](media/tutorial-postman/postman-use-token.png)

Lesen Sie das Feld **Zugriffs Token** , um das neu aufgefüllte Token anzuzeigen. Wenn Sie **senden** auswählen, um die `Patient` Ressourcen Suche zu wiederholen, wird ein **Status** `200 OK` zurückgegeben. Der zurückgegebene Status `200 OK` gibt eine erfolgreiche HTTP-Anforderung an.

![200 – OK](media/tutorial-postman/postman-200-OK.png)

Im Beispiel für die *Patienten Suche* sind keine Patienten in der Datenbank vorhanden, sodass das Suchergebnis leer ist.

Sie können das Zugriffs Token mithilfe eines Tools wie [JWT.ms](https://jwt.ms)untersuchen. Ein Beispiel für den Inhalt finden Sie unten.

```json
{
  "aud": "https://MYACCOUNT.azurehealthcareapis.com",
  "iss": "https://sts.windows.net/{TENANT-ID}/",
  "iat": 1545343803,
  "nbf": 1545343803,
  "exp": 1545347703,
  "acr": "1",
  "aio": "AUQAu/8JXXXXXXXXXdQxcxn1eis459j70Kf9DwcUjlKY3I2G/9aOnSbw==",
  "amr": [
    "pwd"
  ],
  "appid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "oid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "appidacr": "1",

  ...// Truncated
}
```

Bei der Problembehandlung empfiehlt es sich, zuerst zu überprüfen, ob Sie die richtige Zielgruppe (`aud`-Anspruch) haben. Wenn Ihr Token vom richtigen Aussteller ( `iss` Anspruch) ist und über die richtige Zielgruppe ( `aud` Anspruch) verfügt, Sie aber immer noch nicht auf die fhir-API zugreifen können, ist es wahrscheinlich, dass der Benutzer oder Dienst Prinzipal ( `oid` Anspruch) keinen Zugriff auf die fhir-Datenebene hat. Es wird empfohlen, die Rollen [basierte Zugriffs Steuerung von Azure (Azure RBAC)](configure-azure-rbac.md) zu verwenden, um Benutzern Daten auf Datenebene zuzuweisen. Wenn Sie einen externen, sekundären Azure Active Directory-Mandanten für Ihre Datenebene verwenden, müssen Sie die [lokale RBAC für fhir](configure-local-rbac.md) -Zuweisungen konfigurieren.

Es ist auch möglich, ein Token für die [Azure-API für fhir mithilfe des Azure CLI](get-healthcare-apis-access-token-cli.md)zu erhalten. Wenn Sie ein Token verwenden, das mit dem Azure CLI abgerufen wurde, sollten Sie das Autorisierungstyp *Träger Token* verwenden. Fügen Sie das Token direkt ein.

## <a name="inserting-a-patient"></a>Einfügen eines Patienten

Mit einem gültigen Zugriffs Token können Sie nun einen neuen Patienten einfügen. Ändern Sie in Postman die Methode, indem Sie **Post** auswählen, und fügen Sie dann das folgende JSON-Dokument in den Text der Anforderung ein.

[!code-json[](../samples/sample-patient.json)]

Wählen Sie **senden** , um zu bestimmen, ob der Patient erfolgreich erstellt wurde.

![Screenshot: Erfolgreiche Erstellung des Patienten](media/tutorial-postman/postman-patient-created.png)

Wenn Sie die Patienten Suche wiederholen, sollte nun der Patientendaten Satz angezeigt werden.

![Patient erstellt](media/tutorial-postman/postman-patient-found.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie über die Azure-API für die Verwendung von Postman auf die Azure-API zugegriffen. Weitere Informationen zur Azure-API für die Features von Azure-APIs finden Sie unter.
 
>[!div class="nextstepaction"]
>[Unterstützte Features](fhir-features-supported.md)
