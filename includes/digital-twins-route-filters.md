---
author: baanders
description: Includedatei für Routingfilteroptionen für Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 12/04/2020
ms.author: baanders
ms.openlocfilehash: d93f484e318c10489eb1db3e9c65c6e0c7479c90
ms.sourcegitcommit: fc8ce6ff76e64486d5acd7be24faf819f0a7be1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98859622"
---
| Filtername | BESCHREIBUNG | Filtertextschema | Unterstützte Werte | 
| --- | --- | --- | --- |
| True/False | Ermöglicht das Erstellen einer Route ohne Filterung oder das Deaktivieren einer Route, sodass keine Ereignisse gesendet werden. | `<true/false>` | `true` = Route ist ohne Filterung aktiviert. <br> `false` = Route ist deaktiviert. |
| type | Der [Ereignistyp](../articles/digital-twins/concepts-route-events.md#types-of-event-messages), der durch Ihre digitale Zwillingsinstanz fließt. | `type = '<eventType>'` | Folgende Werte für Ereignistypen sind möglich: <br>`Microsoft.DigitalTwins.Twin.Create` <br> `Microsoft.DigitalTwins.Twin.Delete` <br> `Microsoft.DigitalTwins.Twin.Update`<br>`Microsoft.DigitalTwins.Relationship.Create`<br>`Microsoft.DigitalTwins.Relationship.Update`<br> `Microsoft.DigitalTwins.Relationship.Delete` <br> `microsoft.iot.telemetry`  |
| `Source` | Der Name der Azure Digital Twins-Instanz. | `source = '<hostname>'`| Folgende Werte für Hostnamen sind möglich: <br> **Für Benachrichtigungen**: `<yourDigitalTwinInstance>.api.<yourRegion>.digitaltwins.azure.net` <br> **Für Telemetrie**: `<yourDigitalTwinInstance>.api.<yourRegion>.digitaltwins.azure.net/<twinId>`|
| Subject | Eine Beschreibung des Ereignisses im Zusammenhang mit der oben genannten Ereignisquelle. | `subject = '<subject>'` | Folgende Werte für den Betreff sind möglich: <br>**Für Benachrichtigungen**: Der Betreff ist `<twinid>` <br> oder ein URI-Format für Betreffelemente, die durch mehrere Teile oder IDs eindeutig identifiziert werden:<br>`<twinid>/relationships/<relationshipid>`<br> **Für Telemetrie**: Der Betreff ist der Komponentenpfad (wenn die Telemetrie von einer Zwillingskomponente ausgegeben wird). Beispiel: `comp1.comp2`. Wenn die Telemetrie nicht von einer Komponente ausgegeben wird, dann ist ihr Betrefffeld leer. |
| Datenschema | DTDL-Modell-ID | `dataschema = '<model-dtmi-ID>'` | **Für Telemetrie**: Das Datenschema ist die Modell-ID des Zwillings oder der Komponente, der bzw. die die Telemetrie ausgibt. Zum Beispiel, `dtmi:example:com:floor4;2` <br>**Für Benachrichtigungen (Erstellen/Löschen)** : Der Zugriff auf das Datenschema kann im Benachrichtigungstext unter `$body.$metadata.$model` erfolgen. <br>**Für Benachrichtigungen (Aktualisieren)** : Der Zugriff auf das Datenschema kann im Benachrichtigungstext unter `$body.modelId` erfolgen.|
| Inhaltstyp | Der Inhaltstyp des Datenwerts. | `datacontenttype = '<contentType>'` | Der Inhaltstyp lautet `application/json`. |
| Spezifikationsversion | Die Version des von Ihnen verwendeten Ereignisschemas. | `specversion = '<version>'` | Die Version muss `1.0` sein. Dies gibt das CloudEvents-Schema, Version 1.0, an. |
| Benachrichtigungstext | Verweisen auf eine beliebige Eigenschaft im Feld `data` einer Benachrichtigung. | `$body.<property>` | Weitere Informationen finden Sie unter [*Vorgehensweise: Informationen zu Ereignisdaten*](../articles/digital-twins/how-to-interpret-event-data.md) für Beispiele von Benachrichtigungen. Auf jede Eigenschaft im Feld `data` kann mit `$body` verwiesen werden.

Die folgenden Datentypen werden als Werte unterstützt, die von Verweisen auf die oben aufgeführten Daten zurückgegeben werden:

| Datentyp | Beispiel |
|-|-|-|
|**String**| `STARTS_WITH($body.$metadata.$model, 'dtmi:example:com:floor')` <br> `CONTAINS(subject, '<twinID>')`|
|**Integer**|`$body.errorCode > 200`|
|**Double**|`$body.temperature <= 5.5`|
|**Bool**|`$body.poweredOn = true`|
|**NULL**|`$body.prop != null`|

Die folgenden Operatoren werden beim Definieren von Routenfiltern unterstützt:

|Familie|Operatoren|Beispiel|
|-|-|-|
|Logisch|AND, OR, ( )|`(type != 'microsoft.iot.telemetry' OR datacontenttype = 'application/json') OR (specversion != '1.0')`|
|Vergleich|<, <=, >, >=, =, !=|`$body.temperature <= 5.5`

Die folgenden Funktionen werden beim Definieren von Routenfiltern unterstützt:

|Funktion|BESCHREIBUNG|Beispiel|
|--|--|--|
|STARTS_WITH(x,y)|Gibt TRUE zurück, wenn der Wert `x` mit der Zeichenfolge `y` beginnt.|`STARTS_WITH($body.$metadata.$model, 'dtmi:example:com:floor')`|
|ENDS_WITH(x,y) | Gibt TRUE zurück, wenn der Wert `x` mit der Zeichenfolge `y` endet.|`ENDS_WITH($body.$metadata.$model, 'floor;1')`|
|CONTAINS(x,y)| Gibt TRUE zurück, wenn der Wert `x` die Zeichenfolge `y` enthält.|`CONTAINS(subject, '<twinID>')`|

Wenn Sie einen Filter implementieren oder aktualisieren, kann es einige Minuten dauern, bis sich die Änderung in der Datenpipeline widerspiegelt.