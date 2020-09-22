---
title: Azure-Dienste, die Azure Data Lake Storage Gen2 unterstützen | Microsoft-Dokumentation
description: Hier erfahren Sie, welche Azure-Dienste in Azure Data Lake Storage Gen2 integriert werden.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 08/05/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: c3a5f3a984c95af400c9e0c1543e3c1883290668
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2020
ms.locfileid: "89442955"
---
# <a name="azure-services-that-support-azure-data-lake-storage-gen2"></a>Azure-Dienste, die Azure Data Lake Storage Gen2 unterstützen

Mit Azure-Diensten können Sie Daten erfassen, Analysen durchführen und visuelle Darstellungen erstellen. Dieser Artikel enthält eine Liste mit unterstützten Azure-Diensten, nennt deren Supportebene und bietet Links zu hilfreichen Artikeln für die Verwendung dieser Dienste mit Azure Data Lake Storage Gen2.

## <a name="supported-azure-services"></a>Unterstützte Azure-Dienste

In dieser Tabelle sind die Azure-Dienste aufgeführt, die mit Azure Data Lake Storage Gen2 verwendet werden können. Die in diesen Tabellen enthaltenen Elemente ändern sich im Laufe der Zeit, da die Unterstützung laufend erweitert wird.

> [!NOTE]
> Die Supportebene bezieht sich lediglich darauf, wie der Dienst mit Data Lake Storage Gen 2 unterstützt wird.

|Azure-Dienst |Supportebene |Azure AD |Gemeinsam verwendeter Schlüssel| Verwandte Artikel |
|---------------|-------------------|---|---|---|
|Azure Data Factory|Allgemein verfügbar|Ja|Ja|[Laden von Daten in Azure Data Lake Storage Gen2 mit Azure Data Factory](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure Databricks|Allgemein verfügbar|Ja|Ja|[Azure Data Lake Storage Gen2](https://docs.azuredatabricks.net/data/data-sources/azure/azure-datalake-gen2.html) <br> [Schnellstart: Analysieren von Daten in Azure Data Lake Storage Gen2 mit Azure Databricks](data-lake-storage-quickstart-create-databricks-account.md) <br>[Tutorial: Extrahieren, Transformieren und Laden von Daten mithilfe von Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/databricks-extract-load-sql-data-warehouse) <br>[Tutorial: Zugreifen auf Daten vom Typ „Data Lake Storage Gen2“ mit Azure Databricks unter Verwendung von Spark](data-lake-storage-use-databricks-spark.md)|
|Azure Event Hub|Allgemein verfügbar|Nein|Ja|[Erfassen von Ereignissen über Azure Event Hubs in Azure Blob Storage oder Azure Data Lake Storage](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview)|
|Azure Event Grid|Allgemein verfügbar|Ja|Ja|[Tutorial: Implementieren des Data Lake-Erfassungsmusters zum Aktualisieren einer Databricks Delta-Tabelle](data-lake-storage-events.md)|
|Azure Logic Apps|Allgemein verfügbar|Nein|Ja|[Übersicht: Was ist Azure Logic Apps?](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview)|
|Azure Machine Learning|Allgemein verfügbar|Ja|Ja|[Zugreifen auf Daten in Azure Storage-Diensten](https://docs.microsoft.com/azure/machine-learning/how-to-access-data)|
|Azure Stream Analytics|Allgemein verfügbar|Ja|Ja|[Schnellstart: Erstellen eines Stream Analytics-Auftrags mithilfe des Azure-Portals](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-quick-create-portal) <br> [Blobspeicher und Azure Data Lake Gen2](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-define-outputs#blob-storage-and-azure-data-lake-gen2)|
|Data Box|Allgemein verfügbar|Nein|Ja|[Verwenden von Azure Data Box zum Migrieren von Daten aus einem lokalen Hadoop Distributed File System-Speicher zu Azure Storage](data-lake-storage-migrate-on-premises-hdfs-cluster.md)|
|HDInsight |Allgemein verfügbar|Ja|Ja|[Verwenden von Azure Data Lake Storage Gen2 mit Azure HDInsight-Clustern](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)<br>[Verwenden der HDFS-CLI mit Data Lake Storage Gen2](data-lake-storage-use-hdfs-data-lake-storage.md) <br>[Tutorial: Extrahieren, Transformieren und Laden von Daten mithilfe von Apache Hive in Azure HDInsight](data-lake-storage-tutorial-extract-transform-load-hive.md)|
|IoT Hub |Allgemein verfügbar|Nein|Ja|[Verwenden des IoT Hub-Nachrichtenroutings zum Senden von Gerät-zu-Cloud-Nachrichten an verschiedene Endpunkte](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)|
|Power BI|Allgemein verfügbar|Ja|Ja|[Analysieren von Daten in Azure Data Lake Storage Gen2 mit Power BI](https://docs.microsoft.com/power-query/connectors/datalakestorage)|
|Azure Synapse Analytics (ehemals SQL Data Warehouse)|Allgemein verfügbar|Ja|Ja|[Verwenden von Azure Synapse Analytics (vormals SQL Data Warehouse)](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#azure-sql-data-warehouse-polybase)|
|SQL Server Integration Services (SSIS)|Allgemein verfügbar|Ja|Ja|[Azure Storage-Verbindungs-Manager](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-storage-connection-manager?view=sql-server-2017)|
|Azure-Daten-Explorer|Allgemein verfügbar|Ja|Ja|[Abfragen von Daten in Azure Data Lake mit Azure Data Explorer](https://docs.microsoft.com/azure/data-explorer/data-lake-query-data)|
|Azure Cognitive Search|Vorschau|Ja|Ja|[Indizieren von Dokumenten in Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/search/search-howto-index-azure-data-lake-storage)|
|Azure Content Delivery Network|Noch nicht unterstützt|Nicht verfügbar|Nicht verfügbar|[Indizieren von Dokumenten in Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/cdn/cdn-overview)|

## <a name="see-also"></a>Weitere Informationen

- [Bekannte Probleme mit Azure Data Lake Storage Gen2](data-lake-storage-known-issues.md)
- [Verfügbare Blob Storage-Features in Azure Data Lake Storage Gen2](data-lake-storage-supported-blob-storage-features.md)
- [Open-Source-Plattformen, die Azure Data Lake Storage Gen2 unterstützen](data-lake-storage-supported-open-source-platforms.md)
- [Multiprotokollzugriff für Azure Data Lake Storage](data-lake-storage-multi-protocol-access.md)