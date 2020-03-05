---
title: Streamen von Protokollen der Azure-Plattform an Event Hubs
description: Erfahren Sie, wie Ressourcenprotokolle an Event Hubs gestreamt werden, um Daten an externe Systeme zu senden, z.B. SIEMs und andere Protokollanalyselösungen von Drittanbietern.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/15/2019
ms.author: bwren
ms.subservice: ''
ms.openlocfilehash: 72341b6da0068ba4b7e3f53b08e6015cafb70f09
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77658913"
---
# <a name="stream-azure-platform-logs-to-azure-event-hubs"></a>Streamen von Protokollen der Azure-Plattform an Azure Event Hubs
[Plattformprotokolle](platform-logs-overview.md) in Azure, wie Azure-Aktivitätsprotokolle und Ressourcenprotokolle, liefern detaillierte Diagnose- und Überwachungsinformationen für Azure-Ressourcen und die Azure-Plattform, von der sie abhängen.  In diesem Artikel wird das Streamen von Plattformprotokollen an Event Hubs beschrieben, um Daten an externe Systeme zu senden, z. B. SIEMs und andere Protokollanalyselösungen von Drittanbietern.


## <a name="what-you-can-do-with-platform-logs-sent-to-an-event-hub"></a>Verwendungsmöglichkeiten für Plattformprotokolle, die an Event Hubs gesendet werden
Streamen Sie Plattformprotokolle in Azure an Event Hubs, um folgende Funktionalität bereitzustellen:

* **Streamen von Protokollen an Protokollierungs- und Telemetriesysteme von Drittanbietern**: Streamen Sie alle Plattformprotokolle an einen einzelnen Event Hub, um Protokolldaten an ein SIEM- oder Protokollanalysetool eines Drittanbieters weiterzureichen.
  
* **Erstellen einer benutzerdefinierten Telemetrie- und Protokollierungsplattform**: Event Hubs ermöglichen dank des hochgradig skalierbaren Veröffentlichen/Abonnieren-Konzepts eine flexible Erfassung der Plattformprotokolle auf einer benutzerdefinierten Telemetrieplattform. Ausführliche Informationen finden Sie unter [Designing and Sizing a Global Scale Telemetry Platform on Azure Event Hubs](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/) (Entwerfen und Dimensionieren einer globalen Telemetrieplattform in Azure Event Hubs).

* **Anzeigen der Dienstintegrität durch Streamen von Daten an Power BI**: Mithilfe von Event Hubs, Stream Analytics und Power BI können Sie sich anhand Ihrer Diagnosedaten nahezu in Echtzeit einen Einblick in Ihre Azure-Dienste verschaffen. Details zu dieser Lösung finden Sie unter [Stream Analytics und Power BI: Ein Dashboard zur Echtzeitanalyse von Streamingdaten](../../stream-analytics/stream-analytics-power-bi-dashboard.md).

    Der folgende SQL-Code ist ein Beispiel für eine Stream Analytics-Abfrage, mit der Sie alle Protokolldaten in einer Power BI-Tabelle analysieren können:
    
    ```sql
    SELECT
    records.ArrayValue.[Properties you want to track]
    INTO
    [OutputSourceName – the Power BI source]
    FROM
    [InputSourceName] AS e
    CROSS APPLY GetArrayElements(e.records) AS records
    ```

## <a name="prerequisites"></a>Voraussetzungen
Falls Sie noch nicht über einen Event Hub verfügen, müssen Sie [einen Event Hub erstellen](../../event-hubs/event-hubs-create.md). Wenn Sie bereits über eine Diagnoseeinstellung mit diesem Event Hubs-Namespace verfügen, wird dieser Event Hub wiederverwendet.

Die SAS-Richtlinie für den Namespace definiert die Berechtigungen für den Streamingmechanismus. Für das Streaming an Event Hubs werden Berechtigungen für das Verwalten, Senden und Lauschen benötigt. Sie können SAS-Richtlinien im Azure-Portal auf der Registerkarte „Konfigurieren“ für Ihren Event Hubs-Namespace erstellen oder ändern.

Wenn Sie der Diagnoseeinstellung das Streamingfeature hinzufügen möchten, müssen Sie in der Event Hubs-Autorisierungsregel über die ListKey-Berechtigung verfügen. Der Event Hubs-Namespace muss sich nicht in demselben Abonnement wie das Abonnement befinden, das Protokolle ausgibt – sofern der Benutzer, der die Einstellung konfiguriert, über den entsprechenden RBAC-Zugriff auf beide Abonnements verfügt und beide Abonnements sich in demselben AAD-Mandanten befinden.

## <a name="create-a-diagnostic-setting"></a>Erstellen einer Diagnoseeinstellung
Senden Sie Plattformprotokolle an Event Hubs und andere Ziele, indem Sie eine Diagnoseeinstellung für eine Azure-Ressource erstellen. Weitere Informationen finden Sie unter [Erstellen einer Diagnoseeinstellung zum Erfassen von Protokollen und Metriken in Azure](diagnostic-settings.md).

## <a name="collect-data-from-compute-resources"></a>Erfassen von Daten von Computeressourcen
Mit den Diagnoseeinstellungen werden Ressourcenprotokolle für Azure-Computeressourcen wie für alle anderen Ressourcen erfasst, jedoch nicht deren Gastbetriebssystem oder die Workloads. Um diese Daten zu erfassen, installieren Sie den [Log Analytics-Agent](log-analytics-agent.md). 


## <a name="consuming-log-data-from-event-hubs"></a>Nutzen von Protokolldaten aus Event Hubs
Plattformprotokolle aus Event Hubs werden im JSON-Format mit den Elementen in der folgenden Tabelle genutzt.

| Elementname | BESCHREIBUNG |
| --- | --- |
| records |Ein Array sämtlicher Protokollereignisse in dieser Nutzlast. |
| time |Der Zeitpunkt, zu dem das Ereignis aufgetreten ist. |
| category |Die Protokollkategorie für dieses Ereignis. |
| resourceId |Die Ressourcen-ID der Ressource, die dieses Ereignis generiert hat. |
| operationName |Name des Vorgangs. |
| level |Optional. Gibt die Protokollereignisebene an. |
| properties |Die Eigenschaften des Ereignisses. Diese sind für jeden Azure-Dienst verschieden, wie es in []() beschrieben ist. |


Es folgt ein Beispiel für eine Datenausgabe aus Event Hubs für ein Ressourcenprotokoll:

```json
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```



## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zu Ressourcenprotokollen](platform-logs-overview.md).
* [Erstellen einer Diagnoseeinstellung zum Erfassen von Protokollen und Metriken in Azure](diagnostic-settings.md)
* [Streamen von Azure Active Directory-Protokollen mit Azure Monitor](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md)
* [Erste Schritte mit Event Hubs](../../event-hubs/event-hubs-dotnet-standard-getstarted-send.md)

