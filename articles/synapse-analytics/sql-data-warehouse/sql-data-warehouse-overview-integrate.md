---
title: Erstellen von integrierten Lösungen
description: Lösungstools und Partner für die Integration mit einem dedizierten SQL-Pool in Azure Synapse Analytics.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 9f92128266c41912868f6ab74abaa2d374c6d236
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93324496"
---
# <a name="integrate-other-services-with-a-dedicated-sql-pool-in-azure-synapse-analytics"></a>Integrieren Sie andere Dienste mit einem dedizierten SQL-Pool in Azure Synapse Analytics.

Ein dedizierter SQL-Pool in Azure Synapse Analytics ermöglicht Benutzern die Integration mit vielen anderen Diensten in Azure. Mit Synapse SQL können Sie über die zugehörige Ressource des dedizierten SQL-Pools ein Data Warehouse erstellen, über das dann mehrere zusätzliche Dienste genutzt werden können, z. B.:

* Power BI
* Azure Data Factory
* Azure Machine Learning
* Azure Stream Analytics

Weitere Informationen über Integrationsdienste in Azure finden Sie im Artikel zu [Integrationspartnern](sql-data-warehouse-partner-data-integration.md).

## <a name="power-bi"></a>Power BI

Mit der Power BI-Integration können Sie die Rechenleistung eines Data Warehouse mit der dynamischen Berichterstellung und Visualisierung von Power BI kombinieren. Die Power BI-Integration umfasst derzeit:

* **Direct Connect** : Eine erweiterte Verbindung mit logischem Pushdown für ein Data Warehouse, das mit einem dedizierten SQL-Pool bereitgestellt wurde. Pushdown ermöglicht schnellere Analysen im größeren Maßstab.
* **In Power BI öffnen** : Mit der Schaltfläche „In Power BI öffnen“ werden zur vereinfachten Verbindungsherstellung Instanzinformationen an Power BI übergeben.

Weitere Informationen finden Sie unter [Integration in Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md) oder in der [Power BI-Dokumentation](https://powerbi.microsoft.com/blog/exploring-azure-sql-data-warehouse-with-power-bi/).

## <a name="azure-data-factory"></a>Azure Data Factory

Azure Data Factory bietet Benutzern eine verwaltete Plattform zur Erstellung komplexer Pipelines zum Extrahieren und Laden. Die Integration mit einem dedizierten SQL-Pool in Azure Data Factory umfasst Folgendes:

* **Gespeicherte Prozeduren** : Die Orchestrierung der Ausführung von gespeicherten Prozeduren.
* **Kopieren** : Verwenden Sie ADF, um Daten in den dedizierten SQL-Pool zu verschieben. Dieser Vorgang kann im Hintergrund PolyBase oder den standardmäßigen Mechanismus von Azure Data Factory zum Verschieben von Daten verwenden.

Weitere Informationen finden Sie unter [Visualisieren von Daten mit Power BI](../../data-factory/load-azure-sql-data-warehouse.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

## <a name="azure-machine-learning"></a>Azure Machine Learning

Azure Machine Learning ist ein vollständig verwalteter Analytics-Dienst, mit dem Sie komplexe Modelle erstellen können, die einen großen Satz von Vorhersagetools nutzen. Dedizierte SQL-Pools werden sowohl als Quelle als auch als Ziel für diese Modelle unterstützt und verfügen über die folgenden Funktionen:

* **Daten lesen** : Bedarfsabhängige Laufwerkmodelle im großen Stil mit T-SQL für einen dedizierten SQL-Pool
* **Daten schreiben** : Committen von Änderungen von einem beliebigen Modell zurück an den dedizierten SQL-Pool.

Weitere Informationen finden Sie unter [Analysieren von Daten mit Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md).

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

Azure Stream Analytics ist eine komplexe, vollständig verwaltete Infrastruktur für die Verarbeitung und Nutzung von Azure Event Hub generierten Ereignisdaten.  Die Integration mit einem dedizierten SQL-Pool ermöglicht das effiziente Streamen von Daten und die Speicherung zusammen mit relationalen Daten, sodass eine tiefergehende, erweiterte Analyse durchgeführt werden kann.  

* **Auftragsausgabe** : Direktes Senden der Ausgabe von Stream Analytics-Aufträgen an einen dedizierten SQL-Pool.

Weitere Informationen finden Sie unter [Verwenden von Azure Stream Analytics mit SQL Data Warehouse](sql-data-warehouse-integrate-azure-stream-analytics.md).
