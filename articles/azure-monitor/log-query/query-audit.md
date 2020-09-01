---
title: Überwachen von Abfragen in Azure Monitor-Protokollabfragen
description: Details von Überwachungsprotokollen für Protokollabfragen, die Telemetrie zu den in Azure Monitor ausgeführten Protokollabfragen liefern.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/25/2020
ms.openlocfilehash: cb38dcba2f61a432decb56164b816688ad3192d8
ms.sourcegitcommit: c6b9a46404120ae44c9f3468df14403bcd6686c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88893625"
---
# <a name="audit-queries-in-azure-monitor-logs-preview"></a>Überwachen von Abfragen in Azure Monitor-Protokollen (Vorschau)
Überwachungsprotokolle für Protokollabfragen bieten Telemetrie zu den in Azure Monitor ausgeführten Protokollabfragen. Dazu gehören Informationen dazu, wann z. B. eine Abfrage ausgeführt wurde, wer sie ausgeführt hat, welches Tool verwendet wurde, der Abfragetext und die Leistungsstatistiken, die die Ausführung der Abfrage beschreiben.

## <a name="current-limitations"></a>Aktuelle Einschränkungen
Während der Public Preview-Phase gelten die folgenden Einschränkungen:

- Es werden nur arbeitsbereichbezogene Abfragen protokolliert. Abfragen, die im ressourcenbezogenen Modus ausgeführt werden oder in Bezug auf eine Applikation Insights-Instanz ausgeführt werden, die nicht als arbeitsbereichsbasiert konfiguriert ist, werden nicht protokolliert.


## <a name="configure-query-auditing"></a>Konfigurieren der Abfrageüberwachung
Die Abfrageüberwachung wird mit einer [Diagnoseeinstellung](../platform/diagnostic-settings.md) im Log Analytics-Arbeitsbereich aktiviert. Damit können Sie Überwachungsdaten an den aktuellen Arbeitsbereich oder jeden anderen Arbeitsbereich in Ihrem Abonnement, an Azure Event Hubs zum Versenden außerhalb von Azure oder an Azure Storage zur Archivierung senden. 

### <a name="azure-portal"></a>Azure-Portal
Greifen Sie auf die Diagnoseeinstellung für einen Log Analytics-Arbeitsbereich im Azure-Portal an einem der folgenden Orte zu:

