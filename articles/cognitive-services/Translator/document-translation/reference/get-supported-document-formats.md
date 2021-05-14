---
title: Methode „Abrufen unterstützter Dokumentformate“
titleSuffix: Azure Cognitive Services
description: Die Methode „Abrufen unterstützter Dokumentformate“ (Get document formats) gibt eine Liste unterstützter Dokumentformate zurück.
services: cognitive-services
author: jann-skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 04/21/2021
ms.author: v-jansk
ms.openlocfilehash: e47f3363a9e09a3e371c751e0bdd1143cfc72314
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107864899"
---
# <a name="get-supported-document-formats"></a>Abrufen unterstützter Dokumentformate

Die Methode „Abrufen unterstützter Dokumentformate“ gibt eine Liste von Dokumentformaten zurück, die vom Dokumentübersetzungsdienst unterstützt werden. Die Liste enthält die gemeinsame Dateierweiterung und den Inhaltstyp, wenn die Upload-AP verwendet wird.

## <a name="request-url"></a>Anfrage-URL

Sendet eine `GET`-Anforderung an:
```HTTP
GET https://<NAME-OF-YOUR-RESOURCE>.cognitiveservices.azure.com/translator/text/batch/v1.0-preview.1/documents/formats
```

Erfahren Sie, wie Sie Ihren [benutzerdefinierten Domänennamen](../get-started-with-document-translation.md#find-your-custom-domain-name)finden.

> [!IMPORTANT]
>
> * **Für alle API-Anforderungen an den Dienst für die Dokumentübersetzung muss ein Endpunkt einer benutzerdefinierten Domäne verwendet werden**.
> * Zum Senden von HTTP-Anforderungen für die Dokumentübersetzung verwenden Sie nicht den Endpunkt, der im Azure-Portal auf der Ressourcenseite _Schlüssel und Endpunkt_ angegeben ist, und auch nicht den globalen Übersetzungsendpunkt `api.cognitive.microsofttranslator.com`.

## <a name="request-headers"></a>Anforderungsheader

Anforderungsheader:

|Header|BESCHREIBUNG|
|-----|-----|
|Ocp-Apim-Subscription-Key|Erforderlicher Anforderungsheader|

## <a name="response-status-codes"></a>Antwortstatuscodes

Im Folgenden finden Sie die möglichen HTTP-Statuscodes, die eine Anforderung zurückgeben kann.

|Statuscode|BESCHREIBUNG|
|-----|-----|
|200|OK. Gibt die Liste der unterstützten Dokument Dateiformate zurück.|
|500|Interner Serverfehler.|
|Andere Statuscodes|<ul><li>Zu viele Anforderungen</li><li>Server vorübergehend nicht verfügbar</li></ul>|

## <a name="file-format-response"></a>Dateiformat-Antwort

### <a name="successful-fileformatlistresult-response"></a>Erfolgreiche „fileFormatListResult“-Antwort

Die folgenden Informationen werden bei erfolgreicher Antwort zurückgegeben.

|Name|type|BESCHREIBUNG|
|--- |--- |--- |
|value|FileFormat []|File Format [] enthält die unten aufgeführten Details.|
|value.format|string[]|Unterstützte Inhaltstypen für dieses Format.|
|value.fileExtensions|string[]|Unterstützte Dateierweiterung für dieses Format.|
|value.contentTypes|string[]|Der Name des Formats.|
|value.versions|String[]|Unterstützte Version.|

### <a name="error-response"></a>Fehlerantwort

|Name|type|Beschreibung|
|--- |--- |--- |
 |code|Zeichenfolge|Enumerationen, die High-Level-Fehlercodes enthalten. Mögliche Werte:<ul><li>InternalServerError</li><li>InvalidArgument</li><li>InvalidRequest</li><li>RequestRateTooHigh</li><li>ResourceNotFound</li><li>ServiceUnavailable</li><li>Nicht autorisiert</li></ul>|
|message|Zeichenfolge|Ruft High-Level-Fehlermeldung ab.|
|innerError|InnerErrorV2|Neues internes Fehlerformat, das Cognitive Services API-Richtlinien entspricht. Enthält die erforderlichen Eigenschaften ErrorCode, Message und Optional Properties Target, Details (Key Value Pair), Inner Error (kann geschachtelt werden).|
|innerError.code|Zeichenfolge|Ruft Code der Fehlerzeichenfolge ab.|
|innerError.message|Zeichenfolge|Ruft High-Level-Fehlermeldung ab.|

## <a name="examples"></a>Beispiele

### <a name="example-successful-response"></a>Beispiel für erfolgreiche Antwort
Das ist ein Beispiel für eine erfolgreiche Antwort.

Statuscode: 200

```JSON
{
  "value": [
    {
      "format": "PlainText",
      "fileExtensions": [
        ".txt"
      ],
      "contentTypes": [
        "text/plain"
      ],
      "versions": []
    },
    {
      "format": "PortableDocumentFormat",
      "fileExtensions": [
        ".pdf"
      ],
      "contentTypes": [
        "application/pdf"
      ],
      "versions": []
    },
    {
      "format": "OpenXmlPresentation",
      "fileExtensions": [
        ".pptx"
      ],
      "contentTypes": [
        "application/vnd.openxmlformats-officedocument.presentationml.presentation"
      ],
      "versions": []
    },
    {
      "format": "OpenXmlSpreadsheet",
      "fileExtensions": [
        ".xlsx"
      ],
      "contentTypes": [
        "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"
      ],
      "versions": []
    },
    {
      "format": "OutlookMailMessage",
      "fileExtensions": [
        ".msg"
      ],
      "contentTypes": [
        "application/vnd.ms-outlook"
      ],
      "versions": []
    },
    {
      "format": "HtmlFile",
      "fileExtensions": [
        ".html"
      ],
      "contentTypes": [
        "text/html"
      ],
      "versions": []
    },
    {
      "format": "OpenXmlWord",
      "fileExtensions": [
        ".docx"
      ],
      "contentTypes": [
        "application/vnd.openxmlformats-officedocument.wordprocessingml.document"
      ],
      "versions": []
    }
  ]
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