---
title: Behandeln von Problemen mit der Azure-Änderungsnachverfolgung
description: Hier erfahren Sie, wie Sie Probleme mit dem Azure Automation-Feature für Änderungsnachverfolgung und Bestand beheben.
services: automation
ms.service: automation
ms.subservice: change-inventory-management
author: mgoedtel
ms.author: magoedte
ms.date: 01/31/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 6cadaea1a20743071acbe8860df02ca7bbdde954
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77198529"
---
# <a name="troubleshoot-change-tracking-and-inventory"></a>Problembehandlung bei Änderungsnachverfolgung und Inventur

## <a name="windows"></a>Windows

### <a name="scenario-change-tracking-records-arent-showing-for-windows-machines"></a><a name="records-not-showing-windows"></a>Szenario: Datensätze der Änderungsnachverfolgung werden für Windows-Computer nicht angezeigt

#### <a name="issue"></a>Problem

Es werden keine Ergebnisse für die Änderungsnachverfolgung oder den Bestand von Windows-Computern angezeigt, die in die Änderungsnachverfolgung integriert sind.

#### <a name="cause"></a>Ursache

Dieser Fehler kann folgende Ursachen haben:

* Microsoft Monitoring Agent wird nicht ausgeführt.
* Die Kommunikation an das Automation-Konto wird blockiert.
* Die Management Packs für die Änderungsnachverfolgung wurden nicht heruntergeladen.
* Die zu integrierende VM stammt möglicherweise von einem geklonten Computer, der nicht über Sysprep mit installiertem Microsoft Monitoring Agent vorbereitet wurde.

#### <a name="resolution"></a>Lösung

Die nachstehend beschriebenen Lösungen helfen möglicherweise beim Beheben Ihres Problems. Wenn Sie weitere Hilfe benötigen, können Sie Diagnoseinformationen sammeln und sich an den Support wenden. Navigieren Sie auf dem Agent-Computer zu „C:\Programme\Microsoft Monitoring Agent\Agent\Tools“, und führen Sie die folgenden Befehle aus:

```cmd
net stop healthservice
StopTracing.cmd
StartTracing.cmd VER
net start healthservice
```

> [!NOTE]
> Standardmäßig ist die Fehlerablaufverfolgung aktiviert. Um ausführliche Fehlermeldungen wie im vorherigen Beispiel zu aktivieren, verwenden Sie den Parameter *VER*. Verwenden Sie *INF* beim Aufruf von **StartTracing.cmd**, um Ablaufverfolgungen zu Informationen zu erhalten.

##### <a name="microsoft-monitoring-agent-not-running"></a>Microsoft Monitoring Agent nicht ausgeführt

Überprüfen Sie, ob Microsoft Monitoring Agent („HealthService.exe“) auf dem Computer ausgeführt wird.

##### <a name="communication-to-automation-account-blocked"></a>Kommunikation mit dem Automation-Konto blockiert

Überprüfen Sie die Ereignisanzeige auf dem Computer, und suchen Sie nach Ereignissen, die das Wort „changetracking“ enthalten.

