---
title: Datenstreaming in Azure SQL Edge (Vorschau)
description: Informationen zum Datenstreaming in Azure SQL Edge (Vorschau)
keywords: ''
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 15b6b80f9924b050b388d74adc1d67db6a386134
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83594509"
---
# <a name="data-streaming-in-azure-sql-edge-preview"></a>Datenstreaming in Azure SQL Edge (Vorschau)

Azure SQL Edge (Vorschau) bietet zwei verschiedene Optionen zum Implementieren von Datenstreaming. 

1. Bereitstellen von Azure Streaming Analytics Edge-Aufträgen, die in Azure erstellt wurden. Weitere Informationen zum Bereitstellen von Azure Stream Analytics Edge-Aufträgen in Azure SQL Edge finden Sie unter [Bereitstellen von Azure Stream Analytics-Aufträgen](deploy-dacpac.md).
2. Verwenden der neuen **T-SQL Streaming**-Funktion zum Erstellen von Streamingaufträgen in SQL Edge, ohne dass Streamingaufträge in Azure konfiguriert werden müssen. 

Obwohl es möglich ist, beide Optionen zum Implementieren von Datenstreamingfunktionen in SQL Edge zu verwenden, wird dringend empfohlen, nur eine Option zu verwenden. Wenn Sie beide Optionen verwenden, kann es möglicherweise zu Racebedingungen kommen, die sich auf die Funktionsweise der Datenstreamingvorgänge auswirken.

Der Rest dieses Dokuments bezieht sich auf das neue Feature **T-SQL Streaming**, das Datenstreaming in Echtzeit-, Analysen und Ereignisverarbeitung bereitstellt, um große Mengen schneller Streamingdaten aus mehreren Quellen gleichzeitig zu analysieren und zu verarbeiten. *T-SQL Streaming* basiert auf der gleichen Hochleistungsstreaming-Engine wie [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-introduction) in Microsoft Azure und unterstützt eine ähnliche Reihe von Funktionen, die von Azure Stream Analytics in der Edge-Umgebung bereitgestellt werden.

Wie Azure Stream Analytics ermöglicht T-SQL Streaming das Erkennen von Mustern und Beziehungen in Informationen, die aus einer Reihe von IoT-Eingabequellen extrahiert werden, einschließlich Geräten, Sensoren und Anwendungen. Diese Muster können verwendet werden, um Aktionen auszulösen und Workflows einzuleiten, etwa das Erstellen von Warnungen, das Einspeisen von Informationen in eine Berichts- oder Visualisierungslösung oder das Speichern der Daten zur späteren Verwendung. 

Die folgenden Szenarien sind Beispiele für Situationen, in denen T-SQL Streaming verwendet werden kann:

* Analysieren von Telemetriedatenströmen von IoT-Geräten in Echtzeit.
* Echtzeitanalyse von Daten, die von autonomen und fahrerlosen Fahrzeugen generiert werden.
* Remoteüberwachung und Predictive Maintenance hochwertigen Industrie- oder Produktionsanlagen.
* Anomalieerkennung und/oder Mustererkennung von IoT-Sensormessungen in der Landwirtschaft oder einem Energiebetrieb.

## <a name="how-does-t-sql-streaming-work"></a>Wie funktioniert T-SQL Streaming?

T-SQL Streaming funktioniert genau wie [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-introduction#how-does-stream-analytics-work), beispielsweise wird das Konzept von Streamingaufträgen für die Verarbeitung von Datenstreaming in Echtzeit- verwendet. 

Ein Stream Analytics-Auftrag besteht aus den folgenden Elementen:

- Streameingabe: Eine Streameingabe definiert die Verbindungen mit einer Datenquelle, aus der der Datenstrom gelesen werden soll. Azure SQL Edge unterstützt zurzeit die folgenden Streameingabetypen:
    - Edge Hub
    - Kafka: Unterstützung für Kafka-Eingaben ist zurzeit nur für Intel-/AMD64-Versionen von Azure SQL Edge verfügbar.

- Streamausgabe: Eine Streamausgabe definiert die Verbindungen mit einer Datenquelle, in die der Datenstrom geschrieben werden soll. Azure SQL Edge unterstützt zurzeit die folgenden Streamausgabetypen:
    - Edge Hub
    - SQL: Die SQL-Ausgabe kann eine lokale Datenbank innerhalb der SQL Edge-Instanz oder ein SQL-Remoteserver oder eine Azure SQL-Datenbank sein. 
    - Azure Blob Storage

- Streamabfrage: Die Streamabfrage definiert die Transformation, Aggregationen, Filter, Sortierung und Joins, die auf den Eingabestream angewendet werden müssen, bevor er in die Streamausgabe geschrieben wird. Die Streamabfrage basiert auf derselben Abfragesprache, die von Azure Stream Analytics verwendet wird. Weitere Informationen zur Abfragesprache von Azure Stream Analytics finden Sie unter [Stream Analytics-Abfragesprache](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference?).

> [!IMPORTANT]
> T-SQL Streaming unterstützt im Gegensatz zu Azure Stream Analytics derzeit keine Verwendung von[Verweisdaten für Suchvorgänge](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-use-reference-data) oder [UDFs und UDAs in Streamaufträgen](https://docs.microsoft.com/azure/stream-analytics/streaming-technologies#you-want-to-write-udfs-udas-and-custom-deserializers-in-a-language-other-than-javascript-or-c).

> [!NOTE]
> T-SQL Streaming unterstützt nur eine Teilmenge der von Azure Stream Analytics unterstützten Sprachoberfläche. Weitere Informationen zur Abfragesprache von Azure Stream Analytics finden Sie unter [Stream Analytics-Abfragesprache](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference?).

## <a name="limitations-and-restrictions"></a>Einschränkungen

Die folgenden Einschränkungen gelten T-SQL Streaming. 

- Nur ein Streamingauftrag kann jeweils gleichzeitig aktiv sein. Aufträge, die bereits ausgeführt werden, müssen beendet werden, bevor ein anderer Auftrag gestartet werden kann.
- Jede Ausführung eines Streamingauftrags ist ein Singlethreadvorgang. Wenn der Streamingauftrag mehrere Abfragen enthält, wird jede Abfrage in serieller Reihenfolge ausgewertet.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen eines Stream Analytics-Auftrags in Azure SQL Edge (Vorschau)](create-stream-analytics-job.md)
- [Anzeigen von Metadaten für Streamingaufträge in Azure SQL Edge (Vorschau)](streaming-catalog-views.md)
- [Erstellen eines externen Streams](create-external-stream-transact-sql.md)