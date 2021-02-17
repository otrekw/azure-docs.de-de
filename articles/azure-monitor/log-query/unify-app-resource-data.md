---
title: Vereinigen mehrerer Azure Monitor-Application Insights-Ressourcen | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zum Verwenden einer Funktion in Azure Monitor-Protokollen zum Abfragen mehrerer Application Insights-Ressourcen und zum Visualisieren dieser Daten.
author: bwren
ms.author: bwren
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: 54a026bfe9dfba121799d850ca0c81485bf63874
ms.sourcegitcommit: 5a999764e98bd71653ad12918c09def7ecd92cf6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2021
ms.locfileid: "100545665"
---
# <a name="unify-multiple-azure-monitor-application-insights-resources"></a>Vereinigen mehrerer Azure Monitor-Application Insights-Ressourcen 
In diesem Artikel wird beschrieben, wie Sie all Ihre Application Insights-Protokolldaten an einem zentralen Ort abfragen und anzeigen, selbst wenn sie aus unterschiedlichen Azure-Abonnements stammen. Damit soll der veraltete Application Insights-Connector ersetzt werden. Die Anzahl der Application Insights-Ressourcen, die Sie in eine einzelne Abfrage einschließen können, ist auf 100 beschränkt.

## <a name="recommended-approach-to-query-multiple-application-insights-resources"></a>Empfohlener Ansatz zum Abfragen mehrerer Application Insights-Ressourcen 
Das Auflisten mehrerer Application Insights-Ressourcen in einer Abfrage kann aufwendig und schwierig zu verwalten sein. Sie können aber stattdessen eine Funktion zum Trennen der Abfragelogik vom Gültigkeitsbereich einer Anwendung nutzen.  

Dieses Beispiel zeigt, wie Sie mehrere Application Insights-Ressourcen überwachen und die Anzahl fehlerhafter Anforderungen anhand des Anwendungsnamens visualisieren können.

Erstellen Sie mithilfe des union-Operators eine Funktion mit der Liste der Anwendungen, und speichern Sie die Abfrage in Ihrem Arbeitsbereich als Funktion mit dem Alias *applicationsScoping*. 

Sie können die aufgelisteten Anwendungen jederzeit im Portal ändern. Navigieren Sie dazu zum Abfrage-Explorer in Ihrem Arbeitsbereich, und bearbeiten und speichern Sie dann die Funktion, oder verwenden Sie das PowerShell-Cmdlet `SavedSearch`. 

>[!NOTE]
>Diese Methode kann bei Protokollwarnungen nicht verwendet werden, weil die Zugriffsüberprüfung der Warnungsregelressourcen, einschließlich Arbeitsbereichen und Anwendungen, zum Zeitpunkt der Erstellung der Warnung ausgeführt wird. Das Hinzufügen neuer Ressourcen zur Funktion nach dem Erstellen der Warnung wird nicht unterstützt. Wenn Sie eine Funktion zur Ressourcenbereichsdefinition in Protokollwarnungen verwenden möchten, müssen Sie die Warnungsregel im Portal bearbeiten oder eine Resource Manager-Vorlage verwenden, um die bereichsbezogenen Ressourcen zu aktualisieren. Alternativ dazu können Sie die Liste der Ressourcen in die Abfrage der Protokollwarnung einbeziehen.

Der Befehl `withsource= SourceApp` fügt den Ergebnissen eine Spalte hinzu, die die Anwendung angibt, von der das Protokoll gesendet wurde. Der parse-Operator in diesem Beispiel ist optional und wird zum Extrahieren des Anwendungsnamens aus der SourceApp-Eigenschaft verwendet. 

```
union withsource=SourceApp 
app('Contoso-app1').requests,  
app('Contoso-app2').requests, 
app('Contoso-app3').requests, 
app('Contoso-app4').requests, 
app('Contoso-app5').requests 
| parse SourceApp with * "('" applicationName "')" *  
```

Sie können nun die applicationsScoping-Funktion in der ressourcenübergreifenden Abfrage verwenden.  

```
applicationsScoping 
| where timestamp > ago(12h)
| where success == 'False'
| parse SourceApp with * '(' applicationName ')' * 
| summarize count() by applicationName, bin(timestamp, 1h) 
| render timechart
```

Die Abfrage verwendet das Application Insights-Schema, obwohl sie im Arbeitsbereich ausgeführt wird, da die applicationsScoping-Funktion die Application Insights-Datenstruktur zurückgibt. Der Funktionsalias gibt die Vereinigungsmenge der Anforderungen aller definierten Anwendungen zurück. Die Abfrage filtert dann fehlgeschlagene Anforderungen heraus und visualisiert die Trends nach Anwendung sortiert.

![Beispiel für abfrageübergreifende Ergebnisse](media/unify-app-resource-data/app-insights-query-results.png)

>[!NOTE]
>[Ressourcenübergreifende Abfragen](./cross-workspace-query.md) in Protokollwarnungen werden nur in der aktuellen [scheduledQueryRules-API](/rest/api/monitor/scheduledqueryrules) unterstützt. Wenn Sie die Legacy-API für Log Analytics-Warnungen verwenden, müssen Sie [zur aktuellen API wechseln](../platform/alerts-log-api-switch.md). [Siehe Beispielvorlagen](../platform/alerts-log-create-templates.md).

## <a name="application-insights-and-log-analytics-workspace-schema-differences"></a>Schemaunterschiede zwischen Application Insights und Log Analytics-Arbeitsbereichen
Die folgende Tabelle zeigt die Schemaunterschiede zwischen Log Analytics und Application Insights.  

| Eigenschaften von Log Analytics-Arbeitsbereichen| Eigenschaften von Application Insights-Ressourcen|
|------------|------------| 
| AnonUserId | user_id|
| ApplicationId | appId|
| ApplicationName | appName|
| ApplicationTypeVersion | application_Version |
| AvailabilityCount | itemCount |
| AvailabilityDuration | duration |
| AvailabilityMessage | message |
| AvailabilityRunLocation | location |
| AvailabilityTestId | id |
| AvailabilityTestName | name |
| AvailabilityTimestamp | timestamp |
| Browser | client_browser |
| City | client_city |
| ClientIP | client_IP |
| Computer | cloud_RoleInstance | 
| Country | client_CountryOrRegion | 
| CustomEventCount | itemCount | 
| CustomEventDimensions | customDimensions |
| CustomEventName | name | 
| DeviceModel | client_Model | 
| DeviceType | client_Type | 
| ExceptionCount | itemCount | 
| ExceptionHandledAt | handledAt |
| ExceptionMessage | message | 
| ExceptionType | type |
| OperationID | operation_id |
| Vorgangsname | operation_Name | 
| OS | client_OS | 
| PageViewCount | itemCount |
| PageViewDuration | duration | 
| PageViewName | name | 
| ParentOperationID | operation_Id | 
| RequestCount | itemCount | 
| RequestDuration | duration | 
| RequestID | id | 
| RequestName | name | 
| RequestSuccess | success | 
| ResponseCode | resultCode | 
| Role | cloud_RoleName |
| RoleInstance | cloud_RoleInstance |
| SessionID | session_Id | 
| SourceSystem | operation_SyntheticSource |
| TelemetryTYpe | type |
| URL | url |
| UserAccountId | user_AccountId |

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die [Protokollsuche](./log-query-overview.md), um ausführliche Informationen zu Ihren Application Insights-Apps anzuzeigen.

