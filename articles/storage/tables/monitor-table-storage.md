---
title: Überwachen von Azure-Tabellenspeicher | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Leistung und Verfügbarkeit von Azure-Tabellenspeicher überwachen können. Überwachen Sie Azure-Tabellenspeicherdaten, erfahren Sie mehr über die Konfiguration, und analysieren Sie Metrik- und Protokolldaten.
author: normesta
services: storage
ms.service: storage
ms.topic: conceptual
ms.date: 10/02/2020
ms.author: normesta
ms.reviewer: fryu
ms.custom: monitoring, devx-track-csharp
ms.openlocfilehash: 8104d1d1f8864f8b7c5a6add6c602007f2d04822
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91711037"
---
# <a name="monitoring-azure-table-storage"></a>Überwachen von Azure-Tabellenspeicher

Wenn Sie über unternehmenskritische Anwendungen und Geschäftsprozesse verfügen, die auf Azure-Ressourcen basieren, sollten Sie diese Ressourcen auf Verfügbarkeit, Leistung und Betrieb überwachen. In diesem Artikel wird das Überwachen von Daten beschrieben, die von Azure-Tabellenspeicher generiert wurden. Außerdem wird erläutert, wie Sie die Funktionen von Azure Monitor verwenden können, um Warnungen für diese Daten zu analysieren.

> [!NOTE]
> Azure Storage-Protokolle in Azure Monitor befinden sich in der öffentlichen Vorschauphase und stehen in allen Regionen für die öffentliche Cloud für Vorschautests zur Verfügung. Informationen zum Registrieren für die Vorschauversion finden Sie auf [dieser Seite](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u). Diese Vorschau unterstützt Protokolle für Blobdateien (einschließlich Azure Data Lake Storage Gen2), Dateien, Warteschlangen und Tabellen. Dieses Feature ist für alle Speicherkonten verfügbar, die mit dem Azure Resource Manager-Bereitstellungsmodell erstellt wurden. Weitere Informationen finden Sie in der [Speicherkontoübersicht](../common/storage-account-overview.md).

## <a name="monitor-overview"></a>Azure Monitor: Übersicht

Im Azure-Portal enthält die Seite **Übersicht** für jede Tabellenspeicherressource eine kurze Übersicht über die Ressourcennutzung, beispielsweise Anforderungen und stündliche Abrechnung. Dies sind nützliche Informationen, die aber nur einen kleinen Teil der verfügbaren Überwachungsdaten ausmachen. Einige dieser Daten werden automatisch erfasst und sind für die Analyse verfügbar, sobald Sie die Ressource erstellen. Mit einigen Konfigurationsschritten können Sie zusätzliche Typen von Datensammlungen aktivieren.

## <a name="what-is-azure-monitor"></a>Was ist Azure Monitor?
Azure-Tabellenspeicher protokolliert Überwachungsdaten mit [Azure Monitor](../../azure-monitor/overview.md), einem Azure-Dienst zur vollständigen Stapelüberwachung. Azure Monitor bietet einen vollständigen Satz von Funktionen zum Überwachen Ihrer Azure-Ressourcen, von Ressourcen in anderen Clouds sowie von lokalen Ressourcen. 

Beginnen Sie mit dem Artikel [Überwachen von Azure-Ressourcen mit Azure Monitor](../../azure-monitor/insights/monitor-azure-resource.md), in dem die folgenden Punkte beschrieben werden:

- Was ist Azure Monitor?
- Kosten für die Überwachung
- In Azure gesammelte Überwachungsdaten
- Konfigurieren der Datensammlung
- Standardtools in Azure zum Analysieren von Überwachungsdaten sowie zum Generieren von Warnungen

Die folgenden Abschnitte basieren auf diesem Artikel. Darin werden die spezifischen von Azure Storage erfassten Daten beschrieben. In Beispielen wird gezeigt, wie Sie die Datensammlung konfigurieren und diese Daten mit Azure-Tools analysieren.

## <a name="monitoring-data"></a>Überwachungsdaten

