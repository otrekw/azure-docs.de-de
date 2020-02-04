---
title: Datenspeicherung und -eingang in Azure Time Series Insights (Vorschau)| Microsoft-Dokumentation
description: Erfahren Sie mehr über Datenspeicherung und -eingang in Azure Time Series Insights (Vorschau).
author: lyrana
ms.author: lyhughes
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/31/2019
ms.custom: seodec18
ms.openlocfilehash: f00529d00312fd6acb045de698590047f991bec7
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76714286"
---
# <a name="data-storage-and-ingress-in-azure-time-series-insights-preview"></a>Datenspeicherung und -eingang in Azure Time Series Insights Preview

In diesem Artikel werden Updates für Datenspeicherung und Dateneingang für Azure Time Series Insights Preview beschrieben. Es werden die zugrunde liegende Speicherstruktur, das Dateiformat und die „Time Series-ID“-Eigenschaft besprochen. Der Artikel erläutert auch den zugrunde liegenden eingehenden Prozess, Best Practices und aktuelle Einschränkungen der Vorschau.

## <a name="data-ingress"></a>Dateneingang

Ihre Azure Time Series Insights-Umgebung (TSI) enthält eine Erfassungs-Engine, mit der Zeitreihendaten gesammelt, verarbeitet und gespeichert werden. Beim Planen Ihrer Umgebung sind einige Aspekte zu berücksichtigen, um die Verarbeitung aller eingehenden Daten sicherzustellen, ein hohes Eingangsvolumen zu erzielen und die Latenz bei der Erfassung zu minimieren (dies ist die Zeit, die TSI benötigt, um Daten aus der Ereignisquelle zu lesen und zu verarbeiten). 

In Time Series Insights Preview bestimmen Richtlinien für den Dateneingang, aus welchen Quellen Daten bezogen werden können und welches Format diese Daten haben müssen.

### <a name="ingress-policies"></a>Eingangsrichtlinien

#### <a name="event-sources"></a>Ereignisquellen

Time Series Insights Preview unterstützt die folgenden Ereignisquellen:

- [Azure IoT Hub](../iot-hub/about-iot-hub.md)
- [Azure Event Hubs](../event-hubs/event-hubs-about.md)

Time Series Insights Preview unterstützt maximal zwei Ereignisquellen pro Instanz.

> [!WARNING] 
> * Beim Anfügen einer Ereignisquelle an die Vorschau-Umgebung tritt zu Anfang möglicherweise eine hohe Latenz auf. 
> Die Latenz der Ereignisquelle hängt von der Anzahl der Ereignisse ab, die sich aktuell in Ihrem IoT Hub oder Event Hub befinden.
> * Eine hohe Latenz lässt nach der erstmaligen Erfassung der Ereignisquelldaten nach. Wenn bei Ihnen fortgesetzt hohe Latenz auftritt, wenden Sie sich an uns, indem Sie ein Supportticket über das Azure-Portal einreichen.

#### <a name="supported-data-format-and-types"></a>Unterstützte Datenformate und -typen

Azure Time Series Insights unterstützt UTF8-codiertes JSON, das über Azure IoT Hub oder Azure Event Hubs übermittelt wird. 

Es folgt die Liste der unterstützten Datentypen.

| Datentyp | Beschreibung |
|-----------|------------------|-------------|
| bool      |   Ein Datentyp, der einen von zwei Zuständen hat: „true“ oder „false“.       |
| dateTime    |   Stellt einen Zeitpunkt dar, der üblicherweise als Datum und Uhrzeit ausgedrückt wird. „DateTime“-Werte sollten im ISO 8601-Format sein.      |
| double    |   Ein 64-Bit-Gleitkommawert gemäß IEEE 754 mit doppelter Genauigkeit
| string    |   Aus Unicode-Zeichen bestehende Textwerte.          |

#### <a name="objects-and-arrays"></a>Objekte und Arrays

