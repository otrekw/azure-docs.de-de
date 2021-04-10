---
title: Azure-Sicherheitsbaseline für Azure Cosmos DB
description: Die Azure Cosmos DB-Sicherheitsbaseline enthält ausführliche Anleitungen und Ressourcen für die Implementierung der Sicherheitsempfehlungen, die im Azure-Sicherheitsvergleichstest angegeben sind.
author: msmbaldwin
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: dead43f2e9f2e8913bcebde43d543b8df8d33ced
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105565672"
---
# <a name="azure-security-baseline-for-azure-cosmos-db"></a>Azure-Sicherheitsbaseline für Azure Cosmos DB

Diese Sicherheitsbaseline wendet Empfehlungen des [Azure-Sicherheitsvergleichstests Version 1.0](../security/benchmarks/overview-v1.md) auf Azure Cosmos DB an. Der Azure-Sicherheitsvergleichstest enthält Empfehlungen zum Schutz Ihrer Cloudlösungen in Azure.
Der Inhalt wird nach den **Sicherheitskontrollen** gruppiert, die durch den Azure-Sicherheitsvergleichstest und die entsprechenden für Azure Cosmos DB geltenden Empfehlungen definiert sind. Nicht auf Azure Cosmos DB anwendbare **Kontrollen** wurden ausgeschlossen.

 
Die vollständige Zuordnung von Azure Cosmos DB zum Azure-Sicherheitsvergleichstest finden Sie in der [vollständigen Zuordnungsdatei der Azure Cosmos DB-Sicherheitsbaseline](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Netzwerksicherheit

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Netzwerksicherheit](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Schützen von Azure-Ressourcen in virtuellen Netzwerken

**Leitfaden**: Durch Verwendung von Azure Private Link können Sie eine Verbindung mit einem Azure Cosmos-Konto über einen privaten Endpunkt herstellen. Der Datenverkehr zwischen Ihrem virtuellen Netzwerk und dem Dienst wird über das Microsoft-Backbone-Netzwerk übertragen und dadurch vom öffentlichen Internet isoliert. Sie können das Risiko der Datenexfiltration auch verringern, indem Sie einen strengen Satz an Ausgangsregeln für eine Netzwerksicherheitsgruppe (NSG) konfigurieren und diese NSG Ihrem Subnetz zuordnen.

Sie können auch Dienstendpunkte verwenden, um Ihr Azure Cosmos-Konto zu sichern. Indem Sie einen Dienstendpunkt aktivieren, können Sie Azure Cosmos-Konten so konfigurieren, dass der Zugriff nur aus einem bestimmten Subnetz eines virtuellen Azure-Netzwerks zugelassen wird. Sobald der Azure Cosmos DB-Dienstendpunkt aktiviert ist, können Sie den Zugriff auf ein Azure Cosmos-Konto mit Verbindungen von einem Subnetz in ein virtuelles Netzwerk einschränken.

Sie können die in Ihrem Azure Cosmos-Konto gespeicherten Daten auch mithilfe von IP-Firewalls sichern. Azure Cosmos DB unterstützt die IP-basierte Zugriffssteuerung zur Unterstützung der Firewall für eingehende Verbindungen. Sie können eine IP-Firewall im Azure Cosmos-Konto über das Azure-Portal, mit Azure Resource Manager-Vorlagen oder der Azure CLI bzw Azure PowerShell festlegen.

- [Übersicht über Azure Private Link](../private-link/private-link-overview.md)

- [Konfigurieren eines privaten Endpunkts für Azure Cosmos DB](how-to-configure-private-endpoints.md) 

- [Erstellen einer Netzwerksicherheitsgruppe mit einer Sicherheitskonfiguration](../virtual-network/tutorial-filter-network-traffic.md)

- [Konfigurieren der IP-Firewall in Cosmos DB](how-to-configure-firewall.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/azure/governance/policy/samples/azure-security-benchmark) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/azure/security-center/security-center-recommendations). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/azure/security-center/azure-defender)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.DocumentDB**:

[!INCLUDE [Resource Policy for Microsoft.DocumentDB 1.1](../../includes/policy/standards/asb/rp-controls/microsoft.documentdb-1-1.md)]

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2: Überwachen und Protokollieren der Konfiguration und des Datenverkehrs von virtuellen Netzwerken, Subnetzen und Netzwerkschnittstellen

**Leitfaden**: Verwenden Sie Azure Security Center, und befolgen Sie die Empfehlungen für den Netzwerkschutz, um Netzwerkressourcen, die sich auf Ihr Azure Cosmos-Konto beziehen, abzusichern.

Wenn virtuelle Computer im gleichen virtuellen Netzwerk wie Ihr Azure Cosmos-Konto bereitgestellt werden, können Sie eine Netzwerksicherheitsgruppe (NSG) verwenden, um das Risiko der Datenexfiltration zu verringern. Aktivieren Sie NSG-Flussprotokolle, und senden Sie Protokolle zwecks Datenverkehrsüberwachung an ein Azure Storage-Konto. Sie können auch NSG-Flussprotokolle an einen Log Analytics-Arbeitsbereich senden und Traffic Analytics verwenden, um Einblicke in den Datenverkehrsfluss in Ihrer Azure-Cloud zu ermöglichen. Einige Vorteile von Traffic Analytics sind die Möglichkeit, die Netzwerkaktivität zu visualisieren und Hotspots zu erkennen, Sicherheitsbedrohungen zu identifizieren, Datenverkehrsflussmuster zu verstehen und Netzwerkfehlkonfigurationen zu ermitteln.

- [Grundlegendes zu der von Azure Security Center bereitgestellten Netzwerksicherheit](../security-center/security-center-network-recommendations.md)

