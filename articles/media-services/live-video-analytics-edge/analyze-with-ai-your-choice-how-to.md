---
title: Analysieren von Livevideos mit KI Ihrer Wahl – Azure
description: In diesem Artikel erfahren Sie, wie Sie ein IoT Edge-Modul erstellen, das in Live Video Analytics in IoT Edge integriert werden kann, um Livevideos mithilfe eines Modells zum maschinellen Sehen Ihrer Wahl zu analysieren.
ms.topic: how-to
ms.date: 04/27/2020
ms.openlocfilehash: 0ac2af280eefd5ce293a8be422551d5ee6f6d3f3
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84260643"
---
# <a name="analyze-live-video-with-ai-of-your-choice"></a>Analysieren von Livevideos mit KI Ihrer Wahl

In diesem Artikel erfahren Sie, wie Sie ein IoT Edge-Modul erstellen, das in Live Video Analytics in IoT Edge integriert werden kann, um Livevideos mithilfe eines Modells zum maschinellen Sehen Ihrer Wahl zu analysieren. 

## <a name="pre-reading"></a>Vorbereitung

[Mediengraph: Konzepte](media-graph-concept.md)

## <a name="media-graph-extension"></a>Mediengrapherweiterung

Live Video Analytics in IoT Edge definiert ein Erweiterungsmodell, mit dem Sie die Verarbeitungsfunktionen für Mediengraphen durch eine Grapherweiterung auf Ihre eigenen Medienanalysekomponenten erweitern können. Ihre Analysekomponente kann herkömmliche Bildverarbeitungstechniken oder KI-Modelle des maschinellen Sehens verwenden. Grapherweiterungen werden aktiviert, indem der Knoten [HTTP-Erweiterungsprozessor](media-graph-concept.md#http-extension-processor) in einen Mediengraph eingeschlossen wird. Der HTTP-Erweiterungsprozessor kann Videoframes an einen von Ihnen angegebenen HTTP-Endpunkt weiterleiten und fungiert über diesen als Schnittstelle zu Ihrer Komponente. Die Verbindung kann zu einem lokalen oder entfernten Endpunkt hergestellt und bei Bedarf durch Authentifizierung und TLS-Verschlüsselung gesichert werden. Zudem ermöglicht der Prozessor eine optionale Skalierung und Codierung der Videoframes, bevor sie weitergeleitet werden.

Sie können sich dafür entscheiden, das Rückschlussmodell Ihrer Wahl für jede verfügbare Rückschlusslaufzeit auszuführen, z. B. ONNX, TensorFlow, PyTorch oder andere in Ihrem eigenen Docker-Container. Sie können auch die Programmiersprache und Bibliotheken Ihrer Wahl verwenden, um die Bildrückschlussfunktionen über einen HTTP-Server in einem eigenen Container verfügbar zu machen. Der Rückschlusscontainer sollte für eine optimale Leistung zusammen mit dem Edge-Modul Live Video Analytics bereitgestellt werden. Anschließend wird er über den HTTP-Erweiterungsprozessor aufgerufen, der in Ihrer Graphtopologie enthalten ist.
Zusätzlich kann die Häufigkeit der Aufrufe in Ihr benutzerdefiniertes Modul gedrosselt werden, indem wahlweise ein [Bewegungsmelderprozessor](media-graph-concept.md#motion-detection-processor) und ein [Bildfrequenzfilterprozessor](media-graph-concept.md#frame-rate-filter-processor) dem HTTP-Erweiterungsprozessor vorgeschaltet werden.

Das folgende Diagramm zeigt den Datenfluss im Überblick:

![Edgegerät](./media/analyze-live-video-with-ai-your-choice-how-to/edge-device.png)

Auf diese Weise können Sie Videos mithilfe von KI-Modellen Ihrer Wahl analysieren, um Ihre individuellen Geschäftsanforderungen zu erfüllen. Die KI-Modelle könnten aus der Open-Source-Community, von Ihren eigenen Data Scientists oder von einem spezialisierten KI-Anbieter stammen.

## <a name="media-graph-http-extension-contract-definitions"></a>Definitionen von HTTP-Erweiterungsverträgen für Mediengraphen

Dieser Abschnitt definiert den HTTP-Vertrag, der den Datenfluss definiert.

### <a name="http-extensibility-protocol"></a>HTTP-Erweiterbarkeitsprotokoll  

Der HTTP-Vertrag ist wie folgt definiert:

* Ihr Modul fungiert als HTTP-Server.
* Das Modul Live Video Analytics in IoT Edge fungiert als HTTP-Client.

### <a name="request"></a>Anforderung

Anforderungen vom Live Video Analytics-Modul an Ihr Modul lauten wie folgt:

|||
|---|---|
|POST| `https://hostname/optional-path?optional-query`|
|Akzeptieren|application/json, */*|
|Authorization| Basic, Digest, Bearer (durch Unterstützung benutzerdefinierter Header)|
|Content-Type|image/jpeg<br/>image/png<br/>image/bmp<br/>image/x-raw|
|Content-Length|Textlänge in Bytes|
|User-Agent|Azure Media Services|
|Body|Bildbytes, binär kodiert in einem der unterstützten Inhaltstypen.|

#### <a name="example"></a>Beispiel

```
POST http://localhost:8080/inference HTTP/1.1
Host: localhost:8080
x-ms-client-request-id: d6050cd4-c9f2-42d3-9adc-53ba7e440f17
Content-Type: image/bmp
Content-Length: 519222

(Image Binary Content)
```

### <a name="response"></a>Antwort

Antworten von Ihrem Modul an das Live Video Analytics-Modul lauten wie folgt:

|||
|---|---|
|Statuscodes|200 OK. Rückschlussergebnisse gefunden<br/>204 Kein Inhalt. Nicht von der KI gefunden.<br/>400 Ungültige Anforderung. Nicht erwartet.<br/>500 Interner Serverfehler. Nicht erwartet.<br/>503 Server ausgelastet. AMS-Backoff basierend auf dem „Retry-After“-Header oder, falls der Header nicht festgelegt wurde, basierend auf einer Standardzeitspanne.|
|Content-Type|Anwendung/json|
|Content-Length|    Textlänge in Bytes|
|Body|JSON-Objekt mit einzelner „inferences“-Eigenschaft.|

#### <a name="example"></a>Beispiel

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

Es wird dringend empfohlen, dass Antworten mithilfe gültiger JSON-Dokumente zurückgegeben werden, die dem im Folgenden definierten vorab festgelegten Schema entsprechen. Dies gewährleistet eine bessere Interoperabilität mit anderen Komponenten und möglichen zukünftigen Funktionen, die dem Live Video Analytics-Modul hinzugefügt werden.

Wenn Ihr Modul eine Antwort zurückgibt, bei der der Inhaltstyp nicht „application/json“ lautet, codiert Live Video Analytics die Nachricht als Base64-Inhalt und serialisiert sie als transparente JSON-Nutzlast.

Wenn Ihr Modul eine Antwort mit dem Inhaltstyp „application/json“ zurückgibt, das JSON-Schema jedoch nicht dem [unten beschriebenen Rückschlussmetadatenschema](#inference-metadata-schema) folgt, wird die Nachrichtennutzlast über die Pipeline weitergeleitet, aber die Interoperabilität wird verringert.

> [!NOTE]
> Wenn Ihr Modul kein Ergebnis liefert, sollte es den HTTP-204-Statuscode (kein Inhalt) mit einem leeren Antworttext zurückgeben. Live Video Analytics wird dies als leeres Ergebnis verstehen und das Ereignis nicht über die gesamte Pipeline weiterleiten.

### <a name="inference-metadata-schema"></a>Schema für Rückschlussmetadaten

Jedes Rückschlussobjekt folgt diesem übergeordneten Schema:

```
{
  "type": "[classification|motion|entity|text|other]",
  "subtype": "",              // Free form subtype id

  // Object has *at most one of* the following
  "classification":
  {
    "tag": 
    {
      "value": "",            // Tag value
      "confidence": 0.0       // Tag confidence
    },
    "attributes":[            // Optional attributes
      {
        "name": "",           
        "value": "",          
        "confidence": 0.0     
      }
    ]
  },

  "motion":
  {
    "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 }
    // "regionId": ""
  },

  "entity":
  {
    "tag": 
    {
      "value": "",            // Tag value
      "confidence": 0.0       // Tag confidence
    },
    "attributes":[            // Optional attributes
      {
        "name": "",           
        "value": "",          
        "confidence": 0.0     
      }
    ],
    "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 }
  },

  "text":
  {
    "value": "",              // Text value
    "language": "",           // Optional BCP47 Language Code (https://tools.ietf.org/html/bcp47)
    "startTimestamp": 0,      // Optional start timestamp
    "endTimestamp": 0         // Optional end timestamp
  },

  // Plus every object can have zero or more extensions
  "extensions":
  {
    "name1": "value1",
    "name2": "value2"
    // ...
  }
}
```
 
### <a name="data-contracts---class-hierarchy"></a>Datenverträge – Klassenhierarchie

![Datenverträge – Klassenhierarchie](./media/analyze-live-video-with-ai-your-choice-how-to/data-contracts.png)

### <a name="examples"></a>Beispiele  

Das folgende Beispiel enthält ein einzelnes Ereignis mit allen unterstützten Rückschlusstypen:

```
{
  "inferences": [
    // Light detection
    {
      "type": "classification",
      "subtype": "lightDetection",
      "classification": {
        "tag": { "value": "daylight", "confidence": 0.86 },
        "attributes": [
            { "name": "isBlackAndWhite", "value": "false", "confidence": 0.71 }
        ]
      }
    },

    // Motion detection
    {
      "type": "motion",
      "subtype": "motionDetection",
      "motion":
      {
        "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 }
      }
    },

    // Object detection
    {
      "type": "entity",
      "subtype": "objectDetection",    
      "entity":
      {
        "tag": { "value": "dog", "confidence": 0.97 },
        "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 }
      }
    },

    // Vehicle identification
    {
      "type": "entity",
      "subtype": "vehicleIdentification",    
      "entity":
      {
        "tag": { "value": "007-SPY", "confidence": 0.82 },
        "attributes":[  
          { "name": "color", "value": "black", "confidence": 0.90 },
          { "name": "body", "value": "coupe", "confidence": 0.87 },
          { "name": "make", "value": "Aston Martin", "confidence": 0.35 },
          { "name": "model", "value": "DBS V12", "confidence": 0.33 }
        ],
        "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 }
      }
    },

    // People identification
    {
      "type": "entity",
      "subtype": "peopleIdentification",    
      "entity":
      {
        "tag": { "value":"Erwin Schrödinger", "confidence": 0.50 },
        "attributes":[  
          { "name": "age", "value": "73", "confidence": 0.87 },
          { "name": "glasses", "value": "yes", "confidence": 0.94 }
        ],
        "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 }
      },
    }
  ]
}
```
 
## <a name="sample-http-extension-modules"></a>Beispiele für HTTP-Erweiterungsmodule

Einige Beispiele für HTTP-Erweiterungsmodule finden Sie im [GitHub-Repository zu Live Video Analytics](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis). Eines dieser [Videoanalysebeispiele](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis/yolov3-onnx) zeigt, wie das [ONNX](http://onnx.ai/)-Modell [Yolov3](https://pjreddie.com/darknet/yolo/) verwendet wird, um ein IoT Edge-Modul für die Objekterkennung zu erstellen. Sie können den gleichen Ansatz verwenden, um ein eigenes Modul mit einem KI-Modell Ihrer Wahl zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

[Problembehandlung](troubleshoot-how-to.md)
