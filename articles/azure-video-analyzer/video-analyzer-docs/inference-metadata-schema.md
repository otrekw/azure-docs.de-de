---
title: 'Schema für Rückschlussmetadaten: Azure'
description: Im Azure Video Analyzer folgt jedes Inferenzobjekt, unabhängig von der Verwendung eines HTTP-basierten Vertrags oder eines gRPC-basierten Vertrags, dem in diesem Thema beschriebenen Objektmodell.
ms.topic: reference
ms.date: 04/11/2021
ms.openlocfilehash: 4400978fca931741a1a12531d7923b82a5ead3e5
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110386246"
---
# <a name="inference-metadata-schema"></a>Schema für Rückschlussmetadaten 

Im Azure Video Analyzer folgt jedes Inferenzobjekt, unabhängig von der Verwendung eines HTTP-basierten Vertrags oder eines gRPC-basierten Vertrags, dem in diesem Thema beschriebenen Objektmodell.

## <a name="object-model"></a>Objektmodell

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/inference-metadata-schema/object-model.png" alt-text="Azure Video Analyzer Oot-Modell" lightbox="./media/inference-metadata-schema/object-model.png":::

 
|Typendefinition|Beschreibung|
|---|---|
|Tag|Tag oder Etikett, das dem Ergebnis zugeordnet ist. Zusammen mit der Kennzeichnung erhalten Sie auch den mit der Kennzeichnung verbundenen Vertrauenswert.|
|attribute|Zusätzliche Attribute, die dem Ergebnis zugeordnet sind. Sie können neue Attribute hinzufügen, die Sie von der Rückschlussengine zusammen mit dem Zuverlässigkeitswert erhalten.|
|Attributliste|Liste optionaler Attribute.|
|Rechteck|Rechteckiger Bereich relativ zum linken oberen Winkel der Abbildung. Zu den erforderlichen Eigenschaften zählen Länge, Breite, Höhe und Abstand von der Oberkante des Originals.|
|Klassifizierung|Klassifizierungsbeschriftung, die häufig für die gesamte Stichprobe gilt. Anhand von „Tag“ können Sie das Ergebnis klassifizieren.|
|Entität|In der Stichprobe erkannte oder identifizierte Entität. Wenn eine Entität von der Inferenz-Maschine erkannt wird, erhält sie einen Tag. Zudem werden zusätzliche Attribute, die abgeleitet wurden, sowie die Koordinaten eines rechteckigen Felds um die gefundene Entität zurückgegeben.|
|Ereignis|In der Stichprobe erkanntes Ereignis. Wenn in der Stichprobe ein Ereignis erkannt wird, werden der Name des Ereignisses und die Ereignis-spezifischen Eigenschaften zurückgegeben.|
|Bewegung|In der Stichprobe erkannte Bewegung. Wenn eine Bewegung in der Probe erkannt wird, werden die Koordinaten eines rechteckigen Begrenzungsrahmens (Bounding Box) zurückgegeben, in dem die Bewegung erkannt wurde.|
|Text|Mit der Stichprobe verknüpfter Text sowie die Zeitstempel für Textbeginn und Textende werden zurückgegeben.|
|Sonstiges|Gibt weitere generische Nutzlastinformationen zurück.|

Das folgende Beispiel enthält ein einzelnes Ereignis mit einigen unterstützten Rückschlusstypen:

```
[ 
  // Light Detection 
  { 
    "type": "classification", 
    "subtype": "lightDetection", 
    "classification": { 
      "tag": { "value": "daylight", "confidence": 0.86 }, 
      "attributes": [ 
          { "name": "isBlackAndWhite", "value": "false", "confidence": 0.71 } 
      ] 
    } 
  }, 
 
  // Motion Detection 
  { 
    "type": "motion", 
    "subtype": "motionDetection", 
    "motion": 
    { 
      "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 } 
    } 
  }, 
 
  // Yolo V3 
  { 
    "type": "entity", 
    "subtype": "objectDetection",     
    "entity": 
    { 
      "tag": { "value": "dog", "confidence": 0.97 }, 
      "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 } 
    } 
  }, 
 
  // Vehicle Identification 
  { 
    "type": "entity", 
    "subtype": "vehicleIdentification",     
    "entity": 
    { 
      "tag": { "value": "007-SPY", "confidence": 0.82 }, 
      "attributes":[   
        { "name": "color", "value": "black", "confidence": 0.90 }, 
        { "name": "body", "value": "coupe", "confidence": 0.87 }, 
        { "name": "make", "value": "Aston Martin", "confidence": 0.35 }, 
        { "name": "model", "value": "DBS V12", "confidence": 0.33 } 
      ], 
      "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 } 
    } 
  }, 
 
  // People Identification 
  { 
    "type": "entity", 
    "subtype": "peopleIdentification",     
    "entity": 
    { 
      "tag": { "value":"Erwin Schrödinger", "confidence": 0.50 }, 
      "attributes":[   
        { "name": "age", "value": "73", "confidence": 0.87 }, 
        { "name": "glasses", "value": "yes", "confidence": 0.94 } 
      ], 
      "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 } 
    }, 
 
    // Open type coming from the gRPC Map 
    "extensions":  
    { 
      "vector": "e1xkaXNwbGF5c3R5bGUgaVxoYmFyIHtcZnJhYyB7ZH17ZHR9fVx2ZXJ0IFxQc2kgKHQpXHJhbmdsZSA9e1xoYXQge0h9fVx2ZXJ0IFxQc2kgKHQpXHJhbmdsZSB9KQ==", 
      "skeleton": "p1,p2,p3,p4" 
    } 
  }, 
 
  // Captions 
  {     
    "type": "text", 
    "subtype": "speechToText",   
    "text": 
    { 
      "value": "Humor 75%. Confirmed. Self-destruct sequence in T minus 10… 9…", 
      "language": "en-US", 
      "startTimestamp": 12000, 
      "endTimestamp": 13000 
    } 
]
```

## <a name="next-steps"></a>Nächste Schritte

- [gRPC-Datenvertrag](./grpc-extension-protocol.md)
- [HTTP-Datenvertrag](./http-extension-protocol.md)