---
title: Azure-Sicherheitsbaseline für Batch
description: Azure-Sicherheitsbaseline für Batch
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 02/22/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 4094ace5772b99c646a31d6a4722bbb080fbfff7
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77589653"
---
# <a name="azure-security-baseline-for-batch"></a>Azure-Sicherheitsbaseline für Batch

Die Azure-Sicherheitsbaseline für Batch enthält Empfehlungen, mit deren Hilfe Sie den Sicherheitsstatus Ihrer Bereitstellung verbessern können.

Die Baseline für diese Dienste wird aus [Azure Security Benchmark-Version 1.0](https://docs.microsoft.com/azure/security/benchmarks/overview) abgeleitet, die Empfehlungen darüber bietet, wie Sie Ihre Cloudlösungen in Azure mithilfe unserer bewährten Methoden sichern können.

Weitere Informationen finden Sie unter [Übersicht über Azure-Sicherheitsbaselines](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Netzwerksicherheit

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Netzwerksicherheit](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1: Schützen von Ressourcen mithilfe von Netzwerksicherheitsgruppen oder Azure Firewall in Virtual Network

**Leitfaden**: Bereitstellen von Azure Batch-Pools im virtuellen Netzwerk. Um die sichere Kommunikation zwischen Computeknoten und VMs oder lokalen Netzwerken zu ermöglichen, können Sie den Pool in einem Subnetz eines virtuellen Azure-Netzwerks bereitstellen. Durch die Bereitstellung Ihres Pools innerhalb eines virtuellen Netzwerks erhalten Sie außerdem die Kontrolle über die Netzwerksicherheitsgruppe (NSG), die zur Sicherung der Netzwerkschnittstellen (NIC) der einzelnen Knoten sowie des Subnetzes verwendet wird. Konfigurieren Sie die NSG so, dass Datenverkehr von nur vertrauenswürdigen IP-Adressen/Speicherorten im Internet zugelassen wird.


So erstellen Sie einen Azure Batch-Pool in einem virtuellen Netzwerk:

https://docs.microsoft.com/azure/batch/batch-virtual-network

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2: Überwachen und Protokollieren der Konfiguration und des Datenverkehrs von VNETs, Subnetzen und Netzwerkkarten (NICs)

**Leitfaden**: Verwenden Sie Azure Security Center, und setzen Sie die Netzwerkschutzempfehlungen im Zusammenhang mit dem virtuellen Netzwerk bzw. der Netzwerksicherheitsgruppe (NSG) um, das oder die Ihrem Batch-Pool zugeordnet ist. Aktivieren Sie die Flussprotokolle für die NSG, die zum Schutz Ihres Batch-Pools verwendet wird, und senden Sie Protokolle an ein Azure Storage-Konto für die Überwachung des Datenverkehrs. Sie können auch NSG-Flussprotokolle an einen Azure Log Analytics-Arbeitsbereich senden und Azure Traffic Analytics verwenden, um Einblicke in den Datenverkehrsfluss in Ihrer Azure-Cloud bereitzustellen. Einige Vorteile von Azure Traffic Analytics sind die Möglichkeit, die Netzwerkaktivität zu visualisieren und Hotspots zu erkennen, Sicherheitsbedrohungen zu erkennen, Datenverkehrsflussmuster zu verstehen und Netzwerkfehlkonfigurationen zu ermitteln.


Aktivieren der NSG-Flussprotokolle:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal


So aktivieren Sie Traffic Analytics:

https://docs.microsoft.com/azure/network-watcher/traffic-analytics


Grundlegendes zu der von Azure Security Center bereitgestellten Netzwerksicherheit:

https://docs.microsoft.com/azure/security-center/security-center-network-recommendations

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="13-protect-critical-web-applications"></a>1.3: Schützen kritischer Webanwendungen

**Leitfaden**: Nicht zutreffend. Benchmark ist für Webanwendungen gedacht, die in Azure App Service oder in IaaS-Instanzen ausgeführt werden.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Nicht verfügbar

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Ablehnen der Kommunikation mit bekannten bösartigen IP-Adressen

**Leitfaden**: Aktivieren Sie den Azure DDoS-Standardschutz (Distributed Denial-of-Service) im virtuellen Netzwerk, der Ihren Azure Batch-Pool vor DDoS-Angriffen schützt. Verwenden Sie die in Azure Security Center integrierte Threat Intelligence, um die Kommunikation mit bekannten schädlichen oder nicht genutzten IP-Adressen zu verweigern.


Konfigurieren von DDoS-Schutz:

https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection


Grundlegendes zu integrierten Informationen zu Bedrohungen in Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="15-record-network-packets-and-flow-logs"></a>1.5: Aufzeichnen von Netzwerkpaketen und Flussprotokollen

**Leitfaden**: Aktivieren Sie die Flussprotokolle für die Netzwerksicherheitsgruppe (NSG), die zum Schutz Ihres Azure Batch-Pools verwendet wird, und senden Sie Protokolle an ein Azure Storage-Konto für die Überwachung des Datenverkehrs.


Aktivieren der NSG-Flussprotokolle:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems"></a>1.6: Bereitstellen von netzwerkbasierten Angriffserkennungs-/Eindringschutzsystemen

**Leitfaden**: Wenn dies für Kompatibilitätszwecke erforderlich ist, wählen Sie ein virtuelles Netzwerkgerät aus Azure Marketplace aus, das Angriffserkennungssysteme (IDS) und IPS-Funktionen (Eindringschutzsysteme) mit Nutzlastinspektionsfunktionen unterstützt.


Wenn Angriffserkennung und/oder -verhinderung auf der Grundlage der Nutzlastüberprüfung keine Anforderung ist, kann Azure Firewall mit Threat Intelligence verwendet werden. Das Filtern auf Basis von Threat Intelligence mit Azure Firewall kann vor Datenverkehr von und zu bekannten schädlichen IP-Adressen oder Domänen warnen und diesen verweigern. Die IP-Adressen und Domänen stammen aus dem Microsoft Threat Intelligence-Feed.


Stellen Sie Azure Firewall mit einer öffentlichen IP-Adresse im gleichen virtuellen Netzwerk wie Ihre Azure Batch Pool-Knoten bereit. Konfigurieren Sie NAT-Regeln (Network Address Translation, Netzwerkadressübersetzung) zwischen vertrauenswürdigen Speicherorten im Internet und den privaten IP-Adressen der einzelnen Poolknoten. Konfigurieren Sie in Azure Firewall unter Threat Intelligence „Warnen und verweigern“, um zu warnen und den Datenverkehr an bzw. von bekannten schädlichen IP-Adressen und Domänen zu blockieren. Die IP-Adressen und Domänen stammen aus dem Microsoft Threat Intelligence-Feed. Es sind nur Datensätze mit der höchsten Genauigkeit enthalten. 


So erstellen Sie einen Azure Batch-Pool in einem virtuellen Netzwerk:

https://docs.microsoft.com/azure/batch/batch-virtual-network


Bereitstellen von Azure Firewall:

https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal


Azure Marketplace:

https://azuremarketplace.microsoft.com/marketplace/?term=Firewall

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="17-manage-traffic-to-your-web-applications"></a>1.7: Verwalten von Datenverkehr für Webanwendungen

**Leitfaden**: Nicht zutreffend. Benchmark ist für Webanwendungen gedacht, die in Azure App Service oder in IaaS-Instanzen ausgeführt werden.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Nicht verfügbar

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimieren der Komplexität und des Verwaltungsaufwands von Netzwerksicherheitsregeln

**Leitfaden**: Verwenden Sie Virtual Network-Diensttags, um Netzwerkzugriffssteuerungen in Netzwerksicherheitsgruppen oder Azure Firewalls zu definieren, die Ihren Azure Batch-Pools zugeordnet sind. Sie können Diensttags anstelle von spezifischen IP-Adressen nutzen, wenn Sie Sicherheitsregeln erstellen. Indem Sie den Diensttagnamen (z. B. „ApiManagement“) im entsprechenden Quell- oder Zielfeld einer Regel angeben, können Sie den Datenverkehr für den entsprechenden Dienst zulassen oder verweigern. Microsoft verwaltet die Adresspräfixe, für die das Diensttag gilt, und aktualisiert das Diensttag automatisch, wenn sich die Adressen ändern.


Grundlegendes zu Diensttags und Verwenden von Diensttags:

https://docs.microsoft.com/azure/virtual-network/service-tags-overview

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Beibehalten von Standardsicherheitskonfigurationen für Netzwerkgeräte

**Leitfaden**: Definieren und implementieren Sie mit Azure Policy Standardsicherheitskonfigurationen für Netzwerkressourcen, die Ihren Azure Batch-Pools zugeordnet sind. Verwenden Sie Azure Policy-Aliase in den Namespaces „Microsoft.Batch“ und „Microsoft.Network“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen der Netzwerkkonfiguration Ihrer Azure Batch-Pools zu erstellen.



Konfigurieren und Verwalten von Azure Policy:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="110-document-traffic-configuration-rules"></a>1.10: Dokumentieren von Datenverkehrskonfigurationsregeln

**Leitfaden**: Verwenden Sie Tags für Netzwerkdienstgruppen (NSGs) und andere Ressourcen im Zusammenhang mit Netzwerksicherheit und Datenverkehrsfluss, die mit ihren Azure Batch-Pools verknüpft sind. Verwenden Sie für einzelne NSG-Regeln das Feld „Beschreibung“, um geschäftliche Anforderungen und/oder deren Dauer (usw.) für alle Regeln festzulegen, die Datenverkehr in das bzw. aus einem Netzwerk zulassen.


Verwenden Sie eine der integrierten Azure Policy-Definitionen zum Tagging, etwa „Tag und zugehörigen Wert erzwingen“, um sicherzustellen, dass alle Ressourcen mit Tags erstellt werden und Sie über vorhandene nicht markierte Ressourcen benachrichtigt werden.


Sie können Azure PowerShell oder die Azure-Befehlszeilenschnittstelle verwenden, um Ressourcen basierend auf ihren Tags zu suchen oder Aktionen auszuführen.


Erstellen und Verwenden von Tags:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags


Erstellen eines virtuellen Netzwerks:

https://docs.microsoft.com/azure/virtual-network/quick-create-portal


Erstellen und Verwenden einer Netzwerksicherheitsgruppe (NSG):

https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Verwenden automatisierter Tools zum Überwachen von Netzwerkressourcenkonfigurationen und Erkennen von Änderungen

**Leitfaden**: Verwenden Sie das Azure-Aktivitätsprotokoll zum Überwachen der Konfigurationen von Netzwerkressourcen und zum Erkennen von Änderungen bei Netzwerkressourcen, die sich auf Ihre Azure Batch-Pools beziehen. Erstellen Sie Warnungen in Azure Monitor, die bei Änderungen an wichtigen Netzwerkressourcen ausgelöst werden.

Anzeigen und Abrufen von Azure-Aktivitätsprotokollereignissen: https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view 

Erstellen von Warnungen in Azure Monitor: https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

## <a name="logging-and-monitoring"></a>Protokollierung und Überwachung

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Protokollierung und Überwachung](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1: Verwenden von genehmigten Zeitsynchronisierungsquellen

**Leitfaden**: Für Azure Batch bietet Microsoft standardmäßig Zeitsynchronisierung. Wenn Sie jedoch über bestimmte Zeitsynchronisierungsanforderungen verfügen, können Sie diese Änderungen implementieren.

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2.2: Konfigurieren der zentralen Sicherheitsprotokollverwaltung

**Leitfaden**: Integrieren Sie das Azure Batch-Konto in Azure Monitor, um die von den Clustergeräten generierten Sicherheitsdaten zu aggregieren. Nutzen Sie benutzerdefinierte Abfragen, um Bedrohungen in der Umgebung zu erkennen und darauf zu reagieren.  Verwenden Sie für Azure Batch-Überwachung auf Ressourcenebene die Batch-APIs, um den Status von Ressourcen wie Aufträgen, Aufgaben, Knoten und Pools zu überwachen oder abzufragen.



Integrieren eines Azure Batch-Kontos in Azure Monitor:

https://docs.microsoft.com/azure/batch/batch-diagnostics

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Aktivieren der Überwachungsprotokollierung für Azure-Ressourcen

**Leitfaden**: Überwachen Sie für Azure Batch-Überwachung auf Kontoebene jedes Batch-Konto mithilfe der Features von Azure Monitor. Azure Monitor erfasst Metriken und optional Diagnoseprotokolle für Ressourcen auf der Ebene eines Batch-Kontos. Hierzu zählen beispielsweise Pools, Aufträge und Aufgaben. Erfassen und nutzen Sie diese Daten manuell oder programmgesteuert, um Aktivitäten in Ihrem Batch-Konto zu überwachen und Probleme zu diagnostizieren.


Verwenden Sie für Azure Batch-Überwachung auf Ressourcenebene die Azure Batch-APIs, um den Status von Ressourcen wie Aufträgen, Aufgaben, Knoten und Pools zu überwachen oder abzufragen.


Konfigurieren von Azure Batch-Überwachung und -Protokollierung auf Kontoebene:

https://docs.microsoft.com/azure/batch/monitoring-overview


Grundlegendes zur Batch-Überwachung auf Ressourcenebene:

https://docs.microsoft.com/azure/batch/monitoring-overview#batch-resource-monitoring

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="24-collect-security-logs-from-operating-system"></a>2.4: Erfassen von Sicherheitsprotokollen von Betriebssystemen

**Leitfaden**: Azure Monitor erfasst Metriken und Diagnoseprotokolle für Ressourcen in Ihrem Azure Batch-Konto. Sie können mithilfe verschiedener Methoden diese Daten sammeln und nutzen, um Ihr Azure Batch-Konto zu überwachen und Probleme zu diagnostizieren. Sie können auch Metrikwarnungen konfigurieren, um Benachrichtigungen zu erhalten, wenn eine Metrik einen angegebenen Wert erreicht.


Falls erforderlich, stellen Sie möglicherweise eine Verbindung mit Ihren einzelnen Poolknoten über Secure Shell (SSH) oder Remotedesktopprotokoll (RDP) her, um auf lokale Betriebssystemprotokolle zuzugreifen.


Erfassen von Diagnose Protokollen aus Ihrem Azure Batch-Konto:

https://docs.microsoft.com/azure/batch/batch-diagnostics#batch-diagnostics


Herstellen einer Remoteverbindung mit Ihren Azure Batch-Poolknoten:

https://docs.microsoft.com/azure/batch/batch-api-basics#error-handling

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="25-configure-security-log-storage-retention"></a>2.5: Konfigurieren der Sicherheitsprotokoll-Aufbewahrungsdauer im Speicher

**Leitfaden**: Integrieren eines Azure Batch-Kontos in Azure Monitor. Stellen Sie sicher, dass für den verwendeten Azure Log Analytics-Arbeitsbereich der Protokollaufbewahrungszeitraum gemäß den Compliancebestimmungen Ihrer Organisation festgelegt ist.


Konfigurieren von Azure Batch-Überwachung und -Protokollierung:

https://docs.microsoft.com/azure/batch/monitoring-overview


Konfigurieren des Aufbewahrungszeitraums des Azure Log Analytics-Arbeitsbereichs:

https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="26-monitor-and-review-logs"></a>2.6: Überwachen und Überprüfen von Protokollen

**Leitfaden**: Erstellen Sie Azure Batch-Metrikwarnungen, die ausgelöst werden, wenn der Wert für eine bestimmte Metrik einen angegebenen Schwellenwert überschreitet.


Konfigurieren von Azure Batch-Metrikwarnungen:

https://docs.microsoft.com/azure/batch/batch-diagnostics

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="27-enable-alerts-for-anomalous-activity"></a>2.7: Aktivieren von Warnungen für anormale Aktivitäten

**Leitfaden**: Erstellen Sie Azure Batch-Metrikwarnungen, die ausgelöst werden, wenn der Wert für eine bestimmte Metrik einen angegebenen Schwellenwert überschreitet.


Konfigurieren von Azure Batch-Metrikwarnungen:

https://docs.microsoft.com/azure/batch/batch-diagnostics

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="28-centralize-anti-malware-logging"></a>2.8: Zentralisieren der Antischadsoftwareprotokollierung

**Leitfaden**: Verwenden Sie Windows Defender für Ihre einzelnen Batch-Knoten im Fall von Windows-Betriebssystemen, oder stellen Sie eine eigene Antischadsoftwarelösung bereit, wenn Sie Linux verwenden.

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="29-enable-dns-query-logging"></a>2.9: Aktivieren der DNS-Abfrageprotokollierung

**Leitfaden**: Implementieren einer Drittanbieterlösung für DNS-Protokollierung

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="210-enable-command-line-audit-logging"></a>2.10: Aktivieren von Befehlszeilen-Überwachungsprotokollierung

**Leitfaden**: Konfigurieren Sie die Konsolenprotokollierung und PowerShell-Transkription manuell pro Knoten.

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="identity-and-access-control"></a>Identität und Zugriffssteuerung

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Identität und Zugriffssteuerung](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-inventory-of-administrative-accounts"></a>3.1: Verwalten eines Bestands von Administratorkonten

**Leitfaden**: Verwalten Sie den Datensatz für das lokale Administratorkonto, das während der Bereitstellung des Azure Batch-Pools erstellt wird, sowie für alle anderen Konten, die Sie erstellen. Wenn außerdem AAD-Integration (Azure Active Directory) verwendet wird, verfügt AAD über integrierte Rollen, die explizit zugewiesen werden müssen und daher abgefragt werden können. Verwenden Sie das AAD PowerShell-Modul, um Ad-hoc-Abfragen zum Ermitteln von Konten auszuführen, die Mitglieder von administrativen Gruppen sind.


Außerdem können Sie Empfehlungen für die Identitäts- und Zugriffsverwaltung in Azure Security Center anwenden.


Abrufen einer Verzeichnisrolle in AAD mit PowerShell:

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0


Abrufen von Mitgliedern einer Verzeichnisrolle in AAD mit PowerShell:

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0


Überwachen von Identität und Zugriff mit Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Ändern von Standardkennwörtern bei Bedarf

**Leitfaden**: Wenn Sie einen Azure Batch-Pool bereitstellen, haben Sie die Möglichkeit, lokale Computerkonten zu erstellen. Es gibt keine Standardkennwörter, die geändert werden müssen. Sie können jedoch unterschiedliche Kennwörter für den SSH-Zugriff (Secure Shell) und RDP-Zugriff (Remotedesktopprotokoll) angeben. Nachdem Azure Batch Pool konfiguriert wurde, können Sie einen zufälligen Benutzer für einzelne Knoten innerhalb des Azure-Portals oder über die Azure Resource Manager-API generieren.


Vorgehensweise beim Hinzufügen eines Benutzers zu einem bestimmten Computeknoten:

https://docs.microsoft.com/rest/api/batchservice/computenode/adduser

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="33-ensure-the-use-of-dedicated-administrative-accounts"></a>3.3: Sicherstellen der Verwendung dedizierter Administratorkonten

**Leitfaden**: Integrieren Sie Authentifizierung für Azure Batch-Anwendungen mit Azure Active Directory. Erstellen Sie Richtlinien und Vorgänge für die Verwendung dedizierter Administratorkonten.


Außerdem können Sie Empfehlungen für die Identitäts- und Zugriffsverwaltung in Azure Security Center anwenden.


Authentifizieren von Batch-Anwendungen mit Azure Active Directory:

https://docs.microsoft.com/azure/batch/batch-aad-auth


Überwachen von Identität und Zugriff mit Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="34-utilize-single-sign-on-sso-with-azure-active-directory"></a>3.4: Verwenden von einmaligem Anmelden (Single Sign-On, SSO) mit Azure Active Directory

**Leitfaden**: Nicht zutreffend. Azure Batch unterstützt zwar Azure AD-Authentifizierung, SSO wird aber nicht unterstützt.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Nicht verfügbar

### <a name="35-use-multifactor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Verwenden der mehrstufigen Authentifizierung für den gesamten Azure Active Directory-basierten Zugriff.

**Leitfaden**: Integrieren Sie Authentifizierung für Azure Batch-Anwendungen mit Azure Active Directory (AAD). Aktivieren Sie mehrstufige AAD-Authentifizierung (MFA), und befolgen Sie die Empfehlungen für die Identitäts- und Zugriffsverwaltung in Azure Security Center.
 


Aktivieren von MFA in Azure:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted


Überwachen von Identität und Zugriff in Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Verwenden dedizierter Computer (Arbeitsstationen mit privilegiertem Zugriff) für alle administrativen Aufgaben

**Leitfaden**: Verwenden Sie PAWs (Privileged Access Workstation, Arbeitsstation mit privilegiertem Zugriff) mit MFA (mehrstufige Authentifizierung), die für die Anmeldung und Konfiguration von Azure Batch-Ressourcen konfiguriert sind.


Informationen zu Arbeitsstationen mit privilegiertem Zugriff:

https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations


Aktivieren von MFA in Azure:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7: Protokollieren und Warnen bei verdächtigen Aktivitäten in Administratorkonten

**Leitfaden**: Wenn Sie über integrierte Authentifizierung für Azure Batch-Anwendungen mit Azure Active Directory (AAD) verfügen, verwenden Sie Azure Active Directory-Sicherheitsberichte für die Generierung von Protokollen und Warnungen, wenn verdächtige oder unsichere Aktivitäten in der Umgebung auftreten. Verwenden Sie Azure Security Center zum Überwachen von identitäts- und zugriffsbezogenen Aktivitäten.


Identifizieren von Azure AD-Benutzern, die aufgrund riskanter Aktivitäten gekennzeichnet wurden:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk


Überwachen der identitäts- und zugriffsbezogenen Aktivitäten von Benutzern in Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="38-manage-azure-resource-from-only-approved-locations"></a>3.8: Verwalten von Azure-Ressourcen nur über genehmigte Standorte

**Leitfaden**: Wenn Sie über integrierte Authentifizierung für Azure Batch-Anwendungen mit Azure Active Directory verfügen, können Sie bedingten Zugriff mit benannten Speicherorten verwenden, um den Zugriff nur für bestimmte logische Gruppierungen von IP-Adressbereichen oder Ländern/Regionen zuzulassen.



Konfigurieren benannter Standorte in Azure:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="39-use-azure-active-directory"></a>3.9: Verwenden von Azure Active Directory

**Leitfaden**: Verwenden Sie Azure Active Directory (AAD) als zentrales Authentifizierungs- und Autorisierungssystem, und integrieren Sie Authentifizierung für Azure Batch-Anwendungen mit AAD. AAD schützt Daten durch eine starke Verschlüsselung für ruhende und übertragene Daten. Außerdem werden in AAD Salts und Hashs für Anmeldeinformationen verwendet und diese sicher gespeichert.


Erstellen und Konfigurieren einer AAD-Instanz:

https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance


Authentifizieren von Batch-Anwendungen mit AAD:

https://docs.microsoft.com/azure/batch/batch-aad-auth

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Regelmäßiges Überprüfen und Abstimmen des Benutzerzugriffs

**Leitfaden**: Azure Active Directory (AAD) enthält Protokolle zum Ermitteln von veralteten Konten. Außerdem können Sie zusätzlich Zugriffsüberprüfungen für Azure-Identitäten verwenden, um Gruppenmitgliedschaften, den Zugriff auf Unternehmensanwendungen und Rollenzuweisungen effizient zu verwalten. Der Benutzerzugriff kann regelmäßig überprüft werden, um sicherzustellen, dass nur die richtigen Benutzer weiterhin Zugriff besitzen.


Verwenden von Zugriffsüberprüfungen für Azure-Identitäten:

https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11: Überwachen von Zugriffsversuchen auf deaktivierte Konten

**Leitfaden**: Erstellen Sie Diagnoseeinstellungen für Azure Active Directory-Benutzerkonten, und senden Sie die Überwachungs- und Anmeldeprotokolle an einen Log Analytics-Arbeitsbereich. Konfigurieren Sie die gewünschten Warnungen im Log Analytics-Arbeitsbereich.


Integrieren von Azure-Aktivitätsprotokollen in Azure Monitor:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Warnung bei abweichendem Verhalten bei der Kontoanmeldung

**Leitfaden**: Mit den Funktionen zur Risikoerkennung und zum Identitätsschutz von Azure Active Directory (AAD) können Sie automatische Antworten auf erkannte verdächtige Aktionen im Zusammenhang mit Benutzeridentitäten konfigurieren. Außerdem können Sie Daten zur weiteren Untersuchung in Azure Sentinel erfassen.


Anzeigen riskanter AAD-Anmeldungen:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins


Konfigurieren und Aktivieren von Risikorichtlinien für den Identitätsschutz:

https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies


Ausführen des Onboardings für Azure Sentinel:

https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Ermöglichen des Zugriffs auf relevante Kundendaten für Microsoft in Supportszenarien

**Leitfaden**: Nicht verfügbar. Kunden-Lockbox wird für Azure Batch nicht unterstützt. Liste der durch Kunden-Lockbox unterstützten Dienste: https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="data-protection"></a>Datenschutz

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Schutz von Daten](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Verwalten eines Bestands an vertraulichen Informationen

**Leitfaden**: Verwenden Sie Tags für die Nachverfolgung von Azure-Ressourcen, die vertrauliche Informationen speichern oder verarbeiten.


Erstellen und Verwenden von Tags:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Isolieren von Systemen, die vertrauliche Informationen speichern oder verarbeiten

**Leitfaden**: Implementieren Sie separate Abonnements und/oder Verwaltungsgruppen für Entwicklungs-, Test- und Produktionsumgebungen. Azure Batch-Pools sollten durch ein virtuelles Netzwerk/Subnetz getrennt, entsprechend gekennzeichnet und durch eine Netzwerksicherheitsgruppe (NSG) gesichert werden. Azure Batch-Daten sollten in einem gesicherten Azure Storage-Konto enthalten sein.


So erstellen Sie einen Azure Batch-Pool in einem virtuellen Netzwerk:

https://docs.microsoft.com/azure/batch/batch-virtual-network


Schützen von Azure Storage-Konten:

https://docs.microsoft.com/azure/storage/common/storage-security-guide

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Überwachen und Blockieren einer nicht autorisierten Übertragung vertraulicher Informationen.

**Leitfaden**: Bei Azure Storage-Konten, die Ihren Azure Batch-Pools zugeordnet sind, die vertrauliche Informationen enthalten, markieren Sie diese mithilfe von Tags als vertraulich, und schützen Sie sie mit bewährten Azure-Methoden.


Funktionen für Datenidentifizierung, -klassifizierung und -verlust sind für Azure Storage- oder Computeressourcen noch nicht verfügbar. Implementieren Sie eine Drittanbieterlösung, wenn dies für Compliancezwecke erforderlich ist.


Für die zugrundeliegende Plattform, die von Microsoft verwaltet wird, behandelt Microsoft alle Kundeninhalte als vertraulich und unternimmt große Anstrengungen, um Kundendaten vor Verlust und Gefährdung zu schützen. Um die Sicherheit von Kundendaten innerhalb von Azure zu gewährleisten, hat Microsoft eine Reihe von robusten Datenschutzkontrollen und -funktionen implementiert und verwaltet diese.


Grundlegendes zum Schutz von Kundendaten in Azure:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data


Schützen von Azure Storage-Konten:

https://docs.microsoft.com/azure/storage/common/storage-security-guide

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Verschlüsseln aller vertraulichen Informationen während der Übertragung

**Leitfaden**: Verschlüsseln Sie alle vertraulichen Informationen während der Übertragung. Microsoft Azure Ressourcen handeln standardmäßig TLS 1.2 aus. Stellen Sie sicher, dass alle Clients, die sich mit Ihren Azure Batch-Pools oder Datenspeichern (Azure Storage-Konten) verbinden, TLS 1.2 oder höher aushandeln können.


Stellen Sie sicher, dass für den Zugriff auf das Speicherkonto mit den Azure Batch-Daten HTTPS erforderlich ist.


Verstehen der Azure Storage-Kontoverschlüsselung während der Übertragung:

https://docs.microsoft.com/azure/storage/common/storage-security-guide#encryption-in-transit

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Shared

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Verwenden eines aktiven Ermittlungstools zur Bestimmung vertraulicher Daten

**Leitfaden**: Bei Azure Storage-Konten, die Ihren Azure Batch-Pools zugeordnet sind, die vertrauliche Informationen enthalten, markieren Sie diese mithilfe von Tags als vertraulich, und schützen Sie sie mit bewährten Azure-Methoden.


Funktionen für Datenidentifizierung, -klassifizierung und -verlust sind für Azure Storage- oder Computeressourcen noch nicht verfügbar. Implementieren Sie eine Drittanbieterlösung, wenn dies für Compliancezwecke erforderlich ist.


Für die zugrundeliegende Plattform, die von Microsoft verwaltet wird, behandelt Microsoft alle Kundeninhalte als vertraulich und unternimmt große Anstrengungen, um Kundendaten vor Verlust und Gefährdung zu schützen. Um die Sicherheit von Kundendaten innerhalb von Azure zu gewährleisten, hat Microsoft eine Reihe von robusten Datenschutzkontrollen und -funktionen implementiert und verwaltet diese.


Grundlegendes zum Schutz von Kundendaten in Azure:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data


Schützen von Azure Storage-Konten:

https://docs.microsoft.com/azure/storage/common/storage-security-guide

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Shared

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Verwenden von Azure RBAC zum Steuern des Zugriffs auf Ressourcen

**Leitfaden**: Verwenden Sie rollenbasierte Zugriffssteuerung (RBAC) von Azure Active Directory (AAD), um den Zugriff auf die Verwaltungsebene von Azure-Ressourcen (einschließlich Batch-Konto, Batch-Pools und Speicherkonten) zu steuern.


Grundlegendes zu Azure RBAC:

https://docs.microsoft.com/azure/role-based-access-control/overview


Konfigurieren von RBAC in Azure:

https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Verwenden der hostbasierten Verhinderung von Datenverlusten zum Erzwingen der Zugriffssteuerung

**Leitfaden**: Funktionen für Datenidentifizierung, -klassifizierung und -verlust sind für Azure Storage- oder Computeressourcen noch nicht verfügbar. Implementieren Sie eine Drittanbieterlösung, wenn dies für Compliancezwecke erforderlich ist.



Für die zugrundeliegende Plattform, die von Microsoft verwaltet wird, behandelt Microsoft alle Kundeninhalte als vertraulich und unternimmt große Anstrengungen, um Kundendaten vor Verlust und Gefährdung zu schützen. Um die Sicherheit von Kundendaten innerhalb von Azure zu gewährleisten, hat Microsoft eine Reihe von robusten Datenschutzkontrollen und -funktionen implementiert und verwaltet diese.



Grundlegendes zum Schutz von Kundendaten in Azure:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Shared

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Verschlüsseln vertraulicher, ruhender Informationen

**Leitfaden**: Für Speicherkonten, die Ihrem Azure Batch-Konto zugeordnet sind, wird empfohlen, Microsoft die Verwaltung von Verschlüsselungsschlüsseln zu gestatten. Sie haben jedoch die Möglichkeit, bei Bedarf Ihre eigenen Schlüssel zu verwalten.



Verwalten von Verschlüsselungsschlüsseln für Azure Storage-Konten:

https://docs.microsoft.com/azure/storage/common/storage-encryption-keys-portal

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Protokollieren und Warnen bei Änderungen an wichtigen Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Monitor mit dem Azure-Aktivitätsprotokoll, um Warnungen zu erstellen, die bei Änderungen an wichtigen Azure-Ressourcen ausgegeben werden, die Ihren Azure Batch-Konten/-Pools zugeordnet sind.



Konfigurieren Sie Diagnoseeinstellungen für Speicherkonten, die dem Azure Batch-Pool zugeordnet sind, um alle CRUD-Vorgänge für Pooldaten zu überwachen und zu protokollieren.



Erstellen von Warnungen für Azure-Aktivitätsprotokollereignisse:

https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log



Aktivieren zusätzlicher Protokollierung/Überwachung für ein Azure Storage-Konto:

https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="vulnerability-management"></a>Verwaltung von Sicherheitsrisiken

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Verwaltung von Sicherheitsrisiken](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Ausführen automatisierter Scantools für Sicherheitsrisiken

**Leitfaden**: Für Azure Batch-Poolknoten sind Sie für die Verwaltung der Lösung für die Sicherheitsrisikoverwaltung verantwortlich.


Wenn Sie über ein Rapid7-, Qualys- oder anderes Sicherheitsrisiko-Verwaltungsplattformabonnement verfügen, können Sie optional Sicherheitsanfälligkeits-Agents für Batch-Poolknoten manuell installieren und Knoten über das entsprechende Portal verwalten.

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Bereitstellen der automatisierten Lösung für die Patchverwaltung von Betriebssystemen

**Leitfaden**: Microsoft zum Verwalten und Aktualisieren von Basisknotenimages für Azure Batch-Pools. Stellen Sie sicher, dass das Betriebssystem der Azure Batch-Poolknoten für die Clusterlebensdauer gepatcht bleibt, was die Aktivierung automatischer Updates, die Überwachung der Knoten oder die Durchführung regelmäßiger Neustarts erfordern kann.

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Shared

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3: Bereitstellen der automatisierten Lösung für die Patchverwaltung von Drittanbietersoftware

**Leitfaden**: Stellen Sie sicher, dass die Drittanbieteranwendungen der Azure Batch-Poolknoten für die Clusterlebensdauer gepatcht bleibt, was die Aktivierung automatischer Updates, die Überwachung der Knoten oder die Durchführung regelmäßiger Neustarts erfordern kann.

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4 Vergleichen von kaskadierenden Sicherheitsrisikoscans

**Leitfaden**: Wenn Sie über ein Rapid7-, Qualys- oder anderes Sicherheitsrisiko-Verwaltungsplattformabonnement verfügen, können Sie das Portal dieses Anbieters verwenden, um kaskadierende Sicherheitsrisikoprüfungen anzuzeigen und zu vergleichen.

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Verwenden Sie einen Risikobewertungsprozess, um die Behebung von erkannten Sicherheitsrisiken zu priorisieren.

**Leitfaden**: Verwenden Sie ein gängiges Risikobewertungsprogramm (z. B. Common Vulnerability Scoring System) oder die von Ihrem Scantool eines Drittanbieters bereitgestellten Standardrisikobewertungen.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="inventory-and-asset-management"></a>Bestands- und Ressourcenverwaltung

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Bestands- und Ressourcenverwaltung](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-azure-asset-discovery"></a>6.1: Verwenden von Azure Asset Discovery

**Leitfaden**: Verwenden Sie Azure Resource Graph, um alle Ressourcen (z. B. Computeressourcen, Speicher, Netzwerke usw.) in Ihren Abonnements abzufragen bzw. zu ermitteln. Stellen Sie entsprechende Berechtigungen (Leseberechtigungen) in Ihrem Mandanten sicher, und vergewissern Sie sich, dass Sie alle Azure-Abonnements sowie Ressourcen in Ihren Abonnements aufzählen können.


Obwohl klassische Azure-Ressourcen über das Resource Graph ermittelt werden können, wird dringend empfohlen, Azure Resource Manager-Ressourcen zu erstellen und zu verwenden.


Erstellen von Abfragen mit Azure Graph:

https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal


Anzeigen Ihrer Azure-Abonnements:

https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0


Grundlegendes zu Azure RBAC:

https://docs.microsoft.com/azure/role-based-access-control/overview

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="62-maintain-asset-metadata"></a>6.2: Verwalten von Ressourcenmetadaten

**Leitfaden**: Wenden Sie Tags auf Ihre Azure-Ressourcen an, die Metadaten erzeugen, um sie logisch in einer Taxonomie zu organisieren.


Erstellen und Verwenden von Tags:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Löschen nicht autorisierter Azure-Ressourcen

**Leitfaden**: Verwenden Sie nach Bedarf Tagging, Verwaltungsgruppen und separate Abonnements zum Organisieren und Nachverfolgen von Ressourcen. Stimmen Sie den Bestand regelmäßig ab, und stellen Sie sicher, dass nicht autorisierte Ressourcen rechtzeitig aus dem Abonnement gelöscht werden.



Erstellen zusätzlicher Azure-Abonnements:

https://docs.microsoft.com/azure/billing/billing-create-subscription



Erstellen von Verwaltungsgruppen:

https://docs.microsoft.com/azure/governance/management-groups/create



Erstellen und Verwenden von Tags:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="64-maintain-inventory-of-approved-azure-resources-and-software-titles"></a>6.4: Verwalten eines Bestands genehmigter Azure-Ressourcen und Softwaretitel.

**Leitfaden**: Definieren Sie eine Liste der genehmigte Azure-Ressourcen und der genehmigten Software für Computeressourcen.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Überwachung auf nicht genehmigte Azure-Ressourcen

**Leitfaden**: Verwenden Sie eine Azure-Richtlinie, um Einschränkungen für den Typ der Ressourcen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie dazu die folgenden integrierten Richtliniendefinitionen:

- Not allowed resource types (Unzulässige Ressourcentypen)
- Zulässige Ressourcentypen

Verwenden Sie Azure Resource Graph, um Ressourcen in Ihren Abonnements abzufragen und zu ermitteln. Stellen Sie sicher, dass alle in der Umgebung vorhandenen Azure-Ressourcen genehmigt sind.

Konfigurieren und Verwalten von Azure Policy: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Erstellen von Abfragen mit Azure Graph: https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Überwachen auf nicht genehmigte Softwareanwendungen innerhalb von Computeressourcen

**Leitfaden**: Implementieren Sie für Azure Batch-Poolknoten eine Drittanbieterlösung, um Clusterknoten für nicht genehmigte Softwareanwendungen zu überwachen.

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Entfernen nicht genehmigter Azure-Ressourcen und Softwareanwendungen

**Leitfaden**: Implementieren Sie für Azure Batch-Poolknoten eine Drittanbieterlösung, um Clusterknoten für nicht genehmigte Softwareanwendungen zu überwachen.

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="68-use-only-approved-applications"></a>6.8: Ausschließliche Verwendung genehmigter Anwendungen

**Leitfaden**: Implementieren Sie für Azure Batch-Poolknoten eine Drittanbieterlösung, um zu verhindern, dass nicht autorisierte Software ausgeführt wird.

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="69-use-only-approved-azure-services"></a>6.9: Ausschließliche Verwendung genehmigter Azure-Dienste

**Leitfaden**: Verwenden Sie eine Azure-Richtlinie, um Einschränkungen für den Typ der Ressourcen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie dazu die folgenden integrierten Richtliniendefinitionen:

- Not allowed resource types (Unzulässige Ressourcentypen)
- Zulässige Ressourcentypen

Konfigurieren und Verwalten von Azure Policy:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Ablehnen eines bestimmten Ressourcentyps mit Azure Policy:

https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="610-implement-approved-application-list"></a>6.10: Implementieren einer Liste genehmigter Anwendungen

**Leitfaden**: Implementieren Sie für Azure Batch-Poolknoten eine Drittanbieterlösung, um zu verhindern, dass nicht autorisierte Dateitypen ausgeführt werden.

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager-via-scripts"></a>6.11: Einschränken der Möglichkeiten der Benutzer zur Interaktion mit Azure Resource Manager über Skripts

**Leitfaden**: Verwenden Sie den bedingten Azure-Zugriff, um die Möglichkeiten der Benutzer zur Interaktion mit Azure Resource Manager einzuschränken, indem Sie „Zugriff blockieren“ für die App zur „Verwaltung von Microsoft Azure“ konfigurieren.


Konfigurieren des bedingten Zugriffs, um den Zugriff auf Azure Resource Manager zu blockieren:

https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: Einschränken der Möglichkeiten der Benutzer, Skripts innerhalb von Computeressourcen auszuführen

**Leitfaden**: Nicht zutreffend.

Dies gilt nicht für Azure Batch, da Benutzer (Nicht-Administratoren) der Azure Batch-Pools keinen Zugriff auf die einzelnen Knoten benötigen, um Aufträge auszuführen. Der Clusteradministrator besitzt bereits root-Zugriff auf alle Knoten.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Nicht verfügbar

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Physische oder logische Trennung von Anwendungen mit hohem Risiko

**Leitfaden**: Nicht verfügbar.

Benchmark ist für Webanwendungen gedacht, die in Azure App Service oder in IaaS-Instanzen ausgeführt werden.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Nicht verfügbar

## <a name="secure-configuration"></a>Sichere Konfiguration

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Sichere Konfiguration](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Einrichten sicherer Konfigurationen für alle Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy-Aliase im Namespace „Microsoft.Batch“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen der Konfiguration Ihrer Azure Batch-Konten und -Pools zu erstellen.


Anzeigen verfügbarer Azure Policy-Aliase: https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0


Konfigurieren und Verwalten von Azure Policy: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="72-establish-secure-configurations-for-your-operating-system"></a>7.2: Einrichten sicherer Konfigurationen für Ihr Betriebssystem

**Leitfaden**: Einrichten sicherer Konfigurationen für das Betriebssystem Ihrer Batch-Poolknoten.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="73-maintain-secure-configurations-for-all-azure-resources"></a>7.3: Verwalten sicherer Konfigurationen für alle Azure-Ressourcen

**Leitfaden**: Verwenden Sie eine Azure-Richtlinie [deny] (Verweigern) und [deploy if not exist] (Bereitstellen, falls nicht vorhanden), um sichere Einstellungen für die Azure-Ressourcen zu erzwingen, die mit Ihrem Batch-Konto und Ihren Pools verknüpft sind (z. B. virtuelle Netzwerke, Subnetze, Azure Firewalls, Azure Storage-Konten usw.). Sie können Azure Policy-Aliase aus den folgenden Namespaces verwenden, um benutzerdefinierte Richtlinien zu erstellen:

- Microsoft.Batch
- Microsoft.Storage
- Microsoft.Network

Konfigurieren und Verwalten von Azure Policy: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Grundlegendes zu Azure Policy-Auswirkungen: https://docs.microsoft.com/azure/governance/policy/concepts/effects

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="74-maintain-secure-configurations-for-operating-systems"></a>7.4: Verwalten sicherer Konfigurationen für Betriebssysteme

**Leitfaden**: Azure Batch-Poolbetriebssystemimages werden von Microsoft verwaltet und gewartet. Sie sind verantwortlich für die Implementierung der Zustandskonfiguration auf Betriebssystemebene.

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Shared

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Sicheres Speichern der Konfiguration von Azure-Ressourcen

**Leitfaden**: Wenn Sie benutzerdefinierte Azure-Richtliniendefinitionen für Azure Batch-Konten, -Pools oder verwandte Ressourcen verwenden, nutzen Sie Azure DevOps/Repos, um Code sicher zu speichern und zu verwalten.

Speichern von Code in Azure DevOps: https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

Dokumentation zu Azure Repos: https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: Sicheres Speichern von benutzerdefinierten Betriebssystemimages

**Leitfaden**: Wenn Sie benutzerdefinierte Images für Ihre Azure Batch-Pools verwenden, verwenden Sie rollenbasierte Zugriffssteuerung (RBAC), um sicherzustellen, dass nur autorisierte Benutzer auf die Images zugreifen können.


Grundlegendes zu RBAC in Azure: https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles

Konfigurieren von RBAC in Azure: https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="77-deploy-system-configuration-management-tools"></a>7.7: Bereitstellen von Verwaltungstools zur Systemkonfiguration

**Leitfaden**: Verwenden Sie integrierte Azure Policy-Definitionen, um Azure Batch-bezogene Ressourcenkonfigurationen mit Warnungen zu versehen, zu überwachen und zu erzwingen.  Verwenden Sie Azure Policy-Aliase im Namespace „Microsoft.Batch“, um benutzerdefinierte Richtlinien für Ihre Azure Batch-Konten und -Pools zu erstellen. Entwickeln Sie außerdem einen Prozess und eine Pipeline zum Verwalten von Richtlinienausnahmen.



Konfigurieren und Verwalten von Azure Policy:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8: Bereitstellen von Verwaltungstools für Systemkonfigurationen für Betriebssysteme

**Leitfaden**: Implementieren Sie eine Drittanbieterlösung, um den gewünschten Zustand für die Betriebssysteme Ihrer Azure Batch-Poolknoten zu verwalten.

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9: Implementieren der automatisierten Konfigurationsüberwachung für Azure-Dienste

**Leitfaden**: Verwenden Sie Azure Policy-Aliase im Namespace „Microsoft.Batch“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen der Konfiguration Ihrer Azure Batch-Instanz zu erstellen. Sie können auch die integrierten Richtlinien verwenden, die speziell für Azure Batch oder die von Azure Batch verwendeten Ressourcen erstellt wurden, beispielsweise:

- Subnetze sollten einer Netzwerksicherheitsgruppe zugeordnet werden
- Speicherkonten sollten einen VNET-Dienstendpunkt verwenden
- Diagnoseprotokolle in Batch-Konten sollten aktiviert sein.

Anzeigen verfügbarer Azure Policy-Aliase: https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0

Konfigurieren und Verwalten von Azure Policy: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage


**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: Implementieren der automatisierten Konfigurationsüberwachung für Betriebssysteme

**Leitfaden**: Implementieren Sie eine Drittanbieterlösung, um den Zustand für die Betriebssysteme Ihrer Azure Batch-Poolknoten zu überwachen.

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="711-securely-manage-azure-secrets"></a>7.11: Sicheres Verwalten von Azure-Geheimnissen

**Leitfaden**: Azure Key Vault kann mit Azure Batch-Bereitstellungen verwendet werden, um Schlüssel für den Poolspeicher innerhalb von Azure Storage-Konten zu verwalten.


Integration mit verwalteten Azure-Identitäten:

https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity


Erstellen einer Azure Key Vault-Instanz:

https://docs.microsoft.com/azure/key-vault/quick-create-portal


Bereitstellen der Key Vault-Authentifizierung mit einer verwalteten Identität:

https://docs.microsoft.com/azure/key-vault/managed-identity

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="712-securely-and-automatically-manage-identities"></a>7.12: Sicheres und automatisches Verwalten von Identitäten

**Leitfaden**: Nicht verfügbar. Die verwaltete Dienstidentität wird von Azure Batch nicht unterstützt.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Nicht verfügbar

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Beheben der unbeabsichtigten Offenlegung von Anmeldeinformationen

**Leitfaden**: Implementieren Sie Credential Scanner, um Anmeldeinformationen im Code zu identifizieren. In Credential Scanner wird auch das Verschieben von ermittelten Anmeldeinformationen an sicherere Speicherorte (z. B. Azure Key Vault) empfohlen. 

Einrichten von Credential Scanner: https://secdevtools.azurewebsites.net/helpcredscan.html

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="malware-defense"></a>Schutz vor Schadsoftware

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Schutz vor Schadsoftware](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1: Verwenden einer zentral verwalteten Antischadsoftware

**Leitfaden**: Verwenden Sie Windows Defender für Ihre einzelnen Azure Batch-Poolknoten im Fall von Windows-Betriebssystemen, oder stellen Sie eine eigene Antischadsoftwarelösung bereit, wenn Sie Linux verwenden.

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: Vorabprüfen von in computefremde Azure-Ressourcen hochzuladenden Dateien

**Leitfaden**: Microsoft Antimalware ist auf dem zugrunde liegenden Host für die Azure-Dienste (z. B. Azure Batch) aktiviert, wird jedoch nicht für Kundeninhalte ausgeführt.


Führen Sie eine Vorabprüfung aller Dateien durch, die in computefremde Azure-Ressourcen hochgeladen werden sollen, z. B. in App Service, Data Lake Storage, Blob Storage usw. Microsoft kann in diesen Instanzen nicht auf Kundendaten zugreifen.


Grundlegendes zu Microsoft Antimalware für Azure Cloud Services und Virtual Machines:

https://docs.microsoft.com/azure/security/fundamentals/antimalware

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Shared

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>Schritt 8.3: Sicherstellen der Aktualisierung von Antischadsoftware und Signaturen

**Leitfaden**: Verwenden Sie Windows Defender für Ihre einzelnen Azure Batch-Poolknoten im Fall von Windows-Betriebssystemen, und stellen Sie sicher, dass automatische Updates aktiviert sind. Stellen Sie Ihre eigene Antischadsoftwarelösung bereit, wenn Sie Linux verwenden.

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="data-recovery"></a>Datenwiederherstellung

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Datenwiederherstellung](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Sicherstellen regelmäßiger automatisierter Sicherungen

**Leitfaden**: Wenn Sie ein Azure Storage-Konto für den Azure Batch-Pooldatenspeicher verwenden, wählen Sie die entsprechende Redundanzoption (LRS, ZRS, GRS, RA-GRS) aus. 


Konfigurieren der Speicherredundanz für Azure Storage-Konten:

https://docs.microsoft.com/azure/storage/common/storage-redundancy

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Durchführen vollständiger Systemsicherungen und Sichern aller von Kunden verwalteten Schlüssel

**Leitfaden**: Wenn Sie ein Azure Storage-Konto für den Azure Batch-Pooldatenspeicher verwenden, wählen Sie die entsprechende Redundanzoption (LRS, ZRS, GRS, RA-GRS) aus.  Wenn Sie Azure Key Vault für einen Teil Ihrer Azure Batch-Bereitstellung verwenden, stellen Sie sicher, dass Ihre Schlüssel gesichert werden.


Konfigurieren der Speicherredundanz für Azure Storage-Konten:

https://docs.microsoft.com/azure/storage/common/storage-redundancy


Sichern von Key Vault-Schlüsseln in Azure:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Überprüfen aller Sicherungen einschließlich der von Kunden verwalteten Schlüssel

**Leitfaden**: Wenn Sie Ihre eigenen Schlüssel für Azure Storage-Konten oder eine andere Ressource verwalten, die sich auf Ihre Azure Batch-Implementierung bezieht, testen Sie regelmäßig die Wiederherstellung gesicherter Schlüssel.


Sichern von Key Vault-Schlüsseln in Azure:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0


Wiederherstellen eines vom Kunden verwalteten Schlüssels mit PowerShell:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Sicherstellen des Schutzes von Sicherungen und von kundenseitig verwalteten Schlüsseln

**Leitfaden**: Wenn Azure Key Vault zum Speichern von Schlüsseln verwendet wird, die sich auf Azure Batch-Poolspeicherkonten beziehen, aktivieren Sie vorläufiges Löschen in Azure Key Vault, um Schlüssel vor versehentlichem oder böswilligem Löschen zu schützen.


Aktivieren von vorläufigem Löschen in Azure Key Vault:

https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="incident-response"></a>Reaktion auf Vorfälle

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Reaktion auf Vorfälle](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-incident-response-guide"></a>10.1: Erstellen eines Leitfadens für die Reaktion auf Vorfälle

**Leitfaden**: Stellen Sie sicher, dass es schriftliche Pläne für die Reaktion auf Vorfälle gibt, in denen die Rollen der Mitarbeiter sowie die Phasen der Bearbeitung und Verwaltung von Vorfällen definiert sind.



Konfigurieren von Workflowautomatisierungen in Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="102-create-incident-scoring-and-prioritization-procedure"></a>10.2: Erstellen eines Verfahrens zur Bewertung und Priorisierung von Vorfällen

**Leitfaden**: Security Center weist Warnungen einen Schweregrad zu, um Ihnen zu helfen, die Reihenfolge zu priorisieren, in der Sie sich um Warnungen kümmern. So können Sie, sobald eine Ressource gefährdet ist, sofort zu ihr gelangen. Der Schweregrad basiert darauf, wie zuversichtlich Security Center in Bezug auf den Befund oder die Analyse ist, die zum Auslösen der Warnung verwendet wird, sowie auf dem Zuverlässigkeitsgrad, dass hinter der Aktivität, die zu der Warnung führte, eine böswillige Absicht stand.

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="103-test-security-response-procedures"></a>10.3: Verfahren zum Testen der Reaktion auf Sicherheitsvorfälle

**Leitfaden**: Führen Sie in regelmäßigen Abständen Tests zur Reaktionsfähigkeit Ihrer Systeme auf Vorfälle durch. Identifizieren Sie Schwachstellen und Lücken, und überarbeiten Sie den Plan bei Bedarf.

Informationen finden Sie in der folgenden Veröffentlichung des NIST: Leitfaden zum Testen, Trainieren und Ausführen von Programmen für IT-Pläne und -Funktionen: https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-nbspfor-security-incidents"></a>10.4: Angeben von Kontaktdaten für Sicherheitsvorfälle und Konfigurieren von Warnungsbenachrichtigungen &nbsp;für Sicherheitsvorfälle

**Leitfaden**: Microsoft kontaktiert Sie unter den für Sicherheitsvorfälle angegebenen Kontaktdaten, wenn das Microsoft Security Response Center (MSRC) feststellt, dass Personen unrechtmäßig oder unbefugt auf Ihre Daten zugegriffen haben.



Festlegen der Kontaktinformationen in Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Integrieren von Sicherheitswarnungen in das System zur Reaktion auf Vorfälle

**Leitfaden**: Exportieren Sie die Azure Security Center-Warnungen und -Empfehlungen über die Funktion „Fortlaufender Export“. Über „Fortlaufender Export“ können Sie Warnungen und Empfehlungen entweder manuell oder kontinuierlich exportieren. Sie können den Azure Security Center-Datenconnector verwenden, um die Warnungen an Azure Sentinel zu streamen.


Konfigurieren des fortlaufenden Exports:

https://docs.microsoft.com/azure/security-center/continuous-export


Streamen von Warnungen in Azure Sentinel:

https://docs.microsoft.com/azure/sentinel/connect-azure-security-center

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatisieren der Reaktion auf Sicherheitswarnungen

**Leitfaden**: Verwenden Sie die Funktion „Workflowautomatisierung“ in Azure Security Center, um über „Logic Apps“ automatisch Reaktionen auf Sicherheitswarnungen und -empfehlungen auszulösen.



Konfigurieren von „Workflowautomatisierung“ und Logic Apps:

https://docs.microsoft.com/azure/security-center/workflow-automation

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="penetration-tests-and-red-team-exercises"></a>Penetrationstests und Red Team-Übungen

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Penetrationstests und Red Team-Übungen](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-to-remediate-all-critical-security-findings-within-60-days"></a>11.1: Durchführen regelmäßiger Penetrationstests ihrer Azure-Ressourcen und Sicherstellen der Behebung aller kritischen Sicherheitsergebnisse innerhalb von 60 Tagen.

**Leitfaden**: Befolgen Sie die Microsoft Rules of Engagement, um sicherzustellen, dass die Penetrationstests nicht gegen Microsoft-Richtlinien verstoßen:

https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1.



Weitere Informationen zur Microsoft-Strategie und Ausführung von Red Team- und Livewebsite-Penetrationstests für von Microsoft verwaltete Cloudinfrastruktur, Dienste und Anwendungen finden Sie hier: 

https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Shared

