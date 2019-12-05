---
title: Überwachen von Azure Cosmos DB | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Leistung und Verfügbarkeit von Azure Cosmos DB überwachen.
author: bwren
services: cosmos-db
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/11/2019
ms.author: bwren
ms.custom: subject-monitoring
ms.openlocfilehash: 9a36b46d11657ef52051f8bf8df1e4944051da23
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/24/2019
ms.locfileid: "74454263"
---
# <a name="monitoring-azure-cosmos-db"></a>Überwachen von Azure Cosmos DB
Wenn Sie über unternehmenskritische Anwendungen und Geschäftsprozesse verfügen, die auf Azure-Ressourcen beruhen, sollten Sie Verfügbarkeit, Leistung und Betrieb dieser Ressourcen überwachen. In diesem Artikel wird das Überwachen von Daten beschrieben, die von Azure Cosmos-Datenbanken generiert wurden. Außerdem wird erläutert, wie Sie die Funktionen von Azure Monitor nutzen können, um diese Daten zu analysieren und Warnungen dafür zu erstellen.

## <a name="what-is-azure-monitor"></a>Was ist Azure Monitor?
Azure Cosmos DB erstellt Überwachungsdaten mit [Azure Monitor](../azure-monitor/overview.md), einem vollständigen Überwachungsdienst in Azure, der sämtliche Features für das Überwachen Ihrer Azure-Ressourcen sowie der Ressourcen in anderen Clouds und lokaler Ressourcen bereitstellt. 

Wenn Sie mit der Überwachung von Azure-Diensten noch nicht vertraut sind, beginnen Sie mit dem Artikel [Überwachen von Azure-Ressourcen mit Azure Monitor](../azure-monitor/insights/monitor-azure-resource.md), in dem die folgenden Punkte beschrieben werden:

- Was ist Azure Monitor?
- Kosten für die Überwachung
- In Azure gesammelte Überwachungsdaten
- Konfigurieren der Datensammlung
- Standardtools in Azure zum Analysieren von Überwachungsdaten sowie zum Generieren von Warnungen

Die folgenden Abschnitte basieren auf diesem Artikel, indem sie die spezifischen, von Azure Cosmos DB gesammelten Daten beschreiben, und Beispiele für die Konfiguration der Datensammlung und die Analyse dieser Daten mit Azure-Tools bereitstellen.

## <a name="azure-monitor-for-cosmos-db-preview"></a>Azure Monitor für Cosmos DB (Vorschauversion)
[Azure Monitor für Azure Cosmos DB](../azure-monitor/insights/cosmosdb-insights-overview.md) basiert auf der [Arbeitsmappenfunktion von Azure Monitor](../azure-monitor/app/usage-workbooks.md) und verwendet dieselben Überwachungsdaten, die für Cosmos DB gesammelt wurden, wie in den folgenden Abschnitten beschrieben. Verwenden Sie eine Übersicht über Gesamtleistung, Fehler, Kapazität und Betriebsintegrität aller Ihrer Azure Cosmos DB-Ressourcen in einer vereinheitlichten interaktiven Oberfläche, und nutzen Sie die anderen Features von Azure Monitor zu ausführlichen Analyse und zum Generieren von Warnungen. 

![Azure Monitor für Cosmos DB](media/monitor-cosmos-db/azure-monitor-cosmos-db.png)