- [Aktivieren der NSG-Flussprotokolle](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Aktivieren und Verwenden von Traffic Analytics](../network-watcher/traffic-analytics.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="13-protect-critical-web-applications"></a>1.3: Schützen kritischer Webanwendungen

**Leitfaden**: Verwenden Sie das Feature CORS (Cross Origin Resource Sharing; Ressourcenfreigabe zwischen verschiedenen Ursprüngen), um einer Webanwendung, die in einer Domäne ausgeführt wird, den Zugriff auf Ressourcen in einer anderen Domäne zu ermöglichen. In Webbrowser ist eine Sicherheitseinschränkung implementiert, die als „Same Origin Policy“ bekannt ist und verhindert, dass eine Webseite APIs in einer anderen Domäne aufruft. CORS bietet jedoch eine sichere Methode, um der Ursprungsdomäne den Aufruf von APIs in einer anderen Domäne zu erlauben. Nachdem Sie die CORS-Unterstützung für Ihr Azure Cosmos-Konto aktiviert haben, werden nur authentifizierte Anforderungen ausgewertet, um festzustellen, ob sie entsprechend den angegebenen Regeln zulässig sind.

- [Konfigurieren der Ressourcenfreigabe zwischen verschiedenen Ursprüngen](how-to-configure-cross-origin-resource-sharing.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Ablehnen der Kommunikation mit bekannten schädlichen IP-Adressen

**Leitfaden**: Verwenden Sie Advanced Threat Protection (ATP) für Azure Cosmos DB. ATP für Azure Cosmos DB bietet eine zusätzliche Ebene von Security Intelligence, die ungewöhnliche und möglicherweise schädliche Versuche, auf Azure Cosmos DB-Konten zuzugreifen oder diese unbefugt zu nutzen, erkennt. Mit dieser Schutzebene können Sie Bedrohungen begegnen und diese in zentrale Sicherheitsüberwachungssysteme integrieren.

Aktivieren Sie DDoS Protection Standard in den virtuellen Netzwerken für Ihre Azure Cosmos DB-Instanzen, um sich vor DDoS-Angriffen zu schützen. Verwenden Sie die in Azure Security Center integrierte Threat Intelligence, um die Kommunikation mit bekannten schädlichen oder nicht genutzten IP-Adressen zu verweigern.

- [Konfigurieren von Advanced Threat Protection für Azure Cosmos DB](cosmos-db-advanced-threat-protection.md)

- [Konfigurieren von DDoS-Schutz](../ddos-protection/manage-ddos-protection.md)

- [Grundlegendes zur integrierten Threat Intelligence in Azure Security Center](../security-center/azure-defender.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="15-record-network-packets"></a>1.5: Aufzeichnen von Netzwerkpaketen

**Leitfaden**: Aktivieren Sie Netzwerksicherheitsgruppen-Flussprotokolle (NSG), und senden Sie Protokolle an ein Speicherkonto, um den Datenverkehr zu überwachen. Sie können auch NSG-Flussprotokolle an einen Log Analytics-Arbeitsbereich senden und Traffic Analytics verwenden, um Einblicke in den Datenverkehrsfluss in Ihrer Azure-Cloud zu ermöglichen. Einige Vorteile von Traffic Analytics sind die Möglichkeit, die Netzwerkaktivität zu visualisieren und Hotspots zu erkennen, Sicherheitsbedrohungen zu identifizieren, Datenverkehrsflussmuster zu verstehen und Netzwerkfehlkonfigurationen zu ermitteln.

- [Aktivieren der NSG-Flussprotokolle](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Aktivieren und Verwenden von Traffic Analytics](../network-watcher/traffic-analytics.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Bereitstellen von netzwerkbasierten Angriffserkennungs-/Eindringschutzsystemen (Intrusion Detection/Intrusion Prevention Systems, IDS/IPS)

**Leitfaden**: Verwenden Sie Advanced Threat Protection (ATP) für Azure Cosmos DB. ATP für Azure Cosmos DB bietet eine zusätzliche Ebene von Security Intelligence, die ungewöhnliche und möglicherweise schädliche Versuche, auf Azure Cosmos DB-Konten zuzugreifen oder diese unbefugt zu nutzen, erkennt. Mit dieser Schutzebene können Sie Bedrohungen begegnen und diese in zentrale Sicherheitsüberwachungssysteme integrieren. 

- [Konfigurieren von Advanced Threat Protection für Cosmos DB](cosmos-db-advanced-threat-protection.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimieren der Komplexität und des Verwaltungsaufwands von Netzwerksicherheitsregeln

**Leitfaden**: Verwenden Sie für Ressourcen, die Zugriff auf Ihr Azure Cosmos-Konto benötigen, Diensttags in virtuellen Netzwerken, um Netzwerkzugriffssteuerungen für Netzwerksicherheitsgruppen oder Azure Firewall zu definieren. Sie können Diensttags anstelle von spezifischen IP-Adressen nutzen, wenn Sie Sicherheitsregeln erstellen. Wenn Sie den Diensttagnamen (z. B. „AzureCosmosDB“) im entsprechenden Quell- oder Zielfeld einer Regel angeben, können Sie den Datenverkehr für den entsprechenden Dienst zulassen oder verweigern. Microsoft verwaltet die Adresspräfixe, für die das Diensttag gilt, und aktualisiert das Diensttag automatisch, wenn sich die Adressen ändern.

- [Weitere Informationen zur Verwendung von Diensttags](../virtual-network/service-tags-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Beibehalten von Standardsicherheitskonfigurationen für Netzwerkgeräte

**Leitfaden**: Definieren und implementieren Sie Standardsicherheitskonfigurationen für Netzwerkressourcen mit Azure Policy. Verwenden Sie Azure Policy-Aliase in den Namespaces „Microsoft.DocumentDB“ und „Microsoft.Network“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen der Netzwerkkonfiguration Ihrer Azure Cosmos DB-Instanzen zu erstellen. Sie können auch die integrierten Richtliniendefinitionen für Azure Cosmos DB verwenden, z. B.:

- Advanced Threat Protection für Cosmos DB-Konten bereitstellen

- Cosmos DB sollte einen VNET-Dienstendpunkt verwenden

Sie können auch Azure Blueprints verwenden, um umfangreiche Azure-Bereitstellungen zu vereinfachen. Dabei werden wichtige Umgebungsartefakte, z. B. Azure Resource Manager-Vorlagen, rollenbasierte Zugriffssteuerung von Azure (Azure RBAC) und Richtlinien, gemeinsam in einer Blaupausendefinition gepackt. Sie können die Blaupause ganz einfach auf neue Abonnements und Umgebungen anwenden und die Steuerung und Verwaltung durch die Versionsverwaltung optimieren.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Erstellen einer Azure-Blaupause](../governance/blueprints/create-blueprint-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="110-document-traffic-configuration-rules"></a>1.10: Dokumentieren von Datenverkehrskonfigurationsregeln

**Leitfaden**: Verwenden Sie Tags für Netzwerkressourcen, die Ihrer Azure Cosmos DB-Bereitstellung zugeordnet sind, um sie in einer Taxonomie logisch zu organisieren.

- [Erstellen und Verwenden von Tags](../azure-resource-manager/management/tag-resources.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Verwenden automatisierter Tools zum Überwachen von Netzwerkressourcenkonfigurationen und Erkennen von Änderungen

**Leitfaden**: Verwenden Sie das Azure-Aktivitätsprotokoll zum Überwachen der Konfigurationen von Netzwerkressourcen und zum Erkennen von Änderungen bei Netzwerkressourcen, die sich auf Ihre Azure Cosmos DB-Instanzen beziehen. Erstellen Sie Warnungen in Azure Monitor, die bei Änderungen an wichtigen Netzwerkressourcen ausgelöst werden. 

- [Anzeigen und Abrufen von Azure-Aktivitätsprotokollereignissen](../azure-monitor/essentials/activity-log.md#view-the-activity-log)

- [Erstellen von Warnungen in Azure Monitor](../azure-monitor/alerts/alerts-activity-log.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="logging-and-monitoring"></a>Protokollierung und Überwachung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Protokollierung und Überwachung](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Konfigurieren der zentralen Sicherheitsprotokollverwaltung

**Leitfaden**: Erfassen Sie Protokolle über Azure Monitor, um die von Azure Cosmos DB generierten Sicherheitsdaten zu aggregieren. Verwenden Sie in Azure Monitor Log Analytics-Arbeitsbereiche zum Abfragen und Ausführen von Analysen, und verwenden Sie Azure Storage-Konten für langfristige Speicherung/Archivierung. Alternativ dazu können Sie auch Daten in Azure Sentinel oder eine Security Incident & Event Management-Lösung (SIEM) eines Drittanbieters integrieren. 

- [Aktivieren von Diagnoseprotokollen für Azure Cosmos DB](./monitor-cosmos-db.md)

- [Durchführen des Onboardings für Azure Sentinel](../sentinel/quickstart-onboard.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Aktivieren der Überwachungsprotokollierung für Azure-Ressourcen

**Leitfaden**: Aktivieren Sie die Diagnoseeinstellungen für Azure Cosmos DB, und senden Sie die Protokolle an einen Log Analytics-Arbeitsbereich oder ein Azure-Speicherkonto. Diagnoseeinstellungen in Azure Cosmos DB werden zum Sammeln von Ressourcenprotokollen verwendet. Diese Protokolle werden pro Anforderung erfasst und werden auch als **Datenebenenprotokolle** bezeichnet. Einige Beispiele für Datenebenenvorgänge sind „delete“, „insert“ und „read“. 

Sie können auch die Diagnoseeinstellungen für das Azure-Aktivitätsprotokoll aktivieren und diese Protokolle an denselben Log Analytics-Arbeitsbereich senden, den Sie für Azure Cosmos DB-Protokolle verwenden.

- [Aktivieren von Diagnoseeinstellungen für Azure Cosmos DB](./monitor-cosmos-db.md)

- [Erfassen und Analysieren des Azure-Aktivitätsprotokolls in Azure Monitor](../azure-monitor/essentials/activity-log.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="25-configure-security-log-storage-retention"></a>2.5: Konfigurieren der Sicherheitsprotokoll-Aufbewahrungsdauer im Speicher

**Leitfaden**: Legen Sie in Azure Monitor die Protokollaufbewahrungsdauer für Log Analytics-Arbeitsbereiche, die Ihrer Azure Cosmos DB-Instanz zugeordnet sind, gemäß den Compliancevorschriften Ihres Unternehmens fest.

- [Ändern des Datenaufbewahrungszeitraums](../azure-monitor/logs/manage-cost-storage.md#change-the-data-retention-period)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="26-monitor-and-review-logs"></a>2.6: Überwachen und Überprüfen von Protokollen

**Leitfaden**: Sie können Abfragen in Log Analytics durchführen. Dies ist ein Arbeitsbereich, in dem Sie Suchbegriffe ermitteln, Trends ausmachen, Muster analysieren und viele weitere Erkenntnisse auf Grundlage der Azure Cosmos DB-Protokolle gewinnen können, die Sie gesammelt haben.

- [Ausführen von Abfragen für Azure Cosmos DB in Log Analytics-Arbeitsbereichen](monitor-cosmos-db.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Aktivieren von Warnungen bei anomalen Aktivitäten

**Leitfaden**: Aktivieren Sie Advanced Threat Protection für Azure Cosmos DB in Azure Security Center, um anomale Aktivitäten in Sicherheitsprotokollen und -ereignissen zu überwachen. Aktivieren Sie die Diagnoseeinstellungen in Azure Cosmos DB, und senden Sie Protokolle an einen Log Analytics-Arbeitsbereich.

 

Integrieren Sie Ihren Log Analytics-Arbeitsbereich in Azure Sentinel, da er eine Lösung für die Sicherheitsorchestrierung mit automatisierter Reaktion (Security Orchestration Automated Response, SOAR) bereitstellt. Dadurch können Playbooks (automatisierte Lösungen) erstellt und zum Beheben von Sicherheitsproblemen verwendet werden. Darüber hinaus können Sie benutzerdefinierte Protokollwarnungen in Ihrem Log Analytics-Arbeitsbereich mithilfe von Azure Monitor erstellen.

- [Liste der Bedrohungsschutzwarnungen für Azure Cosmos DB](../security-center/alerts-reference.md#alerts-azurecosmos)

- [Durchführen des Onboardings für Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Erstellen, Anzeigen und Verwalten von Protokollwarnungen mithilfe von Azure Monitor](../azure-monitor/alerts/alerts-log.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="identity-and-access-control"></a>Identität und Zugriffssteuerung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Identität und Zugriffssteuerung](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Verwalten eines Bestands von Administratorkonten

**Leitfaden**: Sie können die Identitäts- und Zugriffssteuerung (Identity and Access control, IAM) im Azure-Portal verwenden, um die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) zu konfigurieren und die Inventur an Azure Cosmos DB Ressourcen aufrechtzuerhalten. Die Rollen werden Benutzern, Gruppen, Dienstprinzipalen und verwalteten Identitäten in Azure Active Directory (Azure AD) zugewiesen. Sie können für Einzelbenutzer und Gruppen integrierte oder benutzerdefinierte Rollen verwenden.

Azure Cosmos DB bietet für gängige Verwaltungsszenarien eine integrierte Azure RBAC. Eine Person mit einem Profil in Azure AD kann diese Azure-Rollen Benutzern, Gruppen, Dienstprinzipalen oder verwalteten Identitäten zuweisen, um den Zugriff auf Ressourcen und Vorgänge für Azure Cosmos DB-Ressourcen zu gewähren oder zu verweigern.

Sie können auch das Azure AD PowerShell-Modul verwenden, um Ad-hoc-Abfragen zum Ermitteln von Konten durchzuführen, die Mitglieder von Verwaltungsgruppen sind.

Darüber hinaus können einige Aktionen in Azure Cosmos DB mit Azure AD und kontospezifischen Hauptschlüsseln gesteuert werden. Verwenden Sie die Kontoeinstellung „disableKeyBasedMetadataWriteAccess“, um den Schlüsselzugriff zu steuern.

- [Informationen zur rollenbasierten Zugriffssteuerung in Azure Cosmos DB](role-based-access-control.md)

- [Erstellen eigener benutzerdefinierter Rollen mithilfe von Azure Cosmos DB-Aktionen (Microsoft.DocumentDB-Namespace)](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb)

- [Erstellen einer neuen Gruppe in Azure AD](../role-based-access-control/custom-roles.md)

- [Abrufen einer Verzeichnisrolle in Azure AD mit PowerShell](/powershell/module/azuread/get-azureaddirectoryrole?preserve-view=true&view=azureadps-2.0)

- [Abrufen von Mitgliedern einer Verzeichnisrolle in Azure AD mit PowerShell](/powershell/module/azuread/get-azureaddirectoryrolemember?preserve-view=true&view=azureadps-2.0)

- [Beschränken des Benutzerzugriffs ausschließlich auf Datenvorgänge](how-to-restrict-user-data.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Ändern von Standardkennwörtern bei Bedarf

**Leitfaden**: Das Konzept von Standardkennwörtern oder leeren Kennwörtern gibt es in Bezug auf Azure Active Directory (Azure AD) und Azure Cosmos DB nicht. Stattdessen verwendet Azure Cosmos DB zwei Arten von Schlüsseln, um Benutzer zu authentifizieren und den Zugriff auf Daten und Ressourcen zu ermöglichen: Hauptschlüssel und Ressourcentoken. Die Schlüssel können jederzeit neu generiert werden.

- [Informationen zum sicheren Zugriff auf Daten in Azure Cosmos DB](secure-access-to-data.md)

- [Erneutes Generieren von Azure Cosmos DB-Schlüsseln](./manage-with-powershell.md#regenerate-keys)

- [Programmgesteuerter Zugriff auf Schlüssel mit Azure AD](certificate-based-authentication.md)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Verwenden dedizierter Administratorkonten

**Leitfaden**: Nicht zutreffend; Azure Cosmos DB unterstützt keine Administratorkonten. Der gesamte Zugriff ist in Azure Active Directory (Azure AD) und die rollenbasierte Zugriffssteuerung in Azure (Azure RBAC) integriert.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4: Verwenden des einmaligen Anmeldens (SSO) in Azure Active Directory

**Leitfaden**: Azure Cosmos DB verwendet zwei Arten von Schlüsseln zur Benutzerauthentifizierung und unterstützt kein einmaliges Anmelden (Single Sign-On, SSO) auf Datenebene. Der Zugriff auf die Steuerungsebene für Cosmos DB ist über die REST-API verfügbar und unterstützt SSO. Legen Sie den Autorisierungsheader für Ihre Anforderungen auf ein JSON-Webtoken fest, das Sie aus Azure Active Directory (Azure AD) abrufen.

- [Informationen zur REST-API für Azure Database for Cosmos DB](/rest/api/cosmos-db/)

- [Grundlegendes zu SSO mit Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Verwenden der mehrstufigen Authentifizierung für den gesamten Azure Active Directory-basierten Zugriff

**Leitfaden**: Aktivieren Sie die mehrstufige Authentifizierung für Azure Active Directory (Azure AD), und befolgen Sie die Empfehlungen für die Identitäts- und Zugriffsverwaltung in Azure Security Center.

- [Planen einer Bereitstellung von Azure AD Multi-Factor Authentication](../active-directory/authentication/howto-mfa-getstarted.md)

- [Überwachen von Identität und Zugriff in Azure Security Center](../security-center/security-center-identity-access.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Verwenden dedizierter Computer (Arbeitsstationen mit privilegiertem Zugriff) für alle administrativen Aufgaben

**Leitfaden**: Verwenden Sie Privileged Access Workstations (PAW) mit mehrstufiger Authentifizierung, die für die Anmeldung und Konfiguration von Azure-Ressourcen konfiguriert sind. 

- [Informationen zu Arbeitsstationen mit privilegiertem Zugriff](/security/compass/privileged-access-devices)
 
- [Planen einer Bereitstellung von Azure AD Multi-Factor Authentication](../active-directory/authentication/howto-mfa-getstarted.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Protokollieren von und Warnen bei verdächtigen Aktivitäten in Administratorkonten

**Leitfaden**: Verwenden Sie Advanced Threat Protection (ATP) für Azure Cosmos DB. ATP für Azure Cosmos DB bietet eine zusätzliche Ebene von Security Intelligence, die ungewöhnliche und möglicherweise schädliche Versuche, auf Azure Cosmos DB-Konten zuzugreifen oder diese unbefugt zu nutzen, erkennt. Mit dieser Schutzebene können Sie Bedrohungen begegnen und diese in zentrale Sicherheitsüberwachungssysteme integrieren.

Außerdem können Sie Azure Active Directory (Azure AD) Privileged Identity Management (PIM) für die Generierung von Protokollen und Warnungen bei verdächtigen oder sicherheitsrelevanten Aktivitäten in der Umgebung verwenden.

Verwenden Sie Azure AD-Risikoerkennungen, um Warnungen und Berichte zum Verhalten von Risikobenutzern anzuzeigen.

- [Bereitstellen von Azure AD Privileged Identity Management (PIM)](../active-directory/privileged-identity-management/pim-deployment-plan.md)

- [Azure Active Directory-Risikoerkennungen](../active-directory/identity-protection/overview-identity-protection.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Verwalten von Azure-Ressourcen nur über genehmigte Standorte

**Anleitung:** Konfigurieren Sie die Standortbedingung einer Richtlinie für bedingten Zugriff, und verwalten Sie Ihre benannten Standorte. Mithilfe von benannten Standorten können Sie logische Gruppierungen von IP-Adressbereichen oder Ländern und Regionen erstellen. Sie können den Zugriff auf vertrauliche Ressourcen, z. B. Ihre Azure Cosmos DB-Instanzen, auf die konfigurierten benannten Standorte beschränken.

- [Konfigurieren benannter Standorte in Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="39-use-azure-active-directory"></a>3.9: Verwenden von Azure Active Directory

**Leitfaden**: Verwenden Sie Azure Active Directory (Azure AD) als zentrales Authentifizierungs- und Autorisierungssystem. Azure AD schützt Daten durch eine starke Verschlüsselung für ruhende und übertragene Daten. Außerdem werden in Azure AD Salts und Hashs verwendet, und Anmeldeinformationen werden sicher gespeichert.

- [Erstellen und Konfigurieren einer Azure AD-Instanz](../active-directory-domain-services/tutorial-create-instance.md)

- [Konfigurieren und Verwalten der Azure Active Directory-Authentifizierung mit Azure SQL](../azure-sql/database/authentication-aad-configure.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Regelmäßiges Überprüfen und Abstimmen des Benutzerzugriffs

**Leitfaden**: Azure Active Directory (Azure AD) enthält Protokolle zum Ermitteln von veralteten Konten. Außerdem können Sie zusätzlich Zugriffsüberprüfungen für Azure-Identitäten verwenden, um Gruppenmitgliedschaften, den Zugriff auf Unternehmensanwendungen und Rollenzuweisungen effizient zu verwalten. Der Benutzerzugriff kann regelmäßig überprüft werden, um sicherzustellen, dass nur die richtigen Benutzer weiterhin Zugriff besitzen.

- [Verwenden von Zugriffsüberprüfungen für Azure-Identitäten](../active-directory/governance/access-reviews-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Überwachen von Zugriffsversuchen auf deaktivierte Anmeldeinformationen

**Leitfaden**: Erstellen Sie Diagnoseeinstellungen für Azure AD-Benutzerkonten (Azure Active Directory), und senden Sie die Überwachungs- und Anmeldeprotokolle an einen Log Analytics-Arbeitsbereich, wo Sie die gewünschten Warnungen konfigurieren können.

- [Integrieren von Azure-Aktivitätsprotokollen in Azure Monitor](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12: Warnung bei abweichendem Verhalten bei der Kontoanmeldung

**Leitfaden**: Verwenden Sie Advanced Threat Protection (ATP) für Azure Cosmos DB. ATP für Azure Cosmos DB bietet eine zusätzliche Ebene von Security Intelligence, die ungewöhnliche und möglicherweise schädliche Versuche, auf Azure Cosmos DB-Konten zuzugreifen oder diese unbefugt zu nutzen, erkennt. Mit dieser Schutzebene können Sie Bedrohungen begegnen und diese in zentrale Sicherheitsüberwachungssysteme integrieren.

Verwenden Sie Azure Active Directory (Azure AD) Identity Protection und die Risikoerkennung, um automatische Antworten auf erkannte verdächtige Aktionen im Zusammenhang mit Benutzeridentitäten zu konfigurieren. Außerdem können Sie Protokolle zur weiteren Untersuchung in Azure Sentinel erfassen.

- [Anzeigen riskanter Azure AD-Anmeldungen](../active-directory/identity-protection/overview-identity-protection.md)

- [Konfigurieren und Aktivieren von Risikorichtlinien für den Identitätsschutz](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Durchführen des Onboardings für Azure Sentinel](../sentinel/quickstart-onboard.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="data-protection"></a>Datenschutz

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Schutz von Daten](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Verwalten eines Bestands an vertraulichen Informationen

**Leitfaden**: Verwenden Sie Tags für die Nachverfolgung von Azure Cosmos DB-Instanzen, die vertrauliche Informationen speichern oder verarbeiten.

- [Erstellen und Verwenden von Tags](../azure-resource-manager/management/tag-resources.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Isolieren von Systemen, die vertrauliche Informationen speichern oder verarbeiten

**Leitfaden**: Implementieren Sie separate Abonnements und/oder Verwaltungsgruppen für Entwicklungs-, Test- und Produktionsumgebungen. Azure Cosmos DB-Instanzen sind durch ein virtuelles Netzwerk/Subnetz getrennt, entsprechend gekennzeichnet und durch eine Netzwerksicherheitsgruppe (NSG) oder Azure Firewall gesichert. Azure Cosmos DB-Instanzen, die vertrauliche Daten speichern, müssen isoliert werden. Durch Verwendung von Azure Private Link können Sie eine Verbindung mit einem Azure Cosmos DB-Instanzkonto über einen privaten Endpunkt herstellen. Bei einem privaten Endpunkt handelt es sich um eine Gruppe privater IP-Adressen in einem Subnetz innerhalb Ihres virtuellen Netzwerks. Sie können dann den Zugriff auf die ausgewählten privaten IP-Adressen einschränken. 

- [Erstellen zusätzlicher Azure-Abonnements](../cost-management-billing/manage/create-subscription.md)

- [Erstellen von Verwaltungsgruppen](../governance/management-groups/create-management-group-portal.md)

- [Erstellen und Verwenden von Tags](../azure-resource-manager/management/tag-resources.md)

- [Konfigurieren eines privaten Endpunkts für Azure Cosmos DB](how-to-configure-private-endpoints.md)

- [Erstellen einer Netzwerksicherheitsgruppe mit einer Sicherheitskonfiguration](../virtual-network/tutorial-filter-network-traffic.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Überwachen und Blockieren einer nicht autorisierten Übertragung vertraulicher Informationen

**Leitfaden**: Sie können Advanced Threat Protection für Azure Cosmos DB konfigurieren. Dies erkennt Anomalien bei Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Ihre Datenbanken zuzugreifen oder diese zu nutzen. Es kann die folgenden Warnungen auslösen:

- Zugriff von einem ungewöhnlichen Ort

- Ungewöhnliche Datenextraktion

Wenn Sie virtuelle Computer verwenden, um auf Ihre Azure Cosmos DB-Instanzen zuzugreifen, verwenden Sie Private Link, eine Firewall, Netzwerksicherheitsgruppen und Diensttags, um die Möglichkeit von Datenexfiltration zu verringern. Microsoft verwaltet die zugrunde liegende Infrastruktur für Azure Cosmos DB und hat strenge Kontrollen implementiert, um Verluste oder Offenlegungen von Kundendaten zu verhindern.

- [Konfigurieren von Advanced Threat Protection für Cosmos DB](cosmos-db-advanced-threat-protection.md)

- [Grundlegendes zum Schutz von Kundendaten in Azure](../security/fundamentals/protection-customer-data.md)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Verwenden eines aktiven Ermittlungstools zur Bestimmung vertraulicher Daten

**Leitfaden**: Funktionen für die automatische Datenidentifizierung und -klassifizierung und zum Schutz vor Datenverlust sind für Azure Cosmos DB noch nicht verfügbar. Sie können jedoch die Azure Cognitive Search-Integration für die Klassifizierung und die Datenanalyse verwenden. Oder implementieren Sie eine Drittanbieterlösung, wenn dies für Compliancezwecke erforderlich ist.

Für die zugrundeliegende Plattform, die von Microsoft verwaltet wird, behandelt Microsoft alle Kundeninhalte als vertraulich und unternimmt große Anstrengungen, um Kundendaten vor Verlust und Gefährdung zu schützen. Um die Sicherheit von Kundendaten innerhalb von Azure zu gewährleisten, hat Microsoft eine Reihe von robusten Datenschutzkontrollen und -funktionen implementiert und kümmert sich um deren Verwaltung.

- [Indizieren von Azure Cosmos DB Daten mit Azure Cognitive Search](../search/search-howto-index-cosmosdb.md?bc=%2fazure%2fcosmos-db%2fbreadcrumb%2ftoc.json&toc=%2fazure%2fcosmos-db%2ftoc.json)

- [Grundlegendes zum Schutz von Kundendaten in Azure](../security/fundamentals/protection-customer-data.md)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4.6: Verwenden der rollenbasierten Zugriffssteuerung zum Steuern des Zugriffs auf Ressourcen

**Leitfaden**: Azure Cosmos DB bietet für gängige Verwaltungsszenarien eine integrierte rollenbasierte Zugriffssteuerung in Azure (Azure RBAC). Eine Person mit einem Profil in Azure Active Directory (Azure AD) kann diese Azure-Rollen Benutzern, Gruppen, Dienstprinzipalen oder verwalteten Identitäten zuweisen, um den Zugriff auf Ressourcen und Vorgänge für Azure Cosmos DB-Ressourcen zu gewähren oder zu verweigern. Rollenzuweisungen sind auf den Zugriff auf Steuerungsebene beschränkt, was den Zugriff auf Konten, Datenbanken, Container und Angebote (Durchsatz) für Azure Cosmos umfasst.

- [Implementieren von Azure RBAC in Azure Cosmos DB](role-based-access-control.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Verschlüsseln vertraulicher, ruhender Informationen

**Leitfaden**: Alle in Cosmos DB gespeicherten Benutzerdaten werden standardmäßig im Ruhezustand verschlüsselt. Es gibt keine Möglichkeit, dies auszuschalten. Azure Cosmos DB verwendet AES-256-Verschlüsselung in allen Regionen, in denen das Konto ausgeführt wird.

Standardmäßig verwaltet Microsoft die Schlüssel, die zum Verschlüsseln der Daten in Ihrem Azure Cosmos-Konto verwendet werden. Sie können optional eine zweite Verschlüsselungsschicht mit Ihren eigenen Schlüsseln hinzufügen.

- [Informationen zur Verschlüsselung ruhender Daten mit Azure Cosmos DB](database-encryption-at-rest.md)

- [Informationen zur Schlüsselverwaltung für die Verschlüsselung ruhender Daten mit Azure Cosmos DB]()

- [Konfigurieren von kundenseitig verwalteten Schlüsseln für Ihr Azure Cosmos DB-Konto](how-to-setup-cmk.md)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Protokollieren und Warnen bei Änderungen an wichtigen Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Monitor mit dem Azure-Aktivitätsprotokoll, um Warnungen zu erstellen, die bei Änderungen an Produktionsinstanzen von Azure Cosmos DB ausgegeben werden.

- [Erstellen von Warnungen für Ereignisse des Azure-Aktivitätsprotokolls](../azure-monitor/alerts/alerts-activity-log.md)

- [Erstellen von Warnungen für Ereignisse des Azure-Aktivitätsprotokolls](../azure-monitor/alerts/alerts-activity-log.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="vulnerability-management"></a>Verwaltung von Sicherheitsrisiken

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Verwaltung von Sicherheitsrisiken](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Ausführen automatisierter Scan-Tools für Sicherheitsrisiken

**Leitfaden**: Befolgen Sie die Empfehlungen von Azure Security Center für Ihre Azure Cosmos DB-Instanzen. 

Microsoft führt auf den zugrunde liegenden Hosts, die Ihre Azure Cosmos DB-Instanzen unterstützen, Systempatching durch und verwaltet Sicherheitsrisiken. Um die Sicherheit von Kundendaten innerhalb von Azure zu gewährleisten, hat Microsoft eine Reihe von robusten Datenschutzkontrollen und -funktionen implementiert und kümmert sich um deren Verwaltung.

- [Verfügbare unterstützte Features in Azure Security Center](../security-center/security-center-services.md?tabs=features-windows)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

## <a name="inventory-and-asset-management"></a>Bestands- und Ressourcenverwaltung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Bestands- und Ressourcenverwaltung](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Verwenden der automatisierten Asset Discovery-Lösung

**Leitfaden**: Verwenden Sie das Azure-Portal oder Azure Resource Graph, um alle Ressourcen (nicht beschränkt auf Azure Cosmos DB, sondern beispielsweise auch Computeressourcen, andere Speicher, Netzwerke, Ports, Protokolle usw.) in Ihren Abonnements zu ermitteln. Stellen Sie sicher, dass Sie über entsprechende Berechtigungen in Ihrem Mandanten verfügen und alle Azure-Abonnements und Ressourcen in Ihren Abonnements auflisten können.

Obwohl klassische Azure-Ressourcen über das Resource Graph ermittelt werden können, wird dringend empfohlen, Azure Resource Manager-Ressourcen zu erstellen und zu verwenden.

- [Erstellen von Abfragen mit Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

- [Anzeigen Ihrer Azure-Abonnements](/powershell/module/az.accounts/get-azsubscription?preserve-view=true&view=azps-4.8.0)

- [Informationen zur rollenbasierten Zugriffssteuerung in Azure](../role-based-access-control/overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="62-maintain-asset-metadata"></a>6.2: Verwalten von Ressourcenmetadaten

**Leitfaden**: Wenden Sie Tags auf Ihre Azure Cosmos DB-Instanzen und verwandte Ressourcen mit Metadaten an, um sie logisch in einer Taxonomie zu organisieren.

- [Erstellen und Verwenden von Tags](../azure-resource-manager/management/tag-resources.md)

- [Welche Azure Cosmos DB-Ressourcen Tags unterstützen](../azure-resource-manager/management/tag-support.md#microsoftdocumentdb)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Löschen nicht autorisierter Azure-Ressourcen

**Leitfaden**: Verwenden Sie nach Bedarf Tagging, Verwaltungsgruppen und separate Abonnements zum Organisieren und Nachverfolgen von Ressourcen wie u. a. Azure Cosmos DB-Ressourcen. Stimmen Sie den Bestand regelmäßig ab, und stellen Sie sicher, dass nicht autorisierte Ressourcen rechtzeitig aus dem Abonnement gelöscht werden.

- [Erstellen zusätzlicher Azure-Abonnements](../cost-management-billing/manage/create-subscription.md)

- [Erstellen von Verwaltungsgruppen](../governance/management-groups/create-management-group-portal.md)

- [Erstellen und Verwenden von Tags](../azure-resource-manager/management/tag-resources.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Überwachung auf nicht genehmigte Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy, um Einschränkungen für den Typ der Ressourcen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie dazu die folgenden integrierten Richtliniendefinitionen:

- Not allowed resource types (Unzulässige Ressourcentypen)

- Zulässige Ressourcentypen

Verwenden Sie darüber hinaus Azure Resource Graph, um Ressourcen in Ihren Abonnements abzufragen und zu ermitteln.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Erstellen von Abfragen mit Azure Graph](../governance/resource-graph/first-query-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="69-use-only-approved-azure-services"></a>6.9: Ausschließliche Verwendung genehmigter Azure-Dienste

**Leitfaden**: Verwenden Sie Azure Policy, um Einschränkungen für den Typ der Ressourcen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie hierzu die folgenden integrierten Richtliniendefinitionen:
- Not allowed resource types (Unzulässige Ressourcentypen)
- Zulässige Ressourcentypen

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Ablehnen eines bestimmten Ressourcentyps mit Azure Policy](../governance/policy/samples/built-in-policies.md#general)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Einschränken der Möglichkeiten von Benutzern zur Interaktion mit Azure Resource Manager

**Leitfaden**: Verwenden Sie den bedingten Azure-Zugriff, um die Möglichkeiten der Benutzer zur Interaktion mit Azure Resource Manager einzuschränken, indem Sie „Zugriff blockieren“ für die App zur Verwaltung von Microsoft Azure konfigurieren. Dies kann das Erstellen und Ändern von Ressourcen innerhalb einer Hochsicherheitsumgebung verhindern.

- [Verwalten des Zugriffs auf die Azure-Verwaltung mit bedingtem Zugriff](../role-based-access-control/conditional-access-azure-management.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="secure-configuration"></a>Sichere Konfiguration

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Sichere Konfiguration](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Einrichten sicherer Konfigurationen für alle Azure-Ressourcen

**Leitfaden**: Definieren und implementieren Sie Standardsicherheitskonfigurationen für Ihre Cosmos DB-Instanzen mit Azure Policy. Verwenden Sie Azure Policy-Aliase im Namespace **Microsoft.DocumentDB**, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen der Konfiguration Ihrer Cosmos DB-Instanz zu erstellen. Sie können auch die integrierten Richtliniendefinitionen für Azure Cosmos DB verwenden, z. B.:
- Advanced Threat Protection für Cosmos DB-Konten bereitstellen
- Cosmos DB sollte einen VNET-Dienstendpunkt verwenden

- [Anzeigen verfügbarer Azure Policy-Aliase](/powershell/module/az.resources/get-azpolicyalias?preserve-view=true&view=azps-4.8.0)

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Verwalten von sicheren Konfigurationen für Azure-Ressourcen

**Leitfaden**: Verwenden Sie die Azure Policy-Auswirkungen [Deny] und [DeployIfNotExists], um sichere Einstellungen für alle Ihre Azure-Ressourcen zu erzwingen.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Grundlegendes zu Azure Policy-Auswirkungen](../governance/policy/concepts/effects.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Sicheres Speichern der Konfiguration von Azure-Ressourcen

**Leitfaden**: Wenn Sie für Ihre Cosmos DB-Ressourcen oder für verwandte Ressourcen benutzerdefinierte Azure Policy-Definitionen verwenden, nutzen Sie Azure Repos, um Ihre benutzerdefinierten Richtliniendefinitionen als Code zu speichern und zu verwalten.

- [Entwerfen von Workflows für Policy-as-Code](../governance/policy/concepts/policy-as-code.md)

- [Dokumentation zu Azure Repos](/azure/devops/repos/git/gitworkflow)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Bereitstellen von Konfigurationsverwaltungstools für Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy-Aliase im Namespace „Microsoft.DocumentDB“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen von Systemkonfigurationen zu erstellen und Warnungen dazu zu erhalten. Entwickeln Sie außerdem einen Prozess und eine Pipeline zum Verwalten von Richtlinienausnahmen.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementieren der automatisierten Konfigurationsüberwachung für Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy-Aliase im Namespace „Microsoft.DocumentDB“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen von Systemkonfigurationen zu erstellen und Warnungen dazu zu erhalten. Verwenden Sie die Azure Policy-Einstellungen [audit] (Überwachen), [deny] (Verweigern) und [deploy if not exist] (Bereitstellen, falls nicht vorhanden), um Konfigurationen für Ihre Azure Cosmos DB-Instanzen und die zugehörigen Ressourcen automatisch zu erzwingen. 

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="711-manage-azure-secrets-securely"></a>7.11: Sicheres Verwalten von Azure-Geheimnissen

**Leitfaden**: Wenn Azure-VMs oder Webanwendungen, die in Azure App Service ausgeführt werden, für den Zugriff auf Ihre Azure Cosmos DB-Instanzen verwendet werden, nutzen Sie die verwaltete Dienstidentität in Verbindung mit Azure Key Vault, um die Verwaltung von Azure Cosmos DB-Geheimnissen zu vereinfachen und zu schützen. Stellen Sie sicher, dass das vorläufige Löschen für Azure Key Vault aktiviert ist.

- [Integrieren mit verwalteten Azure-Identitäten](../azure-app-configuration/howto-integrate-azure-managed-service-identity.md)

- [Erstellen einer Key Vault-Instanz](../key-vault/secrets/quick-create-portal.md)

- [Authentifizieren bei Key Vault](../key-vault/general/authentication.md)

- [Zuweisen einer Key Vault-Zugriffsrichtlinie](../key-vault/general/assign-access-policy-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Sicheres und automatisches Verwalten von Identitäten

**Leitfaden**: Wenn Azure-VMs oder Webanwendungen, die in Azure App Service ausgeführt werden, für den Zugriff auf Ihre Azure Cosmos DB-Instanzen verwendet werden, nutzen Sie die verwaltete Dienstidentität in Verbindung mit Azure Key Vault, um die Verwaltung von Azure Cosmos DB-Geheimnissen zu vereinfachen und zu schützen.

Verwenden Sie verwaltete Identitäten, um Azure-Dienste mit einer automatisch verwalteten Identität in Azure Active Directory (AAD) bereitzustellen. Mit verwalteten Identitäten können Sie die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt, einschließlich Key Vault. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein.

- [Konfigurieren von verwalteten Identitäten](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)

- [Integrieren mit verwalteten Azure-Identitäten](../azure-app-configuration/howto-integrate-azure-managed-service-identity.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Beheben der unbeabsichtigten Offenlegung von Anmeldeinformationen

**Anleitung:** Implementieren Sie Credential Scanner, um Anmeldeinformationen im Code zu identifizieren. In Credential Scanner wird auch das Verschieben von ermittelten Anmeldeinformationen an sicherere Speicherorte (z. B. Azure Key Vault) empfohlen.

- [Einrichten von Credential Scanner](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="malware-defense"></a>Schutz vor Schadsoftware

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Schutz vor Schadsoftware](../security/benchmarks/security-control-malware-defense.md).*

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: Vorabprüfen von in computefremde Azure-Ressourcen hochzuladenden Dateien

**Leitfaden**: Microsoft Antimalware ist auf dem zugrunde liegenden Host für die Azure-Dienste (z. B. Azure Cosmos DB) aktiviert, wird jedoch nicht für Kundeninhalte ausgeführt.

Es liegt in ihrer Verantwortung, alle Dateien, die in computefremde Azure-Ressourcen hochgeladen werden, vorab zu scannen. Dies gilt auch für Azure Cosmos DB. Microsoft kann nicht auf Kundendaten zugreifen und kann daher keine Antischadsoftwarescans von Kundeninhalten in Ihrem Namen durchführen.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="data-recovery"></a>Datenwiederherstellung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Datenwiederherstellung](../security/benchmarks/security-control-data-recovery.md).*

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Durchführen vollständiger Systemsicherungen und Sichern aller kundenseitig verwalteten Schlüssel

**Leitfaden**: Azure Cosmos DB erstellt in regelmäßigen Abständen automatisch Sicherungen Ihrer Daten. Falls Sie Ihre Datenbank oder Ihr Container gelöscht wird, können Sie ein Supportticket erstellen oder sich an den Azure-Support wenden, um die Daten aus automatischen Onlinesicherungen wiederherstellen zu lassen. Der Azure-Support steht nur in ausgewählten Tarifen wie Standard und Developer sowie in höheren Tarifen zur Verfügung. Für die Wiederherstellung einer bestimmten Momentaufnahme der Sicherung setzt Azure Cosmos DB voraus, dass die Daten für die Dauer des Sicherungszyklus dieser Momentaufnahme verfügbar sind. 

Wenn Sie Key Vault zum Speichern von Anmeldeinformationen für Ihre Cosmos DB-Instanzen verwenden, führen Sie regelmäßige automatische Sicherungen Ihrer Schlüssel durch.

- [Informationen zu automatisierten Sicherungen in Azure Cosmos DB](online-backup-and-restore.md)

- [Wiederherstellen von Daten in Azure Cosmos DB](./online-backup-and-restore.md)

- [Sichern von Key Vault-Schlüsseln](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Überprüfen aller Sicherungen einschließlich kundenseitig verwalteter Schlüssel

**Leitfaden**: Falls Sie Ihre Datenbank oder Ihr Container gelöscht wird, können Sie ein Supportticket erstellen oder sich an den Azure-Support wenden, um die Daten aus automatischen Onlinesicherungen wiederherstellen zu lassen. Der Azure-Support steht nur in ausgewählten Tarifen wie Standard und Developer sowie in höheren Tarifen zur Verfügung. Für die Wiederherstellung einer bestimmten Momentaufnahme der Sicherung setzt Azure Cosmos DB voraus, dass die Daten für die Dauer des Sicherungszyklus dieser Momentaufnahme verfügbar sind.

Testen Sie die Wiederherstellung Ihrer Geheimnisse, die mithilfe von PowerShell in Azure Key Vault gespeichert wurden. Mit dem Cmdlet „Restore-AzureKeyVaultKey“ wird ein Schlüssel im angegebenen Schlüsseltresor erstellt. Bei diesem Schlüssel handelt es sich um ein Replikat des gesicherten Schlüssels in der Eingabedatei, der denselben Namen hat wie der ursprüngliche Schlüssel.

- [Informationen zu automatisierten Sicherungen in Azure Cosmos DB](online-backup-and-restore.md)

- [Wiederherstellen von Daten in Azure Cosmos DB](./online-backup-and-restore.md)

- [Wiederherstellen von Azure Key Vault-Geheimnissen](/powershell/module/az.keyvault/restore-azkeyvaultkey?preserve-view=true&view=azps-4.8.0)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Sicherstellen des Schutzes von Sicherungen und von kundenseitig verwalteten Schlüsseln

**Leitfaden**: Da alle in Cosmos DB gespeicherten Benutzerdaten sowohl ruhend als auch während der Übertragung verschlüsselt werden, müssen Sie keine Maßnahmen ergreifen. Anders ausgedrückt: Die Verschlüsselung ruhender Daten ist standardmäßig „aktiviert“. Es gibt keine Möglichkeit, dies ein- oder auszuschalten. Azure Cosmos DB verwendet AES-256-Verschlüsselung in allen Regionen, in denen das Konto ausgeführt wird.

Aktivieren Sie das vorläufige Löschen in Key Vault, um Schlüssel vor dem versehentlichen oder böswilligen Löschen zu schützen.

- [Informationen zur Datenverschlüsselung in Azure Cosmos DB](database-encryption-at-rest.md)

- [Aktivieren des vorläufigen Löschens in Key Vault](../storage/blobs/soft-delete-blob-overview.md?tabs=azure-portal)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

## <a name="incident-response"></a>Reaktion auf Vorfälle

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Reaktion auf Vorfälle](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Erstellen eines Leitfadens für die Reaktion auf Vorfälle

**Anleitung:** Erarbeiten Sie einen Leitfaden für die Reaktion auf Vorfälle für Ihre Organisation. Stellen Sie sicher, dass es schriftliche Pläne für die Reaktion auf Vorfälle gibt, in denen alle Rollen der Mitarbeiter sowie die Phasen der Bearbeitung und Verwaltung von Vorfällen von der Ermittlung bis zur abschließenden Überprüfung definiert sind.

- [„Computer Security Incident Handling Guide“ des National Institute of Standards and Technology (NIST)](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

- [Leitfaden zu Planung und Betrieb](../security-center/security-center-planning-and-operations-guide.md)

- [Anleitung zum Entwickeln eines Prozesses für die Reaktion auf Sicherheitsvorfälle](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Struktur eines Vorfalls laut Microsoft Security Response Center](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Erstellen eines Verfahrens zur Bewertung und Priorisierung von Vorfällen

**Anleitung:** Security Center weist jeder Warnung einen Schweregrad zu, damit Sie priorisieren können, welche Warnungen zuerst untersucht werden sollen. Der Schweregrad basiert darauf, wie zuversichtlich Security Center in Bezug auf den Befund oder die Analyse ist, die zum Auslösen der Warnung verwendet wird, sowie auf dem Zuverlässigkeitsgrad, dass hinter der Aktivität, die zu der Warnung führte, eine böswillige Absicht stand.

Kennzeichnen Sie außerdem die Abonnements (z. B. Produktion, Nicht-Produktion), und erstellen Sie ein Namenssystem, um Azure-Ressourcen eindeutig zu identifizieren und zu kategorisieren.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="103-test-security-response-procedures"></a>10.3: Verfahren zum Testen der Reaktion auf Sicherheitsvorfälle

**Leitfaden**: Führen Sie in regelmäßigen Abständen Tests zur Reaktionsfähigkeit Ihrer Systeme auf Vorfälle durch. Identifizieren Sie Schwachstellen und Lücken, und überarbeiten Sie den Plan bei Bedarf.

- [„Guide to Test, Training, and Exercise Programs for IT Plans and Capabilities“ des National Institute of Standards and Technology (NIST)](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Angeben von Kontaktdaten für Sicherheitsvorfälle und Konfigurieren von Warnungsbenachrichtigungen für Sicherheitsvorfälle

**Leitfaden**: Microsoft kontaktiert Sie unter den für Sicherheitsvorfälle angegebenen Kontaktdaten, wenn das Microsoft Security Response Center (MSRC) feststellt, dass Personen unrechtmäßig oder unbefugt auf die Kundendaten zugegriffen haben.  Überprüfen Sie die Vorfälle anschließend, um sicherzustellen, dass die Probleme behoben wurden.

- [Festlegen der Kontaktinformationen in Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Integrieren von Sicherheitswarnungen in das System zur Reaktion auf Vorfälle

**Leitfaden**: Exportieren Sie die Azure Security Center-Warnungen und -Empfehlungen über die Funktion „Fortlaufender Export“. Über „Fortlaufender Export“ können Sie Warnungen und Empfehlungen entweder manuell oder kontinuierlich exportieren. Sie können den Azure Security Center-Datenconnector verwenden, um die Warnungen an Azure Sentinel zu streamen.

- [Konfigurieren des fortlaufenden Exports](../security-center/continuous-export.md)

- [Streamen von Warnungen in Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatisieren der Reaktion auf Sicherheitswarnungen

**Leitfaden**: Verwenden Sie die Funktion „Workflowautomatisierung“ in Azure Security Center, um über „Logic Apps“ automatisch Reaktionen auf Sicherheitswarnungen und -empfehlungen auszulösen.

- [Konfigurieren von Workflowautomatisierung und Logic Apps](../security-center/workflow-automation.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="penetration-tests-and-red-team-exercises"></a>Penetrationstests und Red Team-Übungen

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Penetrationstests und Red Team-Übungen](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1: Durchführen regelmäßiger Penetrationstests Ihrer Azure-Ressourcen und Sicherstellen der Behebung aller kritischen Sicherheitsergebnissen

**Leitfaden**: Befolgen Sie in dem Fall die Einsatzregeln für Penetrationstests von Microsoft Cloud, um sicherzustellen, dass die Penetrationstests nicht gegen Microsoft-Richtlinien verstoßen. Nutzen Sie die Microsoft-Strategie und Durchführung von Red Team- und Livewebsite-Penetrationstests für von Microsoft verwaltete Cloudinfrastruktur, Dienste und Anwendungen. 

- [Penetrationstests – Rules of Engagement](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Cloud Red Teaming](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die [Übersicht über Version 2 des Azure-Sicherheitsvergleichstests](../security/benchmarks/overview.md) an.
- Erfahren Sie mehr über [Azure-Sicherheitsbaselines](../security/benchmarks/security-baselines-overview.md).
