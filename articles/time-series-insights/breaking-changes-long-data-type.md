---
title: Hinzufügen von Unterstützung für den Long-Datentyp | Microsoft-Dokumentation
description: Unterstützung für den Long-Datentyp
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.workload: big-data
ms.topic: conceptual
ms.date: 07/07/2020
ms.custom: dpalled
ms.openlocfilehash: c31ca7fd3eca89159d583b8a51b59a7bd6b8ed67
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86527992"
---
# <a name="adding-support-for-long-data-type"></a>Hinzufügen von Unterstützung für den Long-Datentyp

Diese Änderungen werden nur auf Gen2-Umgebungen angewendet. Wenn Sie eine Gen1-Umgebung verwenden, können Sie diese Änderungen ignorieren.

Wir nehmen Änderungen an der Weise vor, wie numerische Daten in Azure Time Series Insights Gen2 gespeichert und indiziert werden, die Auswirkungen für Sie haben können. Wenn Sie von einem der unten genannten Fälle betroffen sind, nehmen Sie die erforderlichen Änderungen so bald wie möglich vor. Abhängig von Ihrer Region wird die Indizierung Ihrer Daten als Long und Double zwischen dem 29. Juni und dem 30. Juni 2020 beginnen. Wenn Sie Fragen zu dieser Änderung oder Bedenken haben, reichen Sie ein Supportticket über das Azure-Portal ein, und beziehen Sie sich auf diese Kommunikation.

Diese Änderung betrifft Sie in den folgenden Fällen:

1. Wenn Sie aktuell Variablen des Zeitreihenmodells verwenden und in Ihren Telemetriedaten nur ganzzahlige Datentypen senden.
1. Wenn Sie aktuell Variablen des Zeitreihenmodells verwenden und in Ihren Telemetriedaten sowohl ganzzahlige als auch nicht ganzzahlige Datentypen senden.
1. Wenn Sie kategorische Variablen verwenden, um Kategorien ganzzahlige Werte zuzuordnen.
1. Wenn Sie das JavaScript SDK verwenden, um eine benutzerdefinierte Front-End-Anwendung zu erstellen.
1. Wenn Sie sich an den Grenzwert von 1.000 für Eigenschaftsnamen im warmen Speicher (WS) annähern und sowohl ganzzahlige als auch nicht ganzzahlige Daten senden, kann die Anzahl der Eigenschaften als Metrik im [Azure-Portal](https://portal.azure.com/) angezeigt werden.

Wenn einer der oben genannten Fälle auf Sie zutrifft, müssen Sie Änderungen an Ihrem Modell vornehmen, um dieser Änderung Rechnung zu tragen. Aktualisieren Sie den Zeitreihenausdruck in Ihrer Variablendefinition sowohl im Azure Time Series Insights Gen2-Explorer als auch in jedem benutzerdefinierten Client, der unsere APIs verwendet, mit den empfohlenen Änderungen. Weitere Informationen siehe unten.

Abhängig von ihrer IoT-Lösung und den Einschränkungen haben Sie möglicherweise keinen Einblick in die Daten, die an Ihre Azure Time Series Insights Gen2-Umgebung gesendet werden. Wenn Sie nicht sicher sind, ob Ihre Daten rein ganzzahlig oder aus ganzzahligen und nicht ganzzahligen zusammengesetzt sind, stehen Ihnen einige Optionen offen. Sie können auf die Veröffentlichung der Funktion warten und dann Ihre unformatierten Ereignisse auf der Benutzeroberfläche des Explorers untersuchen, um zu verstehen, welche Eigenschaften in zwei separaten Spalten gespeichert wurden. Sie könnten vorsorglich die unten aufgeführten Änderungen für alle numerischen Tags vornehmen oder vorübergehend eine Teilmenge von Ereignissen an den Speicher weiterleiten, um das Schema besser verstehen und untersuchen zu können. Zum Speichern von Ereignissen aktivieren Sie die [Ereigniserfassung](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview) für Event Hubs, oder Sie [routen](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c#azure-storage) sie von Ihrem IoT Hub an Azure Blob Storage. Daten können außerdem im [Event Hub-Explorer](https://marketplace.visualstudio.com/items?itemName=Summer.azure-event-hub-explorer) oder mithilfe des [Ereignisprozessorhosts](https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-send#receive-events) beobachtet werden. Wenn Sie IoT Hub verwenden, finden Sie Informationen zum Zugriff auf den integrierten Endpunkt [hier](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-read-builtin).

Beachten Sie, dass, falls Sie von diesen Änderungen betroffen sind und bis zu den oben genannten Terminen nicht entsprechend reagieren können, eine Unterbrechung auftreten kann, während der die betroffenen Zeitreihenvariablen beim Zugriff über die Abfrage-APIs oder den Time Series Insights-Explorer *NULL* zurückgeben (d. h. keine Daten im Explorer anzeigen).

## <a name="recommended-changes"></a>Empfohlene Änderungen

Fälle 1 und 2: **Verwenden von Variablen des Zeitreihenmodells, wenn nur ganzzahlige Datentypen ODER sowohl ganzzahlige als auch nicht ganzzahlige Datentypen in den Telemetriedaten gesendet werden.**

Wenn Sie derzeit ganzzahlige Telemetriedaten senden, werden Ihre Daten in zwei Spalten aufgeteilt: „propertyValue_double“ und „propertyValue_long“.

Ihre ganzzahligen Daten werden in „propertyValue_long“ geschrieben, wenn die Änderungen wirksam werden, und zuvor erfasste (sowie zukünftig erfasste) numerische Daten in „propertyValue_double“ werden nicht herüberkopiert.

Wenn Sie Daten für die Eigenschaft „propertyValue“ übergreifend für diese zwei Spalten abfragen möchten, müssen Sie die Skalarfunktion *coalesce()* in Ihrem TSX verwenden. Die-Funktion akzeptiert Argumente des gleichen Datentyps und gibt den ersten Wert in der Argumentliste zurück, der nicht NULL ist (weitere Informationen zur Verwendung finden Sie [hier](https://docs.microsoft.com/rest/api/time-series-insights/preview#other-functions)).

### <a name="variable-definition-in-time-series-explorer---numeric"></a>Variablendefinition im Time Series Explorer: Numerisch

*Vorherige Variablendefinition:*

[![Vorherige Variablendefinition](media/time-series-insights-long-data-type/var-def-previous.png)](media/time-series-insights-long-data-type/var-def-previous.png#lightbox)

*Neue Variablendefinition:*

[![Neue Variablendefinition](media/time-series-insights-long-data-type/var-def.png)](media/time-series-insights-long-data-type/var-def.png#lightbox)

Sie können außerdem *„coalesce($event.propertyValue.Double, toDouble($event.propertyValue.Long))“* als benutzerdefinierten [Zeitreihenausdruck](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) verwenden.

### <a name="inline-variable-definition-using-time-series-query-apis---numeric"></a>Definition von Inlinevariablen mithilfe von Zeitreihenabfrage-APIs: Numerisch

*Vorherige Variablendefinition:*

```JSON
"PropertyValueVariable": {

    "kind": "numeric",

    "value": {

        "tsx": "$event.propertyValue.Double"

    },

    "filter": null,

    "aggregation": {

        "tsx": "avg($value)"
    }
}
```

*Neue Variablendefinition:*

```JSON
"PropertyValueVariable ": {

    "kind": "numeric",

    "value": {

        "tsx": "coalesce($event.propertyValue.Long, toLong($event.propertyValue.Double))"

    },

    "filter": null,

    "aggregation": {

        "tsx": "avg($value)"
    }
}
```

Sie können außerdem *„coalesce($event.propertyValue.Double, toDouble($event.propertyValue.Long))“* als benutzerdefinierten [Zeitreihenausdruck](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) verwenden.

> [!NOTE]
> Es empfiehlt sich, diese Variablen an allen Orten zu aktualisieren, an denen sie möglicherweise verwendet werden (Zeitreihenmodell, gespeicherte Abfragen, Power BI Connector-Abfragen).

Fall 3: **Verwenden von kategorischen Variablen, um Kategorien ganzzahlige Werte zuzuordnen**

Wenn Sie zurzeit kategorische Variablen verwenden, die Kategorien ganzzahlige Werte zuordnen, nutzen Sie wahrscheinlich die toLong-Funktion, um Daten aus dem Typ Double in den Typ Long zu konvertieren. Wie in den Fällen oben, müssen Sie die Datentypspalten Double und Long zusammenfügen.

### <a name="variable-definition-in-time-series-explorer---categorical"></a>Variablendefinition im Time Series Explorer: Kategorisch

*Vorherige Variablendefinition:*

[![Vorherige Variablendefinition](media/time-series-insights-long-data-type/var-def-cat-previous.png)](media/time-series-insights-long-data-type/var-def-cat-previous.png#lightbox)

*Neue Variablendefinition:*

[![Neue Variablendefinition](media/time-series-insights-long-data-type/var-def-cat.png)](media/time-series-insights-long-data-type/var-def-cat.png#lightbox)

Sie können außerdem *„coalesce($event.propertyValue.Double, toDouble($event.propertyValue.Long))“* als benutzerdefinierten [Zeitreihenausdruck](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) verwenden.

Kategorische Variablen erfordern weiterhin einen ganzzahligen Werttyp. Der Datentyp aller Argumente in coalesce() im benutzerdefinierten [Zeitreihenausdruck](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) muss Long sein.

### <a name="inline-variable-definition-using-time-series-query-apis---categorical"></a>Definition von Inlinevariablen mithilfe von Zeitreihenabfrage-APIs: Kategorisch

*Vorherige Variablendefinition:*

```JSON
"PropertyValueVariable_Long": {

    "kind": "categorical",

    "value": {

        "tsx": "tolong($event.propertyValue.Double)"

    },

    "categories": [

    {
        "label": "Good",

        "values": [0, 1, 2 ]

    },

    {

        "label": "Bad",

        "values": [ 3, 4 ]

    } ],

    "defaultCategory": {

        "label": "Unknown"

    }
}
```

*Neue Variablendefinition:*

```JSON
"PropertyValueVariable_Long": {

    "kind": "categorical",

    "value": {

        "tsx": "coalesce($event.propertyValue.Long, tolong($event.propertyValue.Double))"

    },

    "categories": [

    {
        "label": "Good",

        "values": [0, 1, 2 ]

    },

    {

        "label": "Bad",

        "values": [ 3, 4 ]

    } ],

    "defaultCategory": {

        "label": "Unknown"

    }
}
```

Kategorische Variablen erfordern weiterhin einen ganzzahligen Werttyp. Der Datentyp aller Argumente in coalesce() im benutzerdefinierten [Zeitreihenausdruck](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) muss Long sein.

> [!NOTE]
> Es empfiehlt sich, diese Variablen an allen Orten zu aktualisieren, an denen sie möglicherweise verwendet werden (Zeitreihenmodell, gespeicherte Abfragen, Power BI Connector-Abfragen).

Fall 4: **Verwenden des JavaScript SDK zum Erstellen einer benutzerdefinierten Front-End-Anwendung**

Wenn Sie von den oben dargestellten Fällen 1–3 betroffen sind und benutzerdefinierte Anwendungen erstellen, müssen Sie Ihre Abfragen für die Verwendung der Funktion *coalesce()* aktualisieren, wie in den Beispielen oben gezeigt.

Fall 5: **Annäherung an den Grenzwert von 1.000 Eigenschaften im warmen Speicher**

Wenn Sie Benutzer eines warmen Speichers mit einer großen Anzahl von Eigenschaften sind und Grund zu der Annahme haben, dass diese Änderung Ihre Umgebung über den WS-Grenzwert von 1.000 Eigenschaftsnamen bringen würde, reichen Sie über das Azure-Portal ein Supportticket ein, und beziehen Sie sich auf diese Kommunikation.

## <a name="next-steps"></a>Nächste Schritte

* Die vollständige Liste der unterstützten Datentypen finden Sie unter [Unterstützte Datentypen](concepts-supported-data-types.md).
