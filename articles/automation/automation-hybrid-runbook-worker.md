---
title: 'Azure Automation: Übersicht über Hybrid Runbook Worker'
description: Dieser Artikel enthält eine Übersicht über Hybrid Runbook Worker, mit denen Sie Runbooks auf Computern in Ihrem lokalen Rechenzentrum oder beim Cloudanbieter ausführen können.
services: automation
ms.subservice: process-automation
ms.date: 04/05/2019
ms.topic: conceptual
ms.openlocfilehash: 9305d0d6443c923c680af0d5fafc58887dadb902
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83835290"
---
# <a name="hybrid-runbook-worker-overview"></a>Übersicht über Hybrid Runbook Worker

Runbooks in Azure Automation haben möglicherweise keinen Zugriff auf Ressourcen in anderen Clouds oder in Ihrer lokalen Umgebung, da sie auf der Azure-Cloudplattform ausgeführt werden. Sie können mit dem Azure Automation-Feature „Hybrid Runbook Worker“ Runbooks direkt auf dem Computer ausführen, der die Rolle hostet, und außerhalb für Ressourcen in der Umgebung zur Verwaltung dieser lokalen Ressourcen. Runbooks werden in Azure Automation gespeichert und verwaltet und an einzelne oder mehrere zugewiesene Computer übermittelt.

Diese Funktionalität wird in der folgenden Abbildung veranschaulicht:

![Übersicht über Hybrid Runbook Worker](media/automation-hybrid-runbook-worker/automation.png)

