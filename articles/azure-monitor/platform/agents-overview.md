---
title: Übersicht über die Azure-Überwachungs-Agents | Microsoft-Dokumentation
description: Dieser Artikel bietet eine ausführliche Übersicht über die verfügbaren Azure-Agents, die die Überwachung virtueller Computer unterstützen, die in Azure oder einer Hybridumgebung gehostet werden.
services: azure-monitor
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/15/2019
ms.openlocfilehash: a01258799efa81c8d3ddba398facaa90c24c2513
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2019
ms.locfileid: "74150025"
---
# <a name="overview-of-the-azure-monitor-agents"></a>Übersicht über die Azure Monitor-Agents 
Computeressourcen (z.B. virtuelle Computer) generieren Daten zum Überwachen ihrer Leistung und Verfügbarkeit genau wie [andere Cloudressourcen](../insights/monitor-azure-resource.md). Computeressourcen verfügen aber auch über ein Gastbetriebssystem und Workloads, die überwacht werden müssen. Zum Erfassen dieser Überwachungsdaten aus der Ressource ist ein Agent erforderlich. In diesem Artikel werden die von Azure Monitor verwendeten Agents beschrieben. Außerdem bietet er Hilfe bei der Entscheidung, welche Agents Sie zum Erfüllen der Anforderungen für Ihre bestimmte Umgebung benötigen.

## <a name="summary-of-agents"></a>Übersicht über die Agents

> [!NOTE]
> Aufgrund der kürzlich vorgenommenen Konsolidierung von Azure Monitor und Log Analytics verfügt Azure Monitor über mehrere Agents. Beide Agents haben einige Funktionen gemeinsam, während andere Funktionen für einen bestimmten Agent einzigartig sind. Je nach Anforderungen benötigen Sie möglicherweise einen der Agents oder beide. 

Azure Monitor verfügt über drei Agents, die jeweils bestimmte Funktionen bereitstellen. Je nach Anforderungen können Sie einen einzelnen oder mehrere Agents auf Ihren virtuellen Computern und anderen Computeressourcen installieren.

