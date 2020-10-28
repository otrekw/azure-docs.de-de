---
title: Überwachen der Integrität des Log Analytics-Arbeitsbereichs in Azure Monitor
description: Hier wird beschrieben, wie Sie die Integrität Ihres Log Analytics-Arbeitsbereichs mithilfe von Daten in der Tabelle „Operation“ überwachen.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/20/2020
ms.openlocfilehash: a4f578ca2e9fc448fb85b803cce46974a8c2e4dc
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2020
ms.locfileid: "92325939"
---
# <a name="monitor-health-of-log-analytics-workspace-in-azure-monitor"></a>Überwachen der Integrität des Log Analytics-Arbeitsbereichs in Azure Monitor
Um die Leistung und Verfügbarkeit Ihres Log Analytics-Arbeitsbereichs in Azure Monitor zu gewährleisten, müssen Sie in der Lage sein, jedes auftretende Probleme proaktiv zu erkennen. In diesem Artikel wird beschrieben, wie Sie die Integrität Ihres Log Analytics-Arbeitsbereichs mithilfe von Daten in der Tabelle [Operation](/azure-monitor/reference/tables/operation) überwachen. Diese Tabelle ist in jedem Log Analytics-Arbeitsbereich enthalten und enthält Fehler und Warnungen, die im Arbeitsbereich auftreten. Sie sollten diese Daten regelmäßig überprüfen und Warnungen erstellen, damit Sie proaktiv benachrichtigt werden, wenn im Arbeitsbereich wichtige Vorfälle auftreten.

## <a name="_logsoperation-function"></a>Funktion „_LogsOperation“
Azure Monitor-Protokolle senden Details zu einem Problem an die Tabelle [Operation](/azure-monitor/reference/tables/operation) in dem Arbeitsbereich, in dem das Problem aufgetreten ist. Die Systemfunktion **_LogsOperation** basiert auf der Tabelle **Operation** und bietet einen vereinfachten Satz von Informationen für die Analyse und Warnungen.

## <a name="columns"></a>Spalten

Die Funktion **_LogsOperation** gibt die Spalten in der folgenden Tabelle zurück.

| Column | BESCHREIBUNG |
|:---|:---|
| TimeGenerated | Der Zeitpunkt in UTC, zu dem der Vorfall aufgetreten ist. |
| Kategorie  | Die Vorgangskategoriegruppe. Hiermit kann nach Vorgangstypen gefiltert werden und ermöglicht das Erstellen genauerer Systemüberwachungen und Warnungen. Eine Liste der Kategorien finden Sie im Abschnitt unten. |
| Vorgang  | Eine Beschreibung des Vorgangstyps. Dies kann auf einen der Log Analytics-Grenzwerte, den Typ des Vorgangs oder einen Teil eines Prozesses hinweisen. |
| Ebene | Der Schweregrad des Problems:<br>– Info: Es ist keine besondere Aufmerksamkeit erforderlich.<br>– Warnung: Der Prozess wurde nicht erwartungsgemäß abgeschlossen, und Aufmerksamkeit ist erforderlich.<br>– Fehler: Beim Prozess ist ein Fehler aufgetreten, und Aufmerksamkeit ist umgehend erforderlich. 
| Detail | Eine ausführliche Beschreibung des Vorgangs, die eine spezifische Fehlermeldung (falls vorhanden) enthält. |
| _ResourceId | Die Ressourcen-ID der Azure-Ressource, die mit dem Vorgang im Zusammenhang steht.  |
| Computer | Der Computername, wenn der Vorgang mit einem Azure Monitor-Agent im Zusammenhang steht. |
| CorrelationId | Hiermit werden aufeinander folgende verwandte Vorgänge gruppiert. |


## <a name="categories"></a>Kategorien
In der folgenden Tabelle werden die Kategorien der Funktion „_LogsOperations“ beschrieben. 

| Kategorie | BESCHREIBUNG |
|:---|:---|
| Erfassung           | Vorgänge, die Teil des Datenerfassungsprozesses sind. Weitere Informationen finden Sie unten. |
| Agent               | Weist auf ein Problem mit der Agentinstallation hin. |
| Datensammlung     | Vorgänge im Zusammenhang mit Datensammlungsprozessen. |
| Zielgruppenadressierung  | Vorgang des Typs *ConfigurationScope* wurde verarbeitet. |
| Bewertungslösung | Ein Bewertungsprozess wurde ausgeführt. |


