---
title: Überwachen von Azure Storage | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Leistung und Verfügbarkeit von Azure Storage überwachen können.
author: normesta
services: storage
ms.service: storage
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: normesta
ms.reviewer: fryu
ms.custom: monitoring
ms.openlocfilehash: 3ede22b5af942c3f0c0cd88d86b56a625c7656c0
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84267612"
---
# <a name="monitor-azure-storage"></a>Überwachen von Azure Storage

Wenn Sie über unternehmenskritische Anwendungen und Geschäftsprozesse verfügen, die auf Azure-Ressourcen basieren, sollten Sie diese Ressourcen auf Verfügbarkeit, Leistung und Betrieb überwachen. In diesem Artikel wird das Überwachen von Daten beschrieben, die von Azure Storage generiert wurden. Außerdem wird erläutert, wie Sie die Funktionen von Azure Monitor verwenden können, um Warnungen für diese Daten zu analysieren.

> [!NOTE]
> Azure Storage-Protokolle in Azure Monitor befinden sich in der öffentlichen Vorschauphase und stehen in allen Regionen für die öffentliche Cloud für Vorschautests zur Verfügung. Informationen zum Registrieren für die Vorschauversion finden Sie auf [dieser Seite](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u). Diese Vorschauversion ermöglicht Protokolle für Blobs (einschließlich Azure Data Lake Storage Gen2), Dateien, Warteschlangen, Tabellen, Universell V1-Premium-Speicherkonten und Universell V2-Speicherkonten. Klassische Speicherkonten werden nicht unterstützt.

## <a name="monitor-overview"></a>Azure Monitor: Übersicht

Im Azure-Portal enthält die Seite **Übersicht** für jede Speicherressource eine kurze Übersicht über die Ressourcennutzung beispielsweise Anforderungen und stündliche Abrechnung. Dies sind nützliche Informationen, die aber nur einen kleinen Teil der verfügbaren Überwachungsdaten ausmachen. Einige dieser Daten werden automatisch erfasst und sind für die Analyse verfügbar, sobald Sie die Speicherressource erstellen. Mit einigen Konfigurationsschritten können Sie zusätzliche Typen von Datensammlungen aktivieren.

## <a name="what-is-azure-monitor"></a>Was ist Azure Monitor?
Azure Storage protokolliert Überwachungsdaten mit [Azure Monitor](../../azure-monitor/overview.md), das ein Azure-Dienst zur vollständigen Stapelüberwachung ist. Azure Monitor bietet einen vollständigen Satz von Funktionen zum Überwachen Ihrer Azure-Ressourcen, von Ressourcen in anderen Clouds sowie von lokalen Ressourcen. 

Weitere Informationen zu Azure Monitor finden Sie unter [Überwachen von Azure-Ressourcen mit Azure Monitor](../../azure-monitor/insights/monitor-azure-resource.md). In diesem Artikel werden die folgenden Themen beschrieben:

- Was ist Azure Monitor?
- Kosten für die Überwachung
- In Azure gesammelte Überwachungsdaten
- Konfigurieren der Datensammlung
- Standardtools in Azure zum Analysieren von Überwachungsdaten sowie zum Generieren von Warnungen

Die folgenden Abschnitte basieren auf diesem Artikel. Darin werden die spezifischen von Azure Storage erfassten Daten beschrieben. In Beispielen wird gezeigt, wie Sie die Datensammlung konfigurieren und diese Daten mit Azure-Tools analysieren.

## <a name="monitor-data-from-azure-storage"></a>Überwachen von Daten aus Azure Storage

