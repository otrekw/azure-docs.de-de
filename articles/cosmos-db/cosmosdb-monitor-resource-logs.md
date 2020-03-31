---
title: Überwachen der Azure Cosmos DB-Daten mithilfe der Azure-Diagnoseeinstellungen
description: Erfahren Sie, wie Sie die Azure-Diagnoseeinstellungen verwenden, um die Leistung und Verfügbarkeit der in Azure Cosmos DB gespeicherten Daten zu überwachen.
author: SnehaGunda
services: cosmos-db
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/09/2019
ms.author: sngun
ms.openlocfilehash: 184fc65dae57292243be9abdca71a129512b3d0b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78252051"
---
# <a name="monitor-azure-cosmos-db-data-by-using-diagnostic-settings-in-azure"></a>Überwachen der Azure Cosmos DB-Daten mithilfe der Diagnoseeinstellungen in Azure

Diagnoseeinstellungen in Azure werden zum Sammeln von Ressourcenprotokollen verwendet. Azure-Ressourcenprotokolle werden von einer Ressource ausgegeben und stellen umfangreiche, in kurzen Abständen erfasste Betriebsdaten der Ressource bereit. Diese Protokolle werden pro Anforderung erfasst und werden auch als „Datenebenenprotokolle“ bezeichnet. Einige Beispiele für Datenebenenvorgänge sind „delete“, „insert“ und „readFeed“. Der Inhalt dieser Protokolle variiert je nach Ressourcentyp.

Plattformmetriken und das Aktivitätsprotokoll werden automatisch gesammelt, aber Sie müssen eine Diagnoseeinstellung erstellen, um Ressourcenprotokolle zu sammeln oder außerhalb von Azure Monitor weiterzuleiten. Sie können die Diagnoseeinstellungen für Azure Cosmos-Konten mithilfe der folgenden Schritte aktivieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Navigieren Sie zu Ihrem Azure Cosmos-Konto. Öffnen Sie den Bereich **Diagnoseeinstellungen**, und wählen Sie dann die Option **Diagnoseeinstellung hinzufügen** aus.

1. Füllen Sie im Bereich **Diagnoseeinstellungen** das Formular mit den folgenden Details aus: 

    * **Name**: Geben Sie einen Namen für die zu erstellenden Protokolle ein.

    * Sie können die Protokolle für Folgendes speichern: **In einem Speicherkonto archivieren**, **An einen Event Hub streamen** oder **An Log Analytics senden**.

1. Wenn Sie eine Diagnoseeinstellung erstellen, legen Sie fest, welche Kategorie von Protokollen gesammelt werden soll. Die Kategorien der von Azure Cosmos DB unterstützten Protokolle sind unten aufgelistet, zusammen mit den von ihnen gesammelten Beispielprotokollen:

 * **DataPlaneRequests**: Verwenden Sie diese Option, um Back-End-Anforderungen an alle APIs in Azure Cosmos DB zu protokollieren, worin SQL-, Graph-, MongoDB-, Cassandra- und Tabellen-API-Konten einbezogen sind. Wichtige zu beachtende Eigenschaften sind: `Requestcharge`, `statusCode`, `clientIPaddress` und `partitionID`.

    ```
    { "time": "2019-04-23T23:12:52.3814846Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "DataPlaneRequests", "operationName": "ReadFeed", "properties": {"activityId": "66a0c647-af38-4b8d-a92a-c48a805d6460","requestResourceType": "Database","requestResourceId": "","collectionRid": "","statusCode": "200","duration": "0","userAgent": "Microsoft.Azure.Documents.Common/2.2.0.0","clientIpAddress": "10.0.0.24","requestCharge": "1.000000","requestLength": "0","responseLength": "372","resourceTokenUserRid": "","region": "East US","partitionId": "062abe3e-de63-4aa5-b9de-4a77119c59f8","keyType": "PrimaryReadOnlyMasterKey","databaseName": "","collectionName": ""}}
    ```

