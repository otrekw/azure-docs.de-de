---
title: 'Azure Automation-Updateverwaltung: Übersicht'
description: Dieser Artikel bietet eine Übersicht über die Updateverwaltungsfunktion, die Updates für Ihre Windows- und Linux-Computer implementiert.
services: automation
ms.subservice: update-management
ms.date: 05/22/2020
ms.topic: conceptual
ms.openlocfilehash: 4c27fa26b19b870f90f2e7d6ecd34f1f3c083323
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2020
ms.locfileid: "83847327"
---
# <a name="update-management-overview"></a>Übersicht über die Updateverwaltung

Mithilfe der Updateverwaltung in Azure Automation können Sie Betriebssystemupdates für Ihre Windows- und Linux-Computer in Azure, in lokalen Umgebungen und in anderen Cloudumgebungen verwalten. Sie können den Status der verfügbaren Updates auf allen Agent-Computern schnell auswerten und die Installation der für den Server erforderlichen Updates initiieren.

Die Updateverwaltung für virtuelle Computer können Sie auf die folgenden Arten aktivieren:

* Aus Ihrem [Azure Automation-Konto](automation-onboard-solutions-from-automation-account.md) für einen oder mehrere Azure-Computer.
* Manuell für Nicht-Azure-Computer.
* Für einen einzelnen virtuellen Azure-Computer auf der Seite für virtuelle Computer im Azure-Portal. Dieses Szenario steht für virtuelle Computer unter [Linux](../virtual-machines/linux/tutorial-config-management.md#enable-update-management) oder [Windows](../virtual-machines/windows/tutorial-config-management.md#enable-update-management) zur Verfügung.
* Für [mehrere Azure-VMs](manage-update-multi.md), indem Sie diese auf der Seite für virtuelle Computer im Azure-Portal auswählen.

> [!NOTE]
> Die Updateverwaltung erfordert, dass ein Log Analytics Arbeitsbereich mit Ihrem Automation-Konto verknüpft wird. Eine aktuelle Liste der unterstützten Regionen finden Sie unter [Arbeitsbereichzuordnungen in Azure](how-to/region-mappings.md). Die Regionszuordnungen haben keine Auswirkung auf die Möglichkeit, virtuelle Computer in einer anderen Region als der Ihres Automation-Kontos zu verwalten.

Eine [Azure Resource Manager-Vorlage](automation-update-management-deploy-template.md) ist verfügbar, die Ihnen bei der Bereitstellung der Updateverwaltung in einem neuen oder vorhandenen Automation-Konto und Log Analytics-Arbeitsbereich in Ihrem Abonnement helfen soll.

> [!NOTE]
> Auf einem Computer, der mit der Updateverwaltung konfiguriert ist, können Sie keine benutzerdefinierten Skripts aus Azure Automation ausführen. Auf diesem Computer kann nur das von Microsoft signierte Updateskript ausgeführt werden. 

## <a name="about-update-management"></a>Informationen zur Updateverwaltung

Computer, die mit der Updateverwaltung verwaltet werden, verwenden folgende Konfigurationen, um Bewertungen und Updatebereitstellungen durchzuführen:

* Log Analytics-Agent für Windows oder Linux
* PowerShell Desired State Configuration (DSC) für Linux
* Automation Hybrid Runbook Worker
* Microsoft Update oder Windows Server Update Services (WSUS) für Windows-Computer

Das folgende Diagramm veranschaulicht, wie die Updateverwaltung alle verbundenen Windows Server- und Linux-Computer eines Arbeitsbereichs bewertet und Sicherheitsupdates darauf anwendet:

![Updateverwaltungs-Workflow](./media/automation-update-management/update-mgmt-updateworkflow.png)

Die Updateverwaltung kann für das systeminterne Bereitstellen von Computern in mehreren Abonnements im selben Mandanten verwendet werden.

Nachdem ein Paket veröffentlicht wurde, dauert es zwei bis drei Stunden, bis der Patch für Linux-Computer für die Bewertung angezeigt wird. Bei Windows-Computern dauert es 12 bis 15 Stunden, bis der Patch nach der Veröffentlichung für die Bewertung angezeigt wird.

Nachdem ein Computer einen Scanvorgang abgeschlossen hat, um die Konformität für das Update zu überprüfen, leitet der Agent die Informationen gesammelt an Azure Monitor-Protokolle weiter. Auf einem Windows-Computer wird der Konformitätsscan standardmäßig alle 12 Stunden ausgeführt.

Darüber hinaus wird der Update-Konformitätsscan innerhalb von 15 Minuten nach dem Neustart des Log Analytics-Agents sowie vor und nach der Updateinstallation gestartet.

Für einen Linux-Computer wird der Konformitätsscan standardmäßig jede Stunde durchgeführt. Wenn der Log Analytics-Agent neu gestartet wird, wird ein Konformitätsscan innerhalb von 15 Minuten gestartet.

Die Updateverwaltung meldet basierend auf der für die Synchronisierung konfigurierten Quelle, wie aktuell der Computer ist. Wenn der Windows-Computer für das Senden von Meldungen an WSUS konfiguriert ist, können sich die Ergebnisse von den angezeigten Microsoft Update-Ergebnissen unterscheiden. Dies hängt davon ab, wann WSUS zuletzt mit Microsoft Update synchronisiert wurde. Dasselbe gilt für Linux-Computer, die für Meldungen an ein lokales Repository konfiguriert sind (anstatt an ein öffentliches Repository).

> [!NOTE]
> Damit Meldungen an den Dienst ordnungsgemäß erfolgen können, erfordert die Updateverwaltung, dass bestimmte URLs und Ports aktiviert werden. Weitere Informationen zu diesen Anforderungen finden Sie unter [Netzwerkkonfiguration](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker#network-planning).

Sie können Softwareupdates auf Computern bereitstellen und installieren, für die die Updates erforderlich sind, indem Sie einen geplante Bereitstellung erstellen. Updates, die als optional klassifiziert sind, sind im Bereitstellungsumfang von Windows-Computern nicht enthalten. Nur erforderliche Updates sind im Bereitstellungsumfang enthalten.

Die geplante Bereitstellung definiert, welche Zielcomputer die anwendbaren Updates erhalten. Hierzu werden entweder explizit bestimmte Computer angegeben, oder es wird eine [Computergruppe](https://docs.microsoft.com/azure/azure-monitor/platform/computer-groups) ausgewählt, die auf Protokollsuchvorgängen einer bestimmten Gruppe von Computern basiert (oder auf einer [Azure-Abfrage](automation-update-management-query-logs.md), die virtuelle Azure-Computer auf der Grundlage bestimmter Kriterien dynamisch auswählt). Diese Gruppen unterscheiden sich von der [Bereichskonfiguration](https://docs.microsoft.com/azure/azure-monitor/insights/solution-targeting), mit der die Zielcomputer gesteuert werden, die die Konfiguration erhalten sollen, um die Updateverwaltung zu aktivieren. Dies verhindert, dass sie die Updatekonformität prüfen und melden und genehmigte erforderliche Updates installieren.

Beim Definieren einer Bereitstellung geben Sie außerdem einen Zeitplan an, um einen Zeitraum zu genehmigen und festzulegen, in dem Updates installiert werden dürfen. Dieser Zeitraum wird das Wartungsfenster bezeichnet. 20 Minuten des Wartungsfensters sind für Neustarts reserviert (sofern ein Neustart erforderlich ist und Sie die entsprechende Neustartoption ausgewählt haben). Wenn das Patchen länger dauert als erwartet und im Wartungsfenster weniger als 20 Minuten verbleiben, wird kein Neustart durchgeführt.

Updates werden mit Runbooks in Azure Automation installiert. Sie können diese Runbooks nicht anzeigen, und für diese Runbooks ist keine Konfiguration erforderlich. Bei der Erstellung einer Updatebereitstellung wird ein Zeitplan erstellt, nach dem für die einbezogenen Computer zur angegebenen Zeit ein Masterrunbook für das Update gestartet wird. Das Masterrunbook startet ein untergeordnetes Runbook für jeden Agent, um die erforderlichen Updates zu installieren.

Wenn die Datums- bzw. Uhrzeitangabe der Updatebereitstellung erreicht ist, führen die Zielcomputer die Bereitstellung parallel aus. Vor der Installation wird ein Scan ausgeführt, um sicherzustellen, dass die Updates weiterhin erforderlich sind. Für WSUS-Clientcomputer tritt ein Fehler bei der Updatebereitstellung auf, wenn die Updates in WSUS nicht genehmigt wurden.
Die Registrierung eines Computers für die Updateverwaltung in mehreren Log Analytics-Arbeitsbereichen (auch als „Multi-Homing“ bezeichnet) wird nicht unterstützt.

## <a name="clients"></a>Clients

### <a name="supported-client-types"></a>Unterstützte Clienttypen

Die folgende Tabelle enthält die unterstützten Betriebssysteme für Updatebewertungen. Patchen erfordert einen Hybrid Runbook Worker. Informationen zu den Anforderungen für Hybrid Runbook Worker finden Sie unter [Bereitstellen eines Windows Hybrid Runbook Workers](automation-windows-hrw-install.md) und [Bereitstellen eines Linux Hybrid Runbook Workers](automation-linux-hrw-install.md).

> [!NOTE]
> Die Updatebewertung von Linux-Computern wird nur in bestimmten Regionen unterstützt, die in der [Zuordnungstabelle](https://docs.microsoft.com/azure/automation/how-to/region-mappings#supported-mappings) für Automation-Konto und Log Analytics-Arbeitsbereich aufgeführt sind. 

|Betriebssystem  |Notizen  |
|---------|---------|
|Windows Server 2019 (Datacenter/Datacenter Core/Standard)<br><br>Windows Server 2016 (Datacenter/Datacenter Core/Standard)<br><br>Windows Server 2012 R2 (Datacenter/Standard)<br><br>Windows Server 2012 || 
|Windows Server 2008 R2 (RTM und SP1 Standard)| Die Updateverwaltung unterstützt nur Bewertungen für dieses Betriebssystem. Patchen wird nicht unterstützt, da der [Hybrid Runbook Worker](automation-windows-hrw-install.md) für Windows Server 2008 R2 nicht unterstützt wird. |
|CentOS 6 (x86/x64) und 7 (x64)      | Linux-Agents erfordern Zugriff auf ein Updaterepository. Für klassifizierungsbasiertes Patchen muss `yum` Sicherheitsdaten zurückgeben, über die CentOS in den RTM-Releases nicht verfügt. Weitere Informationen zu klassifizierungsbasiertem Patching unter CentOS finden Sie unter [Lösung für die Updateverwaltung in Azure](automation-view-update-assessments.md#linux-2).          |
|Red Hat Enterprise 6 (x86/x64) und 7 (x64)     | Linux-Agents erfordern Zugriff auf ein Updaterepository.        |
|SUSE Linux Enterprise Server 11 (x86/x64) und 12 (x64)     | Linux-Agents erfordern Zugriff auf ein Updaterepository.        |
|Ubuntu 14.04 LTS, 16.04 LTS und 18.04 (x86/x64)      |Linux-Agents erfordern Zugriff auf ein Updaterepository.         |

> [!NOTE]
> VM-Skalierungsgruppen von Azure können über die Updateverwaltung verwaltet werden. Die Updateverwaltung arbeitet nicht mit dem Basisimage, sondern mit den Instanzen selbst. Die Updates müssen inkrementell geplant werden, damit nicht alle VM-Instanzen auf einmal aktualisiert werden. Sie können Knoten für VM-Skalierungsgruppen hinzufügen. Eine entsprechende Anleitung finden Sie unter [Hinzufügen eines Nicht-Azure-Computers zu Änderungsnachverfolgung und Bestand](automation-tutorial-installed-software.md#add-a-non-azure-machine-to-change-tracking-and-inventory).

### <a name="unsupported-client-types"></a>Nicht unterstützte Clienttypen

Die folgende Tabelle enthält nicht unterstützte Betriebssysteme:

|Betriebssystem  |Notizen  |
|---------|---------|
|Windows-Client     | Clientbetriebssysteme (z.B. Windows 7 und Windows 10) werden nicht unterstützt.<br> Für Azure Windows Virtual Desktop (WVD) ist die empfohlene Methode<br> zum Verwalten von Updates [Windows Update for Business](https://docs.microsoft.com/windows/deployment/update/waas-manage-updates-wufb) für die Patchverwaltung von Windows 10-Clientcomputern. |
|Windows Server 2016 Nano Server     | Wird nicht unterstützt.       |
|Azure Kubernetes Service-Knoten | Wird nicht unterstützt. Verwenden Sie den unter [Anwenden von Sicherheits- und Kernelupdates auf Linux-Knoten in Azure Kubernetes Service (AKS)](../aks/node-updates-kured.md) beschriebenen Patchprozess.|

### <a name="client-requirements"></a>Clientanforderungen

Der folgende Abschnitt enthält Informationen zu betriebssystemspezifischen Clientanforderungen. Weitere Informationen finden Sie unter [Netzwerkplanung](#ports).

#### <a name="windows"></a>Windows

Windows-Agents müssen für die Kommunikation mit einem WSUS-Server konfiguriert sein oder erfordern Zugriff auf Microsoft Update. Informationen zur Installation des Log Analytics-Agents für Windows finden Sie unter [Verbinden von Windows-Computern mit Azure Monitor](../log-analytics/log-analytics-windows-agent.md).

Sie können die Updateverwaltung zusammen mit Microsoft Endpoint Configuration Manager verwenden. Weitere Informationen zu Integrationsszenarien finden Sie unter [Integrieren der Updateverwaltung in Windows Endpoint Configuration Manager](updatemgmt-mecmintegration.md). Der [Log Analytics-Agent für Windows](../azure-monitor/platform/agent-windows.md) ist für Windows-Server erforderlich, die von Sites in Ihrer Configuration Manager-Umgebung verwaltet werden. 

Über den Azure Marketplace bereitgestellte virtuelle Windows-Computer sind standardmäßig so konfiguriert, dass sie automatisch Updates vom Windows Update-Dienst erhalten. Dieses Verhalten ändert sich nicht, wenn Sie Ihrem Arbeitsbereich virtuelle Windows-Computer hinzufügen. Wenn Sie Updates mithilfe der Updateverwaltung nicht aktiv verwalten, gilt das Standardverhalten (Updates werden automatisch angewendet).

> [!NOTE]
> Sie können die Gruppenrichtlinie so ändern, dass Computerneustarts nur vom Benutzer durchgeführt werden können, nicht aber vom System. Verwaltete Computer bleiben ggf. hängen, wenn die Updateverwaltung nicht berechtigt ist, den Computer ohne manuelle Benutzerinteraktion neu zu starten. Weitere Informationen finden Sie unter [Konfigurieren der Gruppenrichtlinieneinstellungen für automatische Updates](https://docs.microsoft.com/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates).

#### <a name="linux"></a>Linux

Für Linux erfordert der Computer Zugriff auf ein Updaterepository, entweder privat oder öffentlich. Für die Interaktion mit der Updateverwaltung ist TLS 1.1 oder TLS 1.2 erforderlich. Die Updateverwaltung unterstützt keinen Log Analytics-Agent für Linux, der für die Berichterstattung an mehr als einen Log Analytics-Arbeitsbereich konfiguriert ist. Auf dem Computer muss außerdem Python 2.x installiert sein.

> [!NOTE]
> Die Updatebewertung von Linux-Computern wird nur in bestimmten Regionen unterstützt. Informationen finden Sie in der [Zuordnungstabelle](https://docs.microsoft.com/azure/automation/how-to/region-mappings#supported-mappings) für Automation-Konto und Log Analytics-Arbeitsbereich. 

Weitere Informationen zum Installieren des Log Analytics-Agents für Linux und zum Herunterladen der aktuellen Version finden Sie unter [Log Analytics Agent for Linux (Log Analytics-Agent für Linux)](../azure-monitor/platform/agent-linux.md). 

Virtuelle Computer, die auf der Grundlage der über Azure Marketplace erhältlichen On-Demand-RHEL-Images (Red Hat Enterprise Linux) erstellt werden, werden für den Zugriff auf die in Azure bereitgestellte [Red Hat-Updateinfrastruktur (RHUI)](../virtual-machines/workloads/redhat/redhat-rhui.md) registriert. Alle anderen Linux-Distributionen müssen über das jeweilige Onlinedateirepository der Distributionen mithilfe der von der jeweiligen Distribution unterstützten Methoden aktualisiert werden.

## <a name="permissions"></a>Berechtigungen

Zum Erstellen und Verwalten von Updatebereitstellungen benötigen Sie bestimmte Berechtigungen. Weitere Informationen zu diesen Berechtigungen finden Sie unter [Rollenbasierter Zugriff: Updateverwaltung](automation-role-based-access-control.md#update-management-permissions).

## <a name="update-management-components"></a>Updateverwaltungskomponenten

Die Updateverwaltung verwendet die in diesem Abschnitt beschriebenen Ressourcen. Diese Ressourcen werden automatisch Ihrem Automation-Konto hinzugefügt, wenn Sie die Updateverwaltung aktivieren.

### <a name="hybrid-runbook-worker-groups"></a>Hybrid-Runbook-Workergruppen

Nachdem Sie die Updateverwaltung aktiviert haben, werden alle direkt mit dem Log Analytics-Arbeitsbereich verbundenen Windows-Computer automatisch als Hybrid Runbook Worker konfiguriert, um die Runbooks zu unterstützen, die die Updateverwaltung unterstützen.

Jeder von der Updateverwaltung verwaltete Windows-Computer wird im Bereich „Hybrid Worker-Gruppen“ als Hybrid Worker-Systemgruppe für das Automation-Konto aufgeführt. Die Gruppen verwenden die Benennungskonvention `Hostname FQDN_GUID`. Es ist nicht möglich, diese Gruppen mit Runbooks in Ihrem Konto zu erreichen. Entsprechende Versuche sind nicht erfolgreich. Diese Gruppen sind nur für die ausschließliche Unterstützung der Updateverwaltung bestimmt.

Sie können die Windows-Computer einer Hybrid Runbook Worker-Gruppe in Ihrem Automation-Konto hinzufügen, um Automation-Runbooks zu unterstützen, wenn Sie für die Updateverwaltung und die Mitgliedschaft in der Hybrid Runbook Worker-Gruppe dasselbe Konto verwenden. Diese Funktionalität wurde in Version 7.2.12024.0 des Hybrid Runbook Worker hinzugefügt.

### <a name="management-packs"></a>Management Packs

Wenn Ihre Operations Manager-Verwaltungsgruppe [mit einem Log Analytics-Arbeitsbereich verbunden](../azure-monitor/platform/om-agents.md) ist, werden in Operations Manager die folgenden Management Packs installiert. Diese Management Packs werden auch für die Updateverwaltung auf direkt verbundenen Windows-Computern installiert. Für diese Management Packs fällt kein Konfigurations- oder Verwaltungsaufwand an.

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Update Deployment MP

> [!NOTE]
> Wenn Sie über eine Operations Manager 1807- oder 2019-Verwaltungsgruppe verfügen, die mit einem Log Analytics-Arbeitsbereich verbunden ist, und in der Verwaltungsgruppe Agents für die Erfassung von Protokolldaten konfiguriert sind, müssen Sie den Parameter `IsAutoRegistrationEnabled` außer Kraft setzen und in der Regel **Microsoft.IntelligencePacks.AzureAutomation.HybridAgent.Init** auf „True“ festlegen.

Weitere Informationen zu Updates der Management Packs finden Sie unter [Herstellen einer Verbindung zwischen Operations Manager und Azure Monitor-Protokollen](../azure-monitor/platform/om-agents.md).

> [!NOTE]
> Damit die Updateverwaltung Computer mit dem Log Analytics-Agent vollständig verwalten kann, müssen Sie auf den Log Analytics-Agent für Windows oder den Log Analytics-Agent für Linux aktualisieren. Informationen zum Aktualisieren des Agents finden Sie unter [Aktualisieren eines Operations Manager-Agents](https://docs.microsoft.com/system-center/scom/deploy-upgrade-agents). In Umgebungen, in denen Operations Manager verwendet wird, muss mindestens System Center Operations Manager 2012 R2 UR14 ausgeführt werden.

## <a name="data-collection"></a>Datensammlung

### <a name="supported-sources"></a>Unterstützte Quellen

In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die von der Updateverwaltung unterstützt werden:

| Verbundene Quelle | Unterstützt | BESCHREIBUNG |
| --- | --- | --- |
| Windows-Agents |Ja |Die Updateverwaltung sammelt Informationen zu Systemupdates von Windows-Agents und startet dann die Installation von erforderlichen Updates. |
| Linux-Agents |Ja |Die Updateverwaltung sammelt Informationen zu Systemupdates von Linux-Agents und startet dann die Installation von erforderlichen Updates für unterstützte Distributionen. |
| Operations Manager-Verwaltungsgruppe |Ja |Die Updateverwaltung sammelt Informationen zu Systemupdates von Agents in einer verbundenen Verwaltungsgruppe.<br/><br/>Es ist keine direkte Verbindung zwischen dem Operations Manager-Agent und Azure Monitor-Protokollen erforderlich. Daten werden von der Verwaltungsgruppe an den Log Analytics-Arbeitsbereich weitergeleitet. |

### <a name="collection-frequency"></a>Sammlungshäufigkeit

Die Updateverwaltung überprüft verwaltete Computer auf Daten mithilfe der folgenden Regeln. Es kann zwischen 30 Minuten und 6 Stunden dauern, bis im Dashboard aktualisierte Daten von verwalteten Computern angezeigt werden.

* Für jeden Windows-Computer: Die Updateverwaltung führt zweimal am Tag eine Überprüfung jedes Computers durch. Alle 15 Minuten fragt sie bei der Windows-API den letzten Updatezeitpunkt ab, um zu ermitteln, ob sich der Status geändert hat. Wenn sich der Status geändert hat, startet die Updateverwaltung eine Konformitätsprüfung.

* Für jeden Linux-Computer: Die Updateverwaltung führt jede Stunde eine Überprüfung durch.

Die durchschnittliche Datennutzung von Azure Monitor-Protokollen für einen Computer mit Updateverwaltung beträgt etwa 25 MB pro Monat. Dieser Wert ist lediglich eine Schätzung und kann sich abhängig von Ihrer Umgebung ändern. Es empfiehlt sich, die Umgebung zu überwachen, um die exakte Nutzung nachzuverfolgen. Weitere Informationen zur Analyse der Datenverwendung finden Sie unter [Verwalten von Nutzung und Kosten](../azure-monitor/platform/manage-cost-storage.md).

## <a name="network-planning"></a><a name="ports"></a>Netzwerkplanung

Die folgenden Adressen sind speziell für die Updateverwaltung erforderlich. Die Kommunikation mit diesen Adressen erfolgt über Port 443.

|Azure – Öffentlich  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com    | *.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net | *.blob.core.usgovcloudapi.net|
|*.azure-automation.net | *.azure-automation.us|

Bei Windows-Computern müssen Sie auch Datenverkehr zu allen Endpunkten zulassen, die für Windows Update erforderlich sind. Sie finden eine aktualisierte Liste der erforderlichen Endpunkte unter [Probleme im Zusammenhang mit HTTP/Proxy](/windows/deployment/update/windows-update-troubleshooting#issues-related-to-httpproxy). Wenn Sie über einen lokalen [Windows Update-Server](/windows-server/administration/windows-server-update-services/plan/plan-your-wsus-deployment) verfügen, müssen Sie auch Datenverkehr zu dem in Ihrem [WSUS-Schlüssel](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry) angegebenen Server zulassen.

Informationen zu den erforderlichen Endpunkten für Red Hat Linux-Computer finden Sie unter [IP-Adressen der Server für die RHUI-Inhaltsübermittlung](../virtual-machines/workloads/redhat/redhat-rhui.md#the-ips-for-the-rhui-content-delivery-servers). Informationen zu anderen Linux-Distributionen finden Sie in der jeweiligen Dokumentation des Anbieters.

Weitere Informationen zu Ports, die für den Hybrid Runbook Worker erforderlich sind, finden Sie unter [Updateverwaltungsadressen für Hybrid Runbook Worker](automation-hybrid-runbook-worker.md#update-management-addresses-for-hybrid-runbook-worker).

Es wird empfohlen, beim Definieren von Ausnahmen die aufgeführten Adressen zu verwenden. Für IP-Adressen können Sie [IP-Bereiche des Microsoft Azure-Rechenzentrums](https://www.microsoft.com/download/details.aspx?id=41653) herunterladen. Diese Datei wird wöchentlich aktualisiert und enthält die aktuell bereitgestellten Bereiche sowie alle anstehenden Änderungen für die IP-Adressbereiche.

Gehen Sie wie unter [Verbinden von Computern ohne Internetzugriff über das Log Analytics-Gateway in Azure Monitor](../azure-monitor/platform/gateway.md) beschrieben vor, um Computer ohne Internetzugriff zu konfigurieren.

## <a name="update-classifications"></a>Updateklassifizierungen

In der folgenden Tabelle sind die Klassifizierungen definiert, die von der Updateverwaltung für Windows-Updates unterstützt werden. 

|Klassifizierung  |BESCHREIBUNG  |
|---------|---------|
|Kritische Updates     | Ein Update für ein bestimmtes Problem, mit dem ein entscheidender, nicht sicherheitsrelevanter Fehler behoben wird.        |
|Sicherheitsupdates     | Ein Update für ein produktspezifisches, sicherheitsrelevantes Problem.        |
|Updaterollups     | Eine kumulative Gruppe von Hotfixes, die zur Vereinfachung der Bereitstellung gebündelt sind.        |
|Feature Packs     | Neue Produktfeatures, die nicht im Rahmen eines Produktreleases verteilt werden.        |
|Service Packs     | Eine kumulative Gruppe von Hotfixes, die auf eine Anwendung angewendet werden.        |
|Definitionsupdates     | Ein Update für virenbehaftete oder andere Definitionsdateien.        |
|Tools     | Ein Hilfsprogramm oder Feature, mit dem mindestens eine Aufgabe ausgeführt werden kann.        |
|Aktualisierungen     | Ein Update für eine Anwendung oder Datei, die zurzeit installiert ist.        |

In der nächsten Tabelle sind die für Linux-Updates unterstützten Klassifizierungen definiert.

|Klassifizierung  |BESCHREIBUNG  |
|---------|---------|
|Kritische Updates und Sicherheitsupdates     | Updates für ein spezielles oder produktspezifisches, sicherheitsrelevantes Problem.         |
|Andere Updates     | Alle anderen Updates, die nicht kritisch sind oder bei denen es sich nicht um Sicherheitsupdates handelt.        |

Für Linux kann die Updateverwaltung bei der Anzeige von Bewertungsdaten dank der Datenanreicherung in der Cloud zwischen kritischen Updates und Sicherheitsupdates in der Cloud unterscheiden. Für das Patchen verwendet die Updateverwaltung Klassifizierungsdaten, die auf dem Computer verfügbar sind. Im Gegensatz zu anderen Distributionen verfügt CentOS in der RTM-Version standardmäßig nicht über diese Informationen. Wenn Sie CentOS-Computer so konfiguriert haben, dass für den folgenden Befehl Sicherheitsdaten zurückgegeben werden, kann die Updateverwaltung basierend auf Klassifizierungen Patchvorgänge ausführen.

```bash
sudo yum -q --security check-update
```

Aktuell steht keine unterstützte Methode zur Verfügung, mit der unter CentOS eine native Verfügbarkeit von Klassifizierungsdaten implementiert werden kann. Für Kunden, die diese Funktion möglicherweise selbst implementiert haben, wird derzeit nur Support nach bestem Wissen bereitgestellt. 

Sie müssen das Plug-In „yum-security“ installieren, um Updates in Version 6 von Red Hat Enterprise zu klassifizieren. Unter Red Hat Enterprise Linux 7 ist das Plug-In bereits Teil von yum selbst, und Sie müssen also nichts installieren. Weitere Informationen finden Sie im folgenden [Knowledge-Artikel](https://access.redhat.com/solutions/10021) zu Red Hat.

## <a name="integrate-update-management-with-configuration-manager"></a>Integrieren der Updateverwaltung in Configuration Manager

Kunden, die in Microsoft Endpoint Configuration Manager investiert haben, um PCs, Server und mobile Geräte zu verwalten, profitieren auch von der Leistungsstärke und dem Funktionsumfang von Configuration Manager bei der Verwaltung von Softwareupdates. Informationen, wie Sie die Updateverwaltung in Configuration Manager integrieren, finden Sie unter [Integrieren der Updateverwaltung in Windows Endpoint Configuration Manager](updatemgmt-mecmintegration.md).

## <a name="third-party-updates-on-windows"></a>Drittanbieterupdates unter Windows

In der Updateverwaltung wird das lokal konfigurierte Updaterepository verwendet, um unterstützte Windows-Systeme zu aktualisieren, entweder WSUS oder Windows Update. Mit Tools wie [System Center Updates Publisher](https://docs.microsoft.com/configmgr/sum/tools/updates-publisher) können Sie benutzerdefinierte Updates mit WSUS importieren und veröffentlichen. Dadurch kann die Updateverwaltung Computer, auf denen Configuration Manager als Updaterepository verwendet wird, mit Software von Drittanbietern aktualisieren. Informationen zum Konfigurieren von Updates Publisher finden Sie unter [Installieren von Updates Publisher](https://docs.microsoft.com/configmgr/sum/tools/install-updates-publisher).

## <a name="enable-update-management"></a>Aktivieren der Updateverwaltung

Eine [Azure Resource Manager-Vorlage](automation-update-management-deploy-template.md) ist verfügbar, die Ihnen bei der Bereitstellung der Updateverwaltung in einem neuen oder vorhandenen Automation-Konto und Azure Monitor Log Analytics-Arbeitsbereich in Ihrem Abonnement helfen soll. Sie konfiguriert nicht den Bereich der Computer, die verwaltet werden sollen. Dies erfolgt als gesonderter Schritt nach der Verwendung der Vorlage.

Hier finden Sie die Möglichkeiten, wie Sie die Updateverwaltung aktivieren und zu verwaltende Computer auswählen können:

* [Über einen virtuellen Computer](automation-onboard-solutions-from-vm.md)
* [Über Durchsuchen mehrerer Computer](automation-onboard-solutions-from-browse.md)
* [Aus einem Azure Automation-Konto](automation-onboard-solutions.md)

## <a name="next-steps"></a>Nächste Schritte

* Ausführliche Informationen zur Arbeit mit der Updateverwaltung finden Sie unter [Verwalten von Updates und Patches für Ihre Azure-VMs](automation-tutorial-update-management.md).

* Lesen Sie häufig gestellte Fragen zur Updateverwaltung unter [Häufig gestellte Fragen zu Azure Automation](automation-faq.md).
