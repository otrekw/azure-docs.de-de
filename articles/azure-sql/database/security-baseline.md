---
title: Azure-Sicherheitsbaseline für Azure SQL-Datenbank
description: Die Sicherheitsbaseline für Azure SQL-Datenbank enthält Anweisungen zur Vorgehensweise und Ressourcen für die Implementierung der Sicherheitsempfehlungen im Azure-Sicherheitsvergleichstest.
author: msmbaldwin
ms.service: sql-database
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 87072ecc4bff054d64c3d8576f821e725959ea7e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "101657790"
---
# <a name="azure-security-baseline-for-azure-sql-database"></a>Azure-Sicherheitsbaseline für Azure SQL-Datenbank

Diese Sicherheitsbaseline wendet Empfehlungen des [Azure-Sicherheitsvergleichstests Version 1.0](../../security/benchmarks/overview.md) auf Azure SQL-Datenbank an. Der Azure-Sicherheitsvergleichstest enthält Empfehlungen zum Schutz Ihrer Cloudlösungen in Azure. Der Inhalt wird nach den **Sicherheitskontrollen** gruppiert, die durch den Azure-Sicherheitsvergleichstest und die entsprechenden für Azure SQL-Datenbank geltenden Empfehlungen definiert sind. Nicht auf Azure SQL-Datenbank anwendbare **Kontrollen** wurden ausgeschlossen.

