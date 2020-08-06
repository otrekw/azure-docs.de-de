---
title: Webhookaktionen für Protokollwarnungen in Azure-Warnungen
description: In diesem Artikel wird beschrieben, wie Sie eine Protokollwarnungsregel mit einem Log Analytics-Arbeitsbereich mit Application Insights erstellen, wie die Warnung Daten per Pushvorgang als HTTP-Webhook überträgt sowie die Details der verschiedenen möglichen Anpassungen.
author: yanivlavi
ms.author: yalavi
services: monitoring
ms.topic: conceptual
ms.date: 06/25/2019
ms.subservice: alerts
ms.openlocfilehash: 3311819f021533a28a41daf2c2f08193218fae96
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87075274"
---
# <a name="webhook-actions-for-log-alert-rules"></a>Webhookaktionen für Protokollwarnungsregeln
Wenn eine [Protokollwarnung in Azure erstellt](alerts-log.md) wird, haben Sie die Möglichkeit, [sie mithilfe von Aktionsgruppen so zu konfigurieren](action-groups.md), dass sie eine oder mehrere Aktionen ausführt. Dieser Artikel beschreibt die verschiedenen verfügbaren Webhookaktionen und zeigt, wie Sie einen benutzerdefinierten, JSON-basierten Webhook konfigurieren.

