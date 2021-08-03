---
title: Überwachen von Azure Service Bus
description: Erfahren Sie, wie Sie Azure Monitor verwenden können, um Warnungen zu Metriken aus Azure Service Bus anzuzeigen, zu analysieren und zu erstellen.
ms.topic: conceptual
ms.custom: subject-monitoring
ms.date: 05/18/2021
ms.openlocfilehash: 90930a5f76fd1aaccd7968febdf7dbdbd684ab28
ms.sourcegitcommit: a9f131fb59ac8dc2f7b5774de7aae9279d960d74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110192069"
---
# <a name="monitor-azure-service-bus"></a>Überwachen von Azure Service Bus
Wenn Sie über unternehmenskritische Anwendungen und Geschäftsprozesse verfügen, die auf Azure-Ressourcen beruhen, sollten Sie Verfügbarkeit, Leistung und Betrieb dieser Ressourcen überwachen. In diesem Artikel wird das Überwachen von Daten beschrieben, die von Azure Service Bus generiert werden. Außerdem erfahren Sie, wie Sie diese Daten mit Azure Monitor analysieren und Warnungen für sie erstellen.

## <a name="what-is-azure-monitor"></a>Was ist Azure Monitor?
Azure Service Bus protokolliert Überwachungsdaten mit [Azure Monitor](../azure-monitor/overview.md), einem Azure-Dienst zur vollständigen Stapelüberwachung. Azure Monitor bietet einen vollständigen Satz von Funktionen zum Überwachen Ihrer Azure-Ressourcen. Mit Azure Monitor können außerdem Ressourcen in anderen Clouds und lokal überwacht werden.

Beginnen Sie mit dem Artikel [Überwachen von Azure-Ressourcen mit Azure Monitor](../azure-monitor/essentials/monitor-azure-resource.md), in dem die folgenden Konzepte beschrieben werden:

- Was ist Azure Monitor?
- Kosten für die Überwachung
- In Azure gesammelte Überwachungsdaten
- Konfigurieren der Datensammlung
- Standardtools in Azure zum Analysieren von Überwachungsdaten sowie zum Generieren von Warnungen

Die folgenden Abschnitte bauen auf diesem Artikel auf, indem die spezifischen Daten beschrieben werden, die für Azure Service Bus erfasst werden. In diesen Abschnitten finden Sie außerdem Beispiele für die Konfiguration der Datensammlung und die Analyse der Daten mit Azure-Tools.

