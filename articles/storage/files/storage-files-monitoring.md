---
title: Überwachen von Azure Files | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Leistung und Verfügbarkeit von Azure Files überwachen können. Überwachen Sie Azure Files-Daten, erfahren Sie mehr über die Konfiguration, und analysieren Sie Metrik- und Protokolldaten.
author: normesta
services: storage
ms.service: storage
ms.subservice: files
ms.topic: conceptual
ms.date: 10/02/2020
ms.author: normesta
ms.reviewer: fryu
ms.custom: monitoring, devx-track-csharp
ms.openlocfilehash: 4b2f819edd875130c57d487536691b4588dcc71f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91772667"
---
# <a name="monitoring-azure-files"></a>Überwachen von Azure Files

Wenn Sie über unternehmenskritische Anwendungen und Geschäftsprozesse verfügen, die auf Azure-Ressourcen basieren, sollten Sie diese Ressourcen auf Verfügbarkeit, Leistung und Betrieb überwachen. In diesem Artikel wird das Überwachen von Daten beschrieben, die von Azure Files generiert wurden. Außerdem wird erläutert, wie Sie die Funktionen von Azure Monitor verwenden können, um Warnungen für diese Daten zu analysieren.

> [!NOTE]
> Azure Storage-Protokolle in Azure Monitor befinden sich in der öffentlichen Vorschauphase und stehen in allen Regionen für die öffentliche Cloud für Vorschautests zur Verfügung. Informationen zum Registrieren für die Vorschauversion finden Sie auf [dieser Seite](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u). Diese Vorschau unterstützt Protokolle für Blobdateien (einschließlich Azure Data Lake Storage Gen2), Dateien, Warteschlangen und Tabellen. Dieses Feature ist für alle Speicherkonten verfügbar, die mit dem Azure Resource Manager-Bereitstellungsmodell erstellt wurden. Weitere Informationen finden Sie in der [Speicherkontoübersicht](../common/storage-account-overview.md).

## <a name="monitor-overview"></a>Azure Monitor: Übersicht

Im Azure-Portal enthält die Seite **Übersicht** für jede Azure Files-Ressource eine kurze Übersicht über die Ressourcennutzung, beispielsweise Anforderungen und stündliche Abrechnung. Dies sind nützliche Informationen, die aber nur einen kleinen Teil der verfügbaren Überwachungsdaten ausmachen. Einige dieser Daten werden automatisch erfasst und sind für die Analyse verfügbar, sobald Sie die Ressource erstellen. Mit einigen Konfigurationsschritten können Sie zusätzliche Typen von Datensammlungen aktivieren.

## <a name="what-is-azure-monitor"></a>Was ist Azure Monitor?
Azure Files protokolliert Überwachungsdaten mit [Azure Monitor](../../azure-monitor/overview.md), einem Azure-Dienst zur vollständigen Stapelüberwachung. Azure Monitor bietet einen vollständigen Satz von Funktionen zum Überwachen Ihrer Azure-Ressourcen, von Ressourcen in anderen Clouds sowie von lokalen Ressourcen. 

Beginnen Sie mit dem Artikel [Überwachen von Azure-Ressourcen mit Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/monitor-azure-resource), in dem die folgenden Punkte beschrieben werden:

- Was ist Azure Monitor?
- Kosten für die Überwachung
- In Azure gesammelte Überwachungsdaten
- Konfigurieren der Datensammlung
- Standardtools in Azure zum Analysieren von Überwachungsdaten sowie zum Generieren von Warnungen

Die folgenden Abschnitte basieren auf diesem Artikel. Darin werden die spezifischen, von Azure Files erfassten Daten beschrieben. In Beispielen wird gezeigt, wie Sie die Datensammlung konfigurieren und diese Daten mit Azure-Tools analysieren.

## <a name="monitoring-data"></a>Überwachungsdaten

