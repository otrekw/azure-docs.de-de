---
title: Übersicht über die Azure-Überwachungs-Agents | Microsoft-Dokumentation
description: Dieser Artikel bietet eine ausführliche Übersicht über die verfügbaren Azure-Agents, die die Überwachung virtueller Computer unterstützen, die in Azure oder einer Hybridumgebung gehostet werden.
services: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/14/2020
ms.openlocfilehash: 75394768cc41d3ca73398c2691b9ec0feb8c17f8
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86202565"
---
# <a name="overview-of-azure-monitor-agents"></a>Übersicht über Azure Monitor-Agents

Virtuelle Computer und andere Computeressourcen benötigen einen Agent, um Überwachungsdaten für die Messung der Leistung und Verfügbarkeit ihres Gastbetriebssystems und ihrer Workloads zu erfassen. In diesem Artikel werden die von Azure Monitor verwendeten Agents beschrieben. Außerdem bietet er Hilfe bei der Entscheidung, welche Agents Sie zum Erfüllen der Anforderungen für Ihre bestimmte Umgebung benötigen.

> [!NOTE]
> Aufgrund der kürzlich vorgenommenen Konsolidierung von Azure Monitor und Log Analytics verfügt Azure Monitor über mehrere Agents. Die Features überschneiden sich zwar unter Umständen, jeder hat jedoch seine eigenen Funktionen. Abhängig von Ihren Anforderungen benötigen Sie auf Ihren virtuellen Computern möglicherweise einen oder mehrere der Agents. 

Es kann vorkommen, dass spezifische Anforderungen für einen bestimmten virtuellen Computer nicht vollständig durch einen einzelnen Agent abgedeckt werden können. Dies wäre etwa der Fall, wenn Sie Metrikwarnungen verwenden möchten, für die die Azure-Diagnoseerweiterung erforderlich ist, gleichzeitig aber auch die Funktionen von Azure Monitor für VMs nutzen möchten, für die der Log Analytics-Agent und der Dependency-Agent benötigt werden. In solchen Fällen können Sie mehrere Agents verwenden. Dies ist ein gängiges Szenario für Kunden, die Funktionen beider Agents benötigen.

## <a name="summary-of-agents"></a>Übersicht über die Agents

Die folgenden Tabellen enthalten eine kurze Gegenüberstellung der Azure Monitor-Agents für Windows und Linux. Weitere Details zu den jeweiligen Agents finden Sie im Abschnitt weiter unten. 

### <a name="windows-agents"></a>Windows-Agents

| | Diagnose<br>erweiterung (WAD) | Log Analytics<br>Agent | Abhängigkeit<br>Agent |
|:---|:---|:---|:---|
| **Unterstützte Umgebungen** | Azure | Azure<br>Andere Cloud<br>Lokal | Azure<br>Andere Cloud<br>Lokal | 
| **Agent-Anforderungen**  | Keine | Keine | Erfordert Log Analytics-Agent |
| **Gesammelte Daten** | Ereignisprotokolle<br>ETW-Ereignisse<br>Leistung<br>Dateibasierte Protokolle<br>IIS-Protokolle<br>.NET-App-Protokolle<br>Absturzabbilder<br>Agent-Diagnoseprotokolle | Ereignisprotokolle<br>Leistung<IIS logs><br>Dateibasierte Protokolle<br>Erkenntnisse und Lösungen<br>Sonstige Dienste | Prozessdetails und Abhängigkeiten<br>Netzwerkverbindungsmetriken |
| **Senden von Daten an** | Azure Storage<br>Azure Monitor-Metriken<br>Event Hub | Azure Monitor-Protokolle | Azure Monitor-Protokolle |


### <a name="linux-agents"></a>Linux-Agents