> [!NOTE]
> Sie können auch das [allgemeine Warnungsschema](https://aka.ms/commonAlertSchemaDocs) für Ihre Webhookintegrationen verwenden. Das allgemeine Warnungsschema bietet den Vorteil einer einzelnen erweiterbaren und einheitlichen Warnungsnutzlast für alle Benachrichtigungsdienste in Azure Monitor. Beachten Sie, dass beim allgemeinen Warnungsschema die benutzerdefinierte JSON-Option für Protokollwarnungen nicht berücksichtigt wird. Es wird die Nutzlast des allgemeinen Warnungsschemas übernommen, wenn diese ausgewählt ist, unabhängig von der Anpassung, die Sie möglicherweise auf Ebene der Warnungsregel vorgenommen haben. [Hier finden Sie Informationen zu den Definitionen des allgemeinen Warnungsschemas](https://aka.ms/commonAlertSchemaDefinitions).

## <a name="webhook-actions"></a>Webhookaktionen

Mit Webhookaktionen können Sie einen externen Prozess über eine einzige HTTP POST-Anforderung aufrufen. Der aufgerufene Dienst sollte Webhooks unterstützen, und es sollte festgelegt werden, wie empfangene Nutzlasten verwendet werden.

Webhook-Aktionen erfordern die in der folgenden Tabelle aufgeführten Eigenschaften.

| Eigenschaft | BESCHREIBUNG |
|:--- |:--- |
| **Webhook-URL** |Die URL des Webhooks. |
| **Benutzerdefinierte JSON-Nutzlast** |Die benutzerdefinierte Nutzlast, die mit dem Webhook gesendet werden soll, wenn diese Option während der Warnungserstellung ausgewählt wird. Weitere Informationen finden Sie unter [Verwalten von Protokollwarnungen](alerts-log.md).|

> [!NOTE]
> Die Schaltfläche **Webhook anzeigen** zusammen mit der Option **Benutzerdefinierte JSON-Nutzlast für Webhook einschließen** für die Protokollwarnung zeigt die Beispielnutzlast für den Webhook für die bereitgestellte Anpassung an. Es sind keine tatsächlichen Daten enthalten, aber sie ist repräsentativ für ein JSON-Schema, das für Protokollwarnungen verwendet wird. 

Webhooks enthalten eine URL und eine Nutzlast im JSON-Format, wobei es sich um die an den externen Dienst gesendeten Daten handelt. Die Nutzlast enthält standardmäßig die Werte in der folgenden Tabelle. Sie können diese Nutzlast auch durch eine benutzerdefinierte eigene Nutzlast ersetzen. Verwenden Sie in diesem Fall die Variablen in der Tabelle für die einzelnen Parameter, um deren Wert in die benutzerdefinierte Nutzlast einzubinden.


| Parameter | Variable | BESCHREIBUNG |
|:--- |:--- |:--- |
| *AlertRuleName* |#alertrulename |Der Name der Warnungsregel. |
| *Severity* |#severity |Festgelegter Schweregrad für die erste ausgelöste Protokollwarnung |
| *AlertThresholdOperator* |#thresholdoperator |Schwellenwertoperator für die Warnungsregel, der größer als oder kleiner als verwendet. |
| *AlertThresholdValue* |#thresholdvalue |Wert des Schwellenwerts für die Warnungsregel |
| *LinkToSearchResults* |#linktosearchresults |Link zum Analyseportal, das die Datensätze aus der Abfrage zurückgibt, mit der die Warnung erstellt wurde. |
| *LinkToSearchResultsAPI* |#linktosearchresultsapi |Link zur Analytics-API, die die Datensätze aus der Abfrage zurückgibt, mit der die Warnung erstellt wurde |
| *LinkToFilteredSearchResultsUI* |#linktofilteredsearchresultsui |Link zum Analyticsportal, das die Datensätze aus der nach Dimensionswertkombinationen gefilterten Abfrage zurückgibt, mit der die Warnung erstellt wurde |
| *LinkToFilteredSearchResultsAPI* |#linktofilteredsearchresultsapi |Link zur Analytics-API, die die Datensätze aus der nach Dimensionswertkombinationen gefilterten Abfrage zurückgibt, mit der die Warnung erstellt wurde |
| *ResultCount* |#searchresultcount |Anzahl von Datensätzen in den Suchergebnissen |
| *Endzeit für das Suchintervall* |#searchintervalendtimeutc |Abschlusszeit der Abfrage im UTC-Format, mit dem Format mm/tt/jjjj HH:mm:ss AM/PM. |
| *Suchintervall* |#searchinterval |Zeitfenster für die Warnungsregel im Format HH:mm:ss. |
| *Startzeit für das Suchintervall* |#searchintervalstarttimeutc |Startzeit der Abfrage im UTC-Format, mit dem Format mm/tt/jjjj HH:mm:ss AM/PM. 
| *SearchQuery* |#searchquery |Von der Warnungsregel verwendete Protokollsuchabfrage |
| *SearchResults* |"IncludeSearchResults": true|Hierbei handelt es sich um die von der Abfrage als JSON-Tabelle zurückgegebenen Datensätze, beschränkt auf die ersten 1.000 Datensätze. "IncludeSearchResults": true wird in einer benutzerdefinierten JSON-Webhookdefinition als Eigenschaft der obersten Ebene hinzugefügt. |
| *Dimensionen* |"IncludeDimensions": true|Hierbei handelt es sich um Dimensionswertkombinationen, die diese Warnung ausgelöst haben, als JSON-Abschnitt. "IncludeDimensions": true wird in einer benutzerdefinierten JSON-Webhookdefinition als Eigenschaft der obersten Ebene hinzugefügt. |
| *Warnungstyp*| #alerttype | Der Typ der konfigurierten Protokollwarnungsregel als [metrische Maßeinheit](alerts-unified-log.md#metric-measurement-alert-rules) oder [Anzahl von Ergebnissen](alerts-unified-log.md#number-of-results-alert-rules).|
| *WorkspaceID* |#workspaceid |ID Ihres Log Analytics-Arbeitsbereichs. |
| *Anwendungs-ID* |#applicationid |ID Ihrer Application Insights-App. |
| *Abonnement-ID* |#subscriptionid |ID des von Ihnen verwendeten Azure-Abonnements. 

> [!NOTE]
> Die aufgeführten Links übergeben Parameter wie *SearchQuery*, *Search Interval StartTime* und *Search Interval End time* in der URL an das Azure-Portal oder die API.

Sie können beispielsweise die folgende benutzerdefinierte Nutzlast angeben, die einen einzelnen Parameter wie *text*enthält. Der Dienst, der von diesem Webhook aufgerufen wird, erwartet diesen Parameter.

```json

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }
```
Diese Beispielnutzlast wird ähnlich wie hier dargestellt aufgelöst, wenn sie an den Webhook gesendet wird:

```json
    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }
```
Da alle Variablen in einem benutzerdefinierten Webhook innerhalb einer JSON-Umschließung wie „#searchinterval“ angegeben werden müssen, weist auch der resultierende Webhook Variablendaten in Umschließungen auf, etwa „00:05:00“.

Um Suchergebnisse in eine benutzerdefinierte Nutzlast einzuschließen, muss **IncudeSearchResults** als Eigenschaft der obersten Ebene in der JSON-Nutzlast festgelegt werden. 

## <a name="sample-payloads"></a>Beispielnutzlasten
Dieser Abschnitt zeigt Beispielnutzlasten für Webhooks für Protokollwarnungen. Die Beispielnutzlasten enthalten Beispiele dafür, wenn die Nutzlast Standard ist und wenn sie benutzerdefiniert ist.

### <a name="standard-webhook-for-log-alerts"></a>Standardwebhook für Protokollwarnungen 
Beide Beispiele enthalten eine Pseudonutzlast mit nur zwei Spalten und zwei Zeilen.

#### <a name="log-alert-for-log-analytics"></a>Protokollwarnung für Log Analytics
Die folgende Beispielnutzlast ist für eine Standardwebhookaktion *ohne benutzerdefinierte JSON-Option* gedacht, die für Warnungen verwendet wird, die auf Log Analytics basieren.

```json
{
    "SubscriptionId": "12345a-1234b-123c-123d-12345678e",
    "AlertRuleName": "AcmeRule",
    "SearchQuery": "Perf | where ObjectName == \"Processor\" and CounterName == \"% Processor Time\" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 5m), Computer",
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://portal.azure.com/#Analyticsblade/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "LinkToFilteredSearchResultsUI": "https://portal.azure.com/#Analyticsblade/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "LinkToSearchResultsAPI": "https://api.loganalytics.io/v1/workspaces/workspaceID/query?query=Heartbeat&timespan=2020-05-07T18%3a11%3a51.0000000Z%2f2020-05-07T18%3a16%3a51.0000000Z",
    "LinkToFilteredSearchResultsAPI": "https://api.loganalytics.io/v1/workspaces/workspaceID/query?query=Heartbeat&timespan=2020-05-07T18%3a11%3a51.0000000Z%2f2020-05-07T18%3a16%3a51.0000000Z",
    "Description": "log alert rule",
    "Severity": "Warning",
    "AffectedConfigurationItems": [
        "INC-Gen2Alert"
    ],
    "Dimensions": [
        {
            "name": "Computer",
            "value": "INC-Gen2Alert"
        }
    ],
    "SearchResult": {
        "tables": [
            {
                "name": "PrimaryResult",
                "columns": [
                    {
                        "name": "$table",
                        "type": "string"
                    },
                    {
                        "name": "Computer",
                        "type": "string"
                    },
                    {
                        "name": "TimeGenerated",
                        "type": "datetime"
                    }
                ],
                "rows": [
                    [
                        "Fabrikam",
                        "33446677a",
                        "2018-02-02T15:03:12.18Z"
                    ],
                    [
                        "Contoso",
                        "33445566b",
                        "2018-02-02T15:16:53.932Z"
                    ]
                ]
            }
        ]
    },
    "WorkspaceId": "12345a-1234b-123c-123d-12345678e",
    "AlertType": "Metric measurement"
}
 ```

> [!NOTE]
> Der Wert des Felds „Schweregrad“ könnte sich ändern, wenn Sie [Ihre API-Einstellung für Protokollwarnungen in Log Analytics gewechselt](alerts-log-api-switch.md) haben.


#### <a name="log-alert-for-application-insights"></a>Protokollwarnung für Application Insights
Die folgende Beispielnutzlast ist für einen Standardwebhook *ohne benutzerdefinierte JSON-Option* gedacht, wenn er für Protokollwarnungen verwendet wird, die auf Application Insights basieren.
    
```json
{
    "schemaId": "Microsoft.Insights/LogAlert",
    "data": {
        "SubscriptionId": "12345a-1234b-123c-123d-12345678e",
        "AlertRuleName": "AcmeRule",
        "SearchQuery": "requests | where resultCode == \"500\" | summarize AggregatedValue = Count by bin(Timestamp, 5m), IP",
        "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
        "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
        "AlertThresholdOperator": "Greater Than",
        "AlertThresholdValue": 0,
        "ResultCount": 2,
        "SearchIntervalInSeconds": 3600,
        "LinkToSearchResults": "https://portal.azure.com/AnalyticsBlade/subscriptions/12345a-1234b-123c-123d-12345678e/?query=search+*+&timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
        "LinkToFilteredSearchResultsUI": "https://portal.azure.com/AnalyticsBlade/subscriptions/12345a-1234b-123c-123d-12345678e/?query=search+*+&timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
        "LinkToSearchResultsAPI": "https://api.applicationinsights.io/v1/apps/0MyAppId0/metrics/requests/count",
        "LinkToFilteredSearchResultsAPI": "https://api.applicationinsights.io/v1/apps/0MyAppId0/metrics/requests/count",
        "Description": null,
        "Severity": "3",
        "Dimensions": [
            {
                "name": "IP",
                "value": "1.1.1.1"
            }
        ],
        "SearchResult": {
            "tables": [
                {
                    "name": "PrimaryResult",
                    "columns": [
                        {
                            "name": "$table",
                            "type": "string"
                        },
                        {
                            "name": "Id",
                            "type": "string"
                        },
                        {
                            "name": "Timestamp",
                            "type": "datetime"
                        }
                    ],
                    "rows": [
                        [
                            "Fabrikam",
                            "33446677a",
                            "2018-02-02T15:03:12.18Z"
                        ],
                        [
                            "Contoso",
                            "33445566b",
                            "2018-02-02T15:16:53.932Z"
                        ]
                    ]
                }
            ]
        },
        "ApplicationId": "123123f0-01d3-12ab-123f-abc1ab01c0a1",
        "AlertType": "Metric measurement"
    }
}
```

#### <a name="log-alert-with-custom-json-payload"></a>Protokollwarnung mit benutzerdefinierter JSON-Nutzlast
Sie können beispielsweise Folgendes verwenden, um eine benutzerdefinierte Nutzlast zu erstellen, die nur den Warnungsnamen und die Suchergebnisse enthält: 

```json
    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }
```

Die folgende Beispielnutzlast ist für eine benutzerdefinierte Webhookaktion für eine beliebige Protokollwarnung gedacht:
    
```json
    {
    "alertname":"AcmeRule","IncludeSearchResults":true,
    "SearchResults":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    }
```


## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Protokollwarnungen in Azure-Warnungen](alerts-unified-log.md).
- Grundlegendes zum [Verwalten von Protokollwarnungen in Azure](alerts-log.md).
- Erstellen und Verwalten von [Aktionsgruppen in Azure](action-groups.md).
- Erfahren Sie mehr über [Application Insights](../log-query/log-query-overview.md).
- Weitere Informationen zum [Analysieren von Protokolldaten in Azure Monitor](../log-query/log-query-overview.md). 
