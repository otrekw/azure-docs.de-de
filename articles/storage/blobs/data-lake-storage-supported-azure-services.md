---
title: Azure-Dienste, die Azure Data Lake Storage Gen2 unterstützen | Microsoft-Dokumentation
description: Hier erfahren Sie, welche Azure-Dienste in Azure Data Lake Storage Gen2 integriert werden.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 04/07/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: edfcb1d0bfc87c84620b13ed26ec681fef32f1e3
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878322"
---
# <a name="azure-services-that-support-azure-data-lake-storage-gen2"></a>Azure-Dienste, die Azure Data Lake Storage Gen2 unterstützen

Mit Azure-Diensten können Sie Daten erfassen, Analysen durchführen und visuelle Darstellungen erstellen. Dieser Artikel enthält eine Liste mit unterstützten Azure-Diensten, nennt deren Supportebene und bietet Links zu hilfreichen Artikeln für die Verwendung dieser Dienste mit Azure Data Lake Storage Gen2.

## <a name="supported-azure-services"></a>Unterstützte Azure-Dienste

In dieser Tabelle sind die Azure-Dienste aufgeführt, die mit Azure Data Lake Storage Gen2 verwendet werden können. Die in diesen Tabellen enthaltenen Elemente ändern sich im Laufe der Zeit, da die Unterstützung laufend erweitert wird.

> [!NOTE]
> Die Supportebene bezieht sich lediglich darauf, wie der Dienst mit Data Lake Storage Gen 2 unterstützt wird.

|Azure-Dienst |Supportebene |Verwandte Artikel |
|---------------|-------------------|---|
|Azure Data Factory|Allgemein verfügbar|[Laden von Daten in Azure Data Lake Storage Gen2 mit Azure Data Factory](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure Databricks|Allgemein verfügbar|[Azure Data Lake Storage Gen2](https://docs.azuredatabricks.net/data/data-sources/azure/azure-datalake-gen2.html) <br> [Schnellstart: Analysieren von Daten in Azure Data Lake Storage Gen2 mit Azure Databricks](data-lake-storage-quickstart-create-databricks-account.md) <br>[Tutorial: Extrahieren, Transformieren und Laden von Daten mithilfe von Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/databricks-extract-load-sql-data-warehouse) <br>[Tutorial: Zugreifen auf Daten vom Typ „Data Lake Storage Gen2“ mit Azure Databricks unter Verwendung von Spark](data-lake-storage-use-databricks-spark.md)|
|Azure Event Hub|Allgemein verfügbar|[Erfassen von Ereignissen über Azure Event Hubs in Azure Blob Storage oder Azure Data Lake Storage](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview)|
|Azure Event Grid|Allgemein verfügbar|[Tutorial: Implementieren des Data Lake-Erfassungsmusters zum Aktualisieren einer Databricks Delta-Tabelle](data-lake-storage-events.md)|
|Azure Logic Apps|Allgemein verfügbar|[Übersicht: Was ist Azure Logic Apps?](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview)|
|Azure Machine Learning|Allgemein verfügbar|[Zugreifen auf Daten in Azure Storage-Diensten](https://docs.microsoft.com/azure/machine-learning/how-to-access-data)|
|Azure Stream Analytics|Allgemein verfügbar|[Schnellstart: Erstellen eines Stream Analytics-Auftrags mithilfe des Azure-Portals](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-quick-create-portal) <br> [Blobspeicher und Azure Data Lake Gen2](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-define-outputs#blob-storage-and-azure-data-lake-gen2)|
|Data Box|Allgemein verfügbar|[Verwenden von Azure Data Box zum Migrieren von Daten aus einem lokalen Hadoop Distributed File System-Speicher zu Azure Storage](data-lake-storage-migrate-on-premises-hdfs-cluster.md)|
|HDInsight |Allgemein verfügbar|[Verwenden von Azure Data Lake Storage Gen2 mit Azure HDInsight-Clustern](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)<br>[Verwenden der HDFS-CLI mit Data Lake Storage Gen2](data-lake-storage-use-hdfs-data-lake-storage.md) <br>[Tutorial: Extrahieren, Transformieren und Laden von Daten mithilfe von Apache Hive in Azure HDInsight](data-lake-storage-tutorial-extract-transform-load-hive.md)|
|IoT Hub |Allgemein verfügbar|[Verwenden des IoT Hub-Nachrichtenroutings zum Senden von Gerät-zu-Cloud-Nachrichten an verschiedene Endpunkte](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)|
|Power BI|Allgemein verfügbar|[Analysieren von Daten in Azure Data Lake Storage Gen2 mit Power BI](https://docs.microsoft.com/power-query/connectors/datalakestorage)|
|SQL Data Warehouse|Allgemein verfügbar|[Azure SQL Data Warehouse – PolyBase](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#azure-sql-data-warehouse-polybase)|
|SQL Server Integration Services (SSIS)|Allgemein verfügbar|[Azure Storage-Verbindungs-Manager](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-storage-connection-manager?view=sql-server-2017)|
|Azure Cognitive Search|Vorschau|[Indizieren von Dokumenten in Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/search/search-howto-index-azure-data-lake-storage)|
|Azure-Daten-Explorer|Vorschau|[Abfragen von Daten in Azure Data Lake mit Azure Data Explorer](https://docs.microsoft.com/azure/data-explorer/data-lake-query-data)|
|Azure Content Delivery Network|Noch nicht unterstützt|[Indizieren von Dokumenten in Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/cdn/cdn-overview)|


## <a name="see-also"></a>Weitere Informationen

- [Bekannte Probleme mit Azure Data Lake Storage Gen2](data-lake-storage-known-issues.md)
- [Verfügbare Blob Storage-Features in Azure Data Lake Storage Gen2](data-lake-storage-supported-blob-storage-features.md)
- [Open-Source-Plattformen, die Azure Data Lake Storage Gen2 unterstützen](data-lake-storage-supported-open-source-platforms.md)
- [Multiprotokollzugriff für Azure Data Lake Storage](data-lake-storage-multi-protocol-access.md)