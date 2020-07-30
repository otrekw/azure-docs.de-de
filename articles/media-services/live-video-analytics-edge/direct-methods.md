---
title: 'Verwenden von direkten Methoden in Live Video Analytics in IoT Edge: Azure'
description: Live Video Analytics in IoT Edge stellt verschiedene direkte Methoden zur Verfügung. Die direkten Methoden basieren auf den in diesem Thema beschriebenen Konventionen.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: ed7cec7b8513044c2bf9b24600b8d9f42a485aae
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87091826"
---
# <a name="direct-methods"></a>Direkte Methoden

Live Video Analytics in IoT Edge stellt verschiedene direkte Methoden zur Verfügung, die von IoT Hub aufgerufen werden können. Direkte Methoden stellen eine Anforderung-Antwort-Interaktion mit einem Gerät dar, die einem HTTP-Aufruf darin ähnelt, dass sie unverzüglich (nach einem vom Benutzer angegebenen Timeout) zu einem Erfolg oder Fehler führt. Dieser Ansatz eignet sich für Szenarien, in denen die Vorgehensweise bei sofortigen Aktionen unterschiedlich ist, je nachdem, ob das Gerät reagieren konnte oder nicht. Weitere Informationen finden Sie unter [Verstehen und Aufrufen direkter Methoden von IoT Hub](../../iot-hub/iot-hub-devguide-direct-methods.md).

In diesem Thema werden diese Methoden und Konventionen beschrieben.

## <a name="conventions"></a>Konventionen

Die direkten Methoden basieren auf den folgenden Konventionen:

1. Direkte Methoden verfügen über eine im Format MAJOR.MINOR (Hauptversion.Nebenversion) angegebene Version (wie im folgenden Beispiel gezeigt):

    ```
    {
        "methodName": "GraphTopologySet",
        "payload": {
            // API version matches matches module MAJOR.MINOR version
            "@apiVersion": "1.0"
            //payload here…
        }
    }
    ```

2. Eine bestimmte Version von Live Video Analytics im IoT Edge-Modul unterstützt alle direkten Methoden bis zu ihrer aktuellen Version. Beispielsweise unterstützt die Modulversion 1.3 direkte Methoden der Versionen 1.3, 1.2, 1.1 und 1.0.
3. Alle direkten Methoden sind synchron.
4. Fehlerergebnisse folgen dem OData-Fehlerschema.
5. Bei Namen sollten die folgenden Einschränkungen beachtet werden:
    
    * Nur alphanumerische Zeichen und Bindestriche, sofern ein Name nicht mit einem Bindestrich beginnt oder endet
    * Keine Leerzeichen
    * Maximal 32 Zeichen

### <a name="example"></a>Beispiel

```
{
  "status": 400,
  "payload": {
    "error": {
      "code": "BadRequest",
      "message": "Graph instance is not found."
    }
  }
}
```    

### <a name="top-level-error-codes"></a>Fehlercodes der obersten Ebene     

|Status |Code   |`Message`|
|---|---|---|
|400|   BadRequest| Request is not valid (Anforderung ist ungültig)|
|400|   InvalidResource|    Die Ressource ist ungültig.|
|400|   InvalidVersion| Die API-Version ist ungültig.|
|402|   ConnectivityRequired    |Das Edge-Modul erfordert Cloudkonnektivität für die ordnungsgemäße Funktion.|
|404|   NotFound    |Ressource wurde nicht gefunden.|
|409|   Konflikt:|   Vorgangskonflikt|
|500|   InternalServerError|    Interner Serverfehler|
|503|   ServerBusy| Der Server ist ausgelastet.|

### <a name="detailed-error-codes"></a>Detaillierte Fehlercodes

Ausführliche Überprüfungsfehler, wie etwa Überprüfungen des Graphmoduls, werden als Fehlerdetails hinzugefügt:

```
{
  "status": 400,
  "payload": {
    "error": {
      "code": "InvalidResource",
      "message": "The resource format is invalid.",
      "details": [
        {
          "code": "RtspEndpointUrlInvalidScheme",
          "target": "$.Properties.Sources[0]",
          "message": "Uri scheme 'http' is not valid for an RTSP source endpoint. Valid values are: rtsp"
        },
        {
          "code": "PropertyValueInvalidPattern",
          "target": "$.Properties.Sinks[0].AssetNamePattern",
          "message": "The value must match the regular expression '^[^<>%&:\\\\\\/?*+.']{1,260}$'"
        }
      ]
    }
  }
}
}
```

