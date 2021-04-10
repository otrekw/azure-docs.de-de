---
title: Azure-Sicherheitsbaseline für Azure Backup
description: Die Sicherheitsbaseline für Azure Backup enthält Anweisungen zur Vorgehensweise und Ressourcen für die Implementierung der Sicherheitsempfehlungen im Azure-Sicherheitsvergleichstest.
author: msmbaldwin
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 0530baf5c198b5d82527cfb02c66765ec885bd94
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105566709"
---
# <a name="azure-security-baseline-for-azure-backup"></a>Azure-Sicherheitsbaseline für Azure Backup

Diese Sicherheitsbaseline wendet Empfehlungen des [Azure-Sicherheitsvergleichstests Version 1.0](../security/benchmarks/overview-v1.md) auf Azure Backup an. Der Azure-Sicherheitsvergleichstest enthält Empfehlungen zum Schutz Ihrer Cloudlösungen in Azure.
Der Inhalt wird nach den **Sicherheitskontrollen** gruppiert, die durch den Azure-Sicherheitsvergleichstest und die entsprechenden für Azure Backup geltenden Empfehlungen definiert werden. Nicht auf Azure Backup anwendbare **Kontrollen** wurden ausgeschlossen.

 
Die vollständige Zuordnung von Azure Backup zum Azure-Sicherheitsvergleichstest finden Sie in der [vollständigen Zuordnungsdatei der Azure Backup-Sicherheitsbaseline](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Netzwerksicherheit

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Netzwerksicherheit](../security/benchmarks/security-control-network-security.md).*

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Bereitstellen von netzwerkbasierten Angriffserkennungs-/Eindringschutzsystemen (Intrusion Detection/Intrusion Prevention Systems, IDS/IPS)

**Leitfaden**: Die von Azure Backup verwendeten Endpunkte (einschließlich des Microsoft Azure Recovery Services-Agents) werden alle von Microsoft verwaltet. Sie sind für alle zusätzlichen Steuerfunktionen verantwortlich, die Sie für Ihre lokalen Systeme bereitstellen möchten.