> [!TIP]
> Informationen zu den mit Azure Monitor verbundenen Kosten finden Sie unter [Überwachen der Nutzung und geschätzten Kosten in Azure Monitor](../azure-monitor//usage-estimated-costs.md). Informationen hinsichtlich der Zeit, die benötigt wird, bis Ihre Daten in Azure Monitor angezeigt werden, finden Sie unter [Protokolldatenerfassungszeit in Azure Monitor](../azure-monitor/logs/data-ingestion-time.md).

## <a name="monitoring-data-from-azure-service-bus"></a>Überwachen von Daten von Azure Service Bus
Azure Service Bus erfasst dieselben Arten von Überwachungsdaten wie andere Azure-Ressourcen, die unter [Überwachen von Daten von Azure-Ressourcen](../azure-monitor/essentials/monitor-azure-resource.md#monitoring-data) beschrieben werden. 

Eine ausführliche Referenz zu den Protokollen und Metriken, die von Azure Service Bus erstellt werden, finden Sie unter [Referenz zur Azure Service Bus-Überwachung von Daten](monitor-service-bus-reference.md).

## <a name="collection-and-routing"></a>Sammlung und Routing
Plattformmetriken und das Aktivitätsprotokoll werden erfasst und gespeichert, können aber mithilfe einer Diagnoseeinstellung an andere Speicherorte weitergeleitet werden.  

Ressourcenprotokolle werden erst erfasst und gespeichert, nachdem Sie eine Diagnoseeinstellung erstellt und die Protokolle an mindestens einen Speicherort weitergeleitet haben.

Ausführliche Informationen zum Erstellen einer Diagnoseeinstellung über das Azure-Portal, die Befehlszeilenschnittstelle oder PowerShell finden Sie unter [Erstellen einer Diagnoseeinstellung zum Sammeln von Plattformprotokollen und Metriken in Azure](../azure-monitor/essentials/diagnostic-settings.md). Wenn Sie eine Diagnoseeinstellung erstellen, legen Sie fest, welche Kategorien von Protokollen gesammelt werden sollen. Die Kategorien für Azure Service Bus werden in unter [Referenz zur Azure Service Bus-Überwachung von Daten](monitor-service-bus-reference.md#resource-logs) aufgeführt.

### <a name="azure-storage"></a>Azure Storage 
Die Diagnoseprotokollinformationen werden in Containern namens **insights-logs-operationlogs** und **insights-metrics-pt1m** gespeichert.

Beispiel-URL für ein Vorgangsprotokoll: `https://<Azure Storage account>.blob.core.windows.net/insights-logs-operationallogs/resourceId=/SUBSCRIPTIONS/<Azure subscription ID>/RESOURCEGROUPS/<Resource group name>/PROVIDERS/MICROSOFT.SERVICEBUS/NAMESPACES/<Namespace name>/y=<YEAR>/m=<MONTH-NUMBER>/d=<DAY-NUMBER>/h=<HOUR>/m=<MINUTE>/PT1H.json`. Die URL für ein Metrikprotokoll ist ähnlich. 

### <a name="azure-event-hubs"></a>Azure Event Hubs
Die Diagnoseprotokollierungsinformationen werden in Event Hubs namens **insights-logs-operationlogs** und **insights-metrics-pt1m** gespeichert. Sie können auch Ihren eigenen Event Hub auswählen. 

### <a name="log-analytics"></a>Log Analytics 
Die Diagnoseprotokollierungsinformationen werden in Tabellen namens **AzureDiagnostics** und **AzureMetrics** gespeichert. 

### <a name="sample-operational-log-output-formatted"></a>Beispielausgabe des Betriebsprotokolls (formatiert)

```json
{
    "Environment": "PROD",
    "Region": "East US",
    "ScaleUnit": "PROD-BL2-002",
    "ActivityId": "a097a88a-33e5-4c9c-9c64-20f506ec1375",
    "EventName": "Retrieve Namespace",
    "resourceId": "/SUBSCRIPTIONS/<Azure subscription ID>/RESOURCEGROUPS/SPSBUS0213RG/PROVIDERS/MICROSOFT.SERVICEBUS/NAMESPACES/SPSBUS0213NS",
    "SubscriptionId": "<Azure subscription ID>",
    "EventTimeString": "5/18/2021 3:25:55 AM +00:00",
    "EventProperties": "{\"SubscriptionId\":\"<Azure subscription ID>\",\"Namespace\":\"spsbus0213ns\",\"Via\":\"https://spsbus0213ns.servicebus.windows.net/$Resources/topics?api-version=2017-04&$skip=0&$top=100\",\"TrackingId\":\"a097a88a-33e5-4c9c-9c64-20f506ec1375_M8CH3_M8CH3_G8\"}",
    "Status": "Succeeded",
    "Caller": "rpfrontdoor",
    "category": "OperationalLogs"
}
```

### <a name="sample-metric-log-output-formatted"></a>Beispielausgabe des Metrikprotokolls (formatiert)

```json
{
    "count": 1,
    "total": 4,
    "minimum": 4,
    "maximum": 4,
    "average": 4,
    "resourceId": "/SUBSCRIPTIONS/<Azure subscription ID>/RESOURCEGROUPS/SPSBUS0213RG/PROVIDERS/MICROSOFT.SERVICEBUS/NAMESPACES/SPSBUS0213NS",
    "time": "2021-05-18T03:27:00.0000000Z",
    "metricName": "IncomingMessages",
    "timeGrain": "PT1M"
}
```

> [!IMPORTANT]
> Ein Aktivieren dieser Einstellungen erfordert zusätzliche Azure-Dienste (Speicherkonto, Event Hub oder Log Analytics). Dadurch können sich Ihre Kosten erhöhen. Um geschätzte Kosten zu berechnen, wechseln Sie zum [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator).

> [!NOTE]
> Wenn Sie Metriken in einer Diagnoseeinstellung aktivieren, sind Dimensionsinformationen derzeit nicht in den Informationen enthalten, die an ein Speicherkonto, an einen Event Hub oder an Log Analytics gesendet werden.

In den folgenden Abschnitten werden die Metriken und Protokolle behandelt, die Sie erfassen können.

## <a name="analyzing-metrics"></a>Analysieren von Metriken
Sie können Metriken für Azure Service Bus zusammen mit Metriken aus anderen Azure-Diensten analysieren, indem Sie auf der Startseite für Ihren Service Bus-Namespace **Metriken** im Abschnitt **Azure Monitor** auswählen. Ausführliche Informationen zur Verwendung dieses Tools finden Sie unter [Erste Schritte mit dem Azure-Metrik-Explorer](../azure-monitor/essentials/metrics-getting-started.md). Eine Liste der erfassten Plattformmetriken finden Sie unter [Referenz zur Azure Service Bus-Überwachung von Daten](monitor-service-bus-reference.md#metrics).

![Metrik-Explorer mit ausgewähltem Service Bus-Namespace](./media/monitor-service-bus/metrics.png)

Sie können zur Referenz auf eine Liste [aller in Azure Monitor unterstützter Ressourcenmetriken](../azure-monitor/essentials/metrics-supported.md) anzeigen.

> [!TIP]
> Metrikdaten stehen in Azure Monitor 90 Tage zur Verfügung. Beim Erstellen von Diagrammen können jedoch nur 30 Tage visualisiert werden. Wenn Sie z. B. einen 90-tägigen Zeitraum visualisieren möchten, müssen Sie ihn in drei Diagramme mit jeweils 30 Tagen in diesem 90-Tage-Zeitraum aufteilen.

### <a name="filtering-and-splitting"></a>Filtern und Teilen
Für Metriken, die Dimensionen unterstützen, können Sie Filter mit einem Dimensionswert anwenden. Fügen Sie beispielsweise einen Filter hinzu, bei dem `EntityName` auf den Namen einer Warteschlange oder eines Themas festgelegt ist. Sie können eine Metrik auch nach Dimension teilen, um visuell darzustellen, wie verschiedene Segmente der Metrik miteinander zu vergleichen sind. Weitere Informationen zum Filtern und Teilen finden Sie unter [Erweiterte Funktionen von Azure Metrik-Explorer](../azure-monitor/essentials/metrics-charts.md).

## <a name="analyzing-logs"></a>Analysieren von Protokollen
Um Azure Monitor Log Analytics verwenden zu können, müssen Sie eine Diagnosekonfiguration erstellen und __Send information to Log Analytics__ (Informationen an Log Analytics senden) aktivieren. Weitere Informationen finden Sie im Abschnitt [Erfassung und Weiterleitung](#collection-and-routing). Daten in Azure Monitor-Protokollen werden in Tabellen gespeichert, wobei jede Tabelle ihren eigenen Satz eindeutiger Eigenschaften hat. Azure Service Bus speichert Daten in den folgenden Tabellen: **AzureDiagnostics** und **AzureMetrics**.

> [!IMPORTANT]
> Wenn Sie **Protokolle** im Menü von Azure Service Bus auswählen, wird Log Analytics geöffnet, wobei der Abfragebereich auf den aktuellen Arbeitsbereich festgelegt ist. Dies bedeutet, dass Protokollabfragen nur Daten aus dieser Ressource umfassen. Wenn Sie eine Abfrage ausführen möchten, die Daten aus anderen Datenbanken oder Daten aus anderen Azure-Diensten enthält, wählen Sie im Menü **Azure Monitor** die Option **Protokolle** aus. Ausführliche Informationen finden Sie unter [Protokollabfragebereich und Zeitbereich in Azure Monitor Log Analytics](../azure-monitor/logs/scope.md).

Eine ausführliche Referenz zu den Protokollen und Metriken finden Sie unter [Referenz zur Azure Service Bus-Überwachung von Daten](monitor-service-bus-reference.md).

### <a name="sample-kusto-queries"></a>Kusto-Beispielabfragen

> [!IMPORTANT]
> Wenn Sie **Protokolle** im Menü von Azure Service Bus auswählen, wird Log Analytics geöffnet, wobei der Abfragebereich auf den aktuellen Azure Service Bus-Namespace festgelegt ist. Dies bedeutet, dass Protokollabfragen nur Daten aus dieser Ressource umfassen. Wenn Sie eine Abfrage ausführen möchten, die Daten aus anderen Arbeitsbereichen oder anderen Azure-Diensten enthält, klicken Sie im Menü **Azure Monitor** auf **Protokolle**. Ausführliche Informationen finden Sie unter [Protokollabfragebereich und Zeitbereich in Azure Monitor Log Analytics](../azure-monitor/logs/scope.md).

Die folgenden Beispielabfragen können Sie zur Überwachung Ihrer Azure Service Bus-Ressourcen verwenden: 

+ Abrufen der Verwaltungsvorgänge während der letzten 7 Tage. 

    ```Kusto
    AzureDiagnostics
    | where TimeGenerated > ago(7d)
    | where ResourceProvider =="MICROSOFT.SERVICEBUS"
    | where Category == "OperationalLogs"
    | summarize count() by EventName_s, _ResourceId
    ```

+ Abrufen von Zugriffsversuchen auf einen Schlüsseltresor, die zum Fehler „Schlüssel nicht gefunden“ geführt haben.

    ```Kusto
    AzureDiagnostics
    | where ResourceProvider == "MICROSOFT.SERVICEBUS" 
    | where Category == "Error" and OperationName == "wrapkey"
    | project Message, _ResourceId
    ```

+ Abrufen von Fehlern aus den letzten 7 Tagen.

    ```Kusto
    AzureDiagnostics
    | where TimeGenerated > ago(7d)
    | where ResourceProvider =="MICROSOFT.SERVICEBUS"
    | where Category == "Error" 
    | summarize count() by EventName_s, _ResourceId
    ```

+ Abrufen von Vorgängen, die mit einem Schlüsseltresor ausgeführt wurden, um den Schlüssel zu deaktivieren oder wiederherzustellen.

    ```Kusto
    AzureDiagnostics
    | where ResourceProvider == "MICROSOFT.SERVICEBUS"
    | where (Category == "info" and (OperationName == "disable" or OperationName == "restore"))
    | project Message, _ResourceId
    ```

+ Abrufen aller Entitäten, die automatisch gelöscht wurden.

    ```kusto
    AzureDiagnostics
    | where ResourceProvider == "MICROSOFT.SERVICEBUS"
    | where Category == "OperationalLogs"
    | where EventName_s startswith "AutoDelete"
    | summarize count() by EventName_s, _ResourceId    
    ```
    
## <a name="alerts"></a>Alerts
Sie können auf Warnungen für Azure Service Bus zugreifen, indem Sie auf der Startseite für Ihren Service Bus-Namespace **Warnungen** im Abschnitt **Azure Monitor** auswählen. Ausführliche Informationen zum Erstellen von Warnungen finden Sie unter [Erstellen, Anzeigen und Verwalten von Metrikwarnungen mit Azure Monitor](../azure-monitor/alerts/alerts-metric.md).


## <a name="next-steps"></a>Nächste Schritte

- Eine Referenz zu den Protokollen und Metriken finden Sie in der [Referenz zur Azure Service Bus-Überwachung von Daten](monitor-service-bus-reference.md).
- Ausführliche Informationen zur Überwachung von Azure-Ressourcen finden Sie unter [Überwachen von Azure-Ressourcen mit Azure Monitor](../azure-monitor/essentials/monitor-azure-resource.md).
