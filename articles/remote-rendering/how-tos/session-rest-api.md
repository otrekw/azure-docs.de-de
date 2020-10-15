---
title: Die REST-API für die Sitzungsverwaltung
description: Beschreibt die Verwaltung von Sitzungen
author: florianborn71
ms.author: flborn
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: 0af9d6906e038a4b9285a2c302fc0c98345fdbd9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90023753"
---
# <a name="use-the-session-management-rest-api"></a>Verwenden der REST-API für die Sitzungsverwaltung

Um die Azure Remote Rendering-Funktion zu verwenden, müssen Sie eine *Sitzung* erstellen. Jede Sitzung entspricht der Zuordnung eines virtuellen Computers (VM) in Azure und dem Warten auf das Herstellen einer Verbindung durch ein Clientgerät. Wenn ein Gerät eine Verbindung herstellt, rendert die VM die angeforderten Daten und stellt das Ergebnis als Videostream bereit. Bei der Erstellung der Sitzung haben Sie ausgewählt, auf welcher Art von Server die Ausführung stattfinden soll, und somit die Preise festgelegt. Wenn die Sitzung nicht mehr benötigt wird, sollte sie beendet werden. Falls sie nicht manuell angehalten wird, wird sie automatisch heruntergefahren, wenn die *Leasedauer der Sitzung* abläuft.

