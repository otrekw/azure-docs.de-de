---
title: Azure-Sicherheitsbaseline für HDInsight
description: Die HDInsight-Sicherheitsbaseline enthält Anleitungen zu Verfahren und Ressourcen für die Implementierung der Sicherheitsempfehlungen, die im Azure-Sicherheitsvergleichstest angegeben sind.
author: msmbaldwin
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: bf2360bda55735aa8ef4258da5ae47f673f4d71b
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101738921"
---
# <a name="azure-security-baseline-for-hdinsight"></a>Azure-Sicherheitsbaseline für HDInsight

Diese Sicherheitsbaseline wendet Empfehlungen des [Azure-Sicherheitsvergleichstests Version 1.0](../security/benchmarks/overview-v1.md) auf HDInsight an. Der Azure-Sicherheitsvergleichstest enthält Empfehlungen zum Schutz Ihrer Cloudlösungen in Azure.
Der Inhalt wird anhand der **Sicherheitskontrollen** gruppiert, die durch den Azure-Sicherheitsvergleichstest und die entsprechenden für HDInsight geltenden Empfehlungen definiert sind. Nicht auf HDInsight anwendbare **Kontrollen** wurden ausgeschlossen.

 
Die vollständige Zuordnung von HDInsight zum Azure-Sicherheitsvergleichstest finden Sie in der [vollständigen Zuordnungsdatei der HDInsight-Sicherheitsbaseline](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Netzwerksicherheit

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Netzwerksicherheit](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Schützen von Azure-Ressourcen in virtuellen Netzwerken

**Leitfaden**: Die Umgebungssicherheit in Azure HDInsight wird mithilfe von virtuellen Netzwerken erreicht. Ein Unternehmensadministrator kann einen Cluster in einem virtuellen Netzwerk erstellen und den Zugriff auf das virtuelle Netzwerk mithilfe einer Netzwerksicherheitsgruppe (NSG) beschränken. Nur die zulässigen IP-Adressen in den eingehenden Netzwerksicherheitsgruppen-Regeln können mit dem Azure HDInsight-Cluster kommunizieren. Diese Konfiguration bietet Umgebungssicherheit. Alle in einem virtuellen Netzwerk bereitgestellten Cluster verfügen auch über einen privaten Endpunkt, der in eine private IP-Adresse innerhalb des virtuellen Netzwerks aufgelöst wird, um privaten HTTP-Zugriff auf die Clustergateways zu ermöglichen.

Beschränken Sie Netzwerkdatenverkehr in ausgehender Richtung für Azure HDInsight-Cluster per Azure Firewall, um das Risiko für Datenverlust aufgrund einer Exfiltration zu verringern.

- [Bereitstellen von Azure HDInsight in einem virtuellen Netzwerk und Schützen mit einer Netzwerksicherheitsgruppe](hdinsight-create-virtual-network.md)

- [Einschränken des ausgehenden Datenverkehrs für Azure HDInsight-Cluster per Azure Firewall](hdinsight-restrict-outbound-traffic.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2: Überwachen und Protokollieren der Konfiguration und des Datenverkehrs von virtuellen Netzwerken, Subnetzen und Netzwerkschnittstellen

**Leitfaden**: Verwenden Sie Azure Security Center, und setzen Sie die Empfehlungen zum Netzwerkschutz für das virtuelle Netzwerk, das Subnetz und die Netzwerksicherheitsgruppe um, die für den Schutz Ihres Azure HDInsight-Clusters genutzt werden. Aktivieren Sie NSG-Flussprotokolle (Netzwerksicherheitsgruppe), und senden Sie Protokolle an ein Azure Storage-Konto, um den Datenverkehr zu überwachen. Sie können NSG-Flussprotokolle auch an einen Azure Log Analytics-Arbeitsbereich senden und Azure Traffic Analytics verwenden, um Einblicke in den Datenverkehrsfluss in Ihrer Azure-Cloud zu ermöglichen. Einige Vorteile von Azure Traffic Analytics sind die Möglichkeit, die Netzwerkaktivität zu visualisieren und Hotspots zu erkennen, Sicherheitsbedrohungen zu erkennen, Datenverkehrsflussmuster zu verstehen und Netzwerkfehlkonfigurationen zu ermitteln.

- [Aktivieren der NSG-Flussprotokolle](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Traffic Analytics](../network-watcher/traffic-analytics.md)

- [Grundlegendes zu der von Azure Security Center bereitgestellten Netzwerksicherheit](../security-center/security-center-network-recommendations.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Ablehnen der Kommunikation mit bekannten schädlichen IP-Adressen

**Leitfaden**: Aktivieren Sie als Schutz vor DDoS-Angriffen den Azure-DDoS-Standardschutz für das virtuelle Netzwerk, in dem Ihre Azure HDInsight-Instanz bereitgestellt wurde. Verwenden Sie die in Azure Security Center integrierte Threat Intelligence, um die Kommunikation mit bekannten schädlichen oder nicht genutzten IP-Adressen zu verweigern.

- [Konfigurieren von DDoS-Schutz](/azure/virtual-network/manage-ddos-protection)

- [Grundlegendes zur integrierten Threat Intelligence in Azure Security Center](/azure/security-center/security-center-alerts-service-layer)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="15-record-network-packets"></a>1.5: Aufzeichnen von Netzwerkpaketen

**Leitfaden**: Aktivieren Sie NSG-Flussprotokolle für die NSG (Netzwerksicherheitsgruppe), die an das Subnetz angefügt wurde, mit dem Ihr Azure HDInsight-Cluster geschützt wird. Erfassen Sie die NSG-Flussprotokolle in einem Azure Storage-Konto, um Flussdatensätze zu generieren. Aktivieren Sie die Azure Network Watcher-Paketerfassung, falls dies für die Untersuchung anomaler Aktivitäten erforderlich ist.

- [Aktivieren der NSG-Flussprotokolle](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Aktivieren von Network Watcher](../network-watcher/network-watcher-create.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Bereitstellen von netzwerkbasierten Angriffserkennungs-/Eindringschutzsystemen (Intrusion Detection/Intrusion Prevention Systems, IDS/IPS)

**Leitfaden**: Anforderung kann erfüllt werden: Azure-Sicherheitskontrolle, ID 1.1. Stellen Sie den Azure HDInsight-Cluster in einem virtuellen Netzwerk bereit, und schützen Sie ihn durch eine Netzwerksicherheitsgruppe (NSG).

Es gibt mehrere Abhängigkeiten für Azure HDInsight, für die eingehender Datenverkehr erforderlich ist. Der eingehende Verwaltungsdatenverkehr kann nicht über ein Firewallgerät gesendet werden. Die Quelladressen für den erforderlichen Verwaltungsdatenverkehr sind bekannt und werden veröffentlicht. Erstellen Sie mit diesen Informationen Netzwerksicherheitsgruppen-Regeln, um nur Datenverkehr von vertrauenswürdigen Orten zuzulassen und so den eingehenden Datenverkehr für Cluster zu schützen.

Beschränken Sie Netzwerkdatenverkehr in ausgehender Richtung für Azure HDInsight-Cluster per Azure Firewall, um das Risiko für Datenverlust aufgrund einer Exfiltration zu verringern.

- [Bereitstellen von HDInsight in einem virtuellen Netzwerk und Schützen mit einer Netzwerksicherheitsgruppe](hdinsight-create-virtual-network.md)

- [Grundlegendes zu HDInsight-Abhängigkeiten und zur Firewallverwendung](hdinsight-restrict-outbound-traffic.md)

- [HDInsight-Verwaltungs-IP-Adressen](hdinsight-management-ip-addresses.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimieren der Komplexität und des Verwaltungsaufwands von Netzwerksicherheitsregeln

**Leitfaden**: Verwenden Sie Diensttags in virtuellen Netzwerken, um Netzwerkzugriffssteuerungen in Netzwerksicherheitsgruppen (NSGs) zu definieren, die an das Subnetz angefügt sind, in dem Ihr Azure HDInsight-Cluster bereitgestellt wurde. Sie können Diensttags anstelle von spezifischen IP-Adressen nutzen, wenn Sie Sicherheitsregeln erstellen. Indem Sie den Diensttagnamen (z. B. „ApiManagement“) im entsprechenden Quell- oder Zielfeld einer Regel angeben, können Sie den Datenverkehr für den entsprechenden Dienst zulassen oder verweigern. Microsoft verwaltet die Adresspräfixe, für die das Diensttag gilt, und aktualisiert das Diensttag automatisch, wenn sich die Adressen ändern.

- [Grundlegendes zu Diensttags und Verwenden von Diensttags für Azure HDInsight](/azure/virtual-network/security-overview#service-tags)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Beibehalten von Standardsicherheitskonfigurationen für Netzwerkgeräte

**Leitfaden**: Definieren und implementieren Sie Standardsicherheitskonfigurationen für die Netzwerkressourcen Ihres Azure HDInsight-Clusters. Verwenden Sie Azure Policy-Aliase in den Namespaces „Microsoft.HDInsight“ und „Microsoft.Network“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen der Netzwerkkonfiguration Ihres Azure HDInsight-Clusters zu erstellen.

Sie können umfangreiche Azure-Bereitstellungen auch mithilfe von Azure Blueprints vereinfachen, indem Sie wichtige Umgebungsartefakte wie Azure Resource Manager-Vorlagen, Azure RBAC-Kontrollen und Richtlinien in einer einzigen Blaupausendefinition packen. Sie können die Blaupause leicht auf neue Abonnements und Umgebungen anwenden und die Steuerung und Verwaltung durch die Versionsverwaltung optimieren.

- [Anzeigen verfügbarer Azure Policy-Aliase](/powershell/module/az.resources/get-azpolicyalias)

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Erstellen einer Azure-Blaupause](../governance/blueprints/create-blueprint-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="110-document-traffic-configuration-rules"></a>1.10: Dokumentieren von Datenverkehrskonfigurationsregeln

**Leitfaden**: Verwenden Sie Tags für Netzwerksicherheitsgruppen (NSGs) und andere Ressourcen im Zusammenhang mit Netzwerksicherheit und Datenverkehrsfluss, die Ihrem Azure HDInsight-Cluster zugeordnet sind. Verwenden Sie für einzelne NSG-Regeln das Feld „Beschreibung“, um geschäftliche Anforderungen und/oder deren Dauer (usw.) für alle Regeln festzulegen, die Datenverkehr in ein bzw. aus einem Netzwerk zulassen.

Verwenden Sie eine der integrierten Azure Policy-Definitionen zum Tagging, z. B. „Tag und zugehörigen Wert erzwingen“, um sicherzustellen, dass alle Ressourcen mit Tags erstellt werden und Sie über vorhandene nicht markierte Ressourcen benachrichtigt werden.

Sie können Azure PowerShell oder die Azure-Befehlszeilenschnittstelle (CLI) verwenden, um Ressourcen anhand ihrer Tags zu suchen oder Aktionen auszuführen.

- [Erstellen und Verwenden von Tags](/azure/azure-resource-manager/resource-group-using-tags)

- [Erstellen eines virtuellen Netzwerks](../virtual-network/quick-create-portal.md)

- [Erstellen einer NSG mit einer Sicherheitskonfiguration](../virtual-network/tutorial-filter-network-traffic.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Verwenden automatisierter Tools zum Überwachen von Netzwerkressourcenkonfigurationen und Erkennen von Änderungen

**Leitfaden**: Verwenden Sie das Azure-Aktivitätsprotokoll zum Überwachen der Konfigurationen von Netzwerkressourcen und zum Erkennen von Änderungen bei Netzwerkressourcen, die sich auf Ihre Azure HDInsight-Bereitstellungen beziehen. Erstellen Sie Warnungen in Azure Monitor, die bei Änderungen an wichtigen Netzwerkressourcen ausgelöst werden.

- [Anzeigen und Abrufen von Azure-Aktivitätsprotokollereignissen](/azure/azure-monitor/platform/activity-log-view)

- [Erstellen von Warnungen in Azure Monitor](/azure/azure-monitor/platform/alerts-activity-log)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="logging-and-monitoring"></a>Protokollierung und Überwachung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Protokollierung und Überwachung](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Konfigurieren der zentralen Sicherheitsprotokollverwaltung

**Leitfaden**: Sie können für Ihren Azure HDInsight-Cluster das Onboarding für Azure Monitor durchführen, um die vom Cluster generierten Sicherheitsdaten zu aggregieren. Nutzen Sie benutzerdefinierte Abfragen, um Bedrohungen in der Umgebung zu erkennen und darauf zu reagieren. 

- [Durchführen des Onboardings für einen Azure HDInsight-Cluster in Azure Monitor](hdinsight-hadoop-oms-log-analytics-tutorial.md)

- [Erstellen von benutzerdefinierten Abfragen für einen Azure HDInsight-Cluster](hdinsight-hadoop-oms-log-analytics-use-queries.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Aktivieren der Überwachungsprotokollierung für Azure-Ressourcen

**Leitfaden**: Aktivieren Sie Azure Monitor für den HDInsight-Cluster, und verweisen Sie dafür auf einen Log Analytics-Arbeitsbereich. Hierdurch werden dann alle relevanten Clusterinformationen und Betriebssystemmetriken für alle Azure HDInsight-Clusterknoten protokolliert.

- [Aktivieren der Protokollierung für den HDInsight-Cluster](hdinsight-hadoop-oms-log-analytics-tutorial.md)

- [Abfragen von HDInsight-Protokollen](hdinsight-hadoop-oms-log-analytics-use-queries.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: Erfassen von Sicherheitsprotokollen von Betriebssystemen

**Leitfaden**: Führen Sie das Onboarding für einen Azure HDInsight-Cluster für Azure Monitor durch. Stellen Sie sicher, dass für den verwendeten Log Analytics-Arbeitsbereich der Protokollaufbewahrungszeitraum gemäß den Konformitätsbestimmungen Ihrer Organisation festgelegt ist.

- [Durchführen des Onboardings für einen Azure HDInsight-Cluster in Azure Monitor](hdinsight-hadoop-oms-log-analytics-tutorial.md)

- [Verwalten von Nutzung und Kosten mit Azure Monitor-Protokollen](/azure/azure-monitor/platform/manage-cost-storage)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="25-configure-security-log-storage-retention"></a>2.5: Konfigurieren der Sicherheitsprotokoll-Aufbewahrungsdauer im Speicher

**Leitfaden**: Führen Sie das Onboarding für einen Azure HDInsight-Cluster für Azure Monitor durch. Stellen Sie sicher, dass für den verwendeten Azure Log Analytics-Arbeitsbereich der Protokollaufbewahrungszeitraum gemäß den Konformitätsbestimmungen Ihrer Organisation festgelegt ist.

- [Durchführen des Onboardings für einen Azure HDInsight-Cluster in Azure Monitor](hdinsight-hadoop-oms-log-analytics-tutorial.md)

- [Verwalten von Nutzung und Kosten mit Azure Monitor-Protokollen](/azure/azure-monitor/platform/manage-cost-storage)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="26-monitor-and-review-logs"></a>2.6: Überwachen und Überprüfen von Protokollen

**Leitfaden**: Verwenden Sie Abfragen für den Azure Log Analytics-Arbeitsbereich, um Azure HDInsight-Protokolle abzufragen:

- [Erstellen von benutzerdefinierten Abfragen für Azure HDInsight-Cluster](hdinsight-hadoop-oms-log-analytics-use-queries.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Aktivieren von Warnungen bei anomalen Aktivitäten

**Leitfaden**: Verwenden Sie den Azure Log Analytics-Arbeitsbereich zum Überwachen von und Warnen vor anomalen Aktivitäten in Sicherheitsprotokollen und -ereignissen Ihres Azure HDInsight-Clusters.

- [Verwalten von Warnungen in Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md)

- [Warnungen bei Log Analytics-Protokolldaten](/azure/azure-monitor/learn/tutorial-response)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="28-centralize-anti-malware-logging"></a>2.8: Zentralisieren der Antischadsoftwareprotokollierung

**Leitfaden**: Bei Azure HDInsight ist Clamscan vorinstalliert und für die Clusterknotenimages aktiviert, aber Sie müssen die Software verwalten und alle von Clamscan erzeugten Protokolle manuell aggregieren bzw. überwachen.

- [Grundlegendes zu Clamscan](https://docs.microsoft.com/azure/hdinsight/hdinsight-faq#security-and-certificates)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="29-enable-dns-query-logging"></a>2.9: Aktivieren der DNS-Abfrageprotokollierung

**Leitfaden**: Implementieren Sie eine Drittanbieterlösung für die DNS-Protokollierung.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="210-enable-command-line-audit-logging"></a>2.10: Aktivieren der Befehlszeilen-Überwachungsprotokollierung

**Leitfaden**: Führen Sie die manuelle Konfiguration der Konsolenprotokollierung jeweils für die einzelnen Knoten durch.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="identity-and-access-control"></a>Identität und Zugriffssteuerung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Identität und Zugriffssteuerung](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Verwalten eines Bestands von Administratorkonten

**Leitfaden**: Verwalten Sie den Datensatz für das lokale Administratorkonto, das während der Clusterbereitstellung des Azure HDInsight-Clusters erstellt wird, sowie für alle anderen von Ihnen erstellten Konten. Wenn außerdem die Azure AD-Integration verwendet wird, verfügt Azure AD über integrierte Rollen, die explizit zugewiesen werden müssen und daher abgefragt werden können. Verwenden Sie das Azure AD PowerShell-Modul, um Ad-hoc-Abfragen zum Ermitteln von Konten durchzuführen, die Mitglieder von Verwaltungsgruppen sind.

Außerdem können Sie Empfehlungen für die Identitäts- und Zugriffsverwaltung in Azure Security Center anwenden.

- [Abrufen einer Verzeichnisrolle in Azure AD mit PowerShell](/powershell/module/azuread/get-azureaddirectoryrole)

- [Abrufen von Mitgliedern einer Verzeichnisrolle in Azure AD mit PowerShell](/powershell/module/azuread/get-azureaddirectoryrolemember)

- [Überwachen der Identität und des Zugriffs](../security-center/security-center-identity-access.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Ändern von Standardkennwörtern bei Bedarf

**Leitfaden**: Beim Bereitstellen eines Clusters müssen Sie in Azure neue Kennwörter für das Webportal und den SSH-Zugriff (Secure Shell) erstellen. Es müssen keine Standardkennwörter geändert werden, aber Sie können unterschiedliche Kennwörter für den SSH- und Webportalzugriff angeben.

- [Festlegen von Kennwörtern beim Bereitstellen eines Azure HDInsight-Clusters](hdinsight-hadoop-linux-use-ssh-unix.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Verwenden dedizierter Administratorkonten

**Leitfaden**: Integrieren Sie die Authentifizierung für Azure HDInsight-Cluster in Azure Active Directory (Azure AD). Erstellen Sie Richtlinien und Vorgänge für die Verwendung dedizierter Administratorkonten.

Außerdem können Sie Empfehlungen für die Identitäts- und Zugriffsverwaltung in Azure Security Center anwenden.

- [Integrieren der Azure HDInsight-Authentifizierung in Azure AD](domain-joined/apache-domain-joined-configure-using-azure-adds.md)

- [Überwachen der Identität und des Zugriffs](../security-center/security-center-identity-access.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4: Verwenden des einmaligen Anmeldens (SSO) in Azure Active Directory

**Leitfaden**: Verwenden Sie den Azure HDInsight-Identitätsbroker, um sich per mehrstufiger Authentifizierung bei ESP-Clustern (Enterprise-Sicherheitspaket) anzumelden, ohne Kennwörter angeben zu müssen. Wenn Sie bereits bei anderen Azure-Diensten angemeldet sind, z. B. beim Azure-Portal, können Sie sich mittels einmaligem Anmelden (SSO) bei Ihrem Azure HDInsight-Cluster anmelden.

- [Aktivieren des Azure HDInsight-Identitätsbrokers](https://docs.microsoft.com/azure/hdinsight/domain-joined/identity-broker#enable-hdinsight-id-broker)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Verwenden der mehrstufigen Authentifizierung für den gesamten Azure Active Directory-basierten Zugriff

**Leitfaden**: Aktivieren Sie die mehrstufige Authentifizierung von Azure Active Directory (Azure AD), und befolgen Sie die Empfehlungen für die Identitäts- und Zugriffsverwaltung in Azure Security Center. Azure HDInsight-Cluster mit konfiguriertem Enterprise-Sicherheitspaket sind mit einer Domäne verbunden, sodass Domänenbenutzer ihre Domänenanmeldeinformationen zum Authentifizieren beim Cluster verwenden und Big Data-Aufträge ausführen können. Bei der Authentifizierung mit aktivierter Multi-Factor Authentication (MFA) werden die Benutzer aufgefordert, einen zweiten Authentifizierungsfaktor anzugeben.

- [Planen einer Bereitstellung von Azure AD Multi-Factor Authentication](../active-directory/authentication/howto-mfa-getstarted.md)

- [Überwachen von Identität und Zugriff in Azure Security Center](../security-center/security-center-identity-access.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Verwenden dedizierter Computer (Arbeitsstationen mit privilegiertem Zugriff) für alle administrativen Aufgaben

**Leitfaden**: Verwenden Sie PAWs (Privileged Access Workstations, Arbeitsstationen mit privilegiertem Zugriff) mit mehrstufiger Authentifizierung, die für die Anmeldung und Konfiguration Ihrer Azure HDInsight-Cluster und der zugehörigen Ressourcen konfiguriert sind.

- [Informationen zu Arbeitsstationen mit privilegiertem Zugriff](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Planen einer Bereitstellung von Azure AD Multi-Factor Authentication](../active-directory/authentication/howto-mfa-getstarted.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Protokollieren von und Warnen bei verdächtigen Aktivitäten in Administratorkonten

**Leitfaden**: Azure HDInsight-Cluster mit konfiguriertem Enterprise-Sicherheitspaket sind mit einer Domäne verbunden, damit Domänenbenutzer ihre Domänenanmeldeinformationen zum Authentifizieren verwenden können. Sie können Azure AD-Sicherheitsberichte für die Generierung von Protokollen und Warnungen bei verdächtigen oder sicherheitsrelevanten Aktivitäten in der Azure AD-Umgebung verwenden. Verwenden Sie Azure Security Center zum Überwachen von identitäts- und zugriffsbezogenen Aktivitäten.

- [Identifizieren von Azure AD-Benutzern, die aufgrund riskanter Aktivitäten gekennzeichnet wurden](/azure/active-directory/reports-monitoring/concept-user-at-risk)

- [Überwachen der identitäts- und zugriffsbezogenen Aktivitäten von Benutzern in Azure Security Center](../security-center/security-center-identity-access.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Verwalten von Azure-Ressourcen nur über genehmigte Standorte

**Leitfaden**: Azure HDInsight-Cluster mit konfiguriertem Enterprise-Sicherheitspaket sind mit einer Domäne verbunden, damit Domänenbenutzer ihre Domänenanmeldeinformationen zum Authentifizieren verwenden können. Verwenden Sie benannte Standorte mit bedingtem Zugriff, um den Zugriff nur über bestimmte logische Gruppierungen von IP-Adressbereichen oder Ländern/Regionen zuzulassen.

- [Konfigurieren benannter Standorte in Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="39-use-azure-active-directory"></a>3.9: Verwenden von Azure Active Directory

**Leitfaden**: Verwenden Sie Azure Active Directory (Azure AD) als zentrales Authentifizierungs- und Autorisierungssystem. Azure AD schützt Daten durch eine starke Verschlüsselung für ruhende und übertragene Daten. Außerdem werden in Azure AD Salts und Hashs verwendet, und Anmeldeinformationen werden sicher gespeichert.

Azure HDInsight-Cluster mit konfiguriertem Enterprise-Sicherheitspaket (ESP) sind mit einer Domäne verbunden, damit Domänenbenutzer ihre Domänenanmeldeinformationen zum Authentifizieren für die Cluster verwenden können.

- [Erstellen und Konfigurieren einer Azure AD-Instanz](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

- [Konfigurieren des Enterprise-Sicherheitspakets mit Azure AD Domain Services in Azure HDInsight](domain-joined/apache-domain-joined-configure-using-azure-adds.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Regelmäßiges Überprüfen und Abstimmen des Benutzerzugriffs

**Leitfaden**: Verwenden Sie die Azure AD-Authentifizierung mit Ihrem Azure HDInsight-Cluster. Azure AD enthält Protokolle zum Ermitteln von veralteten Konten. Verwenden Sie zusätzlich Zugriffsüberprüfungen für Azure-Identitäten, um Gruppenmitgliedschaften, den Zugriff auf Unternehmensanwendungen und Rollenzuweisungen effizient zu verwalten. Der Benutzerzugriff kann regelmäßig überprüft werden, um sicherzustellen, dass nur die richtigen Benutzer weiterhin Zugriff besitzen.

- [Verwenden von Zugriffsüberprüfungen für Azure-Identitäten](../active-directory/governance/access-reviews-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Überwachen von Zugriffsversuchen auf deaktivierte Anmeldeinformationen

**Leitfaden**: Verwenden Sie Azure AD-Anmelde- und Überwachungsprotokolle, um zu überwachen, ob versucht wird, auf deaktivierte Konten zuzugreifen. Diese Protokolle können in alle SIEM-/Überwachungstools von Drittanbietern integriert werden.

Sie können diesen Prozess optimieren, indem Sie Diagnoseeinstellungen für Azure AD-Benutzerkonten erstellen und die Überwachungs- und Anmeldeprotokolle an einen Azure Log Analytics-Arbeitsbereich senden. Konfigurieren Sie die gewünschten Warnungen im Log Analytics-Arbeitsbereich.

- [Integrieren von Azure-Aktivitätsprotokollen in Azure Monitor](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12: Warnung bei abweichendem Verhalten bei der Kontoanmeldung

**Leitfaden**: Azure HDInsight-Cluster mit konfiguriertem Enterprise-Sicherheitspaket (ESP) sind mit einer Domäne verbunden, damit Domänenbenutzer ihre Domänenanmeldeinformationen zum Authentifizieren für die Cluster verwenden können. Mit den Funktionen zur Risikoerkennung und zum Identitätsschutz von Azure Active Directory (Azure AD) können Sie automatische Antworten auf erkannte verdächtige Aktionen im Zusammenhang mit Benutzeridentitäten konfigurieren. Außerdem können Sie Daten zur weiteren Untersuchung in Azure Sentinel erfassen.

- [Anzeigen riskanter Azure AD-Anmeldungen](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Konfigurieren und Aktivieren von Risikorichtlinien für den Identitätsschutz](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Ermöglichen des Zugriffs auf relevante Kundendaten für Microsoft in Supportszenarien

**Leitfaden**: Nicht verfügbar. Kunden-Lockbox wird für Azure HDInsight noch nicht unterstützt.

- [Unterstützte Dienste und Szenarios bei allgemeiner Verfügbarkeit](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="data-protection"></a>Datenschutz

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Schutz von Daten](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Verwalten eines Bestands an vertraulichen Informationen

**Leitfaden**: Verwenden Sie Tags für Ressourcen Ihrer Azure HDInsight-Bereitstellungen als Hilfe beim Nachverfolgen von Azure-Ressourcen, die vertrauliche Informationen speichern oder verarbeiten.

- [Erstellen und Verwenden von Tags](/azure/azure-resource-manager/resource-group-using-tags)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Isolieren von Systemen, die vertrauliche Informationen speichern oder verarbeiten

**Leitfaden**: Implementieren Sie separate Abonnements und/oder Verwaltungsgruppen für Entwicklungs-, Test- und Produktionsumgebungen. Azure HDInsight-Cluster und alle zugeordneten Speicherkonten sollten durch ein virtuelles Netzwerk bzw. Subnetz getrennt, entsprechend mit Tags versehen und in einer Netzwerksicherheitsgruppe (NSG) oder per Azure Firewall geschützt werden. Clusterdaten sollten in einem geschützten Azure Storage-Konto oder in Azure Data Lake Storage (Gen1 oder Gen2) enthalten sein.

- [Auswählen von Speicheroptionen für Ihren Azure HDInsight-Cluster](hdinsight-hadoop-compare-storage-options.md)

- [Schützen von Azure Data Lake Storage](../data-lake-store/data-lake-store-security-overview.md)

- [Schützen von Azure Storage-Konten](/azure/storage/common/storage-security-guide)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Überwachen und Blockieren einer nicht autorisierten Übertragung vertraulicher Informationen

**Leitfaden**: Kennzeichnen Sie den Cluster und die zugehörigen Ressourcen mit Tags als vertraulich, wenn mit Azure HDInsight-Clustern vertrauliche Informationen gespeichert oder verarbeitet werden. Beschränken Sie Netzwerkdatenverkehr in ausgehender Richtung für Azure HDInsight-Cluster per Azure Firewall, um das Risiko für Datenverlust aufgrund einer Exfiltration zu verringern.

Für die zugrundeliegende Plattform, die von Microsoft verwaltet wird, behandelt Microsoft alle Kundeninhalte als vertraulich und unternimmt große Anstrengungen, um Kundendaten vor Verlust und Gefährdung zu schützen. Um die Sicherheit von Kundendaten innerhalb von Azure zu gewährleisten, hat Microsoft eine Reihe von robusten Datenschutzkontrollen und -funktionen implementiert und kümmert sich um deren Verwaltung.

- [Einschränken des ausgehenden Datenverkehrs für Azure HDInsight-Cluster per Azure Firewall](hdinsight-restrict-outbound-traffic.md)

- [Grundlegendes zum Schutz von Kundendaten in Azure](../security/fundamentals/protection-customer-data.md)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Verschlüsseln aller vertraulichen Informationen während der Übertragung

**Leitfaden**: Verschlüsseln Sie alle vertraulichen Informationen während der Übertragung. Stellen Sie sicher, dass alle Clients, die eine Verbindung mit Ihrem Azure HDInsight-Cluster oder mit Clusterdatenspeichern (Azure Storage-Konten oder Azure Data Lake Storage Gen1/Gen2) herstellen, die Aushandlung mit TLS 1.2 oder höher durchführen können. Für Microsoft Azure-Ressourcen wird standardmäßig die TLS 1.2-Aushandlung durchgeführt. 

- [Grundlegendes zur Azure Data Lake Storage-Verschlüsselung während der Übertragung](../data-lake-store/data-lake-store-security-overview.md)

- [Grundlegendes zur Azure Storage-Kontoverschlüsselung während der Übertragung](../storage/blobs/security-recommendations.md)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Verwenden eines aktiven Ermittlungstools zur Bestimmung vertraulicher Daten

**Leitfaden**: Funktionen für Datenidentifizierung, -klassifizierung und -verlust sind für Azure Storage- oder Computeressourcen noch nicht verfügbar. Implementieren Sie eine Drittanbieterlösung, wenn dies für Compliancezwecke erforderlich ist.

Für die zugrundeliegende Plattform, die von Microsoft verwaltet wird, behandelt Microsoft alle Kundeninhalte als vertraulich und unternimmt große Anstrengungen, um Kundendaten vor Verlust und Gefährdung zu schützen. Um die Sicherheit von Kundendaten innerhalb von Azure zu gewährleisten, hat Microsoft eine Reihe von robusten Datenschutzkontrollen und -funktionen implementiert und kümmert sich um deren Verwaltung.

- [Grundlegendes zum Schutz von Kundendaten in Azure](../security/fundamentals/protection-customer-data.md)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Verwenden von Azure RBAC zum Steuern des Zugriffs auf Ressourcen 

**Leitfaden**: Mit dem Enterprise-Sicherheitspaket (ESP) von Azure HDInsight können Sie Apache Ranger verwenden, um Richtlinien für eine präzise Zugriffssteuerung und Obfuskation Ihrer Daten, die in Dateien, Ordnern, Datenbanken, Tabellen und Zeilen bzw. Spalten gespeichert sind, zu erstellen und zu verwalten. Der Hadoop-Administrator kann die rollenbasierte Zugriffssteuerung (RBAC) konfigurieren, um Apache Hive, HBase, Kafka und Spark mit diesen Plug-Ins in Apache Ranger zu schützen.

Durch die Konfiguration von RBAC-Richtlinien mit Apache Ranger können Sie Berechtigungen einer Rolle in der Organisation zuordnen. Diese Abstraktionsebene erleichtert die Sicherstellung, dass Personen nur über die Berechtigungen verfügen, die sie zur Erfüllung ihrer beruflichen Aufgaben benötigen.

- [Konfigurationen des Enterprise-Sicherheitspakets mit Azure Active Directory Domain Services in Azure HDInsight](domain-joined/apache-domain-joined-configure-using-azure-adds.md)

- [Übersicht über die Unternehmenssicherheit in Azure HDInsight](domain-joined/hdinsight-security-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Verwenden der hostbasierten Verhinderung von Datenverlusten zum Erzwingen der Zugriffssteuerung

**Leitfaden**: Kennzeichnen Sie den Cluster und die zugehörigen Ressourcen mit Tags als vertraulich, wenn mit Azure HDInsight-Clustern vertrauliche Informationen gespeichert oder verarbeitet werden. Funktionen für Datenidentifizierung, -klassifizierung und -verlust sind für Azure Storage- oder Computeressourcen noch nicht verfügbar. Implementieren Sie eine Drittanbieterlösung, wenn dies für Compliancezwecke erforderlich ist.

Für die zugrundeliegende Plattform, die von Microsoft verwaltet wird, behandelt Microsoft alle Kundeninhalte als vertraulich und unternimmt große Anstrengungen, um Kundendaten vor Verlust und Gefährdung zu schützen. Um die Sicherheit von Kundendaten innerhalb von Azure zu gewährleisten, hat Microsoft eine Reihe von robusten Datenschutzkontrollen und -funktionen implementiert und kümmert sich um deren Verwaltung.

- [Grundlegendes zum Schutz von Kundendaten in Azure](../security/fundamentals/protection-customer-data.md)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Verschlüsseln vertraulicher, ruhender Informationen

**Leitfaden**: Stellen Sie bei der Verwendung von Azure SQL-Datenbank zum Speichern von Apache Hive- und Apache Oozie-Metadaten sicher, dass die SQL-Daten jederzeit verschlüsselt sind. Für Azure Storage-Konten und Data Lake Storage (Gen1 oder Gen2) empfehlen wir Ihnen, Microsoft die Verwaltung Ihrer Verschlüsselungsschlüssel zu überlassen. Sie haben aber auch die Möglichkeit, Ihre Schlüssel selbst zu verwalten.

- [Verwalten von Verschlüsselungsschlüsseln für Azure Storage-Konten](/azure/storage/common/storage-encryption-keys-portal)

- [Erstellen von Azure Data Lake Storage mit vom Kunden verwalteten Verschlüsselungsschlüsseln](../data-lake-store/data-lake-store-get-started-portal.md)

- [Grundlegendes zur Verschlüsselung für Azure SQL-Datenbank](/azure/sql-database/sql-database-technical-overview#data-encryption)

- [Konfigurieren von Transparent Data Encryption für SQL-Datenbank mit vom Kunden verwalteten Schlüsseln](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql?tabs=azure-portal)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Protokollieren und Warnen bei Änderungen an wichtigen Azure-Ressourcen

**Leitfaden**: Konfigurieren Sie Diagnoseeinstellungen für Azure Storage-Konten, die Azure HDInsight-Clustern zugeordnet sind, um alle CRUD-Vorgänge für Clusterdaten zu überwachen und zu protokollieren. Aktivieren Sie die Überwachung für alle Storage-Konten oder Data Lake Stores, die dem Azure HDInsight-Cluster zugeordnet sind.

- [Aktivieren zusätzlicher Protokollierung/Überwachung für ein Azure Storage-Konto](/azure/storage/common/storage-monitor-storage-account)

- [Aktivieren zusätzlicher Protokollierung/Überwachung für Azure Data Lake Storage](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="vulnerability-management"></a>Verwaltung von Sicherheitsrisiken

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Verwaltung von Sicherheitsrisiken](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Ausführen automatisierter Scan-Tools für Sicherheitsrisiken

**Leitfaden**: Implementieren Sie eine Drittanbieterlösung zur Verwaltung von Sicherheitsrisiken.

Wenn Sie über ein Rapid7-, Qualys- oder anderes Abonnement einer Plattform für die Sicherheitsrisikoverwaltung verfügen, können Sie Skriptaktionen verwenden, um Agents für die Sicherheitsrisikobewertung auf Ihren Azure HDInsight-Clusterknoten zu installieren, und die Knoten über das entsprechende Portal verwalten.

- [Manuelles Installieren des Rapid7-Agents](https://insightvm.help.rapid7.com/docs/install)

- [Manuelles Installieren des Qualys-Agents](https://www.qualys.com/docs/qualys-cloud-agent-linux-install-guide.pdf)

- [Verwenden von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Bereitstellen der automatisierten Lösung für die Patchverwaltung von Betriebssystemen

**Leitfaden**: Für Clusterknotenimages wurden automatische Systemupdates aktiviert. Sie müssen die Clusterknoten aber regelmäßig neu starten, um sicherzustellen, dass die Updates angewendet werden.

Microsoft verwaltet und aktualisiert die Basisknotenimages für Azure HDInsight.

- [Konfigurieren des Zeitplans für das Patchen des Betriebssystems für HDInsight-Cluster](hdinsight-os-patching.md)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5.3: Bereitstellen einer automatisierten Patchverwaltungslösung für Softwaretitel von Drittanbietern

**Leitfaden**: Verwenden Sie Skriptaktionen oder andere Mechanismen, um Ihre Azure HDInsight-Cluster zu patchen. Neu erstellte Cluster verfügen immer über die aktuellen verfügbaren Updates, einschließlich der neuesten Sicherheitspatches.

- [Konfigurieren des Zeitplans für das Patchen des Betriebssystems für Linux-basierte Azure HDInsight-Cluster](hdinsight-os-patching.md)

- [Verwenden von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4 Vergleichen von kaskadierenden Sicherheitsrisikoscans

**Leitfaden**: Implementieren Sie eine Drittanbieterlösung für die Verwaltung von Sicherheitsrisiken, mit der Überprüfungen auf Sicherheitsrisiken zeitabhängig verglichen werden können. Falls Sie über ein Rapid7- oder Qualys-Abonnement verfügen, können Sie das Portal des Anbieters nutzen, um aufeinanderfolgende Überprüfungen auf Sicherheitsrisiken anzuzeigen und zu vergleichen.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Verwenden eines Risikobewertungsprozesses, um die Behebung von erkannten Sicherheitsrisiken zu priorisieren

**Leitfaden**: Verwenden Sie ein gängiges Risikobewertungsprogramm (z. B. Common Vulnerability Scoring System) oder die von Ihrem Scantool eines Drittanbieters bereitgestellten Standardrisikobewertungen.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="inventory-and-asset-management"></a>Bestands- und Ressourcenverwaltung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Bestands- und Ressourcenverwaltung](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Verwenden der automatisierten Asset Discovery-Lösung

**Leitfaden**: Verwenden Sie Azure Resource Graph, um alle Ressourcen (z. B. Computeressourcen, Speicher, Netzwerke, Ports, Protokolle usw.), einschließlich Azure HDInsight-Cluster, in Ihren Abonnements abzufragen und zu ermitteln. Stellen Sie sicher, dass Sie über entsprechende (Lese-)Berechtigungen in Ihrem Mandanten verfügen und alle Azure-Abonnements und Ressourcen in Ihren Abonnements auflisten können.

Obwohl klassische Azure-Ressourcen über Azure Resource Graph ermittelt werden können, wird dringend empfohlen, Azure Resource Manager-Ressourcen zu erstellen und zu verwenden.

- [Erstellen von Abfragen mit Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

- [Anzeigen Ihrer Azure-Abonnements](/powershell/module/az.accounts/get-azsubscription)

- [Grundlegendes zu Azure RBAC](../role-based-access-control/overview.md)

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

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4: Definieren und Verwalten des Bestands an genehmigten Azure-Ressourcen

**Leitfaden**: Definieren Sie eine Liste mit den genehmigten Azure-Ressourcen und der genehmigten Software für Computeressourcen.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Überwachung auf nicht genehmigte Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy, um Einschränkungen für den Typ der Ressourcen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie dazu die folgenden integrierten Richtliniendefinitionen:

- Not allowed resource types (Unzulässige Ressourcentypen)

- Zulässige Ressourcentypen

Verwenden Sie Azure Resource Graph, um Ressourcen in Ihren Abonnements abzufragen und zu ermitteln. Stellen Sie sicher, dass alle in der Umgebung vorhandenen Azure-Ressourcen genehmigt sind.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Erstellen von Abfragen mit Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Überwachen auf nicht genehmigte Softwareanwendungen innerhalb von Computeressourcen

**Leitfaden**: Implementieren Sie eine Drittanbieterlösung, um Clusterknoten auf nicht genehmigte Softwareanwendungen zu überwachen.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Entfernen nicht genehmigter Azure-Ressourcen und Softwareanwendungen

**Leitfaden**: Verwenden Sie Azure Resource Graph, um alle Ressourcen (z. B. Computeressourcen, Speicher, Netzwerke, Ports, Protokolle usw.), einschließlich Azure HDInsight-Cluster, in Ihren Abonnements abzufragen und zu ermitteln.  Entfernen Sie alle nicht genehmigten Azure-Ressourcen, die Sie finden. Implementieren Sie für Azure HDInsight-Cluster eine Drittanbieterlösung, um nicht genehmigte Software zu entfernen bzw. Warnungen dazu anzuzeigen.

- [Erstellen von Abfragen mit Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="68-use-only-approved-applications"></a>6.8: Ausschließliche Verwendung genehmigter Anwendungen

**Leitfaden**: Implementieren Sie für HDInsight-Clusterknoten eine Drittanbieterlösung, um zu verhindern, dass nicht autorisierte Software ausgeführt wird.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="69-use-only-approved-azure-services"></a>6.9: Ausschließliche Verwendung genehmigter Azure-Dienste

**Leitfaden**: Verwenden Sie Azure Policy, um Einschränkungen für den Typ der Ressourcen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie dazu die folgenden integrierten Richtliniendefinitionen:
- Not allowed resource types (Unzulässige Ressourcentypen)

- Zulässige Ressourcentypen

Weitere Informationen finden Sie in den folgenden Referenzen:

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Ablehnen eines bestimmten Ressourcentyps mit Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#general)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6.10: Verwalten eines Bestands an genehmigten Softwaretiteln

**Leitfaden**: Implementieren Sie für HDInsight-Clusterknoten eine Drittanbieterlösung, um zu verhindern, dass Dateien eines nicht autorisierten Typs ausgeführt werden.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Einschränken der Möglichkeiten von Benutzern zur Interaktion mit Azure Resource Manager

**Leitfaden**: Verwenden Sie den bedingten Azure-Zugriff, um die Möglichkeiten der Benutzer zur Interaktion mit Azure Resource Manager einzuschränken, indem Sie „Zugriff blockieren“ für die App zur „Verwaltung von Microsoft Azure“ konfigurieren.

- [Verwalten des Zugriffs auf die Azure-Verwaltung mit bedingtem Zugriff](../role-based-access-control/conditional-access-azure-management.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="secure-configuration"></a>Sichere Konfiguration

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Sichere Konfiguration](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Einrichten sicherer Konfigurationen für alle Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy-Aliase im Namespace „Microsoft.HDInsight“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen der Netzwerkkonfiguration Ihres HDInsight-Clusters zu erstellen.

- [Anzeigen verfügbarer Azure Policy-Aliase](/powershell/module/az.resources/get-azpolicyalias)

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: Einrichten sicherer Betriebssystemkonfigurationen

**Leitfaden**: Azure HDInsight-Betriebssystemimages werden von Microsoft verwaltet und gewartet. Der Kunde ist verantwortlich für die Implementierung sicherer Konfigurationen für das Betriebssystem Ihrer Clusterknoten.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Verwalten von sicheren Konfigurationen für Azure-Ressourcen

**Leitfaden**: Verwenden Sie die Azure Policy-Richtlinien „[deny]“ (Verweigern) und „[deploy if not exist]“ (Bereitstellen, falls nicht vorhanden), um sichere Einstellungen für Ihre Azure HDInsight-Cluster und die zugehörigen Ressourcen zu erzwingen.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Grundlegendes zu Azure Policy-Auswirkungen](../governance/policy/concepts/effects.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Verwalten sicherer Betriebssystemkonfigurationen

**Leitfaden**: Azure HDInsight-Betriebssystemimages werden von Microsoft verwaltet und gewartet. Der Kunde ist verantwortlich für die Implementierung der Zustandskonfiguration auf Betriebssystemebene.

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Sicheres Speichern der Konfiguration von Azure-Ressourcen

**Leitfaden**: Wenn Sie benutzerdefinierte Azure Policy-Definitionen verwenden, nutzen Sie Azure DevOps oder Azure Repos, um Ihren Code sicher zu speichern und zu verwalten.

- [Git-Tutorial zu Azure Repos](/azure/devops/repos/git/gitworkflow)

- [Dokumentation zu Azure Repos](/azure/devops/repos/index)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Bereitstellen von Konfigurationsverwaltungstools für Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy-Aliase im Namespace „Microsoft.HDInsight“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen von Systemkonfigurationen zu erstellen und Warnungen dazu zu erhalten. Entwickeln Sie außerdem einen Prozess und eine Pipeline zum Verwalten von Richtlinienausnahmen.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7.8: Bereitstellen von Konfigurationsverwaltungstools für Betriebssysteme

**Leitfaden**: Implementieren Sie eine Drittanbieterlösung, um den gewünschten Zustand für die Betriebssysteme Ihrer Clusterknoten zu verwalten.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementieren der automatisierten Konfigurationsüberwachung für Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy-Aliase im Namespace „Microsoft.HDInsight“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen der Konfiguration Ihres HDInsight-Clusters zu erstellen.

- [Anzeigen verfügbarer Azure Policy-Aliase](/powershell/module/az.resources/get-azpolicyalias)

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: Implementieren der automatisierten Konfigurationsüberwachung für Betriebssysteme

**Leitfaden**: Implementieren Sie eine Drittanbieterlösung, um den Zustand der Betriebssysteme Ihrer Clusterknoten zu überwachen.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="711-manage-azure-secrets-securely"></a>7.11: Sicheres Verwalten von Azure-Geheimnissen

**Leitfaden**: Azure HDInsight beinhaltet BYOK-Unterstützung (Bring Your Own Key) für Apache Kafka. Dank dieser Funktion können Sie die Verantwortung und die Verwaltung für die Schlüssel übernehmen, die zum Verschlüsseln ruhender Daten verwendet werden.

Alle verwalteten Datenträger in Azure HDInsight werden mit der Speicherdienstverschlüsselung (Storage Service Encryption, SSE) von Azure geschützt. Die Daten auf diesen Datenträgern werden standardmäßig mit von Microsoft verwalteten Schlüsseln verschlüsselt. Bei der Aktivierung von BYOK geben Sie den Verschlüsselungsschlüssel für Azure HDInsight für die Verwendung und Verwaltung mit Azure Key Vault an.

Key Vault kann auch mit Azure HDInsight-Bereitstellungen verwendet werden, um Schlüssel für Clusterspeicher (Azure Storage-Konten und Azure Data Lake Storage) zu verwalten.

- [Verwenden von „Bring Your Own Key“ für Apache Kafka in Azure HDInsight](/azure/hdinsight/kafka/apache-kafka-byok)

- [Verwalten von Verschlüsselungsschlüsseln für Azure Storage-Konten](/azure/storage/common/storage-encryption-keys-portal)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Sicheres und automatisches Verwalten von Identitäten

**Leitfaden**: Verwaltete Identitäten können in Azure HDInsight verwendet werden, um Ihren Clustern den Zugriff auf Azure Active Directory Domain Services, Azure Key Vault oder Dateien in Azure Data Lake Storage Gen2 zu ermöglichen.

- [Grundlegendes zu verwalteten Identitäten mit Azure HDInsight](hdinsight-managed-identities.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Beheben der unbeabsichtigten Offenlegung von Anmeldeinformationen

**Leitfaden**: Bei Verwendung von Code zu Ihrer Azure HDInsight-Bereitstellung können Sie Credential Scanner implementieren, um Anmeldeinformationen im Code zu identifizieren. In Credential Scanner wird auch das Verschieben von ermittelten Anmeldeinformationen an sicherere Speicherorte (z. B. Azure Key Vault) empfohlen. 

- [Einrichten von Credential Scanner](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="malware-defense"></a>Schutz vor Schadsoftware

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Schutz vor Schadsoftware](../security/benchmarks/security-control-malware-defense.md).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1: Verwenden einer zentral verwalteten Antischadsoftware

**Leitfaden**: Bei Azure HDInsight ist Clamscan vorinstalliert und für die Clusterknotenimages aktiviert, aber Sie müssen die Software verwalten und alle von Clamscan erzeugten Protokolle manuell aggregieren bzw. überwachen.

- [Grundlegendes zu Clamscan für Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-faq#security-and-certificates)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: Vorabprüfen von in computefremde Azure-Ressourcen hochzuladenden Dateien

**Leitfaden**: Microsoft Antimalware ist auf dem zugrunde liegenden Host für die Azure-Dienste aktiviert, wird aber nicht für Kundeninhalte ausgeführt.

Führen Sie für alle Dateien, die auf Azure-Ressourcen Ihrer Azure HDInsight-Clusterbereitstellung hochgeladen werden, z. B. Data Lake Storage, Blob Storage usw., eine Vorabprüfung durch. Microsoft kann auf diesen Instanzen nicht auf Kundendaten zugreifen.

- [Grundlegendes zu Microsoft Antimalware für Azure Cloud Services und Virtual Machines](../security/fundamentals/antimalware.md)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>Schritt 8.3: Sicherstellen der Aktualisierung von Antischadsoftware und Signaturen

**Leitfaden**: Bei Azure HDInsight ist Clamscan vorinstalliert und für die Clusterknotenimages aktiviert. Clamscan führt automatische Updates für die Engine und die Definitionen durch, aber die Aggregation und Verwaltung der Protokolle muss manuell erfolgen.

- [Grundlegendes zu Clamscan für Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-faq#security-and-certificates)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="data-recovery"></a>Datenwiederherstellung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Datenwiederherstellung](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Sicherstellen regelmäßiger automatisierter Sicherungen

**Leitfaden**: Wählen Sie die entsprechende Redundanzoption (LRS, ZRS, GRS, RA-GRS) aus, wenn Sie ein Azure Storage-Konto für den HDInsight-Clusterdatenspeicher verwenden.  Konfigurieren Sie bei Verwendung einer Azure SQL-Datenbankinstanz für den Azure HDInsight-Clusterdatenspeicher die aktive Georeplikation.

- [Konfigurieren der Speicherredundanz für Azure Storage-Konten](../storage/common/storage-redundancy.md)

- [Konfigurieren von Redundanz für Azure SQL-Datenbank](/azure/sql-database/sql-database-active-geo-replication)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Durchführen vollständiger Systemsicherungen und Sichern aller kundenseitig verwalteten Schlüssel

**Leitfaden**: Wählen Sie die entsprechende Redundanzoption (LRS, ZRS, GRS, RA-GRS) aus, wenn Sie ein Azure Storage-Konto für den Azure HDInsight-Clusterdatenspeicher verwenden. Wenn Sie Azure Key Vault für einen Teil Ihrer Azure HDInsight-Bereitstellung verwenden, sollten Sie sicherstellen, dass Ihre Schlüssel gesichert werden.

- [Auswählen von Speicheroptionen für Ihren Azure HDInsight-Cluster](hdinsight-hadoop-compare-storage-options.md)

- [Konfigurieren der Speicherredundanz für Azure Storage-Konten](../storage/common/storage-redundancy.md)

- [Sichern von Schlüsseltresorschlüsseln in Azure](/powershell/module/az.keyvault/backup-azkeyvaultkey)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Überprüfen aller Sicherungen einschließlich kundenseitig verwalteter Schlüssel

**Leitfaden**: Wenn Azure Key Vault für Ihre Azure HDInsight-Bereitstellung verwendet wird, testen Sie die Wiederherstellung der gesicherten vom Kunden verwalteten Schlüssel.

- [Verwenden von „Bring Your Own Key“ für Apache Kafka in Azure HDInsight](/azure/hdinsight/kafka/apache-kafka-byok)

- [Wiederherstellen von Schlüsseltresorschlüsseln in Azure](/powershell/module/az.keyvault/restore-azkeyvaultkey)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Sicherstellen des Schutzes von Sicherungen und von kundenseitig verwalteten Schlüsseln

**Leitfaden**: Wenn Azure Key Vault für Ihre Azure HDInsight-Bereitstellung verwendet wird, aktivieren Sie das vorläufige Löschen in Key Vault, um Schlüssel vor versehentlichen oder böswilligen Löschvorgängen zu schützen.

- [Aktivieren des vorläufigen Löschens in Azure Key Vault](/azure/key-vault/key-vault-ovw-soft-delete)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="incident-response"></a>Reaktion auf Vorfälle

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Reaktion auf Vorfälle](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Erstellen eines Leitfadens für die Reaktion auf Vorfälle

**Leitfaden**: Erarbeiten Sie einen Leitfaden für die Reaktion auf Vorfälle in Ihrer Organisation. Stellen Sie sicher, dass es schriftliche Pläne für die Reaktion auf Vorfälle gibt, in denen alle Rollen der Mitarbeiter sowie die Phasen der Bearbeitung und Verwaltung von Vorfällen von der Ermittlung bis zur abschließenden Überprüfung definiert sind. 

- [Anleitung zum Entwickeln eines Prozesses für die Reaktion auf Sicherheitsvorfälle](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/) 

- [Struktur eines Vorfalls laut Microsoft Security Response Center](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/) 

- [Nutzen des Computer Security Incident Handling Guide des US-amerikanischen National Institute of Standards and Technology (NIST) als Hilfe bei der Entwicklung eines eigenen Plans zur Reaktion auf Vorfälle](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Erstellen eines Verfahrens zur Bewertung und Priorisierung von Vorfällen

**Leitfaden**: Security Center weist Warnungen einen Schweregrad zu, um Ihnen zu helfen, die Reihenfolge zu priorisieren, in der Sie sich um Warnungen kümmern. So können Sie, sobald eine Ressource gefährdet ist, sofort zu ihr gelangen. Der Schweregrad basiert darauf, wie zuversichtlich Security Center in Bezug auf den Befund oder die Analyse ist, die zum Auslösen der Warnung verwendet wird, sowie auf dem Zuverlässigkeitsgrad, dass hinter der Aktivität, die zu der Warnung führte, eine böswillige Absicht stand.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="103-test-security-response-procedures"></a>10.3: Verfahren zum Testen der Reaktion auf Sicherheitsvorfälle

**Leitfaden**: Führen Sie in regelmäßigen Abständen Tests zur Reaktionsfähigkeit Ihrer Systeme auf Vorfälle durch. Identifizieren Sie Schwachstellen und Lücken, und überarbeiten Sie den Plan bei Bedarf.

- [„Guide to Test, Training, and Exercise Programs for IT Plans and Capabilities“ des National Institute of Standards and Technology (NIST)](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Angeben von Kontaktdaten für Sicherheitsvorfälle und Konfigurieren von Warnungsbenachrichtigungen für Sicherheitsvorfälle

**Leitfaden**: Microsoft wendet sich unter den für Sicherheitsvorfälle angegebenen Kontaktdaten an Sie, wenn das Microsoft Security Response Center (MSRC) feststellt, dass Personen unrechtmäßig oder unbefugt auf Ihre Daten zugegriffen haben.

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

- Sehen Sie sich die [Übersicht über Version 2 des Azure-Sicherheitsvergleichstests](/azure/security/benchmarks/overview) an.
- Erfahren Sie mehr über [Azure-Sicherheitsbaselines](/azure/security/benchmarks/security-baselines-overview).