### <a name="ingestion"></a>Erfassung
Erfassungsvorgänge sind Probleme, die während der Datenerfassung aufgetreten sind, einschließlich Benachrichtigungen über das Erreichen der Grenzwerte für den Azure Log Analytics-Arbeitsbereich. Fehlerbedingungen in dieser Kategorie können auf Datenverluste hindeuten und müssen daher unbedingt überwacht werden. In der folgenden Tabelle finden Sie Details zu diesen Vorgängen. Informationen zu Diensteinschränkungen für Log Analytics-Arbeitsbereiche finden Sie unter [Azure Monitor-Diensteinschränkungen](../service-limits.md#log-analytics-workspaces).


| Vorgang | Ebene | Detail | Verwandter Artikel |
|:---|:---|:---|:---|
| Benutzerdefiniertes Protokoll | Fehler   | Das Spaltenlimit für benutzerdefinierte Felder wurde erreicht. | [Azure Monitor-Diensteinschränkungen](../service-limits.md#log-analytics-workspaces) |
| Benutzerdefiniertes Protokoll | Fehler   | Fehler beim Erfassen von benutzerdefinierten Protokollen. | |
| Benutzerdefiniertes Protokoll | Fehler   | Metadaten | |
| Daten | Fehler   | Daten wurden verworfen, da die Anforderung vor der festgelegten Anzahl von Tagen erstellt wurde. | [Verwalten von Nutzung und Kosten mit Azure Monitor-Protokollen](manage-cost-storage.md#alert-when-daily-cap-reached)
| Datensammlung | Info    | Die Konfiguration des Sammlungscomputers wurde erkannt.| |
| Datensammlung | Info    | Die Datensammlung wurde aufgrund eines neuen Tags gestartet. | [Verwalten von Nutzung und Kosten mit Azure Monitor-Protokollen](/manage-cost-storage.md#alert-when-daily-cap-reached) |
| Datensammlung | Warnung | Die Datensammlung wurde beendet, da das Tageslimit erreicht wurde.| [Verwalten von Nutzung und Kosten mit Azure Monitor-Protokollen](/manage-cost-storage.md#alert-when-daily-cap-reached) |
| Erfassungsrate | Info | Der Grenzwert für die Erfassungsrate nähert sich 70 %. | [Azure Monitor-Diensteinschränkungen](../service-limits.md#log-analytics-workspaces) |
| Erfassungsrate | Warnung | Der Grenzwert für die Erfassungsrate nähert sich dem Limit. | [Azure Monitor-Diensteinschränkungen](../service-limits.md#log-analytics-workspaces) |
| Erfassungsrate | Fehler   | Das Ratenlimit wurde erreicht. | [Azure Monitor-Diensteinschränkungen](../service-limits.md#log-analytics-workspaces) |
| JSON-Analyse | Fehler   | Ungültiges JSON-Format. | [Senden von Protokolldaten an Azure Monitor mit der HTTP-Datensammler-API (Public Preview)](data-collector-api.md#request-body) | 
| JSON-Analyse | Warnung | Der Wert wurde auf die maximal zulässige Größe gekürzt. | [Azure Monitor-Diensteinschränkungen](../service-limits.md#log-analytics-workspaces) |
| Limit für maximale Spaltengröße | Warnung | Der Feldwert wurde abgeschnitten, da das Größenlimit erreicht wurde. | [Azure Monitor-Diensteinschränkungen](../service-limits.md#log-analytics-workspaces) | 
| Storage | Fehler   | Es kann nicht auf das Speicherkonto zugegriffen werden, da die verwendeten Anmeldeinformationen ungültig sind.  |
| Tabelle   | Fehler   | Das Limit für maximale benutzerdefinierte Felder wurde erreicht. | [Azure Monitor-Diensteinschränkungen](../service-limits.md#log-analytics-workspaces)|


   

## <a name="alert-rules"></a>Warnregeln
Verwenden Sie [Protokollabfragewarnungen](../platform/alerts-log-query.md) in Azure Monitor, um proaktiv benachrichtigt zu werden, wenn ein Problem in Ihrem Log Analytics-Arbeitsbereich erkannt wird. Sie sollten eine Strategie verwenden, durch die Sie rechtzeitig auf Probleme reagieren können und gleichzeitig die Kosten reduziert werden. Ihrem Abonnement werden für jede Warnungsregel Kosten in Rechnung gestellt, die von der Häufigkeit der Auswertung abhängen.

Als Strategie wird empfohlen, mit zwei Warnungsregeln basierend auf dem Schweregrad des Problems zu beginnen. Verwenden Sie eine größere Häufigkeit (z. B. alle 5 Minuten) für Fehler und eine geringere Häufigkeit (z. B. alle 24 Stunden) für Warnungen. Da Fehler auf potenzielle Datenverluste hindeuten, sollten Sie schnell darauf reagieren, um Verluste zu minimieren. Warnungen weisen in der Regel auf ein Problem hin, das keine sofortige Aufmerksamkeit erfordert, sodass Sie diese täglich überprüfen können.

Zum Erstellen der Protokollwarnungsregeln folgenden Sie der Vorgehensweise unter [Erstellen, Anzeigen und Verwalten von Protokollwarnungen mithilfe von Azure Monitor](../platform/alerts-log.md). In den folgenden Abschnitten werden die Details für jede Regel beschrieben.


| Abfrage | Schwellenwert | Zeitraum | Häufigkeit |
|:---|:---|:---|:---|
| `_LogsOperation | where Level == "Error"`   | 0 | 5 | 5 |
| `_LogsOperation | where Level == "Warning"` | 0 | 1440 | 1440 |

Mit diesen Warnungsregeln wird bei allen Vorgängen auf gleiche Weise mit einem Fehler oder einer Warnung reagiert. Sobald Sie mit den Vorgängen, die Warnungen generieren, besser vertraut sind, können Sie auf bestimmte Vorgänge anders reagieren. Beispielsweise können Sie Benachrichtigungen für bestimmte Vorgänge an verschiedene Personen senden. 

Um eine Warnungsregel für einen bestimmten Vorgang zu erstellen, verwenden Sie eine Abfrage, die die Spalten **Category** und **Operation** enthält. 

Im folgenden Beispiel wird eine Warnung erstellt, wenn die Rate für das Erfassungsvolumen 80 % des Limits erreicht hat.

```kusto
_LogsOperation
| where Category == "Ingestion"
| where Operation == "Ingestion rate"
| where Level == "Warning"
```

Im folgenden Beispiel wird eine Warnung erstellt, wenn die Datensammlung das Tageslimit erreicht hat. 
```kusto
Operation 
| where OperationCategory == "Ingestion" 
|where OperationKey == "Data Collection" 
| where OperationStatus == "Warning"
```




## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Protokollwarnungen](alerts-log.md).
- [Sammeln Sie Abfrageüberwachungsdaten](../log-query/query-audit.md) für Ihren Arbeitsbereich.