## <a name="monitoring-data-collected-from-azure-cosmos-db"></a>Von Azure Cosmos DB gesammelte Überwachungsdaten
Azure Cosmos DB sammelt dieselben Arten von Überwachungsdaten wie andere Azure-Ressourcen, die unter [Überwachen von Daten von Azure-Ressourcen](../azure-monitor/insights/monitor-azure-resource.md#monitoring-data) beschrieben sind. Eine ausführliche Referenz zu den Protokollen und Metriken, die von Azure Cosmos DB erstellt werden, finden Sie unter [Überwachen von Azure Cosmos DB-Daten – Referenz](monitor-cosmos-db-reference.md).

Die Seite **Übersicht** im Azure-Portal für jede Azure Cosmos-Datenbank enthält eine kurze Übersicht über die Datenbanknutzung einschließlich ihrer Anforderung und der Nutzung der stündlichen Abrechnung. Dies sind hilfreiche Informationen, die aber nur einen kleinen Teil der verfügbaren Überwachungsdaten ausmachen. Einige dieser Daten werden automatisch gesammelt und stehen für die Analyse zur Verfügung, sobald Sie die Datenbank erstellen, während Sie mit einer gewissen Konfiguration zusätzliche Datensammlungen aktivieren können.

![Seite „Übersicht“](media/monitor-cosmos-db/overview-page.png)



## <a name="diagnostic-settings"></a>Diagnoseeinstellungen
Plattformmetriken und das Aktivitätsprotokoll werden automatisch gesammelt, aber Sie müssen eine Diagnoseeinstellung erstellen, um Ressourcenprotokolle zu sammeln oder außerhalb von Azure Monitor weiterzuleiten. Ausführliche Informationen zum Erstellen einer Diagnoseeinstellung über das Azure-Portal, die Befehlszeilenschnittstelle oder PowerShell finden Sie unter [Erstellen einer Diagnoseeinstellung zum Sammeln von Plattformprotokollen und Metriken in Azure](../azure-monitor/platform/diagnostic-settings.md).

Wenn Sie eine Diagnoseeinstellung erstellen, legen Sie fest, welche Kategorien von Protokollen gesammelt werden sollen. Die Kategorien für Azure Cosmos DB sind unten zusammen mit Beispieldaten aufgeführt.

 * **DataPlaneRequests**: Verwenden Sie diese Option, um Back-End-Anforderungen an alle APIs in Azure Cosmos DB zu protokollieren, worin SQL-, Graph-, MongoDB-, Cassandra- und Tabellen-API-Konten einbezogen sind. Wichtige zu beachtende Eigenschaften sind: Requestcharge, statusCode, clientIPaddress und partitionID.

    ```
    { "time": "2019-04-23T23:12:52.3814846Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "DataPlaneRequests", "operationName": "ReadFeed", "properties": {"activityId": "66a0c647-af38-4b8d-a92a-c48a805d6460","requestResourceType": "Database","requestResourceId": "","collectionRid": "","statusCode": "200","duration": "0","userAgent": "Microsoft.Azure.Documents.Common/2.2.0.0","clientIpAddress": "10.0.0.24","requestCharge": "1.000000","requestLength": "0","responseLength": "372","resourceTokenUserRid": "","region": "East US","partitionId": "062abe3e-de63-4aa5-b9de-4a77119c59f8","keyType": "PrimaryReadOnlyMasterKey","databaseName": "","collectionName": ""}}
    ```

* **MongoRequests**: Verwenden Sie diese Option, um vom Benutzer initiierte Anforderungen vom Front-End zum Erfüllen von Anforderungen an die API für MongoDB von Azure Cosmos DB zu protokollieren. MongoDB-Anforderungen werden in MongoRequests sowie DataPlaneRequests angezeigt. Wichtige zu beachtende Eigenschaften sind: Requestcharge, opCode.

    ```
    { "time": "2019-04-10T15:10:46.7820998Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "MongoRequests", "operationName": "ping", "properties": {"activityId": "823cae64-0000-0000-0000-000000000000","opCode": "MongoOpCode_OP_QUERY","errorCode": "0","duration": "0","requestCharge": "0.000000","databaseName": "admin","collectionName": "$cmd","retryCount": "0"}}
    ```

* **QueryRuntimeStatistics**: Wählen Sie diese Option aus, um den Abfragetext zu protokollieren, der ausgeführt wurde. 

    ```
    { "time": "2019-04-14T19:08:11.6353239Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "QueryRuntimeStatistics", "properties": {"activityId": "278b0661-7452-4df3-b992-8aa0864142cf","databasename": "Tasks","collectionname": "Items","partitionkeyrangeid": "0","querytext": "{"query":"SELECT *\nFROM c\nWHERE (c.p1__10 != true)","parameters":[]}"}}
    ```

* **PartitionKeyStatistics**: Wählen Sie diese Option aus, um die Statistiken der Partitionsschlüssel zu protokollieren. Dies wird aktuell mit der Speichergröße (KB) der Partitionsschlüssel dargestellt. Das Protokoll wird für die ersten drei Partitionsschlüssel ausgegeben, die den meisten Datenspeicher beanspruchen.

    ```
    { "time": "2019-10-11T02:33:24.2018744Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "PartitionKeyStatistics", "properties": {"subscriptionId": "<your_subscription_ID>","regionName": "West US 2","databaseName": "KustoQueryResults","collectionname": "CapacityMetrics","partitionkey": "["CapacityMetricsPartition.136"]","sizeKb": "2048270"}}
    ```

* **Metrikanforderungen:** Wählen Sie diese Option aus, um Metrikdaten von Azure Cosmos DB zu den Zielen in der Diagnoseeinstellung zu sammeln. Dies sind dieselben Daten, die automatisch in Azure-Metriken gesammelt werden. Sammeln Sie Metrikdaten mit Ressourcenprotokollen, um beide Arten von Daten gemeinsam zu analysieren und Metrikdaten außerhalb von Azure Monitor zu senden.

## <a name="analyzing-metric-data"></a>Analysieren von Metrikdaten
Azure Cosmos DB bietet ein benutzerdefiniertes Erlebnis für die Arbeit mit Metriken. Ausführliche Informationen zur Verwendung dieser Erfahrung und zur Analyse verschiedener Azure Cosmos DB-Szenarien finden Sie unter [Überwachen und Debuggen von Azure Cosmos DB-Metriken aus Azure Monitor](cosmos-db-azure-monitor-metrics.md).

Sie können Metriken für Azure Cosmos DB mit Metriken aus anderen Azure-Diensten mit dem Metrik-Explorer analysieren, indem Sie **Metriken** aus dem Menü **Azure Monitor** öffnen. Ausführliche Informationen zur Verwendung dieses Tools finden Sie unter [Erste Schritte mit dem Azure-Metrik-Explorer](../azure-monitor/platform/metrics-getting-started.md). Alle Metriken für Azure Cosmos DB befinden sich im Namespace **Cosmos DB-Standardmetriken**. Sie können die folgenden Dimensionen mit diesen Metriken verwenden, wenn Sie einen Filter zu einem Diagramm hinzufügen:

- CollectionName
- DatabaseName
- OperationType
- Region
- StatusCode


## <a name="analyzing-log-data"></a>Analysieren von Protokolldaten
Daten in Azure Monitor-Protokollen werden in Tabellen gespeichert, wobei jede Tabelle ihren eigenen Satz eindeutiger Eigenschaften hat. In Azure Cosmos DB werden Daten in den folgenden Tabellen gespeichert.

| Table | BESCHREIBUNG |
|:---|:---|
| AzureDiagnostics | Allgemeine Tabelle, die von mehreren Diensten verwendet wird, um Ressourcenprotokolle zu speichern. Ressourcenprotokolle von Azure Cosmos DB können mit `MICROSOFT.DOCUMENTDB` identifiziert werden.   |
| AzureActivity    | Allgemeine Tabelle, die alle Datensätze aus dem Aktivitätsprotokoll speichert. 


> [!IMPORTANT]
> Wenn Sie **Protokolle** im Menü von Azure Cosmos DB auswählen, wird Log Analytics geöffnet, wobei der Abfragebereich auf die aktuelle Azure Cosmos-Datenbank festgelegt ist. Dies bedeutet, dass Protokollabfragen nur Daten aus dieser Ressource umfassen. Wenn Sie eine Abfrage ausführen möchten, die Daten aus anderen Datenbanken oder Daten aus anderen Azure-Diensten enthält, wählen Sie im Menü **Azure Monitor** die Option **Protokolle** aus. Ausführliche Informationen finden Sie unter [Protokollabfragebereich und Zeitbereich in Azure Monitor Log Analytics](../azure-monitor/log-query/scope.md).

### <a name="azure-cosmos-db-log-analytics-queries-in-azure-monitor"></a>Azure Cosmos DB Log Analytics-Abfragen in Azure Monitor

Hier sind einige Abfragen, die Sie in die Suchleiste **Protokollsuche** eingeben können, um die Überwachung Ihrer Azure Cosmos-Container zu vereinfachen. Diese Abfragen arbeiten mit der [neuen Sprache](../log-analytics/log-analytics-log-search-upgrade.md).

Die folgenden Abfragen sind Abfragen, mit denen Sie Ihre Azure Cosmos-Datenbanken überwachen können:

* Das folgende Beispiel zeigt eine Abfrage für alle Diagnoseprotokolle von Azure Cosmos DB für einen angegebenen Zeitraum:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"

    ```

* So fragen Sie die letzten 10 protokollierten Ereignisse ab:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | limit 10
    ```

* So fragen Sie alle Vorgänge gruppiert nach Vorgangstyp ab:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by OperationName
    ```

* So fragen Sie alle Vorgänge gruppiert nach Ressource ab:

    ```Kusto
    AzureActivity 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by Resource

    ```

* So fragen Sie die gesamte Benutzeraktivität gruppiert nach Ressource ab:

    ```Kusto
    AzureActivity 
    | where Caller == "test@company.com" and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by Resource
    ```
* So rufen Sie alle Abfragen ab, die größer als 100 RUs sind und mit Daten aus **DataPlaneRequests** und **QueryRunTimeStatistics** verknüpft sind:

    ```Kusto
    AzureDiagnostics
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" and todouble(requestCharge_s) > 100.0
    | project activityId_g, requestCharge_s
    | join kind= inner (
           AzureDiagnostics
           | where ResourceProvider =="MICROSOFT.DOCUMENTDB" and Category == "QueryRuntimeStatistics"
           | project activityId_g, querytext_s
    ) on $left.activityId_g == $right.activityId_g
    | order by requestCharge_s desc
    | limit 100
    ```

* So fragen Sie ab, welche Vorgänge länger als 3 Millisekunden dauern:

    ```Kusto
    AzureDiagnostics 
    | where toint(duration_s) > 3 and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by clientIpAddress_s, TimeGenerated
    ```

* So fragen Sie ab, welcher Agent die Vorgänge ausführt:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by OperationName, userAgent_s
    ```

* So fragen Sie ab, wann die Vorgänge mit langer Ausführungszeit ausgeführt wurden:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | project TimeGenerated , duration_s 
    | summarize count() by bin(TimeGenerated, 5s)
    | render timechart
    ```
    
* So rufen Sie Partitionsschlüsselstatistiken zum Auswerten der Schiefe in den obersten drei Partitionen für das Datenbankkonto ab:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="PartitionKeyStatistics" 
    | project SubscriptionId, regionName_s, databaseName_s, collectionname_s, partitionkey_s, sizeKb_s, ResourceId 
    ```

## <a name="monitor-azure-cosmos-db-programmatically"></a>Programmgesteuertes Überwachen von Azure Cosmos DB
Die im Portal für Konten verfügbaren Metriken, z. B. für die Speichernutzung von Konten und die Gesamtzahl der Anforderungen, stehen über die SQL-APIs nicht zur Verfügung. Sie können jedoch mithilfe der SQL-APIs die Nutzungsdaten auf Sammlungsebene abrufen. Gehen Sie zum Abrufen von Daten auf Sammlungsebene wie folgt vor:

* Führen Sie zur Verwendung der REST-API [einen GET-Befehl für die Sammlung](https://msdn.microsoft.com/library/mt489073.aspx)aus. Die Kontingent- und Nutzungsinformationen für die Sammlung werden in den Headern „x-ms-resource-quota“ und „x-ms-resource-usage“ in der Antwort zurückgegeben.
* Verwenden Sie für das .NET SDK die Methode [DocumentClient.ReadDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx), die eine [ResourceResponse](https://msdn.microsoft.com/library/dn799209.aspx) mit mehreren Nutzungseigenschaften zurückgibt, z.B. **CollectionSizeUsage**, **DatabaseUsage**, **DocumentUsage** sowie weitere Eigenschaften.

Für den Zugriff auf weitere Metriken verwenden Sie das [Azure Monitor SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights). Verfügbare Metrikdefinitionen können folgendermaßen aufgerufen werden:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metricDefinitions?api-version=2015-04-08

Abfragen zum Abrufen einzelner Metriken verwenden das folgende Format:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metrics?api-version=2015-04-08&$filter=%28name.value%20eq%20%27Total%20Requests%27%29%20and%20timeGrain%20eq%20duration%27PT5M%27%20and%20startTime%20eq%202016-06-03T03%3A26%3A00.0000000Z%20and%20endTime%20eq%202016-06-10T03%3A26%3A00.0000000Z



## <a name="next-steps"></a>Nächste Schritte

- Eine Referenz zu den Protokollen und Metriken, die von Azure Cosmos DB erstellt werden, finden Sie unter [Überwachen von Azure Cosmos DB-Daten – Referenz](monitor-cosmos-db-reference.md).
- Ausführliche Informationen zur Überwachung von Azure-Ressourcen finden Sie unter [Überwachen von Azure-Ressourcen mit Azure Monitor](../azure-monitor/insights/monitor-azure-resource.md).
