---
title: Behandeln von Problemen mit der Änderungsnachverfolgung und dem Bestand
description: Erfahren Sie, wie Sie Probleme mit der Azure Automation-Lösung für Änderungsnachverfolgung und Bestand beheben.
services: automation
ms.service: automation
ms.subservice: change-inventory-management
author: mgoedtel
ms.author: magoedte
ms.date: 01/31/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 11c1fd05055922b07801c20d525d852d5360b069
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81679346"
---
# <a name="troubleshoot-change-tracking-and-inventory-issues"></a>Behandeln von Problemen mit Änderungsnachverfolgung und Bestand

In diesem Artikel wird beschrieben, wie Sie Probleme mit der Änderungsnachverfolgung und dem Bestand beheben.

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](../automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="windows"></a>Windows

### <a name="scenario-change-tracking-and-inventory-records-arent-showing-for-windows-machines"></a><a name="records-not-showing-windows"></a>Szenario: Datensätze von Änderungsnachverfolgung und Bestand werden für Windows-Computer nicht angezeigt

#### <a name="issue"></a>Problem

Es werden keine Ergebnisse für die Änderungsnachverfolgung oder den Bestand von integrierten Windows-Computern angezeigt.

#### <a name="cause"></a>Ursache

Dieser Fehler kann folgende Ursachen haben:

* Der Log Analytics-Agent für Windows wird nicht ausgeführt.
* Die Kommunikation an das Automation-Konto wird blockiert.
* Die Management Packs für Änderungsnachverfolgung und Bestand wurden nicht heruntergeladen.
* Die VM, für die das Onboarding durchgeführt wird, stammt möglicherweise von einem geklonten Computer, der nicht über Sysprep mit installiertem Log Analytics-Agent für Windows vorbereitet wurde.

#### <a name="resolution"></a>Lösung

Navigieren Sie auf dem Computer mit dem Log Analytics-Agent zu **C:\Programme\Microsoft Monitoring Agent\Agent\Tools**, und führen Sie die folgenden Befehle aus:

```cmd
net stop healthservice
StopTracing.cmd
StartTracing.cmd VER
net start healthservice
```

Wenn Sie weitere Hilfe benötigen, können Sie Diagnoseinformationen sammeln und sich an den Support wenden. 

> [!NOTE]
> Der Log Analytics-Agent aktiviert standardmäßig die Fehlerablaufverfolgung. Um ausführliche Fehlermeldungen wie im vorherigen Beispiel zu aktivieren, verwenden Sie den Parameter `VER`. Für Ablaufverfolgungen zur Information verwenden Sie `INF` beim Aufrufen von `StartTracing.cmd`.

##### <a name="log-analytics-agent-for-windows-not-running"></a>Der Log Analytics-Agent für Windows wird nicht ausgeführt.

Vergewissern Sie sich, dass der Log Analytics-Agent (**HealthService.exe**) auf dem Computer ausgeführt wird.

##### <a name="communication-to-automation-account-blocked"></a>Kommunikation mit dem Automation-Konto blockiert

Überprüfen Sie die Ereignisanzeige auf dem Computer, und suchen Sie nach allen Ereignisse, die das Wort `changetracking` enthalten.

Informationen zu Adressen und Ports, die für die Arbeit mit Änderungsnachverfolgung und Bestand zugelassen werden müssen, finden Sie unter [Automatisieren von Ressourcen im Rechenzentrum oder in der Cloud mit Hybrid Runbook Worker](../automation-hybrid-runbook-worker.md#network-planning).

##### <a name="management-packs-not-downloaded"></a>Management Packs nicht heruntergeladen

Überprüfen Sie, ob die Management Packs für die Änderungsnachverfolgung und den Bestand lokal installiert wurden:

* `Microsoft.IntelligencePacks.ChangeTrackingDirectAgent.*`
* `Microsoft.IntelligencePacks.InventoryChangeTracking.*`
* `Microsoft.IntelligencePacks.SingletonInventoryCollection.*`

##### <a name="vm-from-cloned-machine-that-has-not-been-sysprepped"></a>VM von geklontem Computer, der nicht mit Sysprep vorbereitet wurde

Wenn Sie ein geklontes Image verwenden, bereiten Sie das Image zunächst mit Sysprep vor, und installieren Sie dann den Log Analytics-Agent für Windows.

## <a name="linux"></a>Linux

### <a name="scenario-no-change-tracking-and-inventory-results-on-linux-machines"></a>Szenario: Keine Ergebnisse für Änderungsnachverfolgung und Bestand auf Linux-Computern

#### <a name="issue"></a>Problem

Es werden keine Ergebnisse für den Bestand oder die Änderungsnachverfolgung für Linux-Computer angezeigt, die in die Lösung integriert sind. 

#### <a name="cause"></a>Ursache
Im Folgenden sind mögliche Ursachen für dieses Problem aufgeführt:
* Der Log Analytics-Agent für Linux wird nicht ausgeführt.
* Der Log Analytics-Agent für Linux wurde nicht ordnungsgemäß konfiguriert.
* Es bestehen Konflikte mit der Überwachung der Dateiintegrität (File Integrity Monitoring, FIM).

#### <a name="resolution"></a>Lösung 

##### <a name="log-analytics-agent-for-linux-not-running"></a>Log Analytics-Agent für Linux nicht ausgeführt

Überprüfen Sie, ob der Daemon für den Log Analytics-Agent für Linux (**omsagent**) auf dem Computer ausgeführt wird. Führen Sie in dem Log Analytics-Arbeitsbereich, der mit Ihrem Automation-Konto verknüpft ist, die folgende Abfrage aus.

```loganalytics Copy
Heartbeat
| summarize by Computer, Solutions
```

Wenn Ihr Computer nicht in den Abfrageergebnissen aufgeführt ist, wurde er in letzter Zeit nicht eingecheckt. Es liegt wahrscheinlich ein Problem mit der lokalen Konfiguration vor, und Sie sollten den Agent neu installieren. Weitere Informationen zur Installation und Konfiguration finden Sie unter [Sammeln von Protokolldaten mit dem Log Analytics-Agent](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent). 

Wenn Ihr Computer in den Abfrageergebnissen angezeigt wird, überprüfen Sie die Bereichskonfiguration. Weitere Informationen finden Sie unter [Zielgruppenadressierung für Überwachungslösungen in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/solution-targeting).

Weitere Informationen zum Troubleshooting für dieses Problem finden Sie unter [Problem: Es werden keine Linux-Daten angezeigt](https://docs.microsoft.com/azure/azure-monitor/platform/agent-linux-troubleshoot#issue-you-are-not-seeing-any-linux-data).

##### <a name="log-analytics-agent-for-linux-not-configured-correctly"></a>Log Analytics-Agent für Linux nicht ordnungsgemäß konfiguriert

Der Log Analytics-Agent für Linux wurde möglicherweise nicht ordnungsgemäß für die Sammlung der Protokolle und Befehlszeilenausgaben mit dem Tool OMS Log Collector konfiguriert. Weitere Informationen finden Sie unter [Nachverfolgen von Änderungen in Ihrer Umgebung mit der Lösung für Änderungsnachverfolgung und Bestand](../change-tracking.md).

##### <a name="fim-conflicts"></a>FIM-Konflikte

Das FIM-Feature von Azure Security Center kann die Integrität Ihrer Linux-Dateien nicht ordnungsgemäß validieren. Vergewissern Sie sich, dass FIM betriebsbereit und ordnungsgemäß für die Linux-Dateiüberwachung konfiguriert ist. Weitere Informationen finden Sie unter [Nachverfolgen von Änderungen in Ihrer Umgebung mit der Lösung für Änderungsnachverfolgung und Bestand](../change-tracking.md).

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt wird oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.
* Erstellen Sie einen Azure-Supportfall. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.
