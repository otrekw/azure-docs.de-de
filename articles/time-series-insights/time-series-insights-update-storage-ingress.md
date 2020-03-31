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
ms.date: 02/10/2020
ms.custom: seodec18
ms.openlocfilehash: 2f12cf303c58f0fa614c59ffe643c6c2ee5d2415
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78246189"
---
# <a name="data-storage-and-ingress-in-azure-time-series-insights-preview"></a>Datenspeicherung und -eingang in Azure Time Series Insights Preview

In diesem Artikel werden Updates für Datenspeicherung und Dateneingang für Azure Time Series Insights Preview beschrieben. Es werden die zugrunde liegende Speicherstruktur, das Dateiformat und die „Time Series-ID“-Eigenschaft beschrieben. Darüber hinaus werden auch der zugrunde liegende Eingangsprozess, die bewährten Methoden und aktuelle Einschränkungen der Vorschauversion beschrieben.

## <a name="data-ingress"></a>Dateneingang

Ihre Azure Time Series Insights-Umgebung (TSI) enthält eine *Erfassungs-Engine*, mit der Zeitreihendaten gesammelt, verarbeitet und gespeichert werden. 

Beim [Planen Ihrer Umgebung](time-series-insights-update-plan.md) sollten einige Aspekte berücksichtigt werden, um die Verarbeitung aller eingehenden Daten sicherzustellen, ein hohes Eingangsvolumen zu erzielen und die *Latenz bei der Erfassung* zu minimieren (dies ist die Zeit, die Time Series Insights benötigt, um Daten aus der Ereignisquelle zu lesen und zu verarbeiten).

Bei Time Series Insights (Vorschauversion) wird mit Richtlinien für den Dateneingang bestimmt, aus welchen Quellen Daten bezogen werden können und welches Format diese Daten haben müssen.

### <a name="ingress-policies"></a>Eingangsrichtlinien

Beim *Dateneingang* geht es auch darum, wie Daten an eine Azure Time Series Insights-Umgebung (Vorschauversion) gesendet werden. 

Die Informationen zur Schlüsselkonfiguration, zur Formatierung und zu den bewährten Methoden sind unten zusammengefasst.

#### <a name="event-sources"></a>Ereignisquellen

Azure Time Series Insights Preview unterstützt die folgenden Ereignisquellen:

- [Azure IoT Hub](../iot-hub/about-iot-hub.md)
- [Azure Event Hubs](../event-hubs/event-hubs-about.md)

Azure Time Series Insights Preview unterstützt maximal zwei Ereignisquellen pro Instanz.

> [!IMPORTANT] 
> * Beim Anfügen einer Ereignisquelle an die Vorschau-Umgebung tritt zu Anfang möglicherweise eine hohe Latenz auf. 
> Die Latenz der Ereignisquelle hängt von der Anzahl der Ereignisse ab, die sich aktuell in Ihrem IoT Hub oder Event Hub befinden.
> * Eine hohe Latenz lässt nach der erstmaligen Erfassung der Ereignisquelldaten nach. Senden Sie ein Supportticket über das Azure-Portal, falls es bei Ihnen zu einer dauerhaft hohen Latenz kommt.

#### <a name="supported-data-format-and-types"></a>Unterstützte Datenformate und -typen

Azure Time Series Insights unterstützt UTF8-codierten JSON-Code, der über Azure IoT Hub oder Azure Event Hubs gesendet wird. 

Die unterstützten Datentypen lauten:

| Datentyp | BESCHREIBUNG |
|---|---|
| **bool** | Ein Datentyp, der diese beiden Zustände aufweisen kann: `true` oder `false`. |
| **dateTime** | Stellt einen Zeitpunkt dar, der üblicherweise als Datum und Uhrzeit ausgedrückt wird. Wird im Format [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html) ausgedrückt. |
| **double** | Ein 64-Bit-Gleitkommawert gemäß [IEEE 754](https://ieeexplore.ieee.org/document/8766229) mit doppelter Genauigkeit. |
| **string** | Aus Unicode-Zeichen bestehende Textwerte.          |

#### <a name="objects-and-arrays"></a>Objekte und Arrays

Sie können komplexe Typen wie Objekte und Arrays als Teil Ihrer Ereignisnutzlast senden, aber Ihre Daten werden bei der Speicherung einem Vereinfachungsprozess unterzogen. 

Ausführliche Informationen zur Beschreibung, wie Sie Ihre JSON-Ereignisse strukturieren, komplexe Typen senden und geschachtelte Objekte vereinfachen, finden Sie unter [Strukturieren von Ereignissen in Azure Time Series Insights Preview](./time-series-insights-update-how-to-shape-events.md). Diese Informationen dienen Ihnen als Hilfe bei der Planung und Optimierung.

### <a name="ingress-best-practices"></a>Best Practices für den Dateneingang

Folgende Best Practices werden empfohlen:

* Konfigurieren Sie Azure Time Series Insights und alle IoT Hub- oder Event Hub-Instanzen in derselben Region, um die potenziell vorhandene Latenz zu reduzieren.

* [Planen Sie Ihre Anforderungen an die Skalierung](time-series-insights-update-plan.md), indem Sie die erwartete Erfassungsrate berechnen und überprüfen, ob diese sich innerhalb der unterstützten Rate befindet. Dies ist weiter unten beschrieben.

* Informieren Sie sich unter [Gestalten von JSON für Abfragen](./time-series-insights-update-how-to-shape-events.md) über die Optimierung und Gestaltung Ihrer JSON-Daten sowie über die aktuellen Einschränkungen der Vorschau.

### <a name="ingress-scale-and-preview-limitations"></a>Dateneingangsvolumen und Einschränkungen der Vorschauversion 

Unten sind die Eingangseinschränkungen von Azure Time Series Insights Preview beschrieben.

> [!TIP]
> Eine umfassende Liste mit allen Einschränkungen der Vorschauversion finden Sie unter [Planen Ihrer Azure Time Series Insights Preview-Umgebung](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-plan#review-preview-limits).

#### <a name="per-environment-limitations"></a>Einschränkungen pro Umgebung

Im Allgemeinen werden Eingangsraten als Faktor der Anzahl von Geräten in Ihrer Organisation, der Häufigkeit der Ereignisausgabe und der Größe der einzelnen Ereignisse angesehen.

*  **Anzahl der Geräte** × **Häufigkeit der Ereignisausgabe** × **Größe jedes Ereignisses**.

Standardmäßig kann Time Series Insights Preview eingehende Daten mit einer Rate von **bis zu 1 MB pro Sekunde (MBit/s) pro Time Series Insights-Umgebung** erfassen.

> [!TIP] 
> * Die Umgebungsunterstützung für Erfassungsgeschwindigkeiten von bis zu 16 MBit/s kann auf Anfrage bereitgestellt werden.
> * Falls Sie einen höheren Durchsatz benötigen, können Sie sich an uns wenden, indem Sie über das Azure-Portal ein Supportticket senden.
 
* **Beispiel 1:**

    Contoso Shipping verfügt über 100.000 Geräte, die dreimal pro Minute ein Ereignis ausgeben. Die Größe eines Ereignisses beträgt 200 Byte. Hierfür wird ein Event Hub mit vier Partitionen als Time Series Insights-Ereignisquelle genutzt.

    * Die Erfassungsrate für die Time Series Insights-Umgebung lautet: **100.000 Geräte · 200 Byte/Ereignis · (3/60 Ereignis/s) = 1 MBit/s**.
    * Die Erfassungsrate pro Partition wäre 0,25 Mbit/s.
    * Die Erfassungsrate von Contoso Shipping läge damit innerhalb der Skalierungseinschränkungen der Preview.

* **Beispiel 2:**

    Contoso Fleet Analytics verfügt über 60.000 Geräte, die jede Sekunde ein Ereignis ausgeben. Sie verwenden eine IoT Hub 24-Partitionsanzahl von 4 als Time Series Insights-Ereignisquelle. Die Größe eines Ereignisses beträgt 200 Byte.

    * Die Erfassungsrate der Umgebung wäre wie folgt: **20.000 Geräte · 200 Byte/Ereignis · 1 Ereignis/s = 4 MBit/s**.
    * Die Rate pro Partition wäre dann 1 MBit/s.
    * Von Contoso Fleet Analytics über das Azure-Portal kann eine Anfrage für Time Series Insights gesendet werden, um eine Erhöhung der Erfassungsrate für die Umgebung anzufordern.

#### <a name="hub-partitions-and-per-partition-limits"></a>Hub-Partitionen und Limits pro Partition

Bei der Planung Ihrer Time Series Insights-Umgebung ist es wichtig, die Konfiguration der Ereignisquellen zu berücksichtigen, die Sie mit Time Series Insights verbinden möchten. Sowohl Azure IoT Hub als auch Event Hubs verwenden Partitionen, um die horizontale Skalierung für die Ereignisverarbeitung zu ermöglichen. 

Eine *Partition* ist eine geordnete Sequenz von Ereignissen, die in einem Event Hub besteht. Die Partitionsanzahl wird beim Erstellen des Hubs festgelegt und kann nicht geändert werden. 

Informationen zu den bewährten Methoden für die Event Hubs-Partitionierung finden Sie unter [Wie viele Partitionen benötige ich?](https://docs.microsoft.com/azure/event-hubs/event-hubs-faq#how-many-partitions-do-i-need).

> [!NOTE]
> Für die meisten IoT Hub-Instanzen, die mit Azure Time Series Insights genutzt werden, sind nur vier Partitionen erforderlich.

Unabhängig davon, ob Sie einen neuen Hub für Ihre Time Series Insights-Umgebung erstellen oder einen vorhandenen Hub verwenden, müssen Sie die Erfassungsrate pro Partition berechnen, um festzustellen, ob diese innerhalb der Limits der Vorschauversion liegt. 

Für Azure Time Series Insights Preview gilt derzeit ein allgemeiner **Grenzwert von 0,5 MBit/s pro Partition**.

#### <a name="iot-hub-specific-considerations"></a>IoT Hub-spezifische Überlegungen

Wenn ein Gerät in IoT Hub erstellt wird, wird es einer Partition dauerhaft zugewiesen. Auf diese Weise kann von IoT Hub die Ereignisreihenfolge sichergestellt werden (da sich die Zuweisung niemals ändert).

Eine feste Partitionszuweisung wirkt sich auch auf Time Series Insights-Instanzen aus, mit denen Daten erfasst werden, die von IoT Hub nachfolgend gesendet werden. Wenn Nachrichten von mehreren Geräten mit derselben Gatewaygeräte-ID an den Hub weitergeleitet werden, kommen sie ggf. gleichzeitig in derselben Partition an, und dies kann unter Umständen dazu führen, dass die Skalierungslimits pro Partition überschritten werden. 

**Auswirkung**:

* Wenn es für eine einzelne Partition zu einer dauerhaften Erfassungsrate oberhalb des Limits der Vorschauversion kommt, werden von Time Series Insights ggf. nicht alle Gerätetelemetriedaten synchronisiert, bevor der Zeitraum für die IoT Hub-Datenaufbewahrung überschritten wurde. Aus diesem Grund können gesendete Daten verloren gehen, falls die Erfassungslimits dauerhaft überschritten werden.

Wir empfehlen Ihnen die folgenden bewährten Methoden, um dieses Problem zu umgehen:

* Berechnen Sie Ihre Erfassungsraten pro Umgebung und pro Partition, bevor Sie Ihre Lösung bereitstellen.
* Stellen Sie sicher, dass für Ihre IoT Hub-Geräte ein maximaler Lastenausgleich erfolgt.

> [!IMPORTANT]
> In Umgebungen, die IoT Hub als Ereignisquelle verwenden, berechnen Sie die Erfassungsrate anhand der Anzahl der verwendeten Hubgeräte, um sicherzugehen, dass die Rate unter 0,5 MBit/s pro Partitionseinschränkung in der Preview fällt.
> * Auch wenn mehrere Ereignisse gleichzeitig eintreffen, wird das Limit der Vorschauversion nicht überschritten.

  ![IoT Hub-Partitionsdiagramm](media/concepts-ingress-overview/iot-hub-partiton-diagram.png)

Unter den folgenden Ressourcen finden Sie weitere Informationen zur Optimierung des Hub-Durchsatzes und der zugehörigen Partitionen:

* [Skalieren von Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-scaling)
* [Skalierbarkeit – Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-scalability#throughput-units)
* [Event Hub-Partitionen](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#partitions)

### <a name="data-storage"></a>Datenspeicher

Bei der Erstellung einer Time Series Insights Preview-Umgebung mit einer SKU mit *nutzungsbasierter Bezahlung* erstellen Sie zwei Azure-Ressourcen:

* Eine Azure Time Series Insights Preview-Umgebung, die für die Speicherung von warmen Daten (Warm Storage) konfiguriert werden kann.
* Ein Azure Storage-Blobkonto vom Typ „Universell V1“ für die Speicherung von kalten Daten.

Daten im warmen Speicher sind nur über [Zeitreihenabfragen](./time-series-insights-update-tsq.md) und den [Azure Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md) verfügbar. Der warme Speicher enthält aktuelle Daten innerhalb des beim Erstellen der Time Series Insights-Umgebung ausgewählten [Aufbewahrungszeitraums](./time-series-insights-update-plan.md#the-preview-environment).

Time Series Insights Preview speichert kalte Speicherdaten im [Parquet-Dateiformat](#parquet-file-format-and-folder-structure) im Azure-Blobspeicher. Time Series Insights Preview verwaltet diese kalten Speicherdaten exklusiv. Sie können diese Daten jedoch direkt als standardmäßige Parquet-Dateien lesen.

> [!WARNING]
> Als Besitzer des Azure-Blobspeicherkontos, in dem sich die kalten Daten befinden, haben Sie Vollzugriff auf alle Daten im Konto. Dieser Zugriff umfasst Berechtigungen zum Schreiben und Löschen. Bearbeiten oder löschen Sie keine von Time Series Insights Preview geschriebenen Daten, da dies zu Datenverlusten führen kann.

### <a name="data-availability"></a>Datenverfügbarkeit

Azure Time Series Insights Preview partitioniert und indiziert Daten, um eine optimale Abfrageleistung zu erzielen. Die Daten können nach der Indizierung sowohl aus dem warmen (sofern aktiviert) als auch aus dem kalten Speicher abgefragt werden. Die Menge an erfassten Daten kann sich auf diese Verfügbarkeit auswirken.

> [!IMPORTANT]
> Während der Vorschauphase kann es bis zu 60 Sekunden dauern, bis Daten verfügbar werden. Wenn Sie eine erhebliche Latenz von weit über 60 Sekunden feststellen, übermitteln Sie ein Supportticket über das Azure-Portal.

## <a name="azure-storage"></a>Azure Storage

Dieser Abschnitt enthält für Azure Time Series Insights Preview relevante Informationen zu Azure Storage.

Eine ausführliche Beschreibung des Azure Blobspeichers finden Sie in der [Einführung in Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md).

### <a name="your-storage-account"></a>Ihr Speicherkonto

Wenn Sie eine Azure Time Series Insights Preview-Umgebung mit nutzungsbasierter Bezahlung erstellen, wird ein Azure Storage-Blobkonto vom Typ „Universell V1“ als langfristiger Speicher für kalte Daten erstellt.  

Azure Time Series Insights Preview behält bis zu zwei Kopien jedes Ereignisses in Ihrem Azure Storage-Konto bei. Eine Kopie speichert Ereignisse nach Erfassungszeit, wobei der Zugriff auf Ereignisse in einer zeitlich geordneten Sequenz immer zulässig ist. Im Laufe der Zeit wird in Time Series Insights Preview auch eine neu partitionierte Kopie der Daten erstellt, um die leistungsfähige Time Series Insights-Abfragen zu optimieren. 

Während der öffentlichen Vorschauphase werden Daten mit unbegrenzter Dauer in Ihrem Azure-Speicherkonto gespeichert.

#### <a name="writing-and-editing-time-series-insights-blobs"></a>Schreiben und Bearbeiten von Time Series Insights-Blobs

Um Abfrageleistung und Datenverfügbarkeit sicherzustellen, bearbeiten oder löschen Sie keine Blobs, die von Time Series Insights Preview erstellt werden.

#### <a name="accessing-time-series-insights-preview-cold-store-data"></a>Zugreifen auf Daten des kalten Speichers von Time Series Insights Preview 

Zusätzlich zum Zugriff auf Ihre Daten über den [Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md) und [Time Series-Abfragen](./time-series-insights-update-tsq.md) können Sie auch direkt über die im kalten Speicher gespeicherten Parquet-Dateien auf Ihre Daten zugreifen. Beispielsweise können Sie Daten in einem Jupyter-Notebook lesen, transformieren und bereinigen und dann verwenden, um Ihr Azure Machine Learning-Modell im gleichen Spark-Workflow zu trainieren.

Um direkt über Ihr Azure Storage-Konto auf Daten zuzugreifen, benötigen Sie Lesezugriff auf das Konto, das zum Speichern Ihrer Time Series Insights Preview-Daten verwendet wird. Sie können dann ausgewählte Daten basierend auf der Erstellungszeit der Parquet-Datei lesen, die sich im unten im Abschnitt [Parquet-Dateiformat](#parquet-file-format-and-folder-structure) beschriebenen Ordner `PT=Time` befindet.  Weitere Informationen zum Aktivieren des Lesezugriffs auf Ihr Speicherkonto finden Sie unter [Verwalten des Zugriffs auf Ihre Speicherkontoressourcen](../storage/blobs/storage-manage-access-to-resources.md).

#### <a name="data-deletion"></a>Löschen von Daten

Löschen Sie Ihre Time Series Insights Preview-Daten nicht. Verwalten Sie zugehörige Daten nur innerhalb von Time Series Insights Preview.

### <a name="parquet-file-format-and-folder-structure"></a>Parquet-Dateiformat und Ordnerstruktur

Parquet ist ein spaltenbasiertes Open-Source-Dateiformat, das für effiziente Speicherung und Leistung konzipiert wurde. Time Series Insights Preview verwendet Parquet, um Time Series-ID-basierte Abfrageleistung im richtigen Maßstab zu ermöglichen.  

Weitere Informationen zum Parquet-Dateityp finden Sie in der [Parquet-Dokumentation](https://parquet.apache.org/documentation/latest/).

Time Series Insights Preview speichert wie folgt Kopien Ihrer Daten:

* Die erste, anfängliche Kopie wird anhand des Erfassungszeitpunkts partitioniert und speichert Daten in etwa in der Reihenfolge ihres Eingangs. Diese Daten befinden sich im Ordner `PT=Time`:

  `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

* Die zweite, neu partitionierte Kopie wird nach Time Serie-IDs gruppiert und befindet sich im Ordner `PT=TsId`:

  `V=1/PT=TsId/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

In beiden Fällen entspricht die time-Eigenschaft der Parquet-Datei der Erstellungszeit des Blobs. Daten im Ordner `PT=Time` werden ohne Änderungen beibehalten, nachdem sie in die Datei geschrieben wurden. Daten im Ordner `PT=TsId` werden im Lauf der Zeit für Abfragen optimiert und sind nicht statisch.

> [!NOTE]
> * `<YYYY>` entspricht einer vierstelligen Jahresdarstellung.
> * `<MM>` entspricht einer zweistelligen Monatsdarstellung.
> * `<YYYYMMDDHHMMSSfff>` entspricht einer Zeitstempeldarstellung mit folgendem Format: vierstellige Jahresangabe (`YYYY`), zweistellige Monatsangabe (`MM`), zweistellige Tagesangabe (`DD`), zweistellige Stundenangabe (`HH`), zweistellige Minutenangabe (`MM`), zweistellige Sekundenangabe (`SS`) und dreistellige Millisekundenangabe (`fff`).

Time Series Insights Preview-Ereignisse werden dem Inhalt von Parquet-Dateien wie folgt zugeordnet:

* Jedes Ereignis wird einer einzelnen Zeile zugeordnet.
* Jede Zeile enthält die Spalte **timestamp** mit einem Zeitstempel für das Ereignis. Die timestamp-Eigenschaft ist nie NULL. Ihr Standardwert ist der **Zeitpunkt der Einreihung des Ereignisses in die Warteschlange**, wenn die timestamp-Eigenschaft in der Ereignisquelle nicht angegeben ist. Der gespeicherte Zeitstempel wird immer in UTC angegeben.
* Jede Zeile enthält die TSID-Spalten (Time Series-ID), wie beim Erstellen der Time Series Insights-Umgebung definiert. Der TSID-Eigenschaftsname enthält das Suffix `_string`.
* Alle anderen als Telemetriedaten gesendeten Eigenschaften werden Spaltennamen zugeordnet, die je nach Eigenschaftstyp auf `_string` (Zeichenfolge), `_bool` (boolesch), `_datetime` (Datum/Uhrzeit) und `_double` (Double) enden.
* Dieses Zuordnungsschema gilt für die erste Version des Dateiformats, auf die als **V=1** verwiesen und die im Basisordner desselben Namens gespeichert wird. Wenn diese Funktion weiterentwickelt wird, kann sich dieses Zuordnungsschema ändern und der Verweisname erhöht werden.

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie [Strukturieren von JSON für Eingang und Abfragen](./time-series-insights-update-how-to-shape-events.md).

- Informieren Sie sich über die neue [Datenmodellierung](./time-series-insights-update-tsm.md).