- Wählen Sie im Menü **Azure Monitor** die Option **Diagnoseeinstellungen** aus, und suchen Sie dann den Arbeitsbereich und wählen Sie ihn aus.

    [![Diagnoseeinstellungen in Azure Monitor](media/log-query-audit/diagnostic-setting-monitor.png) ](media/log-query-audit/diagnostic-setting-monitor.png#lightbox) 

- Wählen Sie im Menü **Log Analytics-Arbeitsbereiche** den Arbeitsbereich und dann **Diagnoseeinstellungen** aus.

    [![Diagnoseeinstellungen im Log Analytics-Arbeitsbereich](media/log-query-audit/diagnostic-setting-workspace.png) ](media/log-query-audit/diagnostic-setting-workspace.png#lightbox) 

### <a name="resource-manager-template"></a>Resource Manager-Vorlage
Eine Resource Manager-Beispielvorlage erhalten Sie unter [Diagnoseeinstellung für den Log Analytics-Arbeitsbereich](../samples/resource-manager-diagnostic-settings.md#diagnostic-setting-for-log-analytics-workspace).

## <a name="audit-data"></a>Überwachungsdaten
Bei jeder Ausführung einer Abfrage wird ein Überwachungsdatensatz erstellt. Wenn Sie die Daten an einen Log Analytics-Arbeitsbereich senden, werden sie in einer Tabelle namens *LAQueryLogs* gespeichert. Die folgende Tabelle beschreibt die Eigenschaften in jedem Datensatz der Überwachungsdaten.

| Feld | BESCHREIBUNG |
|:---|:---|
| TimeGenerated         | Die UTC-Zeit, zu der die Abfrage übermittelt wurde. |
| CorrelationId         | Die eindeutige ID zur Bezeichnung der Abfrage. Kann in Problembehandlungsszenarien verwendet werden, wenn Microsoft um Unterstützung gebeten wird. |
| AADObjectId           | Die Azure Active Directory-ID des Benutzerkontos, das die Abfrage gestartet hat.  |
| AADTenantId           | Die ID des Mandanten des Benutzerkontos, das die Abfrage gestartet hat.  |
| AADEmail              | Die E-Mail-Adresse des Mandanten des Benutzerkontos, das die Abfrage gestartet hat.  |
| AADClientId           | Die ID und der aufgelöste Name der Anwendung, mit der die Abfrage gestartet wurde. |
| RequestClientApp      | Der aufgelöste Name der Anwendung, mit der die Abfrage gestartet wurde. |
| QueryTimeRangeStart   | Gibt den Beginn des für die Abfrage ausgewählten Zeitraums an. In bestimmten Szenarien, wenn z. B. die Abfrage von Log Analytics aus gestartet wird und der Zeitraum innerhalb der Abfrage und nicht in der Zeitauswahl angegeben ist, wird dieser möglicherweise nicht ausgefüllt. |
| QueryTimeRangeEnd     | Gibt das Ende des für die Abfrage ausgewählten Zeitraums an. In bestimmten Szenarien, wenn z. B. die Abfrage von Log Analytics aus gestartet wird und der Zeitraum innerhalb der Abfrage und nicht in der Zeitauswahl angegeben ist, wird dieser möglicherweise nicht ausgefüllt.  |
| QueryText             | Der Text der Abfrage, die ausgeführt wurde. |
| RequestTarget         | Die API-URL wurde zur Übermittlung der Abfrage verwendet.  |
| RequestContext        | Liste der Ressourcen, für die die Abfrage angefordert wurde. Enthält bis zu drei Zeichenfolgenarrays: Arbeitsbereiche, Anwendungen und Ressourcen. Abfragen, die auf Abonnements oder Ressourcengruppen ausgerichtet sind, werden als *Ressourcen* angezeigt. Umfasst das von RequestTarget implizierte Ziel. |
| RequestContextFilters | Ein Satz von Filtern, die als Teil des Abfrageaufrufs angegeben werden. Umfasst bis zu drei mögliche Zeichenfolgenarrays:<br>- ResourceTypes – Typ der Ressource zum Einzuschränken des Umfangs der Abfrage.<br>- Workspaces – Liste der Arbeitsbereiche, auf die die Abfrage beschränkt werden soll.<br>- WorkspaceRegions – Liste der Arbeitsbereichregionen zum Einschränken der Abfrage. |
| ResponseCode          | HTTP-Antwortcode, der beim Absenden der Abfrage zurückgegeben wurde. |
| ResponseDurationMs    | Der Zeitpunkt für die Rückgabe der Antwort.  |
| StatsCPUTimeMs       | Die gesamte für Computing, Analyse und Abrufen von Daten verwendete Computezeit. Wird nur ausgefüllt, wenn die Abfrage mit Statuscode 200 zurückgegeben wird. |
| StatsDataProcessedKB | Die Menge der Daten, auf die zur Verarbeitung der Abfrage zugegriffen wurde. Beeinflusst durch die Größe der Zieltabelle, die verwendete Zeitspanne, die angewendeten Filter und die Anzahl der referenzierten Spalten. Wird nur ausgefüllt, wenn die Abfrage mit Statuscode 200 zurückgegeben wird. |
| StatsDataProcessedStart | Die Uhrzeit der ältesten Daten, auf die zur Verarbeitung der Abfrage zugegriffen wurde. Durch die Abfrage beeinflusste explizite Zeitspanne und angewandte Filter. Diese kann aufgrund der Datenpartitionierung größer sein als die explizite Zeitspanne. Wird nur ausgefüllt, wenn die Abfrage mit Statuscode 200 zurückgegeben wird. |
| StatsDataProcessedEnd  |Die Uhrzeit der neuesten Daten, auf die zur Verarbeitung der Abfrage zugegriffen wurde. Durch die Abfrage beeinflusste explizite Zeitspanne und angewandte Filter. Diese kann aufgrund der Datenpartitionierung größer sein als die explizite Zeitspanne. Wird nur ausgefüllt, wenn die Abfrage mit Statuscode 200 zurückgegeben wird. |
| StatsWorkspaceCount | Die Anzahl der Arbeitsbereiche, auf die die Abfrage zugreift. Wird nur ausgefüllt, wenn die Abfrage mit Statuscode 200 zurückgegeben wird. |
| StatsRegionCount | Die Anzahl der Regionen, auf die von der Abfrage zugegriffen wird. Wird nur ausgefüllt, wenn die Abfrage mit Statuscode 200 zurückgegeben wird. |



## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Diagnoseeinstellungen](../platform/diagnostic-settings.md).
- Erfahren Sie mehr über das [Optimieren von Protokollabfragen](query-optimization.md).