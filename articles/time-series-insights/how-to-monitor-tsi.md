---
title: Überwachen von Time Series Insights | Microsoft-Dokumentation
description: Überwachen Sie mit Time Series Insights Verfügbarkeit, Leistung und Betrieb.
author: deepakpalled
ms.author: lyhughes
manager: diviso
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/10/2020
ms.custom: lyrana
ms.openlocfilehash: 940f7efc7130ef80817be3b42e3c0eff83588a90
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100587990"
---
# <a name="monitoring-time-series-insights"></a>Überwachung mithilfe von Time Series Insights

Wenn Sie über unternehmenskritische Anwendungen und Geschäftsprozesse verfügen, die auf Azure-Ressourcen beruhen, sollten Sie Verfügbarkeit, Leistung und Betrieb dieser Ressourcen überwachen. In diesem Artikel werden die Überwachunsdaten beschrieben, die von Time Series Insights generiert werden. Außerdem wird erläutert, wie Sie die Funktionen von Azure Monitor nutzen, um diese Daten zu analysieren und Warnungen dafür zu erstellen.

## <a name="monitor-overview"></a>Azure Monitor: Übersicht

Die Seite **Übersicht** im Azure-Portal für jede Time Series Insights-Umgebung enthält eine Kurzübersicht über die Ressourcennutzung, z. B. Anzahl der empfangenen Nachrichten und gespeicherten Bytes. Dies sind hilfreiche Informationen, die aber nur einen kleinen Teil der in diesem Bereich verfügbaren Überwachungsdaten ausmachen. Einige dieser Daten werden automatisch erfasst und sind für die Analyse verfügbar, sobald Sie die Ressource erstellen. Mit einigen Konfigurationsschritten können Sie zusätzliche Typen von Datensammlungen aktivieren.

## <a name="what-is-azure-monitor"></a>Was ist Azure Monitor?

Time Series Insights erstellt Überwachungsdaten mithilfe von [Azure Monitor](../azure-monitor/overview.md), einem Dienst zur Überwachung des gesamten Azure-Stapels, der eine vollständige Palette von Features zum Überwachen Ihrer Azure-Ressourcen sowie von Ressourcen in anderen Clouds und lokalen Umgebungen bietet.

Beginnen Sie mit dem Artikel [Überwachen von Azure-Ressourcen mit Azure Monitor](../azure-monitor/essentials/monitor-azure-resource.md), in dem die folgenden Konzepte beschrieben werden:

- Was ist Azure Monitor?
- Kosten für die Überwachung
- In Azure gesammelte Überwachungsdaten
- Konfigurieren der Datensammlung
- Standardtools in Azure zum Analysieren von Überwachungsdaten sowie zum Generieren von Warnungen

Die folgenden Abschnitte bauen dahingehend auf diesem Artikel auf, dass die für Azure Time Series Insights spezifischen Daten beschrieben werden. In diesen Abschnitten finden Sie außerdem Beispiele für die Konfiguration der Datensammlung und die Analyse der Daten mit Azure-Tools.