* [Azure-Diagnoseerweiterung](#azure-diagnostic-extension)
* [Log Analytics-Agent](#log-analytics-agent)
* [Dependency-Agent](#dependency-agent)

Die folgende Tabelle zeigt einen schnellen Vergleich der verschiedenen Agents. Ausführliche Informationen zu den einzelnen Agents finden Sie weiter unten in diesem Artikel.

| | Azure-Diagnoseerweiterung | Log Analytics-Agent | Abhängigkeits-Agent |
|:---|:---|:---|:---|
| Unterstützte Umgebungen | Azure | Azure<br>Andere Cloud<br>Lokal | Azure<br>Andere Cloud<br>Lokal |
| Betriebssysteme | Windows<br>Linux | Windows<br>Linux | Windows<br>Linux
| Agent-Abhängigkeiten  | Keine | Keine | Erfordert Log Analytics-Agent |
| Gesammelte Daten | Ereignisprotokolle<br>ETW-Ereignisse<br>syslog<br>Leistung<br>IIS-Protokolle<br>Ausgabeprotokolle zur Ablaufverfolgung von .NET-Apps<br>Absturzabbilder | Ereignisprotokolle<br>syslog<br>Leistung<br>IIS-Protokolle<br>Benutzerdefinierte Protokolle<br>Daten aus Lösungen | Prozessdetails und Abhängigkeiten<br>Netzwerkverbindungsmetriken |
| Senden von Daten an | Azure Storage<br>Azure Monitor-Metriken<br>Event Hub | Azure Monitor-Protokolle | Azure Monitor-Protokolle |



## <a name="azure-diagnostic-extension"></a>Azure-Diagnoseerweiterung
Mit der [Azure-Diagnoseerweiterung](../../azure-monitor/platform/diagnostics-extension-overview.md) erfasst Überwachungsdaten vom Gastbetriebssystem und Workloads von Azure-Computeressourcen. Daten werden primär in Azure Storage gesammelt. Sie können Azure Monitor so konfigurieren, dass die Daten aus Storage in einen Log Analytics-Arbeitsbereich kopiert werden. Sie können auch Gastleistungsdaten in Azure Monitor-Metriken erfassen.

Die Azure-Diagnoseerweiterung wird häufig als Windows Azure-Diagnoseerweiterung (WAD) oder Linux Azure-Diagnoseerweiterung (LAD) bezeichnet.


### <a name="scenarios-supported"></a>Unterstützte Szenarien

Folgende Szenarien werden von der Azure-Diagnoseerweiterung unterstützt:

* Sammeln von Protokollen und Leistungsdaten der Azure-Computeressourcen.
* Archivieren von Protokollen und Leistungsdaten des Gastbetriebssystems in Azure Storage.
* Anzeigen von Überwachungsdaten in Storage mithilfe eines Tools wie [Azure Storage-Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md).
* Sammeln von Leistungsdaten in einer Metrikdatenbank, um die von [Azure Monitor-Metriken](data-platform-metrics.md) unterstützten Funktionen zu nutzen, z.B. [Metrikwarnungen](../../azure-monitor/platform/alerts-metric-overview.md) nahezu in Echtzeit und [automatische Skalierung](autoscale-overview.md). 
* Sammeln von Überwachungsdaten aus [Storage in einem Log Analytics-Arbeitsbereich](azure-storage-iis-table.md), um die von [Azure Monitor-Protokollen](data-platform-logs.md#what-can-you-do-with-azure-monitor-logs) unterstützten Funktionen zu nutzen, z.B. [Protokollabfragen](../log-query/log-query-overview.md).
* Senden von Überwachungsdaten an Drittanbietertools mithilfe von [Azure Event Hubs](diagnostics-extension-stream-event-hubs.md).
* Untersuchen von VM-Startfehlern mit der [Startdiagnose](../../virtual-machines/troubleshooting/boot-diagnostics.md).
* Kopieren von Daten aus Anwendungen, die auf Ihrer VM ausgeführt werden, [in Application Insights](diagnostics-extension-to-application-insights.md) zur Integration in andere Anwendungsüberwachungen.

## <a name="log-analytics-agent"></a>Log Analytics-Agent
Der [Log Analytics-Agent](log-analytics-agent.md) erfasst Überwachungsdaten vom Gastbetriebssystem und Workloads von virtuellen Computern in Azure, anderen Cloudanbietern und lokal. Die Daten werden in einem Log Analytics-Arbeitsbereich gesammelt.

Der Log Analytics-Agent ist derselbe Agent, der von System Center Operations Manager verwendet wird, und Sie verwenden ein Multihoming von Agent-Computern, um mit Ihrer Verwaltungsgruppe und Azure Monitor gleichzeitig zu kommunizieren. Dieser Agent ist auch für bestimmte Lösungen in Azure Monitor erforderlich.

Der Log Analytics-Agent für Windows wird häufig als Microsoft-Verwaltungs-Agent (Microsoft Management Agent, MMA) bezeichnet. Der Log Analytics-Agent für Linux wird häufig als OMS-Agent bezeichnet.


### <a name="scenarios-supported"></a>Unterstützte Szenarien

Folgende Szenarien werden vom Log Analytics-Agent unterstützt:

* Sammeln von Protokollen und Leistungsdaten der virtuellen Computer in Azure, anderer Cloudanbietern und lokal. 
* Sammeln von Überwachungsdaten in einem Log Analytics-Arbeitsbereich, um die von [Azure Monitor-Protokollen](data-platform-logs.md#what-can-you-do-with-azure-monitor-logs) unterstützten Funktionen zu nutzen, z.B. [Protokollabfragen](../log-query/log-query-overview.md).
* Verwenden von Azure Monitor-Überwachungslösungen wie z.B. [Azure Monitor für VMs](../insights/vminsights-overview.md), [Azure Monitor für Container](../insights/container-insights-overview.md) usw.  
* Verwalten der Sicherheit Ihrer virtuellen Computer mit [Azure Sentinel](../../sentinel/overview.md), für das der Log Analytics-Agent erforderlich ist.
* Verwalten der Sicherheit Ihrer virtuellen Computer mit [Azure Security Center](../../security-center/security-center-intro.md), für das der Log Analytics-Agent erforderlich ist.
* Verwenden von Funktionen in [Azure Automation](../../automation/automation-intro.md), um eine umfassende Verwaltung der Azure-VMs über ihren Lebenszyklus zu ermöglichen.  Nachfolgend sind Beispiele für diese Funktionen aufgeführt, die den Log Analytics-Agent erfordern:
  * [Azure Automation-Updateverwaltung](../../automation/automation-update-management.md) von Betriebssystemupdates.
  * [Azure Automation State Configuration](../../automation/automation-dsc-overview.md) zur Beibehaltung eines konsistenten Konfigurationsstatus.
  * Nachverfolgen von Konfigurationsänderungen mit [Azure Automation-Änderungsnachverfolgung und Bestand](../../automation/change-tracking.md).

## <a name="dependency-agent"></a>Abhängigkeits-Agent
Der Dependency-Agent sammelt ermittelte Daten über Prozesse, die auf dem virtuellen Computer ausgeführt werden, und über Abhängigkeiten von externen Prozessen. Dieser Agent ist für [Service Map](../insights/service-map.md) und die Zuordnungsfunktion von [Azure Monitor für VMs](../insights/vminsights-overview.md) erforderlich. Der Dependency-Agent benötigt den Log Analytics-Agent und schreibt Daten in einen Log Analytics-Arbeitsbereich in Azure Monitor.


## <a name="using-multiple-agents"></a>Verwenden mehrerer Agents
Möglicherweise gibt es spezielle Anforderungen, um entweder die Azure-Diagnoseerweiterung oder den Log Analytics-Agent für einen bestimmten virtuellen Computer zu verwenden. Sie möchten beispielsweise Metrikwarnungen verwenden, wofür die Azure-Diagnoseerweiterung erforderlich ist. Möglicherweise möchten Sie aber auch die Zuordnungsfunktion von Azure Monitor für VMs nutzen, für die der Dependency-Agent und der Log Analytics-Agent erforderlich sind. In diesem Fall können Sie mehrere Agents verwenden. Dies ist ein gängiges Szenario für Kunden, die Funktionen beider Agents benötigen.

### <a name="considerations"></a>Überlegungen

- Für den Dependency-Agent muss der Log Analytics-Agent auf demselben virtuellen Computer installiert sein.
- Bei Linux-VMs muss der Log Analytics-Agent vor der Azure-Diagnoseerweiterung installiert werden.


## <a name="next-steps"></a>Nächste Schritte

- In der [Übersicht zum Log Analytics-Agent](../../azure-monitor/platform/log-analytics-agent.md) finden Sie die Anforderungen und unterstützten Methoden zum Bereitstellen des Agents auf Computern, die in Azure, in Ihrem Rechenzentrum oder in einer anderen Cloudumgebung gehostet werden.

