---
title: 'Übersicht über Speicher: Azure Time Series Insights | Microsoft-Dokumentation'
description: Erfahren Sie mehr über Datenspeicherung in Azure Time Series Insights.
author: esung22
ms.author: elsung
manager: diegoviso
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 06/03/2020
ms.custom: seodec18
ms.openlocfilehash: fc6b6b42293b4f2028f1a12af950e96e28febc87
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86085564"
---
# <a name="data-storage"></a>Datenspeicherung

Bei der Erstellung einer Time Series Insights Preview-Umgebung mit einer SKU mit *nutzungsbasierter Bezahlung* erstellen Sie zwei Azure-Ressourcen:

* Eine Azure Time Series Insights-Umgebung, die für die Speicherung von warmen Daten (Warm Storage) konfiguriert werden kann.
* Ein Azure Storage-Konto für die Speicherung von kalten Daten (Cold Storage).

Daten im warmen Speicher sind nur über [Zeitreihenabfragen](./time-series-insights-update-tsq.md) und den [Azure Time Series Insights-Explorer](./time-series-insights-update-explorer.md) verfügbar. Der warme Speicher enthält aktuelle Daten innerhalb des beim Erstellen der Time Series Insights-Umgebung ausgewählten [Aufbewahrungszeitraums](./time-series-insights-update-plan.md#the-preview-environment).

Azure Time Series Insights speichert kalte Speicherdaten im [Parquet-Dateiformat](#parquet-file-format-and-folder-structure) im Azure-Blobspeicher. Time Series Insights verwaltet diese kalten Speicherdaten exklusiv. Sie können diese Daten jedoch direkt als standardmäßige Parquet-Dateien lesen.

> [!WARNING]
> Als Besitzer des Azure-Blobspeicherkontos, in dem sich die kalten Daten befinden, haben Sie Vollzugriff auf alle Daten im Konto. Dieser Zugriff umfasst Berechtigungen zum Schreiben und Löschen. Bearbeiten oder löschen Sie keine von Azure Time Series Insights geschriebenen Daten, da dies zu Datenverlusten führen kann.

## <a name="data-availability"></a>Datenverfügbarkeit

Azure Time Series Insights partitioniert und indiziert Daten, um eine optimale Abfrageleistung zu erzielen. Die Daten können nach der Indizierung sowohl aus dem warmen (sofern aktiviert) als auch aus dem kalten Speicher abgefragt werden. Die Menge an erfassten Daten kann sich auf diese Verfügbarkeit auswirken.

> [!IMPORTANT]
> Es kann bis zu 60 Sekunden dauern, bis Daten verfügbar werden. Wenn Sie eine erhebliche Latenz von weit über 60 Sekunden feststellen, übermitteln Sie ein Supportticket über das Azure-Portal.

## <a name="azure-storage"></a>Azure Storage

Dieser Abschnitt enthält für Azure Time Series Insights relevante Azure Storage-Informationen.

Eine ausführliche Beschreibung des Azure Blobspeichers finden Sie in der [Einführung in Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md).

### <a name="your-storage-account"></a>Ihr Speicherkonto

Wenn Sie eine Azure Time Series Insights-Umgebung mit nutzungsbasierter Bezahlung erstellen, wird ein Azure Storage-Konto als langfristiger Speicher für kalte Daten erstellt.  

Azure Time Series Insights behält bis zu zwei Kopien jedes Ereignisses in Ihrem Azure Storage-Konto bei. Eine Kopie speichert Ereignisse nach Erfassungszeit, wobei der Zugriff auf Ereignisse in einer zeitlich geordneten Sequenz immer zulässig ist. Im Laufe der Zeit wird in Time Series Insights auch eine neu partitionierte Kopie der Daten erstellt, um die leistungsfähige Time Series Insights-Abfragen zu optimieren.

Alle Ihre Daten werden mit unbegrenzter Dauer in Ihrem Azure Storage-Konto gespeichert.

#### <a name="writing-and-editing-time-series-insights-blobs"></a>Schreiben und Bearbeiten von Time Series Insights-Blobs

Um Abfrageleistung und Datenverfügbarkeit sicherzustellen, bearbeiten oder löschen Sie keine Blobs, die von Time Series Insights erstellt werden.

#### <a name="accessing-time-series-insights-cold-store-data"></a>Zugreifen auf Daten des kalten Speichers von Time Series Insights

Zusätzlich zum Zugriff auf Ihre Daten über den [Time Series Insights-Explorer](./time-series-insights-update-explorer.md) und [Zeitreihenabfragen](./time-series-insights-update-tsq.md) können Sie auch direkt über die im kalten Speicher gespeicherten Parquet-Dateien auf Ihre Daten zugreifen. Beispielsweise können Sie Daten in einem Jupyter-Notebook lesen, transformieren und bereinigen und dann verwenden, um Ihr Azure Machine Learning-Modell im gleichen Spark-Workflow zu trainieren.

Um direkt über Ihr Azure Storage-Konto auf Daten zuzugreifen, benötigen Sie Lesezugriff auf das Konto, das zum Speichern Ihrer Time Series Insights Preview-Daten verwendet wird. Sie können dann ausgewählte Daten basierend auf der Erstellungszeit der Parquet-Datei lesen, die sich im unten im Abschnitt [Parquet-Dateiformat](#parquet-file-format-and-folder-structure) beschriebenen Ordner `PT=Time` befindet.  Weitere Informationen zum Aktivieren des Lesezugriffs auf Ihr Speicherkonto finden Sie unter [Verwalten des Zugriffs auf Ihre Speicherkontoressourcen](../storage/blobs/storage-manage-access-to-resources.md).

#### <a name="data-deletion"></a>Löschen von Daten

Löschen Sie Ihre Azure Time Series Insights-Daten nicht. Verwalten Sie zugehörige Daten nur innerhalb von Azure Time Series Insights.

### <a name="parquet-file-format-and-folder-structure"></a>Parquet-Dateiformat und Ordnerstruktur

Parquet ist ein spaltenbasiertes Open-Source-Dateiformat, das für effiziente Speicherung und Leistung konzipiert wurde. Azure Time Series Insights verwendet Parquet, um Time Series-ID-basierte Abfrageleistung im richtigen Maßstab zu ermöglichen.  

Weitere Informationen zum Parquet-Dateityp finden Sie in der [Parquet-Dokumentation](https://parquet.apache.org/documentation/latest/).

Azure Time Series Insights speichert wie folgt Kopien Ihrer Daten:

* Die erste, anfängliche Kopie wird anhand des Erfassungszeitpunkts partitioniert und speichert Daten in etwa in der Reihenfolge ihres Eingangs. Diese Daten befinden sich im Ordner `PT=Time`:

  `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

* Die zweite, neu partitionierte Kopie wird nach Time Serie-IDs gruppiert und befindet sich im Ordner `PT=TsId`:

  `V=1/PT=TsId/<TSI_INTERNAL_NAME>.parquet`

Der Zeitstempel in den Blobnamen im Ordner `PT=Time` entspricht der Eingangszeit der Daten bei Azure Time Series Insights und nicht dem Zeitstempel der Ereignisse.

Daten im Ordner `PT=TsId` werden im Lauf der Zeit für Abfragen optimiert und sind nicht statisch. Während der Neupartitionierung können einige Ereignisse in mehreren Blobs vorhanden sein. Es wird nicht garantiert, dass die Namen der Blobs in diesem Ordner unverändert bleiben. 

Wenn Sie über Parquet-Dateien direkt auf Daten zugreifen müssen, verwenden Sie im Allgemeinen den Ordner `PT=Time`.  Zukünftige Funktionen werden einen effizienten Zugriff auf den Ordner `PT=TsId` ermöglichen. 

> [!NOTE]
>
> * `<YYYY>` entspricht einer vierstelligen Jahresdarstellung.
> * `<MM>` entspricht einer zweistelligen Monatsdarstellung.
> * `<YYYYMMDDHHMMSSfff>` entspricht einer Zeitstempeldarstellung mit folgendem Format: vierstellige Jahresangabe (`YYYY`), zweistellige Monatsangabe (`MM`), zweistellige Tagesangabe (`DD`), zweistellige Stundenangabe (`HH`), zweistellige Minutenangabe (`MM`), zweistellige Sekundenangabe (`SS`) und dreistellige Millisekundenangabe (`fff`).

Time Series Insights Preview-Ereignisse werden dem Inhalt von Parquet-Dateien wie folgt zugeordnet:

* Jedes Ereignis wird einer einzelnen Zeile zugeordnet.
* Jede Zeile enthält die Spalte **timestamp** mit einem Zeitstempel für das Ereignis. Die timestamp-Eigenschaft ist nie NULL. Ihr Standardwert ist der **Zeitpunkt der Einreihung des Ereignisses in die Warteschlange**, wenn die Zeitstempeleigenschaft in der Ereignisquelle nicht angegeben ist. Der gespeicherte Zeitstempel wird immer in UTC angegeben.
* Jede Zeile enthält die TSID-Spalten (Time Series-ID), wie beim Erstellen der Time Series Insights-Umgebung definiert. Der TSID-Eigenschaftsname enthält das Suffix `_string`.
* Alle anderen als Telemetriedaten gesendeten Eigenschaften werden Spaltennamen zugeordnet, die je nach Eigenschaftstyp auf `_bool` (boolesch), `_datetime` (Zeitstempel), `_long` (long), `_double` (double), `_string` (Zeichenfolge) oder `dynamic` (dynamisch) enden.  Weitere Informationen finden Sie unter [Unterstützte Datentypen](./concepts-supported-data-types.md).
* Dieses Zuordnungsschema gilt für die erste Version des Dateiformats, auf die als **V=1** verwiesen und die im Basisordner desselben Namens gespeichert wird. Wenn diese Funktion weiterentwickelt wird, kann sich dieses Zuordnungsschema ändern und der Verweisname erhöht werden.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur [Datenmodellierung](./time-series-insights-update-tsm.md).

* Planen Ihrer [Azure Time Series Insights Preview-Umgebung](./time-series-insights-update-plan.md).