Azure-Tabellenspeicher erfasst dieselben Arten von Überwachungsdaten wie andere Azure-Ressourcen, die unter [Überwachungsdaten von Azure-Ressourcen](../../azure-monitor/insights/monitor-azure-resource.md#monitoring-data) beschrieben werden. 

Ausführliche Informationen zu den Metriken und Protokollen, die von Azure-Tabellenspeicher erstellt werden, finden Sie in der [Überwachungsdatenreferenz zu Azure-Tabellenspeicher](monitor-table-storage-reference.md).

Die Metriken und Protokolle in Azure Monitor unterstützen nur Azure Resource Manager-Speicherkonten. Klassische Speicherkonten werden von Azure Monitor nicht unterstützt. Wenn Sie Metriken oder Protokolle für ein klassisches Speicherkonto verwenden möchten, müssen Sie das Konto zu einem Azure Resource Manager-Speicherkonto migrieren. Weitere Informationen finden Sie unter [Migrieren zu Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview).

Wenn Sie möchten, können Sie klassische Metriken und Protokolle auch weiterhin verwenden. Klassische Metriken und Protokolle sind parallel zu den Metriken und Protokollen in Azure Monitor verfügbar. Die Unterstützung bleibt bestehen, bis Azure Storage den Dienst für Legacymetriken und -protokolle einstellt.

## <a name="collection-and-routing"></a>Sammlung und Routing

Plattformmetriken und das Aktivitätsprotokoll werden automatisch erfasst, können jedoch mithilfe einer Diagnoseeinstellung an andere Speicherorte weitergeleitet werden. Sie müssen eine Diagnoseeinstellung zum Sammeln von Ressourcenprotokollen erstellen. 

Informationen zum Erstellen einer Diagnoseeinstellung über das Azure-Portal, die Azure CLI oder in PowerShell finden Sie unter [Erstellen von Diagnoseeinstellungen zum Sammeln von Plattformprotokollen und Metriken in Azure](../../azure-monitor/platform/diagnostic-settings.md). 

Eine Azure Resource Manager-Vorlage zum Erstellen einer Diagnoseeinstellung finden Sie unter [Diagnoseeinstellung für Azure Storage](https://docs.microsoft.com/azure/azure-monitor/samples/resource-manager-diagnostic-settings#diagnostic-setting-for-azure-storage).

Beim Erstellen einer Diagnoseeinstellung wählen 0Sie den Speichertyp aus, für den Sie Protokolle aktivieren möchten, z. B. Blob, Warteschlange, Tabelle oder Datei. Wählen Sie für Tabellenspeicher **Tabelle**. 

Wenn Sie die Diagnoseeinstellung im Azure-Portal erstellen, können Sie die Ressource aus einer Liste auswählen. Bei Verwendung von PowerShell oder der Azure-Befehlszeilenschnittstelle (Command Line Interface, CLI) müssen Sie die Ressourcen-ID des Tabellenspeicher-Endpunkts verwenden. Sie finden die Ressourcen-ID im Azure-Portal, indem Sie die Seite **Eigenschaften** Ihres Speicherkontos öffnen.

Außerdem müssen Sie eine der folgenden Kategorien von Vorgängen angeben, für die Protokolle erfasst werden sollen. 

| Kategorie | BESCHREIBUNG |
|:---|:---|
| StorageRead | Lesevorgänge für Objekte. |
| StorageWrite | Schreibvorgänge für Objekte. |
| StorageDelete | Löschvorgänge für Objekte. |

## <a name="analyzing-metrics"></a>Analysieren von Metriken

Mit dem Metrik-Explorer können Sie Metriken für Azure Storage mit Metriken aus anderen Azure-Diensten analysieren. Öffnen Sie den Metrik-Explorer, indem Sie im Menü **Azure Monitor** die Option **Metriken** auswählen. Ausführliche Informationen zur Verwendung dieses Tools finden Sie unter [Erste Schritte mit dem Azure-Metrik-Explorer](../../azure-monitor/platform/metrics-getting-started.md). 

Dieses Beispiel zeigt, wie Sie **Transaktionen** auf der Kontoebene anzeigen.

![Screenshot für den Zugriff auf Metriken über das Azure-Portal](./media/monitor-table-storage/access-metrics-portal.png)

Bei Metriken mit Dimensionsunterstützung können Sie die Metrik nach dem gewünschten Dimensionswert filtern. Dieses Beispiel zeigt, wie Sie **Transaktionen** auf der Kontoebene für einen bestimmten Vorgang anzeigen, indem Sie Werte für die Dimension **API-Name** auswählen.

![Screenshot für den Zugriff auf Metriken mit Dimension über das Azure-Portal](./media/monitor-table-storage/access-metrics-portal-with-dimension.png)

Eine vollständige Liste der von Azure Storage unterstützten Dimensionen finden Sie unter [Metrikdimensionen](monitor-table-storage-reference.md#metrics-dimensions).

Metriken für Azure-Tabellenspeicher befinden sich in diesen Namespaces: 

- Microsoft.Storage/storageAccounts
- Microsoft.Storage/storageAccounts/tableServices

Eine Liste aller von Azure Monitor unterstützten Metriken (einschließlich Azure-Tabellenspeicher) finden Sie unter [Unterstützte Metriken von Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported).


### <a name="accessing-metrics"></a>Zugreifen auf Metriken

> [!TIP]
> Wählen Sie zum Anzeigen von Azure CLI- oder .NET-Beispielen die hier aufgeführten entsprechenden Registerkarten aus.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

#### <a name="list-the-metric-definition"></a>Auflisten der Metrikdefinition

Sie können die Metrikdefinition Ihres Speicherkontos oder des Tabellenspeicherdiensts auflisten. Verwenden Sie das Cmdlet [Get-AzMetricDefinition](https://docs.microsoft.com/powershell/module/az.monitor/get-azmetricdefinition).

Ersetzen Sie in diesem Beispiel den Platzhalter `<resource-ID>` durch die Ressourcen-ID des gesamten Speicherkontos oder die Ressourcen-ID des Tabellenspeicherdiensts.  Sie finden diese Ressourcen-IDs im Azure-Portal auf der jeweiligen Seite **Eigenschaften** Ihres Speicherkontos.

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetricDefinition -ResourceId $resourceId
```

#### <a name="reading-metric-values"></a>Lesen von Metrikwerten

Sie können die Metrikwerte auf der Kontoebene Ihres Speicherkontos oder des Tabellenspeicherdiensts lesen. Verwenden Sie das Cmdlet [Get-AzMetric](https://docs.microsoft.com/powershell/module/Az.Monitor/Get-AzMetric).

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetric -ResourceId $resourceId -MetricNames "UsedCapacity" -TimeGrain 01:00:00
```

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

#### <a name="list-the-account-level-metric-definition"></a>Auflisten der Metrikdefinition auf Kontoebene

Sie können die Metrikdefinition Ihres Speicherkontos oder des Tabellenspeicherdiensts auflisten. Verwenden Sie den Befehl [az monitor metrics list-definitions](https://docs.microsoft.com/cli/azure/monitor/metrics#az-monitor-metrics-list-definitions).
 
Ersetzen Sie in diesem Beispiel den Platzhalter `<resource-ID>` durch die Ressourcen-ID des gesamten Speicherkontos oder die Ressourcen-ID des Tabellenspeicherdiensts. Sie finden diese Ressourcen-IDs im Azure-Portal auf der jeweiligen Seite **Eigenschaften** Ihres Speicherkontos.

```azurecli-interactive
   az monitor metrics list-definitions --resource <resource-ID>
```

#### <a name="read-account-level-metric-values"></a>Lesen der Metrikwerte auf der Kontoebene

Sie können die Metrikwerte Ihres Speicherkontos oder des Tabellenspeicherdiensts lesen. Verwenden Sie den Befehl [az monitor metrics list](https://docs.microsoft.com/cli/azure/monitor/metrics#az-monitor-metrics-list).

```azurecli-interactive
   az monitor metrics list --resource <resource-ID> --metric "UsedCapacity" --interval PT1H
```

### <a name="net"></a>[.NET](#tab/dotnet)

Azure Monitor bietet das [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/) zum Lesen von Metrikdefinition und -werten. Die [Beispielcode](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/) zeigt, wie das SDK mit unterschiedlichen Parametern verwendet wird. Sie benötigen `0.18.0-preview` oder eine höhere Version für Speichermetriken.
 
Ersetzen Sie in diesen Beispielen den Platzhalter `<resource-ID>` durch die Ressourcen-ID des gesamten Speicherkontos oder des Tabellenspeicherdiensts. Sie finden diese Ressourcen-IDs im Azure-Portal auf der jeweiligen Seite **Eigenschaften** Ihres Speicherkontos.

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
        // Resource ID for table storage
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/tableServices/default";
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

Eine ausführliche Referenz zu den in diesen Protokollen angezeigten Feldern finden Sie in der [Überwachungsdatenreferenz zu Azure-Tabellenspeicher](monitor-table-storage-reference.md).

> [!NOTE]
> Azure Storage-Protokolle in Azure Monitor befinden sich in der öffentlichen Vorschauphase und stehen in allen Regionen für die öffentliche Cloud für Vorschautests zur Verfügung. Informationen zum Registrieren für die Vorschauversion finden Sie auf [dieser Seite](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u). Diese Vorschauversion ermöglicht Protokolle für Blobs (einschließlich Azure Data Lake Storage Gen2), Dateien, Warteschlangen, Tabellen, Universell V1-Premium-Speicherkonten und Universell V2-Speicherkonten. Klassische Speicherkonten werden nicht unterstützt.

Protokolleinträge werden nur erstellt, wenn Anforderungen für den Dienstendpunkt gestellt wurden. Wenn beispielsweise ein Speicherkonto Aktivität im Tabellenendpunkt, jedoch nicht im Blob- oder Warteschlangen-Endpunkt aufweist, werden nur Protokolle für den Tabellendienst erstellt. Azure Storage-Protokolle enthalten ausführliche Informationen über erfolgreiche und fehlgeschlagene Anforderungen an einen Speicherdienst. Anhand dieser Informationen können einzelne Anforderungen überwacht und Probleme mit einem Speicherdienst untersucht werden. Anforderungen werden auf Grundlage der besten Leistung protokolliert.

### <a name="log-authenticated-requests"></a>Protokollieren von authentifizierten Anforderungen

 Die folgenden Typen authentifizierter Anforderungen werden protokolliert:

- Erfolgreiche Anforderungen
- Fehlerhafte Anforderungen, einschließlich Timeout-, Drosselungs-, Netzwerk- und Autorisierungsfehler sowie anderer Fehler
- Anforderungen, die eine SAS (Shared Access Signature) oder OAuth verwenden, einschließlich fehlerhafter und erfolgreicher Anforderungen
- Anforderungen an Analysedaten (klassische Protokolldaten im Container **$logs** und klassische Metrikdaten in den **$metric**-Tabellen)

Anforderungen, die durch den Tabellenspeicherdienst selbst erfolgen, z. B. Protokollerstellungs- oder -löschvorgänge, werden nicht protokolliert. Eine vollständige Liste der protokollierten Daten finden Sie unter [Protokollierte Storage Analytics-Vorgänge und -Statusmeldungen](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) und [Storage Analytics-Protokollformat](monitor-table-storage-reference.md).

### <a name="log-anonymous-requests"></a>Protokollieren anonymer Anforderungen

 Die folgenden Typen anonymer Anforderungen werden protokolliert:

- Erfolgreiche Anforderungen
- Serverfehler
- Timeoutfehler für Client und Server
- Mit Fehlercode 304 (nicht geändert) fehlgeschlagene GET-Anforderungen

Alle anderen fehlgeschlagenen, anonymen Anforderungen werden nicht protokolliert. Eine vollständige Liste der protokollierten Daten finden Sie unter [Protokollierte Storage Analytics-Vorgänge und -Statusmeldungen](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) und [Storage Analytics-Protokollformat](monitor-table-storage-reference.md).

### <a name="accessing-logs-in-a-storage-account"></a>Zugreifen auf Protokolle in einem Speicherkonto

Protokolle werden als Blobs angezeigt, die im Zielspeicherkonto in einem Container gespeichert sind. Die erfassten Daten werden in einem einzelnen Blob als durch Zeilen getrennte JSON-Nutzlast gespeichert. Für den Namen des Blobs wird diese Benennungskonvention verwendet:

`https://<destination-storage-account>.blob.core.windows.net/insights-logs-<storage-operation>/resourceId=/subscriptions/<subscription-ID>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<source-storage-account>/tableServices/default/y=<year>/m=<month>/d=<day>/h=<hour>/m=<minute>/PT1H.json`

Hier sehen Sie ein Beispiel:

`https://mylogstorageaccount.blob.core.windows.net/insights-logs-storagewrite/resourceId=/subscriptions/`<br>`208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/tableServices/default/y=2019/m=07/d=30/h=23/m=12/PT1H.json`

### <a name="accessing-logs-in-an-event-hub"></a>Zugreifen auf Protokolle in einem Event Hub

An einen Event Hub gesendete Protokolle werden nicht als Datei gespeichert. Sie können jedoch überprüfen, ob der Event Hub die Protokollinformationen empfangen hat. Wechseln Sie im Azure-Portal zu Ihrem Event Hub, und vergewissern Sie sich, dass die Anzahl **eingehender Nachrichten** größer als Null ist. 

![Überwachungsprotokolle](media/monitor-table-storage/event-hub-log.png)

Mithilfe von SIEM- (Security Information & Event Management) und Überwachungstools können Sie auf die an Ihren Event Hub gesendeten Protokolldaten zugreifen und diese lesen. Unter [Wie kann ich die an meinen Event Hub gesendeten Überwachungsdaten nutzen?](https://docs.microsoft.com/azure/azure-monitor/platform/stream-monitoring-data-event-hubs#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub) finden Sie weitere Informationen dazu.

### <a name="accessing-logs-in-a-log-analytics-workspace"></a>Zugreifen auf Protokolle in einem Log Analytics-Arbeitsbereich

Mithilfe von Azure Monitor-Protokollabfragen können Sie auf Protokolle zugreifen, die an einen Log Analytics-Arbeitsbereich gesendet wurden.

Weitere Informationen finden Sie unter [Erste Schritte mit Log Analytics in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal).

Die Daten werden in der **StorageTableLogs**-Tabelle gespeichert. 

#### <a name="sample-kusto-queries"></a>Kusto-Beispielabfragen

Im Folgenden finden Sie einige Abfragen, die Sie in die **Protokollsuchleiste** eingeben können, um die Überwachung Ihres Tabellenspeichers zu vereinfachen. Diese Abfragen arbeiten mit der [neuen Sprache](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

> [!IMPORTANT]
> Wenn Sie **Protokolle** im Speicherkonto-Ressourcengruppenmenü auswählen, wird Log Analytics geöffnet, wobei der Abfragebereich auf die aktuelle Ressourcengruppe festgelegt ist. Dies bedeutet, dass Protokollabfragen nur Daten aus dieser Ressourcengruppe umfassen. Wenn Sie eine Abfrage ausführen möchten, die Daten aus anderen Ressourcen oder Daten aus anderen Azure-Diensten enthält, wählen Sie im Menü **Azure Monitor** die Option **Protokolle** aus. Ausführliche Informationen finden Sie unter [Protokollabfragebereich und Zeitbereich in Azure Monitor Log Analytics](/azure/azure-monitor/log-query/scope/).

Verwenden Sie diese Abfragen, um Ihre Azure Storage-Konten zu überwachen:

* Abfrage zum Auflisten der 10 häufigsten Fehler in den letzten 3 Tagen.

    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d) and StatusText !contains "Success"
    | summarize count() by StatusText
    | top 10 by count_ desc
    ```
* Abfrage zum Auflisten der 10 häufigsten Vorgänge, die in den letzten 3 Tagen die meisten Fehler verursacht haben.

    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d) and StatusText !contains "Success"
    | summarize count() by OperationName
    | top 10 by count_ desc
    ```
* Abfrage zum Auflisten der 10 häufigsten Vorgänge mit der längsten End-to-End-Latenz in den letzten 3 Tagen.

    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d)
    | top 10 by DurationMs desc
    | project TimeGenerated, OperationName, DurationMs, ServerLatencyMs, ClientLatencyMs = DurationMs - ServerLatencyMs
    ```
* Abfrage zum Auflisten aller Vorgänge, die in den letzten 3 Tagen serverseitige Drosselungsfehler verursacht haben.

    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d) and StatusText contains "ServerBusy"
    | project TimeGenerated, OperationName, StatusCode, StatusText
    ```
* Abfrage zum Auflisten aller Anforderungen mit anonymem Zugriff in den letzten 3 Tagen.

    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d) and AuthenticationType == "Anonymous"
    | project TimeGenerated, OperationName, AuthenticationType, Uri
    ```
* Abfrage zum Erstellen eines Kreisdiagramms der in den letzten 3 Tagen verwendeten Vorgänge.
    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d)
    | summarize count() by OperationName
    | sort by count_ desc 
    | render piechart
    ```
## <a name="faq"></a>Häufig gestellte Fragen

**Unterstützt Azure Storage Metriken für verwaltete oder nicht verwaltete Datenträger?**

Nein. Die Metriken für Datenträger werden von Azure Compute unterstützt. Weitere Informationen finden Sie unter [Metriken pro Datenträger für verwaltete und nicht verwaltete Datenträger](https://azure.microsoft.com/blog/per-disk-metrics-managed-disks/).

## <a name="next-steps"></a>Nächste Schritte

- Eine Referenz zu den von Azure-Tabellenspeicher erstellten Protokollen und Metriken finden Sie unter [Überwachungsdatenreferenz zu Azure-Tabellenspeicher](monitor-table-storage-reference.md).
- Ausführliche Informationen zur Überwachung von Azure-Ressourcen finden Sie unter [Überwachen von Azure-Ressourcen mit Azure Monitor](../../azure-monitor/insights/monitor-azure-resource.md).
- Weitere Informationen zur Migration von Metriken finden Sie unter [Migration von Azure Storage-Metriken](../common/storage-metrics-migration.md).
