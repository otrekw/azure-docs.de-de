---
title: Überwachung von Azure Event Hubs
description: Erfahren Sie, wie Sie Azure Monitor verwenden können, um Metriken aus Azure Event Hubs anzuzeigen und zu erstellen sowie Warnungen zu solchen Metriken zu erstellen.
ms.topic: conceptual
ms.custom: subject-monitoring
ms.date: 06/13/2021
ms.openlocfilehash: 1a7ea209b3aaf47095723f8d2831fff4a94ffe54
ms.sourcegitcommit: 23040f695dd0785409ab964613fabca1645cef90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112063642"
---
# <a name="monitor-azure-event-hubs"></a>Überwachen von Azure Event Hubs
Wenn Sie über unternehmenskritische Anwendungen und Geschäftsprozesse verfügen, die auf Azure-Ressourcen beruhen, sollten Sie Verfügbarkeit, Leistung und Betrieb dieser Ressourcen überwachen. In diesem Artikel wird das Überwachen von Daten beschrieben, die von Azure Event Hubs generiert werden. Außerdem erfahren Sie, wie Sie diese Daten mit Azure Monitor analysieren und Warnungen für sie erstellen.

## <a name="what-is-azure-monitor"></a>Was ist Azure Monitor?
Azure Event Hubs protokolliert Überwachungsdaten mit [Azure Monitor](../azure-monitor/overview.md), einem Azure-Dienst zur vollständigen Stapelüberwachung. Azure Monitor bietet einen vollständigen Satz von Funktionen zum Überwachen Ihrer Azure-Ressourcen. Mit Azure Monitor können außerdem Ressourcen in anderen Clouds und lokal überwacht werden.

Beginnen Sie mit dem Artikel [Überwachen von Azure-Ressourcen mit Azure Monitor](../azure-monitor/essentials/monitor-azure-resource.md), in dem die folgenden Konzepte beschrieben werden:

- Was ist Azure Monitor?
- Kosten für die Überwachung
- In Azure gesammelte Überwachungsdaten
- Konfigurieren der Datensammlung
- Standardtools in Azure zum Analysieren von Überwachungsdaten sowie zum Generieren von Warnungen

Die folgenden Abschnitte bauen auf diesem Artikel auf, indem die spezifischen Daten beschrieben werden, die für Azure Event Hubs erfasst werden. In diesen Abschnitten finden Sie außerdem Beispiele für die Konfiguration der Datensammlung und die Analyse der Daten mit Azure-Tools.