Informationen zu Adressen und Ports, die für die Arbeit mit der Änderungsnachverfolgung zugelassen werden müssen, finden Sie unter [Automatisieren von Ressourcen im Rechenzentrum oder in der Cloud mit Hybrid Runbook Worker](../automation-hybrid-runbook-worker.md#network-planning).

##### <a name="management-packs-not-downloaded"></a>Management Packs nicht heruntergeladen

Überprüfen Sie, ob die Management Packs für die Änderungsnachverfolgung und den Bestand lokal installiert wurden:

* Microsoft.IntelligencePacks.ChangeTrackingDirectAgent.*
* Microsoft.IntelligencePacks.InventoryChangeTracking.*
* Microsoft.IntelligencePacks.SingletonInventoryCollection.*

##### <a name="vm-from-cloned-machine-that-has-not-been-sysprepped"></a>VM von geklontem Computer, der nicht mit Sysprep vorbereitet wurde

Wenn Sie ein geklontes Image verwenden, bereiten Sie das Image zunächst mit Sysprep vor, und installieren Sie dann Microsoft Monitoring Agent.

## <a name="linux"></a>Linux

### <a name="scenario-no-change-tracking-or-inventory-results-on-linux-machines"></a>Szenario: Keine Änderungsnachverfolgung oder Bestandsergebnisse auf Linux-Computern

#### <a name="issue"></a>Problem

Es werden keine Ergebnisse für den Bestand oder die Änderungsnachverfolgung für Linux-Computer angezeigt, die in die Änderungsnachverfolgung integriert sind. 

#### <a name="cause"></a>Ursache
Im Folgenden sind mögliche Ursachen für dieses Problem aufgeführt:
* Der Log Analytics-Agent für Linux wird nicht ausgeführt.
* Der Log Analytics-Agent für Linux wurde nicht ordnungsgemäß konfiguriert.
* Es bestehen Konflikte mit der Überwachung der Dateiintegrität (File Integrity Monitoring, FIM).

#### <a name="resolution"></a>Lösung 

##### <a name="log-analytics-agent-for-linux-not-running"></a>Log Analytics-Agent für Linux nicht ausgeführt

Überprüfen Sie, ob der Daemon für den Log Analytics-Agent für Linux (omsagent) auf Ihrem Computer ausgeführt wird. Führen Sie in dem Log Analytics-Arbeitsbereich, der mit Ihrem Automation-Konto verknüpft ist, die folgende Abfrage aus.

```loganalytics Copy
Heartbeat
| summarize by Computer, Solutions
```

Wenn Ihr Computer nicht in den Abfrageergebnissen aufgeführt ist, wurde er in letzter Zeit nicht eingecheckt. Es liegt wahrscheinlich ein Problem mit der lokalen Konfiguration vor, und Sie sollten den Agent neu installieren. Weitere Informationen zur Installation und Konfiguration finden Sie unter [Sammeln von Protokolldaten mit dem Log Analytics-Agent](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent). 

Wenn Ihr Computer in den Abfrageergebnissen angezeigt wird, überprüfen Sie die Bereichskonfiguration. Weitere Informationen finden Sie unter [Zielgruppenadressierung für Überwachungslösungen in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/solution-targeting).

Weitere Informationen zum Troubleshooting für dieses Problem finden Sie unter [Problem: Es werden keine Linux-Daten angezeigt](https://docs.microsoft.com/azure/azure-monitor/platform/agent-linux-troubleshoot#issue-you-are-not-seeing-any-linux-data).

##### <a name="log-analytics-agent-for-linux-not-configured-correctly"></a>Log Analytics-Agent für Linux nicht ordnungsgemäß konfiguriert

Der Log Analytics-Agent für Linux wurde möglicherweise nicht ordnungsgemäß für die Sammlung der Protokolle und Befehlszeilenausgaben mit dem Tool OMS Log Collector konfiguriert. Weitere Informationen finden Sie unter [Nachverfolgen von Änderungen in Ihrer Umgebung mit der Lösung für die Änderungsnachverfolgung](../change-tracking.md).

##### <a name="fim-conflicts"></a>FIM-Konflikte

Das FIM-Feature von Azure Security Center kann die Integrität Ihrer Linux-Dateien nicht ordnungsgemäß validieren. Vergewissern Sie sich, dass FIM betriebsbereit und ordnungsgemäß für die Linux-Dateiüberwachung konfiguriert ist. Weitere Informationen finden Sie unter [Nachverfolgen von Änderungen in Ihrer Umgebung mit der Lösung für die Änderungsnachverfolgung](../change-tracking.md).

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, erhalten Sie über einen der folgenden Kanäle weitere Unterstützung.

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
* Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.