|Status|    Detailcode   |BESCHREIBUNG|
|---|---|---|
|400|   GraphValidationError|   Allgemeine Graphfehler, wie etwa Zyklen oder Partitionierung usw.|
|400|   ModuleValidationError|  Modulspezifische Überprüfungsfehler.|
|409|   GraphTopologyInUse| Auf die Graphtopologie wird noch von mindestens einer Graphinstanz verwiesen.|
|409|   OperationNotAllowedInState| Der angeforderte Vorgang kann im aktuellen Zustand nicht ausgeführt werden.|
|409|   ResourceValidationError|    Die referenzierte Ressource (Beispiel: Medienobjekt) befindet sich nicht in einem gültigen Zustand.|

## <a name="details"></a>Details  

### <a name="graphtopologyget"></a>GraphTopologyGet

Diese direkte Methode ruft eine einzelne Graphtopologie ab.

#### <a name="request"></a>Anforderung

```
{
    "methodName": "GraphTopologyGet",
    "payload": {
        "@apiVersion": "1.0",
        "name": "CaptureAndRecord"
    }
}
```

#### <a name="response"></a>Antwort

```
{
    "status": 200,
    "payload": {
        "name": "CaptureAndRecord",
        "properties": {
            // Complete graph topology
        }
    }
}
```

#### <a name="status-codes"></a>Statuscodes

|Bedingung  |Statuscode    |Detaillierter Fehlercode|
|---|---|---|
|Entität gefunden|  200 |–
|Allgemeine Benutzerfehler    |400er Bereich  ||
|Entität nicht gefunden   |404        ||
|Allgemeine Serverfehler| 500er Bereich       ||

### <a name="graphtopologyset"></a>GraphTopologySet

Erstellt eine einzelne Topologie, wenn keine mit dem angegebenen Namen vorhanden ist, oder aktualisiert eine vorhandene Topologie mit dem gleichen Namen.

Schlüsselaspekte:

* Die Topologie kann frei aktualisiert werden, sofern kein Graph auf sie verweist.
* Die Topologie kann frei aktualisiert werden, wenn alle auf sie verweisenden Graphen deaktiviert sind, sofern:

    * Neu hinzugefügte Parameter Standardwerte aufweisen
    * Auf entfernte Parameter von keinem Graph verwiesen wird
* Topologieaktualisierungen sind nicht zulässig, wenn aktive Graphen vorhanden sind

#### <a name="request"></a>Anforderung

```
{
    "methodName": "GraphTopologySet",
    "payload": {
        "@apiVersion": "1.0",
        "name": "CaptureAndRecord",
        "properties": {
            // Desired graph topology properties
        }
    }
}
```

#### <a name="response"></a>Antwort

```
{
    "status": 201,
    "payload": {
        "name": " CaptureAndRecord ",
        "properties": {
            // Complete graph topology
        }
    }
}
```

#### <a name="status-codes"></a>Statuscodes

|Bedingung  |Statuscode    |Detaillierter Fehlercode|
|---|---|---|
Vorhandene Entität aktualisiert |200|   –|
Neue Entität erstellt  |201|   –|
Allgemeine Benutzerfehler |400er Bereich  ||
Graphüberprüfungsfehler |400    |GraphValidationError|
Modulüberprüfungsfehler|   400 |ModuleValidationError|
Allgemeine Serverfehler   |500er Bereich  ||

### <a name="graphtopologydelete"></a>GraphTopologyDelete

Löscht eine einzelne Graphtopologie.

#### <a name="request"></a>Anforderung

```
{
    "methodName": "GraphTopologyDelete",
    "payload": {
        "@apiVersion": "1.0",
        "name": "CaptureAndRecord"
    }
}
```

#### <a name="response"></a>Antwort

```
{
    "status": 200,
    "payload": null
}
```

#### <a name="status-codes"></a>Statuscodes

|Bedingung  |Statuscode    |Detaillierter Fehlercode|
|---|---|---|
|Entität gelöscht|    200|    –|
|Entität nicht gefunden|  204|    –|
|Allgemeine Benutzerfehler|   400er Bereich   ||
|Auf die Graphtopologie wird von mindestens einer Graphinstanz verwiesen.| 409 |GraphTopologyInUse|
|Allgemeine Serverfehler| 500er Bereich   ||

### <a name="graphtopologylist"></a>GraphTopologyList

Ruft eine Liste aller Graphtopologien ab, die den Filterkriterien entsprechen.

#### <a name="request"></a>Anforderung

```
{
    "methodName": "GraphTopologyList",
    "payload": {
        // Required
        "@apiVersion": "1.0",
        
        // Optional
        "@continuationToken": "xxxxx",    
        "@query": "$orderby=name asc"
    }
}
```

#### <a name="response"></a>Antwort

```
{
    "status": 200,
    "payload": {
        "@continuationToken": "xxxx",
        "value": [
            {
                "name": "Capture & Record",
                // ...
            },
            {
                "name": "Capture, Analyze & Record",
                // ...
            }
        ]
    }
}
```