Sie können komplexe Typen wie Objekte und Arrays als Teil ihrer Ereignisnutzlast senden, aber Ihre Daten werden bei der Speicherung einem Vereinfachungsprozess unterzogen. Weitere Informationen zum Strukturieren von JSON-Ereignissen sowie Details zur Vereinfachung komplexer Typen und geschachtelter Objekte finden Sie auf der Seite [Strukturieren von JSON für Eingang und Abfrage](./time-series-insights-update-how-to-shape-events.md).


### <a name="ingress-best-practices"></a>Best Practices für den Dateneingang

Folgende Best Practices werden empfohlen:

* Konfigurieren Sie Time Series Insights und Ihren IoT Hub oder Event Hub in derselben Region, um durch das Netzwerk verursachte Erfassungswartezeiten zu verringern.
* Planen Sie Ihre Anforderungen an das Eingangsvolumen, indem Sie die erwartete Erfassungsrate berechnen und überprüfen, ob diese sich innerhalb der unterstützten Rate befindet, wie weiter unten erläutert.
* Informieren Sie sich unter [Gestalten von JSON für Abfragen](./time-series-insights-update-how-to-shape-events.md) über die Optimierung und Gestaltung Ihrer JSON-Daten sowie über die aktuellen Einschränkungen der Vorschau.

### <a name="ingress-scale-and-limitations-in-preview"></a>Dateneingangsvolumen und Einschränkungen der Vorschauversion

#### <a name="per-environment-limitations"></a>Einschränkungen pro Umgebung

Im Allgemeinen werden Eingangsraten als Faktor der Anzahl von Geräten in Ihrer Organisation, der Häufigkeit der Ereignisausgabe und der Größe der einzelnen Ereignisse angesehen.

*  **Anzahl der Geräte** × **Häufigkeit der Ereignisausgabe** × **Größe jedes Ereignisses**.

Standardmäßig kann Time Series Insights Preview eingehende Daten mit einer Rate von bis zu 1 MB pro Sekunde (MBit/s) **pro TSI-Umgebung** erfassen. Kontaktieren Sie uns, wenn dies nicht Ihren Anforderungen entspricht. Wir können bis zu 16 MBit/s für eine Umgebung unterstützen, indem Sie ein Supportticket im Azure-Portal einreichen.
 
Beispiel 1: Contoso Shipping verfügt über 100.000 Geräte, die dreimal pro Minute ein Ereignis ausgeben. Die Größe eines Ereignisses beträgt 200 Byte. Sie verwenden einen Event Hub mit 4 Partitionen als TSI-Ereignisquelle.
Die Erfassungsrate für ihre TSI-Umgebung wäre wie folgt: 100.000 Geräte * 200 Bytes/Ereignis * (3/60 Ereignis/s) = 1 Mbit/s.
Die Erfassungsrate pro Partition wäre 0,25 Mbit/s.
Die Erfassungsrate von Contoso Shipping läge damit innerhalb der Skalierungseinschränkungen der Preview.
 
Beispiel 2: Contoso Fleet Analytics verfügt über 60.000 Geräte, die jede Sekunde ein Ereignis ausgeben. Sie verwenden eine IoT Hub 24-Partitionsanzahl von 4 als TSI-Ereignisquelle. Die Größe eines Ereignisses beträgt 200 Byte.
Die Erfassungsrate der Umgebung wäre wie folgt: 20.000 Geräte * 200 Bytes/Ereignis * 1 Ereignis/s = 4 Mbit/s.
Die Rate pro Partition wäre dann 1 MBit/s.
Contoso Fleet Analytics müsste eine Anforderung für eine dedizierte Umgebung über das Azure-Portal an TSI senden, um diese Skalierung zu erzielen.

#### <a name="hub-partitions-and-per-partition-limits"></a>Hub-Partitionen und Limits pro Partition