Azure Files erfasst dieselben Arten von Überwachungsdaten wie andere Azure-Ressourcen, die unter [Überwachungsdaten von Azure-Ressourcen](../../azure-monitor/insights/monitor-azure-resource.md#monitoring-data) beschrieben sind. 

Ausführliche Informationen zu den Metriken und Protokollen, die von Azure Files erstellt werden, finden Sie in der [Überwachungsdatenreferenz zu Azure Files](storage-files-monitoring-reference.md).

Die Metriken und Protokolle in Azure Monitor unterstützen nur Azure Resource Manager-Speicherkonten. Klassische Speicherkonten werden von Azure Monitor nicht unterstützt. Wenn Sie Metriken oder Protokolle für ein klassisches Speicherkonto verwenden möchten, müssen Sie das Konto zu einem Azure Resource Manager-Speicherkonto migrieren. Weitere Informationen finden Sie unter [Migrieren zu Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview).

## <a name="collection-and-routing"></a>Sammlung und Routing

Plattformmetriken und das Aktivitätsprotokoll werden automatisch erfasst, können jedoch mithilfe einer Diagnoseeinstellung an andere Speicherorte weitergeleitet werden. Sie müssen eine Diagnoseeinstellung zum Sammeln von Ressourcenprotokollen erstellen. 

> [!NOTE]
> Azure Storage-Protokolle in Azure Monitor befinden sich in der öffentlichen Vorschauphase und stehen in allen Regionen für die öffentliche Cloud für Vorschautests zur Verfügung. Informationen zum Registrieren für die Vorschauversion finden Sie auf [dieser Seite](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u). Diese Vorschauversion ermöglicht Protokolle für Blobs (einschließlich Azure Data Lake Storage Gen2), Dateien, Warteschlangen, Tabellen, Universell V1-Premium-Speicherkonten und Universell V2-Speicherkonten. Klassische Speicherkonten werden nicht unterstützt.

Informationen zum Erstellen einer Diagnoseeinstellung über das Azure-Portal, die Azure CLI oder in PowerShell finden Sie unter [Erstellen von Diagnoseeinstellungen zum Sammeln von Plattformprotokollen und Metriken in Azure](../../azure-monitor/platform/diagnostic-settings.md). 

Eine Azure Resource Manager-Vorlage zum Erstellen einer Diagnoseeinstellung finden Sie unter [Diagnoseeinstellung für Azure Storage](https://docs.microsoft.com/azure/azure-monitor/samples/resource-manager-diagnostic-settings#diagnostic-setting-for-azure-storage).

Beim Erstellen einer Diagnoseeinstellung wählen 0Sie den Speichertyp aus, für den Sie Protokolle aktivieren möchten, z. B. Blob, Warteschlange, Tabelle oder Datei. Wählen Sie für Azure Files **Datei** aus. 

Wenn Sie die Diagnoseeinstellung im Azure-Portal erstellen, können Sie die Ressource aus einer Liste auswählen. Bei Verwendung von PowerShell oder der Azure-Befehlszeilenschnittstelle (Command Line Interface, CLI) müssen Sie die Ressourcen-ID des Azure Files-Endpunkts verwenden. Sie finden die Ressourcen-ID im Azure-Portal, indem Sie die Seite **Eigenschaften** Ihres Speicherkontos öffnen.

Außerdem müssen Sie eine der folgenden Kategorien von Vorgängen angeben, für die Protokolle erfasst werden sollen. 

| Kategorie | BESCHREIBUNG |
|:---|:---|
| StorageRead | Lesevorgänge für Objekte. |
| StorageWrite | Schreibvorgänge für Objekte. |
| StorageDelete | Löschvorgänge für Objekte. |

Wie Sie die Liste der protokollierten SMB- und REST-Vorgänge abrufen, erfahren Sie unter [Storage Analytics: protokollierte Vorgänge und Statusmeldungen](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) und [Überwachungsdatenreferenz zu Azure Files](storage-files-monitoring-reference.md).

## <a name="analyzing-metrics"></a>Analysieren von Metriken

Mit dem Metrik-Explorer können Sie Metriken für Azure Storage mit Metriken aus anderen Azure-Diensten analysieren. Öffnen Sie den Metrik-Explorer, indem Sie im Menü **Azure Monitor** die Option **Metriken** auswählen. Ausführliche Informationen zur Verwendung dieses Tools finden Sie unter [Erste Schritte mit dem Azure-Metrik-Explorer](../../azure-monitor/platform/metrics-getting-started.md). 

Bei Metriken mit Dimensionsunterstützung können Sie die Metrik nach dem gewünschten Dimensionswert filtern.  Eine vollständige Liste der von Azure Storage unterstützten Dimensionen finden Sie unter [Metrikdimensionen](storage-files-monitoring-reference.md#metrics-dimensions). Metriken für Azure Files befinden sich in diesen Namespaces: 

- Microsoft.Storage/storageAccounts
- Microsoft.Storage/storageAccounts/fileServices

Eine Liste aller von Azure Monitor unterstützten Metriken (einschließlich Azure Files) finden Sie unter [Unterstützte Metriken von Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftstoragestorageaccountsfileservices).

### <a name="accessing-metrics"></a>Zugreifen auf Metriken

> [!TIP]
> Wählen Sie zum Anzeigen von Azure CLI- oder .NET-Beispielen die hier aufgeführten entsprechenden Registerkarten aus.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

#### <a name="list-the-metric-definition"></a>Auflisten der Metrikdefinition

Sie können die Metrikdefinition Ihres Speicherkontos oder des Azure Files-Diensts auflisten. Verwenden Sie das Cmdlet [Get-AzMetricDefinition](https://docs.microsoft.com/powershell/module/az.monitor/get-azmetricdefinition).

Ersetzen Sie in diesem Beispiel den Platzhalter `<resource-ID>` durch die Ressourcen-ID des gesamten Speicherkontos oder die Ressourcen-ID des Azure Files-Diensts.  Sie finden diese Ressourcen-IDs im Azure-Portal auf der jeweiligen Seite **Eigenschaften** Ihres Speicherkontos.

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetricDefinition -ResourceId $resourceId
```

#### <a name="reading-metric-values"></a>Lesen von Metrikwerten

Sie können die Metrikwerte auf der Kontoebene Ihres Speicherkontos oder des Azure Files-Diensts lesen. Verwenden Sie das Cmdlet [Get-AzMetric](https://docs.microsoft.com/powershell/module/Az.Monitor/Get-AzMetric).

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetric -ResourceId $resourceId -MetricNames "UsedCapacity" -TimeGrain 01:00:00
```

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

#### <a name="list-the-account-level-metric-definition"></a>Auflisten der Metrikdefinition auf Kontoebene

Sie können die Metrikdefinition Ihres Speicherkontos oder des Azure Files-Diensts auflisten. Verwenden Sie den Befehl [az monitor metrics list-definitions](https://docs.microsoft.com/cli/azure/monitor/metrics#az-monitor-metrics-list-definitions).
 
Ersetzen Sie in diesem Beispiel den Platzhalter `<resource-ID>` durch die Ressourcen-ID des gesamten Speicherkontos oder die Ressourcen-ID des Azure Files-Diensts. Sie finden diese Ressourcen-IDs im Azure-Portal auf der jeweiligen Seite **Eigenschaften** Ihres Speicherkontos.

```azurecli-interactive
   az monitor metrics list-definitions --resource <resource-ID>
```

#### <a name="read-account-level-metric-values"></a>Lesen der Metrikwerte auf der Kontoebene

Sie können die Metrikwerte Ihres Speicherkontos oder des Azure Files-Diensts lesen. Verwenden Sie den Befehl [az monitor metrics list](https://docs.microsoft.com/cli/azure/monitor/metrics#az-monitor-metrics-list).

```azurecli-interactive
   az monitor metrics list --resource <resource-ID> --metric "UsedCapacity" --interval PT1H
```

### <a name="net"></a>[.NET](#tab/dotnet)

Azure Monitor bietet das [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/) zum Lesen von Metrikdefinition und -werten. Die [Beispielcode](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/) zeigt, wie das SDK mit unterschiedlichen Parametern verwendet wird. Sie benötigen `0.18.0-preview` oder eine höhere Version für Speichermetriken.
 
Ersetzen Sie in diesen Beispielen den Platzhalter `<resource-ID>` durch die Ressourcen-ID des gesamten Speicherkontos oder des Azure Files-Diensts. Sie finden diese Ressourcen-IDs im Azure-Portal auf der jeweiligen Seite **Eigenschaften** Ihres Speicherkontos.

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

#### <a name="reading-account-level-metric-values"></a>Lesen von Metrikwerten auf Kontoebene

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

#### <a name="reading-multidimensional-metric-values"></a>Lesen von Werten mehrdimensionaler Metriken

Für mehrdimensionale Metriken müssen Sie Metadatenfilter definieren, wenn Metrikdaten für bestimmte Dimensionswerte gelesen werden sollen.

Im folgenden Beispiel wird gezeigt, wie Metrikdaten bei Metriken mit Unterstützung für mehrere Dimensionen gelesen werden:

```csharp
    public static async Task ReadStorageMetricValueTest()
    {
        // Resource ID for Azure Files
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/fileServices/default";
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

## <a name="analyzing-logs"></a>Analysieren von Protokollen

Sie können auf Ressourcenprotokolle entweder als Blob in einem Speicherkonto, als Ereignisdaten oder über Log Analytics-Abfragen zugreifen.

Wie Sie die Liste der protokollierten SMB- und REST-Vorgänge abrufen, erfahren Sie unter [Storage Analytics: protokollierte Vorgänge und Statusmeldungen](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) und [Überwachungsdatenreferenz zu Azure Files](storage-files-monitoring-reference.md).

> [!NOTE]
> Azure Storage-Protokolle in Azure Monitor befinden sich in der öffentlichen Vorschauphase und stehen in allen Regionen für die öffentliche Cloud für Vorschautests zur Verfügung. Informationen zum Registrieren für die Vorschauversion finden Sie auf [dieser Seite](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u). Diese Vorschauversion ermöglicht Protokolle für Blobs (einschließlich Azure Data Lake Storage Gen2), Dateien, Warteschlangen, Tabellen, Universell V1-Premium-Speicherkonten und Universell V2-Speicherkonten. Klassische Speicherkonten werden nicht unterstützt.

Protokolleinträge werden nur erstellt, wenn Anforderungen für den Dienstendpunkt gestellt wurden. Wenn beispielsweise ein Speicherkonto Aktivität im Dateiendpunkt, jedoch nicht im Tabellen- oder Warteschlangenendpunkt aufweist, werden nur Protokolle für den Dateidienst erstellt. Azure Storage-Protokolle enthalten ausführliche Informationen über erfolgreiche und fehlgeschlagene Anforderungen an einen Speicherdienst. Anhand dieser Informationen können einzelne Anforderungen überwacht und Probleme mit einem Speicherdienst untersucht werden. Anforderungen werden auf Grundlage der besten Leistung protokolliert.

### <a name="log-authenticated-requests"></a>Protokollieren von authentifizierten Anforderungen

 Die folgenden Typen authentifizierter Anforderungen werden protokolliert:

- Erfolgreiche Anforderungen
- Fehlerhafte Anforderungen, einschließlich Timeout-, Drosselungs-, Netzwerk- und Autorisierungsfehler sowie anderer Fehler
- Anforderungen, die eine SAS (Shared Access Signature) oder OAuth verwenden, einschließlich fehlerhafter und erfolgreicher Anforderungen
- Anforderungen an Analysedaten (klassische Protokolldaten im Container **$logs** und klassische Metrikdaten in den **$metric**-Tabellen)

Anforderungen, die durch den Azure Files-Dienst selbst erfolgen, wie z. B. Protokollerstellungs- oder -löschvorgänge, werden nicht protokolliert. Eine vollständige Liste der protokollierten SMB- und REST-Anforderungen finden Sie unter [Storage Analytics: protokollierte Vorgänge und Statusmeldungen](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) und [Überwachungsdatenreferenz zu Azure Files](storage-files-monitoring-reference.md).

### <a name="log-anonymous-requests"></a>Protokollieren anonymer Anforderungen

 Die folgenden Typen anonymer Anforderungen werden protokolliert:

- Erfolgreiche Anforderungen
- Serverfehler
- Timeoutfehler für Client und Server
- Mit Fehlercode 304 (nicht geändert) fehlgeschlagene GET-Anforderungen

Alle anderen fehlgeschlagenen, anonymen Anforderungen werden nicht protokolliert. Eine vollständige Liste der protokollierten SMB- und REST-Anforderungen finden Sie unter [Storage Analytics: protokollierte Vorgänge und Statusmeldungen](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) und [Überwachungsdatenreferenz zu Azure Files](storage-files-monitoring-reference.md).

### <a name="accessing-logs-in-a-storage-account"></a>Zugreifen auf Protokolle in einem Speicherkonto

Protokolle werden als Blobs angezeigt, die im Zielspeicherkonto in einem Container gespeichert sind. Die erfassten Daten werden in einem einzelnen Blob als durch Zeilen getrennte JSON-Nutzlast gespeichert. Für den Namen des Blobs wird diese Benennungskonvention verwendet:

`https://<destination-storage-account>.blob.core.windows.net/insights-logs-<storage-operation>/resourceId=/subscriptions/<subscription-ID>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<source-storage-account>/fileServices/default/y=<year>/m=<month>/d=<day>/h=<hour>/m=<minute>/PT1H.json`

Hier sehen Sie ein Beispiel:

`https://mylogstorageaccount.blob.core.windows.net/insights-logs-storagewrite/resourceId=/subscriptions/`<br>`208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/fileServices/default/y=2019/m=07/d=30/h=23/m=12/PT1H.json`

### <a name="accessing-logs-in-an-event-hub"></a>Zugreifen auf Protokolle in einem Event Hub

An einen Event Hub gesendete Protokolle werden nicht als Datei gespeichert. Sie können jedoch überprüfen, ob der Event Hub die Protokollinformationen empfangen hat. Wechseln Sie im Azure-Portal zu Ihrem Event Hub, und vergewissern Sie sich, dass die Anzahl **eingehender Nachrichten** größer als Null ist. 

![Überwachungsprotokolle](media/storage-files-monitoring/event-hub-log.png)

Mithilfe von SIEM- (Security Information & Event Management) und Überwachungstools können Sie auf die an Ihren Event Hub gesendeten Protokolldaten zugreifen und diese lesen. Unter [Wie kann ich die an meinen Event Hub gesendeten Überwachungsdaten nutzen?](https://docs.microsoft.com/azure/azure-monitor/platform/stream-monitoring-data-event-hubs#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub) finden Sie weitere Informationen dazu.

### <a name="accessing-logs-in-a-log-analytics-workspace"></a>Zugreifen auf Protokolle in einem Log Analytics-Arbeitsbereich

Mithilfe von Azure Monitor-Protokollabfragen können Sie auf Protokolle zugreifen, die an einen Log Analytics-Arbeitsbereich gesendet wurden. Die Daten werden in der **StorageFileLogs**-Tabelle gespeichert. 

Weitere Informationen finden Sie unter [Erste Schritte mit Log Analytics in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal).

#### <a name="sample-kusto-queries"></a>Kusto-Beispielabfragen

Im Folgenden finden Sie einige Abfragen, die Sie in die **Protokollsuchleiste** eingeben können, um die Überwachung Ihrer Azure Files-Instanz zu vereinfachen. Diese Abfragen arbeiten mit der [neuen Sprache](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

> [!IMPORTANT]
> Wenn Sie **Protokolle** im Speicherkonto-Ressourcengruppenmenü auswählen, wird Log Analytics geöffnet, wobei der Abfragebereich auf die aktuelle Ressourcengruppe festgelegt ist. Dies bedeutet, dass Protokollabfragen nur Daten aus dieser Ressourcengruppe umfassen. Wenn Sie eine Abfrage ausführen möchten, die Daten aus anderen Ressourcen oder Daten aus anderen Azure-Diensten enthält, wählen Sie im Menü **Azure Monitor** die Option **Protokolle** aus. Ausführliche Informationen finden Sie unter [Protokollabfragebereich und Zeitbereich in Azure Monitor Log Analytics](/azure/azure-monitor/log-query/scope/).

Verwenden Sie diese Abfragen, um Ihre Azure-Dateifreigaben zu überwachen:

- Anzeigen von SMB-Fehlern in der letzten Woche

```Kusto
StorageFileLogs
| where Protocol == "SMB" and TimeGenerated >= ago(7d) and StatusCode contains "-"
| sort by StatusCode
```
- Erstellen eines Kreisdiagramms der SMB-Vorgänge in der letzten Woche

```Kusto
StorageFileLogs
| where Protocol == "SMB" and TimeGenerated >= ago(7d) 
| summarize count() by OperationName
| sort by count_ desc
| render piechart
```

- Anzeigen von REST-Fehlern in der letzten Woche

```Kusto
StorageFileLogs
| where Protocol == "HTTPS" and TimeGenerated >= ago(7d) and StatusText !contains "Success"
| sort by StatusText asc
```

- Erstellen eines Kreisdiagramms der REST-Vorgänge in der letzten Woche

```Kusto
StorageFileLogs
| where Protocol == "HTTPS" and TimeGenerated >= ago(7d) 
| summarize count() by OperationName
| sort by count_ desc
| render piechart
```

Informationen zum Anzeigen der Liste der Spaltennamen und Beschreibungen für Azure Files finden Sie unter [StorageFileLogs](https://docs.microsoft.com/azure/azure-monitor/reference/tables/storagefilelogs).

Weitere Informationen zum Schreiben von Abfragen finden Sie unter [Tutorial: Erste Schritte mit Log Analytics-Abfragen](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal).

## <a name="alerts"></a>Alerts

Azure Monitor-Warnungen informieren Sie proaktiv, wenn wichtige Bedingungen in Ihren Überwachungsdaten gefunden werden. Sie ermöglichen Ihnen, Probleme in Ihrem System zu identifizieren und zu beheben, bevor Ihre Kunden sie bemerken. Sie können Warnungen für [Metriken](/azure/azure-monitor/platform/alerts-metric-overview), [Protokolle](/azure/azure-monitor/platform/alerts-unified-log) und das [Aktivitätsprotokoll](/azure/azure-monitor/platform/activity-log-alerts) festlegen. 

In der folgenden Tabelle sind einige Beispielszenarios für die Überwachung und die jeweils geeignete Metrik für die Warnung aufgeführt:

| Szenario | Für Warnung zu verwendende Metrik |
|-|-|
| Dateifreigabe wird gedrosselt. | Metrik: Transaktionen<br>Dimensionsname: Antworttyp <br>Dimensionsname: FileShare (nur Freigabe von Premiumdateien) |
| Die Größe der Dateifreigabe beträgt 80 % der Kapazität. | Metrik: Dateikapazität<br>Dimensionsname: FileShare (nur Freigabe von Premiumdateien) |
| Der Dateifreigabeausgang hat 500 GiB an einem Tag überschritten. | Metrik: Ausgehende Daten<br>Dimensionsname: FileShare (nur Freigabe von Premiumdateien) |

### <a name="how-to-create-alerts-for-azure-files"></a>Erstellen von Warnungen für Azure Files

1. Navigieren Sie im **Azure-Portal** zu Ihrem **Speicherkonto**. 

2. Klicken Sie auf **Warnungen** und dann auf **+ Neue Warnungsregel**.

3. Klicken Sie auf **Ressource bearbeiten**, wählen Sie den **Dateiressourcentyp** aus, und klicken Sie anschließend auf **Fertig**. 

4. Klicken Sie auf **Bedingung auswählen**, und geben Sie die folgenden Informationen für die Warnung an: 

    - **Metrik**
    - **Dimensionsname**
    - **Warnungslogik**

5. Klicken Sie auf **Aktionsgruppe auswählen**, und fügen Sie der Warnung eine Aktionsgruppe (E-Mail, SMS usw.) hinzu, indem Sie entweder eine bestehende Aktionsgruppe auswählen oder eine neue Aktionsgruppe erstellen.

6. Geben Sie die **Warnungsdetails** wie **Warnungsregelname**, **Beschreibung** und **Schweregrad** ein.

7. Klicken Sie auf **Warnungsregel erstellen**, um die Warnung zu erstellen.

> [!NOTE]  
> Wenn Sie eine Warnung erstellen und zu viele Warnungen ausgelöst werden, passen Sie den Schwellenwert und die Warnungslogik an.

### <a name="how-to-create-an-alert-if-a-file-share-is-throttled"></a>Erstellen einer Warnung, wenn eine Dateifreigabe gedrosselt ist

1. Navigieren Sie im **Azure-Portal** zu Ihrem **Speicherkonto**.
2. Klicken Sie im Abschnitt **Überwachung** auf **Warnungen** und anschließend auf **+ Neue Warnungsregel**.
3. Klicken Sie auf **Ressource bearbeiten**, wählen Sie unter **Ressourcentyp** den Ressourcentyp für das Speicherkonto aus, und klicken Sie anschließend auf **Fertig**. Wenn der Name des Speicherkontos beispielsweise `contoso` ist, wählen Sie die `contoso/file`-Ressource aus.
4. Klicken Sie auf **Bedingung auswählen**, um eine Bedingung hinzuzufügen.
5. Wählen Sie in der daraufhin angezeigten Liste der für das Speicherkonto unterstützten Signale die Metrik **Transaktionen** aus.
6. Klicken Sie auf dem Blatt **Signallogik konfigurieren** auf das Dropdownmenü **Dimensionsname**, und wählen Sie **Antworttyp** aus.
7. Klicken Sie auf die Dropdownliste **Dimensionswerte**, und wählen Sie **SuccessWithThrottling** (für SMB) oder **ClientThrottlingError** (für REST) aus.

   > [!NOTE]
   > Wenn der Dimensionswert „SuccessWithThrottling“ oder „ClientThrottlingError“ nicht angezeigt wird, bedeutet dies, dass die Ressource nicht gedrosselt wurde. Klicken Sie zum Hinzufügen des Dimensionswerts neben der Dropdownliste **Dimensionswerte** auf **Benutzerdefinierten Wert hinzufügen**, geben Sie **SuccessWithThrottling** oder **ClientThrottlingError** ein, klicken Sie auf **OK**, und wiederholen Sie anschließend Schritt 7.

8. Klicken Sie auf die Dropdownliste **Dimensionsname**, und wählen Sie **Dateifreigabe** aus.
9. Klicken Sie auf die Dropdownliste **Dimensionswerte**, und wählen Sie die Dateifreigaben aus, für die Sie eine Warnung einrichten möchten.

   > [!NOTE]
   > Handelt es sich bei der Dateifreigabe um eine Standarddateifreigabe, wählen Sie **Alle aktuellen und zukünftigen Werte** aus. Die Dateifreigaben werden in der Dropdownliste mit den Dimensionswerten nicht aufgeführt, da für Standarddateifreigaben keine freigabespezifischen Metriken zur Verfügung stehen. Die Drosselung von Warnungen für Standarddateifreigaben wird ausgelöst, wenn eine Dateifreigabe im Speicherkonto gedrosselt wird und die Warnung nicht identifiziert, welche Freigabe gedrosselt wurde. Da Metriken pro Freigabe für Standarddateifreigaben nicht verfügbar sind, wird empfohlen, dass es eine einzige Dateifreigabe pro Speicherkonto gibt.

10. Definieren Sie die **Warnungsparameter** (Schwellenwert, Operator, Aggregationsgranularität und Häufigkeit der Auswertung), und klicken Sie auf **Fertig**.

    > [!TIP]
    > Bei Verwendung eines statischen Schwellenwerts kann das Metrikdiagramm bei der Ermittlung eines sinnvollen Schwellenwerts helfen, wenn die Dateifreigabe gerade gedrosselt wird. Falls Sie einen dynamischen Schwellenwert verwenden, zeigt das Metrikdiagramm die berechneten Schwellenwerte basierend auf aktuellen Daten an.

11. Klicken Sie auf **Aktionsgruppe auswählen**, um der Warnung eine **Aktionsgruppe** (E-Mail, SMS usw.) hinzuzufügen, indem Sie entweder eine bestehende Aktionsgruppe auswählen oder eine neue Aktionsgruppe erstellen.
12. Geben Sie die **Warnungsdetails** wie **Warnungsregelname**, **Beschreibung und **Schweregrad** ein.
13. Klicken Sie auf **Warnungsregel erstellen**, um die Warnung zu erstellen.

### <a name="how-to-create-an-alert-if-the-azure-file-share-size-is-80-of-capacity"></a>Erstellen einer Warnung, wenn die Größe der Azure-Dateifreigabe 80 % der Kapazität beträgt

1. Navigieren Sie im **Azure-Portal** zu Ihrem **Speicherkonto**.
2. Klicken Sie im Abschnitt **Überwachung** auf **Warnungen** und anschließend auf **+ Neue Warnungsregel**.
3. Klicken Sie auf **Ressource bearbeiten**, wählen Sie unter **Ressourcentyp** den Ressourcentyp für das Speicherkonto aus, und klicken Sie anschließend auf **Fertig**. Wenn der Name des Speicherkontos beispielsweise `contoso` ist, wählen Sie die `contoso/file`-Ressource aus.
4. Klicken Sie auf **Bedingung auswählen**, um eine Bedingung hinzuzufügen.
5. Wählen Sie in der daraufhin angezeigten Liste der für das Speicherkonto unterstützten Signale die Metrik **Dateikapazität** aus.
6. Klicken Sie auf dem Blatt **Signallogik konfigurieren** auf das Dropdownmenü **Dimensionsname**, und wählen Sie **Dateifreigabe** aus.
7. Klicken Sie auf die Dropdownliste **Dimensionswerte**, und wählen Sie die Dateifreigaben aus, für die Sie eine Warnung einrichten möchten.

   > [!NOTE]
   > Handelt es sich bei der Dateifreigabe um eine Standarddateifreigabe, wählen Sie **Alle aktuellen und zukünftigen Werte** aus. Die Dateifreigaben werden in der Dropdownliste mit den Dimensionswerten nicht aufgeführt, da für Standarddateifreigaben keine freigabespezifischen Metriken zur Verfügung stehen. Warnungen für Standarddateifreigaben basieren auf allen Dateifreigaben im Speicherkonto. Da Metriken pro Freigabe für Standarddateifreigaben nicht verfügbar sind, wird empfohlen, dass es eine einzige Dateifreigabe pro Speicherkonto gibt.

8. Geben Sie den **Schwellenwert** in Bytes ein. Wenn die Größe der Dateifreigabe z. B. 100 TiB beträgt und Sie eine Warnung erhalten möchten, wenn die Größe der Dateifreigabe 80 % der Kapazität beträgt, ist der Schwellenwert in Bytes 87960930222080.
9. Definieren Sie die übrigen **Warnungsparameter** (Aggregationsgranularität und Häufigkeit der Auswertung), und klicken Sie auf **Fertig**.
10. Klicken Sie auf Aktionsgruppe auswählen, um der Warnung eine Aktionsgruppe (E-Mail, SMS usw.) hinzuzufügen, indem Sie entweder eine bestehende Aktionsgruppe auswählen oder eine neue Aktionsgruppe erstellen.
11. Geben Sie die **Warnungsdetails** wie **Warnungsregelname**, **Beschreibung und **Schweregrad** ein.
12. Klicken Sie auf **Warnungsregel erstellen**, um die Warnung zu erstellen.

### <a name="how-to-create-an-alert-if-the-azure-file-share-egress-has-exceeded-500-gib-in-a-day"></a>Erstellen einer Warnung, wenn der Ausgang der Azure-Dateifreigabe 500 GiB pro Tag überschritten hat

1. Navigieren Sie im **Azure-Portal** zu Ihrem **Speicherkonto**.
2. Klicken Sie im Abschnitt „Überwachung“ auf **Warnungen** und anschließend auf **+ Neue Warnungsregel**.
3. Klicken Sie auf **Ressource bearbeiten**, wählen Sie unter **Ressourcentyp** den Ressourcentyp für das Speicherkonto aus, und klicken Sie anschließend auf **Fertig**. Wenn der Name des Speicherkontos z. B. „contoso“ lautet, wählen Sie die Ressource „contoso/datei“ aus.
4. Klicken Sie auf **Bedingung auswählen**, um eine Bedingung hinzuzufügen.
5. Wählen Sie in der daraufhin angezeigten Liste der für das Speicherkonto unterstützten Signale die Metrik **Ausgehend** aus.
6. Klicken Sie auf dem Blatt **Signallogik konfigurieren** auf das Dropdownmenü **Dimensionsname**, und wählen Sie **Dateifreigabe** aus.
7. Klicken Sie auf die Dropdownliste **Dimensionswerte**, und wählen Sie die Dateifreigaben aus, für die Sie eine Warnung einrichten möchten.

   > [!NOTE]
   > Handelt es sich bei der Dateifreigabe um eine Standarddateifreigabe, wählen Sie **Alle aktuellen und zukünftigen Werte** aus. Die Dateifreigaben werden in der Dropdownliste mit den Dimensionswerten nicht aufgeführt, da für Standarddateifreigaben keine freigabespezifischen Metriken zur Verfügung stehen. Warnungen für Standarddateifreigaben basieren auf allen Dateifreigaben im Speicherkonto. Da Metriken pro Freigabe für Standarddateifreigaben nicht verfügbar sind, wird empfohlen, dass es eine einzige Dateifreigabe pro Speicherkonto gibt.

8. Geben Sie **536870912000** Bytes für den Schwellenwert ein. 
9. Klicken Sie auf die Dropdownliste **Aggregationsgranularität**, und wählen Sie **24 Stunden** aus.
10. Wählen Sie die **Häufigkeit der Auswertung** aus, und klicken Sie auf **Fertig**.
11. Klicken Sie auf **Aktionsgruppe auswählen**, um der Warnung eine **Aktionsgruppe** (E-Mail, SMS usw.) hinzuzufügen, indem Sie entweder eine bestehende Aktionsgruppe auswählen oder eine neue Aktionsgruppe erstellen.
12. Geben Sie die **Warnungsdetails** wie **Warnungsregelname**, **Beschreibung und **Schweregrad** ein.
13. Klicken Sie auf **Warnungsregel erstellen**, um die Warnung zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

- [Überwachen von Azure Files](storage-files-monitoring.md)
- [Überwachen von Azure-Ressourcen mit Azure Monitor](../../azure-monitor/insights/monitor-azure-resource.md)
- [Migration von Azure Storage-Metriken](../common/storage-metrics-migration.md)
- [Planung für eine Azure Files-Bereitstellung](https://docs.microsoft.com/azure/storage/files/storage-files-planning)
- [Bereitstellen von Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-deployment-guide)
- [Problembehandlung für Azure Files unter Windows](https://docs.microsoft.com/azure/storage/files/storage-troubleshoot-windows-file-connection-problems)
- [Problembehandlung für Azure Files unter Linux](https://docs.microsoft.com/azure/storage/files/storage-troubleshoot-linux-file-connection-problems)