Azure Storage erfasst dieselben Arten von Überwachungsdaten wie andere Azure-Ressourcen, die unter [Überwachungsdaten von Azure-Ressourcen](../../azure-monitor/insights/monitor-azure-resource.md#monitoring-data) beschrieben sind. Weitere Informationen zu den Protokollen und Metriken, die von Azure Storage erstellt werden, finden Sie unter [Referenz zu Azure Storage-Überwachungsdaten](monitor-storage-reference.md).

Die Metriken und Protokolle in Azure Monitor unterstützen nur Azure Resource Manager-Speicherkonten. Klassische Speicherkonten werden von Azure Monitor nicht unterstützt. Wenn Sie Metriken oder Protokolle für ein klassisches Speicherkonto verwenden möchten, müssen Sie das Konto zu einem Azure Resource Manager-Speicherkonto migrieren. Weitere Informationen finden Sie unter [Migrieren zu Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview).

Wenn Sie möchten, können Sie klassische Metriken und Protokolle auch weiterhin verwenden. Klassische Metriken und Protokolle sind parallel zu den Metriken und Protokollen in Azure Monitor verfügbar. Die Unterstützung bleibt bestehen, bis Azure Storage den Dienst für Legacymetriken und -protokolle einstellt.

### <a name="logs-in-azure-monitor-preview"></a>Protokolle in Azure Monitor (Vorschau)

Protokolleinträge werden nur erstellt, wenn Anforderungen für den Dienstendpunkt gestellt wurden. Wenn beispielsweise ein Speicherkonto Aktivität im Blob-Endpunkt, jedoch nicht im Tabellen- oder Warteschlangenendpunkt aufweist, werden nur Protokolle für den Blobdienst erstellt. Azure Storage-Protokolle enthalten ausführliche Informationen über erfolgreiche und fehlgeschlagene Anforderungen an einen Speicherdienst. Anhand dieser Informationen können einzelne Anforderungen überwacht und Probleme mit einem Speicherdienst untersucht werden. Anforderungen werden auf Grundlage der besten Leistung protokolliert.

#### <a name="log-authenticated-requests"></a>Protokollieren von authentifizierten Anforderungen

 Die folgenden Typen authentifizierter Anforderungen werden protokolliert:

- Erfolgreiche Anforderungen
- Fehlerhafte Anforderungen, einschließlich Timeout-, Drosselungs-, Netzwerk- und Autorisierungsfehler sowie anderer Fehler
- Anforderungen, die eine SAS (Shared Access Signature) oder OAuth verwenden, einschließlich fehlerhafter und erfolgreicher Anforderungen
- Anforderungen an Analysedaten (klassische Protokolldaten im Container **$logs** und klassische Metrikdaten in den **$metric**-Tabellen)

Anforderungen, die durch den Speicherdienst selbst erfolgen, wie z. B. Protokollerstellungs- oder -löschvorgänge, werden nicht protokolliert. Eine vollständige Liste der protokollierten Daten finden Sie unter [Protokollierte Storage Analytics-Vorgänge und -Statusmeldungen](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) und [Storage Analytics-Protokollformat](monitor-storage-reference.md).

#### <a name="log-anonymous-requests"></a>Protokollieren anonymer Anforderungen

 Die folgenden Typen anonymer Anforderungen werden protokolliert:

- Erfolgreiche Anforderungen
- Serverfehler
- Timeoutfehler für Client und Server
- Mit Fehlercode 304 (nicht geändert) fehlgeschlagene GET-Anforderungen

Alle anderen fehlgeschlagenen, anonymen Anforderungen werden nicht protokolliert. Eine vollständige Liste der protokollierten Daten finden Sie unter [Protokollierte Storage Analytics-Vorgänge und -Statusmeldungen](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) und [Storage Analytics-Protokollformat](monitor-storage-reference.md).

## <a name="configuration"></a>Konfiguration

Plattformmetriken und das Aktivitätsprotokoll werden automatisch gesammelt, aber Sie müssen eine Diagnoseeinstellung erstellen, um Ressourcenprotokolle zu sammeln oder außerhalb von Azure Monitor weiterzuleiten. Informationen zum Erstellungsprozess für eine Diagnoseeinstellung über das Azure-Portal, Azure CLI oder PowerShell finden Sie unter [Erstellen einer Diagnoseeinstellung zum Sammeln von Plattformprotokollen und Metriken in Azure](../../azure-monitor/platform/diagnostic-settings.md).

Beim Erstellen einer Diagnoseeinstellung wählen 0Sie den Speichertyp aus, für den Sie Protokolle aktivieren möchten, z. B. Blob, Warteschlange, Tabelle oder Datei. Wenn Sie die Diagnoseeinstellung im Azure-Portal erstellen, können Sie die Ressource aus einer Liste auswählen. Bei Verwendung von PowerShell oder der Azure-Befehlszeilenschnittstelle (Command Line Interface, CLI) müssen Sie die Ressourcen-ID des Speichertyps verwenden. Sie finden die Ressourcen-ID im Azure-Portal, indem Sie die Seite **Eigenschaften** Ihres Speicherkontos öffnen.

Außerdem müssen Sie angeben, für welche Kategorien von Vorgängen Protokolle erfasst werden sollen. Die für Azure Storage vorhandenen Kategorien sind in dieser Tabelle aufgeführt.

| Category | BESCHREIBUNG |
|:---|:---|
| StorageRead | Lesevorgänge für Blobs. |
| StorageWrite | Schreibvorgänge für Blobs. |
| StorageDelete | Löschvorgänge für Blobs. |

## <a name="analyze-metric-data"></a>Analysieren von Metrikdaten

Mit dem Metrik-Explorer können Sie Metriken für Azure Storage mit Metriken aus anderen Azure-Diensten analysieren. Öffnen Sie den Metrik-Explorer, indem Sie im Menü **Azure Monitor** die Option **Metriken** auswählen. Ausführliche Informationen zur Verwendung dieses Tools finden Sie unter [Erste Schritte mit dem Azure-Metrik-Explorer](../../azure-monitor/platform/metrics-getting-started.md). 

Dieses Beispiel zeigt, wie Sie **Transaktionen** auf der Kontoebene anzeigen.

![Screenshot für den Zugriff auf Metriken über das Azure-Portal](./media/monitor-storage/access-metrics-portal.png)

Bei Metriken mit Dimensionsunterstützung können Sie die Metrik nach dem gewünschten Dimensionswert filtern. Dieses Beispiel zeigt, wie Sie **Transaktionen** auf der Kontoebene für einen bestimmten Vorgang anzeigen, indem Sie Werte für die Dimension **API-Name** auswählen.

![Screenshot für den Zugriff auf Metriken mit Dimension über das Azure-Portal](./media/monitor-storage/access-metrics-portal-with-dimension.png)

Eine vollständige Liste der von Azure Storage unterstützten Dimensionen finden Sie unter [Metrikdimensionen](monitor-storage-reference.md#metrics-dimensions).

Sämtliche Metriken für Azure Storage befinden sich in diesen Namespaces:

- Microsoft.Storage/storageAccounts
- Microsoft.Storage/storageAccounts/blobServices
- Microsoft.Storage/storageAccounts/fileServices
- Microsoft.Storage/storageAccounts/queueServices
- Microsoft.Storage/storageAccounts/tableServices

Eine Liste aller von Azure Monitor unterstützten Metriken (einschließlich Azure Storage) finden Sie unter [Unterstützte Metriken von Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported).


### <a name="access-metrics"></a>Zugreifen auf Metriken

> [!TIP]
> Wählen Sie zum Anzeigen von Azure CLI- oder .NET-Beispielen die hier aufgeführten entsprechenden Registerkarten aus.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

#### <a name="list-the-metric-definition"></a>Auflisten der Metrikdefinition

Sie können die Metrikdefinition Ihres Speicherkontos oder des einzelnen Speicherdiensts (z. B. des Blob-, Datei-, Tabellen- oder Warteschlangendiensts) auflisten. Verwenden Sie das Cmdlet [Get-AzMetricDefinition](https://docs.microsoft.com/powershell/module/az.monitor/get-azmetricdefinition?view=azps-3.3.0).

Ersetzen Sie in diesem Beispiel den Platzhalter `<resource-ID>` durch die Ressourcen-ID des gesamten Speicherkontos oder die Ressourcen-ID eines einzelnen Speicherdiensts, beispielsweise des Blob-, Datei-, Tabellen- oder Warteschlangendiensts. Sie finden diese Ressourcen-IDs im Azure-Portal auf der jeweiligen Seite **Eigenschaften** Ihres Speicherkontos.

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetricDefinition -ResourceId $resourceId
```

#### <a name="read-metric-values"></a>Lesen von Metrikwerten

Sie können die Metrikwerte auf der Kontoebene Ihres Speicherkontos oder des einzelnen Speicherdiensts (z. B. des Blob-, Datei-, Tabellen- oder Warteschlangendiensts) lesen. Verwenden Sie das Cmdlet [Get-AzMetric](https://docs.microsoft.com/powershell/module/Az.Monitor/Get-AzMetric?view=azps-3.3.0).

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetric -ResourceId $resourceId -MetricNames "UsedCapacity" -TimeGrain 01:00:00
```

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

#### <a name="list-the-account-level-metric-definition"></a>Auflisten der Metrikdefinition auf Kontoebene

Sie können die Metrikdefinition Ihres Speicherkontos oder des einzelnen Speicherdiensts (z. B. des Blob-, Datei-, Tabellen- oder Warteschlangendiensts) auflisten. Verwenden Sie den Befehl [az monitor metrics list-definitions](https://docs.microsoft.com/cli/azure/monitor/metrics?view=azure-cli-latest#az-monitor-metrics-list-definitions).
 
Ersetzen Sie in diesem Beispiel den Platzhalter `<resource-ID>` durch die Ressourcen-ID des gesamten Speicherkontos oder die Ressourcen-ID eines einzelnen Speicherdiensts, beispielsweise des Blob-, Datei-, Tabellen- oder Warteschlangendiensts. Sie finden diese Ressourcen-IDs im Azure-Portal auf der jeweiligen Seite **Eigenschaften** Ihres Speicherkontos.

```azurecli-interactive
   az monitor metrics list-definitions --resource <resource-ID>
```

#### <a name="read-account-level-metric-values"></a>Lesen der Metrikwerte auf der Kontoebene

Sie können die Metrikwerte Ihres Speicherkontos oder des einzelnen Speicherdiensts (z. B. des Blob-, Datei-, Tabellen- oder Warteschlangendiensts) lesen. Verwenden Sie den Befehl [az monitor metrics list](https://docs.microsoft.com/cli/azure/monitor/metrics?view=azure-cli-latest#az-monitor-metrics-list).

```azurecli-interactive
   az monitor metrics list --resource <resource-ID> --metric "UsedCapacity" --interval PT1H
```

### <a name="net"></a>[.NET](#tab/dotnet)

Azure Monitor bietet das [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/) zum Lesen von Metrikdefinition und -werten. Die [Beispielcode](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/) zeigt, wie das SDK mit unterschiedlichen Parametern verwendet wird. Sie benötigen `0.18.0-preview` oder eine höhere Version für Speichermetriken.
 
Ersetzen Sie in diesen Beispielen den Platzhalter `<resource-ID>` durch die Ressourcen-ID des gesamten Speicherkontos oder die Ressourcen-ID eines einzelnen Speicherdiensts, beispielsweise des Blob-, Datei-, Tabellen- oder Warteschlangendiensts. Sie finden diese Ressourcen-IDs im Azure-Portal auf der jeweiligen Seite **Eigenschaften** Ihres Speicherkontos.

Ersetzen Sie die Variable `<subscription-ID>` durch die ID Ihres Abonnements. Eine Anleitung zum Abrufen von Werten für `<tenant-ID>`, `<application-ID>`, und `<AccessKey>` finden Sie unter [Erstellen einer Azure AD-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff über das Portal](https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/). 

#### <a name="list-the-account-level-metric-definition"></a>Auflisten der Metrikdefinition auf Kontoebene

Das folgende Beispiel zeigt, wie Sie die Metrikdefinition auf der Kontoebene auflisten:

```csharp
    public static async Task ListStorageMetricDefinition()
    {
        var resourceId = "<resource-ID>";
        var subscriptionId = "<subscription-ID>";
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";


        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;
        IEnumerable<MetricDefinition> metricDefinitions = await readOnlyClient.MetricDefinitions.ListAsync(resourceUri: resourceId, cancellationToken: new CancellationToken());

        foreach (var metricDefinition in metricDefinitions)
        {
            // Enumrate metric definition:
            //    Id
            //    ResourceId
            //    Name
            //    Unit
            //    MetricAvailabilities
            //    PrimaryAggregationType
            //    Dimensions
            //    IsDimensionRequired
        }
    }

```

#### <a name="read-account-level-metric-values"></a>Lesen der Metrikwerte auf der Kontoebene

Das folgende Beispiel zeigt, wie Sie `UsedCapacity`-Daten auf der Kontoebene lesen:

```csharp
    public static async Task ReadStorageMetricValue()
    {
        var resourceId = "<resource-ID>";
        var subscriptionId = "<subscription-ID>";
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";

        MonitorClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;

        Response = await readOnlyClient.Metrics.ListAsync(
            resourceUri: resourceId,
            timespan: timeSpan,
            interval: System.TimeSpan.FromHours(1),
            metricnames: "UsedCapacity",

            aggregation: "Average",
            resultType: ResultType.Data,
            cancellationToken: CancellationToken.None);

        foreach (var metric in Response.Value)
        {
            // Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

#### <a name="read-multidimensional-metric-values"></a>Lesen von Werten mehrdimensionaler Metriken

Für mehrdimensionale Metriken müssen Sie Metadatenfilter definieren, wenn Metrikdaten für bestimmte Dimensionswerte gelesen werden sollen.

Im folgenden Beispiel wird gezeigt, wie Metrikdaten bei Metriken mit Unterstützung für mehrere Dimensionen gelesen werden:

```csharp
    public static async Task ReadStorageMetricValueTest()
    {
        // Resource ID for blob storage
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/blobServices/default";
        var subscriptionId = "<subscription-ID}";
        // How to identify Tenant ID, Application ID and Access Key: https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";

        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;
        // It's applicable to define meta data filter when a metric support dimension
        // More conditions can be added with the 'or' and 'and' operators, example: BlobType eq 'BlockBlob' or BlobType eq 'PageBlob'
        ODataQuery<MetadataValue> odataFilterMetrics = new ODataQuery<MetadataValue>(
            string.Format("BlobType eq '{0}'", "BlockBlob"));

        Response = readOnlyClient.Metrics.List(
                        resourceUri: resourceId,
                        timespan: timeSpan,
                        interval: System.TimeSpan.FromHours(1),
                        metricnames: "BlobCapacity",
                        odataQuery: odataFilterMetrics,
                        aggregation: "Average",
                        resultType: ResultType.Data);

        foreach (var metric in Response.Value)
        {
            //Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

---

## <a name="analyze-log-data"></a>Analysieren von Protokolldaten

Sie können auf Ressourcenprotokolle entweder als Blob in einem Speicherkonto, als Ereignisdaten oder über Log Analytics-Abfragen zugreifen.

Eine ausführliche Referenz zu den in diesen Protokollen angezeigten Feldern finden Sie unter [Referenz zu Azure Storage-Überwachungsdaten](monitor-storage-reference.md).

> [!NOTE]
> Azure Storage-Protokolle in Azure Monitor befinden sich in der öffentlichen Vorschauphase und stehen in allen Regionen für die öffentliche Cloud für Vorschautests zur Verfügung. Informationen zum Registrieren für die Vorschauversion finden Sie auf [dieser Seite](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u). Diese Vorschauversion ermöglicht Protokolle für Blobs (einschließlich Azure Data Lake Storage Gen2), Dateien, Warteschlangen, Tabellen, Universell V1-Premium-Speicherkonten und Universell V2-Speicherkonten. Klassische Speicherkonten werden nicht unterstützt.

### <a name="access-logs-in-a-storage-account"></a>Zugreifen auf Protokolle in einem Speicherkonto

Protokolle werden als Blobs angezeigt, die im Zielspeicherkonto in einem Container gespeichert sind. Die erfassten Daten werden in einem einzelnen Blob als durch Zeilen getrennte JSON-Nutzlast gespeichert. Für den Namen des Blobs wird diese Benennungskonvention verwendet:

`https://<destination-storage-account>.blob.core.windows.net/insights-logs-<storage-operation>/resourceId=/subscriptions/<subscription-ID>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<source-storage-account>/blobServices/default/y=<year>/m=<month>/d=<day>/h=<hour>/m=<minute>/PT1H.json`

Hier sehen Sie ein Beispiel:

`https://mylogstorageaccount.blob.core.windows.net/insights-logs-storagewrite/resourceId=/subscriptions/`<br>`208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/blobServices/default/y=2019/m=07/d=30/h=23/m=12/PT1H.json`

### <a name="access-logs-in-an-event-hub"></a>Zugreifen auf Protokolle in einem Event Hub

An einen Event Hub gesendete Protokolle werden nicht als Datei gespeichert. Sie können jedoch überprüfen, ob der Event Hub die Protokollinformationen empfangen hat. Wechseln Sie im Azure-Portal zu Ihrem Event Hub, und vergewissern Sie sich, dass die Anzahl **eingehender Nachrichten** größer als Null ist. 

![Überwachungsprotokolle](media/monitor-storage/event-hub-log.png)

Mithilfe von SIEM- (Security Information & Event Management) und Überwachungstools können Sie auf die an Ihren Event Hub gesendeten Protokolldaten zugreifen und diese lesen. Unter [Wie kann ich die an meinen Event Hub gesendeten Überwachungsdaten nutzen?](https://docs.microsoft.com/azure/azure-monitor/platform/stream-monitoring-data-event-hubs#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub) finden Sie weitere Informationen dazu.

### <a name="access-logs-in-a-log-analytics-workspace"></a>Zugreifen auf Protokolle im Log Analytics-Arbeitsbereich

Mithilfe von Azure Monitor-Protokollabfragen können Sie auf Protokolle zugreifen, die an einen Log Analytics-Arbeitsbereich gesendet wurden.

Weitere Informationen finden Sie unter [Erste Schritte mit Log Analytics in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal).

Die Daten werden in diesen Tabellen gespeichert.

| Tabelle | BESCHREIBUNG |
|:---|:---|
|StorageBlobLogs | Protokolle, die Aktivitäten im Blobspeicher beschreiben. |
|StorageFileLogs | Protokolle, die Aktivitäten in Dateifreigaben beschreiben. |
|StorageQueueLogs | Protokolle, die Aktivitäten in Warteschlangen beschreiben.|
|StorageTableLogs| Protokolle, die Aktivitäten in Tabellen beschreiben.|

### <a name="azure-storage-log-analytics-queries-in-azure-monitor"></a>Azure Storage Log Analytics-Abfragen in Azure Monitor

Im Folgenden finden Sie einige Abfragen, die Sie in die **Protokollsuch**leiste eingeben können, um die Überwachung Ihrer Azure Storage-Konten zu vereinfachen. Diese Abfragen arbeiten mit der [neuen Sprache](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

Verwenden Sie diese Abfragen, um Ihre Azure Storage-Konten zu überwachen:

* Abfrage zum Auflisten der 10 häufigsten Fehler in den letzten 3 Tagen.

    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d) and StatusText !contains "Success"
    | summarize count() by StatusText
    | top 10 by count_ desc
    ```
* Abfrage zum Auflisten der 10 häufigsten Vorgänge, die in den letzten 3 Tagen die meisten Fehler verursacht haben.

    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d) and StatusText !contains "Success"
    | summarize count() by OperationName
    | top 10 by count_ desc
    ```
* Abfrage zum Auflisten der 10 häufigsten Vorgänge mit der längsten End-to-End-Latenz in den letzten 3 Tagen.

    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d)
    | top 10 by DurationMs desc
    | project TimeGenerated, OperationName, DurationMs, ServerLatencyMs, ClientLatencyMs = DurationMs - ServerLatencyMs
    ```
* Abfrage zum Auflisten aller Vorgänge, die in den letzten 3 Tagen serverseitige Drosselungsfehler verursacht haben.

    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d) and StatusText contains "ServerBusy"
    | project TimeGenerated, OperationName, StatusCode, StatusText
    ```
* Abfrage zum Auflisten aller Anforderungen mit anonymem Zugriff in den letzten 3 Tagen.

    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d) and AuthenticationType == "Anonymous"
    | project TimeGenerated, OperationName, AuthenticationType, Uri
    ```
* Abfrage zum Erstellen eines Kreisdiagramms der in den letzten 3 Tagen verwendeten Vorgänge.
    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d)
    | summarize count() by OperationName
    | sort by count_ desc 
    | render piechart
    ```
## <a name="faq"></a>Häufig gestellte Fragen

**Unterstützt Azure Storage Metriken für verwaltete oder nicht verwaltete Datenträger?**

Nein. Die Metriken für Datenträger werden von Azure Compute unterstützt. Weitere Informationen finden Sie unter [Metriken pro Datenträger für verwaltete und nicht verwaltete Datenträger](https://azure.microsoft.com/blog/per-disk-metrics-managed-disks/).

## <a name="next-steps"></a>Nächste Schritte

- Eine Referenz zu den von Azure Storage erstellten Protokollen und Metriken finden Sie unter [Referenz zu Azure Storage-Überwachungsdaten](monitor-storage-reference.md).
- Ausführliche Informationen zur Überwachung von Azure-Ressourcen finden Sie unter [Überwachen von Azure-Ressourcen mit Azure Monitor](../../azure-monitor/insights/monitor-azure-resource.md).
- Weitere Informationen zur Migration von Metriken finden Sie unter [Migration von Azure Storage-Metriken](./storage-metrics-migration.md).