| | Diagnose<br>erweiterung (LAD) | Telegraf-<br>Agent | Log Analytics<br>Agent | Abhängigkeit<br>Agent |
|:---|:---|:---|:---|:---|
| **Unterstützte Umgebungen** | Azure | Azure<br>Andere Cloud<br>Lokal | Azure<br>Andere Cloud<br>Lokal | Azure<br>Andere Cloud<br>Lokal |
| **Agent-Anforderungen**  | Keine | Keine | Keine | Erfordert Log Analytics-Agent |
| **Gesammelte Daten** | syslog<br>Leistung | Leistung | syslog<br>Leistung| Prozessdetails und Abhängigkeiten<br>Netzwerkverbindungsmetriken |
| **Senden von Daten an** | Azure Storage<br>Event Hub | Azure Monitor-Metriken | Azure Monitor-Protokolle | Azure Monitor-Protokolle |

## <a name="log-analytics-agent"></a>Log Analytics-Agent

Der [Log Analytics-Agent](log-analytics-agent.md) erfasst Überwachungsdaten vom Gastbetriebssystem und Workloads von virtuellen Computern in Azure, anderen Cloudanbietern und lokal. Die Daten werden in einem Log Analytics-Arbeitsbereich gesammelt. Der Log Analytics-Agent ist der gleiche Agent, der auch von System Center Operations Manager verwendet wird, und Sie können Multihoming-Agent-Computer verwenden, um gleichzeitig mit Ihrer Verwaltungsgruppe und Azure Monitor zu kommunizieren. Dieser Agent wird auch für bestimmte Erkenntnisse und Lösungen in Azure Monitor benötigt.


> [!NOTE]
> Der Log Analytics-Agent für Windows wird häufig als Microsoft Monitoring Agent (MMA) bezeichnet. Der Log Analytics-Agent für Linux wird häufig als OMS-Agent bezeichnet.



Verwenden Sie den Log Analytics-Agent für Folgendes:

* Erfassen von Protokollen und Leistungsdaten von virtuellen oder physischen Computern außerhalb von Azure 
* Senden von Überwachungsdaten an einen Log Analytics-Arbeitsbereich, um die von [Azure Monitor-Protokollen](data-platform-logs.md#what-can-you-do-with-azure-monitor-logs) unterstützten Features wie etwa [Protokollabfragen](../log-query/log-query-overview.md) zu nutzen
* Verwenden von [Azure Monitor für VMs](../insights/vminsights-overview.md) zur bedarfsgerechten Überwachung Ihrer virtuellen Computer sowie zur Überwachung der zugehörigen Prozesse und Abhängigkeiten von anderen Ressourcen und externen Prozessen  
* Verwalten der Sicherheit Ihrer virtuellen Computer mit [Azure Security Center](../../security-center/security-center-intro.md) oder [Azure Sentinel](../../sentinel/overview.md)
* Verwenden von [Azure Automation-Updateverwaltung](../../automation/automation-update-management.md), [Azure Automation State Configuration](../../automation/automation-dsc-overview.md) oder [Azure Automation für Änderungsnachverfolgung und Bestand](../../automation/change-tracking.md) zur umfassenden Verwaltung Ihrer virtuellen Azure-Computer
* Verwenden verschiedener [Lösungen](../monitor-reference.md#insights-and-core-solutions) zur Überwachung eines bestimmten Diensts oder einer bestimmten Anwendung

Einschränkungen des Log Analytics-Agents:

- Kann keine Daten an Azure Monitor-Metriken, an Azure Storage oder an Azure Event Hubs senden

## <a name="azure-diagnostics-extension"></a>Azure-Diagnoseerweiterung

Die [Azure-Diagnoseerweiterung](diagnostics-extension-overview.md) erfasst Überwachungsdaten vom Gastbetriebssystem sowie von Workloads auf virtuellen Azure-Computern und anderen Computeressourcen. Die Daten werden in erster Linie in Azure Storage erfasst. Es können aber auch Datensenken definiert werden, um Daten an andere Ziele zu senden (etwa an Azure Monitor-Metriken oder an Azure Event Hubs).

Verwenden Sie die Azure-Diagnoseerweiterung für Folgendes:

- Senden von Daten an Azure Storage, um sie zu archivieren oder mit Tools wie dem [Azure Storage-Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md) zu analysieren
- Senden von Daten an [Azure Monitor-Metriken](data-platform-metrics.md), um sie mit dem [Metrik-Explorer](metrics-getting-started.md) analysieren und Features wie [Metrikwarnungen](../../azure-monitor/platform/alerts-metric-overview.md) in Quasi-Echtzeit und [automatische Skalierung](autoscale-overview.md) (nur unter Windows) nutzen zu können
- Senden von Daten an Drittanbietertools mithilfe von [Azure Event Hubs](diagnostics-extension-stream-event-hubs.md)
- Erfassen von [Startdiagnosedaten](../../virtual-machines/troubleshooting/boot-diagnostics.md) zur Untersuchung von VM-Startfehlern

Einschränkungen der Azure-Diagnoseerweiterung:

- Kann nur mit Azure-Ressourcen verwendet werden
- Senden von Daten an Azure Monitor-Protokolle nur eingeschränkt möglich

## <a name="telegraf-agent"></a>Telegraf-Agent

Der [InfluxData Telegraf-Agent](collect-custom-metrics-linux-telegraf.md) wird verwendet, um Leistungsdaten von Linux-Computern in Azure Monitor-Metriken zu erfassen.

Verwenden Sie den Telegraf-Agent für Folgendes:

* Senden von Daten an [Azure Monitor-Metriken](data-platform-metrics.md), um sie mit dem [Metrik-Explorer](metrics-getting-started.md) analysieren und Features wie [Metrikwarnungen](../../azure-monitor/platform/alerts-metric-overview.md) in Quasi-Echtzeit und [automatische Skalierung](autoscale-overview.md) (nur unter Linux) nutzen zu können 



## <a name="dependency-agent"></a>Abhängigkeits-Agent

Der Dependency-Agent sammelt ermittelte Daten über Prozesse, die auf dem virtuellen Computer ausgeführt werden, und über Abhängigkeiten von externen Prozessen. 

Verwenden Sie den Dependency-Agent für Folgendes:

* Verwenden des Zuordnungsfeatures [Azure Monitor für VMs](../insights/vminsights-overview.md) oder der [Dienstzuordnung](../insights/service-map.md)

Beachten Sie bei der Verwendung des Dependency-Agents Folgendes:

- Für den Dependency-Agent muss der Log Analytics-Agent auf demselben virtuellen Computer installiert sein.
- Bei Linux-VMs muss der Log Analytics-Agent vor der Azure-Diagnoseerweiterung installiert werden.

## <a name="extensions-compared-to-agents"></a>Gegenüberstellung von Erweiterungen und Agents

Durch die Log Analytics-Erweiterung für [Windows](../../virtual-machines/extensions/oms-windows.md) und [Linux](../../virtual-machines/extensions/oms-linux.md) wird der Log Analytics-Agent auf virtuellen Azure-Computern installiert. Durch die Azure Monitor-Dependency-Erweiterung für [Windows](../../virtual-machines/extensions/agent-dependency-windows.md) und [Linux](../../virtual-machines/extensions/agent-dependency-linux.md) wird der Dependency-Agent auf virtuellen Azure-Computern installiert. Hierbei handelt es sich um die oben beschriebenen Agents, allerdings mit der Möglichkeit, sie über [VM-Erweiterungen](../../virtual-machines/extensions/overview.md) zu verwalten. Verwenden Sie zum Installieren und Verwalten der Agents möglichst immer Erweiterungen.


## <a name="next-steps"></a>Nächste Schritte

Weitere Details zu den einzelnen Agents finden Sie hier:

- [Übersicht über den Log Analytics-Agent](log-analytics-agent.md)
- [Übersicht zur Azure-Diagnoseerweiterung](diagnostics-extension-overview.md)
- [Erfassen von benutzerdefinierten Metriken für einen virtuellen Linux-Computer mit dem InfluxData Telegraf-Agent](collect-custom-metrics-linux-telegraf.md)
