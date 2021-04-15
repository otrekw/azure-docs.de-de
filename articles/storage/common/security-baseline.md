---
title: Azure-Sicherheitsbaseline für Azure Storage
description: Die Azure Storage-Sicherheitsbaseline enthält Anleitungen und Ressourcen für die Implementierung der Sicherheitsempfehlungen im Azure-Sicherheitsvergleichstest.
author: msmbaldwin
ms.service: storage
ms.topic: conceptual
ms.date: 03/16/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 56f340a8d64346fd8934e9cfbae26079d4ee7f29
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104576545"
---
# <a name="azure-security-baseline-for-azure-storage"></a>Azure-Sicherheitsbaseline für Azure Storage

Diese Sicherheitsbaseline wendet Empfehlungen des [Azure-Sicherheitsvergleichstests Version 1.0](../../security/benchmarks/overview-v1.md) auf Azure Storage an. Der Azure-Sicherheitsvergleichstest enthält Empfehlungen zum Schutz Ihrer Cloudlösungen in Azure.
Der Inhalt wird nach den **Sicherheitskontrollen** gruppiert, die durch den Azure-Sicherheitsvergleichstest und die entsprechenden für Azure Storage geltenden Empfehlungen definiert sind. Nicht auf Azure Storage anwendbare **Kontrollen** wurden ausgeschlossen.

 
Die vollständige Zuordnung von Azure Storage zum Azure-Sicherheitsvergleichstest finden Sie in der [vollständigen Zuordnungsdatei der Azure Storage-Sicherheitsbaseline](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Netzwerksicherheit

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Netzwerksicherheit](../../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Schützen von Azure-Ressourcen in virtuellen Netzwerken

**Leitfaden**: Konfigurieren Sie die Firewall für das Storage-Konto, indem Sie den Zugriff auf Clients über bestimmte öffentliche IP-Adressbereiche, ausgewählte virtuelle Netzwerke oder bestimmte Azure-Ressourcen einschränken.  Sie können auch private Endpunkte konfigurieren, sodass der Datenverkehr an den Speicherdienst Ihres Unternehmens ausschließlich über private Netzwerke verläuft.

Hinweis: Firewalls und virtuelle Netzwerke werden von klassischen Speicherkonten nicht unterstützt.

- [Konfigurieren der Azure Storage-Firewall](https://docs.microsoft.com/azure/storage/common/storage-network-security#change-the-default-network-access-rule)

- [Konfigurieren von privaten Endpunkten für Azure Storage](storage-private-endpoints.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.Storage**:

[!INCLUDE [Resource Policy for Microsoft.Storage 1.1](../../../includes/policy/standards/asb/rp-controls/microsoft.storage-1-1.md)]

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2: Überwachen und Protokollieren der Konfiguration und des Datenverkehrs von virtuellen Netzwerken, Subnetzen und Netzwerkschnittstellen

**Leitfaden**: Azure Storage bietet ein mehrschichtiges Sicherheitsmodell. Sie können den Zugriff auf Ihr Speicherkonto auf Anforderungen beschränken, die aus angegebenen IP-Adressen, IP-Adressbereichen oder einer Liste von Subnetzen in einem virtuellen Azure-Netzwerk stammen. Verwenden Sie das Azure Security Center, und befolgen Sie die Empfehlungen für den Netzwerkschutz, um Ihre Netzwerkressourcen in Azure zu schützen. Aktivieren Sie außerdem NSG-Datenflussprotokolle (Network Security Group, Netzwerksicherheitsgruppe) für virtuelle Netzwerke oder Subnetze, die für die Storage-Konten über die zugehörige Firewall konfiguriert sind, und senden Sie Protokolle an ein Storage-Konto für die Überwachung des Datenverkehrs. 

 
Wenn Sie private Endpunkte besitzen, die an Ihr Speicherkonto angefügt sind, können Sie keine NSG-Regeln für Subnetze konfigurieren. 
 

 
- [Konfigurieren von Azure Storage-Firewalls und virtuellen Netzwerken](storage-network-security.md) 
 

 
- [Aktivieren der NSG-Flussprotokolle](../../network-watcher/network-watcher-nsg-flow-logging-portal.md) 
 

 
- [Grundlegendes zur Netzwerksicherheit über das Azure Security Center](../../security-center/security-center-network-recommendations.md) 
 

 
- [Grundlegendes zu privaten Endpunkten für Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-private-endpoints#known-issues)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="13-protect-critical-web-applications"></a>1.3: Schützen kritischer Webanwendungen

**Leitfaden**: Nicht zutreffend; diese Empfehlung ist für Webanwendungen gedacht, die in Azure App Service oder auf Computeressourcen ausgeführt werden.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Ablehnen der Kommunikation mit bekannten schädlichen IP-Adressen

**Leitfaden**: Aktivieren Sie Azure Defender für Storage für Ihr Azure Storage-Konto. Azure Defender für Storage ermöglicht die Nutzung intelligenter Sicherheitsfunktionen zur Erkennung von ungewöhnlichen und möglicherweise schädlichen Versuchen, auf Speicherkonten zuzugreifen oder diese unbefugt zu nutzen.  Die integrierten Azure Security Center-Warnungen basieren auf Aktivitäten, bei denen der Netzwerkkommunikation eine IP-Adresse zugeordnet wurde, die erfolgreich aufgelöst wurde, und zwar unabhängig davon, ob die IP-Adresse eine bekannte riskante IP-Adresse (z. B. ein bekannter Cryptominer ) oder eine IP-Adresse ist, die zuvor nicht als riskant erkannt wurde. Bei Anomalien im Rahmen von Aktivitäten werden Sicherheitswarnungen ausgelöst. 

- [Konfigurieren von Azure Defender für Storage](azure-defender-storage-configure.md) 

- [Grundlegendes zur integrierten Threat Intelligence in Azure Security Center](../../security-center/azure-defender.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="15-record-network-packets"></a>1.5: Aufzeichnen von Netzwerkpaketen

**Leitfaden**: Mithilfe der Paketerfassung von Network Watcher können Sie Sitzungen erfassen, um den Datenverkehr zwischen Storage-Konten und virtuellen Computern nachzuverfolgen. Für die Erfassungssitzung werden Filter bereitgestellt, um sicherzustellen, dass nur der gewünschte Datenverkehr erfasst wird. Mithilfe der Paketerfassung können Sie Netzwerkanomalien sowohl reaktiv als auch proaktiv diagnostizieren. Weitere Verwendungszwecke sind das Erfassen von Netzwerkstatistiken, das Gewinnen von Informationen zu Netzwerkangriffen, das Debuggen der Kommunikation zwischen Client und Server und vieles mehr. Durch die Möglichkeit zur Remoteauslösung von Paketerfassungen wird die manuelle Ausführung einer Paketerfassung auf einem gewünschten virtuellen Computer erleichtert. So lässt sich wertvolle Zeit sparen. 

- [Verwalten von Paketerfassungen mit Azure Network Watcher über das Portal](../../network-watcher/network-watcher-packet-capture-manage-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Bereitstellen von netzwerkbasierten Angriffserkennungs-/Eindringschutzsystemen (Intrusion Detection/Intrusion Prevention Systems, IDS/IPS)

**Leitfaden**: Azure Defender für Storage ermöglicht die Nutzung intelligenter Sicherheitsfunktionen zur Erkennung von ungewöhnlichen und möglicherweise schädlichen Versuchen, auf Speicherkonten zuzugreifen oder diese unbefugt zu nutzen. Bei Anomalien im Rahmen von Aktivitäten werden Sicherheitswarnungen ausgelöst. Diese Sicherheitswarnungen sind mit dem Azure Security Center integriert und werden mit Informationen zu verdächtigen Aktivitäten und Empfehlungen zur Untersuchung und Beseitigung von Bedrohungen auch per E-Mail an Abonnementadministratoren gesendet. 

- [Konfigurieren von Azure Defender für Storage](https://docs.microsoft.com/azure/storage/common/azure-defender-storage-configure?tabs=azure-security-center)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="17-manage-traffic-to-web-applications"></a>1.7: Verwalten von Datenverkehr für Webanwendungen

**Leitfaden**: Nicht zutreffend; diese Empfehlung ist für Webanwendungen gedacht, die in Azure App Service oder auf Computeressourcen ausgeführt werden.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimieren der Komplexität und des Verwaltungsaufwands von Netzwerksicherheitsregeln

**Leitfaden**: Verwenden Sie für Ressourcen in virtuellen Netzwerken, die Zugriff auf Ihr Storage-Konto benötigen, Virtual Network-Diensttags für das konfigurierte virtuelle Netzwerk, um die Netzwerkzugriffssteuerung in Netzwerksicherheitsgruppen oder Azure Firewall zu definieren. Sie können Diensttags anstelle von spezifischen IP-Adressen nutzen, wenn Sie Sicherheitsregeln erstellen. Indem Sie den Diensttagnamen (wie Storage) im entsprechenden Quell- oder Zielfeld einer Regel angeben, können Sie den Datenverkehr für den entsprechenden Dienst zulassen oder verweigern. Microsoft verwaltet die Adresspräfixe, für die das Diensttag gilt, und aktualisiert das Diensttag automatisch, wenn sich die Adressen ändern. 

Wenn der Netzwerkzugriff auf bestimmte Storage-Konten beschränkt werden muss, sollten Sie Richtlinien für VNET-Dienstendpunkte einsetzen.

- [Weitere Informationen zur Verwendung von Diensttags](../../virtual-network/service-tags-overview.md)

- [Weitere Informationen zu Richtlinien für VNET-Dienstendpunkte für Azure Storage](../../virtual-network/virtual-network-service-endpoint-policies-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Beibehalten von Standardsicherheitskonfigurationen für Netzwerkgeräte

**Leitfaden**: Definieren und implementieren Sie mit Azure Policy Standardsicherheitskonfigurationen für Netzwerkressourcen, die Ihrem Azure Storage-Konto zugeordnet sind. Verwenden Sie Azure Policy-Aliase in den Namespaces „Microsoft.Storage“ und „Microsoft.Network“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen der Netzwerkkonfiguration Ihrer Storage-Kontoressourcen zu erstellen. 

Sie können auch die integrierten Richtliniendefinitionen für Storage-Konten verwenden: Speicherkonten sollten einen VNET-Dienstendpunkt verwenden 

- [Konfigurieren und Verwalten von Azure Policy](../../governance/policy/tutorials/create-and-manage.md) 

- [Azure Policy-Beispiele für Speicher](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#storage) 

- [Azure Policy-Beispiele für Netzwerke](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#network) 

- [Erstellen einer Azure-Blaupause](../../governance/blueprints/create-blueprint-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="110-document-traffic-configuration-rules"></a>1.10: Dokumentieren von Datenverkehrskonfigurationsregeln

**Leitfaden**: Verwenden Sie Tags für Netzwerksicherheitsgruppen und andere Ressourcen im Zusammenhang mit Netzwerksicherheit und Datenverkehrsfluss. Mithilfe von Tagging können Sie integrierte und benutzerdefinierte Name/Wert-Paare einer bestimmten Netzwerkressource zuordnen, um Netzwerkressourcen zu organisieren und Azure-Ressourcen mit dem Netzwerkentwurf zu verknüpfen.

Verwenden Sie eine der integrierten Azure Policy-Definitionen zum Tagging, z. B. „Tag und zugehörigen Wert erzwingen“, um sicherzustellen, dass alle Ressourcen mit Tags erstellt werden und Sie über vorhandene nicht markierte Ressourcen benachrichtigt werden. 

Netzwerksicherheitsgruppen unterstützen Tags, aber keine einzelnen Sicherheitsregeln. Sicherheitsregeln verfügen über ein **Beschreibungsfeld**, das Sie zum Speichern von Informationen verwenden können, die Sie normalerweise in einem Tag speichern würden.

- [Verwenden von Tags zum Organisieren von Azure-Ressourcen und Verwaltungshierarchie](../../azure-resource-manager/management/tag-resources.md)

- [Filtern des Netzwerkdatenverkehrs mit einer Netzwerksicherheitsgruppe](../../virtual-network/tutorial-filter-network-traffic.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Verwenden automatisierter Tools zum Überwachen von Netzwerkressourcenkonfigurationen und Erkennen von Änderungen

**Leitfaden**: Verwenden Sie Azure Policy, um Konfigurationsänderungen für Netzwerkressourcen zu protokollieren.  Erstellen Sie Warnungen in Azure Monitor, die bei Änderungen an wichtigen Netzwerkressourcen ausgelöst werden.  

 
- [Konfigurieren und Verwalten von Azure Policy](../../governance/policy/tutorials/create-and-manage.md)
 
 
- [Erstellen von Warnungen in Azure Monitor](../../azure-monitor/alerts/alerts-activity-log.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="logging-and-monitoring"></a>Protokollierung und Überwachung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Protokollierung und Überwachung](../../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Konfigurieren der zentralen Sicherheitsprotokollverwaltung

**Leitfaden**: Erfassen Sie Protokolle über Azure Monitor, um Sicherheitsdaten zu aggregieren, die von Endpunktgeräten, Netzwerkressourcen und anderen Sicherheitssystemen generiert werden. Verwenden Sie Log Analytics-Arbeitsbereiche in Azure Monitor, um Analysen abzufragen und durchzuführen, und Azure Storage-Konten für die langfristige Speicherung bzw. Archivierung. Optional können Sie Sicherheitsfeatures wie den unveränderlichen Speicher und die erzwungene Aufbewahrung nutzen.

 
- [Sammeln von Plattformprotokollen und -metriken mit Azure Monitor](../../azure-monitor/essentials/diagnostic-settings.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Aktivieren der Überwachungsprotokollierung für Azure-Ressourcen

**Leitfaden**: Azure Storage Analytics bietet Protokolle für Blobs, Warteschlangen und Tabellen. Sie können das Azure-Portal verwenden, um zu konfigurieren, welche Protokolle für Ihr Konto angelegt werden.   

 
- [Konfigurieren der Überwachung für ein Azure Storage-Konten](manage-storage-analytics-logs.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="25-configure-security-log-storage-retention"></a>2.5: Konfigurieren der Sicherheitsprotokoll-Aufbewahrungsdauer im Speicher

**Leitfaden**: Beim Speichern von Sicherheitsereignisprotokollen im Azure Storage-Konto oder Log Analytics-Arbeitsbereich können Sie die Aufbewahrungsrichtlinie den Anforderungen Ihrer Organisation entsprechend festlegen.  

 
- [Konfigurieren der Aufbewahrungsrichtlinie für Azure Storage-Kontoprotokolle](https://docs.microsoft.com/azure/storage/common/manage-storage-analytics-logs#configure-logging) 

 
- [Ändern des Datenaufbewahrungszeitraums in Protokollanalyse](https://docs.microsoft.com/azure/azure-monitor/logs/manage-cost-storage#change-the-data-retention-period)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="26-monitor-and-review-logs"></a>2.6: Überwachen und Überprüfen von Protokollen

**Leitfaden**: Sie können Azure Storage-Protokolle über gängige Methoden wie Abfragen über Log Analytics überprüfen oder die Protokolldateien direkt anzeigen. In Azure Storage werden die Protokolle in Blobs gespeichert, auf die direkt über `http://accountname.blob.core.windows.net/$logs` zugegriffen werden muss. Der Protokollordner ist standardmäßig ausgeblendet, sodass Sie direkt zu diesem navigieren müssen. Er wird nicht über List-Befehle angezeigt. 

 
Aktivieren Sie zudem Azure Defender für Storage für Ihr Storage-Konto. Azure Defender für Storage ermöglicht die Nutzung intelligenter Sicherheitsfunktionen zur Erkennung von ungewöhnlichen und möglicherweise schädlichen Versuchen, auf Speicherkonten zuzugreifen oder diese unbefugt zu nutzen. Bei Anomalien im Rahmen von Aktivitäten werden Sicherheitswarnungen ausgelöst. Diese Sicherheitswarnungen sind mit dem Azure Security Center integriert und werden mit Informationen zu verdächtigen Aktivitäten und Empfehlungen zur Untersuchung und Beseitigung von Bedrohungen auch per E-Mail an Abonnementadministratoren gesendet. 
 

 
- [Protokollieren und Überprüfen von Daten](https://docs.microsoft.com/azure/storage/common/storage-analytics-logging#how-logs-are-stored) 
 

 
- [Konfigurieren von Azure Defender für Storage](https://docs.microsoft.com/azure/storage/common/azure-defender-storage-configure?tabs=azure-portal)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Aktivieren von Warnungen bei anomalen Aktivitäten

**Leitfaden**: Aktivieren Sie in Azure Security Center Ihr Konto für Azure Defender für Storage. Aktivieren Sie die Diagnoseeinstellungen für das Storage-Konto, und senden Sie Protokolle an einen Log Analytics-Arbeitsbereich. Integrieren Sie Ihren Log Analytics-Arbeitsbereich in Azure Sentinel, da er eine Lösung für die Sicherheitsorchestrierung mit automatisierter Reaktion (Security Orchestration Automated Response, SOAR) bereitstellt. Dadurch können Playbooks (automatisierte Lösungen) erstellt und zum Beheben von Sicherheitsproblemen verwendet werden.  

 
- [Durchführen des Onboardings für Azure Sentinel](../../sentinel/quickstart-onboard.md)  

 
- [Verwalten von Warnungen in Azure Security Center](../../security-center/security-center-managing-and-responding-alerts.md)  

 
- [Warnungen bei Log Analytics-Protokolldaten](../../azure-monitor/alerts/tutorial-response.md)  

 
- [Azure Storage Analytics-Protokollierung](storage-analytics-logging.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="28-centralize-anti-malware-logging"></a>2.8: Zentralisieren der Antischadsoftwareprotokollierung

**Leitfaden**: Verwenden Sie das Azure Security Center, und aktivieren Sie Azure Defender für Storage, damit Schadsoftwareuploads in Azure Storage über eine Hashzuverlässigkeitsanalyse und verdächtige Zugriffe über einen aktiven Tor-Exitknoten (einen anonymisierenden Proxy) erkannt werden. 

- [Konfigurieren von Azure Defender für Storage](azure-defender-storage-configure.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="29-enable-dns-query-logging"></a>2.9: Aktivieren der DNS-Abfrageprotokollierung

**Leitfaden**: Mit der Azure DNS-Analyse-Lösung (Vorschauversion) in Azure Monitor können Sie Daten zur DNS-Infrastruktur hinsichtlich Sicherheit, Leistung und Betrieb sammeln.  Derzeit werden Azure Storage-Konten nicht unterstützt. Sie können jedoch auch eine Drittanbieterlösung für die DNS-Protokollierung verwenden.  

 
- [Sammeln von Daten zu Ihrer DNS-Infrastruktur mit der DNS-Analyse-Lösung (Vorschauversion)](../../azure-monitor/insights/dns-analytics.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="identity-and-access-control"></a>Identität und Zugriffssteuerung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Identität und Zugriffssteuerung](../../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Verwalten eines Bestands von Administratorkonten

**Leitfaden**: Azure Active Directory (Azure AD) umfasst integrierte Rollen, die explizit zugewiesen werden müssen und abgefragt werden können. Verwenden Sie das Azure AD PowerShell-Modul, um Ad-hoc-Abfragen zum Ermitteln von Konten auszuführen, die Mitglieder von administrativen Gruppen sind.

- [Abrufen einer Verzeichnisrolle in Azure AD mit PowerShell](/powershell/module/azuread/get-azureaddirectoryrole)

- [Abrufen von Mitgliedern einer Verzeichnisrolle in Azure AD mit PowerShell](/powershell/module/azuread/get-azureaddirectoryrolemember)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Ändern von Standardkennwörtern bei Bedarf

**Leitfaden**: Azure Storage-Konten und Azure Active Directory (Azure AD) unterstützen keine Standardkennwörter oder leeren Kennwörter. Azure Storage implementiert ein Zugriffssteuerungsmodell, das die rollenbasierte Zugriffssteuerung von Azure (Azure RBAC) sowie Shared Key und Shared Access Signature (SAS) unterstützt. Ein Merkmal der Shared-Key- und SAS-Authentifizierung ist, dass dem Aufrufer keine Identität zugeordnet wird und daher keine auf Sicherheitsprinzipalberechtigungen basierende Autorisierung erfolgen kann.

- [Autorisierung des Datenzugriffs in Azure Storage](storage-auth.md)

- [Grundlegendes zu Sicherheitsprinzipalen und der Zugriffssteuerung für Azure Storage-Konten](storage-introduction.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Verwenden dedizierter Administratorkonten

**Leitfaden**: Erstellen Sie Standardbetriebsverfahren für die Verwendung dedizierter Administratorkonten, die Zugriff auf Ihr Storage-Konto haben. Verwenden Sie die Identitäts- und Zugriffsverwaltung in Azure Security Center, um die Anzahl der Administratorkonten zu überwachen.

Sie können außerdem den Just-In-Time-/Just-Enough-Zugriff mithilfe von privilegierten Rollen für Microsoft-Dienste von Azure Active Directory Privileged Identity Management und Azure Resource Manager aktivieren.

- [Grundlegendes zu Identität und Zugriff im Azure Security Center](../../security-center/security-center-identity-access.md)

- [Übersicht über Privileged Identity Management](/azure/active-directory/privileged-identity-management/)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4: Verwenden des einmaligen Anmeldens (SSO) in Azure Active Directory

**Leitfaden**: Verwenden Sie nach Möglichkeit einmaliges Anmelden in Azure Active Directory (Azure AD), anstatt einzelne eigenständige Anmeldeinformationen pro Dienst zu konfigurieren. Wenden Sie die Empfehlungen für die Identitäts- und Zugriffsverwaltung in Azure Security Center an.
 

 
- [Grundlegendes zu SSO mit Azure AD](../../active-directory/manage-apps/what-is-single-sign-on.md)
 

 
- [Autorisierung des Datenzugriffs in Azure Storage](storage-auth.md)
 

 
- [Autorisieren des Zugriffs auf Blobs und Warteschlangen mit Azure AD](storage-auth-aad.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Verwenden der mehrstufigen Authentifizierung für den gesamten Azure Active Directory-basierten Zugriff

**Leitfaden**: Aktivieren Sie die mehrstufige Authentifizierung in Azure Active Directory (Azure AD), und befolgen Sie die Empfehlungen zur Identitäts- und Zugriffsverwaltung des Azure Security Center, um Ihre Storage-Kontoressourcen zu schützen.

- [Planen einer Bereitstellung von Azure AD Multi-Factor Authentication](../../active-directory/authentication/howto-mfa-getstarted.md)

- [Überwachen von Identität und Zugriff in Azure Security Center](../../security-center/security-center-identity-access.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3.6: Verwenden von sicheren, von Azure verwalteten Arbeitsstationen für Verwaltungsaufgaben

**Leitfaden**: Verwenden Sie PAWs (Privileged Access Workstations) mit mehrstufiger Authentifizierung, die für die Anmeldung bei Storage-Kontoressourcen und deren Konfiguration konfiguriert sind.  

 
- [Informationen zu Arbeitsstationen mit privilegiertem Zugriff](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/) 

 
- [Planen einer Bereitstellung von Azure AD Multi-Factor Authentication](../../active-directory/authentication/howto-mfa-getstarted.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Protokollieren von und Warnen bei verdächtigen Aktivitäten in Administratorkonten

**Leitfaden**: Senden Sie die Risikoerkennungswarnungen des Azure Security Center an Azure Monitor, und konfigurieren Sie mithilfe von Aktionsgruppen benutzerdefinierte Warnungen oder Benachrichtigungen. Aktivieren Sie das Azure Defender für Storage-Konto, um Warnungen für verdächtige Aktivitäten zu generieren. Nutzen Sie zudem die Risikoerkennung von Azure Active Directory, um Warnungen und Berichte zu riskantem Benutzerverhalten anzuzeigen.

- [Konfigurieren von Azure Defender für Storage](azure-defender-storage-configure.md)
 

- [Grundlegendes zu Azure AD-Risikoerkennungen](../../active-directory/identity-protection/overview-identity-protection.md)
 

- [Konfigurieren von Aktionsgruppen für benutzerdefinierte Warnungen und Benachrichtigungen](../../azure-monitor/alerts/action-groups.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Verwalten von Azure-Ressourcen nur über genehmigte Standorte

**Leitfaden**: Verwenden Sie benannte Standorte mit bedingtem Zugriff, um den Zugriff nur über bestimmte logische Gruppierungen von IP-Adressbereichen oder Ländern/Regionen zuzulassen. 

- [Konfigurieren benannter Standorte in Azure](../../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="39-use-azure-active-directory"></a>3.9: Verwenden von Azure Active Directory

**Leitfaden**: Verwenden Sie Azure Active Directory (Azure AD) als zentrales Authentifizierungs- und Autorisierungssystem. Azure bietet rollenbasierte Zugriffssteuerung in Azure (Azure RBAC, Azure Role-Based Access Control) für eine präzise Steuerung des Zugriffs eines Clients auf Ressourcen in einem Speicherkonto.   Verwenden Sie nach Möglichkeit Azure AD-Anmeldeinformationen anstelle des Kontoschlüssels, da dieser leichter kompromittiert werden kann. Wenn Ihr Anwendungsentwurf Shared Access Signatures (SAS) für den Zugriff auf Blobspeicher erfordert, verwenden Sie Azure AD-Anmeldeinformationen, um nach Möglichkeit eine SAS für die Benutzerdelegierung zu erstellen und damit die Sicherheit zu erhöhen.

- [Erstellen und Konfigurieren einer Azure AD-Instanz](../../active-directory/fundamentals/active-directory-access-create-new-tenant.md) 

- [Verwenden des Azure Storage-Ressourcenanbieters für den Zugriff auf Verwaltungsressourcen](authorization-resource-provider.md)

- [Zuweisen einer Azure-Rolle für den Zugriff auf Blob- und Warteschlangendaten über das Azure-Portal](storage-auth-aad-rbac-portal.md)

- [Autorisierung des Datenzugriffs in Azure Storage](storage-auth.md)

- [Gewähren von eingeschränktem Zugriff auf Azure Storage-Ressourcen mithilfe von SAS (Shared Access Signature)](storage-sas-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Regelmäßiges Überprüfen und Abstimmen des Benutzerzugriffs

**Leitfaden**: Überprüfen Sie die Azure AD-Protokolle (Azure Active Directory), um veraltete Konten zu ermitteln, zu denen auch Storage-Konten mit Administratorrolle zählen können. Verwenden Sie zusätzlich Azure-Identitätszugriffsüberprüfungen, um Gruppenmitgliedschaften, den Zugriff auf Unternehmensanwendungen, die für den Zugriff auf Storage-Kontoressourcen verwendet werden könnten, und Rollenzuweisungen effizient zu verwalten. Der Benutzerzugriff sollte regelmäßig überprüft werden, um sicherzustellen, dass nur die richtigen Benutzer weiterhin Zugriff haben. 
 

 
Sie können auch SAS (Shared Access Signature) verwenden, um den sicheren delegierten Zugriff auf Ressourcen in Ihrem Speicherkonto zu ermöglichen, ohne die Sicherheit Ihrer Daten zu beeinträchtigen. Sie können steuern, auf welche Ressourcen der Client zugreifen kann, welche Berechtigungen er für diese Ressourcen besitzt und wie lange die SAS gültig ist (neben anderen Parametern).
 

 
Überprüfen Sie auch den anonymen Lesezugriff auf Container und Blobs. In der Standardeinstellung kann nur ein Benutzer mit den entsprechenden Berechtigungen auf einen Container und auf darin enthaltene Blobs zugreifen. Mithilfe von Azure Monitor können Sie Warnungen auslösen, wenn anonym auf Storage-Konten zugegriffen wird, indem Sie Bedingungen für die anonyme Authentifizierung einrichten.
 

 
Eine effektive Möglichkeit, das Risiko eines unerwarteten Benutzerkontozugriffs zu reduzieren, besteht darin, die Dauer des Benutzerzugriffs einzuschränken. Zeitlich begrenzte SAS-URIs sorgen zuverlässig dafür, dass der Benutzerzugriff auf Storage-Konten automatisch abläuft. Außerdem können Sie Storage-Kontoschlüssel in regelmäßigen Abständen rotieren, um sicherzustellen, dass der unerwartete Zugriff über Storage-Kontoschlüssel nur für eine begrenzte Dauer möglich ist.
 

 
- [Grundlegendes zur Azure AD-Berichterstellung](/azure/active-directory/reports-monitoring/) 
 

 
- [Anzeigen und Ändern des Zugriffs auf Azure Storage-Kontoebene](storage-auth-aad-rbac-portal.md)
 

 
- [Gewähren von eingeschränktem Zugriff auf Azure Storage-Ressourcen mithilfe von SAS (Shared Access Signature)](storage-sas-overview.md)
 

 
- [Verwalten des anonymen Lesezugriffs auf Container und Blobs](../blobs/anonymous-read-access-configure.md)
 

 
- [Überwachen eines Speicherkontos im Azure-Portal](manage-storage-analytics-logs.md)
 

 
- [Verwalten von Zugriffsschlüsseln für Speicherkonten](storage-account-keys-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Überwachen von Zugriffsversuchen auf deaktivierte Anmeldeinformationen

**Leitfaden**: Verwenden Sie Storage Analytics, um ausführliche Informationen zu erfolgreichen und nicht erfolgreichen Anforderungen an einen Speicherdienst zu protokollieren. Alle Protokolle werden in Block-Blobs in einem Container namens „$logs“ gespeichert, der automatisch erstellt wird, wenn die Speicheranalyse für ein Speicherkonto aktiviert ist.
 

Erstellen Sie Diagnoseeinstellungen für Azure AD-Benutzerkonten (Azure Active Directory), und senden Sie die Überwachungs- und Anmeldeprotokolle an einen Log Analytics-Arbeitsbereich. Sie können gewünschte Warnungen im Log Analytics-Arbeitsbereich konfigurieren.

Zur Überwachung von Authentifizierungsfehlern bei Azure Storage-Konten können Sie Warnungen erstellen, die Sie benachrichtigen, wenn bestimmte Schwellenwerte für Speicherressourcenmetriken erreicht wurden. Mithilfe von Azure Monitor können Sie zudem Warnungen auslösen, wenn anonym auf Storage-Konten zugegriffen wird, indem Sie Bedingungen für die anonyme Authentifizierung einrichten.

- [Azure Storage Analytics-Protokollierung](storage-analytics-logging.md)
 

- [Integrieren von Azure-Aktivitätsprotokollen in Azure Monitor](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)
 

- [Konfigurieren von Metrikwarnungen für Azure Storage-Konten](manage-storage-analytics-logs.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12: Warnung bei abweichendem Verhalten bei der Kontoanmeldung

**Leitfaden**: Mit Azure AD-Features (Azure Active Directory) zum Risiko- und Identitätsschutz können Sie automatische Reaktionen auf erkannte verdächtige Aktionen im Zusammenhang mit Storage-Kontoressourcen konfigurieren. Sie sollten automatisierte Antworten über Azure Sentinel aktivieren, um die Sicherheitsmaßnahmen Ihrer Organisation zu implementieren.

- [Anzeigen riskanter Azure AD-Anmeldungen](../../active-directory/identity-protection/overview-identity-protection.md)

- [Konfigurieren und Aktivieren von Risikorichtlinien für den Identitätsschutz](../../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Durchführen des Onboardings für Azure Sentinel](../../sentinel/quickstart-onboard.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Ermöglichen des Zugriffs auf relevante Kundendaten für Microsoft in Supportszenarien

**Leitfaden**: In Supportszenarios, bei denen Microsoft auf Kundendaten zugreifen muss, wird per Kunden-Lockbox (für Storage-Konten in der Vorschauphase) eine Benutzeroberfläche bereitgestellt, auf der Kunden Anforderungen des Zugriffs auf Kundendaten prüfen und dann genehmigen oder ablehnen können. Microsoft benötigt keinen Zugriff auf die im Storage-Konto gespeicherten Geheimnisse Ihrer Organisation und fordert auch keinen Zugriff darauf an.

- [Grundlegendes zu Kunden-Lockbox](../../security/fundamentals/customer-lockbox-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="data-protection"></a>Datenschutz

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Schutz von Daten](../../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Verwalten eines Bestands an vertraulichen Informationen

**Leitfaden**: Verwenden Sie Tags für die Nachverfolgung von Storage-Kontoressourcen, die vertrauliche Informationen speichern oder verarbeiten. 

- [Erstellen und Verwenden von Tags](../../azure-resource-manager/management/tag-resources.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Isolieren von Systemen, die vertrauliche Informationen speichern oder verarbeiten

**Leitfaden**: Implementieren Sie eine Isolation mit separaten Abonnements, Verwaltungsgruppen und Speicherkonten für einzelne Sicherheitsdomänen wie die Umgebung und die Vertraulichkeit der Daten.  Sie können Ihr Storage-Konto so einschränken, dass die Zugriffsberechtigungen auf Ihre Storage-Konten kontrolliert werden, die von Anwendungen und Unternehmensumgebungen gefordert werden – abhängig vom Typ und der Teilmenge der verwendeten Netzwerke. Wenn Netzwerkregeln konfiguriert wurden, können nur Anwendungen, die Daten über die angegebene Gruppe von Netzwerken anfordern, auf ein Speicherkonto zugreifen. Sie können den Zugriff auf Azure Storage über Azure RBAC (Azure RBAC) steuern.

Sie können auch private Endpunkte konfigurieren, um die Sicherheit zu verbessern, wenn der Datenverkehr zwischen Ihrem virtuellen Netzwerk und dem Dienst über das Microsoft-Backbonenetzwerk übertragen wird, damit dieser nicht dem öffentlichen Internet offengelegt wird.

- [Erstellen zusätzlicher Azure-Abonnements](../../cost-management-billing/manage/create-subscription.md)

- [Erstellen von Verwaltungsgruppen](../../governance/management-groups/create-management-group-portal.md)

- [Erstellen und Verwenden von Tags](../../azure-resource-manager/management/tag-resources.md)

- [Konfigurieren von Azure Storage-Firewalls und virtuellen Netzwerken](storage-network-security.md)

- [Virtual Network-Dienstendpunkte](../../virtual-network/virtual-network-service-endpoints-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Überwachen und Blockieren einer nicht autorisierten Übertragung vertraulicher Informationen

**Leitfaden**: Kennzeichnen Sie Storage-Kontoressourcen, die vertrauliche Informationen speichern oder verarbeiten, mit Tags als vertraulich. Schränken Sie den ausgehenden Netzwerkdatenverkehr für Azure Storage-Konten über Azure Firewall ein, um das Datenverlustrisiko aufgrund von Exfiltration zu verringern.  

Richtlinien für VNET-Dienstendpunkte ermöglichen es Ihnen zudem, ausgehenden virtuellen Netzwerkdatenverkehr zu Azure Storage-Konten über den Dienstendpunkt zu filtern. Außerdem lassen diese die Datenexfiltration nur an bestimmte Azure Storage-Konten zu.

- [Konfigurieren von Azure Storage-Firewalls und virtuellen Netzwerken](../../virtual-network/virtual-network-service-endpoint-policies-overview.md)

- [Richtlinien für VNET-Dienstendpunkte für Azure Storage](../../private-link/tutorial-private-endpoint-storage-portal.md)

- [Grundlegendes zum Schutz von Kundendaten in Azure](../../security/fundamentals/protection-customer-data.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Verschlüsseln aller vertraulichen Informationen während der Übertragung

**Leitfaden**: Sie können die Verwendung von HTTPS erzwingen, indem Sie die sichere Übertragung für das Speicherkonto vorschreiben. Sobald diese Option aktiviert ist, werden Verbindungen über HTTP abgelehnt.  Verwenden Sie darüber hinaus das Azure Security Center und Azure Policy, um eine sichere Übertragung für Ihr Speicherkonto zu erzwingen.

- [Vorschreiben einer sicheren Übertragung in Azure Storage](storage-require-secure-transfer.md)

- [In Security Center überwachte Azure-Sicherheitsrichtlinien](../../security-center/policy-reference.md)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.Storage**:

[!INCLUDE [Resource Policy for Microsoft.Storage 4.4](../../../includes/policy/standards/asb/rp-controls/microsoft.storage-4-4.md)]

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Verwenden eines aktiven Ermittlungstools zur Bestimmung vertraulicher Daten

**Leitfaden**: Die Features zur Datenermittlung sind für Azure Storage-Konten und zugehörige Ressourcen noch nicht verfügbar. Implementieren Sie eine Drittanbieterlösung, wenn dies für Compliancezwecke erforderlich ist. 

- [Grundlegendes zum Schutz von Kundendaten in Azure](../../security/fundamentals/protection-customer-data.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4.6: Verwenden der rollenbasierten Zugriffssteuerung zum Steuern des Zugriffs auf Ressourcen

**Leitfaden**: Azure Active Directory (Azure AD) autorisiert Rechte für den Zugriff auf geschützte Ressourcen über die rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC). Azure Storage bietet eine Reihe von in Azure RBAC integrierten Rollen mit allgemeinen Berechtigungssätzen für den Zugriff auf Blob- und Warteschlangendaten.

- [Zuweisen von Azure RBAC-Rollen für Azure Storage-Konten](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal#assign-azure-roles-using-the-azure-portal)

- [Verwenden des Azure Storage-Ressourcenanbieters für den Zugriff auf Verwaltungsressourcen](authorization-resource-provider.md)

- [Zuweisen einer Azure-Rolle für den Zugriff auf Blob- und Warteschlangendaten über das Azure-Portal](storage-auth-aad-rbac-portal.md)

- [Erstellen und Konfigurieren einer Azure AD-Instanz](../../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

- [Autorisierung des Datenzugriffs in Azure Storage](storage-auth.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Verschlüsseln vertraulicher, ruhender Informationen

**Leitfaden**: Die Azure Storage-Verschlüsselung wird für alle Speicherkonten aktiviert und kann nicht deaktiviert werden. Mit Azure Storage werden Ihre Daten beim Speichern in der Cloud automatisch verschlüsselt. Wenn Sie Daten aus Azure Storage lesen, werden sie von Azure Storage vor der Rückgabe entschlüsselt. Mit der Azure Storage-Verschlüsselung können Sie ruhende Daten schützen, ohne in Anwendungen Code ändern oder hinzufügen zu müssen. 

- [Grundlegendes zur Azure Storage-Verschlüsselung für ruhende Daten](storage-service-encryption.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Protokollieren und Warnen bei Änderungen an wichtigen Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Monitor mit dem Azure-Aktivitätsprotokoll, um Warnungen zu erstellen, die bei Änderungen an Storage-Kontoressourcen ausgegeben werden.  Aktivieren Sie auch die Azure Storage-Protokollierung, um nachzuverfolgen, wie jede für Azure Storage durchgeführte Anforderung autorisiert wurde. Die Protokolle geben an, ob eine Anforderung anonym, mithilfe eines OAuth 2.0-Tokens, mit einem gemeinsam verwendeten Schlüssel oder mithilfe einer SAS (Shared Access Signature) durchgeführt wurde.  Mithilfe von Azure Monitor können Sie zudem Warnungen auslösen, wenn anonym auf Storage-Konten zugegriffen wird, indem Sie Bedingungen für die anonyme Authentifizierung einrichten. 

 
- [Erstellen von Warnungen für Ereignisse des Azure-Aktivitätsprotokolls](../../azure-monitor/alerts/alerts-activity-log.md) 

 
- [Azure Storage Analytics-Protokollierung](storage-analytics-logging.md) 

 
- [Konfigurieren von Metrikwarnungen für Azure Storage-Konten](manage-storage-analytics-logs.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="vulnerability-management"></a>Verwaltung von Sicherheitsrisiken

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Verwaltung von Sicherheitsrisiken](../../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Ausführen automatisierter Scan-Tools für Sicherheitsrisiken

**Leitfaden**: Befolgen Sie die Empfehlungen des Azure Security Center, um die Konfiguration Ihrer Speicherkonten fortlaufend zu überwachen.  

- [Sicherheitsempfehlungen: Referenzhandbuch](../../security-center/recommendations-reference.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4 Vergleichen von kaskadierenden Sicherheitsrisikoscans

**Leitfaden**: Nicht zutreffend, da Microsoft Sicherheitsrisiken auf den zugrunde liegenden Systemen verwaltet, die Storage-Konten unterstützen.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Verwenden eines Risikobewertungsprozesses, um die Behebung von erkannten Sicherheitsrisiken zu priorisieren

**Leitfaden**: Verwenden Sie die Standardrisikobewertungen (Secure Score), die von Azure Security Center bereitgestellt werden. 

- [Grundlegendes zum Secure Score des Azure Security Center](/azure/security-center/security-center-secure-score)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="inventory-and-asset-management"></a>Bestands- und Ressourcenverwaltung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Bestands- und Ressourcenverwaltung](../../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Verwenden der automatisierten Asset Discovery-Lösung

**Leitfaden**: Verwenden Sie Azure Resource Graph, um alle Ressourcen (einschließlich Storage-Konten) innerhalb Ihrer Abonnements abzufragen und zu ermitteln. Stellen Sie sicher, dass Sie über entsprechende (Lese-)Berechtigungen in Ihrem Mandanten verfügen und alle Azure-Abonnements und Ressourcen in Ihren Abonnements auflisten können.

- [Erstellen von Abfragen mit Azure Graph](../../governance/resource-graph/first-query-portal.md)

- [Anzeigen Ihrer Azure-Abonnements](/powershell/module/az.accounts/get-azsubscription)

- [Grundlegendes zu Azure RBAC](../../role-based-access-control/overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="62-maintain-asset-metadata"></a>6.2: Verwalten von Ressourcenmetadaten

**Leitfaden**: Wenden Sie Tags auf Ihre Storage-Konten an, die Metadaten erzeugen, um sie logisch in einer Taxonomie zu organisieren. 

- [Erstellen und Verwenden von Tags](../../azure-resource-manager/management/tag-resources.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Löschen nicht autorisierter Azure-Ressourcen

**Leitfaden**: Verwenden Sie nach Bedarf Tags, Verwaltungsgruppen und separate Abonnements, um Ihre Storage-Konten und die zugehörigen Ressourcen zu organisieren und nachzuverfolgen. Stimmen Sie den Bestand regelmäßig ab, und stellen Sie sicher, dass nicht autorisierte Ressourcen rechtzeitig aus dem Abonnement gelöscht werden. 

Verwenden Sie zudem Azure Defender für Storage zur Erkennung von nicht autorisierten Azure-Ressourcen. 

- [Erstellen zusätzlicher Azure-Abonnements](../../cost-management-billing/manage/create-subscription.md) 

- [Erstellen von Verwaltungsgruppen](../../governance/management-groups/create-management-group-portal.md)

- [Erstellen und Verwenden von Tags](../../azure-resource-manager/management/tag-resources.md)

- [Konfigurieren von Azure Defender für Storage](azure-defender-storage-configure.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4: Definieren und Verwalten des Bestands an genehmigten Azure-Ressourcen

**Leitfaden**: Gemäß Ihren organisatorischen Anforderungen müssen Sie ein Inventar genehmigter Azure-Ressourcen erstellen.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Überwachung auf nicht genehmigte Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy, um Einschränkungen für den Typ der Ressourcen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie dazu die folgenden integrierten Richtliniendefinitionen: 

 
 - Not allowed resource types (Unzulässige Ressourcentypen) 
 
 - Zulässige Ressourcentypen 
 

 
Verwenden Sie darüber hinaus Azure Resource Graph, um Ressourcen in Ihren Abonnements abzufragen und zu ermitteln. Dies kann bei Umgebungen mit hohen Sicherheitsanforderungen hilfreich sein, z. B. Umgebungen mit Speicherkonten. 
 

 
- [Konfigurieren und Verwalten von Azure Policy](../../governance/policy/tutorials/create-and-manage.md) 
 

 
- [Erstellen von Abfragen mit Azure Graph](../../governance/resource-graph/first-query-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Entfernen nicht genehmigter Azure-Ressourcen und Softwareanwendungen

**Leitfaden**: Der Kunde kann die Erstellung oder Verwendung von Ressourcen mit Azure Policy den Unternehmensrichtlinien entsprechend verhindern. 

- [Konfigurieren und Verwalten von Azure Policy](../../governance/policy/tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="69-use-only-approved-azure-services"></a>6.9: Ausschließliche Verwendung genehmigter Azure-Dienste

**Leitfaden**: Verwenden Sie Azure Policy, um Einschränkungen für den Typ der Ressourcen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie dazu die folgenden integrierten Richtliniendefinitionen:

- Not allowed resource types (Unzulässige Ressourcentypen)
- Zulässige Ressourcentypen

Zusätzliche Informationen finden Sie unter den aufgeführten Links.

- [Konfigurieren und Verwalten von Azure Policy](../../governance/policy/tutorials/create-and-manage.md)

- [Ablehnen eines bestimmten Ressourcentyps mit Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#general)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.ClassicStorage**:

[!INCLUDE [Resource Policy for Microsoft.ClassicStorage 6.9](../../../includes/policy/standards/asb/rp-controls/microsoft.classicstorage-6-9.md)]

**Integrierte Azure Policy-Definitionen – Microsoft.Storage**:

[!INCLUDE [Resource Policy for Microsoft.Storage 6.9](../../../includes/policy/standards/asb/rp-controls/microsoft.storage-6-9.md)]

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Einschränken der Möglichkeiten von Benutzern zur Interaktion mit Azure Resource Manager

**Leitfaden**: Verwenden Sie den bedingten Azure-Zugriff, um die Möglichkeiten der Benutzer zur Interaktion mit Azure Resource Manager einzuschränken, indem Sie „Zugriff blockieren“ für die App zur Verwaltung von Microsoft Azure konfigurieren. Dadurch können das Erstellen und Ändern von Ressourcen innerhalb einer Umgebung mit hohen Sicherheitsanforderungen (z. B. mit Storage-Konten) verhindert werden. 

- [Verwalten des Zugriffs auf die Azure-Verwaltung mit bedingtem Zugriff](../../role-based-access-control/conditional-access-azure-management.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="secure-configuration"></a>Sichere Konfiguration

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Sichere Konfiguration](../../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Einrichten sicherer Konfigurationen für alle Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy-Aliase im Namespace **Microsoft.Storage**, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen der Konfiguration Ihrer Storage-Konten zu erstellen. Sie können auch integrierte Azure Policy-Definitionen für Azure Storage-Konten verwenden:

- Nicht eingeschränkten Netzwerkzugriff auf Speicherkonten überwachen
- Bereitstellen von Azure Defender für Storage
- Speicherkonten sollten zu neuen Azure Resource Manager-Ressourcen migriert werden.
- Sichere Übertragung in Speicherkonten sollte aktiviert werden.

Nutzen Sie die Empfehlungen des Azure Security Center als Konfigurationsbaseline für den Schutz Ihrer Storage-Konten.

- [Anzeigen verfügbarer Azure Policy-Aliase](/powershell/module/az.resources/get-azpolicyalias)

- [Konfigurieren und Verwalten von Azure Policy](../../governance/policy/tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Verwalten von sicheren Konfigurationen für Azure-Ressourcen

**Leitfaden**: Verwenden Sie die Azure Policy-Optionen „Deny“ und „DenyIfNotExists“, um sichere Einstellungen für Azure-Ressourcen zu erzwingen. 

- [Konfigurieren und Verwalten von Azure Policy](../../governance/policy/tutorials/create-and-manage.md) 

- [Grundlegendes zu Azure Policy-Auswirkungen](../../governance/policy/concepts/effects.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Sicheres Speichern der Konfiguration von Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Repos, um Ihren Code sicher zu speichern und wie benutzerdefinierte Azure-Richtlinien, Azure Resource Manager-Vorlagen, Desired State Configuration-Skripts usw. zu verwalten. Um auf die Ressourcen zuzugreifen, die Sie in Azure DevOps verwalten, können Sie bestimmten Benutzern, integrierten Sicherheitsgruppen oder in Azure Active Directory (Azure AD) definierten Gruppen (falls in Azure DevOps integriert) oder in Azure AD definierten Gruppen (falls in TFS integriert), Berechtigungen gewähren oder verweigern.

- [Speichern von Code in Azure DevOps](/azure/devops/repos/git/gitworkflow)

- [Informationen über Berechtigungen und Gruppen in Azure DevOps](/azure/devops/organizations/security/about-permissions)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Bereitstellen von Konfigurationsverwaltungstools für Azure-Ressourcen

**Leitfaden**: Nutzen Sie Azure Policy, um Systemkonfigurationen für das Storage-Konto zu melden, zu überwachen und zu erzwingen. Entwickeln Sie außerdem einen Prozess und eine Pipeline zum Verwalten von Richtlinienausnahmen. 

- [ Konfigurieren und Verwalten von Azure Policy](../../governance/policy/tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementieren der automatisierten Konfigurationsüberwachung für Azure-Ressourcen

**Leitfaden**: Nutzen Sie Azure Security Center, um Baselineüberprüfungen für Ihre Azure Storage-Kontoressourcen durchzuführen. 

- [Umsetzen von Empfehlungen in Azure Security Center](../../security-center/security-center-remediate-recommendations.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="711-manage-azure-secrets-securely"></a>7.11: Sicheres Verwalten von Azure-Geheimnissen

**Leitfaden**: Mit Azure Storage werden Ihre Daten beim Speichern in der Cloud automatisch verschlüsselt. Sie können von Microsoft verwaltete Schlüssel für die Verschlüsselung Ihres Speicherkontos nutzen oder die Verschlüsselung mit deren eigenen Schlüsseln verwalten. Wenn Sie vom Kunden bereitgestellte Schlüssel verwenden, können Sie Azure Key Vault nutzen, um die Schlüssel sicher zu speichern. 

Darüber hinaus sollten Sie Speicherkontoschlüssel regelmäßig rotieren, um die Auswirkungen bei Verlust oder Offenlegung von Speicherkontoschlüsseln einzuschränken.

- [Azure Storage encryption for data at rest (Azure Storage-Verschlüsselung für ruhende Daten)](storage-service-encryption.md)

- [Verwalten von Zugriffsschlüsseln für Speicherkonten](storage-account-keys-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Sicheres und automatisches Verwalten von Identitäten

**Leitfaden**: Autorisieren Sie den Zugriffs auf Blobs und Warteschlangen in Azure Storage-Konten mit Azure Active Directory (Azure AD) und verwalteten Identitäten. Azure Blob Storage und Azure Queue Storage unterstützen die Azure AD-Authentifizierung mit verwalteten Identitäten für Azure-Ressourcen. 

Sie können verwaltete Identitäten für Azure-Ressourcen verwenden, um den Zugriff auf Blob- und Warteschlangendaten mithilfe von Azure AD-Anmeldeinformationen über Anwendungen zu autorisieren, die auf Azure Virtual Machines, in Funktions-Apps, in VM-Skalierungsgruppen und anderen Diensten ausgeführt werden. Durch Verwendung von verwalteten Identitäten für Azure-Ressourcen zusammen mit der Azure AD-Authentifizierung können Sie vermeiden, dass Anmeldeinformationen mit den in der Cloud ausgeführten Anwendungen gespeichert werden.

- [Gewähren von Zugriff auf Azure-Blobdaten und -Warteschlangendaten mithilfe einer verwalteten Identität](storage-auth-aad-rbac-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Beheben der unbeabsichtigten Offenlegung von Anmeldeinformationen

**Anleitung:** Implementieren Sie Credential Scanner, um Anmeldeinformationen im Code zu identifizieren. In Credential Scanner wird auch das Verschieben von ermittelten Anmeldeinformationen an sicherere Speicherorte (z. B. Azure Key Vault) empfohlen. 

- [Einrichten von Credential Scanner](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="malware-defense"></a>Schutz vor Schadsoftware

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Schutz vor Schadsoftware](../../security/benchmarks/security-control-malware-defense.md).*

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: Vorabprüfen von in computefremde Azure-Ressourcen hochzuladenden Dateien

**Leitfaden**: Nutzen Sie Azure Defender für Storage, damit Schadsoftwareuploads in Azure Storage über eine Hashzuverlässigkeitsanalyse und verdächtige Zugriffe über einen aktiven Tor-Exitknoten (einen anonymisierenden Proxy) erkannt werden. 
 

 
Sie können auch sämtliche Inhalte auf Schadsoftware überprüfen, bevor diese auf Nicht-Compute-Ressourcen in Azure (wie App Service, Data Lake Storage, Blob Storage usw.) hochgeladen werden, um die Anforderungen Ihrer Organisation zu erfüllen.
 

 
- [Konfigurieren von Azure Defender für Storage](azure-defender-storage-configure.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="data-recovery"></a>Datenwiederherstellung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Datenwiederherstellung](../../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Sicherstellen regelmäßiger automatisierter Sicherungen

**Leitfaden**: Die Daten in Ihrem Microsoft Azure-Speicherkonto werden stets automatisch repliziert, um Beständigkeit und Hochverfügbarkeit sicherzustellen. Azure Storage kopiert Ihre Daten so, dass sie vor geplanten und ungeplanten Ereignissen geschützt sind – von vorübergehend auftretenden Hardwarefehlern und Netzwerk- oder Stromausfällen bis zu schweren Naturkatastrophen usw. Sie können Ihre Daten wahlweise im selben Rechenzentrum, Rechenzentren in derselben Region und sogar übergreifend über geografisch getrennte Regionen replizieren. 

Sie können auch Azure Automation aktivieren, um regelmäßig Momentaufnahmen Ihrer Blobs zu erstellen.

- [Grundlegendes zu Azure Storage-Redundanz und Vereinbarungen zum Servicelevel](storage-redundancy.md)

- [Erstellen einer Momentaufnahme eines Blobs](/rest/api/storageservices/creating-a-snapshot-of-a-blob)

- [Azure Automation – Übersicht](../../automation/automation-intro.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Durchführen vollständiger Systemsicherungen und Sichern aller kundenseitig verwalteten Schlüssel

**Leitfaden**: Es gibt mehrere Methoden, um Daten aus Diensten zu sichern, die von Speicherkonten unterstützt werden, zum Beispiel die Verwendung von AzCopy oder Drittanbietertools. Unveränderlicher Speicher für Azure-Blobspeicher ermöglicht es Benutzern, unternehmenskritische Datenobjekte im WORM-Zustand (Write Once, Read Many – Einmal schreiben, oft lesen) zu speichern. In diesem Zustand sind die Daten für einen vom Benutzer angegebenen Zeitraum nicht löschbar und nicht änderbar.
 

Kundenseitig verwaltete/bereitgestellte Schlüssel können innerhalb von Azure Key Vault über die Azure CLI oder PowerShell gesichert werden. 

 
- [Erste Schritte mit AzCopy](storage-use-azcopy-v10.md)  

- [Festlegen und Verwalten von Unveränderlichkeitsrichtlinien für Blobspeicher](../blobs/storage-blob-immutability-policies-manage.md)
 

 
- [Sichern von Schlüsseltresorschlüsseln in Azure](/powershell/module/az.keyvault/backup-azkeyvaultkey)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Überprüfen aller Sicherungen einschließlich kundenseitig verwalteter Schlüssel

**Anleitung:** Führen Sie mithilfe der folgenden PowerShell-Befehle regelmäßig Datenwiederherstellungen Ihrer Key Vault-Zertifikate, -Schlüssel und -Geheimnisse und der verwalteten Speicherkonten durch:

Restore-AzKeyVaultCertificate

Restore-AzKeyVaultKey

Restore-AzKeyVaultManagedStorageAccount

Restore-AzKeyVaultSecret

- [Wiederherstellen von Key Vault-Zertifikaten](/powershell/module/az.keyvault/restore-azkeyvaultcertificate)

- [Wiederherstellen von Key Vault-Schlüsseln](/powershell/module/az.keyvault/restore-azkeyvaultkey)

- [Wiederherstellen von per Key Vault verwalteten Storage-Konten](/powershell/module/az.keyvault/backup-azkeyvaultmanagedstorageaccount)

- [Wiederherstellen von Key Vault-Geheimnissen](/powershell/module/az.keyvault/restore-azkeyvaultsecret)

- [AzCopy ist ein Befehlszeilenhilfsprogramm, das Sie verwenden können, um Blobs, Dateien und Tabellendaten in oder aus einem Speicherkonto zu kopieren.](storage-use-azcopy-v10.md)

Hinweis: Wenn Sie Daten in und aus Ihrem Azure-Tabellenspeicherdienst kopieren möchten, installieren Sie Version 7.3 von AzCopy.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Sicherstellen des Schutzes von Sicherungen und von kundenseitig verwalteten Schlüsseln

**Leitfaden**: Zum Aktivieren von kundenseitig verwalteten Schlüsseln in einem Speicherkonto müssen Sie die Schlüssel in einer Azure Key Vault-Instanz speichern. Sie müssen die Eigenschaften „Vorläufiges Löschen“ und „Do Not Purge“ (Nicht bereinigen) im Schlüsseltresor aktivieren.  Das Key Vault-Feature „Vorläufiges Löschen“ ermöglicht die Wiederherstellung gelöschter Tresore und Tresorobjekte wie Schlüssel, Geheimnisse und Zertifikate.  Wenn Sie Daten aus Storage-Konten in Azure Storage-Blobs speichern, sollten Sie das vorläufige Löschen aktivieren, um Ihre Daten speichern und wiederherstellen zu können, wenn Blobs oder Blobmomentaufnahmen gelöscht werden.  Sie sollten Ihre Sicherungen als vertrauliche Daten behandeln und die relevanten Zugriffs- und Datenschutzsteuerungen als Teil dieser Baseline anwenden.  Sie können unternehmenskritische Datenobjekte zudem in einem WORM-Zustand (Write Once, Read Many) speichern, um diese noch besser zu schützen.

- [Verwenden des vorläufigen Löschens in Azure Key Vault](../../key-vault/general/key-vault-recovery.md)

- [Vorläufiges Löschen für Azure Storage-Blobs](../blobs/soft-delete-blob-overview.md)

- [Speichern unternehmenskritischer Blobdaten mit unveränderlichem Speicher](../blobs/storage-blob-immutable-storage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="incident-response"></a>Reaktion auf Vorfälle

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Reaktion auf Vorfälle](../../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Erstellen eines Leitfadens für die Reaktion auf Vorfälle

**Anleitung:** Erarbeiten Sie einen Leitfaden für die Reaktion auf Vorfälle für Ihre Organisation. Stellen Sie sicher, dass es schriftliche Pläne für die Reaktion auf Vorfälle gibt, in denen alle Rollen der Mitarbeiter sowie die Phasen der Bearbeitung und Verwaltung von Vorfällen von der Ermittlung bis zur abschließenden Überprüfung definiert sind.

- [Anleitung zum Entwickeln eines Prozesses für die Reaktion auf Sicherheitsvorfälle](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Struktur eines Vorfalls laut Microsoft Security Response Center](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

- [Computer Security Incident Handling Guide](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final) (Leitfaden für den Umgang mit Computersicherheitsincidents) des NIST (National Institute of Standards and Technology)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Erstellen eines Verfahrens zur Bewertung und Priorisierung von Vorfällen

**Anleitung:** Security Center weist jeder Warnung einen Schweregrad zu, damit Sie priorisieren können, welche Warnungen zuerst untersucht werden sollen. Der Schweregrad basiert darauf, wie zuversichtlich Security Center in Bezug auf den Befund oder die Analyse ist, die zum Auslösen der Warnung verwendet wird, sowie auf dem Zuverlässigkeitsgrad, dass hinter der Aktivität, die zu der Warnung führte, eine böswillige Absicht stand. 

 
Kennzeichnen Sie außerdem die Abonnements (z. B. Produktion, Nicht-Produktion) mithilfe von Tags, und erstellen Sie ein Namenssystem, um Azure-Ressourcen eindeutig zu identifizieren und zu kategorisieren, insbesondere solche, die vertrauliche Daten verarbeiten.  Die Priorisierung der Behebung von Warnungen basierend auf der Wichtigkeit der Azure-Ressourcen und der Umgebung, in der der Vorfall aufgetreten ist, liegt in Ihrer Verantwortung.
 

 
- [Sicherheitswarnungen in Azure Security Center](../../security-center/security-center-alerts-overview.md)
 

 
- [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../../azure-resource-manager/management/tag-resources.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="103-test-security-response-procedures"></a>10.3: Verfahren zum Testen der Reaktion auf Sicherheitsvorfälle

**Leitfaden**: Führen Sie in regelmäßigen Abständen Übungen durch, um die Reaktionsfähigkeit Ihrer Systeme zu testen und so Ihre Azure-Ressourcen zu schützen. Identifizieren Sie Schwachstellen und Lücken, und überarbeiten Sie den Plan bei Bedarf.

- [NIST-Veröffentlichung: Leitfaden zum Testen, Trainieren und Ausführen von Programmen für IT-Pläne und -Funktionen](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Angeben von Kontaktdaten für Sicherheitsvorfälle und Konfigurieren von Warnungsbenachrichtigungen für Sicherheitsvorfälle

**Leitfaden**: Microsoft kontaktiert Sie unter den für Sicherheitsvorfälle angegebenen Kontaktdaten, wenn das Microsoft Security Response Center (MSRC) feststellt, dass Personen unrechtmäßig oder unbefugt auf Ihre Daten zugegriffen haben. Überprüfen Sie die Vorfälle anschließend, um sicherzustellen, dass die Probleme behoben wurden.

- [Festlegen der Kontaktinformationen in Azure Security Center](../../security-center/security-center-provide-security-contact-details.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Integrieren von Sicherheitswarnungen in das System zur Reaktion auf Vorfälle

**Leitfaden**: Exportieren Sie die Azure Security Center-Warnungen und -Empfehlungen über die Funktion „Fortlaufender Export“, um Risiken für Azure-Ressourcen zu ermitteln. Über „Fortlaufender Export“ können Sie Warnungen und Empfehlungen entweder manuell oder kontinuierlich exportieren. Sie können den Azure Security Center-Datenconnector verwenden, um die Warnungen an Azure Sentinel zu streamen.

- [Konfigurieren des fortlaufenden Exports](../../security-center/continuous-export.md)

- [Streamen von Warnungen in Azure Sentinel](../../sentinel/connect-azure-security-center.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatisieren der Reaktion auf Sicherheitswarnungen

**Leitfaden**: Verwenden Sie die Funktion „Workflowautomatisierung“ in Azure Security Center, um über Logic Apps automatisch Reaktionen auf Sicherheitswarnungen und -empfehlungen auszulösen und dadurch Ihre Azure-Ressourcen noch besser zu schützen.
    

- [ Konfigurieren von Workflowautomatisierung und Logic Apps](../../security-center/workflow-automation.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="penetration-tests-and-red-team-exercises"></a>Penetrationstests und Red Team-Übungen

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Penetrationstests und Red Team-Übungen](../../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1: Durchführen regelmäßiger Penetrationstests Ihrer Azure-Ressourcen und Sicherstellen der Behebung aller kritischen Sicherheitsergebnissen

**Leitfaden**: Befolgen Sie die Microsoft Rules of Engagement, um sicherzustellen, dass Ihre Penetrationstests nicht gegen Microsoft-Richtlinien verstoßen. Nutzen Sie die Microsoft-Strategie und die Red Teaming- und Livewebsite-Penetrationstests für von Microsoft verwaltete Cloudinfrastrukturen, Dienste und Anwendungen.

- [Penetrationstests – Rules of Engagement](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

- [Microsoft Cloud Red Teaming](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die [Übersicht über Version 2 des Azure-Sicherheitsvergleichstests](/azure/security/benchmarks/overview) an.
- Erfahren Sie mehr über [Azure-Sicherheitsbaselines](/azure/security/benchmarks/security-baselines-overview).
