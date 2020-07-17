---
title: Unterstützte Datentypen – Azure Time Series Insights | Microsoft-Dokumentation
description: Erfahren Sie mehr über die unterstützten Datentypen in Azure Time Series Insights (Vorschauversion).
author: lyrana
ms.author: lyhughes
manager: deepakpalled
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 06/03/2020
ms.custom: seodec18
ms.openlocfilehash: 1a1ab9fa19956341e930c85d13d7067be4449cea
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86049394"
---
# <a name="supported-data-types"></a>Unterstützte Datentypen

In der folgenden Tabelle sind die von Time Series Insights unterstützten Datentypen aufgeführt.

| Datentyp | BESCHREIBUNG | Beispiel | Spaltenname für Eigenschaft in Parquet
|---|---|---|---|
| **bool** | Ein Datentyp, der diese beiden Zustände aufweisen kann: `true` oder `false`. | "isQuestionable" : true | isQuestionable_bool
| **datetime** | Stellt einen Zeitpunkt dar, der üblicherweise als Datum und Uhrzeit ausgedrückt wird. Wird im Format [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html) ausgedrückt. Datetime-Eigenschaften werden immer im UTC-Format gespeichert. Zeitzonenverschiebungen werden, sofern korrekt formatiert, angewendet, und der resultierende Wert wird in UTC gespeichert. Weitere Informationen über die Zeitstempel-Eigenschaft einer Umgebung und die Datetime-Verschiebungen finden Sie in [diesem](concepts-streaming-ingestion-event-sources.md#event-source-timestamp) Abschnitt. | "eventProcessedLocalTime": "2020-03-20T09:03:32.8301668Z" | eventProcessedLocalTime_datetime 
| **double** | Eine 64-Bit-Zahl mit doppelter Genauigkeit  | "value": 31.0482941 | value_double
| **long** | Eine 64-Bit-Ganzzahl mit Vorzeichen  | "value" : 31 | value_long
| **string** | Textwerte müssen aus gültigen Zeichen in UTF-8 bestehen. |  "site": "DIM_MLGGG" | site_string
| **dynamic** | Ein komplexer (nicht primitiver) Typ, der entweder aus einem Array oder einer Eigenschaftensammlung (Wörterbuch) besteht. Derzeit werden nur JSON-Arrays, die Primitive als Zeichenfolgen enthalten, oder Arrays von Objekten, die nicht die TS-ID oder Zeitstempel-Eigenschaft(en) enthalten, mit dem Typ „dynamic“ gespeichert. Lesen Sie diesen [Artikel](./concepts-json-flattening-escaping-rules.md), um zu erfahren, wie Objekte vereinfacht und Arrays möglicherweise aufgelöst werden. |  "values": "[197, 194, 189, 188]" | values_dynamic

> [!IMPORTANT]
>
> * Ihre TSI-Umgebung ist stark typisiert. Wenn Geräte oder Tags sowohl ganzzahlige als auch nicht ganzzahlige Daten senden, werden die Geräteeigenschaftswerte in zwei getrennten Double- und Long-Spalten gespeichert, und beim Ausführen von API-Aufrufen und Definieren der Zeitreihenmodellvariablen sollte die [coalesce()-Funktion](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) verwendet werden.

#### <a name="objects-and-arrays"></a>Objekte und Arrays

Sie können komplexe Typen, z. B. Objekte und Arrays, als Teil ihrer Ereignisnutzlast senden. Geschachtelte Objekte werden vereinfacht, und Arrays werden entweder als `dynamic` gespeichert oder vereinfacht, um abhängig von Ihrer Umgebungskonfiguration und JSON-Form mehrere Ereignisse zu erzeugen. Weitere Informationen finden Sie unter [JSON-Vereinfachungs- und -Escaperegeln](./concepts-json-flattening-escaping-rules.md).

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie die [JSON-Vereinfachungs- und -Escaperegeln](./concepts-json-flattening-escaping-rules.md), um zu verstehen, wie Ereignisse gespeichert werden. 

* Ermitteln der [Durchsatzbeschränkungen](concepts-streaming-throughput-limitations.md) Ihrer Umgebung

* Erfahren Sie mehr über [Ereignisquellen](concepts-streaming-ingestion-event-sources.md), um Streamingdaten zu erfassen.