Ein Hybrid Runbook Worker kann mit den Betriebssystemen Windows oder Linux ausgeführt werden. Für die Überwachung müssen Sie Azure Monitor und einen Log Analytics-Agent für das unterstützte Betriebssystem verwenden. Weitere Informationen finden Sie im [Überblick über die Überwachung in Microsoft Azure](automation-runbook-execution.md#azure-monitor).

Jeder Hybrid-Runbook-Worker ist ein Mitglied einer Hybrid-Runbook-Workergruppe, die Sie beim Installieren des Agents angeben. Eine Gruppe kann einen einzelnen Agent umfassen, aber für Hochverfügbarkeit können Sie mehrere Agents installieren. Jeder Computer kann einen Hybrid Worker hosten, der einem Azure Automation-Konto untersteht. 

Wenn Sie ein Runbook auf einen Hybrid Runbook Worker starten, geben Sie die Gruppe an, in der das Runbook ausgeführt werden soll. Jeder Worker in der Gruppe ruft Azure Automation ab, um festzustellen, ob Aufträge verfügbar sind. Wenn ein Auftrag verfügbar ist, übernimmt ihn der erste verfügbare Worker. Die Verarbeitungszeit der Auftragswarteschlange hängt vom Hybrid Worker-Hardwareprofil und der Auslastung ab. Sie können keinen bestimmten Worker angeben. 

Verwenden Sie anstelle einer [Azure-Sandbox](automation-runbook-execution.md#runbook-execution-environment) einen Hybrid Runbook Worker, da für diesen viele der [Grenzwerte](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits) für eine Sandbox bei Speicherplatz, Arbeitsspeicher und Netzwerksockets nicht gelten. Die Grenzwerte für einen Hybrid Worker beziehen sich lediglich auf die eigenen Ressourcen des Workers. 

> [!NOTE]
> Hybrid Runbook Worker unterliegen nicht der von der [gleichmäßigen Verteilung](automation-runbook-execution.md#fair-share) in einer Azure-Sandbox vorgegebenen Zeitbeschränkung. 

## <a name="hybrid-runbook-worker-installation"></a>Installation von Hybrid Runbook Workern

Der Vorgang zum Installieren eines Hybrid Runbook Workers ist vom Betriebssystem abhängig. In der folgenden Tabelle sind die Bereitstellungstypen definiert.

|Betriebssystem  |Bereitstellungstypen  |
|---------|---------|
|Windows     | [Automatisiert](automation-windows-hrw-install.md#automated-deployment)<br>[Manuell](automation-windows-hrw-install.md#manual-deployment)        |
|Linux     | [Python](automation-linux-hrw-install.md#install-a-linux-hybrid-runbook-worker)        |

Die empfohlene Installationsmethode besteht darin, mithilfe eines Azure Automation-Runbooks den Prozess zum Konfigurieren eines Windows-Computers vollständig zu automatisieren. Bei der zweiten Methode wird die Rolle Schritt für Schritt manuell installiert und konfiguriert. Bei Linux-Computern führen Sie zur Installation des Agents auf dem Computer ein Python-Skript aus.

## <a name="network-planning"></a><a name="network-planning"></a>Netzwerkplanung

Der Hybrid Runbook Worker benötigt Zugriff auf die in diesem Abschnitt angegebene Portnummer und die URLs, damit er eine Verbindung mit Azure Automation herstellen und sich bei dem Dienst registrieren kann. Der Worker benötigt zusätzlich Zugriff auf die [für den Log Analytics-Agent erforderlichen Ports und URLs](../azure-monitor/platform/agent-windows.md), um eine Verbindung mit dem Azure Monitor Log Analytics-Arbeitsbereich herstellen zu können.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Nachfolgend sind der Port und die URLs aufgeführt, die für den Hybrid Runbook Worker erforderlich sind:

* Port: Für ausgehenden Zugriff auf das Internet ist nur TCP 443 erforderlich
* Globale URL: *.azure-automation.net
* Globale URL von „US Gov Virginia“: *.azure-automation.us
* Agent-Dienst: https://\<Arbeitsbereichs-ID\>.agentsvc.azure-automation.net

Es wird empfohlen, beim Definieren von [Ausnahmen](automation-runbook-execution.md#exceptions) die aufgeführten Adressen zu verwenden. Für IP-Adressen können Sie die [IP-Bereiche des Microsoft Azure-Rechenzentrums](https://www.microsoft.com/en-us/download/details.aspx?id=56519) herunterladen. Diese Datei mit den jeweils aktuellen bereitgestellten Bereichen und allen anstehenden Änderungen an den IP-Adressbereichen wird wöchentlich veröffentlicht.

### <a name="dns-records-per-region"></a>DNS-Einträge pro Region

Wenn eines Ihrer Automation-Konten für eine bestimmte Region definiert ist, können Sie die Kommunikation der Hybrid Runbook Worker auf dieses regionale Rechenzentrum einschränken. Die folgende Tabelle enthält den DNS-Eintrag für jede Region.

| **Region** | **DNS-Eintrag** |
| --- | --- |
| Australien, Mitte |ac-jobruntimedata-prod-su1.azure-automation.net</br>ac-agentservice-prod-1.azure-automation.net |
| Australien (Osten) |ae-jobruntimedata-prod-su1.azure-automation.net</br>ae-agentservice-prod-1.azure-automation.net |
| Australien, Südosten |ase-jobruntimedata-prod-su1.azure-automation.net</br>ase-agentservice-prod-1.azure-automation.net |
| Kanada, Mitte |cc-jobruntimedata-prod-su1.azure-automation.net</br>cc-agentservice-prod-1.azure-automation.net |
| Indien, Mitte |cid-jobruntimedata-prod-su1.azure-automation.net</br>cid-agentservice-prod-1.azure-automation.net |
| USA (Ost) 2 |eus2-jobruntimedata-prod-su1.azure-automation.net</br>eus2-agentservice-prod-1.azure-automation.net |
| Japan, Osten |jpe-jobruntimedata-prod-su1.azure-automation.net</br>jpe-agentservice-prod-1.azure-automation.net |
| Nordeuropa |ne-jobruntimedata-prod-su1.azure-automation.net</br>ne-agentservice-prod-1.azure-automation.net |
| USA Süd Mitte |scus-jobruntimedata-prod-su1.azure-automation.net</br>scus-agentservice-prod-1.azure-automation.net |
| Südostasien |sea-jobruntimedata-prod-su1.azure-automation.net</br>sea-agentservice-prod-1.azure-automation.net|
| UK, Süden | uks-jobruntimedata-prod-su1.azure-automation.net</br>uks-agentservice-prod-1.azure-automation.net |
| US Government, Virginia | usge-jobruntimedata-prod-su1.azure-automation.us<br>usge-agentservice-prod-1.azure-automation.us |
| USA, Westen-Mitte | wcus-jobruntimedata-prod-su1.azure-automation.net</br>wcus-agentservice-prod-1.azure-automation.net |
| Europa, Westen |we-jobruntimedata-prod-su1.azure-automation.net</br>we-agentservice-prod-1.azure-automation.net |
| USA, Westen 2 |wus2-jobruntimedata-prod-su1.azure-automation.net</br>wus2-agentservice-prod-1.azure-automation.net |

Wenn Sie eine Liste mit IP-Adressen der Regionen anstelle ihrer Namen benötigen, können Sie die XML-Datei mit den [IP-Adressen der Azure-Rechenzentren](https://www.microsoft.com/download/details.aspx?id=41653) aus dem Microsoft Download Center herunterladen. Eine aktualisierte Datei mit den IP-Adressen wird wöchentlich veröffentlicht. 

Die Datei mit den IP-Adressen enthält die IP-Adressbereiche, die in den Microsoft Azure-Rechenzentren verwendet werden. Sie umfasst die jeweils aktuellen bereitgestellten Compute-, SQL- und Speicherbereiche sowie alle anstehenden Änderungen an den IP-Adressbereichen. In der Datei enthaltene neue Bereiche werden frühestens nach einer Woche in den Rechenzentren verwendet.

Sie sollten die neue Datei mit den IP-Adressen jede Woche herunterladen. Führen Sie damit dann eine Aktualisierung an Ihrem Standort durch, um in Azure ausgeführte Dienste ordnungsgemäß zu identifizieren. 

> [!NOTE]
> Wenn Sie Azure ExpressRoute verwenden, denken Sie daran, dass diese Datei mit den IP-Adressen zum Aktualisieren der BGP-Ankündigung (Border Gateway Protocol) von Azure-Bereichen jeweils in der ersten Woche des Monats verwendet wird.

### <a name="proxy-server-use"></a>Verwenden von Proxyservern

Wenn Sie einen Proxyserver für die Kommunikation zwischen Azure Automation und dem Log Analytics-Agent verwenden, müssen Sie sicherstellen, dass auf die entsprechenden Ressourcen zugegriffen werden kann. Das Zeitlimit für Anforderungen vom Hybrid Runbook Worker und von den Automation-Diensten beträgt 30 Sekunden. Nach drei Versuchen tritt bei einer Anforderung ein Fehler auf. 

### <a name="firewall-use"></a>Verwenden von Firewalls

Wenn Sie eine Firewall verwenden, um den Zugriff auf das Internet einzuschränken, müssen Sie die Firewall so konfigurieren, dass der Zugriff möglich ist. Wenn Sie das Log Analytics-Gateway als Proxy verwenden, achten Sie darauf, dass es für Hybrid Runbook Worker konfiguriert ist. Weitere Informationen finden Sie unter [Konfigurieren des Log Analytics-Gateways für Automation Hybrid Worker](https://docs.microsoft.com/azure/log-analytics/log-analytics-oms-gateway).

## <a name="update-management-on-hybrid-runbook-worker"></a>Updateverwaltung auf Hybrid Runbook Workern

Wenn die Azure Automation-[Updateverwaltung](automation-update-management.md) aktiviert ist, werden alle mit dem Log Analytics-Arbeitsbereich verbundenen Computer automatisch als Hybrid Runbook Worker konfiguriert. Jeder Worker kann Runbooks unterstützen, die für die Updateverwaltung bestimmt sind. 

Ein auf diese Weise konfigurierter Computer wird jedoch nicht bei Hybrid Runbook Worker-Gruppen registriert, die bereits in Ihrem Automation-Konto definiert wurden. Sie können den Computer einer Hybrid Runbook Worker-Gruppe hinzufügen, sofern Sie für die Updateverwaltung und die Mitgliedschaft in der Hybrid Runbook Worker-Gruppe dasselbe Konto verwenden. Diese Funktionalität wurde in Version 7.2.12024.0 von Hybrid Runbook Worker hinzugefügt.

### <a name="update-management-addresses-for-hybrid-runbook-worker"></a>Updateverwaltung-Adressen für Hybrid Runbook Worker

Neben den Standardadressen und -ports, die der Hybrid Runbook Worker erfordert, sind die Adressen in der folgenden Tabelle für die Updateverwaltung erforderlich. Für die Kommunikation mit diesen Adressen wird Port 443 verwendet.

|Azure – Öffentlich  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com     | *.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net | *.blob.core.usgovcloudapi.net|

## <a name="azure-automation-state-configuration-on-a-hybrid-runbook-worker"></a>Azure Automation State Configuration auf einem Hybrid Runbook Worker

Sie können [Azure Automation State Configuration](automation-dsc-overview.md) auf einem Hybrid Runbook Worker ausführen. Um die Konfiguration von Servern zu verwalten, die den Hybrid Runbook Worker unterstützen, müssen Sie die Server als DSC-Knoten hinzufügen. Weitere Informationen finden Sie unter [Aktivieren von Computern zur Verwaltung durch Azure Automation State Configuration](automation-dsc-onboarding.md).

## <a name="runbooks-on-a-hybrid-runbook-worker"></a>Runbooks auf einem Hybrid Runbook Worker

Möglicherweise verfügen Sie über Runbooks, die Ressourcen auf dem lokalen Computer verwalten oder für Ressourcen in der lokalen Umgebung ausgeführt werden, in der ein Hybrid Runbook Worker bereitgestellt ist. In diesem Fall können Sie sich entscheiden, Ihre Runbooks auf dem Hybrid Worker statt in einem Automation-Konto auszuführen. Runbooks, die auf einem Hybrid Runbook Worker ausgeführt werden, sind strukturell identisch mit denen, die Sie im Automation-Konto ausführen. Weitere Informationen finden Sie unter [Ausführen von Runbooks auf einem Hybrid Runbook Worker](automation-hrw-run-runbooks.md).

### <a name="hybrid-runbook-worker-jobs"></a>Hybrid-Runbook-Worker-Aufträge

Hybrid Runbook Worker-Aufträge werden unter Windows unter dem lokalen Konto **System** und unter Linux unter dem Konto [nxautomation](automation-runbook-execution.md#log-analytics-agent-for-linux) ausgeführt. Azure Automation behandelt Aufträge in Hybrid Runbook Workern etwas anders als Aufträge, die in Azure-Sandboxes ausgeführt werden. Weitere Informationen finden Sie unter [Ausführungsumgebung für Runbooks](automation-runbook-execution.md#runbook-execution-environment).

Wenn der Hostcomputer des Hybrid Runbook Workers neu gestartet wird, werden alle ausgeführten Runbookaufträge von vorn oder – im Fall von PowerShell-Workflow-Runbooks – beim letzten Prüfpunkt neu gestartet. Nachdem ein Runbookauftrag mehr als dreimal neu gestartet wurde, wird er angehalten.

### <a name="runbook-permissions-for-a-hybrid-runbook-worker"></a>Runbook-Berechtigungen für einen Hybrid Runbook Worker

Da sie auf Nicht-Azure-Ressourcen zugreifen, können Runbooks, die auf einem Hybrid Runbook Worker laufen, nicht den Authentifizierungsmechanismus verwenden, der normalerweise von Runbooks zur Authentifizierung bei Azure-Ressourcen verwendet wird. Ein Runbook stellt entweder eine eigene Authentifizierung für lokale Ressourcen bereit oder konfiguriert die Authentifizierung mithilfe von [verwalteten Identitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager). Alternativ hierzu können Sie auch ein ausführendes Konto angeben, um einen Benutzerkontext für alle Runbooks bereitzustellen.

## <a name="next-steps"></a>Nächste Schritte

* Um zu erfahren, wie Sie Ihre Runbooks für die Automatisierung von Prozessen in Ihrem lokalen Rechenzentrum oder in einer anderen Cloudumgebung konfigurieren, lesen Sie sich [Ausführen von Runbooks auf einem Hybrid Runbook Worker](automation-hrw-run-runbooks.md) durch.
* Informationen zum Behandeln von Problemen mit Ihren Hybrid Runbook Workern finden Sie unter [Problembehandlung von Hybrid Runbook Workern](troubleshoot/hybrid-runbook-worker.md#general).