- [Netzwerk und Zugriffsunterstützung](./backup-support-matrix-mars-agent.md#networking-and-access-support)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimieren der Komplexität und des Verwaltungsaufwands von Netzwerksicherheitsregeln

**Leitfaden**: Wenn Sie den MARS-Agent auf einem virtuellen Azure-Computer verwenden, verwenden Sie in ihrer Netzwerksicherheitsgruppe oder Azure Firewall das Diensttag „AzureBackup“, um den ausgehenden Zugriff auf Azure Backup zuzulassen.

- [Sichern von SQL Server-Datenbanken auf virtuellen Azure-Computern](backup-sql-server-database-azure-vms.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="110-document-traffic-configuration-rules"></a>1.10: Dokumentieren von Datenverkehrskonfigurationsregeln

**Leitfaden**: Wenn Sie den MARS-Agent auf einem virtuellen Azure-Computer verwenden, ordnen Sie diese VM einer Netzwerksicherheitsgruppe zu, und geben Sie in der Beschreibung die Geschäftsanforderungen für die Regel an.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Verwenden automatisierter Tools zum Überwachen von Netzwerkressourcenkonfigurationen und Erkennen von Änderungen

**Leitfaden**: Wenn Sie den MARS-Agent auf einem virtuellen Azure-Computer verwenden, der durch eine Netzwerksicherheitsgruppe (NSG) oder Azure Firewall geschützt wird, überwachen Sie die Konfiguration der NSG oder Firewall mit dem Azure-Aktivitätsprotokoll. Sie haben die Möglichkeit, in Azure Monitor Warnungen zu erstellen, die bei Änderungen an diesen Ressourcen ausgelöst werden.

- [Anzeigen und Abrufen von Azure-Aktivitätsprotokollereignissen](../azure-monitor/essentials/activity-log.md#view-the-activity-log)

- [Erstellen, Anzeigen und Verwalten von Aktivitätsprotokollwarnungen mit Azure Monitor](../azure-monitor/alerts/alerts-activity-log.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="logging-and-monitoring"></a>Protokollierung und Überwachung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Protokollierung und Überwachung](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Konfigurieren der zentralen Sicherheitsprotokollverwaltung

**Leitfaden**: Aktivieren Sie für die Überwachungsprotokollierung auf der Steuerungsebene die Diagnoseeinstellungen für das Azure-Aktivitätsprotokoll, und senden Sie die Protokolle zwecks Archivierung an einen Log Analytics-Arbeitsbereich, eine Azure Event Hubs-Instanz oder ein Azure-Speicherkonto. Mit den Daten aus Azure-Aktivitätsprotokollen können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge (PUT, POST, DELETE) ermitteln, die auf Steuerungsebene für Ihre Azure-Ressourcen ausgeführt werden.

Außerdem können Sie Protokolle über Azure Monitor erfassen, um die von Azure Backup generierten Sicherheitsdaten zu aggregieren. Verwenden Sie in Azure Monitor Log Analytics-Arbeitsbereiche zum Abfragen und Ausführen von Analysen, und verwenden Sie Azure Storage-Konten für langfristige Speicherung/Archivierung. Alternativ dazu können Sie auch Daten in Azure Sentinel oder eine Security Incident & Event Management-Lösung (SIEM) eines Drittanbieters integrieren.

- [Erfassen und Analysieren des Azure-Aktivitätsprotokolls in Azure Monitor](../azure-monitor/essentials/activity-log.md)

- [Verwenden von Diagnoseeinstellungen für Recovery Services-Tresore](backup-azure-diagnostic-events.md)

- [Durchführen des Onboardings für Azure Sentinel](../sentinel/quickstart-onboard.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Aktivieren der Überwachungsprotokollierung für Azure-Ressourcen

**Leitfaden**: Aktivieren Sie für die Überwachungsprotokollierung auf der Steuerungsebene die Diagnoseeinstellungen für das Azure-Aktivitätsprotokoll, und senden Sie die Protokolle zwecks Archivierung an einen Log Analytics-Arbeitsbereich, eine Azure Event Hubs-Instanz oder ein Azure-Speicherkonto. Mit den Daten aus Azure-Aktivitätsprotokollen können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge (PUT, POST, DELETE) ermitteln, die auf Steuerungsebene für Ihre Azure-Ressourcen ausgeführt werden.

Außerdem sendet Azure Backup Diagnoseereignisse, die gesammelt und dann für die Analyse, Warnung und Berichterstellung verwendet werden können. Diagnoseereignisse für Recovery Services-Tresore lassen sich im Azure-Portal konfigurieren. Sie können ein oder mehrere Diagnoseereignisse an ein Speicherkonto, eine Event Hubs-Instanz oder einen Log Analytics-Arbeitsbereich senden.

- [Erfassen und Analysieren des Azure-Aktivitätsprotokolls in Azure Monitor](../azure-monitor/essentials/activity-log.md)

- [Verwenden von Diagnoseeinstellungen für Recovery Services-Tresore](backup-azure-diagnostic-events.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="25-configure-security-log-storage-retention"></a>2.5: Konfigurieren der Sicherheitsprotokoll-Aufbewahrungsdauer im Speicher

**Leitfaden**: Legen Sie in Azure Monitor den Aufbewahrungszeitraum für Protokolle für Log Analytics-Arbeitsbereiche, die Ihren Azure Recovery Services-Tresoren zugeordnet sind, gemäß den Compliancevorschriften Ihrer Organisation fest.

- [Ändern des Datenaufbewahrungszeitraums](../azure-monitor/logs/manage-cost-storage.md#change-the-data-retention-period)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="26-monitor-and-review-logs"></a>2.6: Überwachen und Überprüfen von Protokollen

**Leitfaden**: Azure Backup stellt integrierte Überwachungs-und Warnungsfunktionen in einem Recovery Services-Tresor bereit. Diese Funktionen sind ohne zusätzliche Verwaltungsinfrastruktur verfügbar. Sie können den Umfang Ihrer Überwachung und Berichterstellung auch mit Azure Monitor erweitern.

Aktivieren Sie die Diagnoseeinstellungen für das Azure-Aktivitätsprotokoll, und senden Sie die Protokolle an einen Log Analytics-Arbeitsbereich. Außerdem können Sie Abfragen in Log Analytics durchführen, um Suchbegriffe zu ermitteln, Trends auszumachen, Muster zu analysieren und viele weitere Erkenntnisse auf Grundlage der Aktivitätsprotokolldaten zu gewinnen, die möglicherweise für Recovery Services-Tresore gesammelt wurden.

- [Überwachen von Azure Backup-Workloads](backup-azure-monitoring-built-in-monitor.md)

- [Erfassen und Analysieren des Azure-Aktivitätsprotokolls in Azure Monitor](../azure-monitor/essentials/activity-log.md)

- [Erfassen und Analysieren des Azure-Aktivitätsprotokolls in Azure Monitor](../azure-monitor/essentials/activity-log.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Aktivieren von Warnungen bei anomalen Aktivitäten

**Leitfaden**: Azure Backup stellt integrierte Überwachungs-und Warnungsfunktionen in einem Recovery Services-Tresor bereit. Diese Funktionen sind ohne zusätzliche Verwaltungsinfrastruktur verfügbar. Sie können den Umfang Ihrer Überwachung und Berichterstellung auch mit Azure Monitor erweitern.

Bei Warnungen handelt es sich in erster Linie um Szenarios, in denen Benutzer benachrichtigt werden, damit Sie entsprechende Maßnahmen ergreifen können. Im Abschnitt zu Sicherungswarnungen finden Sie die in Azure Backup generierten Warnungen. Diese Warnungen werden vom Dienst definiert, und Sie können keine benutzerdefinierten Warnungen erstellen.

Integrieren Sie einen Log Analytics-Arbeitsbereich in Azure Sentinel, eine Lösung für die Sicherheitsorchestrierung mit automatisierter Reaktion (Security Orchestration Automated Response, SOAR). Dadurch können Playbooks (automatisierte Lösungen) erstellt und zum Beheben von Sicherheitsproblemen verwendet werden. Darüber hinaus können Sie benutzerdefinierte Protokollwarnungen in Ihrem Log Analytics-Arbeitsbereich mithilfe von Azure Monitor erstellen.

- [Überwachen von Azure Backup-Workloads](backup-azure-monitoring-built-in-monitor.md)

- [Durchführen des Onboardings für Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Erstellen, Anzeigen und Verwalten von Protokollwarnungen mithilfe von Azure Monitor](../azure-monitor/alerts/alerts-log.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="identity-and-access-control"></a>Identität und Zugriffssteuerung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Identität und Zugriffssteuerung](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Verwalten eines Bestands von Administratorkonten

**Leitfaden**: Azure Active Directory (Azure AD) verfügt über integrierte Rollen, die explizit zugewiesen werden müssen, und Sie können durch Abfragen bestimmen, welche Konten Mitglieder dieser Rollen sind. Verwenden Sie das Azure AD PowerShell-Modul, um Ad-hoc-Abfragen zum Ermitteln von Konten auszuführen, die Mitglieder von administrativen Gruppen sind.

- [Abrufen einer Verzeichnisrolle in Azure AD mit PowerShell](/powershell/module/azuread/get-azureaddirectoryrole)

- [Abrufen von Mitgliedern einer Verzeichnisrolle in Azure AD mit PowerShell](/powershell/module/azuread/get-azureaddirectoryrolemember)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Ändern von Standardkennwörtern bei Bedarf

**Leitfaden**: Azure Active Directory (Azure AD) verfolgt nicht das Konzept der Standardkennwörter. Bei anderen Azure-Ressourcen, für die ein Kennwort erforderlich ist, wird erzwungen, dass ein Kennwort mit komplexen Anforderungen und einer minimalen Kennwortlänge, die sich je nach Dienst unterscheiden, erstellt wird. Sie sind verantwortlich für Anwendungen und Marketplace-Dienste von Drittanbietern, bei denen möglicherweise Standardkennwörter verwendet werden.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Verwenden dedizierter Administratorkonten

**Leitfaden**: Erstellen Sie Standardbetriebsvorgänge für die Verwendung dedizierter Administratorkonten. Verwenden Sie die Identitäts- und Zugriffsverwaltung in Azure Security Center, um die Anzahl der Administratorkonten zu überwachen.

Zusätzlich können Sie, um den Überblick über dedizierte Verwaltungskonten zu behalten, Empfehlungen des Azure Security Center oder integrierte Azure-Richtlinien befolgen, wie z. B:

- Ihrem Abonnement sollte mehr als ein Besitzer zugewiesen sein.

- Veraltete Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.

- Externe Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.

Zusätzliche Informationen finden Sie unter den aufgeführten Links.

- [Überwachen der Identität und des Zugriffs](../security-center/security-center-identity-access.md)

- [Tutorial: Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung](../governance/policy/tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4: Verwenden des einmaligen Anmeldens (SSO) in Azure Active Directory

**Leitfaden**: Verwenden Sie eine Azure-App-Registrierung (Dienstprinzipal), um ein Token abzurufen, das der Interaktion mit Ihren Recovery Services-Tresoren über API-Aufrufe dient.

- [Aufrufen von Azure-REST-APIs mit Postman](/rest/api/azure/#how-to-call-azure-rest-apis-with-postman)

- [Registrieren Ihrer Clientanwendung (Dienstprinzipal) bei Azure Active Directory (Azure AD)](/rest/api/azure/#register-your-client-application-with-azure-ad)

- [Recovery Services](/rest/api/recoveryservices/)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Verwenden der mehrstufigen Authentifizierung für den gesamten Azure Active Directory-basierten Zugriff

**Leitfaden**: Wenn Sie wichtige Vorgänge in Azure Backup ausführen, müssen Sie eine im Azure-Portal verfügbare Sicherheits-PIN eingeben. Durch das Aktivieren von Azure Active Directory Multifactor Authentication (Azure AD) wird eine Sicherheitsebene hinzugefügt. Nur autorisierte Benutzer mit gültigen Azure-Anmeldeinformationen, die über ein zweites Gerät authentifiziert sind, können auf das Azure-Portal zugreifen.

- [Azure Multi-Factor Authentication in Azure Backup](backup-azure-security-feature.md)

- [Planen einer cloudbasierten Azure AD Multi-Factor Authentication-Bereitstellung](../active-directory/authentication/howto-mfa-getstarted.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Verwenden dedizierter Computer (Arbeitsstationen mit privilegiertem Zugriff) für alle administrativen Aufgaben

**Leitfaden**: Verwenden Sie von Azure verwaltete Arbeitsstationen mit Azure AD Multi-Factor Authentication (MFA), die für die Anmeldung und Konfiguration Ihrer Azure Backup-fähigen Ressourcen konfiguriert sind.

- [Privileged Access Workstations](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Planen einer cloudbasierten Azure AD Multi-Factor Authentication-Bereitstellung](../active-directory/authentication/howto-mfa-getstarted.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Protokollieren von und Warnen bei verdächtigen Aktivitäten in Administratorkonten

**Leitfaden**: Verwenden Sie Azure Active Directory Privileged Identity Management (Azure AD/PIM) für die Generierung von Protokollen und Warnungen bei verdächtigen oder sicherheitsrelevanten Aktivitäten in der Umgebung.

Verwenden Sie zusätzlich Azure AD-Risikoerkennungen, um Warnungen und Berichte zu riskantem Benutzerverhalten anzuzeigen.

- [Bereitstellen von Azure AD Privileged Identity Management (PIM)](../active-directory/privileged-identity-management/pim-deployment-plan.md)

- [Azure Active Directory-Risikoerkennungen](../active-directory/identity-protection/overview-identity-protection.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Verwalten von Azure-Ressourcen nur über genehmigte Standorte

**Leitfaden**: Verwenden Sie benannte Standorte mit bedingtem Zugriff, um den Zugriff auf das Azure-Portal nur über bestimmte logische Gruppierungen von IP-Adressbereichen oder Ländern/Regionen zuzulassen.

- [Konfigurieren benannter Standorte in Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="39-use-azure-active-directory"></a>3.9: Verwenden von Azure Active Directory

**Leitfaden**: Verwenden Sie Azure Active Directory (Azure AD) als zentrales Authentifizierungs- und Autorisierungssystem für Ihre Azure Backup-Instanzen. Azure AD schützt Daten durch eine starke Verschlüsselung für ruhende und übertragene Daten. Außerdem werden in Azure AD Salts und Hashs verwendet, und Anmeldeinformationen werden sicher gespeichert.

- [Konfigurieren Ihrer App Service- oder Azure Functions-App zur Verwendung der Azure AD-Anmeldung](../app-service/configure-authentication-provider-aad.md)

- [Erstellen und Konfigurieren einer Azure AD-Instanz](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Regelmäßiges Überprüfen und Abstimmen des Benutzerzugriffs

**Leitfaden**: Azure Active Directory (Azure AD) enthält Protokolle zum Ermitteln von veralteten Konten. Verwenden Sie zusätzlich Azure AD-Zugriffsüberprüfungen, um Gruppenmitgliedschaften den Zugriff auf Unternehmensanwendungen und Rollenzuweisungen effizient zu verwalten. Der Benutzerzugriff sollte regelmäßig überprüft werden, um sicherzustellen, dass nur die richtigen Benutzer weiterhin Zugriff haben.

- [Grundlegendes zur Azure AD-Berichterstellung](../active-directory/reports-monitoring/index.yml)

- [Verwenden von Azure AD-Zugriffsüberprüfungen](../active-directory/governance/access-reviews-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Überwachen von Zugriffsversuchen auf deaktivierte Anmeldeinformationen

**Leitfaden**: Verwenden Sie Azure Active Directory (Azure AD) als zentrales Authentifizierungs- und Autorisierungssystem für Ihre Azure Backup-Instanzen. Azure AD schützt Daten durch eine starke Verschlüsselung für ruhende und übertragene Daten. Außerdem werden in Azure AD Salts und Hashs verwendet, und Anmeldeinformationen werden sicher gespeichert.

Sie haben Zugriff auf Azure AD-Anmeldeaktivitäten sowie auf Quellen für Überwachungs- und Risikoereignisprotokolle, wodurch die Integration mit Azure Sentinel oder der SIEM-Lösung eines Drittanbieters möglich ist.

Sie können diesen Prozess optimieren, indem Sie Diagnoseeinstellungen für Azure AD-Benutzerkonten erstellen und die Überwachungs- und Anmeldeprotokolle an einen Log Analytics-Arbeitsbereich senden. Sie können gewünschte Warnungen in Log Analytics konfigurieren.

- [Integrieren von Azure-Aktivitätsprotokollen in Azure Monitor](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

- [Schnellstart: Ausführen des Onboardings für Azure Sentinel](../sentinel/quickstart-onboard.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12: Warnung bei abweichendem Verhalten bei der Kontoanmeldung

**Leitfaden**: Verwenden Sie Azure Active Directory (Azure AD) als zentrales Authentifizierungs- und Autorisierungssystem für Ihre Recovery Services-Tresore. Falls abweichendes Anmeldeverhalten bei der Kontoanmeldung auf Steuerungsebene (dem Azure-Portal) auftritt, verwenden Sie Azure AD Identity Protection und Risikoerkennungsfeatures, um automatisierte Reaktionen auf erkannte verdächtige Aktionen im Zusammenhang mit Benutzeridentitäten zu konfigurieren. Außerdem können Sie Daten zur weiteren Untersuchung in Azure Sentinel erfassen.

- [Bericht „Riskante Anmeldungen“ im Azure Active Directory-Portal](../active-directory/identity-protection/overview-identity-protection.md)

- [Konfigurieren und Aktivieren von Risikorichtlinien für den Identitätsschutz](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Durchführen des Onboardings für Azure Sentinel](../sentinel/quickstart-onboard.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Ermöglichen des Zugriffs auf relevante Kundendaten für Microsoft in Supportszenarien

**Leitfaden**: Zurzeit nicht verfügbar. Die Kunden-Lockbox wird für Azure Backup noch nicht unterstützt.

- [Unterstützte Dienste und Szenarios bei allgemeiner Verfügbarkeit](../security/fundamentals/customer-lockbox-overview.md#supported-services-and-scenarios-in-general-availability)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="data-protection"></a>Datenschutz

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Schutz von Daten](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Verwalten eines Bestands an vertraulichen Informationen

**Leitfaden**: Verwenden Sie Tags für die Nachverfolgung von Azure-Ressourcen, die vertrauliche Informationen speichern oder verarbeiten.

- [Erstellen und Verwenden von Tags](../azure-resource-manager/management/tag-resources.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Isolieren von Systemen, die vertrauliche Informationen speichern oder verarbeiten

**Leitfaden**: Wenn Sie Azure-IaaS-VMs (Infrastructure-as-a-Service) sichern, bietet Azure Backup unabhängige und isolierte Sicherungen zum Schutz vor dem versehentlichen Löschen von Originaldaten. Sicherungen werden in einem Recovery Services-Tresor mit integrierter Verwaltung von Wiederherstellungspunkten gespeichert.

Implementieren Sie zu Entwicklungs-, Test- und Produktionszwecken jeweils separate Abonnements und optional Verwaltungsgruppen für Recovery Services-Tresore. Ressourcen sollten durch virtuelle Netzwerke oder Subnetze getrennt, entsprechend gekennzeichnet und durch Netzwerksicherheitsgruppen oder Azure Firewall gesichert werden. Ressourcen, die vertrauliche Daten speichern oder verarbeiten, müssen ausreichend isoliert werden. Implementieren Sie für VMs, auf denen vertrauliche Informationen gespeichert oder verarbeitet werden, Richtlinien und Verfahren, mit denen Sie sie bei Nichtverwendung deaktivieren können.

- [Azure Backup – Übersicht](backup-overview.md)

- [Erstellen zusätzlicher Azure-Abonnements](../cost-management-billing/manage/create-subscription.md)

- [Erstellen von Verwaltungsgruppen](../governance/management-groups/create-management-group-portal.md)

- [Erstellen und Verwenden von Tags](../azure-resource-manager/management/tag-resources.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Überwachen und Blockieren einer nicht autorisierten Übertragung vertraulicher Informationen

**Leitfaden**: Zurzeit nicht verfügbar. Features für die Datenidentifizierung und -klassifizierung sowie zur Vermeidung von Datenverlust sind noch nicht für Azure Backup verfügbar.

Microsoft verwaltet die zugrunde liegende Infrastruktur für Azure Backup und hat strenge Kontrollen implementiert, um Verluste oder Offenlegungen von Kundendaten zu verhindern.

- [Grundlegendes zum Schutz von Kundendaten in Azure](../security/fundamentals/protection-customer-data.md)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Verwenden eines aktiven Ermittlungstools zur Bestimmung vertraulicher Daten

**Leitfaden**: Zurzeit nicht verfügbar. Features für die Datenidentifizierung und -klassifizierung sowie zur Vermeidung von Datenverlust sind noch nicht für Azure Backup verfügbar.

Microsoft verwaltet die zugrunde liegende Infrastruktur für Azure Backup und hat strenge Kontrollen implementiert, um Verluste oder Offenlegungen von Kundendaten zu verhindern.

- [Grundlegendes zum Schutz von Kundendaten in Azure](../security/fundamentals/protection-customer-data.md)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4.6: Verwenden der rollenbasierten Zugriffssteuerung zum Steuern des Zugriffs auf Ressourcen

**Leitfaden**: Die rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC) ermöglicht eine präzise Zugriffsverwaltung für Azure. Mithilfe von Azure RBAC können Sie Aufgaben in Ihrem Team verteilen und Benutzern nur den Zugriff gewähren, den sie zur Ausführung ihrer Aufgaben benötigen.

Azure Backup bietet drei integrierte Rollen, um Vorgänge der Sicherungsverwaltung zu steuern: Sicherungsmitwirkender, Sicherungsoperator und Benutzer mit Leseberechtigung für Sicherungsfunktionen. Sie können die integrierten Backup-Rollen zu Aktionen der Sicherungsverwaltung zuordnen.

- [Konfigurieren von Azure RBAC](../role-based-access-control/role-assignments-portal.md)

- [Verwenden der rollenbasierten Zugriffssteuerung in Azure zum Verwalten von Azure Backup-Wiederherstellungspunkten](backup-rbac-rs-vault.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Verschlüsseln vertraulicher, ruhender Informationen

**Leitfaden**: Azure Backup unterstützt die Verschlüsselung von ruhenden Daten. Für die lokale Sicherung erfolgt eine Verschlüsselung im Ruhezustand über die beim Sichern in Azure bereitgestellte Passphrase. Für Cloudworkloads werden die Daten im Ruhezustand mit der Speicherdienstverschlüsselung verschlüsselt. Die Sicherungsdaten werden zu keinem Zeitpunkt von Microsoft entschlüsselt.

Wenn Sie mithilfe des MARS-Agents oder eines Recovery Services-Tresors, der mit einem vom Kunden verwalteten Schlüssel verschlüsselt wurde, eine Sicherung erstellen, haben Sie nur Zugriff auf den Verschlüsselungsschlüssel. Microsoft bewahrt keine Kopie auf und hat keinen Zugriff auf den Schlüssel. Wenn der Schlüssel verlegt wird, kann Microsoft die gesicherten Daten nicht wiederherstellen.

- [Verschlüsselung in Azure Backup](backup-encryption.md)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Protokollieren und Warnen bei Änderungen an wichtigen Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Monitor mit dem Azure-Aktivitätsprotokoll, um Warnungen für den Fall zu erstellen, dass Änderungen an Azure Recovery Services-Produktionstresoren und anderen kritischen bzw. zugehörigen Ressourcen vorgenommen werden.

- [Erstellen von Warnungen für Ereignisse des Azure-Aktivitätsprotokolls](../azure-monitor/alerts/alerts-activity-log.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="vulnerability-management"></a>Verwaltung von Sicherheitsrisiken

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Verwaltung von Sicherheitsrisiken](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Ausführen automatisierter Scan-Tools für Sicherheitsrisiken

**Leitfaden**: Noch nicht verfügbar. Die Beurteilung von Sicherheitsrisiken im Azure Security Center ist noch nicht für Azure Backup verfügbar.

Die zugrunde liegende Plattform wird von Microsoft überprüft und gepatcht. Überprüfen Sie die für Azure Backup verfügbaren Sicherheitsfunktionen, um Sicherheitsrisiken im Zusammenhang mit der Dienstkonfiguration zu minimieren.

- [Sicherheitsfeatures für Azure Backup]()

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Verwenden eines Risikobewertungsprozesses, um die Behebung von erkannten Sicherheitsrisiken zu priorisieren

**Leitfaden**: Zurzeit nicht verfügbar. Sicherheitskonfigurationen für Azure Backup werden im Azure Security Center noch nicht unterstützt.

- [Features für Azure PaaS-Ressourcen](../security-center/features-paas.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="inventory-and-asset-management"></a>Bestands- und Ressourcenverwaltung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Bestands- und Ressourcenverwaltung](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Verwenden der automatisierten Asset Discovery-Lösung

**Leitfaden**: Verwenden Sie Azure Resource Graph, um alle Ressourcen (z. B. Computeressourcen, Speicher, Netzwerke, Ports und Protokolle) in ihren Abonnements abzufragen bzw. zu ermitteln. Stellen Sie entsprechende (Lese-) Berechtigungen in Ihrem Mandanten sicher, und zählen Sie alle Azure-Abonnements sowie Ressourcen in ihren Abonnements auf.

Obwohl klassische Azure-Ressourcen über das Resource Graph ermittelt werden können, wird dringend empfohlen, Azure Resource Manager-Ressourcen zu erstellen und zu verwenden.

- [Erstellen von Abfragen mit Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

- [Anzeigen Ihrer Azure-Abonnements](/powershell/module/az.accounts/get-azsubscription?preserve-view=true&view=azps-4.8.0)

- [Grundlegendes zu Azure RBAC](../role-based-access-control/overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="62-maintain-asset-metadata"></a>6.2: Verwalten von Ressourcenmetadaten

**Leitfaden**: Wenden Sie Tags auf Ihre Azure-Ressourcen an, die Metadaten erzeugen, um sie logisch in einer Taxonomie zu organisieren.

- [Erstellen und Verwenden von Tags](../azure-resource-manager/management/tag-resources.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Löschen nicht autorisierter Azure-Ressourcen

**Leitfaden**: Verwenden Sie Tagging, Verwaltungsgruppen und separate Abonnements nach Bedarf, um Ressourcen zu verwalten und nachzuverfolgen. Stimmen Sie den Bestand regelmäßig ab, und stellen Sie sicher, dass nicht autorisierte Ressourcen rechtzeitig aus dem Abonnement gelöscht werden.

Verwenden Sie darüber hinaus Azure Policy, um Einschränkungen für die Ressourcentypen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie dazu die folgenden integrierten Richtliniendefinitionen:

- Not allowed resource types (Unzulässige Ressourcentypen)

- Zulässige Ressourcentypen

Zusätzliche Informationen finden Sie unter den aufgeführten Links.

- [Erstellen zusätzlicher Azure-Abonnements](../cost-management-billing/manage/create-subscription.md)

- [Erstellen von Verwaltungsgruppen](../governance/management-groups/create-management-group-portal.md)

- [Erstellen und Verwenden von Tags](../azure-resource-manager/management/tag-resources.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4: Definieren und Verwalten des Bestands an genehmigten Azure-Ressourcen

**Leitfaden**: Definieren Sie genehmigte Azure-Ressourcen und genehmigte Software für Computeressourcen.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Überwachung auf nicht genehmigte Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy, um Einschränkungen für den Typ der Ressourcen festzulegen, die in Ihren Abonnements erstellt werden können.

Verwenden Sie Azure Resource Graph, um Ressourcen in ihren Abonnements abzufragen und zu ermitteln.  Stellen Sie sicher, dass alle in der Umgebung vorhandenen Azure-Ressourcen genehmigt sind.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Erstellen von Abfragen mit Azure Graph](../governance/resource-graph/first-query-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="69-use-only-approved-azure-services"></a>6.9: Ausschließliche Verwendung genehmigter Azure-Dienste

**Leitfaden**: Verwenden Sie Azure Policy, um Einschränkungen für den Typ der Ressourcen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie hierzu die folgenden integrierten Richtliniendefinitionen:

- Not allowed resource types (Unzulässige Ressourcentypen)

- Zulässige Ressourcentypen

Zusätzliche Informationen finden Sie unter den aufgeführten Links.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Ablehnen eines bestimmten Ressourcentyps mit Azure Policy](../governance/policy/samples/built-in-policies.md#general)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Einschränken der Möglichkeiten von Benutzern zur Interaktion mit Azure Resource Manager 

**Leitfaden**: Konfigurieren Sie bedingten Azure-Zugriff, um die Möglichkeiten der Benutzer zur Interaktion mit Azure Resource Manager einzuschränken, indem Sie „Zugriff blockieren“ für die App zur „Verwaltung von Microsoft Azure“ konfigurieren.

- [Verwalten des Zugriffs auf die Azure-Verwaltung mit bedingtem Zugriff](../role-based-access-control/conditional-access-azure-management.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="secure-configuration"></a>Sichere Konfiguration

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Sichere Konfiguration](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Einrichten sicherer Konfigurationen für alle Azure-Ressourcen

**Leitfaden**: Definieren und implementieren Sie Standardsicherheitskonfigurationen für Ihren Recovery Services-Tresor mit Azure Policy. Verwenden Sie Azure Policy-Aliase im Namespace „Microsoft.RecoveryServices“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen der Konfiguration Ihrer Recovery Services-Tresore zu erstellen.

- [Anzeigen verfügbarer Azure Policy-Aliase](/powershell/module/az.resources/get-azpolicyalias?preserve-view=true&view=azps-4.8.0)

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Verwalten von sicheren Konfigurationen für Azure-Ressourcen

**Leitfaden**: Verwenden Sie die Azure Policy-Auswirkungen [deny] (Verweigern) und [deploy if not exist] (Bereitstellen, falls nicht vorhanden), um sichere Einstellungen für alle Ihre Azure-Ressourcen zu erzwingen.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Grundlegendes zu Azure Policy-Auswirkungen](../governance/policy/concepts/effects.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Sicheres Speichern der Konfiguration von Azure-Ressourcen

**Leitfaden**: Wenn Sie benutzerdefinierte Azure Policy-Definitionen verwenden, nutzen Sie Azure DevOps oder Azure Repos, um Ihren Code sicher zu speichern und zu verwalten.

- [Speichern von Code in Azure DevOps](/azure/devops/repos/git/gitworkflow)

- [Dokumentation zu Azure Repos](/azure/devops/repos)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Bereitstellen von Konfigurationsverwaltungstools für Azure-Ressourcen

**Leitfaden**: Verwenden Sie integrierte Azure Policy-Definitionen sowie Azure Policy-Aliase im Namespace „Microsoft.RecoveryServices“, um benutzerdefinierte Richtlinien zum Überwachen und Erzwingen von Systemkonfigurationen zu erstellen und Warnungen dazu zu erhalten. Entwickeln Sie außerdem einen Prozess und eine Pipeline zum Verwalten von Richtlinienausnahmen.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementieren der automatisierten Konfigurationsüberwachung für Azure-Ressourcen

**Leitfaden**: Verwenden Sie integrierte Azure Policy-Definitionen sowie Azure Policy-Aliase im Namespace **Microsoft.RecoveryServices**, um benutzerdefinierte Richtlinien zum Überwachen und Erzwingen von Systemkonfigurationen zu erstellen und Warnungen dazu zu erhalten. Verwenden Sie die Azure Policy-Auswirkungen [audit] (überwachen), [deny] (verweigern) und [deploy if not exist] (bereitstellen, falls nicht vorhanden), um Konfigurationen für Ihre Azure-Ressourcen automatisch zu erzwingen.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="711-manage-azure-secrets-securely"></a>7.11: Sicheres Verwalten von Azure-Geheimnissen

**Leitfaden**: Wenn Sie den MARS-Agent einrichten, speichern Sie die Verschlüsselungspassphrase in Azure Key Vault.

- [Authentifizieren bei Key Vault](../key-vault/general/authentication.md)

- [Zuweisen einer Key Vault-Zugriffsrichtlinie](../key-vault/general/assign-access-policy-portal.md)

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

**Leitfaden**: Microsoft Antimalware für Azure ist auf dem zugrunde liegenden Host aktiviert, der die Azure-Dienste (z. B. Azure Backup) unterstützt. Das Feature wird jedoch nicht für Ihre Inhalte ausgeführt.

Führen Sie eine Vorabprüfung von Dateien durch, die in computefremde Azure-Ressourcen hochgeladen werden sollen, z. B. in App Service, Data Lake Storage und Blob Storage.

Mithilfe des Bedrohungsschutzes für Datendienste in Azure Security Center können Sie Schadsoftware erkennen, die in Speicherkonten hochgeladen wurde.

- [Grundlegendes zu Microsoft Antimalware für Azure Cloud Services und Virtual Machines](../security/fundamentals/antimalware.md)

- [Grundlegendes zum Bedrohungsschutz für Datendienste in Azure Security Center](../security-center/azure-defender.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="data-recovery"></a>Datenwiederherstellung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Datenwiederherstellung](../security/benchmarks/security-control-data-recovery.md).*

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Durchführen vollständiger Systemsicherungen und Sichern aller kundenseitig verwalteten Schlüssel

**Leitfaden**: Lokal redundanter Speicher (Locally Redundant Storage, LRS) repliziert Ihre Daten dreimal in einer Speicherskalierungseinheit in einem Datencenter. (Es werden also drei Kopien Ihrer Daten erstellt.) Alle Kopien der Daten befinden sich in derselben Region. LRS ist eine kostengünstige Möglichkeit, um Daten vor lokalen Hardwarefehlern zu schützen. Geografisch redundanter Speicher (Geo-Redundant Storage, GRS) ist die standardmäßige und empfohlene Replikationsoption. GRS repliziert Ihre Daten in einer sekundären Region, die mehrere hundert Kilometer vom primären Speicherort der Quelldaten entfernt ist. GRS führt zu höheren Kosten als LRS, bietet aber eine höhere Dauerhaftigkeit Ihrer Daten (auch im Falle eines regionalen Ausfalls).

Sichern Sie die von Kunden verwalteten Schlüssel in Azure Key Vault.

- [Azure Backup – Übersicht](backup-overview.md)

- [Sichern von Schlüsseltresorschlüsseln in Azure](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey)

- [Verschlüsselung von Sicherungsdaten mit vom Kunden verwalteten Schlüsseln](backup-encryption.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/azure/governance/policy/samples/azure-security-benchmark) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/azure/security-center/security-center-recommendations). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/azure/security-center/azure-defender)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.RecoveryServices**:

[!INCLUDE [Resource Policy for Microsoft.RecoveryServices 9.2](../../includes/policy/standards/asb/rp-controls/microsoft.recoveryservices-9-2.md)]

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Überprüfen aller Sicherungen einschließlich kundenseitig verwalteter Schlüssel

**Leitfaden**: Testen Sie die Wiederherstellung von gesicherten von Kunden verwalteten Schlüsseln.

- [Wiederherstellen von Schlüsseltresorschlüsseln in Azure](/powershell/module/az.keyvault/restore-azkeyvaultkey)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Sicherstellen des Schutzes von Sicherungen und von kundenseitig verwalteten Schlüsseln

**Leitfaden**: Für die lokale Sicherung erfolgt eine Verschlüsselung im Ruhezustand über die beim Sichern in Azure bereitgestellte Passphrase. Für virtuelle Azure-Computer werden die Daten mit der Speicherdienstverschlüsselung (Storage Service Encryption, SSE) im Ruhezustand verschlüsselt. Sie können das vorläufige Löschen in Key Vault aktivieren, um Schlüssel vor dem versehentlichen oder böswilligen Löschen zu schützen.

- [Vorläufiges Löschen für Azure Storage-Blobs](../storage/blobs/soft-delete-blob-overview.md?tabs=azure-portal)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="incident-response"></a>Reaktion auf Vorfälle

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Reaktion auf Vorfälle](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Erstellen eines Leitfadens für die Reaktion auf Vorfälle

**Anleitung:** Erarbeiten Sie einen Leitfaden für die Reaktion auf Vorfälle für Ihre Organisation. Stellen Sie sicher, dass es schriftliche Pläne für die Reaktion auf Vorfälle gibt, in denen alle Rollen der Mitarbeiter sowie die Phasen der Bearbeitung und Verwaltung von Vorfällen von der Ermittlung bis zur abschließenden Überprüfung definiert sind.

- [Leitfaden zu Planung und Betrieb](../security-center/security-center-planning-and-operations-guide.md)

- [Anleitung zum Entwickeln eines Prozesses für die Reaktion auf Sicherheitsvorfälle](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process)

- [Struktur eines Vorfalls laut Microsoft Security Response Center](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process)

- [Computer Security Incident Handling Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) (Leitfaden für den Umgang mit Computersicherheitsincidents) des NIST (National Institute of Standards and Technology)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Erstellen eines Verfahrens zur Bewertung und Priorisierung von Vorfällen

**Anleitung:** Security Center weist jeder Warnung einen Schweregrad zu, damit Sie priorisieren können, welche Warnungen zuerst untersucht werden sollen. Der Schweregrad basiert darauf, wie zuversichtlich Security Center in Bezug auf den Befund oder die Analyse ist, die zum Auslösen der Warnung verwendet wird, sowie auf dem Zuverlässigkeitsgrad, dass hinter der Aktivität, die zu der Warnung führte, eine böswillige Absicht stand.

Markieren Sie außerdem Abonnements aussagekräftig, und erstellen Sie ein Benennungssystem, um Azure-Ressourcen eindeutig zu identifizieren und zu kategorisieren.

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

**Leitfaden**: Befolgen Sie die Microsoft Rules of Engagement, um sicherzustellen, dass Ihre Penetrationstests nicht gegen Microsoft-Richtlinien verstoßen. Weitere Informationen zur Microsoft-Strategie im Zusammenhang mit Red Team- und Livewebsite-Penetrationstests für von Microsoft verwaltete Cloudinfrastrukturen, Dienste und Anwendungen sowie zu deren Durchführung finden Sie unter den angegebenen Links.

- [Penetrationstests – Rules of Engagement](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

- [Microsoft Cloud Red Teaming](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die [Übersicht über Version 2 des Azure-Sicherheitsvergleichstests](../security/benchmarks/overview.md) an.
- Erfahren Sie mehr über [Azure-Sicherheitsbaselines](../security/benchmarks/security-baselines-overview.md).
