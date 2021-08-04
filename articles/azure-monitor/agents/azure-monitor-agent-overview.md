---
title: Übersicht über den Azure Monitor-Agent
description: Übersicht über den Azure Monitor-Agent (AMA) zur Sammlung von Überwachungsdaten aus dem Gastbetriebssystem virtueller Computer.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/22/2021
ms.custom: references_regions
ms.openlocfilehash: 7c12fff502d8c4b68470370e0a5eede1dd3866a9
ms.sourcegitcommit: 3941df51ce4fca760797fa4e09216fcfb5d2d8f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2021
ms.locfileid: "114605373"
---
# <a name="azure-monitor-agent-overview"></a>Übersicht über den Azure Monitor-Agent
Der Azure Monitor-Agent (AMA) sammelt Überwachungsdaten aus dem Gastbetriebssystem virtueller Azure-Computer und übermittelt sie an Azure Monitor. Dieser Artikel enthält eine Übersicht über den Azure Monitor-Agent sowie Informationen zu seiner Installation und zur Konfiguration der Datensammlung.

## <a name="relationship-to-other-agents"></a>Beziehung zu anderen Agents
Der Azure Monitor-Agent ersetzt die folgenden Legacy-Agents, die derzeit von Azure Monitor verwendet werden, um Gastdaten von virtuellen Computern zu sammeln ([bekannte Lücken anzeigen](/azure/azure-monitor/faq#is-the-new-azure-monitor-agent-at-parity-with-existing-agents)):

- [Log Analytics-Agent:](./log-analytics-agent.md) Sendet Daten an den Log Analytics-Arbeitsbereich und unterstützt VM Insights und Überwachungslösungen.
- [Diagnoseerweiterung:](./diagnostics-extension-overview.md) Sendet Daten an Azure Monitor-Metriken (nur Windows), Azure Event Hubs und Azure Storage.
- [Telegraf-Agent:](../essentials/collect-custom-metrics-linux-telegraf.md) Sendet Daten an Azure Monitor-Metriken (nur Linux).

Neben der Konsolidierung dieser Funktionalität in einem einzelnen Agent bietet der Azure Monitor-Agent folgende Vorteile gegenüber den vorhandenen Agents:

- Überwachungsumfang: Konfigurieren Sie die Sammlung zentral für verschiedene Daten von verschiedenen virtuellen Computern.  
- Multi-Homing (Linux): Senden Sie Daten von virtuellen Linux-Computern an mehrere Arbeitsbereiche.
- Ereignisfilterung (Windows): Verwenden Sie XPATH-Abfragen, um zu filtern, welche Windows-Ereignisse gesammelt werden.
- Verbesserte Erweiterungsverwaltung: Der Azure Monitor-Agent verwendet eine neue Methode für den Umgang mit der Erweiterbarkeit. Diese Methode ist transparenter und besser steuerbar als Management Packs und Linux-Plug-Ins in den aktuellen Log Analytics-Agents.

### <a name="changes-in-data-collection"></a>Änderungen bei der Datensammlung
Die Methoden zum Definieren der Datensammlung für die vorhandenen Agents unterscheiden sich deutlich voneinander und sind jeweils mit gewissen Herausforderungen verbunden, die durch den Azure Monitor-Agent beseitigt werden.

- Der Log Analytics-Agent bezieht seine Konfiguration aus einem Log Analytics-Arbeitsbereich. Dies lässt sich zwar ganz einfach zentral konfigurieren, es ist jedoch schwierig, unabhängige Definitionen für verschiedene virtuelle Computer zu definieren. Es können nur Daten an einen Log Analytics-Arbeitsbereich gesendet werden.

- Die Diagnoseerweiterung verfügt über eine Konfiguration für jeden virtuellen Computer. Es lassen sich ganz einfach unabhängige Definitionen für verschiedene virtuelle Computer definieren, die zentrale Verwaltung gestaltet sich jedoch schwierig. Daten können nur an Azure Monitor-Metriken, an Azure Event Hubs oder an Azure Storage gesendet werden. Für Linux-Agents ist der Telegraf-Agent erforderlich, um Daten an Azure Monitor-Metriken zu senden.

Der Azure Monitor-Agent verwendet [Datensammlungsregeln (Data Collection Rules, DCR)](data-collection-rule-overview.md), um die Daten zu konfigurieren, die von den einzelnen Agents gesammelt werden sollen. Datensammlungsregeln ermöglichen die Verwaltung von Sammlungseinstellungen im großen Stil sowie die Verwendung individueller, bereichsspezifischer Konfigurationen für Untergruppen von Computern. Dank ihrer Unabhängigkeit von Arbeitsbereichen und virtuellen Computern können sie einmalig definiert und für verschiedene Computer und Umgebungen wiederverwendet werden. Weitere Informationen finden Sie unter [Konfigurieren der Datensammlung für den Azure Monitor-Agent](data-collection-rule-azure-monitor-agent.md).

## <a name="should-i-switch-to-azure-monitor-agent"></a>Sollte ich zum Azure Monitor-Agent wechseln?
Der Azure Monitor-Agent ersetzt die [Legacy-Agents für Azure Monitor](agents-overview.md), und Sie können mit dem Übergang Ihrer VMs von den aktuellen Agents zum neuen Agent beginnen. Berücksichtigen Sie dabei die folgenden Faktoren:

- **Umgebungsanforderungen.** Der Azure Monitor-Agent unterstützt derzeit [diese Betriebssysteme](./agents-overview.md#supported-operating-systems). Unterstützung für zukünftige Betriebssystemversionen, Umgebungsunterstützung und Netzwerkanforderungen werden wahrscheinlich in diesem neuen Agent bereitgestellt. Sie sollten bewerten, ob Ihre Umgebung von Azure Monitor-Agent unterstützt wird. Wenn dies nicht der Fall ist, müssen Sie unter Umständen den aktuellen Agent beibehalten. Wenn Azure Monitor-Agent Ihre aktuelle Umgebung unterstützt, sollten Sie einen Wechsel in Erwägung ziehen.
- **Aktuelle und neue Funktionsanforderungen.** Der Azure Monitor-Agent führt mehrere neue Funktionen wie Filterung, Bereichsdefinition und Multi-Homing ein, ist aber noch nicht gleichwertig mit den aktuellen Agents für andere Funktionen wie benutzerdefinierte Protokollsammlung und Integration in alle Lösungen (siehe [Häufig gestellte Fragen zu Azure Monitor](/azure/azure-monitor/faq#which-log-analytics-solutions-are-supported-on-the-new-azure-monitor-agent)). Die meisten neuen Funktionen in Azure Monitor werden nur mit Azure Monitor-Agent zur Verfügung gestellt, sodass im Laufe der Zeit mehr Funktionen nur im neuen Agent verfügbar sind. Berücksichtigen Sie, ob der Azure Monitor-Agent die von Ihnen benötigten Funktionen hat und ob Sie auf einige Funktionen vorübergehend verzichten können, um andere wichtige Funktionen im neuen Agent zu erhalten. Wenn der Azure Monitor-Agent über alle Kernfunktionen verfügt, die Sie benötigen, sollten Sie eine Umstellung darauf erwägen. Wenn es entscheidende Funktionen gibt, die Sie benötigen, fahren Sie mit dem aktuellen Agent fort, bis der Azure Monitor-Agent die Featureparität erreicht.
- **Toleranz für Nacharbeit.** Wenn Sie eine neue Umgebung mit Ressourcen wie Bereitstellungsskripts und Onboardingvorlagen einrichten, bewerten Sie den dafür erforderlichen Aufwand. Wenn erheblicher Arbeitsaufwand erforderlich ist, sollten Sie Ihre neue Umgebung mit dem neuen Agent einrichten. Dieser ist nun allgemein verfügbar. Für die Log Analytics-Agents wird im August 2021 ein Veraltungsdatum veröffentlicht. Die aktuellen Agents werden nach Beginn der Veraltungsphase noch einige Jahre lang unterstützt.

## <a name="supported-resource-types"></a>Unterstützte Ressourcentypen
Derzeit werden virtuelle Azure-Computer, VM-Skalierungsgruppen und Server mit Azure Arc-Unterstützung unterstützt. Azure Kubernetes Service und andere Compute-Ressourcentypen werden zurzeit nicht unterstützt.


## <a name="supported-regions"></a>Unterstützte Regionen
Der Azure Monitor-Agent ist in allen öffentlichen Regionen verfügbar, die Log Analytics unterstützen. Government-Regionen und -Clouds werden momentan nicht unterstützt.
## <a name="supported-services-and-features"></a>Unterstützte Dienste und Features
In der folgenden Tabelle sehen Sie die aktuelle Unterstützung für den Azure Monitor-Agent mit anderen Azure-Diensten:

| Azure-Dienst | Aktuelle Unterstützung | Weitere Informationen |
|:---|:---|:---|
| [Azure Security Center](../../security-center/security-center-introduction.md) | Private Vorschau | [Registrierungslink](https://aka.ms/AMAgent) |
| [Azure Sentinel](../../sentinel/overview.md) | Private Vorschau | [Registrierungslink](https://aka.ms/AMAgent) |


In der folgenden Tabelle sehen Sie die aktuelle Unterstützung für den Azure Monitor-Agent mit Azure Monitor-Features:

| Azure Monitor-Feature | Aktuelle Unterstützung | Weitere Informationen |
|:---|:---|:---|
| [VM Insights](../vm/vminsights-overview.md) | Private Vorschau  | [Registrierungslink](https://forms.office.com/r/jmyE821tTy) |
| [Gastintegrität von VM Insights](../vm/vminsights-health-overview.md) | Public Preview | Nur im neuen Agent verfügbar |
| [Erkenntnisse zu SQL](../insights/sql-insights-overview.md) | Public Preview | Nur im neuen Agent verfügbar |

In der folgenden Tabelle sehen Sie die aktuelle Unterstützung für den Azure Monitor-Agent mit Azure-Lösungen:

| Lösung | Aktuelle Unterstützung | Weitere Informationen |
|:---|:---|:---|
| [Änderungsnachverfolgung](../../automation/change-tracking/overview.md) | Als Dateiintegritätsüberwachung (File Integrity Monitoring, FIM) in der privaten Azure Security Center-Vorschau unterstützt  | [Registrierungslink](https://aka.ms/AMAgent) |
| [Updateverwaltung](../../automation/update-management/overview.md) | Verwenden Sie Updateverwaltung v2 (private Vorschau), für die kein Agent erforderlich ist. | [Registrierungslink](https://www.yammer.com/azureadvisors/threads/1064001355087872) |



## <a name="coexistence-with-other-agents"></a>Koexistenz mit anderen Agents
Der Azure Monitor-Agent kann parallel zu den bereits vorhandenen Agents vorhanden sein, sodass deren Funktionen bei der Bewertung oder Migration weiterhin zur Verfügung stehen. Dies ist besonders wichtig, da gewisse Einschränkungen bei der Unterstützung bereits vorhandener Lösungen bestehen. Eine doppelte Datensammlung kann jedoch zu einer Verzerrung von Abfrageergebnissen führen und zusätzliche Kosten für die Datenerfassung und -aufbewahrung zur Folge haben. 

So wird von VM Insights beispielsweise der Log Analytics-Agent verwendet, um Leistungsdaten an einen Log Analytics-Arbeitsbereich zu senden. Gegebenenfalls haben Sie auch den Arbeitsbereich für die Sammlung von Windows- und Syslog-Ereignissen konfiguriert. Wenn Sie nun den Azure Monitor-Agent installieren und eine Datensammlungsregel für die gleichen Ereignisse und Leistungsdaten erstellen, führt dies zu doppelten Daten.

Stellen Sie daher sicher, dass Sie nicht die gleichen Daten von beiden Agents sammeln. Ist dies der Fall, stellen Sie sicher, dass sie an separate Ziele gesendet werden.


## <a name="costs"></a>Kosten
Für den Azure Monitor-Agent fallen keine Kosten an, möglicherweise aber für die erfassten Daten. Ausführliche Informationen zur Datensammlung und -aufbewahrung von Log Analytics sowie zu Kundenmetriken finden Sie unter [Azure Monitor – Preise](https://azure.microsoft.com/pricing/details/monitor/).

## <a name="data-sources-and-destinations"></a>Datenquellen und -ziele
Die folgende Tabelle gibt Aufschluss darüber, welche Datentypen Sie aktuell mit dem Azure Monitor-Agent unter Verwendung von Datensammlungsregeln sammeln und wohin diese Daten gesendet werden können. Unter [Was wird von Azure Monitor überwacht?](../monitor-reference.md) finden Sie eine Liste mit Erkenntnissen und wichtigen Lösungen sowie mit anderen Lösungen, die den Azure Monitor-Agent verwenden, um andere Arten von Daten zu sammeln.


Der Azure Monitor-Agent sendet Daten an Azure Monitor-Metriken oder an einen Log Analytics-Arbeitsbereich, der Azure Monitor-Protokolle unterstützt.

| Data source | Destinations | BESCHREIBUNG |
|:---|:---|:---|
| Leistung        | Azure Monitor-Metriken<sup>1</sup><br>Log Analytics-Arbeitsbereich | Numerische Werte zur Messung der Leistung verschiedener Betriebssystem- und Workloadaspekte |
| Windows-Ereignisprotokolle | Log Analytics-Arbeitsbereich | An das Windows-System für die Ereignisprotokollierung gesendete Informationen |
| syslog             | Log Analytics-Arbeitsbereich | Informationen, die an das Linux-System für die Ereignisprotokollierung gesendet werden |

<sup>1</sup> Derzeit gilt eine Einschränkung für den Azure Monitor-Agent für Linux, die bewirkt, dass die Verwendung von Azure Monitor-Metriken als *einziges* Ziel nicht unterstützt wird. Die Verwendung zusammen mit Azure Monitor-Protokollen funktioniert. Diese Einschränkung wird im nächsten Erweiterungsupdate behoben.

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme
Eine Liste der Windows- und Linux-Betriebssystemversionen, die derzeit vom Azure Monitor-Agent unterstützt werden, finden Sie unter [Unterstützte Betriebssysteme](agents-overview.md#supported-operating-systems).



## <a name="security"></a>Sicherheit
Der Azure Monitor-Agent benötigt zwar keine Schlüssel, dafür aber eine [systemseitig zugewiesene verwaltete Identität](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md#system-assigned-managed-identity). Für die Agent-Bereitstellung muss auf dem jeweiligen virtuellen Computer eine systemseitig zugewiesene verwaltete Identität aktiviert sein.

## <a name="networking"></a>Netzwerk
Der Azure Monitor-Agent unterstützt Azure-Diensttags (es sind sowohl AzureMonitor- als auch AzureResourceManager-Tags erforderlich), kann jedoch noch nicht mit Azure Monitor-Private Link-Bereichen verwendet werden. Wenn der Computer für die Kommunikation über das Internet einen Proxyserver durchlaufen muss, sehen Sie sich die weiter unten angegebenen Anforderungen an, um sich mit der erforderlichen Netzwerkkonfiguration vertraut zu machen.

### <a name="proxy-configuration"></a>Proxykonfiguration

Die Azure Monitor-Agent-Erweiterungen für Windows und Linux können mithilfe des HTTPS-Protokolls entweder über einen Proxyserver oder über ein Log Analytics-Gateway mit Azure Monitor kommunizieren (für virtuelle Azure-Computer, Azure-VM-Skalierungsgruppen und Azure Arc für Server). Diese Methode wird mithilfe von Erweiterungseinstellungen wie unten beschrieben konfiguriert und unterstützt sowohl die anonyme Authentifizierung als auch die Standardauthentifizierung (Benutzername/Kennwort).  

1. Verwenden Sie dieses einfache Flussdiagramm, um zuerst die Werte der Parameter *setting* und *protectedSetting* zu bestimmen:

   ![Flussdiagramm zum Bestimmen der Werte von der Parameter „setting“ und „protectedSetting“ beim Aktivieren der Erweiterung](media/azure-monitor-agent-overview/proxy-flowchart.png)


2. Wenn Sie die Werte der Parameter *setting* und *protectedSetting* bestimmt haben, geben Sie diese zusätzlichen Parameter beim Bereitstellen des Azure Monitor-Agents mithilfe von PowerShell-Befehlen an (die folgenden Beispiele sind für virtuelle Azure-Computer):

    | Parameter | Wert |
    |:---|:---|
    | SettingString | JSON-Objekt aus dem obigen Flussdiagramm, konvertiert in eine Zeichenfolge; überspringen, falls nicht zutreffend. Beispiel: {"proxy":{"mode":"application","address":"http://[Adresse]:[Port]","auth": false}} |
    | ProtectedSettingString | JSON-Objekt aus dem obigen Flussdiagramm, konvertiert in eine Zeichenfolge; überspringen, falls nicht zutreffend. Beispiel: {"proxy":{"username": "[Benutzername]","password": "[Kennwort]"}} |


# <a name="windows"></a>[Windows](#tab/PowerShellWindows)
```powershell
Set-AzVMExtension -ExtensionName AzureMonitorWindowsAgent -ExtensionType AzureMonitorWindowsAgent -Publisher Microsoft.Azure.Monitor -ResourceGroupName <resource-group-name> -VMName <virtual-machine-name> -Location <location> -TypeHandlerVersion 1.0 -SettingString <settingString> -ProtectedSettingString <protectedSettingString>
```

# <a name="linux"></a>[Linux](#tab/PowerShellLinux)
```powershell
Set-AzVMExtension -ExtensionName AzureMonitorLinuxAgent -ExtensionType AzureMonitorLinuxAgent -Publisher Microsoft.Azure.Monitor -ResourceGroupName <resource-group-name> -VMName <virtual-machine-name> -Location <location> -TypeHandlerVersion 1.5 -SettingString <settingString> -ProtectedSettingString <protectedSettingString>
```

---

## <a name="next-steps"></a>Nächste Schritte

- [Installieren des Azure Monitor-Agents](azure-monitor-agent-install.md) auf Windows- und Linux-VMs
- [Erstellen Sie eine Datensammlungsregel](data-collection-rule-azure-monitor-agent.md), um Daten des Agents zu sammeln und an Azure Monitor zu senden.
