---
title: Ereignis zur Autoskalierung von Azure Batch-Pools
description: Referenz zum Ereignis zur Autoskalierung von Batch-Pools, das ausgelöst wird, sobald die automatische Skalierung eines Pools ausgeführt wird. Der Inhalt des Protokolls macht die Formel für die automatische Skalierung und die Auswertungsergebnisse für den Pool verfügbar.
ms.topic: reference
ms.date: 10/08/2020
ms.openlocfilehash: e548ed484e5f683a8f79434ce4095ac66900f01a
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91852071"
---
# <a name="pool-autoscale-event"></a>Ereignis zur Autoskalierung von Pools

 Dieses Ereignis wird ausgelöst, sobald die automatische Skalierung eines Pools ausgeführt wird. Der Inhalt des Protokolls macht die Formel für die automatische Skalierung und die Auswertungsergebnisse für den Pool verfügbar.

 Das folgende Beispiel zeigt den Text eines Ereignisses zur Autoskalierung eines Pools für eine automatische Poolskalierung, bei der aufgrund unzureichender Beispieldaten ein Fehler aufgetreten ist.

```
{
    "id": "myPool1",
    "timestamp": "2020-09-21T18:59:56.204Z",
    "formula": "...",
    "results": "...",
    "error": {
        "code": "InsufficientSampleData",
        "message": "Autoscale evaluation failed due to insufficient sample data",
        "values": [{
                "name": "Message",
                "value": "Line 15, Col 44: Insufficient data from data set: $RunningTasks wanted 70%, received 50%"
            }
        ]
    }
}
```

|Element|type|Notizen|
|-------------|----------|-----------|
|`id`|String|Die ID des Pools.|
|`timestamp`|Datetime|Der Zeitstempel der Ausführung der automatischen Skalierung.|
|`formula`|Zeichenfolge|Die Formel, die für die automatische Skalierung definiert ist.|
|`results`|Zeichenfolge|Auswertungsergebnisse für alle Variablen, die in der Formel verwendet werden.|
|[`error`](#error)|Komplexer Typ|Die ausführliche Fehlermeldung für die automatische Skalierung.|

###  <a name="error"></a><a name="error"></a> Fehler

|Elementname|type|Notizen|
|------------------|----------|-----------|
|`code`|String|Ein Bezeichner für den Fehler bei der automatischen Skalierung. Codes sind unveränderlich und sollten programmgesteuert verwendet werden.|
|`message`|Zeichenfolge|Eine Meldung, die den Fehler bei der automatischen Skalierung beschreibt und für die Anzeige auf einer Benutzeroberfläche geeignet sein sollte.|
|`values`|Array|Liste von Name-Wert-Paaren, die weitere Details des Fehlers bei der automatischen Skalierung beschreiben.|
