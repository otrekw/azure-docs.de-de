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
ms.date: 07/23/2020
ms.openlocfilehash: 9d29d83ed92ee0fa2217bb1c27fbf6c2fbb3584c
ms.sourcegitcommit: d7bd8f23ff51244636e31240dc7e689f138c31f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2020
ms.locfileid: "87170877"
---
# <a name="supported-data-types"></a>Unterstützte Datentypen

In der folgenden Tabelle sind die von Azure Time Series Insights Gen2 unterstützten Datentypen aufgeführt.

| Datentyp | BESCHREIBUNG | Beispiel | Spaltenname für Eigenschaft in Parquet
|---|---|---|---|
| **bool** | Ein Datentyp, der diese beiden Zustände aufweisen kann: `true` oder `false`. | `"isQuestionable" : true` | isQuestionable_bool
| **datetime** | Stellt einen Zeitpunkt dar, der üblicherweise als Datum und Uhrzeit ausgedrückt wird. Wird im Format [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html) ausgedrückt. Datetime-Eigenschaften werden immer im UTC-Format gespeichert. Zeitzonenverschiebungen werden, sofern korrekt formatiert, angewendet, und der resultierende Wert wird in UTC gespeichert. Weitere Informationen über die Zeitstempel-Eigenschaft einer Umgebung und die Datetime-Verschiebungen finden Sie in [diesem](concepts-streaming-ingestion-event-sources.md#event-source-timestamp) Abschnitt. | `"eventProcessedLocalTime": "2020-03-20T09:03:32.8301668Z"` | eventProcessedLocalTime_datetime
| **double** | Eine 64-Bit-Zahl mit doppelter Genauigkeit  | `"value": 31.0482941` | value_double
| **long** | Eine 64-Bit-Ganzzahl mit Vorzeichen  | `"value" : 31` | value_long
| **string** | Textwerte müssen aus gültigen Zeichen in UTF-8 bestehen. NULL und leere Zeichenfolgen werden gleich behandelt. |  `"site": "DIM_MLGGG"` | site_string
| **dynamic** | Ein komplexer (nicht primitiver) Typ, der entweder aus einem Array oder einer Eigenschaftensammlung (Wörterbuch) besteht. Derzeit werden nur JSON-Arrays, die Primitive als Zeichenfolgen enthalten, oder Arrays von Objekten, die nicht die TS-ID oder Zeitstempel-Eigenschaft(en) enthalten, mit dem Typ „dynamic“ gespeichert. Lesen Sie diesen [Artikel](./concepts-json-flattening-escaping-rules.md), um zu erfahren, wie Objekte vereinfacht und Arrays möglicherweise aufgelöst werden. Auf Nutzlasteigenschaften, die mit diesem Typ gespeichert werden, kann über den Azure Time Series Insights Gen2-Explorer und die `GetEvents` Abfrage-API zugegriffen werden. |  `"values": "[197, 194, 189, 188]"` | values_dynamic

## <a name="sending-mixed-data-types"></a>Senden von gemischten Datentypen

Ihre Azure Time Series Insights Gen2-Umgebung ist stark typisiert. Wenn Geräte oder Tags Daten unterschiedlichen Typs für eine Geräteeigenschaft senden, werden die Werte in zwei verschiedenen Spalten gespeichert. Zudem sollten Sie die [coalesce()-Funktion](https://docs.microsoft.com/rest/api/time-series-insights/preview#other-functions) verwenden, wenn Sie die Ausdrücke Ihrer Zeitreihenmodellvariablen in API-Aufrufen definieren.

Der Azure Time Series Insights-Explorer bietet eine Möglichkeit zum automatischen Zusammenfügen der verschiedenen Spalten einer Geräteeigenschaft. Im folgenden Beispiel sendet der Sensor eine `PresentValue`-Eigenschaft, die sowohl vom Typ „Long“ als auch vom Typ „Double“ sein kann. Wählen Sie `PresentValue (Double | Long)` aus, um alle gespeicherten Werte der `PresentValue`-Eigenschaft (unabhängig vom Datentyp) abzufragen. Daraufhin werden die Spalten zusammengefügt.

[![Explorer: automatisches Zusammenfügen](media\concepts-supported-data-types/explorer-auto-coalesce-sample.png)](media\concepts-supported-data-types/explorer-auto-coalesce-sample.png#lightbox)

## <a name="objects-and-arrays"></a>Objekte und Arrays

Sie können komplexe Typen, z. B. Objekte und Arrays, als Teil ihrer Ereignisnutzlast senden. Geschachtelte Objekte werden vereinfacht, und Arrays werden entweder als `dynamic` gespeichert oder vereinfacht, um abhängig von Ihrer Umgebungskonfiguration und JSON-Form mehrere Ereignisse zu erzeugen. Weitere Informationen finden Sie unter [JSON-Vereinfachungs- und -Escaperegeln](./concepts-json-flattening-escaping-rules.md).

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie die [JSON-Vereinfachungs- und -Escaperegeln](./concepts-json-flattening-escaping-rules.md), um zu verstehen, wie Ereignisse gespeichert werden.

* Ermitteln der [Durchsatzbeschränkungen](./concepts-streaming-ingress-throughput-limits.md) Ihrer Umgebung

* Erfahren Sie mehr über [Ereignisquellen](concepts-streaming-ingestion-event-sources.md), um Streamingdaten zu erfassen.
