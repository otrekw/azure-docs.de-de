---
title: Abhängigkeitsanalyse in der Azure Migrate-Serverbewertung
description: Hier wird beschrieben, wie Sie die Abhängigkeitsanalyse zur Bewertung mithilfe der Azure Migrate-Serverbewertung verwenden.
ms.topic: conceptual
ms.date: 04/15/2020
ms.openlocfilehash: f0b956620895ae2264b53916015d440f5e586eb2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82024760"
---
# <a name="dependency-analysis"></a>Abhängigkeitsanalyse

In diesem Artikel wird die Abhängigkeitsanalyse in der Azure Migrate-Serverbewertung beschrieben.

## <a name="overview"></a>Übersicht

Mit der Abhängigkeitsanalyse können Sie Abhängigkeiten zwischen lokalen Computern identifizieren, die Sie bewerten und zu Azure migrieren möchten. 

- In der Serverbewertung von Azure Migrate fassen Sie Computer in einer Gruppe zusammen und bewerten dann die Gruppe. Mithilfe der Abhängigkeitsanalyse können Sie Computer präziser gruppieren und eine hohe Genauigkeit bei der Bewertung erreichen.
- Die Abhängigkeitsanalyse ermöglicht Ihnen, Computer zu identifizieren, die gemeinsam migriert werden müssen. Sie können ermitteln, ob Computer verwendet werden oder ob sie außer Betrieb genommen werden können, anstatt sie zu migrieren.
- Die Analyse von Abhängigkeiten trägt dazu bei, dass nichts übersehen wird. Außerdem werden unerwartete Ausfälle während der Migration vermieden.
- Die Analyse ist besonders nützlich, wenn Sie nicht sicher sind, ob die Computer Teil einer App-Bereitstellung sind, die Sie zu Azure migrieren möchten.
- [Lesen Sie](common-questions-discovery-assessment.md#what-is-dependency-visualization) häufig gestellte Fragen zur Abhängigkeitsanalyse.

Es gibt zwei Optionen für die Bereitstellung der Abhängigkeitsanalyse:

- **Agent-basiert:** Die Agent-basierte Abhängigkeitsanalyse erfordert, dass auf jedem lokalen Computer, den Sie analysieren möchten, Agents installiert sind.
- **Ohne Agent:** Bei der Analyse ohne Agent müssen Sie keine Agents auf Computern installieren, die Sie überprüfen möchten. Diese Option befindet sich derzeit in der Vorschauphase und ist nur für VMware-VMs verfügbar.

> [!NOTE]
> Die Agent-basierte Abhängigkeitsanalyse ist in Azure Government nicht verfügbar. Sie können die Abhängigkeitsanalyse ohne Agent verwenden.

## <a name="agentless-analysis"></a>Analyse ohne Agent

Bei der Abhängigkeitsanalyse ohne Agent werden TCP-Verbindungsdaten von den Computern erfasst, für die sie aktiviert wurde. Auf Computern, die Sie analysieren möchten, sind keine Agents installiert.

### <a name="collected-data"></a>Gesammelte Daten

Nach dem Start der Abhängigkeitsermittlung ruft die Appliance alle fünf Minuten Daten von Computern ab, um diese zu sammeln. Diese Daten werden mithilfe von vSphere-APIs von Gast-VMs über vCenter Server gesammelt. Die gesammelten Daten werden auf der Azure Migrate-Appliance verarbeitet, um Identitätsinformationen abzuleiten, und alle sechs Stunden an Azure Migrate gesendet.

Beim Abruf werden folgende Daten von Computern gesammelt: 
- Name von Prozessen mit aktiven Verbindungen
- Name der Anwendung, die Prozesse mit aktiven Verbindungen ausführt
- Zielport für die aktiven Verbindungen

## <a name="agent-based-analysis"></a>Agent-basierte Analyse

Bei der Agent-basierten Analyse verwendet die Serverbewertung die [Dienstzuordnungslösung](../azure-monitor/insights/service-map.md) in Azure Monitor, um die Visualisierung und Analyse von Abhängigkeiten zu ermöglichen. Der [Microsoft Monitoring Agent/Log Analytics-Agent](../azure-monitor/platform/agents-overview.md#log-analytics-agent) und der [Dependency-Agent](../azure-monitor/platform/agents-overview.md#dependency-agent) müssen auf jedem Computer installiert sein, den Sie analysieren möchten.

### <a name="collected-data"></a>Gesammelte Daten

Bei der Agent-basierten Analyse werden folgende Daten gesammelt:

- Servername des Quellcomputers, Prozess, Anwendungsname.
- Servername des Zielcomputers, Prozess, Anwendungsname und Port.
- Anzahl der Verbindungen, Wartezeiten und Datenübertragungsinformationen werden gesammelt und stehen für Log Analytics-Abfragen zur Verfügung. 


## <a name="compare-agentless-and-agent-based"></a>Vergleich zwischen Visualisierung ohne Agent und Agent-basierter Visualisierung

Die Unterschiede zwischen der Visualisierung ohne Agent und der Agent-basierten Visualisierung sind in der Tabelle zusammengefasst.

**Anforderung** | **Ohne Agent** | **Agent-basiert**
--- | --- | ---
Support | Diese Option befindet sich derzeit in der Vorschauphase und ist nur für VMware-VMs verfügbar. [Lesen](migrate-support-matrix-vmware.md#agentless-dependency-analysis-requirements) Sie den Artikel zu den unterstützten Betriebssystemen. | In der allgemeinen Verfügbarkeit (General Availability, GA).
Agent | Sie müssen keine Agents auf Computern installieren, die Sie überprüfen möchten. | Auf jedem lokalen Computer, den Sie analysieren möchten, müssen Agents installiert sein: Der [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows) und der [Dependency-Agent](https://docs.microsoft.com/azure/azure-monitor/platform/agents-overview#dependency-agent). 
Log Analytics | Nicht erforderlich. | Azure Migrate verwendet für die Abhängigkeitsanalyse die [Dienstzuordnungslösung](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) in [Azure Monitor-Protokolle](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview). 
Funktionsweise | Erfasst TCP-Verbindungsdaten auf Computern, die für die Visualisierung von Abhängigkeiten aktiviert sind. Nach der Ermittlung werden Daten in Abständen von fünf Minuten gesammelt. | Auf einem Computer installierte Agents für die Dienstzuordnung sammeln Daten über TCP-Prozesse und ein-/ausgehende Verbindungen für die einzelnen Prozesse.
Daten | Servername des Quellcomputers, Prozess, Anwendungsname.<br/><br/> Servername des Zielcomputers, Prozess, Anwendungsname und Port. | Servername des Quellcomputers, Prozess, Anwendungsname.<br/><br/> Servername des Zielcomputers, Prozess, Anwendungsname und Port.<br/><br/> Anzahl der Verbindungen, Wartezeiten und Datenübertragungsinformationen werden gesammelt und stehen für Log Analytics-Abfragen zur Verfügung. 
Visualisierung | Das Abhängigkeitsdiagramm eines einzelnen Servers kann über eine Dauer von einer Stunde bis hin zu 30 Tagen angezeigt werden. | Abhängigkeitsdiagramm eines einzelnen Servers.<br/><br/> Das Diagramm kann nur über eine Stunde angezeigt werden.<br/><br/> Abhängigkeitsdiagramm für eine Gruppe von Servern.<br/><br/> Hinzufügen und Entfernen von Servern in einer Gruppe aus der Diagrammansicht.
Datenexport | Kann derzeit nicht im Tabellenformat heruntergeladen werden. | Daten können mit Log Analytics abgefragt werden.



## <a name="next-steps"></a>Nächste Schritte
- Überprüfen Sie die Anforderungen für das Einrichten der Agent-basierten Analyse für [VMware-VMs](migrate-support-matrix-vmware.md#agent-based-dependency-analysis-requirements), [physische Server](migrate-support-matrix-physical.md#agent-based-dependency-analysis-requirements) und [Hyper-V-VMs](migrate-support-matrix-hyper-v.md#agent-based-dependency-analysis-requirements).
- [Überprüfen Sie](migrate-support-matrix-vmware.md#agentless-dependency-analysis-requirements) die Anforderungen für die Analyse von VMware-VMs ohne Agent.
- [Richten Sie](how-to-create-group-machine-dependencies.md) die Agent-basierte Visualisierung von Abhängigkeiten ein.
- [Probieren Sie](how-to-create-group-machine-dependencies-agentless.md) die Abhängigkeitsvisualisierung ohne Agent für VMware-VMs aus.
- Lesen Sie [häufige Fragen](common-questions-discovery-assessment.md#what-is-dependency-visualization) zur Abhängigkeitsvisualisierung.