> [!TIP]
> Informationen zu den mit Azure Monitor verbundenen Kosten finden Sie unter [Überwachen der Nutzung und geschätzten Kosten in Azure Monitor](../azure-monitor//usage-estimated-costs.md). Informationen hinsichtlich der Zeit, die benötigt wird, bis Ihre Daten in Azure Monitor angezeigt werden, finden Sie unter [Protokolldatenerfassungszeit in Azure Monitor](../azure-monitor/logs/data-ingestion-time.md).

## <a name="monitoring-data-from-azure-time-series-insights"></a>Überwachen von aus Azure Time Series Insights stammenden Daten

Azure Time Series Insights sammelt dieselben Arten von Überwachungsdaten wie andere Azure-Ressourcen, die in [Überwachungsdaten von Azure-Ressourcen](../azure-monitor/essentials/monitor-azure-resource.md#monitoring-data) beschrieben werden. 

Eine ausführliche Referenz zu den Protokollen und Metriken finden Sie unter [Referenz zu Time Series Insights-Überwachungsdaten](how-to-monitor-tsi-reference.md).

## <a name="collection-and-routing"></a>Sammlung und Routing

Plattformmetriken werden automatisch gesammelt und gespeichert, können aber mithilfe einer Diagnoseeinstellung an andere Speicherorte umgeleitet werden.

Ressourcenprotokolle werden erst erfasst und gespeichert, sobald Sie eine Diagnoseeinstellung erstellt und an einen oder mehrere Standorte weitergeleitet haben.
Ausführliche Informationen zum Erstellen einer Diagnoseeinstellung über das Azure-Portal, die Befehlszeilenschnittstelle oder PowerShell finden Sie unter [Erstellen einer Diagnoseeinstellung zum Sammeln von Plattformprotokollen und Metriken in Azure](../azure-monitor/essentials/diagnostic-settings.md). Wenn Sie eine Diagnoseeinstellung erstellen, legen Sie fest, welche Kategorien von Protokollen gesammelt werden sollen.

Sie können für Azure Time Series Insights Protokolle der folgenden Kategorien erfassen:

   | Category | BESCHREIBUNG |
   |---|---|
   | Eingehend  | In der Kategorie „Eingehend“ werden Fehler nachverfolgt, die in der Eingangspipeline auftreten. Diese Kategorie umfasst Fehler beim Empfangen von Ereignissen (z. B. Fehler beim Herstellen einer Verbindung mit einer Ereignisquelle) sowie beim Verarbeiten von Ereignissen (z. B. Fehler beim Analysieren von Ereignisnutzdaten). |

## <a name="analyzing-metrics"></a>Analysieren von Metriken

Sie können Metriken für Azure Time Series Insights zusammen mit Metriken anderer Azure-Dienste analysieren, indem Sie im Menü „Azure Monitor“ die Option „Metriken“ öffnen. Ausführliche Informationen zur Verwendung dieses Tools finden Sie unter [Erste Schritte mit dem Azure-Metrik-Explorer](../azure-monitor/platform/metrics-getting-started.md).

Eine Liste der erfassten Plattformmetriken finden Sie in der [Referenz zur Überwachung von Azure Time Series Insights-Daten](how-to-monitor-tsi-reference.md#metrics).

Dieses Beispiel zeigt die Anzahl der Bytes, die von allen Ereignisquellen in Ihrer Azure Time Series Insights-Umgebung empfangen wurden.

**Eingehende empfangene Bytes** [![Azure Time Series: eingehende empfangene Bytes](media/how-to-monitor-tsi/ingress-received-bytes.png)](media/how-to-monitor-tsi/ingress-received-bytes.png#lightbox)

Das Beispiel zeigt die Anzahl der Bytes, die erfolgreich verarbeitet und in ihrer Azure Time Series Insights-Umgebung für Abfragen verfügbar sind.

**Eingehende gespeicherte Bytes** [![Azure Time Series: eingehende gespeicherte Bytes](media/how-to-monitor-tsi/ingress-stored-bytes.png)](media/how-to-monitor-tsi/ingress-stored-bytes.png#lightbox)

## <a name="analyzing-logs"></a>Analysieren von Protokollen
Sie können auf Ressourcenprotokolle entweder als Blob in einem Speicherkonto, als Ereignisdaten oder über Log Analytics-Abfragen zugreifen.

Daten in Azure Monitor-Protokollen werden in Tabellen gespeichert, wobei jede Tabelle ihren eigenen Satz eindeutiger Eigenschaften hat.

Alle Ressourcenprotokolle in Azure Monitor enthalten dieselben Felder, gefolgt von dienstspezifischen Feldern. Das allgemeine Schema wird in [Azure Monitor-Ressourcenprotokollschema](../azure-monitor/essentials/resource-logs-schema.md#top-level-common-schema) beschrieben. Eine Liste der Arten von Ressourcenprotokollen, die für Azure Time Series Insights gesammelt werden, finden Sie in der [Referenz zur Überwachung von Azure Time Series Insights-Daten](how-to-monitor-tsi-reference.md#resource-logs).

Azure Time Series Insights speichert Daten in den folgenden Tabellen.

| Tabelle | BESCHREIBUNG |
|:---|:---|
| TSIIngress | Die Tabelle, in der Daten der Kategorie „Eingehend“ gespeichert werden. In der Kategorie „Eingehend“ werden Fehler nachverfolgt, die in der Eingangspipeline auftreten. Diese Kategorie umfasst Fehler beim Empfangen von Ereignissen (z. B. Fehler beim Herstellen einer Verbindung mit einer Ereignisquelle) sowie beim Verarbeiten von Ereignissen (z. B. Fehler beim Analysieren von Ereignisnutzdaten).

Zum Weiterleiten von Daten an Azure Monitor-Protokolle müssen Sie eine Diagnoseeinstellung erstellen, um Ressourcenprotokolle oder Plattformmetriken an einen Log Analytics-Arbeitsbereich zu senden. Weitere Informationen finden Sie unter [Sammlung und Routing](../iot-hub/monitor-iot-hub.md#collection-and-routing).

## <a name="sample-queries"></a>Beispielabfragen

Es folgen Abfragen zur Überwachung Ihrer Azure Time Series Insights-Umgebung:

+ Abrufen von Details zu Fehlern bei der Verbindung mit Ereignisquellen in den letzten fünf Tagen:

    ```Kusto
   TSIIngress
   | where OperationName == "Microsoft.TimeSeriesInsights/environments/eventsources/ingress/connect"
   | where _ResourceId contains "<your environment name, event source name, or the full event source resource URL>"
   | where TimeGenerated > ago(5d)

    ```
+ Abrufen von Details zu ungültigen Nachrichten, die in den letzten fünf Tagen empfangen wurden:

    ```Kusto
   TSIIngress
   | where OperationName == "Microsoft.TimeSeriesInsights/environments/eventsources/ingress/deserialize"
   | where _ResourceId contains "<your environment name, event source name, or the full event source resource URL>"
   | where TimeGenerated > ago(5d)

    ```

## <a name="alerts"></a>Alerts

Azure Monitor-Warnungen informieren Sie proaktiv, wenn wichtige Bedingungen in Ihren Überwachungsdaten gefunden werden. Sie ermöglichen Ihnen, Probleme in Ihrem System zu identifizieren und zu beheben, bevor Ihre Kunden sie bemerken. Sie können Warnungen für [Metriken](../azure-monitor/platform/alerts-metric-overview.md), [Protokolle](../azure-monitor/platform/alerts-unified-log.md) und das [Aktivitätsprotokoll](../azure-monitor/platform/activity-log-alerts.md) festlegen. Verschiedene Arten von Warnungen haben Vor- und Nachteile.

Beachten Sie Folgendes beim Erstellen einer Warnungsregel, die auf Plattformmetriken basiert: Bei Time Series Insights-Plattformmetriken, die als Anzahl von Einheiten gesammelt werden, sind einige Aggregationen möglicherweise nicht verfügbar oder nutzbar.

## <a name="next-steps"></a>Nächste Schritte

* Eine Referenz zu den von Azure Time Series Insights erstellten Protokollen und Metriken finden Sie in der [Referenz zur Überwachung von Azure Time Series Insights-Daten](how-to-monitor-tsi-reference.md).
* Ausführliche Informationen zur Überwachung von Azure-Ressourcen finden Sie unter [Überwachen von Azure-Ressourcen mit Azure Monitor](../azure-monitor/essentials/monitor-azure-resource.md).