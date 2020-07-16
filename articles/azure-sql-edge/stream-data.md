---
title: Datenstreaming in Azure SQL Edge (Vorschau)
description: Erfahren Sie mehr zum Datenstreaming in Azure SQL Edge (Vorschau).
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 866c74fbdfcfcef7cbb7d6cddb360c4265a2f776
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84669612"
---
# <a name="data-streaming-in-azure-sql-edge-preview"></a>Datenstreaming in Azure SQL Edge (Vorschau)

Azure SQL Edge (Vorschau) bietet die folgenden Optionen zum Implementieren von Datenstreaming: 

- Bereitstellen von Azure Stream Analytics Edge-Aufträgen, die in Azure erstellt wurden. Weitere Informationen finden Sie unter [Bereitstellen von Azure Stream Analytics-Aufträgen](deploy-dacpac.md).
- Verwenden des neuen T-SQL-Streamings zum Erstellen von Streamingaufträgen in Azure SQL Edge, ohne dass Streamingaufträge in Azure konfiguriert werden müssen. 

Obwohl es möglich ist, beide Optionen zum Implementieren des Datenstreamings in Azure SQL Edge zu verwenden, sollten Sie nur eine Option wählen. Wenn Sie beide Optionen verwenden, kann es zu Racebedingungen kommen, die sich auf die Funktionsweise der Datenstreamingvorgänge auswirken.

T-SQL-Streaming bildet den Schwerpunkt dieses Artikels. Es ermöglicht Datenstreaming, Analysen und Ereignisverarbeitung in Echtzeit, um große Mengen schneller Streamingdaten aus mehreren Quellen gleichzeitig zu analysieren und zu verarbeiten. T-SQL-Streaming basiert auf der gleichen Hochleistungsstreaming-Engine wie [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-introduction) in Microsoft Azure. Das Feature unterstützt eine ähnliche Reihe von Funktionen, die von Azure Stream Analytics in der Edge-Umgebung geboten werden.

Wie Azure Stream Analytics erkennt T-SQL-Streaming Muster und Beziehungen in Informationen, die aus einer Reihe von IoT-Eingabequellen wie z. B. Geräten, Sensoren und Anwendungen extrahiert werden. Sie können mithilfe dieser Muster Aktionen auslösen und Workflows einleiten. Sie können z. B. Warnungen erstellen, Informationen in eine Berichts- oder Visualisierungslösung übertragen oder die Daten zur späteren Verwendung speichern. 

T-SQL-Streaming eignet sich für folgende Aufgaben:

* Analysieren von Telemetriedatenströmen von IoT-Geräten in Echtzeit
* Analysieren von Daten in Echtzeit, die von autonomen und fahrerlosen Fahrzeugen generiert werden
* Remoteüberwachung und Predictive Maintenance hochwertiger Industrie- oder Produktionsanlagen
* Anomalie- und Mustererkennung von IoT-Sensormessungen in der Landwirtschaft oder einem Energiebetrieb

## <a name="how-does-t-sql-streaming-work"></a>Wie funktioniert T-SQL-Streaming?

T-SQL-Streaming funktioniert genauso wie [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-introduction#how-does-stream-analytics-work). Beispielsweise wird das Konzept von *Streamingaufträgen* für die Verarbeitung von Datenstreaming in Echtzeit befolgt. 

Ein Stream Analytics-Auftrag besteht aus den folgenden Elementen:

- **Streameingabe**: Definiert die Verbindungen mit einer Datenquelle, aus der der Datenstrom gelesen werden soll. Azure SQL Edge unterstützt zurzeit die folgenden Streameingabetypen:
    - Edge Hub
    - Kafka (Unterstützung für Kafka-Eingaben ist zurzeit nur für Intel-/AMD64-Versionen von Azure SQL Edge verfügbar.)

- **Streamausgabe**: Definiert die Verbindungen mit einer Datenquelle, in die der Datenstrom gelesen werden soll. Azure SQL Edge unterstützt zurzeit die folgenden Streamausgabetypen:
    - Edge Hub
    - SQL (Die SQL-Ausgabe kann eine lokale Datenbank in der Azure SQL Edge-Instanz, eine SQL Server-Remoteinstanz oder eine Azure SQL-Datenbank-Instanz sein.) 
    - Azure Blob Storage

- **Streamabfrage**: Definiert die Transformation, Aggregationen, Filter, Sortierung und Joins, die auf den Eingabestream angewendet werden sollen, bevor er in die Streamausgabe geschrieben wird. Die Streamabfrage basiert auf der von Azure Stream Analytics verwendeten Abfragesprache. Weitere Informationen finden Sie unter [Stream Analytics-Abfragesprache](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference?).

> [!IMPORTANT]
> T-SQL-Streaming unterstützt im Gegensatz zu Stream Analytics derzeit keine [Verweisdaten für Suchvorgänge](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-use-reference-data) oder [benutzerdefinierten Funktionen (User Defined Functions, UDFs) und benutzerdefinierten Aggregate (User-Defined Aggregates, UDA) in Streamaufträgen](https://docs.microsoft.com/azure/stream-analytics/streaming-technologies#you-want-to-write-udfs-udas-and-custom-deserializers-in-a-language-other-than-javascript-or-c).

> [!NOTE]
> T-SQL-Streaming unterstützt nur eine Teilmenge der von Stream Analytics unterstützten Sprachoberfläche. Weitere Informationen finden Sie unter [Stream Analytics-Abfragesprache](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference?).

## <a name="limitations-and-restrictions"></a>Einschränkungen

Für T-SQL-Streaming gelten die folgenden Einschränkungen. 

- Nur ein Streamingauftrag kann jeweils gleichzeitig aktiv sein. Aufträge, die bereits laufen, müssen beendet werden, bevor ein anderer Auftrag gestartet werden kann.
- Jede Ausführung eines Streamingauftrags ist ein Singlethreadvorgang. Wenn der Streamingauftrag mehrere Abfragen enthält, wird jede Abfrage in serieller Reihenfolge ausgewertet.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen eines Stream Analytics-Auftrags in Azure SQL Edge (Vorschau)](create-stream-analytics-job.md)
- [Anzeigen von Metadaten für Streamingaufträge in Azure SQL Edge (Vorschau)](streaming-catalog-views.md)
- [Erstellen eines externen Streams](create-external-stream-transact-sql.md)