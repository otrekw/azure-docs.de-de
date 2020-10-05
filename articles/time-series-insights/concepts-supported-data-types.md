---
title: Unterstützte Datentypen – Azure Time Series Insights Gen2 | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über die unterstützten Datentypen in Azure Time Series Insights Gen2.
author: lyrana
ms.author: lyhughes
manager: deepakpalled
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 08/31/2020
ms.openlocfilehash: 9dbc66e3331325e9b79d0434fb452d01d69d550a
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2020
ms.locfileid: "89482588"
---
# <a name="supported-data-types"></a>Unterstützte Datentypen

In der folgenden Tabelle sind die von Azure Time Series Insights Gen2 unterstützten Datentypen aufgeführt.

| Datentyp | BESCHREIBUNG | Beispiel | [Syntax des Zeitreihenausdrucks](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax) | Spaltenname für Eigenschaft in Parquet
|---|---|---|---|---|
| **bool** | Ein Datentyp, der diese beiden Zustände aufweisen kann: `true` oder `false`. | `"isQuestionable" : true` | `$event.isQuestionable.Bool` oder `$event['isQuestionable'].Bool` | `isQuestionable_bool`
| **datetime** | Stellt einen Zeitpunkt dar, der üblicherweise als Datum und Uhrzeit ausgedrückt wird. Wird im Format [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html) ausgedrückt. Datetime-Eigenschaften werden immer im UTC-Format gespeichert. Zeitzonenverschiebungen werden, sofern korrekt formatiert, angewendet, und der resultierende Wert wird in UTC gespeichert. Weitere Informationen über die Zeitstempel-Eigenschaft einer Umgebung und die Datetime-Verschiebungen finden Sie in [diesem](concepts-streaming-ingestion-event-sources.md#event-source-timestamp) Abschnitt. | `"eventProcessedLocalTime": "2020-03-20T09:03:32.8301668Z"` |  Wenn „eventProcessedLocalTime“ der Zeitstempel für die Ereignisquelle ist: `$event.$ts`. Wenn es sich um eine andere JSON-Eigenschaft handelt: `$event.eventProcessedLocalTime.DateTime` oder `$event['eventProcessedLocalTime'].DateTime` | `eventProcessedLocalTime_datetime`
| **double** | Eine 64-Bit-Zahl mit doppelter Genauigkeit  | `"value": 31.0482941` | `$event.value.Double` oder `$event['value'].Double` |  `value_double`
| **long** | Eine 64-Bit-Ganzzahl mit Vorzeichen  | `"value" : 31` | `$event.value.Long` oder `$event['value'].Long` |  `value_long`
| **string** | Textwerte müssen aus gültigen Zeichen in UTF-8 bestehen. NULL und leere Zeichenfolgen werden gleich behandelt. |  `"site": "DIM_MLGGG"`| `$event.site.String` oder `$event['site'].String`| `site_string`
| **dynamic** | Ein komplexer (nicht primitiver) Typ, der entweder aus einem Array oder einer Eigenschaftensammlung (Wörterbuch) besteht. Derzeit werden nur JSON-Arrays, die Primitive als Zeichenfolgen enthalten, oder Arrays von Objekten, die nicht die TS-ID oder Zeitstempel-Eigenschaft(en) enthalten, mit dem Typ „dynamic“ gespeichert. Lesen Sie diesen [Artikel](./concepts-json-flattening-escaping-rules.md), um zu erfahren, wie Objekte vereinfacht und Arrays möglicherweise aufgelöst werden. Auf die Nutzlasteigenschaften, die als dieser Typ gespeichert sind, kann nur durch Auswählen von `Explore Events` im Time Series Insights-Explorer zum Anzeigen von Rohereignissen oder über die [`GetEvents`](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/execute#getevents) -Abfrage-API für die clientseitige Analyse zugegriffen werden. |  `"values": "[197, 194, 189, 188]"` | Verweise auf dynamische Typen in einem Zeitreihenausdruck werden noch nicht unterstützt. | `values_dynamic`

> [!NOTE]
> 64-Bit-Ganzzahlwerte werden unterstützt. Die größte Zahl, die vom Azure Time Series Insights-Explorer sicher ausgedrückt werden kann, ist aufgrund von JavaScript-Einschränkungen folgende: 9.007.199.254.740.991 (2^53-1). Wenn Sie in Ihrem Datenmodell mit größeren Zahlen arbeiten, können Sie die Größe verringern, indem Sie eine [Zeitreihenmodellvariable](/azure/time-series-insights/concepts-variables#numeric-variables) erstellen und den Wert [konvertieren](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax#conversion-functions).

> [!NOTE]
> Der Typ **Zeichenfolge** lässt keine NULL-Werte zu:
>
> * Ein in einer [Zeitreihenabfrage](https://docs.microsoft.com/rest/api/time-series-insights/reference-query-apis) ausgedrückter [Zeitreihenausdruck (Time Series Expression, TSX)](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax), der den Wert einer leeren Zeichenfolge ( **''** ) mit **NULL** vergleicht, weist das gleiche Verhalten auf: `$event.siteid.String = NULL` entspricht `$event.siteid.String = ''`.
> * Die API gibt möglicherweise **NULL**-Werte zurück, auch wenn ursprüngliche Ereignisse leere Zeichenfolgen enthielten.
> * Verwenden Sie keine Abhängigkeit von **NULL**-Werten in Spalten vom Typ **Zeichenfolge**, um Vergleiche oder Auswertungen durchzuführen. Behandeln Sie sie wie leere Zeichenfolgen.

## <a name="sending-mixed-data-types"></a>Senden von gemischten Datentypen

Ihre Azure Time Series Insights Gen2-Umgebung ist stark typisiert. Wenn Geräte oder Tags Daten unterschiedlichen Typs für eine Geräteeigenschaft senden, werden die Werte in zwei verschiedenen Spalten gespeichert. Zudem sollten Sie die [coalesce()-Funktion](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax#other-functions) verwenden, wenn Sie die Ausdrücke Ihrer Zeitreihenmodellvariablen in API-Aufrufen definieren.

Der Azure Time Series Insights-Explorer bietet eine Möglichkeit zum automatischen Zusammenfügen der verschiedenen Spalten einer Geräteeigenschaft. Im folgenden Beispiel sendet der Sensor eine `PresentValue`-Eigenschaft, die sowohl vom Typ „Long“ als auch vom Typ „Double“ sein kann. Wählen Sie `PresentValue (Double | Long)` aus, um alle gespeicherten Werte der `PresentValue`-Eigenschaft (unabhängig vom Datentyp) abzufragen. Daraufhin werden die Spalten zusammengefügt.

[![Explorer: automatisches Zusammenfügen](media\concepts-supported-data-types/explorer-auto-coalesce-sample.png)](media\concepts-supported-data-types/explorer-auto-coalesce-sample.png#lightbox)

## <a name="objects-and-arrays"></a>Objekte und Arrays

Sie können komplexe Typen, z. B. Objekte und Arrays, als Teil ihrer Ereignisnutzlast senden. Geschachtelte Objekte werden vereinfacht, und Arrays werden entweder als `dynamic` gespeichert oder vereinfacht, um abhängig von Ihrer Umgebungskonfiguration und JSON-Form mehrere Ereignisse zu erzeugen. Weitere Informationen finden Sie unter [JSON-Vereinfachungs- und -Escaperegeln](./concepts-json-flattening-escaping-rules.md).

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie die [JSON-Vereinfachungs- und -Escaperegeln](./concepts-json-flattening-escaping-rules.md), um zu verstehen, wie Ereignisse gespeichert werden.

* Ermitteln der [Durchsatzbeschränkungen](./concepts-streaming-ingress-throughput-limits.md) Ihrer Umgebung

* Erfahren Sie mehr über [Ereignisquellen](concepts-streaming-ingestion-event-sources.md), um Streamingdaten zu erfassen.