* **MongoRequests**: Verwenden Sie diese Option, um vom Benutzer initiierte Anforderungen vom Front-End zum Erfüllen von Anforderungen an die API für MongoDB von Azure Cosmos DB zu protokollieren. Dieser Protokolltyp ist für andere API-Konten nicht verfügbar. MongoDB-Anforderungen werden in MongoRequests sowie DataPlaneRequests angezeigt. Wichtige zu beachtende Eigenschaften sind: `Requestcharge`, `opCode`.

    ```
    { "time": "2019-04-10T15:10:46.7820998Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "MongoRequests", "operationName": "ping", "properties": {"activityId": "823cae64-0000-0000-0000-000000000000","opCode": "MongoOpCode_OP_QUERY","errorCode": "0","duration": "0","requestCharge": "0.000000","databaseName": "admin","collectionName": "$cmd","retryCount": "0"}}
    ```

* **QueryRuntimeStatistics**: Wählen Sie diese Option aus, um den Abfragetext zu protokollieren, der ausgeführt wurde. Dieser Protokolltyp ist nur für SQL-API-Konten verfügbar.

    ```
    { "time": "2019-04-14T19:08:11.6353239Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "QueryRuntimeStatistics", "properties": {"activityId": "278b0661-7452-4df3-b992-8aa0864142cf","databasename": "Tasks","collectionname": "Items","partitionkeyrangeid": "0","querytext": "{"query":"SELECT *\nFROM c\nWHERE (c.p1__10 != true)","parameters":[]}"}}
    ```