Bei der Planung Ihrer TSI-Umgebung ist es wichtig, die Konfiguration der Ereignisquelle(n) zu berücksichtigen, die Sie mit TSI verbinden möchten. Sowohl Azure IoT Hub als auch Event Hubs verwenden Partitionen, um die horizontale Skalierung für die Ereignisverarbeitung zu ermöglichen.  Eine Partition ist eine geordnete Sequenz von Ereignissen, die in einem Event Hub besteht. Die Partitionsanzahl wird während der Erstellungsphase von IoT oder Event Hubs festgelegt und kann nicht geändert werden. Weitere Informationen zum Ermitteln der Partitionsanzahl finden Sie in den häufig gestellten Fragen zu Event Hubs unter „Wie viele Partitionen benötige ich?“. Für TSI-Umgebungen, die IoT Hub verwenden, benötigen die meisten IoT Hubs in der Regel nur 4 Partitionen. Unabhängig davon, ob Sie einen neuen Hub für Ihre TSI-Umgebung erstellen oder einen vorhandenen Hub verwenden, müssen Sie die Erfassungsrate pro Partition berechnen, um festzustellen, ob diese innerhalb der Limits der Preview liegt. Die TSI Preview besitzt zurzeit ein Limit **pro Partition** von 0,5 MB/s. Verwenden Sie die folgenden Beispiele als Referenz, und beachten Sie die folgenden IoT Hub-spezifischen Überlegungen, wenn Sie ein IoT Hub-Benutzer sind.

#### <a name="iot-hub-specific-considerations"></a>IoT Hub-spezifische Überlegungen

Wenn ein Gerät in IoT Hub erstellt wird, wird es einer Partition zugewiesen, und die Partitionszuweisung ändert sich nicht mehr. Hierdurch kann IoT Hub die Ereignisreihenfolge garantieren. Dies hat jedoch Auswirkungen auf TSI als Downstreamreader in bestimmten Szenarios. Wenn Nachrichten von mehreren Geräten mit derselben Gatewaygeräte-ID an den Hub weitergeleitet werden, kommen Sie in derselben Partition an und können so die Skalierungseinschränkung pro Partition möglicherweise überschreiten. 

**Auswirkung**: Wenn es bei einer einzelnen Partition zu einer dauerhaften Erfassungsrate gegenüber der Einschränkung der Preview kommt, besteht die Möglichkeit, dass der TSI-Reader es niemals schaffen wird, vor Ablauf des Aufbewahrungszeitraums für IoT Hub-Daten aufzuholen. Dies würde zu einem Datenverlust führen.

Es wird Folgendes empfohlen: 

* Berechnen Sie Ihre Erfassungsrate pro Umgebung und pro Partition, bevor Sie Ihre Lösung bereitstellen.
* Stellen Sie sicher, dass für Ihre IoT Hub-Geräte (und somit Partitionen) ein maximaler Lastenausgleich erfolgt.

> [!WARNING]
> In Umgebungen, die IoT Hub als Ereignisquelle verwenden, berechnen Sie die Erfassungsrate anhand der Anzahl der verwendeten Hubgeräte, um sicherzugehen, dass die Rate unter 0,5 MBit/s pro Partitionseinschränkung in der Preview fällt.

  ![IoT Hub-Partitionsdiagramm](media/concepts-ingress-overview/iot-hub-partiton-diagram.png)

Weitere Informationen zu Durchsatzeinheiten und Partitionen finden Sie unter den folgenden Links:

* [Skalieren von Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-scaling)
* [Skalierbarkeit – Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-scalability#throughput-units)
* [Event Hub-Partitionen](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#partitions)

### <a name="data-storage"></a>Datenspeicher

Wenn Sie eine Time Series Insights Preview-Umgebung mit einer SKU mit nutzungsbasierter Bezahlung erstellen, erstellen Sie zwei Azure-Ressourcen:

* Eine Time Series Insights Preview-Umgebung, die optional warme Speicherfunktionen enthalten kann.
* Ein Azure Storage-Blobkonto vom Typ „Universell V1“ für die Speicherung von kalten Daten.

Daten im warmen Speicher sind nur über [Zeitreihenabfragen](./time-series-insights-update-tsq.md) und den [Azure Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md) verfügbar. 

Time Series Insights Preview speichert kalte Speicherdaten im [Parquet-Dateiformat](#parquet-file-format-and-folder-structure) im Azure-Blobspeicher. Time Series Insights Preview verwaltet diese kalten Speicherdaten exklusiv. Sie können diese Daten jedoch direkt als standardmäßige Parquet-Dateien lesen.

> [!WARNING]
> Als Besitzer des Azure-Blobspeicherkontos, in dem sich die kalten Daten befinden, haben Sie Vollzugriff auf alle Daten im Konto. Dieser Zugriff umfasst Berechtigungen zum Schreiben und Löschen. Bearbeiten oder löschen Sie keine von Time Series Insights Preview geschriebenen Daten, da dies zu Datenverlusten führen kann.

### <a name="data-availability"></a>Datenverfügbarkeit

Time Series Insights Preview partitioniert und indiziert Daten, um eine optimale Abfrageleistung zu erzielen. Die Daten stehen nach der Indizierung für Abfragen zur Verfügung. Die Menge an erfassten Daten kann sich auf diese Verfügbarkeit auswirken.

> [!IMPORTANT]
> Während der Vorschauphase kann es bis zu 60 Sekunden dauern, bis Daten verfügbar werden. Wenn Sie eine erhebliche Latenz von weit über 60 Sekunden feststellen, übermitteln Sie ein Supportticket über das Azure-Portal.

## <a name="azure-storage"></a>Azure Storage

Dieser Abschnitt enthält für Azure Time Series Insights Preview relevante Informationen zu Azure Storage.

Eine ausführliche Beschreibung des Azure Blobspeichers finden Sie in der [Einführung in Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md).

### <a name="your-storage-account"></a>Ihr Speicherkonto

Wenn Sie eine Time Series Insights Preview-Umgebung mit nutzungsbasierter Bezahlung erstellen, wird ein Azure Storage-Blobkonto vom Typ „Universell V1“ als langfristiger Speicher für kalte Daten erstellt.  

Time Series Insights Preview veröffentlicht bis zu zwei Kopien jedes Ereignisses in Ihrem Azure-Speicherkonto. In der ersten Kopie sind die Ereignisse nach Erfassungszeitpunkt sortiert. Diese Kopie wird dauerhaft beibehalten, sodass Sie andere Dienste für den Zugriff darauf verwenden können. Sie können Spark, Hadoop oder andere vertraute Tools verwenden, um die unformatierten Parquet-Dateien zu verarbeiten. 

Time Series Insights Preview partitioniert die Parquet-Dateien neu, um sie für die Time Series Insights-Abfragen zu optimieren. Diese neu partitionierte Kopie der Daten wird ebenfalls gespeichert.

Während der öffentlichen Preview werden Daten mit unbegrenzter Dauer in Ihrem Azure-Speicherkonto gespeichert.

#### <a name="writing-and-editing-time-series-insights-blobs"></a>Schreiben und Bearbeiten von Time Series Insights-Blobs

Um Abfrageleistung und Datenverfügbarkeit sicherzustellen, bearbeiten oder löschen Sie keine Blobs, die von Time Series Insights Preview erstellt werden.

#### <a name="accessing-and-exporting-data-from-time-series-insights-preview"></a>Zugreifen auf und Exportieren von Daten aus Time Series Insights Preview

Sie können auf im Time Series Insights Preview-Explorer gespeicherte Daten zugreifen, um sie in Verbindung mit anderen Diensten zu verwenden. Sie können die Daten z. B. verwenden, um einen Bericht in Power BI zu erstellen oder in Azure Machine Learning Studio ein Machine Learning-Modell zu trainieren. Sie können die Daten auch zum Transformieren, Visualisieren und Modellieren in Ihren Jupyter-Notebooks verwenden.

Sie können auf drei allgemeine Arten auf Ihre Daten zugreifen:

* Aus dem Time Series Insights Preview-Explorer. Sie können Daten als CSV-Datei aus dem Explorer exportieren. Weitere Informationen finden Sie unter [Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md).
* Aus der Time Series Insights Preview-API unter Verwendung der Abfrage zum Abrufen von Ereignissen. Weitere Informationen zu dieser API finden Sie unter [Zeitreihenabfrage](./time-series-insights-update-tsq.md).
* Direkt aus einem Azure-Speicherkonto. Sie benötigen Lesezugriff auf das jeweilige Konto, das Sie für den Zugriff auf Ihre Time Series Insights Preview-Daten verwenden. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf Ihre Speicherkontoressourcen](../storage/blobs/storage-manage-access-to-resources.md).

#### <a name="data-deletion"></a>Löschen von Daten

Löschen Sie Ihre Time Series Insights Preview-Daten nicht. Verwalten Sie zugehörige Daten nur innerhalb von Time Series Insights Preview.

### <a name="parquet-file-format-and-folder-structure"></a>Parquet-Dateiformat und Ordnerstruktur

Parquet ist ein spaltenbasiertes Open-Source-Dateiformat, das für effiziente Speicherung und Leistung konzipiert wurde. Aus diesen Gründen wird Parquet von Time Series Insights Preview verwendet. Die Daten werden anhand der Zeitreihen-ID partitioniert, um bei großen Datenmengen eine hohe Abfrageleistung zu erzielen.  

Weitere Informationen zum Parquet-Dateityp finden Sie in der [Parquet-Dokumentation](https://parquet.apache.org/documentation/latest/).

Time Series Insights Preview speichert wie folgt Kopien Ihrer Daten:

* Die erste, anfängliche Kopie wird anhand des Erfassungszeitpunkts partitioniert und speichert Daten in etwa in der Reihenfolge ihres Eingangs. Die Daten befinden sich im Ordner `PT=Time`:

  `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

* Die zweite, neu partitionierte Kopie wird durch Gruppierung von Zeitreihen-IDs partitioniert und befindet sich im Ordner `PT=TsId`:

  `V=1/PT=TsId/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

In beiden Fällen entsprechen die Werte für die Uhrzeit der Uhrzeit der Bloberstellung. Daten im Ordner `PT=Time` werden beibehalten. Daten im Ordner `PT=TsId` werden im Lauf der Zeit für Abfragen optimiert und bleiben nicht statisch.

> [!NOTE]
> * `<YYYY>` entspricht einer vierstelligen Jahresdarstellung.
> * `<MM>` entspricht einer zweistelligen Monatsdarstellung.
> * `<YYYYMMDDHHMMSSfff>` entspricht einer Zeitstempeldarstellung mit folgendem Format: vierstellige Jahresangabe (`YYYY`), zweistellige Monatsangabe (`MM`), zweistellige Tagesangabe (`DD`), zweistellige Stundenangabe (`HH`), zweistellige Minutenangabe (`MM`), zweistellige Sekundenangabe (`SS`) und dreistellige Millisekundenangabe (`fff`).

Time Series Insights Preview-Ereignisse werden dem Inhalt von Parquet-Dateien wie folgt zugeordnet:

* Jedes Ereignis wird einer einzelnen Zeile zugeordnet.
* Jede Zeile enthält die Spalte **timestamp** mit einem Zeitstempel für das Ereignis. Die timestamp-Eigenschaft ist nie NULL. Ihr Standardwert ist der **Zeitpunkt der Einreihung des Ereignisses in die Warteschlange**, wenn die timestamp-Eigenschaft in der Ereignisquelle nicht angegeben ist. Der Zeitstempel wird immer in UTC angegeben.
* Jede Zeile enthält eine oder mehrere Spalten mit der Zeitreihen-ID, wie beim Erstellen der Time Series Insights-Umgebung definiert. Der Eigenschaftsname enthält das Suffix `_string`.
* Alle anderen als Telemetriedaten gesendeten Eigenschaften werden Spaltennamen zugeordnet, die je nach Eigenschaftstyp auf `_string` (Zeichenfolge), `_bool` (boolesch), `_datetime` (Datum/Uhrzeit) und `_double` (Double) enden.
* Dieses Zuordnungsschema gilt für die erste Version des Dateiformats, auf das als **V=1** verwiesen wird. Mit der Weiterentwicklung dieses Features wird der Name hochgezählt.

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie [Strukturieren von JSON für Eingang und Abfragen](./time-series-insights-update-how-to-shape-events.md).

- Informieren Sie sich über die neue [Datenmodellierung](./time-series-insights-update-tsm.md).