#### <a name="filter-support"></a>Filterunterstützung

|Vorgang      |Feld(er)   |Operatoren|
|---|---|---|
|$orderby|name  |asc|


#### <a name="status-codes"></a>Statuscodes

|Bedingung  |Statuscode    |Detaillierter Fehlercode|
|---|---|---|
|Erfolg|   200 |–|
|Allgemeine Benutzerfehler|   400er Bereich   ||
|Allgemeine Serverfehler| 500er Bereich   ||

### <a name="graphinstanceget"></a>GraphInstanceGet

Ruft eine einzelne Graphinstanz ab:

#### <a name="request"></a>Anforderung

```
{
    "methodName": "GraphInstanceGet",
    "payload": {
        "@apiVersion": "1.0",
        "name": "Camera001"
    }
}
```

#### <a name="response"></a>Antwort

```
{
    "status": 200,
    "payload": {
        "name": "Camera001",
        "properties": {
            // Complete graph instance
        }
    }
}
```

#### <a name="status-codes"></a>Statuscodes

|Bedingung  |Statuscode    |Detaillierter Fehlercode|
|---|---|---|
|Entität gefunden   |200|   –|
|Allgemeine Benutzerfehler|   400er Bereich   ||
|Entität nicht gefunden|  404 ||
|Allgemeine Serverfehler| 500er Bereich   ||

### <a name="graphinstanceset"></a>GraphInstanceSet

Erstellt eine einzelne Graphinstanz, wenn keine mit dem angegebenen Namen vorhanden ist, oder aktualisiert eine vorhandene Graphinstanz mit dem gleichen Namen.

Schlüsselaspekte:

* Eine Graphinstanz kann frei aktualisiert werden, während sie sich im Zustand „Deaktiviert“ befindet.

    * Der Graph wird bei jeder Aktualisierung neu überprüft.
* Aktualisierungen von Graphinstanzen sind teilweise eingeschränkt, während sich der Graph nicht im Zustand „inaktiv“ befindet.
* Aktualisierungen von Graphinstanzen sind für aktive Graphen nicht zulässig.

#### <a name="request"></a>Anforderung

```
{
"methodName": "GraphInstanceSet",
    "payload": {
        "@apiVersion": "1.0",
        "name": "Camera001",
        "properties": {
            // Desired graph instance properties
        }
    }
}
```

#### <a name="response"></a>Antwort

````
{
    "status": 201,
    "payload": {
        "name": " Camera001",
        "properties": {
            // Complete graph instance
        }   
    }
}
````

#### <a name="status-codes"></a>Statuscodes

|Bedingung  |Statuscode    |Detaillierter Fehlercode|
|---|---|---|
|Vorhandene Entität aktualisiert    |200    |–|
|Neue Entität erstellt|    201 |–|
|Allgemeine Benutzerfehler|   400er Bereich   ||
|Graphüberprüfungsfehler    |400|   GraphValidationError|
|Modulüberprüfungsfehler|  400 |ModuleValidationError|
|Ressourcenüberprüfungsfehler |409    |ResourceValidationError|
|Allgemeine Serverfehler  |500er Bereich||    

### <a name="graphinstancedelete"></a>GraphInstanceDelete

Löscht eine einzelne Graphinstanz.

Schlüsselaspekte:

* Nur deaktivierte Graphen können gelöscht werden.

#### <a name="request"></a>Anforderung

```
{
    "methodName": "GraphInstanceDelete",
    "payload": {
        "@apiVersion": "1.0",
        "name": "Camera001"
    }
}
```

#### <a name="response"></a>Antwort

```
{
    "status": 200,
    "payload": null
}
```

#### <a name="status-codes"></a>Statuscodes

|Bedingung  |Statuscode    |Detaillierter Fehlercode|
|---|---|---|
|Graph erfolgreich gelöscht|    200|    –|
|Graph nicht gefunden|   204|    –|
|Allgemeine Benutzerfehler    |400er Bereich  ||
|Der Graph befindet sich nicht im Zustand „Beendet“    |409    |OperationNotAllowedInState|
|Allgemeine Serverfehler| 500er Bereich   ||

### <a name="graphinstancelist"></a>GraphInstanceList

Ähnlich der GraphTopologyList. Ermöglicht Benutzern die Enumeration der Graphinstanzen.
Ruft eine Liste aller Graphinstanzen ab, die den Filterkriterien entsprechen.

#### <a name="request"></a>Anforderung

```
{
    "methodName": "GraphInstanceList",
    "payload": {
        // Required
        "@apiVersion": "1.0",
        
        // Optional
        "@continuationToken": "xxxxx",    
        "@query": "$orderby=name asc"
    }
}
```

#### <a name="response"></a>Antwort

