---
title: Übersicht über den Azure Monitor-Agent
description: Übersicht über den Azure Monitor-Agent (AMA) zur Sammlung von Überwachungsdaten aus dem Gastbetriebssystem virtueller Computer.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/10/2020
ms.openlocfilehash: ea2fae483da495bce9551899b9646868251f0454
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2020
ms.locfileid: "90030826"
---
# <a name="azure-monitor-agent-overview-preview"></a>Übersicht über den Azure Monitor-Agent (Vorschauversion)
Der Azure Monitor-Agent (AMA) sammelt Überwachungsdaten aus dem Gastbetriebssystem virtueller Computer und übermittelt sie an Azure Monitor. Dieser Artikel enthält eine Übersicht über den Azure Monitor-Agent sowie Informationen zur Installation und zur Konfiguration der Datensammlung.

## <a name="relationship-to-other-agents"></a>Beziehung zu anderen Agents
Der Azure Monitor-Agent ersetzt die folgenden Agents, die derzeit von Azure Monitor verwendet werden, um Gastdaten von virtuellen Computern zu sammeln:

- [Log Analytics-Agent:](log-analytics-agent.md) Sendet Daten an den Log Analytics-Arbeitsbereich und unterstützt Azure Monitor für VMs und Überwachungslösungen.
- [Diagnoseerweiterung:](diagnostics-extension-overview.md) Sendet Daten an Azure Monitor-Metriken (nur Windows), Azure Event Hubs und Azure Storage.
- [Telegraf-Agent:](collect-custom-metrics-linux-telegraf.md) Sendet Daten an Azure Monitor-Metriken (nur Linux).

Neben der Konsolidierung dieser Funktionalität in einem einzelnen Agent bietet der Azure Monitor-Agent folgende Vorteile gegenüber den vorhandenen Agents:

- Überwachungsumfang: Konfigurieren Sie die Sammlung zentral für verschiedene Daten von verschiedenen virtuellen Computern.  
- Multi-Homing (Linux): Senden Sie Daten von virtuellen Linux-Computern an mehrere Arbeitsbereiche.
- Ereignisfilterung (Windows): Verwenden Sie XPATH-Abfragen, um zu filtern, welche Windows-Ereignisse gesammelt werden.
- Verbesserte Erweiterungsverwaltung: Der Azure Monitor-Agent verwendet eine neue Methode für den Umgang mit der Erweiterbarkeit. Diese Methode ist transparenter und besser steuerbar als Management Packs und Linux-Plug-Ins in den aktuellen Log Analytics-Agents.

### <a name="changes-in-data-collection"></a>Änderungen bei der Datensammlung
Die Methoden zum Definieren der Datensammlung für die vorhandenen Agents unterscheiden sich deutlich voneinander und sind jeweils mit gewissen Herausforderungen verbunden, die durch den Azure Monitor-Agent beseitigt werden.

- Der Log Analytics-Agent bezieht seine Konfiguration aus einem Log Analytics-Arbeitsbereich. Dies lässt sich zwar ganz einfach zentral konfigurieren, es ist jedoch schwierig, unabhängige Definitionen für verschiedene virtuelle Computer zu definieren. Es können nur Daten an einen Log Analytics-Arbeitsbereich gesendet werden.

- Die Diagnoseerweiterung verfügt über eine Konfiguration für jeden virtuellen Computer. Dadurch lassen sich ganz einfach unabhängige Definitionen für verschiedene virtuelle Computer definieren, die zentrale Verwaltung gestaltet sich jedoch schwierig. Daten können nur an Azure Monitor-Metriken, an Azure Event Hubs oder an Azure Storage gesendet werden. Für Linux-Agents ist der Telegraf-Agent erforderlich, um Daten an Azure Monitor-Metriken zu senden.

