---
title: 'Schema für Rückschlussmetadaten: Azure'
description: Dieser Artikel enthält Informationen zum Schema für Rückschlussmetadaten.
ms.topic: overview
ms.date: 09/14/2020
ms.openlocfilehash: 6239713fc92b75b8ed026a8f04953e92a24c4596
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "88701913"
---
# <a name="inference-metadata-schema"></a>Schema für Rückschlussmetadaten 

Jedes Rückschlussobjekt basiert auf dem in diesem Thema beschriebenen Objektmodell. Das gilt sowohl bei Verwendung eines HTTP-basierten Vertrags als auch bei Verwendung eines gRPC-basierten Vertrags.

## <a name="object-model"></a>Objektmodell

![Objektmodell](./media/inference-metadata-schema/object-model.png)
 
|Typendefinition|Beschreibung|
|---|---|
|Tag|Tag oder Beschriftung, das bzw. die dem Ergebnis zugeordnet ist. Neben dem Tagging erhalten Sie auch den mit dem Tag verknüpften Zuverlässigkeitswert.|
|attribute|Zusätzliche Attribute, die dem Ergebnis zugeordnet sind. Sie können neue Attribute hinzufügen, die Sie von der Rückschlussengine zusammen mit dem Zuverlässigkeitswert erhalten.|
|Attributliste|Liste optionaler Attribute.|
|Rechteck|Rechteckiger Bereich relativ zur linken oberen Ecke des Bilds. Zu den erforderlichen Eigenschaften zählen Länge, Breite, Höhe und Abstand von der Oberkante des Originals.|
|Klassifizierung|Klassifizierungsbeschriftung, die häufig für die gesamte Stichprobe gilt. Mithilfe von „tag“ können Sie das Ergebnis klassifizieren.|
|Entität|In der Stichprobe erkannte oder identifizierte Entität. Wenn eine Entität von der Rückschlussengine erkannt wird, erhält sie ein Tag. Außerdem werden zusätzliche Attribute, die abgeleitet wurden, sowie die Koordinaten eines rechteckigen Felds um die gefundene Entität zurückgegeben.|
|Ereignis|In der Stichprobe erkanntes Ereignis. Wenn in der Stichprobe ein Ereignis erkannt wird, werden der Name des Ereignisses und die ereignisspezifischen Eigenschaften zurückgegeben.|
|Bewegung|In der Stichprobe erkannte Bewegung. Wenn in der Stichprobe eine Bewegung erkannt wird, werden die Koordinaten eines umgebenden Rechtecks für die Position zurückgegeben, an der die Bewegung erkannt wurde.|
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

- [gRPC-Datenvertrag](grpc-data-contract.md)
- [HTTP-Datenvertrag](http-data-contract.md)