* **PartitionKeyStatistics**: Wählen Sie diese Option aus, um die Statistiken der Partitionsschlüssel zu protokollieren. Dies wird aktuell mit der Speichergröße (KB) der Partitionsschlüssel dargestellt. Weitere Informationen finden Sie im Abschnitt [Problembehandlung mithilfe von Azure-Diagnoseabfragen](#diagnostic-queries) in diesem Artikel. Beispiel: Abfragen, die „PartitionKeyStatistics“ verwenden. Das Protokoll wird für die ersten drei Partitionsschlüssel ausgegeben, die den meisten Datenspeicher beanspruchen. Dieses Protokoll enthält Daten wie die Abonnement-ID, den Namen der Region, den Namen der Datenbank, den Namen der Sammlung, den Partitionsschlüssel und die Speichergröße in KB.

    ```
    { "time": "2019-10-11T02:33:24.2018744Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "PartitionKeyStatistics", "properties": {"subscriptionId": "<your_subscription_ID>","regionName": "West US 2","databaseName": "KustoQueryResults","collectionname": "CapacityMetrics","partitionkey": "["CapacityMetricsPartition.136"]","sizeKb": "2048270"}}
    ```

* **PartitionKeyRUConsumption**: Dieses Protokoll meldet den aggregierten Pro-Sekunde-RU/s-Verbrauch von Partitionsschlüsseln. Derzeit meldet Azure Cosmos DB Partitionsschlüssel nur für SQL-API-Konten und für Punktlese-/Punktschreibvorgänge sowie für Vorgänge für gespeicherte Prozeduren. Andere APIs und Vorgangstypen werden nicht unterstützt. Bei anderen APIs ist die Partitionsschlüsselspalte in der Diagnoseprotokolltabelle leer. Dieses Protokoll enthält Daten wie die Abonnement-ID, den Namen der Region, den Namen der Datenbank, den Namen der Sammlung, den Partitionsschlüssel, den Vorgangstyp und die Anforderungskosten. Weitere Informationen finden Sie im Abschnitt [Problembehandlung mithilfe von Azure-Diagnoseabfragen](#diagnostic-queries) in diesem Artikel. Beispiel: Abfragen, die „PartitionKeyRUConsumption“ verwenden. 

* **ControlPlaneRequests**: Dieses Protokoll enthält Details zu Vorgängen auf der Steuerungsebene, z. B. das Erstellen eines Kontos, das Hinzufügen oder Entfernen einer Region, das Aktualisieren der Einstellungen für die Kontoreplikation usw. Dieser Protokolltyp ist für alle API-Typen verfügbar, zu denen die SQL- (Core), MongoDB-, Gremlin-, Cassandra- und Tabellen-API gehören.

* **Anforderungen**: Wählen Sie diese Option aus, um Metrikdaten von Azure Cosmos DB zu den Zielen in der Diagnoseeinstellung zu sammeln. Dies sind dieselben Daten, die automatisch in Azure-Metriken gesammelt werden. Sammeln Sie Metrikdaten mit Ressourcenprotokollen, um beide Arten von Daten gemeinsam zu analysieren und Metrikdaten außerhalb von Azure Monitor zu senden.

Ausführliche Informationen zum Erstellen einer Diagnoseeinstellung über das Azure-Portal, die Befehlszeilenschnittstelle oder PowerShell finden Sie unter [Erstellen einer Diagnoseeinstellung zum Sammeln von Plattformprotokollen und Metriken in Azure](../azure-monitor/platform/diagnostic-settings.md).


## <a name="troubleshoot-issues-with-diagnostics-queries"></a><a id="diagnostic-queries"></a> Problembehandlung mit Diagnoseabfragen

1. Wie erhalte ich die Anforderungskosten für ressourcenintensive Abfragen?

   ```Kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" and todouble(requestCharge_s) > 10.0
   | project activityId_g, requestCharge_s
   | join kind= inner (
   AzureDiagnostics
   | where ResourceProvider =="MICROSOFT.DOCUMENTDB" and Category == "QueryRuntimeStatistics"
   | project activityId_g, querytext_s
   ) on $left.activityId_g == $right.activityId_g
   | order by requestCharge_s desc
   | limit 100
   ```

1. Wie finde ich heraus, welche Vorgänge die meisten RU/s in Anspruch nehmen?

    ```Kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
   | where TimeGenerated >= ago(2h) 
   | summarize max(responseLength_s), max(requestLength_s), max(requestCharge_s), count = count() by OperationName, requestResourceType_s, userAgent_s, collectionRid_s, bin(TimeGenerated, 1h)
   ```
1. Wie erhalte ich die Verteilung für verschiedene Vorgänge?

   ```Kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
   | where TimeGenerated >= ago(2h) 
   | summarize count = count()  by OperationName, requestResourceType_s, bin(TimeGenerated, 1h) 
   ```

1. Was ist der maximale Durchsatz, den eine Partition verbraucht?

   ```Kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
   | where TimeGenerated >= ago(2h) 
   | summarize max(requestCharge_s) by bin(TimeGenerated, 1h), partitionId_g
   ```

1. Wie erhalte ich die Informationen über den RU/s-Verbrauch pro Sekunde der Partitionsschlüssel?

   ```Kusto
   AzureDiagnostics 
   | where ResourceProvider == "MICROSOFT.DOCUMENTDB" and Category == "PartitionKeyRUConsumption" 
   | summarize total = sum(todouble(requestCharge_s)) by databaseName_s, collectionName_s, partitionKey_s, TimeGenerated 
   | order by TimeGenerated asc 
   ```

1. Abrufen der Anforderungskosten für einen bestimmten Partitionsschlüssel

   ```Kusto
   AzureDiagnostics 
   | where ResourceProvider == "MICROSOFT.DOCUMENTDB" and Category == "PartitionKeyRUConsumption" 
   | where parse_json(partitionKey_s)[0] == "2" 
   ```

1. Wie erhalte ich die wichtigsten Partitionsschlüssel mit den meisten verbrauchten RU/s in einem bestimmten Zeitraum? 

   ```Kusto
   AzureDiagnostics 
   | where ResourceProvider == "MICROSOFT.DOCUMENTDB" and Category == "PartitionKeyRUConsumption" 
   | where TimeGenerated >= datetime("11/26/2019, 11:20:00.000 PM") and TimeGenerated <= datetime("11/26/2019, 11:30:00.000 PM") 
   | summarize total = sum(todouble(requestCharge_s)) by databaseName_s, collectionName_s, partitionKey_s 
   | order by total desc
    ```

1. Wie erhalte ich die Protokolle für die Partitionsschlüssel, deren Speichergröße größer als 8 GB ist?

   ```Kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="PartitionKeyStatistics"
   | where todouble(sizeKb_d) > 800000
   ```

1. Wie erhalten ich die Partitionsschlüsselstatistiken zum Auswerten der Schiefe in den obersten drei Partitionen für das Datenbankkonto?

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="PartitionKeyStatistics" 
    | project SubscriptionId, regionName_s, databaseName_s, collectionname_s, partitionkey_s, sizeKb_s, ResourceId 
    ```

## <a name="next-steps"></a>Nächste Schritte

* [Azure Monitor für Azure Cosmos DB](../azure-monitor/insights/cosmosdb-insights-overview.md?toc=/azure/cosmos-db/toc.json)
* [Überwachen und Debuggen mit Metriken in Azure Cosmos DB](use-metrics.md)
