---
title: Abrufen des Dokumentstatus
titleSuffix: Azure Cognitive Services
description: Die Methode „Abrufen des Dokumentstatus“ (Get Operation Documents Status) gibt den Status aller Dokumente in einer Dokumentübersetzungs-Batchanforderung zurück.
services: cognitive-services
author: jann-skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 04/21/2021
ms.author: v-jansk
ms.openlocfilehash: 8476c4891cef9d9055b16c7ac574e569ecf5b3f2
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107864862"
---
# <a name="get-documents-status"></a>Abrufen des Dokumentstatus

Die Methode „Abrufen des Dokumentstatus“ (Get Operation Documents Status) gibt den Status aller Dokumente in einer Dokumentübersetzungs-Batchanforderung zurück.

Die in der Antwort enthaltenen Dokumente werden nach Dokument-ID in absteigender Reihenfolge sortiert. Wenn die Anzahl der Dokumente in der Antwort das Paging-Limit überschreitet, wird serverseitiges Paging verwendet. Paging-Antworten geben ein Teilergebnis an und enthalten ein Fortsetzungstoken in der Antwort. Das Fehlen eines Fortsetzungstokens bedeutet, dass keine zusätzlichen Seiten verfügbar sind.

$top- und $skip-Abfrageparameter können verwendet werden, um eine Reihe von Ergebnissen, die zurückgegeben werden sollen, und einen Offset für die Auflistung anzugeben. Der Server berücksichtigt die Werte, die vom Client angegeben werden. Allerdings müssen Clients darauf vorbereitet sein, Antworten zu verarbeiten, die eine andere Seitengröße enthalten oder ein Fortsetzungstoken enthalten.

Wenn sowohl $top als auch $skip enthalten sind, sollte der Server zuerst $skip anwenden und dann auf die Sammlung $top.

> [!NOTE]
> Wenn der Server $top und/oder $skip nicht berücksichtigt, muss der Server einen Fehler an den Client zurückgeben, der darüber informiert, anstatt nur die Abfrageoptionen zu ignorieren. Dadurch wird das Risiko verringert, dass der Client Annahmen über die zurückgegebenen Daten vornimmt.

## <a name="request-url"></a>Anfrage-URL

Sendet eine `GET`-Anforderung an:
```HTTP
GET https://<NAME-OF-YOUR-RESOURCE>.cognitiveservices.azure.com/translator/text/batch/v1.0-preview.1/batches/{id}/documents
```

