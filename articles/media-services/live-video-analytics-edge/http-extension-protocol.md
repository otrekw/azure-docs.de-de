---
title: HTTP-Erweiterungsprotokoll – Azure
description: In diesem Artikel erfahren Sie mehr über die Verwendung des HTTP-Erweiterungsprotokolls zum Senden von Nachrichten zwischen dem Live Video Analytics-Modul und Ihrem KI- oder CV-Modul.
ms.topic: overview
ms.date: 09/14/2020
ms.openlocfilehash: c3a9b5b70467479fdb9b1473a1f6738eaa8a7e1b
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102455889"
---
# <a name="http-extension-protocol"></a>HTTP-Erweiterungsprotokoll

Mit Live Video Analytics in IoT Edge können Sie die Mediengraph-Verarbeitungsfunktionen über einen [Grapherweiterungsknoten](/azure/media-services/live-video-analytics-edge/media-graph-extension-concept) erweitern. Wenn Sie den HTTP-Erweiterungsprozessor als Erweiterungsknoten verwenden, erfolgt die Kommunikation zwischen dem Live Video Analytics-Modul und Ihrem KI- oder CV-Modul über HTTP.

In diesem Artikel erfahren Sie mehr über die Verwendung des HTTP-Erweiterungsprotokolls zum Senden von Nachrichten zwischen dem Live Video Analytics-Modul und Ihrem KI- oder CV-Modul. 

Der HTTP-Vertrag wird zwischen den folgenden beiden Komponenten definiert:

* HTTP-Server
* Das Modul „Live Video Analytics in IoT Edge“ fungiert als HTTP-Client.

## <a name="request"></a>Anforderung

Anforderungen vom Live Video Analytics-Modul an Ihren HTTP-Server lauten wie folgt:

|Key|Wert|
|---|---|
|POST|`https://hostname/optional-path?optional-query`|
|Akzeptieren|application/json, */*|
|Authorization|Basic, Digest, Bearer (durch Unterstützung benutzerdefinierter Header)|
|Content-Type|image/jpeg<br/>image/png<br/>image/bmp<br/>image/x-raw|
|Content-Length (Textlänge in Bytes)|
|User-Agent|Azure Media Services|
|Body|Bildbytes, binär kodiert in einem der unterstützten Inhaltstypen.|

### <a name="example"></a>Beispiel

```
POST http://localhost:8080/inference HTTP/1.1
Host: localhost:8080
x-ms-client-request-id: d6050cd4-c9f2-42d3-9adc-53ba7e440f17
Content-Type: image/bmp
Content-Length: 519222

(Image Binary Content)
```

## <a name="response"></a>Antwort

Antworten von Ihrem Modul an das Live Video Analytics-Modul lauten wie folgt:

|Key|Wert|
|---|---|
|Statuscodes|200 OK. Rückschlussergebnisse gefunden<br/>204 Kein Inhalt. Nicht von der KI gefunden.<br/>400 Ungültige Anforderung. Nicht erwartet.<br/>500 Interner Serverfehler. Nicht erwartet.<br/>503 Server ausgelastet. AMS-Backoff basierend auf dem Header „Retry-After“ oder, falls der Header nicht festgelegt wurde, basierend auf einer Standardzeitspanne|
|Content-Type|Anwendung/json|
|Content-Length|Textlänge in Bytes|
|Body|JSON-Objekt mit einzelner Eigenschaft vom Typ „inferences“|

### <a name="example"></a>Beispiel

```
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 468
Server: Microsoft-HTTPAPI/2.0
Date: Fri, 17 Apr 2020 04:44:01 GMT

{
  "inferences": [
    {
      "type": "entity",
      "entity": {
        "tag": { "value": "car", "confidence": 0.9048132 },
        "box": { "l": 0.42681578, "t": 0.47660735, "w": 0.019501392, "h": 0.020954132 }
      }
    },
    {
      "type": "entity",
      "entity": {
        "tag": { "value": "car", "confidence": 0.8953932 },
        "box": { "l": 0.55083525, "t": 0.4843858, "w": 0.046550274, "h": 0.046502113 }
      }
    }    
  ]
}
```

Es wird dringend empfohlen, dass Antworten mithilfe gültiger JSON-Dokumente zurückgegeben werden, die dem gemäß dem [Schemaobjektmodell für Rückschlussmetadaten](/azure/media-services/live-video-analytics-edge/inference-metadata-schema) vorab definierten Schema entsprechen. Dies gewährleistet eine bessere Interoperabilität mit anderen Komponenten und möglichen zukünftigen Funktionen, die dem Live Video Analytics-Modul hinzugefügt werden.

Wenn Ihr Modul eine Antwort zurückgibt, bei der der Inhaltstyp nicht „application/json“ lautet, codiert Live Video Analytics die Nachricht als Base64-Inhalt und serialisiert sie als transparente JSON-Nutzlast.

Wenn Ihr Modul eine Antwort mit dem Inhaltstyp „application/json“ zurückgibt, das JSON-Schema jedoch nicht dem unten beschriebenen Rückschlussmetadatenschema folgt, wird die Nachrichtennutzlast über die Pipeline weitergeleitet, aber die Interoperabilität wird verringert. Ausführliche und aktuelle Informationen zum Rückschlussmetadatenschema finden Sie [hier](/azure/media-services/live-video-analytics-edge/inference-metadata-schema).

> [!NOTE]
> Wenn Ihr Modul kein Ergebnis liefert, sollte es den HTTP-204-Statuscode (kein Inhalt) mit einem leeren Antworttext zurückgeben. Live Video Analytics wird dies als leeres Ergebnis verstehen und das Ereignis nicht über die gesamte Pipeline weiterleiten.


## <a name="next-steps"></a>Nächste Schritte

[gRPC-Erweiterungsprotokoll](./grpc-extension-protocol.md)
