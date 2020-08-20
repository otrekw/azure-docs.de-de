---
author: baanders
description: Includedatei für Routingfilteroptionen für Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 8/3/2020
ms.author: baanders
ms.openlocfilehash: c5b78d80b1aa958af50b81dc152b4a746ce85f70
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87902193"
---
| Filtername | BESCHREIBUNG | Filtertextschema | Unterstützte Werte | 
| --- | --- | --- | --- |
| True/False | Ermöglicht das Erstellen einer Route ohne Filterung oder das Deaktivieren einer Route, sodass keine Ereignisse gesendet werden. | `<true/false>` | `true` = Route ist ohne Filterung aktiviert. <br> `false` = Route ist deaktiviert. |
| type | Der [Ereignistyp](../articles/digital-twins/concepts-route-events.md#types-of-event-messages), der durch Ihre digitale Zwillingsinstanz fließt. | `type = '<eventType>'` | Folgende Werte für Ereignistypen sind möglich: <br>`Microsoft.DigitalTwins.Twin.Create` <br> `Microsoft.DigitalTwins.Twin.Delete` <br> `Microsoft.DigitalTwins.Twin.Update`<br>`Microsoft.DigitalTwins.Relationship.Create`<br>`Microsoft.DigitalTwins.Relationship.Update`<br> `Microsoft.DigitalTwins.Relationship.Delete` <br> `microsoft.iot.telemetry`  |
| `Source` | Der Name der Azure Digital Twins-Instanz. | `source = '<hostname>'`| Folgende Werte für Hostnamen sind möglich: <br> **Für Benachrichtigungen**: `<yourDigitalTwinInstance>.<yourRegion>.azuredigitaltwins.net` <br> **Für Telemetrie**: `<yourDigitalTwinInstance>.<yourRegion>.azuredigitaltwins.net/digitaltwins/<twinId>`|
| Subject | Eine Beschreibung des Ereignisses im Zusammenhang mit der oben genannten Ereignisquelle. | `subject = '<subject>'` | Folgende Werte für den Betreff sind möglich: <br>**Für Benachrichtigungen**: Der Betreff ist `<twinid>` <br> oder ein URI-Format für Betreffelemente, die durch mehrere Teile oder IDs eindeutig identifiziert werden:<br>`<twinid>/relationships/<relationshipid>`<br> **Für Telemetrie**: Der Betreff ist der Komponentenpfad (wenn die Telemetrie von einer Zwillingskomponente ausgegeben wird). Beispiel: `comp1.comp2`. Wenn die Telemetrie nicht von einer Komponente ausgegeben wird, dann ist ihr Betrefffeld leer. |
| Datenschema | DTDL-Modell-ID | `dataschema = '<model-dtmi-ID>'` | **Für Telemetrie**: Das Datenschema ist die Modell-ID des Zwillings oder der Komponente, der bzw. die die Telemetrie ausgibt. Zum Beispiel, `dtmi:example:com:floor4;2` <br>**Für Benachrichtigungen**: Das Datenschema wird nicht unterstützt.|
| Inhaltstyp | Der Inhaltstyp des Datenwerts. | `datacontenttype = '<contentType>'` | Der Inhaltstyp lautet `application/json`. |
| Spezifikationsversion | Die Version des von Ihnen verwendeten Ereignisschemas. | `specversion = '<version>'` | Die Version muss `1.0` sein. Dies gibt das CloudEvents-Schema, Version 1.0, an. |

Es ist auch möglich, Filter mit den folgenden Vorgängen zu kombinieren:

| Filtername | Filtertextschema | Beispiel | 
| --- | --- | --- |
| AND/OR | `<filter1> AND <filter2>` | `type != 'microsoft.iot.telemetry' AND datacontenttype = 'application/json'` |
| AND/OR | `<filter1> OR <filter2>` | `type != 'microsoft.iot.telemetry' OR datacontenttype = 'application/json'` |
| Geschachtelte Vorgänge | `(<Comparison1>) AND (<Comparison2>)` | `(type != 'microsoft.iot.telemetry' OR datacontenttype = 'application/json') OR (specversion != '1.0')` |

> [!NOTE]
> Während der Vorschau wird nur die Zeichenfolgengleichheit unterstützt (=, !=).

Wenn Sie einen Filter implementieren oder aktualisieren, kann es einige Minuten dauern, bis sich die Änderung in der Datenpipeline widerspiegelt.