Erfahren Sie, wie Sie Ihren [benutzerdefinierten Domänennamen](../get-started-with-document-translation.md#find-your-custom-domain-name)finden.

> [!IMPORTANT]
>
> * **Für alle API-Anforderungen an den Dienst für die Dokumentübersetzung muss ein Endpunkt einer benutzerdefinierten Domäne verwendet werden**.
> * Zum Senden von HTTP-Anforderungen für die Dokumentübersetzung verwenden Sie nicht den Endpunkt, der im Azure-Portal auf der Ressourcenseite _Schlüssel und Endpunkt_ angegeben ist, und auch nicht den globalen Übersetzungsendpunkt `api.cognitive.microsofttranslator.com`.

## <a name="request-parameters"></a>Anforderungsparameter

Die folgenden Anforderungsparameter werden in der Abfragezeichenfolge übergeben:

|Query parameter (Abfrageparameter)|Erforderlich|BESCHREIBUNG|
|--- |--- |--- |
|id|Richtig|Vorgangs-ID.|
|$skip|Falsch|Überspringt die $skip-Einträge in der Auflistung. Wenn sowohl $top als auch $skip bereitgestellt werden, wird $skip zuerst angewendet.|
|$top|Falsch|Nimmt die $to- Einträge in die Auflistung auf. Wenn sowohl $top als auch $skip bereitgestellt werden, wird $skip zuerst angewendet.|

## <a name="request-headers"></a>Anforderungsheader

Anforderungsheader:

|Header|BESCHREIBUNG|
|--- |--- |
|Ocp-Apim-Subscription-Key|Erforderlicher Anforderungsheader|

## <a name="response-status-codes"></a>Antwortstatuscodes

Im Folgenden finden Sie die möglichen HTTP-Statuscodes, die eine Anforderung zurückgeben kann.

|Statuscode|BESCHREIBUNG|
|--- |--- |
|200|OK. Die Anforderung wurde erfolgreich ausgeführt, und der Status aller Dokumente wird zurückgegeben. HeadersRetry-After: integerETag: Zeichenfolge|
|400|Ungültige Anforderung. Eingabeparameter prüfen.|
|401|Nicht autorisiert. Anmeldeinformationen prüfen.|
|404|Ressource nicht gefunden.|
|500|Interner Serverfehler.|
|Andere Statuscodes|<ul><li>Zu viele Anforderungen</li><li>Server vorübergehend nicht verfügbar</li></ul>|


## <a name="get-documents-status-response"></a>„Abrufen des Dokumentstatus“-Antwort

### <a name="successful-get-documents-status-response"></a>Erfolgreiche „Abrufen des Dokumentstatus“-Antwort

Die folgenden Informationen werden bei erfolgreicher Antwort zurückgegeben.

|Name|type|BESCHREIBUNG|
|--- |--- |--- |
|@nextLink|Zeichenfolge|Die URL für die nächste Seite. Null, wenn keine weiteren Seiten verfügbar sind.|
|Wert|DocumentStatusDetail []|Der detaillierte Status der einzelnen unten aufgeführten Dokumente.|
|value.path|Zeichenfolge|Speicherort des Dokuments oder des Ordners.|
|value.createdDateTimeUtc|Zeichenfolge|Das Datum und die Uhrzeit des Vorgangs.|
|value.lastActionDateTimeUt|Zeichenfolge|Datum und Uhrzeit, zu der der Status des Vorgangs aktualisiert wurde.|
|value.status|status|Liste möglicher Status für Auftrag oder Dokument:<ul><li>Canceled</li><li>Wird abgebrochen</li><li>Fehler</li><li>NotStarted</li><li>Wird ausgeführt</li><li>Erfolgreich</li><li>ValidationFailed</li></ul>|
|value.to|Zeichenfolge|In Sprache.|
|value.progress|Zeichenfolge|Der Fortschritt der Übersetzung, falls verfügbar.|
|value.id|Zeichenfolge|Dokument-ID|
|value.characterCharged|integer|Zeichen, die von der API abgerechnet werden.|

### <a name="error-response"></a>Fehlerantwort

|Name|type|BESCHREIBUNG|
|--- |--- |--- |
|code|Zeichenfolge|Enumerationen, die High-Level-Fehlercodes enthalten. Mögliche Werte:<br/><ul><li>InternalServerError</li><li>InvalidArgument</li><li>InvalidRequest</li><li>RequestRateTooHigh</li><li>ResourceNotFound</li><li>ServiceUnavailable</li><li>Nicht autorisiert</li></ul>|
|message|Zeichenfolge|Ruft High-Level-Fehlermeldung ab.|
|target|Zeichenfolge|Ruft die Ursache des Fehlers ab. Dies wäre z. B. „Dokumente“ oder „Dokument-ID“ im Falle eines ungültigen Dokuments.|
|innerError|InnerErrorV2|Neues internes Fehlerformat, das Cognitive Services API-Richtlinien entspricht. Enthält die erforderlichen Eigenschaften ErrorCode, Message und Optional Properties Target, Details (Key Value Pair), Inner Error (kann geschachtelt werden).|
|innerError.code|Zeichenfolge|Ruft Code der Fehlerzeichenfolge ab.|
|innerError.message|Zeichenfolge|Ruft High-Level-Fehlermeldung ab.|

## <a name="examples"></a>Beispiele

### <a name="example-successful-response"></a>Beispiel für erfolgreiche Antwort

Das ist ein Beispiel für eine erfolgreiche Antwort.

```JSON
{
  "value": [
    {
      "path": "https://myblob.blob.core.windows.net/destinationContainer/fr/mydoc.txt",
      "createdDateTimeUtc": "2020-03-26T00:00:00Z",
      "lastActionDateTimeUtc": "2020-03-26T01:00:00Z",
      "status": "Running",
      "to": "fr",
      "progress": 0.1,
      "id": "273622bd-835c-4946-9798-fd8f19f6bbf2",
      "characterCharged": 0
    }
  ],
  "@nextLink": "https://westus.cognitiveservices.azure.com/translator/text/batch/v1.0.preview.1/operation/0FA2822F-4C2A-4317-9C20-658C801E0E55/documents?$top=5&$skip=15"
}
```

### <a name="example-error-response"></a>Beispiel für Fehlerantwort

Das folgende Beispiel veranschaulicht eine Fehlerantwort. Das Schema für andere Fehlercodes ist identisch.

Statuscode: 500

```JSON
{
  "error": {
    "code": "InternalServerError",
    "message": "Internal Server Error",
    "target": "Operation",
    "innerError": {
      "code": "InternalServerError",
      "message": "Unexpected internal server error has occurred"
    }
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

Befolgen Sie unsere Schnellstartanleitung, um mehr über die Verwendung der Dokumentübersetzung und der Clientbibliothek zu erfahren.

> [!div class="nextstepaction"]
> [Erste Schritte bei der Dokumentübersetzung](../get-started-with-document-translation.md)