Die vollständige Zuordnung von Azure SQL-Datenbank zum Azure-Sicherheitsvergleichstest finden Sie in der [vollständigen Zuordnungsdatei der Azure SQL-Datenbank-Sicherheitsbaseline](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Netzwerksicherheit

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Netzwerksicherheit](../../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Schützen von Azure-Ressourcen in virtuellen Netzwerken

**Leitfaden**: Sie können Azure Private Link aktivieren, um den Zugriff über einen privaten Endpunkt Ihres virtuellen Netzwerks auf Azure PaaS-Dienste (z. B. SQL-Datenbank) sowie auf in Azure gehostete Kunden-/Partnerdienste zu ermöglichen. Der Datenverkehr zwischen Ihrem virtuellen Netzwerk und dem Dienst wird über das Microsoft-Backbone-Netzwerk übertragen und dadurch vom öffentlichen Internet isoliert. 

Wenn Sie Datenverkehr für Azure SQL-Datenbank zulassen möchten, verwenden Sie die SQL-Diensttags, um den ausgehenden Datenverkehr über Netzwerksicherheitsgruppen zu ermöglichen.

Mithilfe von VNET-Regeln kann Azure SQL-Datenbank nur Verbindungen akzeptieren, die von ausgewählten Subnetzen innerhalb eines virtuellen Netzwerks stammen.

- [Einrichten von Private Link für Azure SQL-Datenbank](/azure/sql-database/sql-database-private-endpoint-overview#how-to-set-up-private-link-for-azure-sql-database)

- [Verwenden von VNET-Dienstendpunkten und -Regeln für Datenbankserver](/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.Sql**:

[!INCLUDE [Resource Policy for Microsoft.Sql 1.1](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-1-1.md)]

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2: Überwachen und Protokollieren der Konfiguration und des Datenverkehrs von virtuellen Netzwerken, Subnetzen und Netzwerkschnittstellen

**Leitfaden**: Verwenden Sie Azure Security Center, und setzen Sie die Empfehlungen zum Netzwerkschutz für das Subnetz um, in dem Ihr Azure SQL-Datenbank-Server bereitgestellt wurde. 

Aktivieren Sie für virtuelle Azure-Computer (VM), die eine Verbindung mit Ihrer Instanz des Azure SQL-Datenbank-Servers herstellen, NSG-Flussprotokolle für die NSGs (Netzwerksicherheitsgruppen), mit denen diese VMs geschützt sind. Senden Sie die Protokolle anschließend zur Überwachung des Datenverkehrs an ein Azure Storage-Konto. 

Sie können auch NSG-Flussprotokolle an einen Log Analytics-Arbeitsbereich senden und Traffic Analytics verwenden, um Einblicke in den Datenverkehrsfluss in Ihrer Azure-Cloud zu ermöglichen. Einige Vorteile von Traffic Analytics sind die Möglichkeit, die Netzwerkaktivität zu visualisieren und Hotspots zu erkennen, Sicherheitsbedrohungen zu identifizieren, Datenverkehrsflussmuster zu verstehen und Netzwerkfehlkonfigurationen zu ermitteln.

- [Aktivieren der NSG-Flussprotokolle](../../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Grundlegendes zu der von Azure Security Center bereitgestellten Netzwerksicherheit](../../security-center/security-center-network-recommendations.md)

- [Aktivieren und Verwenden von Traffic Analytics](../../network-watcher/traffic-analytics.md)

- [Grundlegendes zu der von Azure Security Center bereitgestellten Netzwerksicherheit](../../security-center/security-center-network-recommendations.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Ablehnen der Kommunikation mit bekannten schädlichen IP-Adressen

**Leitfaden**: Aktivieren Sie DDoS Protection Standard in den virtuellen Netzwerken, die Ihren SQL Server-Instanzen zugeordnet sind, um diese vor verteilten Denial-of-Service-Angriffen zu schützen. Verwenden Sie die in Azure Security Center integrierte Threat Intelligence, um die Kommunikation mit bekannten schädlichen oder nicht genutzten IP-Adressen zu verweigern.

- [Konfigurieren von DDoS-Schutz](/azure/virtual-network/manage-ddos-protection)

- [Grundlegendes zur integrierten Threat Intelligence in Azure Security Center](/azure/security-center/security-center-alerts-data-services)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="15-record-network-packets"></a>1.5: Aufzeichnen von Netzwerkpaketen

**Leitfaden**: Aktivieren Sie für virtuelle Azure-Computer (VMs), die eine Verbindung mit Ihrer Instanz von Azure SQL-Datenbank herstellen, NSG-Flowprotokolle für die NSGs (Netzwerksicherheitsgruppen), mit denen diese VMs geschützt sind. Senden Sie die Protokolle dann zur Überwachung des Datenverkehrs an ein Azure Storage-Konto. Aktivieren Sie Network Watcher-Paketerfassung, falls dies für die Untersuchung anomaler Aktivitäten erforderlich ist.

- [Aktivieren der NSG-Flussprotokolle](../../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Aktivieren von Network Watcher](../../network-watcher/network-watcher-create.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Bereitstellen von netzwerkbasierten Angriffserkennungs-/Eindringschutzsystemen (Intrusion Detection/Intrusion Prevention Systems, IDS/IPS)

**Leitfaden**: Aktivieren von Advanced Threat Protection (ATP) für Azure SQL-Datenbank.  Benutzer erhalten Warnungen zu verdächtigen Datenbankaktivitäten, potenziellen Sicherheitsrisiken sowie Angriffen durch Einschleusung von SQL-Befehlen und anomalen Datenbankzugriffs- und -abfragemustern. Warnungen von Advanced Threat Protection sind auch in Azure Security Center integriert. 

- [Verstehen und Verwenden von Advanced Threat Protection für Azure SQL-Datenbank](/azure/sql-database/sql-database-threat-detection-overview)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimieren der Komplexität und des Verwaltungsaufwands von Netzwerksicherheitsregeln

**Leitfaden**: Verwenden Sie Diensttags in virtuellen Netzwerken, um Netzwerkzugriffssteuerungen für Netzwerksicherheitsgruppen oder Azure Firewall zu definieren. Sie können Diensttags anstelle von spezifischen IP-Adressen nutzen, wenn Sie Sicherheitsregeln erstellen. Indem Sie den Diensttagnamen (z. B. „ApiManagement“) im entsprechenden Quell- oder Zielfeld einer Regel angeben, können Sie den Datenverkehr für den entsprechenden Dienst zulassen oder verweigern. Microsoft verwaltet die Adresspräfixe, für die das Diensttag gilt, und aktualisiert das Diensttag automatisch, wenn sich die Adressen ändern.

Bei Verwendung von Dienstendpunkten für Azure SQL-Datenbank ist eine ausgehende Verbindung zu den öffentlichen IP-Adressen von Azure SQL-Datenbank erforderlich: Netzwerksicherheitsgruppen (NSGs) müssen für IP-Adressen der Azure SQL-Datenbank geöffnet werden, um Verbindungen zuzulassen. Sie erreichen dies, indem Sie NSG-Diensttags für Azure SQL-Datenbank verwenden.

- [Grundlegendes zu Diensttags mit Dienstendpunkten für Azure SQL-Datenbank](/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview#limitations)

- [Grundlegendes zu Diensttags und Verwenden von Diensttags](../../virtual-network/service-tags-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Beibehalten von Standardsicherheitskonfigurationen für Netzwerkgeräte

**Leitfaden**: Definieren und implementieren Sie Netzwerksicherheitskonfigurationen für Ihre Instanzen von Azure SQL-Datenbank-Server mit Azure Policy. Sie können den Namespace „Microsoft.Sql“ verwenden, um benutzerdefinierte Richtliniendefinitionen zu definieren, oder die integrierten Richtliniendefinitionen, die für den Netzwerkschutz in Bezug auf Azure SQL-Datenbank-Server ausgelegt sind. Ein Beispiel für eine geeignete integrierte Netzwerksicherheitsrichtlinie für Azure SQL-Datenbank-Server ist: „SQL Server sollte einen VNET-Dienstendpunkt verwenden“.

 

Verwenden Sie Azure Blueprints, um umfangreiche Azure-Bereitstellungen zu vereinfachen, indem wichtige Umgebungsartefakte, z. B. Azure Resource Manager-Vorlagen, rollenbasierte Zugriffssteuerung von Azure (Azure RBAC) und Richtlinien, gemeinsam in einer Blaupausendefinition gepackt werden. Sie können die Blaupause leicht auf neue Abonnements und Umgebungen anwenden und die Steuerung und Verwaltung durch die Versionsverwaltung optimieren.

- [Konfigurieren und Verwalten von Azure Policy](../../governance/policy/tutorials/create-and-manage.md)

- [Erstellen einer Azure-Blaupause](../../governance/blueprints/create-blueprint-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="110-document-traffic-configuration-rules"></a>1.10: Dokumentieren von Datenverkehrskonfigurationsregeln

**Leitfaden**: Verwenden Sie Tags für Netzwerksicherheitsgruppen (NSGs) und andere Ressourcen im Zusammenhang mit Netzwerksicherheit und Datenverkehrsfluss. Verwenden Sie für einzelne NSG-Regeln das Feld „Beschreibung“, um geschäftliche Anforderungen und/oder deren Dauer (usw.) für alle Regeln festzulegen, die Datenverkehr in ein bzw. aus einem Netzwerk zulassen.

Verwenden Sie eine der integrierten Azure Policy-Definitionen zum Tagging, z. B. „Tag und zugehörigen Wert erzwingen“, um sicherzustellen, dass alle Ressourcen mit Tags erstellt werden und Sie über vorhandene nicht markierte Ressourcen benachrichtigt werden.

Sie können Azure PowerShell oder die Azure-Befehlszeilenschnittstelle verwenden, um Ressourcen anhand ihrer Tags zu suchen oder Aktionen auszuführen.

- [Erstellen und Verwenden von Tags](/azure/azure-resource-manager/resource-group-using-tags)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Verwenden automatisierter Tools zum Überwachen von Netzwerkressourcenkonfigurationen und Erkennen von Änderungen

**Leitfaden**: Verwenden Sie das Azure-Aktivitätsprotokoll zum Überwachen der Konfigurationen von Netzwerkressourcen und zum Erkennen von Änderungen bei Netzwerkressourcen, die sich auf Ihre Instanzen von Azure SQL-Datenbank-Server beziehen. Erstellen Sie Warnungen in Azure Monitor, die bei Änderungen an wichtigen Netzwerkressourcen ausgelöst werden.

- [Anzeigen und Abrufen von Azure-Aktivitätsprotokollereignissen](/azure/azure-monitor/platform/activity-log-view)

- [Erstellen von Warnungen in Azure Monitor](/azure/azure-monitor/platform/alerts-activity-log)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="logging-and-monitoring"></a>Protokollierung und Überwachung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Protokollierung und Überwachung](../../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Konfigurieren der zentralen Sicherheitsprotokollverwaltung

**Leitfaden**: Aktivieren Sie die Überwachung für Azure SQL-Datenbank, um Datenbankereignisse nachzuverfolgen und diese in ein Überwachungsprotokoll in Ihrem Azure Storage-Konto, Ihrem Log Analytics-Arbeitsbereich oder in Event Hubs zu schreiben.

Darüber hinaus können Sie Azure SQL-Diagnosetelemetriedaten an Azure SQL-Analyse streamen. Dies ist eine Cloudlösung, die die Leistung von Azure SQL-Datenbank und Azure SQL Managed Instance bedarfsgesteuert und abonnementübergreifend überwacht. Die Lösung ermöglicht Ihnen das Erfassen und Visualisieren von Leistungsmetriken für Azure SQL-Datenbank und verfügt über integrierte intelligente Funktionen für die Behandlung von Leistungsproblemen.

- [Einrichten der Überwachung für Ihre Azure SQL-Datenbank](/azure/sql-database/sql-database-auditing)

- [Sammeln von Plattformprotokollen und -metriken mit Azure Monitor](/azure/sql-database/sql-database-metrics-diag-logging)

- [Streamen von Diagnosedaten an Azure SQL-Analyse](/azure/sql-database/sql-database-metrics-diag-logging#stream-into-azure-sql-analytics)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Aktivieren der Überwachungsprotokollierung für Azure-Ressourcen

**Leitfaden**: Aktivieren Sie die Überwachung für Ihre Instanz von Azure SQL-Datenbank-Server, und wählen Sie einen Speicherort für die Überwachungsprotokolle (Azure Storage, Log Analytics oder Event Hub) aus.

- [Aktivieren der Überwachung für Azure SQL Server](/azure/sql-database/sql-database-auditing)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.Sql**:

[!INCLUDE [Resource Policy for Microsoft.Sql 2.3](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-2-3.md)]

### <a name="25-configure-security-log-storage-retention"></a>2.5: Konfigurieren der Sicherheitsprotokoll-Aufbewahrungsdauer im Speicher

**Leitfaden**: Legen Sie den Aufbewahrungszeitraum für Protokolle beim Speichern Ihrer Azure SQL-Datenbank-Protokolle in einem Log Analytics-Arbeitsbereich gemäß den Konformitätsbestimmungen Ihrer Organisation fest.

- [Ändern des Datenaufbewahrungszeitraums](/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.Sql**:

[!INCLUDE [Resource Policy for Microsoft.Sql 2.5](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-2-5.md)]

### <a name="26-monitor-and-review-logs"></a>2.6: Überwachen und Überprüfen von Protokollen

**Leitfaden**: Analysieren und überwachen Sie Protokolle auf anomales Verhalten, und prüfen Sie die Ergebnisse regelmäßig. Verwenden Sie Advanced Threat Protection von Azure Security Center, um für Ihre Instanz von Azure SQL-Datenbank Warnungen vor ungewöhnlichen Aktivitäten zu erhalten. Alternativ können Sie Warnungen basierend auf Metrikwerten oder Einträgen des Azure-Aktivitätsprotokolls für Ihre Instanzen von Azure SQL-Datenbank konfigurieren.

- [Grundlegendes zu Advanced Threat Protection und Warnungen für Azure SQL Server](/azure/sql-database/sql-database-threat-detection-overview)

- [Konfigurieren von benutzerdefinierten Warnungen für Azure SQL-Datenbank](alerts-insights-configure-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Aktivieren von Warnungen bei anomalen Aktivitäten

**Leitfaden**: Verwenden Sie Advanced Threat Protection von Azure Security Center für Azure SQL-Datenbanken, um die Überwachung auf anomale Aktivitäten zu ermöglichen und entsprechende Warnungen zu erhalten. Aktivieren Sie Azure Defender für SQL für Ihre SQL-Datenbanken. Azure Defender für SQL umfasst Funktionen zur Ermittlung und Klassifizierung sensibler Daten, Ermittlung und Verringerung potenzieller Datenbankschwachstellen und Erkennung ungewöhnlicher Aktivitäten, die eine Bedrohung für Ihre Datenbank darstellen können.

- [Grundlegendes zu Advanced Threat Protection und zu Warnungen für Azure SQL-Datenbank](/azure/sql-database/sql-database-threat-detection-overview)

- [So aktivieren Sie Azure Defender für SQL für Azure SQL-Datenbank](azure-defender-for-sql.md)

- [Verwalten von Warnungen in Azure Security Center](../../security-center/security-center-managing-and-responding-alerts.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.Sql**:

[!INCLUDE [Resource Policy for Microsoft.Sql 2.7](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-2-7.md)]

## <a name="identity-and-access-control"></a>Identität und Zugriffssteuerung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Identität und Zugriffssteuerung](../../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Verwalten eines Bestands von Administratorkonten

**Leitfaden**: Azure Active Directory (Azure AD) umfasst integrierte Rollen, die explizit zugewiesen werden müssen und abgefragt werden können. Verwenden Sie das Azure AD PowerShell-Modul, um Ad-hoc-Abfragen zum Ermitteln von Konten auszuführen, die Mitglieder von administrativen Gruppen sind.

- [Abrufen einer Verzeichnisrolle in Azure AD mit PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0&amp;preserve-view=true)

- [Abrufen von Mitgliedern einer Verzeichnisrolle in Azure AD mit PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0&amp;preserve-view=true)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Ändern von Standardkennwörtern bei Bedarf

**Leitfaden**: Azure Active Directory (Azure AD) verfolgt nicht das Konzept der Standardkennwörter. Wir empfehlen Ihnen, beim Bereitstellen einer Instanz von Azure SQL-Datenbank die Option zum Integrieren der Authentifizierung in Azure AD auszuwählen.

- [Konfigurieren und Verwalten der Azure Active Directory-Authentifizierung mit Azure SQL](/azure/sql-database/azure-sql/database/authentication-aad-configure)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Verwenden dedizierter Administratorkonten

**Leitfaden**: Erstellen Sie Richtlinien und Vorgänge für die Verwendung dedizierter Administratorkonten. Verwenden Sie die Identitäts- und Zugriffsverwaltung in Azure Security Center, um die Anzahl der Administratorkonten zu überwachen.

- [Grundlegendes zu Identität und Zugriff im Azure Security Center](../../security-center/security-center-identity-access.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Verwenden der mehrstufigen Authentifizierung für den gesamten Azure Active Directory-basierten Zugriff

**Leitfaden**: Aktivieren Sie die mehrstufige Authentifizierung für Azure Active Directory (Azure AD), und befolgen Sie die Empfehlungen für die Identitäts- und Zugriffsverwaltung in Azure Security Center.

- [Planen einer Bereitstellung von Azure AD Multi-Factor Authentication](../../active-directory/authentication/howto-mfa-getstarted.md)

- [Überwachen von Identität und Zugriff in Azure Security Center](../../security-center/security-center-identity-access.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3.6: Verwenden von sicheren, von Azure verwalteten Arbeitsstationen für Verwaltungsaufgaben

**Leitfaden**: Verwenden Sie Privileged Access Workstations (PAW) mit mehrstufiger Authentifizierung, die für die Anmeldung und Konfiguration von Azure-Ressourcen konfiguriert sind.

- [Informationen zu Arbeitsstationen mit privilegiertem Zugriff](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Planen einer Bereitstellung von Azure AD Multi-Factor Authentication](../../active-directory/authentication/howto-mfa-getstarted.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Protokollieren von und Warnen bei verdächtigen Aktivitäten in Administratorkonten

**Leitfaden**: Verwenden Sie Azure Active Directory-Sicherheitsberichte (Azure AD) für die Generierung von Protokollen und Warnungen bei verdächtigen oder sicherheitsrelevanten Aktivitäten in der Umgebung.

Verwenden Sie Advanced Threat Protection für Azure SQL-Datenbank, um Anomalien bei Aktivitäten zu erkennen, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Ihre Datenbanken zuzugreifen oder diese zu nutzen.

- [Identifizieren von Azure AD-Benutzern, die aufgrund riskanter Aktivitäten gekennzeichnet wurden](/azure/active-directory/reports-monitoring/concept-user-at-risk)

- [Überwachen der identitäts- und zugriffsbezogenen Aktivitäten von Benutzern in Azure Security Center](../../security-center/security-center-identity-access.md)

- [Lesen von Informationen zu Advanced Threat Protection-Warnungen und potenziellen Warnungen](https://docs.microsoft.com/azure/azure-sql/database/threat-detection-overview#alerts)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Verwalten von Azure-Ressourcen nur über genehmigte Standorte

**Leitfaden**: Verwenden Sie benannte Standorte mit bedingtem Zugriff, um den Zugriff auf das Portal und die Azure-Ressourcenverwaltung nur über bestimmte logische Gruppierungen von IP-Adressbereichen oder Ländern/Regionen zuzulassen.

- [Konfigurieren benannter Standorte in Azure](../../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="39-use-azure-active-directory"></a>3.9: Verwenden von Azure Active Directory

**Leitfaden**: Erstellen Sie einen Azure AD-Administrator (Azure Active Directory) für Ihre Instanzen von Azure SQL-Datenbank-Server.

- [Konfigurieren und Verwalten der Azure Active Directory-Authentifizierung mit Azure SQL](authentication-aad-configure.md)

- [Erstellen und Konfigurieren einer Azure AD-Instanz](../../active-directory-domain-services/tutorial-create-instance.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.Sql**:

[!INCLUDE [Resource Policy for Microsoft.Sql 3.9](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-3-9.md)]

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Regelmäßiges Überprüfen und Abstimmen des Benutzerzugriffs

**Leitfaden**: Azure Active Directory (Azure AD) enthält Protokolle zum Ermitteln von veralteten Konten. Verwenden Sie zusätzlich Zugriffsüberprüfungen für Azure-Identitäten, um Gruppenmitgliedschaften, den Zugriff auf Unternehmensanwendungen und Rollenzuweisungen effizient zu verwalten. Der Benutzerzugriff kann regelmäßig überprüft werden, um sicherzustellen, dass nur die richtigen Benutzer weiterhin Zugriff besitzen.

- [Verwenden von Zugriffsüberprüfungen für Azure-Identitäten](../../active-directory/governance/access-reviews-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Überwachen von Zugriffsversuchen auf deaktivierte Anmeldeinformationen

**Leitfaden**: Konfigurieren Sie die Azure AD-Authentifizierung (Azure Active Directory) mit Azure SQL, und erstellen Sie Diagnoseeinstellungen für Azure Active Directory-Benutzerkonten. Senden Sie die Überwachungsprotokolle und Anmeldeprotokolle anschließend an einen Log Analytics-Arbeitsbereich. Konfigurieren Sie die gewünschten Warnungen im Log Analytics-Arbeitsbereich.

- [Konfigurieren und Verwalten der Azure Active Directory-Authentifizierung mit Azure SQL](authentication-aad-configure.md)

- [Integrieren von Azure-Aktivitätsprotokollen in Azure Monitor](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12: Warnung bei abweichendem Verhalten bei der Kontoanmeldung

**Leitfaden**: Verwenden Sie Azure Active Directory (AAD) Identity Protection und die Risikoerkennung, um automatische Antworten auf erkannte verdächtige Aktionen im Zusammenhang mit Benutzeridentitäten zu konfigurieren. Außerdem können Sie Daten zur weiteren Untersuchung in Azure Sentinel erfassen.

- [Anzeigen von Azure AD-Risikoanmeldungen](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Konfigurieren und Aktivieren von Risikorichtlinien für den Identitätsschutz](../../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Ermöglichen des Zugriffs auf relevante Kundendaten für Microsoft in Supportszenarien

**Leitfaden**: In Supportszenarien, bei denen Microsoft auf Kundendaten zugreifen muss, wird per Azure-Kunden-Lockbox eine Benutzeroberfläche bereitgestellt, auf der Kunden Anforderungen des Zugriffs auf Kundendaten prüfen und dann genehmigen oder ablehnen können.

- [Grundlegendes zu Kunden-Lockbox](../../security/fundamentals/customer-lockbox-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="data-protection"></a>Datenschutz

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Schutz von Daten](../../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Verwalten eines Bestands an vertraulichen Informationen

**Leitfaden**: Verwenden Sie Tags für die Nachverfolgung von Azure-Ressourcen, die vertrauliche Informationen speichern oder verarbeiten.

- [Erstellen und Verwenden von Tags](/azure/azure-resource-manager/resource-group-using-tags)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.Sql**:

[!INCLUDE [Resource Policy for Microsoft.Sql 4.1](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-4-1.md)]

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Isolieren von Systemen, die vertrauliche Informationen speichern oder verarbeiten

**Leitfaden**: Implementieren Sie separate Abonnements und/oder Verwaltungsgruppen für Entwicklungs-, Test- und Produktionsumgebungen. Ressourcen sollten durch ein VNET/Subnetz getrennt, korrekt gekennzeichnet und mit einer NSG oder Azure Firewall geschützt werden. Ressourcen, die vertrauliche Daten speichern oder verarbeiten, müssen isoliert werden. Verwenden Sie Private Link. Stellen Sie Azure SQL Server in Ihrem VNET bereit, und stellen Sie eine private Verbindung her, indem Sie private Endpunkte verwenden.

- [Erstellen zusätzlicher Azure-Abonnements](/azure/billing/billing-create-subscription)

- [Erstellen von Verwaltungsgruppen](/azure/governance/management-groups/create)

- [Erstellen und Verwenden von Tags](/azure/azure-resource-manager/resource-group-using-tags)

- [Einrichten von Private Link für Azure SQL-Datenbank](/azure/sql-database/sql-database-private-endpoint-overview#how-to-set-up-private-link-for-azure-sql-database)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Überwachen und Blockieren einer nicht autorisierten Übertragung vertraulicher Informationen

**Leitfaden**: Kennzeichnen Sie die Datenbank und die zugehörigen Ressourcen mit Tags als vertraulich, wenn mit Datenbanken in Azure SQL-Datenbank vertrauliche Informationen gespeichert oder verarbeitet werden. Konfigurieren Sie Private Link in Verbindung mit Diensttags für Netzwerksicherheitsgruppen auf Ihren Azure SQL-Datenbankinstanzen, um die Exfiltration von vertraulichen Informationen zu verhindern.

Für die zugrunde liegende Plattform, die von Microsoft verwaltet wird, behandelt Microsoft alle Kundeninhalte als vertraulich und unternimmt große Anstrengungen, um Kundendaten vor Verlust und Gefährdung zu schützen. Um die Sicherheit von Kundendaten innerhalb von Azure zu gewährleisten, hat Microsoft eine Reihe von robusten Datenschutzkontrollen und -funktionen implementiert und kümmert sich um deren Verwaltung.

- [Konfigurieren von Private Link und Netzwerksicherheitsgruppen zur Verhinderung der Exfiltration von Daten auf Ihren Instanzen von Azure SQL-Datenbank](/azure/sql-database/sql-database-private-endpoint-overview)

- [Grundlegendes zum Schutz von Kundendaten in Azure](../../security/fundamentals/protection-customer-data.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Verwenden eines aktiven Ermittlungstools zur Bestimmung vertraulicher Daten

**Leitfaden**: Verwenden Sie die Azure SQL-Datenbank-Funktion zur Datenermittlung und -klassifizierung. Die Datenermittlung und -klassifizierung verfügt über erweiterte Funktionen für Azure SQL-Datenbank zum Ermitteln, Klassifizieren, Bezeichnen und Schützen der vertraulichen Daten in Ihren Datenbanken.

- [Verwenden der Datenermittlung und -klassifizierung für Azure SQL Server](/azure/sql-database/sql-database-data-discovery-and-classification)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.Sql**:

[!INCLUDE [Resource Policy for Microsoft.Sql 4.5](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-4-5.md)]

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4.6: Verwenden der rollenbasierten Zugriffssteuerung zum Steuern des Zugriffs auf Ressourcen

**Leitfaden**: Verwenden Sie Azure Active Directory (AAD), um den Zugriff auf Instanzen von Azure SQL-Datenbank zu authentifizieren und zu steuern.

- [Integrieren von Azure SQL Server in Azure AD für die Authentifizierung](/azure/sql-database/sql-database-aad-authentication)

- [Steuern des Zugriffs in Azure SQL Server](/azure/sql-database/sql-database-control-access)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Verschlüsseln vertraulicher, ruhender Informationen

**Leitfaden**: Transparente Datenverschlüsselung (Transparent Data Encryption, TDE) trägt durch die Verschlüsselung von ruhenden Daten zum Schutz von Azure SQL-Datenbank, einer verwalteten Azure SQL-Instanz und Azure Data Warehouse vor der Bedrohung durch schädliche Offlineaktivitäten bei. TDE ver- und entschlüsselt die Datenbank, die zugehörigen Sicherungen und die Transaktionsprotokolldateien im Ruhezustand in Echtzeit, ohne dass Änderungen an der Anwendung erforderlich sind. TDE ist standardmäßig für alle neu bereitgestellten Datenbanken in SQL-Datenbank und SQL Managed Instance aktiviert. Der TDE-Verschlüsselungsschlüssel kann entweder von Microsoft oder vom Kunden verwaltet werden.

- [Verwalten von Transparent Data Encryption und Verwenden Ihrer eigenen Verschlüsselungsschlüssel](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql?tabs=azure-portal#manage-transparent-data-encryption)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.Sql**:

[!INCLUDE [Resource Policy for Microsoft.Sql 4.8](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-4-8.md)]

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Protokollieren und Warnen bei Änderungen an wichtigen Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Monitor mit dem Azure-Aktivitätsprotokoll, um Warnungen für den Fall zu erstellen, dass Änderungen an Produktionsinstanzen von Azure SQL-Datenbank und anderen kritischen bzw. verwandten Ressourcen vorgenommen werden.

- [Erstellen von Warnungen für Ereignisse des Azure-Aktivitätsprotokolls](/azure/azure-monitor/platform/alerts-activity-log)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="vulnerability-management"></a>Verwaltung von Sicherheitsrisiken

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Verwaltung von Sicherheitsrisiken](../../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Ausführen automatisierter Scan-Tools für Sicherheitsrisiken

**Leitfaden**: Aktivieren Sie Azure Defender für SQL für Azure SQL-Datenbank, und halten Sie sich an die Azure Security Center-Empfehlungen zur Durchführung von Sicherheitsrisikobewertungen auf Ihren Azure SQL-Servern.

- [Ermitteln von Sicherheitsrisiken für die Datenbank mithilfe der SQL-Sicherheitsrisikobewertung](/azure/sql-database/sql-vulnerability-assessment)

- [So aktivieren Sie Azure Defender für SQL](azure-defender-for-sql.md)

- [Implementieren von Empfehlungen für die Sicherheitsrisikobewertung aus Azure Security Center](/azure/security-center/security-center-vulnerability-assessment-recommendations)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.Sql**:

[!INCLUDE [Resource Policy for Microsoft.Sql 5.1](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-5-1.md)]

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4 Vergleichen von kaskadierenden Sicherheitsrisikoscans

**Leitfaden**: Aktivieren Sie regelmäßig wiederkehrende Überprüfungen für Ihre Instanzen von Azure SQL-Datenbank. Hierdurch wird eine Sicherheitsrisikobewertung konfiguriert, um einmal pro Woche eine Überprüfung Ihrer Datenbank durchzuführen. Eine Zusammenfassung der Scanergebnisse wird an die angegebenen E-Mail-Adressen gesendet. Vergleichen Sie die Ergebnisse, um sicherzustellen, dass die Sicherheitsrisiken beseitigt wurden.

- [Exportieren eines Berichts zur Sicherheitsrisikobewertung in Azure Security Center](/azure/sql-database/sql-vulnerability-assessment#implementing-vulnerability-assessment)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Verwenden eines Risikobewertungsprozesses, um die Behebung von erkannten Sicherheitsrisiken zu priorisieren

**Leitfaden**: Verwenden Sie die Standardrisikobewertungen (Secure Score), die von Azure Security Center bereitgestellt werden.

- [Grundlegendes zum Secure Score des Azure Security Center](/azure/security-center/security-center-secure-score)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.Sql**:

[!INCLUDE [Resource Policy for Microsoft.Sql 5.5](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-5-5.md)]

## <a name="inventory-and-asset-management"></a>Bestands- und Ressourcenverwaltung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Bestands- und Ressourcenverwaltung](../../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Verwenden der automatisierten Asset Discovery-Lösung

**Leitfaden**: Verwenden Sie Azure Resource Graph, um alle Ressourcen (einschließlich Azure SQL Server-Instanzen) innerhalb Ihrer Abonnements abzufragen und zu ermitteln. Stellen Sie sicher, dass Sie über entsprechende (Lese-)Berechtigungen in Ihrem Mandanten verfügen und alle Azure-Abonnements und Ressourcen in Ihren Abonnements auflisten können.

Obwohl klassische Azure-Ressourcen über das Resource Graph ermittelt werden können, wird dringend empfohlen, Azure Resource Manager-Ressourcen zu erstellen und zu verwenden.

- [Erstellen von Abfragen mit Azure Resource Graph](../../governance/resource-graph/first-query-portal.md)

- [Anzeigen Ihrer Azure-Abonnements](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-4.8.0&amp;preserve-view=true)

- [Grundlegendes zu Azure RBAC](../../role-based-access-control/overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="62-maintain-asset-metadata"></a>6.2: Verwalten von Ressourcenmetadaten

**Leitfaden**: Wenden Sie Tags auf Ihre Azure-Ressourcen an, die Metadaten erzeugen, um sie logisch in einer Taxonomie zu organisieren.

- [Erstellen und Verwenden von Tags](/azure/azure-resource-manager/resource-group-using-tags)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Löschen nicht autorisierter Azure-Ressourcen

**Leitfaden**: Verwenden Sie nach Bedarf Tagging, Verwaltungsgruppen und separate Abonnements zum Organisieren und Nachverfolgen von Ressourcen. Stimmen Sie den Bestand regelmäßig ab, und stellen Sie sicher, dass nicht autorisierte Ressourcen rechtzeitig aus dem Abonnement gelöscht werden.

- [Erstellen zusätzlicher Azure-Abonnements](/azure/billing/billing-create-subscription)

- [Erstellen von Verwaltungsgruppen](/azure/governance/management-groups/create)

- [Erstellen und Verwenden von Tags](/azure/azure-resource-manager/resource-group-using-tags)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Überwachung auf nicht genehmigte Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy, um Einschränkungen für den Typ der Ressourcen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie hierzu die folgenden integrierten Richtliniendefinitionen:

- Not allowed resource types (Unzulässige Ressourcentypen)

- Zulässige Ressourcentypen

Verwenden Sie Azure Resource Graph, um Ressourcen in Ihren Abonnements abzufragen und zu ermitteln. Stellen Sie sicher, dass alle in der Umgebung vorhandenen Azure-Ressourcen genehmigt sind.

- [Konfigurieren und Verwalten von Azure Policy](../../governance/policy/tutorials/create-and-manage.md)

- [Erstellen von Abfragen mit Azure Resource Graph](../../governance/resource-graph/first-query-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="69-use-only-approved-azure-services"></a>6.9: Ausschließliche Verwendung genehmigter Azure-Dienste

**Leitfaden**: Verwenden Sie Azure Policy, um Einschränkungen für den Typ der Ressourcen festzulegen, die in Kundenabonnements erstellt werden können. Nutzen Sie hierzu die folgenden integrierten Richtliniendefinitionen:
- Not allowed resource types (Unzulässige Ressourcentypen)
- Zulässige Ressourcentypen

Verwenden Sie Azure Resource Graph, um Ressourcen in Ihren Abonnements abzufragen und zu ermitteln. Stellen Sie sicher, dass alle in der Umgebung vorhandenen Azure-Ressourcen genehmigt sind.

- [Konfigurieren und Verwalten von Azure Policy](../../governance/policy/tutorials/create-and-manage.md)

- [Ablehnen eines bestimmten Ressourcentyps mit Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#general)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Einschränken der Möglichkeiten von Benutzern zur Interaktion mit Azure Resource Manager

**Leitfaden**: Verwenden Sie den bedingten Azure-Zugriff, um die Möglichkeiten der Benutzer zur Interaktion mit Azure Resource Manager einzuschränken, indem Sie „Zugriff blockieren“ für die App zur „Verwaltung von Microsoft Azure“ konfigurieren.

- [Verwalten des Zugriffs auf die Azure-Verwaltung mit bedingtem Zugriff](../../role-based-access-control/conditional-access-azure-management.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="secure-configuration"></a>Sichere Konfiguration

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Sichere Konfiguration](../../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Einrichten sicherer Konfigurationen für alle Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy- oder Azure Security Center-Empfehlungen für Azure SQL Server/-Datenbank-Instanzen, um Sicherheitskonfigurationen für alle Azure-Ressourcen zu verwalten.

- [Konfigurieren und Verwalten von Azure Policy](../../governance/policy/tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Verwalten von sicheren Konfigurationen für Azure-Ressourcen

**Leitfaden**: Verwenden Sie die Azure Policy-Auswirkungen [Deny] und [DeployIfNotExists], um sichere Einstellungen für alle Ihre Azure-Ressourcen zu erzwingen.

- [Konfigurieren und Verwalten von Azure Policy](../../governance/policy/tutorials/create-and-manage.md)

- [Grundlegendes zu Azure Policy-Auswirkungen](../../governance/policy/concepts/effects.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Sicheres Speichern der Konfiguration von Azure-Ressourcen

**Leitfaden**: Wenn Sie benutzerdefinierte Azure Policy-Definitionen verwenden, nutzen Sie Azure DevOps oder Azure Repos, um Ihren Code sicher zu speichern und zu verwalten.

- [Speichern von Code in Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops&amp;preserve-view=true)

- [Dokumentation zu Azure Repos](https://docs.microsoft.com/azure/devops/repos/?view=azure-devops&amp;preserve-view=true)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Bereitstellen von Konfigurationsverwaltungstools für Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy-Aliase im Namespace „Microsoft.SQL“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen von Systemkonfigurationen zu erstellen und Warnungen dazu zu erhalten. Entwickeln Sie außerdem einen Prozess und eine Pipeline zum Verwalten von Richtlinienausnahmen.

- [Konfigurieren und Verwalten von Azure Policy](../../governance/policy/tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementieren der automatisierten Konfigurationsüberwachung für Azure-Ressourcen

**Leitfaden**: Nutzen Sie Azure Security Center, um Baselineüberprüfungen für Ihre Azure SQL-Server und -Datenbanken durchzuführen.

- [Umsetzen von Empfehlungen in Azure Security Center](/azure/security-center/security-center-sql-service-recommendations)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="711-manage-azure-secrets-securely"></a>7.11: Sicheres Verwalten von Azure-Geheimnissen

**Leitfaden**: Verwenden Sie Azure Key Vault zum Speichern von Verschlüsselungsschlüsseln für Transparent Data Encryption (TDE) von Azure SQL-Datenbank.

- [Schützen von vertraulichen Daten, die unter Azure SQL Server gespeichert sind, und Speichern der Verschlüsselungsschlüssel in Azure Key Vault](/azure/sql-database/sql-database-always-encrypted-azure-key-vault)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Sicheres und automatisches Verwalten von Identitäten

**Leitfaden**: Verwenden Sie verwaltete Identitäten, um Azure-Dienste mit einer automatisch verwalteten Identität in Azure Active Directory (AAD) bereitzustellen. Mit verwalteten Identitäten können Sie die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt, einschließlich Azure Key Vault. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein.

- [Tutorial: Verwenden der systemseitig zugewiesenen verwalteten Identität eines virtuellen Windows-Computers für den Zugriff auf Azure SQL](../../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-sql.md)

- [Konfigurieren von verwalteten Identitäten](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Beheben der unbeabsichtigten Offenlegung von Anmeldeinformationen

**Leitfaden**: Implementieren Sie Credential Scanner, um Anmeldeinformationen in Ihrem Code zu identifizieren. In Credential Scanner wird auch das Verschieben von ermittelten Anmeldeinformationen an sicherere Speicherorte (z. B. Azure Key Vault) empfohlen. 

- [Einrichten von Credential Scanner](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="malware-defense"></a>Schutz vor Schadsoftware

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Schutz vor Schadsoftware](../../security/benchmarks/security-control-malware-defense.md).*

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: Vorabprüfen von in computefremde Azure-Ressourcen hochzuladenden Dateien

**Leitfaden**: Microsoft Antimalware ist auf dem zugrunde liegenden Host für die Azure-Dienste (z. B. Azure App Service) aktiviert, wird aber nicht für Kundeninhalte ausgeführt.

Führen Sie eine Vorabprüfung aller Inhalte durch, die in computefremde Azure-Ressourcen hochgeladen werden sollen, z. B. App Service, Data Lake Storage, Blob Storage, Azure SQL Server usw. Microsoft kann auf diesen Instanzen nicht auf Ihre Daten zugreifen.

- [Grundlegendes zu Microsoft Antimalware für Azure Cloud Services und Virtual Machines](../../security/fundamentals/antimalware.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="data-recovery"></a>Datenwiederherstellung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Datenwiederherstellung](../../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Sicherstellen regelmäßiger automatisierter Sicherungen

**Leitfaden**: Um Ihr Unternehmen vor Datenverlusten zu schützen, werden von Azure SQL-Datenbank automatisch wöchentliche vollständige Datenbanksicherungen, alle 12 Stunden differenzielle Datenbanksicherungen und alle 5 bis 10 Minuten Transaktionsprotokollsicherungen durchgeführt. Die Sicherungen werden für alle Dienstebenen mindestens 7 Tage im RA-GRS-Speicher vorgehalten. Alle Dienstebenen außer „Basic“ unterstützen konfigurierbare Aufbewahrungszeiträume von bis zu 35 Tagen für die Point-in-Time-Wiederherstellung.

Zur Einhaltung unterschiedlicher Konformitätsanforderungen können Sie verschiedene Aufbewahrungszeiträume für wöchentliche, monatliche oder jährliche Sicherungen auswählen. Der Speicherverbrauch hängt von der ausgewählten Häufigkeit der Sicherungen und von den Aufbewahrungsdauern ab.

- [Grundlegendes zu Sicherungen und Geschäftskontinuität mit Azure SQL Server](/azure/sql-database/sql-database-business-continuity)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.Sql**:

[!INCLUDE [Resource Policy for Microsoft.Sql 9.1](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-9-1.md)]

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Durchführen vollständiger Systemsicherungen und Sichern aller kundenseitig verwalteten Schlüssel

**Leitfaden**: Azure SQL-Datenbank erstellt automatisch die Datenbanksicherungen, für die eine Speicherdauer von mindestens 7 und maximal 35 Tagen gilt. Es wird georedundanter Azure-Speicher mit Lesezugriff (RA-GRS) verwendet, um sicherzustellen, dass diese auch dann beibehalten werden, wenn das Rechenzentrum nicht verfügbar ist. Diese Sicherungskopien werden automatisch erstellt. Aktivieren Sie bei Bedarf langfristige georedundante Sicherungen für Ihre Azure SQL-Datenbank-Instanzen.

Stellen Sie beim Verwenden der vom Kunden verwalteten Schlüssel für Transparent Data Encryption sicher, dass Ihre Schlüssel gesichert werden.

- [Grundlegendes zu Sicherungen in Azure SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups?tabs=single-database)

- [Sichern von Schlüsseltresorschlüsseln in Azure](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey?view=azps-4.8.0&amp;preserve-view=true)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.Sql**:

[!INCLUDE [Resource Policy for Microsoft.Sql 9.2](../../../includes/policy/standards/asb/rp-controls/microsoft.sql-9-2.md)]

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Überprüfen aller Sicherungen einschließlich kundenseitig verwalteter Schlüssel

**Leitfaden**: Stellen Sie regelmäßig sicher, dass die Inhalte in Azure Backup wiederhergestellt werden können. Testen Sie die Wiederherstellung von Inhalten ggf. in einem isolierten VLAN. Testen Sie die Wiederherstellung von gesicherten von Kunden verwalteten Schlüsseln.

- [Wiederherstellen von Schlüsseltresorschlüsseln in Azure](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultkey?view=azps-4.8.0&amp;preserve-view=true)

- [Wiederherstellen von Azure SQL-Datenbank-Sicherungen per Point-in-Time-Wiederherstellung](/azure/sql-database/sql-database-recovery-using-backups#point-in-time-restore)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Sicherstellen des Schutzes von Sicherungen und von kundenseitig verwalteten Schlüsseln

**Leitfaden**: Aktivieren Sie das vorläufige Löschen in Azure Key Vault, um Schlüssel vor dem versehentlichen oder böswilligen Löschen zu schützen.

- [Aktivieren des vorläufigen Löschens in Key Vault](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="incident-response"></a>Reaktion auf Vorfälle

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Reaktion auf Vorfälle](../../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Erstellen eines Leitfadens für die Reaktion auf Vorfälle

**Leitfaden**: Stellen Sie sicher, dass es schriftliche Pläne für die Reaktion auf Vorfälle gibt, in denen die Rollen der Mitarbeiter sowie die Phasen der Bearbeitung und Verwaltung von Vorfällen definiert sind.

- [Leitfaden zu Planung und Betrieb](../../security-center/security-center-planning-and-operations-guide.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Erstellen eines Verfahrens zur Bewertung und Priorisierung von Vorfällen

**Leitfaden**: Security Center weist Warnungen einen Schweregrad zu, um Ihnen zu helfen, die Reihenfolge zu priorisieren, in der Sie sich um Warnungen kümmern. So können Sie, sobald eine Ressource gefährdet ist, sofort zu ihr gelangen. Der Schweregrad basiert darauf, wie zuversichtlich Security Center in Bezug auf den Befund oder die Analyse ist, die zum Auslösen der Warnung verwendet wird, sowie auf dem Zuverlässigkeitsgrad, dass hinter der Aktivität, die zu der Warnung führte, eine böswillige Absicht stand.

- [Sicherheitswarnungen in Azure Security Center](../../security-center/security-center-alerts-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="103-test-security-response-procedures"></a>10.3: Verfahren zum Testen der Reaktion auf Sicherheitsvorfälle

**Leitfaden**: Führen Sie in regelmäßigen Abständen Tests zur Reaktionsfähigkeit Ihrer Systeme auf Vorfälle durch. Identifizieren Sie Schwachstellen und Lücken, und überarbeiten Sie den Plan bei Bedarf.

- [Informationen hierzu finden Sie in der folgenden Veröffentlichung des NIST: des National Institute of Standards and Technology (NIST)](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Angeben von Kontaktdaten für Sicherheitsvorfälle und Konfigurieren von Warnungsbenachrichtigungen für Sicherheitsvorfälle

**Leitfaden**: Microsoft kontaktiert Sie unter den für Sicherheitsvorfälle angegebenen Kontaktdaten, wenn das Microsoft Security Response Center (MSRC) feststellt, dass Personen unrechtmäßig oder unbefugt auf Ihre Daten zugegriffen haben.

- [Festlegen der Kontaktinformationen in Azure Security Center](../../security-center/security-center-provide-security-contact-details.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Integrieren von Sicherheitswarnungen in das System zur Reaktion auf Vorfälle

**Leitfaden**: Exportieren Sie die Azure Security Center-Warnungen und -Empfehlungen über die Funktion „Fortlaufender Export“. Über „Fortlaufender Export“ können Sie Warnungen und Empfehlungen entweder manuell oder kontinuierlich exportieren. Sie können den Azure Security Center-Datenconnector verwenden, um die Warnungen an Azure Sentinel zu streamen.

- [Konfigurieren des fortlaufenden Exports](../../security-center/continuous-export.md)

- [Streamen von Warnungen in Azure Sentinel](../../sentinel/connect-azure-security-center.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatisieren der Reaktion auf Sicherheitswarnungen

**Leitfaden**: Verwenden Sie die Funktion „Workflowautomatisierung“ in Azure Security Center, um über „Logic Apps“ automatisch Reaktionen auf Sicherheitswarnungen und -empfehlungen auszulösen.

- [Konfigurieren von Workflowautomatisierung und Logic Apps](../../security-center/workflow-automation.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="penetration-tests-and-red-team-exercises"></a>Penetrationstests und Red Team-Übungen

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Penetrationstests und Red Team-Übungen](../../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1: Durchführen regelmäßiger Penetrationstests Ihrer Azure-Ressourcen und Sicherstellen der Behebung aller kritischen Sicherheitsergebnissen

**Leitfaden**: Befolgen Sie in dem Fall die Einsatzregeln für Penetrationstests von Microsoft Cloud, um sicherzustellen, dass die Penetrationstests nicht gegen Microsoft-Richtlinien verstoßen. Nutzen Sie die Microsoft-Strategie und Durchführung von Red Team- und Livewebsite-Penetrationstests für von Microsoft verwaltete Cloudinfrastruktur, Dienste und Anwendungen. 

- [Penetrationstests – Rules of Engagement](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Cloud Red Teaming](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die [Übersicht über Version 2 des Azure-Sicherheitsvergleichstests](/azure/security/benchmarks/overview) an.
- Erfahren Sie mehr über [Azure-Sicherheitsbaselines](/azure/security/benchmarks/security-baselines-overview).