> [!TIP]
> Informationen zu den mit Azure Monitor verbundenen Kosten finden Sie unter [Überwachen der Nutzung und geschätzten Kosten in Azure Monitor](../azure-monitor//usage-estimated-costs.md). Informationen hinsichtlich der Zeit, die benötigt wird, bis Ihre Daten in Azure Monitor angezeigt werden, finden Sie unter [Protokolldatenerfassungszeit in Azure Monitor](../azure-monitor/logs/data-ingestion-time.md).

## <a name="monitoring-data-from-azure-event-hubs"></a>Überwachen von Daten aus Azure Event Hubs
Azure Event Hubs sammelt dieselben Arten von Überwachungsdaten wie andere Azure-Ressourcen, die in [Überwachungsdaten von Azure-Ressourcen](../azure-monitor/essentials/monitor-azure-resource.md#monitoring-data) beschrieben werden. 

Eine ausführliche Referenz zu den Protokollen und Metriken, die von Azure Event Hubs erstellt werden, finden Sie in der [Datenreferenz zur Überwachung von Azure Event Hubs](monitor-event-hubs-reference.md).

## <a name="collection-and-routing"></a>Sammlung und Routing
Plattformmetriken und das Aktivitätsprotokoll werden erfasst und gespeichert, können aber mithilfe einer Diagnoseeinstellung an andere Speicherorte geleitet werden.  

Ressourcenprotokolle werden erst dann erfasst und gespeichert, nachdem Sie eine Diagnoseeinstellung erstellt und die Protokolle an mindestens einen Speicherort geleitet haben.

Ausführliche Informationen zum Erstellen einer Diagnoseeinstellung über das Azure-Portal, die Befehlszeilenschnittstelle oder PowerShell finden Sie unter [Erstellen einer Diagnoseeinstellung zum Sammeln von Plattformprotokollen und Metriken in Azure](../azure-monitor/essentials/diagnostic-settings.md). Wenn Sie eine Diagnoseeinstellung erstellen, legen Sie fest, welche Kategorien von Protokollen gesammelt werden sollen. Die Kategorien für Azure Event Hubs sind unter [Ressourcenprotokolle](monitor-event-hubs-reference.md#resource-logs) aufgelistet.

Wenn Sie **Azure Storage** zum Speichern der Diagnoseprotokollinformationen verwenden, werden die Informationen in Containern namens **insights-logs-operationlogs** und **insights-metrics-pt1m** gespeichert. Beispiel-URL für ein Vorgangsprotokoll: `https://<Azure Storage account>.blob.core.windows.net/insights-logs-operationallogs/resourceId=/SUBSCRIPTIONS/<Azure subscription ID>/RESOURCEGROUPS/<Resource group name>/PROVIDERS/MICROSOFT.SERVICEBUS/NAMESPACES/<Namespace name>/y=<YEAR>/m=<MONTH-NUMBER>/d=<DAY-NUMBER>/h=<HOUR>/m=<MINUTE>/PT1H.json`. Die URL für ein Metrikprotokoll ist ähnlich. 

Wenn Sie **Azure Event Hubs** zum Speichern der Diagnoseprotokollinformationen verwenden, werden die Informationen in Event Hubs namens **insights-logs-operationlogs** und **insights-metrics-pt1m** gespeichert. Sie können auch Ihren eigenen Event Hub auswählen. 

Wenn Sie **Log Analytics** zum Speichern der Diagnoseprotokollinformationen verwenden, werden die Informationen in Tabellen namens **AzureDiagnostics** und **AzureMetrics** gespeichert. 

> [!IMPORTANT]
> Ein Aktivieren dieser Einstellungen erfordert zusätzliche Azure-Dienste (Speicherkonto, Event Hub oder Log Analytics). Dadurch können sich Ihre Kosten erhöhen. Um geschätzte Kosten zu berechnen, wechseln Sie zum [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator).

> [!NOTE]
> Wenn Sie Metriken in einer Diagnoseeinstellung aktivieren, sind Dimensionsinformationen derzeit nicht in den Informationen enthalten, die an ein Speicherkonto, an einen Event Hub oder an Log Analytics gesendet werden.

In den folgenden Abschnitten werden die Metriken und Protokolle behandelt, die Sie erfassen können.

## <a name="analyzing-metrics"></a>Analysieren von Metriken
Sie können Metriken für Azure Event Hubs zusammen mit Metriken aus anderen Azure-Diensten analysieren, indem Sie auf der Startseite für Ihren Event Hubs-Namespace **Metriken** im Abschnitt **Azure Monitor** auswählen. Ausführliche Informationen zur Verwendung dieses Tools finden Sie unter [Erste Schritte mit dem Azure-Metrik-Explorer](../azure-monitor/essentials/metrics-getting-started.md). Eine Liste der erfassten Plattformmetriken finden Sie unter [Metriken](monitor-event-hubs-reference.md#metrics).

![Metrik-Explorer mit ausgewähltem Event Hubs-Namespace](./media/monitor-event-hubs/metrics.png)

Sie können zur Referenz auf eine Liste [aller in Azure Monitor unterstützter Ressourcenmetriken](../azure-monitor/essentials/metrics-supported.md) anzeigen.

> [!TIP]
> Metrikdaten stehen in Azure Monitor 90 Tage zur Verfügung. Beim Erstellen von Diagrammen können jedoch nur 30 Tage visualisiert werden. Wenn Sie z. B. einen 90-tägigen Zeitraum visualisieren möchten, müssen Sie ihn in drei Diagramme mit jeweils 30 Tagen in diesem 90-Tage-Zeitraum aufteilen.

### <a name="filtering-and-splitting"></a>Filtern und Teilen
Für Metriken, die Dimensionen unterstützen, können Sie Filter mit einem Dimensionswert anwenden. Fügen Sie beispielsweise einen Filter hinzu, bei dem für `EntityName` der Name eines Event Hubs festgelegt ist. Sie können eine Metrik auch nach Dimension teilen, um visuell darzustellen, wie verschiedene Segmente der Metrik miteinander zu vergleichen sind. Weitere Informationen zum Filtern und Teilen finden Sie unter [Erweiterte Funktionen von Azure Metrik-Explorer](../azure-monitor/essentials/metrics-charts.md).

:::image type="content" source="./media/monitor-event-hubs/metrics-filter-split.png" alt-text="Abbildung: Filtern und Teilen von Metriken":::

## <a name="analyzing-logs"></a>Analysieren von Protokollen
Um Azure Monitor Log Analytics verwenden zu können, müssen Sie eine Diagnosekonfiguration erstellen und __Send information to Log Analytics__ (Informationen an Log Analytics senden) aktivieren. Weitere Informationen finden Sie im Abschnitt [Erfassung und Weiterleitung](#collection-and-routing). Daten in Azure Monitor-Protokollen werden in Tabellen gespeichert, wobei jede Tabelle ihren eigenen Satz eindeutiger Eigenschaften hat. Azure Event Hubs speichert Daten in den folgenden Tabellen: **AzureDiagnostics** und **AzureMetrics**.

> [!IMPORTANT]
> Wenn Sie **Protokolle** im Menü von Azure Event Hubs auswählen, wird Log Analytics geöffnet, wobei der Abfragebereich auf den aktuellen Arbeitsbereich festgelegt ist. Dies bedeutet, dass Protokollabfragen nur Daten aus dieser Ressource umfassen. Wenn Sie eine Abfrage ausführen möchten, die Daten aus anderen Datenbanken oder Daten aus anderen Azure-Diensten enthält, wählen Sie im Menü **Azure Monitor** die Option **Protokolle** aus. Ausführliche Informationen finden Sie unter [Protokollabfragebereich und Zeitbereich in Azure Monitor Log Analytics](../azure-monitor/logs/scope.md).

Eine ausführliche Referenz zu den Protokollen und Metriken finden Sie in der [Datenreferenz zur Überwachung von Azure Event Hubs](monitor-event-hubs-reference.md).

### <a name="sample-kusto-queries"></a>Kusto-Beispielabfragen

> [!IMPORTANT]
> Wenn Sie **Protokolle** im Menü von Azure Event Hubs auswählen, wird Log Analytics geöffnet, wobei der Abfragebereich auf den aktuellen Azure Event Hubs-Namespace festgelegt ist. Dies bedeutet, dass Protokollabfragen nur Daten aus dieser Ressource umfassen. Wenn Sie eine Abfrage ausführen möchten, die Daten aus anderen Arbeitsbereichen oder anderen Azure-Diensten enthält, klicken Sie im Menü **Azure Monitor** auf **Protokolle**. Ausführliche Informationen finden Sie unter [Protokollabfragebereich und Zeitbereich in Azure Monitor Log Analytics](../azure-monitor/logs/scope.md).

Die folgenden Beispielabfragen können Sie zur Überwachung Ihrer Azure Event Hubs-Ressourcen verwenden: 

+ Fehler aus den letzten 7 Tagen abrufen

    ```Kusto
    AzureDiagnostics
    | where TimeGenerated > ago(7d)
    | where ResourceProvider =="MICROSOFT.EVENTHUB"
    | where Category == "OperationalLogs"
    | summarize count() by "EventName"
    ```

+ Versuche des Zugriffs auf einen Schlüsseltresor abrufen, die zum Fehler „Schlüssel nicht gefunden“ führten.

    ```Kusto
    AzureDiagnostics
    | where ResourceProvider == "MICROSOFT.EVENTHUB" 
    | where Category == "Error" and OperationName == "wrapkey"
    | project Message
    ```

+ Vorgänge abrufen, die mit einem Schlüsseltresor ausgeführt wurden, um den Schlüssel zu deaktivieren oder wiederherzustellen.

    ```Kusto
    AzureDiagnostics
    | where ResourceProvider == "MICROSOFT.EVENTHUB"
    | where Category == "info" and OperationName == "disable" or OperationName == "restore"
    | project Message
    ```
+ Nicht gelungene Erfassungen und deren Dauer in Sekunden abrufen

    ```kusto
    AzureDiagnostics
    | where ResourceProvider == "MICROSOFT.EVENTHUB"
    | where Category == "ArchiveLogs"
    | summarize count() by "failures", "durationInSeconds"    
    ```
    
## <a name="alerts"></a>Alerts
Sie können auf Warnungen für Azure Event Hubs zugreifen, indem Sie auf der Startseite für Ihren Event Hubs-Namespace **Warnungen** im Abschnitt **Azure Monitor** auswählen. Ausführliche Informationen zum Erstellen von Warnungen finden Sie unter [Erstellen, Anzeigen und Verwalten von Metrikwarnungen mit Azure Monitor](../azure-monitor/alerts/alerts-metric.md).


## <a name="next-steps"></a>Nächste Schritte

- Eine Referenz zu den Protokollen und Metriken finden Sie in der [Datenreferenz zur Überwachung von Azure Event Hubs](monitor-event-hubs-reference.md).
- Ausführliche Informationen zur Überwachung von Azure-Ressourcen finden Sie unter [Überwachen von Azure-Ressourcen mit Azure Monitor](../azure-monitor/essentials/monitor-azure-resource.md).