Der Azure Monitor-Agent verwendet [Datensammlungsregeln (Data Collection Rules, DCR)](data-collection-rule-overview.md), um die Daten zu konfigurieren, die von den einzelnen Agents gesammelt werden sollen. Datensammlungsregeln ermöglichen die Verwaltung von Sammlungseinstellungen im großen Stil sowie die Verwendung individueller, bereichsspezifischer Konfigurationen für Untergruppen von Computern. Dank ihrer Unabhängigkeit von Arbeitsbereichen und virtuellen Computern können sie einmalig definiert und für verschiedene Computer und Umgebungen wiederverwendet werden. Weitere Informationen finden Sie unter [Konfigurieren der Datensammlung für den Azure Monitor-Agent (Vorschau)](data-collection-rule-azure-monitor-agent.md).



## <a name="current-limitations"></a>Aktuelle Einschränkungen
Während der Public Preview-Phase des Azure Monitor-Agents gelten folgende Einschränkungen:

- Der Azure Monitor-Agent unterstützt keine Lösungen und Erkenntnisse wie Azure Monitor für VMs und Azure Security Center. Das einzige derzeit unterstützte Szenario ist die Sammlung von Daten unter Verwendung der von Ihnen konfigurierten Datensammlungsregeln. 
- Datensammlungsregeln müssen in der gleichen Region erstellt werden, in der sich auch die als Ziel verwendeten Log Analytics-Arbeitsbereiche befinden.
- Aktuell werden nur virtuelle Azure-Computer unterstützt. Lokale virtuelle Computer, VM-Skalierungsgruppen, Arc für Server, Azure Kubernetes Service und andere Computeressourcentypen werden derzeit nicht unterstützt.
- Der virtuelle Computer benötigt Zugriff auf die folgenden HTTPS-Endpunkte:
  - *.ods.opinsights.azure.com
  - *.ingest.monitor.azure.com
  - *.control.monitor.azure.com


## <a name="coexistence-with-other-agents"></a>Koexistenz mit anderen Agents
Der Azure Monitor-Agent kann parallel zu den bereits vorhandenen Agents vorhanden sein, sodass deren Funktionen bei der Bewertung oder Migration weiterhin zur Verfügung stehen. Dies ist besonders wichtig, da während der Public Preview-Phase gewisse Einschränkungen bei der Unterstützung bereits vorhandener Lösungen bestehen. Eine doppelte Datensammlung kann jedoch zu einer Verzerrung von Abfrageergebnissen führen und zusätzliche Kosten für die Datenerfassung und -aufbewahrung zur Folge haben.

So wird von Azure Monitor für VMs beispielsweise der Log Analytics-Agent verwendet, um Leistungsdaten an einen Log Analytics-Arbeitsbereich zu senden. Gegebenenfalls haben Sie auch den Arbeitsbereich für die Sammlung von Windows- und Syslog-Ereignissen konfiguriert. Wenn Sie nun den Azure Monitor-Agent installieren und eine Datensammlungsregel für die gleichen Ereignisse und Leistungsdaten erstellen, führt dies zu doppelten Daten.