```
{
    "status": 200,
    "payload": {
        "@continuationToken": "xxxx",
        "value": [
            {
                "name": "Capture & Record",
                // ...
            },
            {
                "name": "Capture, Analyze & Record",
                // ...
            }
        ]
    }
}
```

#### <a name="filter-support"></a>Filterunterstützung

|Vorgang  |   Feld(er)|   Operatoren|
|---|---|---|
|$orderby|  name|   asc|

#### <a name="status-codes"></a>Statuscodes

|Bedingung  |Statuscode    |Detaillierter Fehlercode|
|---|---|---|
|Erfolg    |200    |–|
|Allgemeine Benutzerfehler|   400er Bereich   ||
|Allgemeine Serverfehler| 500er Bereich   ||

### <a name="graphinstanceactivate"></a>GraphInstanceActivate

Aktiviert eine einzelne Graphinstanz. 

Schlüsselaspekte

* Die Methode gibt nur zurück, wenn der Graph aktiviert ist 
* Der Graph nimmt den Zustand „Wird aktiviert“ an, während die Aktivierung erfolgt.

    * Ein List/Get-Vorgang für den Graph würde den Graph im richtigen Zustand zurückgeben.
* Idempotenz:

    * Das Starten eines Graphs im Status „Wird aktiviert“ zeigt das gleiche Verhalten wie bei einem deaktivierten Graph (das heißt: Aufrufe werden blockiert, bis der Graph aktiviert ist)
    * Das Aktivieren eines Graphs im Zustand „Aktiv“ bewirkt eine sofortige erfolgreiche Rückgabe.

#### <a name="request"></a>Anforderung

```
{
    "methodName": "GraphInstanceActivate",
    "payload": {
        "@apiVersion": "1.0",
        "name": "Camera001"
    }
}
```

#### <a name="response"></a>Antwort

```
{
    "status": 200,
    "payload": null
}
```

#### <a name="status-codes"></a>Statuscodes

|Bedingung  |Statuscode    |Detaillierter Fehlercode|
|---|---|---|
|Graph erfolgreich aktiviert   |200    |–|
|Neue Entität erstellt |201|   –|
|Allgemeine Benutzerfehler    |400er Bereich  ||
|Modulüberprüfungsfehler   |400|   ModuleValidationError|
|Ressourcenüberprüfungsfehler|    409|    ResourceValidationError|
|Der Graph befindet sich im Zustand „Wird deaktiviert“. |409    |OperationNotAllowedInState|
|Allgemeine Serverfehler| 500er Bereich   ||

### <a name="graphinstancedeactivate"></a>GraphInstanceDeactivate

Deaktiviert eine einzelne Graphinstanz. Beim ordnungsgemäßen Deaktivieren eines Graphs wird die Medienverarbeitung deaktiviert und sichergestellt, dass alle vorübergehend auf dem Edge gespeicherten Ereignisse und Medien sofern möglich in die Cloud übernommen werden.

Schlüsselaspekte:

* Die Methode gibt nur zurück, wenn der Graph deaktiviert ist
* Der Graph nimmt den Zustand „Wird deaktiviert“ an, während die Deaktivierung erfolgt.

    * Ein List/Get-Vorgang für den Graph würde den Graph im richtigen Zustand zurückgeben.
    * Beenden wird nur abgeschlossen, wenn alle Medien in die Cloud hochgeladen wurden.
* Idempotenz:

    * Das Deaktivieren eines Graphs im Status „Wird deaktiviert“ zeigt das gleiche Verhalten wie bei einem deaktivierten Graph (das heißt: Aufrufe werden blockiert, bis der Graph deaktiviert ist)
    * Das Deaktivieren eines Graphs im Zustand „Inaktiv“ bewirkt eine sofortige erfolgreiche Rückgabe.

#### <a name="request"></a>Anforderung

```
{
    "methodName": "GraphInstanceDeactivate",
    // A high response timeout is recommended here since there might be data movement    // being executed as part of this operation
    "responseTimeoutInSeconds": 300,
    "payload": {
        "@apiVersion": "1.0",
        "name": "Camera001"
    }
}
```

#### <a name="response"></a>Antwort

```
{
    "status": 200,
    "payload": null
}
```

|Bedingung  |Statuscode    |Detaillierter Fehlercode|
|---|---|---|
|Graph erfolgreich aktiviert   |200|   –|
|Neue Entität erstellt |201|   –|
|Allgemeine Benutzerfehler    |400er Bereich  ||
|Der Graph befindet sich im Zustand „Wird aktiviert“.   |409|   OperationNotAllowedInState|
|Allgemeine Serverfehler  |500er Bereich  ||

## <a name="next-steps"></a>Nächste Schritte

[Konfigurationsschema für Modulzwillinge](module-twin-configuration-schema.md)
