---
title: Übersicht über den Log Analytics-Agent
description: In diesem Thema erfahren Sie, wie Sie mit Log Analytics Daten sammeln und Computer überwachen, die in Azure, lokal oder in einer anderen Cloudumgebung gehostet werden.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/04/2020
ms.openlocfilehash: bf2939c28afb682d4053a27920b9cf57795d2e86
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77467231"
---
# <a name="log-analytics-agent-overview"></a>Übersicht über den Log Analytics-Agent
Der Azure Log Analytics-Agent wurde für die umfassende Verwaltung von virtuellen Computern in einer beliebigen Cloud, von lokalen Computern und von Computern entwickelt, die von [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/) überwacht werden. Die Windows- und Linux-Agents senden erfasste Protokolldaten aus verschiedenen Quellen an Ihren Log Analytics-Arbeitsbereich in Azure Monitor, ebenso wie individuelle Protokolle oder Metriken (gemäß Definition in einer Überwachungslösung). Der Log Analytics-Agent unterstützt auch Erkenntnisse und andere Dienste in Azure Monitor wie etwa [Azure Monitor für VMs](), [Azure Security Center]() und [Azure Automation]().

Dieser Artikel enthält eine ausführliche Übersicht über den Agent sowie Informationen zu System- und Netzwerkanforderungen und zu den verschiedenen Bereitstellungsmethoden.

> [!NOTE]
> Der Log Analytics-Agent wird ggf. auch als Microsoft Monitoring Agent (MMA) oder als OMS-Linux-Agent bezeichnet.

> [!NOTE]
> Die Azure-Diagnoseerweiterung ist einer der Agents, die zum Erfassen von Überwachungsdaten aus dem Gastbetriebssystem von Computeressourcen verfügbar sind. Unter [Übersicht über die Azure Monitor-Agents](agents-overview.md) finden Sie eine Beschreibung der verschiedenen Agents sowie Informationen zur Wahl der geeigneten Agents für Ihre Anforderungen.

## <a name="comparison-to-azure-diagnostics-extension"></a>Vergleich mit der Azure-Diagnoseerweiterung
Die [Azure-Diagnoseerweiterung](diagnostics-extension-overview.md) in Azure Monitor kann ebenfalls zum Erfassen von Überwachungsdaten aus dem Gastbetriebssystem virtueller Azure-Computer verwendet werden. Je nach Ihren Anforderungen können Sie eine der Möglichkeiten oder beide nutzen. Einen ausführlichen Vergleich der Azure Monitor-Agents finden Sie unter [Übersicht über die Azure Monitor-Agents](agents-overview.md). 

Beachten Sie die folgenden Hauptunterschiede:

- Die Azure-Diagnoseerweiterung kann nur mit virtuellen Azure-Computern verwendet werden. Der Log Analytics-Agent kann mit virtuellen Computern in Azure, anderen Clouds und lokal verwendet werden.
- Die Azure-Diagnoseerweiterung sendet Daten an Azure Storage, [Azure Monitor-Metriken](data-platform-metrics.md) (nur Windows) und Event Hubs. Der Log Analytics-Agent erfasst Daten für [Azure Monitor-Protokolle](data-platform-logs.md).
- Der Log Analytics-Agent ist für [Lösungen](../monitor-reference.md#insights-and-core-solutions), [Azure Monitor für VMs](../insights/vminsights-overview.md) und andere Dienste wie [Azure Security Center](/azure/security-center/) erforderlich.

## <a name="costs"></a>Kosten
Für den Log Analytics-Agent fallen keine Kosten an, möglicherweise aber für die erfassten Daten. Ausführliche Informationen zu den Kosten von Daten, die in einem Log Analytics-Arbeitsbereich erfasst werden, finden Sie unter [Verwalten von Nutzung und Kosten mit Azure Monitor-Protokollen](manage-cost-storage.md).

## <a name="data-collected"></a>Gesammelte Daten
Die folgende Tabelle enthält die Arten von Daten, mit denen Sie einen Log Analytics-Arbeitsbereich konfigurieren können, damit sie von allen verbundenen Agents erfasst werden. Unter [Was wird von Azure Monitor überwacht?](../monitor-reference.md) finden Sie eine Liste mit Erkenntnissen und wichtigen Lösungen sowie mit anderen Lösungen, die den Log Analytics-Agent verwenden, um andere Arten von Daten zu erfassen.

| Datenquelle | Beschreibung |
| --- | --- |
| [Windows-Ereignisprotokolle](data-sources-windows-events.md) | An das Windows-System für die Ereignisprotokollierung gesendete Informationen |
| [Syslog](data-sources-syslog.md)                     | Informationen, die an das Linux-System für die Ereignisprotokollierung gesendet werden |
| [Leistung](data-sources-performance-counters.md)  | Numerische Werte zur Messung der Leistung verschiedener Betriebssystem- und Workloadaspekte |
| [IIS-Protokolle](data-sources-iis-logs.md)                 | Nutzungsinformationen für IIS-Websites, die unter dem Gastbetriebssystem ausgeführt werden |
| [Benutzerdefinierte Protokolle](data-sources-custom-logs.md)           | Ereignisse aus Textdateien auf Windows- und Linux-Computern |

## <a name="data-destinations"></a>Datenziele
Der Log Analytics-Agent sendet Daten an einen Log Analytics-Arbeitsbereich in Azure Monitor. Der Windows-Agent kann mehrfach vernetzt werden, um Daten an mehrere Arbeitsbereiche und System Center Operations Manager-Verwaltungsgruppen zu senden. Der Linux-Agent kann nur Daten an ein einzelnes Ziel senden.

## <a name="other-services"></a>Sonstige Dienste
Der Agent für Linux und Windows dient nicht zur Verbindungsherstellung mit Azure Monitor, sondern unterstützt auch Azure Automation, um die Hybrid Runbook-Workerrolle und andere Dienste wie [Änderungsnachverfolgung](../../automation/change-tracking.md), [Updateverwaltung](../../automation/automation-update-management.md) und [Azure Security Center](../../security-center/security-center-intro.md) zu hosten. Weitere Informationen zur Hybrid Runbook Workerrolle finden Sie unter [Azure Automation Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md).  

## <a name="installation-and-configuration"></a>Installation und Konfiguration

Wenn Sie die Log Analytics-Agents zum Sammeln von Daten verwenden, muss Ihnen zum Planen der Agent-Bereitstellung Folgendes bekannt sein:

* Um Daten von Windows-Agents zu sammeln, können Sie [jeden Agent so konfigurieren, dass er Berichte an einen oder mehrere Arbeitsbereiche übermittelt](agent-windows.md), auch wenn er Berichte parallel an eine System Center Operations Manager-Verwaltungsgruppe sendet. Der Windows-Agent kann Berichte an bis zu vier Arbeitsbereiche übermitteln.
* Der Linux-Agent bietet keine Unterstützung für Multihosting und kann Berichte nur an einen einzelnen Arbeitsbereich übermitteln.
* Der Windows-Agent unterstützt den [FIPS 140-Standard](https://docs.microsoft.com/windows/security/threat-protection/fips-140-validation), während der Linux-Agent diesen nicht unterstützt.  

Wenn Sie System Center Operations Manager 2012 R2 oder höher verwenden, gilt Folgendes:

* Jede Operations Manager-Verwaltungsgruppe kann [nur mit einem Arbeitsbereich verbunden sein](om-agents.md).
* Linux-Computer, die Berichte an eine Verwaltungsgruppe senden, müssen so konfiguriert sein, dass die Berichte direkt an einen Log Analytics-Arbeitsbereich übermittelt werden. Wenn Ihre Linux-Computer Berichte bereits direkt an einen Arbeitsbereich übermitteln und Sie diese mit Operations Manager überwachen möchten, führen Sie die Schritte zum [Senden von Berichten an eine Operations Manager-Verwaltungsgruppe](agent-manage.md#configure-agent-to-report-to-an-operations-manager-management-group) aus.
* Sie können den Log Analytics-Windows-Agent auf dem Windows-Computer installieren und so konfigurieren, dass er Berichte sowohl an die in einen Arbeitsbereich integrierte Operations Manager-Instanz als auch an einen anderen Arbeitsbereich sendet.


Abhängig von Ihren Anforderungen gibt es mehrere Möglichkeiten, um den Log Analytics-Agents zu installieren und Ihren Computer mit Azure Monitor zu verbinden. Die folgende Tabelle hebt die einzelnen Methoden hervor, um festzustellen, welche Methode in Ihrer Organisation am besten funktioniert.

|`Source` | Methode | Beschreibung|
|-------|-------------|-------------|
|Azure VM| [Manuell über das Azure-Portal](../../azure-monitor/learn/quick-collect-azurevm.md?toc=/azure/azure-monitor/toc.json) | Geben Sie virtuelle Computer für die Bereitstellung über den Log Analytics-Arbeitsbereich an. |
| | Log Analytics-VM-Erweiterung für [Windows](../../virtual-machines/extensions/oms-windows.md) oder [Linux](../../virtual-machines/extensions/oms-linux.md) unter Verwendung der Azure-Befehlszeilenschnittstelle oder einer Azure Resource Manager-Vorlage | Die Erweiterung installiert den Log Analytics-Agent auf virtuellen Azure-Computern und registriert sie in einem vorhandenen Azure Monitor-Arbeitsbereich. |
| | [Azure Monitor für VMs](../insights/vminsights-enable-overview.md) | Wenn Sie die Überwachung mit Azure Monitor für VMs aktivieren, werden die Log Analytics-Erweiterung und der Log Analytics-Agent installiert. |
| | [Automatische Bereitstellung von Azure Security Center](../../security-center/security-center-enable-data-collection.md) | Azure Security Center kann den Log Analytics-Agent auf allen unterstützten und neu erstellten virtuellen Azure-Computern bereitstellen, wenn Sie die Überwachung auf Sicherheitslücken und Bedrohungen aktivieren. Wenn diese Option aktiviert ist, werden alle neuen oder vorhandenen VMs ohne installierten Agent bereitgestellt. |
| Windows-Hybridcomputer| [Manuelle Installation](agent-windows.md) | Installieren Sie den Microsoft Monitoring-Agent über die Befehlszeile. |
| | [Azure Automation DSC](agent-windows.md#install-the-agent-using-dsc-in-azure-automation) | Automatisieren Sie die Installation mit Azure Automation DSC. |
| | [Resource Manager-Vorlage mit Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) | Verwenden Sie eine Azure Resource Manager-Vorlage, wenn Sie Microsoft Azure Stack in Ihrem Rechenzentrum bereitgestellt haben.| 
| Linux-Hybridcomputer| [Manuelle Installation](../../azure-monitor/learn/quick-collect-linux-computer.md)|Installieren Sie den Agent für Linux durch Aufrufen eines Wrapperskripts, das auf GitHub gehostet wird. | 
| System Center Operations Manager|[Integrieren von Operations Manager mit Log Analytics](../../azure-monitor/platform/om-agents.md) | Konfigurieren Sie die Integration zwischen Operations Manager und Azure Monitor-Protokollen, um die gesammelten Daten von Windows-Computern weiterzuleiten, die Berichte für eine Verwaltungsgruppe erstellen.|  


## <a name="supported-windows-operating-systems"></a>Unterstützte Windows-Betriebssysteme

Die folgenden Versionen des Windows-Betriebssystems werden für den Windows-Agent offiziell unterstützt:

* Windows Server 2019
* Windows Server 2008 SP2 (x64), 2008 R2, 2012, 2012 R2, 2016, Version 1709 und 1803
* Windows 7 SP1, Windows 8 Enterprise und Pro sowie Windows 10 Enterprise und Pro

>[!NOTE]
>Während der Log Analytics-Agent für Windows für die Unterstützung von Serverüberwachungsszenarios konzipiert wurde, führen Sie möglicherweise den Windows-Client aus, um Workloads zu unterstützen, die für das Serverbetriebssystem konfiguriert und optimiert wurden. Der Agent unterstützt den Windows-Client, unsere Überwachungslösungen konzentrieren sich jedoch nicht auf Clientüberwachungsszenarios, sofern nicht explizit angegeben.

## <a name="supported-linux-operating-systems"></a>Unterstützte Linux-Betriebssysteme

Dieser Abschnitt enthält Details zu den unterstützten Linux-Distributionen.

Beginnend mit den nach August 2018 veröffentlichten Versionen gelten folgende Änderungen für unser Supportmodell:  

* Es werden nur die Serverversionen und keine Clientversionen unterstützt.  
* Neue Versionen der [von Azure unterstützten Linux-Distributionen](../../virtual-machines/linux/endorsed-distros.md) werden immer unterstützt.  
* Alle Nebenversionen werden für jede aufgeführte Hauptversion unterstützt.
* Versionen, für die der Support des Herstellers abgelaufen ist, werden nicht unterstützt.  
* Neue Versionen von AMI werden nicht unterstützt.  
* Nur Versionen, die standardmäßig SSL 1.x ausführen, werden unterstützt.

>[!NOTE]
>Wenn Sie eine Distribution oder eine Version verwenden, die derzeit nicht unterstützt wird und nicht auf unser Supportmodell abgestimmt ist, wird empfohlen, dieses Repository zu forken und dabei anzuerkennen, dass der Microsoft-Support bei geforkten Agent-Versionen keine Unterstützung bietet.

* Amazon Linux 2017.09 (x64)
* CentOS Linux 6 (x86/x64) und 7 (x64)  
* Oracle Linux 6 und 7 (x86/x64) 
* Red Hat Enterprise Linux Server 6 (x86/x64) und 7 (x64)
* Debian GNU/Linux 8 und 9 (x86/x64)
* Ubuntu 14.04 LTS (x86/x64), 16.04 LTS (x86/x64) und 18.04 LTS (x64)
* SUSE Linux Enterprise Server 12 (x64) und 15 (x64)

>[!NOTE]
>OpenSSL 1.1.0 wird nur auf x86_x64-Plattformen (64 Bit) unterstützt. OpenSSL vor Version 1.x wird auf keiner Plattform unterstützt.
>

### <a name="agent-prerequisites"></a>Agent-Voraussetzungen

In der folgenden Tabelle werden die Pakete hervorgehoben, die für unterstützte Linux-Distributionen erforderlich sind, auf denen der Agent installiert wird.

|Erforderliches Paket |Beschreibung |Mindestversion |
|-----------------|------------|----------------|
|Glibc |    GNU C-Bibliothek | 2.5-12 
|Openssl    | OpenSSL-Bibliotheken | 1.0.x oder 1.1.x |
|Curl | cURL-Webclient | 7.15.5 |
|Python-ctypes | | 
|PAM | Module für austauschbare Authentifizierung | | 

>[!NOTE]
>Zum Sammeln von syslog-Nachrichten sind entweder rsyslog oder syslog-ng erforderlich. Der Standard-syslog-Daemon in Version 5 von Red Hat Enterprise Linux, CentOS und Oracle Linux-Version (sysklog) wird für die syslog-Ereigniserfassung nicht unterstützt. Der rsyslog-Daemon sollte installiert und so konfiguriert werden, dass er sysklog ersetzt, um syslog-Daten von dieser Version dieser Verteilung zu sammeln.

## <a name="tls-12-protocol"></a>TLS 1.2-Protokoll

Um die Sicherheit von Daten bei der Übertragung an Azure Monitor-Protokolle sicherzustellen, wird dringend empfohlen, den Agent so zu konfigurieren, dass er mindestens TLS 1.2 (Transport Layer Security) verwendet. Bei älteren Versionen von TLS/Secure Sockets Layer (SSL) wurde ein Sicherheitsrisiko festgestellt. Sie funktionieren aus Gründen der Abwärtskompatibilität zwar noch, werden jedoch **nicht empfohlen**.  Weitere Informationen finden Sie unter [Senden von Daten über TLS 1.2](data-security.md#sending-data-securely-using-tls-12). 


## <a name="network-requirements"></a>Netzwerkanforderungen
Der Agent für Linux und Windows kommuniziert in ausgehender Richtung über den TCP-Port 443 mit dem Azure Monitor-Dienst. Wenn der Computer für die Kommunikation über das Internet eine Firewall oder einen Proxyserver durchlaufen muss, sehen Sie sich die weiter unten angegebenen Anforderungen an, um sich mit der erforderlichen Netzwerkkonfiguration vertraut zu machen. Wenn Computer im Netzwerk aufgrund von IT-Sicherheitsrichtlinien keine Internetverbindung herstellen können, können Sie ein [Log Analytics-Gateway](gateway.md) einrichten und den Agent so konfigurieren, dass er die Verbindung mit Azure Monitor-Protokollen über das Gateway herstellt. Der Agent kann dann Konfigurationsinformationen empfangen und Daten senden, die je nach in Ihrem Arbeitsbereich aktivierten Datensammlungsregeln und Überwachungslösungen gesammelt werden.

![Kommunikationsdiagramm des Log Analytics-Agents](./media/log-analytics-agent/log-analytics-agent-01.png)


## <a name="network-firewall-requirements"></a>Netzwerkfirewallanforderungen
Die Aufstellung unten enthält die Proxy- und Firewall-Konfigurationsinformationen, die der Linux- und Windows-Agent benötigt, um mit Azure Monitor-Protokollen zu kommunizieren.  

|Agent-Ressource|Ports |Direction |Umgehung der HTTPS-Überprüfung|
|------|---------|--------|--------|   
|*.ods.opinsights.azure.com |Port 443 |Ausgehend|Ja |  
|*.oms.opinsights.azure.com |Port 443 |Ausgehend|Ja |  
|*.blob.core.windows.net |Port 443 |Ausgehend|Ja |  

Informationen zur Firewall, die für Azure Government erforderlich sind, finden Sie unter [Azure Government-Verwaltung](../../azure-government/documentation-government-services-monitoringandmanagement.md#azure-monitor-logs). 

Wenn Sie den Azure Automation Hybrid Runbook Worker zum Herstellen einer Verbindung mit dem Automatisierungsdienst bzw. die Registrierung bei diesem nutzen möchten, um Runbooks oder Verwaltungslösungen in Ihrer Umgebung zu verwenden, muss dieser Zugriff auf die Portnummer und die unter [Konfigurieren Ihres Netzwerks für den Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md#network-planning) beschriebenen URLs besitzen. 

Der Windows- und Linux-Agent unterstützt die Kommunikation mit Azure Monitor über einen Proxyserver oder ein Log Analytics-Gateway mithilfe des HTTPS-Protokolls.  Es wird sowohl die anonyme als auch die Standardauthentifizierung (Benutzername und Kennwort) unterstützt.  Für den Windows-Agent, der direkt mit dem Dienst verbunden ist, wird die Proxykonfiguration während der Installation oder [nach der Bereitstellung](agent-manage.md#update-proxy-settings) über die Systemsteuerung oder mit PowerShell angegeben.  

Für den Linux-Agent wird der Proxyserver während der Installation oder [nach der Installation](agent-manage.md#update-proxy-settings) durch Ändern der Konfigurationsdatei „proxy.conf“ angegeben.  Der Wert für die Proxykonfiguration des Linux-Agents weist die folgende Syntax auf:

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> Wenn Ihr Proxyserver keine Authentifizierung erfordert, muss der Linux-Agent trotzdem einen Pseudo-Benutzernamen und -Kennwort angeben. Dies kann ein beliebiger Benutzername oder ein beliebiges Kennwort sein.

|Eigenschaft| Beschreibung |
|--------|-------------|
|Protocol | https |
|user | Optionaler Benutzername für die Proxyauthentifizierung |
|password | Optionales Kennwort für die Proxyauthentifizierung |
|proxyhost | Adresse oder FQDN des Proxyservers oder Log Analytics-Gateways |
|port | Optionale Portnummer für den Proxyserver oder das Log Analytics-Gateway |

Beispiel: `https://user01:password@proxy01.contoso.com:30443`

> [!NOTE]
> Wenn Sie in Ihrem Kennwort Sonderzeichen wie „\@“ verwenden, erhalten Sie einen Proxyverbindungsfehler, da der Wert falsch analysiert wird.  Zur Umgehung dieses Problems codieren Sie das Kennwort in der URL mit einem Tool wie [URLDecode](https://www.urldecoder.org/).  



## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie [Datenquellen](agent-data-sources.md), um die Datenquellen zu verstehen, die für die Erfassung von Daten aus Ihrem Windows- oder Linux-System zur Verfügung stehen. 
* Erfahren Sie mehr über [Protokollabfragen](../log-query/log-query-overview.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten. 
* Erfahren Sie mehr über [Überwachungslösungen](../insights/solutions.md), die Azure Monitor um zusätzliche Funktionen erweitern und ebenfalls Daten für den Log Analytics-Arbeitsbereich sammeln.