Wir stellen ein PowerShell-Skript im [ARR-Beispielrepository](https://github.com/Azure/azure-remote-rendering) im Ordner *Scripts* bereit, das den Namen *RenderingSession.ps1* trägt. Dieses Skript veranschaulicht die Verwendung unseres Diensts. Das Skript und seine Konfiguration werden hier beschrieben: [PowerShell-Beispielskripts](../samples/powershell-example-scripts.md)

> [!TIP]
> Die auf dieser Seite aufgeführten PowerShell-Befehle sollen sich gegenseitig ergänzen. Wenn Sie alle Skripts in derselben Reihenfolge an derselben PowerShell-Eingabeaufforderung ausführen, bauen sie aufeinander auf.

## <a name="regions"></a>Regions

Informationen zu den Basis-URLs, an die Anforderungen gesendet werden, finden Sie in der [Liste der verfügbaren Regionen](../reference/regions.md).

Für die nachstehenden Beispielskripts haben wir die Region *westus2* ausgewählt.

### <a name="example-script-choose-an-endpoint"></a>Beispielskript: Auswählen eines Endpunkts

```PowerShell
$endPoint = "https://remoterendering.westus2.mixedreality.azure.com"
```

## <a name="accounts"></a>Konten

Wenn Sie nicht über ein Remote Rendering-Konto verfügen, [erstellen Sie eines](create-an-account.md). Jede Ressource wird durch eine *accountId* identifiziert, die in allen Sitzungs-APIs verwendet wird.

### <a name="example-script-set-accountid-and-accountkey"></a>Beispielskript: Festlegen von accountId und accountKey

```PowerShell
$accountId = "********-****-****-****-************"
$accountKey = "*******************************************="
```

## <a name="common-request-headers"></a>Allgemeine Anforderungsheader

* Der Header *Authorization* muss den Wert `Bearer TOKEN` aufweisen, wobei `TOKEN` das Authentifizierungstoken ist, das [vom Sicherheitstokendienst zurückgegeben wird](tokens.md).

### <a name="example-script-request-a-token"></a>Beispielskript: Anfordern eines Tokens

```PowerShell
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12
$webResponse = Invoke-WebRequest -Uri "https://sts.mixedreality.azure.com/accounts/$accountId/token" -Method Get -ContentType "application/json" -Headers @{ Authorization = "Bearer ${accountId}:$accountKey" }
$response = ConvertFrom-Json -InputObject $webResponse.Content
$token = $response.AccessToken;
```

## <a name="common-response-headers"></a>Allgemeine Antwortheader

* Der Header *MS-CV* kann vom Produktteam verwendet werden, um den Aufruf im Dienst nachzuverfolgen.

## <a name="create-a-session"></a>Erstellen einer Sitzung

Dieser Befehl erstellt eine Sitzung. Er gibt die ID der neuen Sitzung zurück. Sie benötigen die Sitzungs-ID für alle anderen Befehle.

| URI | Methode |
|-----------|:-----------|
| /v1/accounts/*accountId*/sessions/create | POST |

**Anforderungstext:**

* maxLeaseTime (Zeitspanne): ein Timeoutwert, nach dem die Sitzung automatisch außer Betrieb gesetzt wird
* models (Array): Ressourcencontainer-URLs, die vorab geladen werden sollen
* size (Zeichenfolge): die zu konfigurierende Servergröße ([ **„Standard“** ](../reference/vm-sizes.md) oder [ **„Premium“** ](../reference/vm-sizes.md)). Weitere Informationen finden Sie bei den spezifischen [Größenbeschränkungen](../reference/limits.md#overall-number-of-polygons).

**Antworten:**

| Statuscode | JSON-Nutzlast | Kommentare |
|-----------|:-----------|:-----------|
| 202 | - sessionId: GUID | Erfolg |

### <a name="example-script-create-a-session"></a>Beispielskript: Erstellen einer Sitzung

```PowerShell
Invoke-WebRequest -Uri "$endPoint/v1/accounts/$accountId/sessions/create" -Method Post -ContentType "application/json" -Body "{ 'maxLeaseTime': '4:0:0', 'models': [], 'size': 'standard' }" -Headers @{ Authorization = "Bearer $token" }
```

Beispielausgabe:

```PowerShell
StatusCode        : 202
StatusDescription : Accepted
Content           : {"sessionId":"d31bddca-dab7-498e-9bc9-7594bc12862f"}
RawContent        : HTTP/1.1 202 Accepted
                    MS-CV: 5EqPJ1VdTUakDJZc6/ZhTg.0
                    Content-Length: 52
                    Content-Type: application/json; charset=utf-8
                    Date: Thu, 09 May 2019 16:17:50 GMT
                    Location: accounts/11111111-1111-1111-11...
Forms             : {}
Headers           : {[MS-CV, 5EqPJ1VdTUakDJZc6/ZhTg.0], [Content-Length, 52], [Content-Type, application/json;
                    charset=utf-8], [Date, Thu, 09 May 2019 16:17:50 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 52
```

### <a name="example-script-store-sessionid"></a>Beispielskript: Speichern der sessionId

Die Antwort der obigen Anforderung enthält eine **sessionId**, die Sie für alle folgenden Anforderungen benötigen.

```PowerShell
$sessionId = "d31bddca-dab7-498e-9bc9-7594bc12862f"
```

## <a name="modify-and-query-session-properties"></a>Ändern und Abfragen von Sitzungseigenschaften

Mit einigen Befehlen können die Parameter vorhandener Sitzungen abgefragt oder geändert werden.

> [!CAUTION]
> Wie bei allen REST-Aufrufen bewirkt das zu häufige Senden dieser Befehle, dass der Server eine Drosselung durchführt und schließlich einen Fehler zurückgibt. Der Statuscode lautet in diesem Fall 429 („Zu viele Anforderungen“). Als Faustregel sollte eine Verzögerung von **5–10 Sekunden zwischen nachfolgenden Aufrufen** erfolgen.

### <a name="update-session-parameters"></a>Aktualisieren der Sitzungsparameter

Mit diesem Befehl werden die Parameter einer Sitzung aktualisiert. Derzeit können Sie nur die Leasedauer einer Sitzung verlängern.

> [!IMPORTANT]
> Die Leasedauer wird immer als Gesamtzeit seit dem Beginn der Sitzung angegeben. Wenn Sie eine Sitzung mit einer Leasedauer von einer Stunde erstellt haben und diese um eine weitere Stunde verlängern möchten, müssen Sie daher maxLeaseTime auf zwei Stunden aktualisieren.

| URI | Methode |
|-----------|:-----------|
| /v1/accounts/*accountID*/sessions/*sessionId* | PATCH |

**Anforderungstext:**

* maxLeaseTime (Zeitspanne): ein Timeoutwert, nach dem die Sitzung automatisch außer Betrieb gesetzt wird

**Antworten:**

| Statuscode | JSON-Nutzlast | Kommentare |
|-----------|:-----------|:-----------|
| 200 | | Erfolg |

#### <a name="example-script-update-a-session"></a>Beispielskript: Aktualisieren einer Sitzung

```PowerShell
Invoke-WebRequest -Uri "$endPoint/v1/accounts/$accountId/sessions/$sessionId" -Method Patch -ContentType "application/json" -Body "{ 'maxLeaseTime': '5:0:0' }" -Headers @{ Authorization = "Bearer $token" }
```

Beispielausgabe:

```PowerShell
StatusCode        : 200
StatusDescription : OK
Content           : {}
RawContent        : HTTP/1.1 200 OK
                    MS-CV: Fe+yXCJumky82wuoedzDTA.0
                    Content-Length: 0
                    Date: Thu, 09 May 2019 16:27:31 GMT


Headers           : {[MS-CV, Fe+yXCJumky82wuoedzDTA.0], [Content-Length, 0], [Date, Thu, 09 May 2019 16:27:31 GMT]}
RawContentLength  : 0
```

### <a name="get-active-sessions"></a>Abrufen aktiver Sitzungen

Dieser Befehl gibt eine Liste von aktiven Sitzungen zurück.

| URI | Methode |
|-----------|:-----------|
| /v1/accounts/*accountId*/sessions | GET |

**Antworten:**

| Statuscode | JSON-Nutzlast | Kommentare |
|-----------|:-----------|:-----------|
| 200 | - sessions: Array von Sitzungseigenschaften | Eine Beschreibung der Sitzungseigenschaften finden Sie unter „Abrufen von Sitzungseigenschaften“. |

#### <a name="example-script-query-active-sessions"></a>Beispielskript: Abfragen aktiver Sitzungen

```PowerShell
Invoke-WebRequest -Uri "$endPoint/v1/accounts/$accountId/sessions" -Method Get -Headers @{ Authorization = "Bearer $token" }
```

Beispielausgabe:

```PowerShell
StatusCode        : 200
StatusDescription : OK
Content           : []
RawContent        : HTTP/1.1 200 OK
                    MS-CV: WfB9Cs5YeE6S28qYkp7Bhw.1
                    Content-Length: 15
                    Content-Type: application/json; charset=utf-8
                    Date: Thu, 25 Jul 2019 16:23:50 GMT

                    {"sessions":[]}
Forms             : {}
Headers           : {[MS-CV, WfB9Cs5YeE6S28qYkp7Bhw.1], [Content-Length, 2], [Content-Type, application/json;
                    charset=utf-8], [Date, Thu, 25 Jul 2019 16:23:50 GMT]}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 2
```

### <a name="get-sessions-properties"></a>Abrufen von Sitzungseigenschaften

Dieser Befehl gibt Informationen zu einer Sitzung zurück, z. B. den VM-Hostnamen.

| URI | Methode |
|-----------|:-----------|
| /v1/accounts/*accountId*/sessions/*sessionId*/properties | GET |

**Antworten:**

| Statuscode | JSON-Nutzlast | Kommentare |
|-----------|:-----------|:-----------|
| 200 | - message: Zeichenfolge<br/>- sessionElapsedTime: Zeitspanne<br/>- sessionHostname: Zeichenfolge<br/>- sessionId: Zeichenfolge<br/>- sessionMaxLeaseTime: Zeitspanne<br/>- sessionSize: Enumeration<br/>- sessionStatus: Enumeration | enum sessionStatus { starting, ready, stopping, stopped, expired, error}<br/>Wenn der Status „error“ oder „expired“ lautet, enthält die Nachricht weitere Informationen. |

#### <a name="example-script-get-session-properties"></a>Beispielskript: Ruft Sitzungseigenschaften ab.

```PowerShell
Invoke-WebRequest -Uri "$endPoint/v1/accounts/$accountId/sessions/$sessionId/properties" -Method Get -Headers @{ Authorization = "Bearer $token" }
```

Beispielausgabe:

```PowerShell
StatusCode        : 200
StatusDescription : OK
Content           : {"message":null,"sessionElapsedTime":"00:00:01","sessionHostname":"5018fee8-817e-4366-9179-556af79a4240.remoterenderingvm.westeurope.mixedreality.azure.com","sessionId":"e13d2c44-63e0-4591-991e-f9e05e599a93","sessionMaxLeaseTime":"04:00:00","sessionStatus":"Ready"}
RawContent        : HTTP/1.1 200 OK
                    MS-CV: CMXegpZRMECH4pbOW2j5GA.0
                    Content-Length: 60
                    Content-Type: application/json; charset=utf-8
                    Date: Thu, 09 May 2019 16:30:38 GMT

                    {"message":null,...
Forms             : {}
Headers           : {[MS-CV, CMXegpZRMECH4pbOW2j5GA.0], [Content-Length, 60], [Content-Type, application/json;
                    charset=utf-8], [Date, Thu, 09 May 2019 16:30:38 GMT]}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 60
```

## <a name="stop-a-session"></a>Beenden einer Sitzung

Dieser Befehl beendet eine Sitzung. Die zugewiesene VM wird kurz danach freigegeben.

| URI | Methode |
|-----------|:-----------|
| /v1/accounts/*accountId*/sessions/*sessionId* | Delete |

**Antworten:**

| Statuscode | JSON-Nutzlast | Kommentare |
|-----------|:-----------|:-----------|
| 204 | | Erfolg |

### <a name="example-script-stop-a-session"></a>Beispielskript: Beenden einer Sitzung

```PowerShell
Invoke-WebRequest -Uri "$endPoint/v1/accounts/$accountId/sessions/$sessionId" -Method Delete -Headers @{ Authorization = "Bearer $token" }
```

Beispielausgabe:

```PowerShell
StatusCode        : 204
StatusDescription : No Content
Content           : {}
RawContent        : HTTP/1.1 204 No Content
                    MS-CV: YDxR5/7+K0KstH54WG443w.0
                    Date: Thu, 09 May 2019 16:45:41 GMT


Headers           : {[MS-CV, YDxR5/7+K0KstH54WG443w.0], [Date, Thu, 09 May 2019 16:45:41 GMT]}
RawContentLength  : 0
```

## <a name="next-steps"></a>Nächste Schritte

* [PowerShell-Beispielskripts](../samples/powershell-example-scripts.md)