## <a name="costs"></a>Kosten
Für den Azure Monitor-Agent fallen keine Kosten an, möglicherweise aber für die erfassten Daten. Ausführliche Informationen zur Datensammlung und -aufbewahrung von Log Analytics sowie zu Kundenmetriken finden Sie unter [Azure Monitor – Preise](https://azure.microsoft.com/pricing/details/monitor/).

## <a name="data-sources-and-destinations"></a>Datenquellen und -ziele
Die folgende Tabelle gibt Aufschluss darüber, welche Datentypen Sie aktuell mit dem Azure Monitor-Agent unter Verwendung von Datensammlungsregeln sammeln und wohin diese Daten gesendet werden können. Unter [Was wird von Azure Monitor überwacht?](../monitor-reference.md) finden Sie eine Liste mit Erkenntnissen und wichtigen Lösungen sowie mit anderen Lösungen, die den Azure Monitor-Agent verwenden, um andere Arten von Daten zu sammeln.


Der Azure Monitor-Agent sendet Daten an Azure Monitor-Metriken oder an einen Log Analytics-Arbeitsbereich, der Azure Monitor-Protokolle unterstützt.

| Data source | Destinations | BESCHREIBUNG |
|:---|:---|:---|
| Leistung        | Azure Monitor-Metriken<br>Log Analytics-Arbeitsbereich | Numerische Werte zur Messung der Leistung verschiedener Betriebssystem- und Workloadaspekte |
| Windows-Ereignisprotokolle | Log Analytics-Arbeitsbereich | An das Windows-System für die Ereignisprotokollierung gesendete Informationen |
| syslog             | Log Analytics-Arbeitsbereich | Informationen, die an das Linux-System für die Ereignisprotokollierung gesendet werden |


## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme
Der Azure Monitor-Agent unterstützt aktuell folgende Betriebssysteme:

### <a name="windows"></a>Windows 
  - Windows Server 2019
  - Windows Server 2016
  - Windows Server 2012
  - Windows Server 2012 R2

### <a name="linux"></a>Linux
  - CentOS 6<sup>1</sup>, 7
  - Debian 9, 10
  - Oracle Linux 6<sup>1</sup>, 7
  - RHEL 6<sup>1</sup>, 7
  - SLES 11, 12, 15
  - Ubuntu 14.04 LTS, 16.04 LTS, 18.04 LTS

> [!IMPORTANT]
> <sup>1</sup>Für diese Verteilungen zum Senden von Syslog-Daten müssen Sie den rsyslog-Dienst einmal nach der Installation des Agents neu starten.


## <a name="security"></a>Sicherheit
Der Azure Monitor-Agent benötigt zwar keine Schlüssel, dafür aber eine [systemseitig zugewiesene verwaltete Identität](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md#system-assigned-managed-identity). Für die Agent-Bereitstellung muss auf dem jeweiligen virtuellen Computer eine systemseitig zugewiesene verwaltete Identität aktiviert sein.

## <a name="networking"></a>Netzwerk
Der Azure Monitor-Agent unterstützt Azure-Diensttags (es sind sowohl AzureMonitor- als auch AzureResourceManager-Tags erforderlich), derzeit jedoch weder Azure Monitor Private Link-Bereiche noch direkte Proxys.

## <a name="install-the-azure-monitor-agent"></a>Installieren des Azure Monitor-Agents:
Der Azure Monitor-Agent wird als [Azure-VM-Erweiterung](../../virtual-machines/extensions/overview.md) mit den Details aus der folgenden Tabelle implementiert: 

| Eigenschaft | Windows | Linux |
|:---|:---|:---|
| Herausgeber | Microsoft.Azure.Monitor  | Microsoft.Azure.Monitor |
| type      | AzureMonitorWindowsAgent | AzureMonitorLinuxAgent  |
| TypeHandlerVersion  | 1.0 | 1.5 |

Der Azure Monitor-Agent kann mithilfe einer beliebigen Installationsmethode für VM-Agents installiert werden. Hierzu zählen auch die im Anschluss angegebenen Methoden mit PowerShell oder über die Befehlszeilenschnittstelle. Alternativ können Sie wie unter [Konfigurieren der Datensammlung für den Azure Monitor-Agent (Vorschau)](data-collection-rule-azure-monitor-agent.md#create-using-the-azure-portal) beschrieben das Azure-Portal verwenden, um den Agent zu installieren und die Datensammlung auf virtuellen Computern in Ihrem Azure-Abonnement zu konfigurieren.

### <a name="windows"></a>Windows

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/CLI1)

```azurecli
az vm extension set --name AzureMonitorWindowsAgent --publisher Microsoft.Azure.Monitor --ids {resource ID of the VM}

```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell1)

```powershell
Set-AzVMExtension -Name AMAWindows -ExtensionType AzureMonitorWindowsAgent -Publisher Microsoft.Azure.Monitor -ResourceGroupName {Resource Group Name} -VMName {VM name} -Location eastus
```
---


### <a name="linux"></a>Linux

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/CLI2)

```azurecli
az vm extension set --name AzureMonitorLinuxAgent --publisher Microsoft.Azure.Monitor --ids {resource ID of the VM}

```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell2)

```powershell
Set-AzVMExtension -Name AMALinux -ExtensionType AzureMonitorLinuxAgent -Publisher Microsoft.Azure.Monitor -ResourceGroupName {Resource Group Name} -VMName {VM name} -Location eastus
```
---

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen Sie eine Datensammlungsregel](data-collection-rule-azure-monitor-agent.md), um Daten des Agents zu sammeln und an Azure Monitor zu senden.
