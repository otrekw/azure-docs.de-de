---
title: Azure-Sicherheitsbaseline für Azure DevTest Labs
description: Azure-Sicherheitsbaseline für Azure DevTest Labs
ms.topic: conceptual
ms.date: 07/23/2020
ms.openlocfilehash: 7f0dc1fb18740e2b0611b5954821a5ceda2d2657
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87387828"
---
# <a name="azure-security-baseline-for-azure-devtest-labs"></a>Azure-Sicherheitsbaseline für Azure DevTest Labs

Die Azure-Sicherheitsbaseline für Azure DevTest Labs enthält Empfehlungen, mit deren Hilfe Sie den Sicherheitsstatus Ihrer Bereitstellung verbessern können.

Die Baseline für diesen Dienst wird von [Azure Security Benchmark-Version 1.0](../security/benchmarks/overview.md) abgeleitet, die Empfehlungen dazu enthält, wie Sie Ihre Cloudlösungen in Azure schützen können.

Weitere Informationen finden Sie unter [Übersicht über Azure-Sicherheitsbaselines](../security/benchmarks/security-baselines-overview.md).

## <a name="logging-and-monitoring"></a>Protokollierung und Überwachung

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Protokollierung und Überwachung](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1: Verwenden von genehmigten Zeitsynchronisierungsquellen
**Leitfaden**: Microsoft verwaltet Zeitquellen für Azure-Ressourcen. Sie können jedoch Zeitsynchronisierungseinstellungen für Ihre Computeressourcen verwalten.

Weitere Informationen zum Konfigurieren der Zeitsynchronisierung für Azure-Computeressourcen finden Sie im folgenden Artikel: [Zeitsynchronisierung für Windows-VMs in Azure](../virtual-machines/windows/time-sync.md). 

**Azure Security Center-Überwachung:** Zurzeit nicht verfügbar

**Verantwortlichkeit:** Microsoft

### <a name="22-configure-central-security-log-management"></a>2.2: Konfigurieren der zentralen Sicherheitsprotokollverwaltung
**Leitfaden**: Aktivieren Sie die Diagnoseeinstellungen für das Azure-Aktivitätsprotokoll, und senden Sie die Protokolle zwecks Archivierung an einen Log Analytics-Arbeitsbereich, eine Azure Event Hub-Instanz oder ein Azure-Speicherkonto. Aktivitätsprotokolle geben Einblick in die Vorgänge, die für Ihre Azure DevTest Labs-Instanzen auf Verwaltungsebene erfolgen. Mit den Daten aus Azure-Aktivitätsprotokollen können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge (PUT, POST, DELETE) ermitteln, die auf Verwaltungsebene Ihre Azure DevTest Labs-Instanzen durchgeführt werden.

Weitere Informationen finden Sie unter [Erstellen von Diagnoseeinstellungen zum Senden von Plattformprotokollen und Metriken an verschiedene Ziele](../azure-monitor/platform/diagnostic-settings.md).

**Azure Security Center-Überwachung:** Zurzeit nicht verfügbar

**Verantwortlichkeit:** Kunde

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Aktivieren der Überwachungsprotokollierung für Azure-Ressourcen
**Leitfaden**: Aktivieren Sie die Diagnoseeinstellungen für das Azure-Aktivitätsprotokoll, und senden Sie die Protokolle zwecks Archivierung an einen Log Analytics-Arbeitsbereich, eine Azure Event Hub-Instanz oder ein Azure-Speicherkonto. Aktivitätsprotokolle geben Einblick in die Vorgänge, die für Ihre Azure DevTest Labs-Instanzen auf Verwaltungsebene erfolgen. Mit den Daten aus Azure-Aktivitätsprotokollen können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge (PUT, POST, DELETE) ermitteln, die auf Verwaltungsebene Ihre Azure DevTest Labs-Instanzen durchgeführt werden.

Weitere Informationen finden Sie unter [Erstellen von Diagnoseeinstellungen zum Senden von Plattformprotokollen und Metriken an verschiedene Ziele](../azure-monitor/platform/diagnostic-settings.md).

**Azure Security Center-Überwachung:** Zurzeit nicht verfügbar

**Verantwortlichkeit:** Kunde

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: Erfassen von Sicherheitsprotokollen von Betriebssystemen
**Leitfaden**: Azure DevTest Labs-VMs werden erstellt und befinden sich im Besitz des Kunden. Daher liegt es in der Verantwortung der Organisation, sie zu überwachen. Sie können Azure Security Center zum Überwachen des Computebetriebssystems verwenden. Von Azure Security Center vom Betriebssystem erfasste Daten sind z. B. Betriebssystemtyp und -version, Betriebssystem (Windows-Ereignisprotokolle), ausgeführte Prozesse, Computername, IP-Adressen und angemeldeter Benutzer. Darüber hinaus erfasst der Log Analytics Agent Absturzabbilddateien.

Weitere Informationen finden Sie in den folgenden Artikeln: 

- [Sammeln von internen Azure Virtual Machine-Hostprotokollen mit Azure Monitor](../azure-monitor/learn/quick-collect-azurevm.md)
- [Grundlegendes zur Datensammlung in Azure Security Center](../security-center/security-center-enable-data-collection.md)

**Azure Security Center-Überwachung:** Ja

**Verantwortlichkeit:** Kunde

### <a name="25-configure-security-log-storage-retention"></a>2.5: Konfigurieren der Sicherheitsprotokoll-Aufbewahrungsdauer im Speicher
***Leitfaden:** Legen Sie in Azure Monitor die Protokollaufbewahrungsdauer für Log Analytics-Arbeitsbereiche, die Ihrer Azure DevTest Labs-Instanz zugeordnet sind, gemäß den Compliancevorschriften Ihres Unternehmens fest.

Weitere Informationen finden Sie im folgenden Artikel: [Ändern des Datenaufbewahrungszeitraums](../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

**Azure Security Center-Überwachung:** Nicht verfügbar

**Verantwortlichkeit:** Kunde

### <a name="26-monitor-and-review-logs"></a>2.6: Überwachen und Überprüfen von Protokollen
**Leitfaden**: Aktivieren Sie die Diagnoseeinstellungen für das Azure-Aktivitätsprotokoll, und senden Sie die Protokolle an einen Log Analytics-Arbeitsbereich. Führen Sie Abfragen in Log Analytics durch, um Suchbegriffe zu ermitteln, Trends auszumachen, Muster zu analysieren und viele weitere Erkenntnisse auf Grundlage der Aktivitätsprotokolldaten zu gewinnen, die möglicherweise für Azure DevTest Labs erfasst wurden.

Weitere Informationen finden Sie in den folgenden Artikeln:

- [Erfassen und Analysieren des Azure-Aktivitätsprotokolls in Azure Monitor](../azure-monitor/platform/diagnostic-settings.md)
- [Erfassen und Analysieren des Azure-Aktivitätsprotokolls in Azure Monitor](../azure-monitor/platform/activity-log.md)

**Azure Security Center-Überwachung:** Nicht verfügbar

**Verantwortlichkeit:** Kunde

### <a name="27-enable-alerts-for-anomalous-activity"></a>2.7: Aktivieren von Warnungen für anormale Aktivitäten
**Leitfaden**: Verwenden Sie den Azure Log Analytics-Arbeitsbereich zum Überwachen von und Warnen vor anomalen Aktivitäten in Sicherheitsprotokollen und -ereignissen für Azure DevTest Labs.

Weitere Informationen finden Sie im folgenden Artikel: [Warnungen bei Log Analytics-Protokolldaten](../azure-monitor/learn/tutorial-response.md)

**Azure Security Center-Überwachung:** Zurzeit nicht verfügbar

**Verantwortlichkeit:** Kunde

### <a name="28-centralize-anti-malware-logging"></a>2.8: Zentralisieren der Antischadsoftwareprotokollierung
**Leitfaden**: Nicht zutreffend Azure DevTest Labs verarbeitet oder generiert keine Protokolle zu Antischadsoftware.

**Azure Security Center-Überwachung:** Nicht verfügbar

**Verantwortlichkeit:** Kunde

### <a name="29-enable-dns-query-logging"></a>2.9: Aktivieren der DNS-Abfrageprotokollierung
**Leitfaden**: Nicht zutreffend Mit Azure DevTest Labs werden keine DNS-bezogenen Protokolle verarbeitet oder generiert.

**Azure Security Center-Überwachung:** Nicht verfügbar

**Verantwortlichkeit:** Kunde

### <a name="210-enable-command-line-audit-logging"></a>2.10: Aktivieren der Befehlszeilen-Überwachungsprotokollierung
**Leitfaden**: Azure DevTest Labs erstellt Azure Compute-Computer, die sich im Besitz des Kunden befinden und von diesem verwaltet werden. Verwenden Sie Microsoft Monitoring Agent auf allen unterstützten Azure Windows-VMs, um das Prozesserstellungsereignis und das Feld `CommandLine` zu protokollieren. Bei unterstützten virtuellen Azure Linux-VMs können Sie die Konsolenprotokollierung pro Knoten manuell konfigurieren und die Daten mithilfe von Syslog speichern. Verwenden Sie außerdem den Log Analytics-Arbeitsbereich von Azure Monitor, um Protokolle zu überprüfen und Abfragen für protokollierte Daten von Azure-VMs auszuführen.

- [Datensammlung in Azure Security Center](../security-center/security-center-enable-data-collection.md#data-collection-tier)
- [Ausführen benutzerdefinierter Abfragen in Azure Monitor](../azure-monitor/log-query/get-started-queries.md)
- [Syslog-Datenquellen in Azure Monitor](../azure-monitor/platform/data-sources-syslog.md)

**Azure Security Center-Überwachung:** Ja

**Verantwortlichkeit:** Kunde

## <a name="identity-and-access-control"></a>Identität und Zugriffssteuerung
*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Identität und Zugriffssteuerung](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Verwalten eines Bestands von Administratorkonten
**Leitfaden**: Azure Active Directory (Azure AD) umfasst integrierte Rollen, die explizit zugewiesen werden müssen und abgefragt werden können. Verwenden Sie das Azure AD PowerShell-Modul, um Ad-hoc-Abfragen zum Ermitteln von Konten auszuführen, die Mitglieder von administrativen Gruppen sind.

- [Abrufen einer Verzeichnisrolle in Azure AD mit PowerShell](/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)
- [Abrufen von Mitgliedern einer Verzeichnisrolle in Azure AD mit PowerShell](/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)
- [Azure DevTest Labs-Rollen](devtest-lab-add-devtest-user.md)  

**Azure Security Center-Überwachung:** Ja

**Verantwortlichkeit:** Kunde

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Ändern von Standardkennwörtern bei Bedarf
**Leitfaden**: Azure Active Directory (Azure AD) verfolgt nicht das Konzept von Standardkennwörtern. Bei anderen Azure-Ressourcen, für die ein Kennwort erforderlich ist, wird erzwungen, dass ein Kennwort mit komplexen Anforderungen und einer minimalen Kennwortlänge, die sich je nach Dienst unterscheiden, erstellt wird. Sie sind verantwortlich für Anwendungen und Marketplace-Dienste von Drittanbietern, bei denen möglicherweise Standardkennwörter verwendet werden.

Azure DevTest Labs verwendet nicht über das Konzept von Standardkennwörtern. 

**Azure Security Center-Überwachung:** Nicht verfügbar

**Verantwortlichkeit:** Kunde

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Verwenden dedizierter Administratorkonten
**Leitfaden**: Erstellen Sie Standardbetriebsvorgänge für die Verwendung dedizierter Administratorkonten. Verwenden Sie die Identitäts- und Zugriffsverwaltung in Azure Security Center, um die Anzahl der Administratorkonten zu überwachen.

Zusätzlich können Sie, um den Überblick über dedizierte Verwaltungskonten zu behalten, Empfehlungen des Azure Security Center oder integrierte Azure-Richtlinien befolgen, wie z. B:

- Ihrem Abonnement sollte mehr als ein Besitzer zugewiesen sein.
- Veraltete Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.
- Externe Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.

- [Überwachen der Identität und des Zugriffs](../security-center/security-center-identity-access.md)  
- [Tutorial: Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung](../governance/policy/tutorials/create-and-manage.md)
- [Azure DevTest Labs-Rollen](devtest-lab-add-devtest-user.md)  

**Azure Security Center-Überwachung:** Ja

**Verantwortlichkeit:** Kunde

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4: Verwenden des einmaligen Anmeldens (Single Sign-On, SSO) mit Azure Active Directory
**Leitfaden**: DevTest Labs verwendet den Azure AD-Dienst für Identitätsverwaltung. Berücksichtigen Sie diese beiden Hauptaspekte, wenn Sie Benutzern Zugriff auf eine Umgebung gewähren, die auf DevTest Labs basiert:

- **Ressourcenverwaltung:** Ermöglicht den Zugriff auf das Azure-Portal, um Ressourcen zu verwalten (Erstellen von VMs und Umgebungen sowie Starten, Beenden, Neustarten, Löschen und Anwenden von Artefakten). Die Ressourcenverwaltung erfolgt in Azure über die rollenbasierte Zugriffssteuerung (RBAC). Sie weisen Rollen den Benutzern zu und legen Ressourcen- und Zugriffsberechtigungen fest.
- **Virtuelle Computer (Netzwerkebene):** In der Standardkonfiguration verwenden VMs ein lokales Administratorkonto. Ist eine Domäne verfügbar (Azure AD Domain Services, eine lokale Domäne oder eine cloudbasierte Domäne), können Computer in diese Domäne eingebunden werden. Benutzer können dann Ihre domänenbasierten Identitäten mithilfe des Artefakts „Domänenbeitritt“ verwenden, um eine Verbindung mit den Computern herzustellen. 

- [Referenzarchitektur für DevTest Labs](devtest-lab-reference-architecture.md#architecture)
- [Grundlegendes zu SSO mit Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

**Azure Security Center-Überwachung:** Nicht verfügbar

**Verantwortlichkeit:** Kunde

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Verwenden der mehrstufigen Authentifizierung für den gesamten Azure Active Directory-basierten Zugriff
**Leitfaden**: Aktivieren Sie die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) für Azure Active Directory (AAD), und befolgen Sie die Empfehlungen für die Identitäts- und Zugriffsverwaltung in Azure Security Center.

- [Aktivieren von MFA in Azure](../active-directory/authentication/howto-mfa-getstarted.md)  
- [Überwachen von Identität und Zugriff in Azure Security Center](../security-center/security-center-identity-access.md)

**Azure Security Center-Überwachung**:* Ja

**Verantwortlichkeit:** Kunde


### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Verwenden dedizierter Computer (Arbeitsstationen mit privilegiertem Zugriff) für alle administrativen Aufgaben
**Leitfaden**: Verwenden Sie Arbeitsstationen mit privilegiertem Zugriff (Privileged Access Workstations, PAWs) mit MFA, die für die Anmeldung bei und die Konfiguration von Azure-Ressourcen konfiguriert sind.

- [Informationen zu Arbeitsstationen mit privilegiertem Zugriff](/windows-server/identity/securing-privileged-access/privileged-access-workstations)  
- [Aktivieren von MFA in Azure](../active-directory/authentication/howto-mfa-getstarted.md)  

**Azure Security Center-Überwachung:** –

**Verantwortlichkeit:** Kunde

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7: Protokollieren und Warnen bei verdächtigen Aktivitäten in Administratorkonten
**Leitfaden**: Verwenden Sie Azure Active Directory-Sicherheitsberichte (Azure AD) für die Generierung von Protokollen und Warnungen bei verdächtigen oder sicherheitsrelevanten Aktivitäten in der Umgebung. Verwenden Sie Azure Security Center zum Überwachen von identitäts- und zugriffsbezogenen Aktivitäten.

- [Identifizieren von Azure AD-Benutzern, die aufgrund riskanter Aktivitäten gekennzeichnet wurden](../active-directory/identity-protection/overview-identity-protection.md)  
- [Überwachen der identitäts- und zugriffsbezogenen Aktivitäten von Benutzern in Azure Security Center](../security-center/security-center-identity-access.md)  

**Azure Security Center-Überwachung:** Zurzeit nicht verfügbar

**Verantwortlichkeit:** Kunde

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Verwalten von Azure-Ressourcen nur über genehmigte Standorte
**Leitfaden**: Verwenden Sie benannte Standorte mit bedingtem Zugriff, um den Zugriff nur über bestimmte logische Gruppierungen von IP-Adressbereichen oder Ländern/Regionen zuzulassen.

- [Konfigurieren benannter Standorte in Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)  

**Azure Security Center-Überwachung:** Zurzeit nicht verfügbar

**Verantwortlichkeit:** Kunde

### <a name="39-use-azure-active-directory"></a>3.9: Verwenden von Azure Active Directory
**Leitfaden**: Verwenden Sie Azure Active Directory (Azure AD) als zentrales Authentifizierungs- und Autorisierungssystem. Azure AD schützt Daten durch eine starke Verschlüsselung für ruhende und übertragene Daten. Außerdem werden in Azure AD Salts und Hashs verwendet, und Anmeldeinformationen werden sicher gespeichert.

- [Erstellen und Konfigurieren einer Azure AD-Instanz](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)  

**Azure Security Center-Überwachung:** Zurzeit nicht verfügbar

**Verantwortlichkeit:** Kunde

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Regelmäßiges Überprüfen und Abstimmen des Benutzerzugriffs
**Leitfaden**: Azure Active Directory (Azure AD) enthält Protokolle zum Ermitteln von veralteten Konten. Verwenden Sie außerdem Zugriffsüberprüfungen für Azure-Identitäten, um Gruppenmitgliedschaften, den Zugriff auf Unternehmensanwendungen sowie Rollenzuweisungen effizient zu verwalten. Der Benutzerzugriff kann regelmäßig überprüft werden, um sicherzustellen, dass nur die richtigen Benutzer weiterhin Zugriff haben.

- [Grundlegendes zur Azure AD-Berichterstellung](../active-directory/reports-monitoring/overview-reports.md)  
- [Was sind Azure AD-Zugriffsüberprüfungen?](../active-directory/governance/access-reviews-overview.md)  

**Azure Security Center-Überwachung:** Nicht verfügbar

**Verantwortlichkeit:** Kunde

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11: Überwachen von Zugriffsversuchen auf deaktivierte Konten
**Leitfaden**: Sie haben Zugriff auf die Protokollquellen von Azure Active Directory-Anmeldeaktivitäten (Azure AD) und von Überwachungs- und Risikoereignissen, sodass die Integration in jedes SIEM- (Security Information and Event Management) und Überwachungstool möglich ist.

Sie können diesen Prozess optimieren, indem Sie Diagnoseeinstellungen für Azure Active Directory-Benutzerkonten erstellen und die Überwachungs- und Anmeldeprotokolle an einen Log Analytics-Arbeitsbereich senden. Sie können Warnungen im Log Analytics-Arbeitsbereich konfigurieren.

- [Integrieren von Azure-Aktivitätsprotokollen in Azure Monitor](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)  

**Azure Security Center-Überwachung:** Zurzeit nicht verfügbar

**Verantwortlichkeit:** Kunde

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Warnung bei abweichendem Verhalten bei der Kontoanmeldung
**Leitfaden**: Mit Azure Active Directory-Funktionen (Azure AD) zum Risiko- und Identitätsschutz können Sie automatische Antworten auf erkannte verdächtige Aktionen im Zusammenhang mit Benutzeridentitäten konfigurieren.

- [Anzeigen riskanter Azure AD-Anmeldungen](../active-directory/identity-protection/overview-identity-protection.md)  
- [Konfigurieren und Aktivieren von Risikorichtlinien für den Identitätsschutz](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)  

**Azure Security Center-Überwachung:** Zurzeit nicht verfügbar

**Verantwortlichkeit:** Kunde

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Ermöglichen des Zugriffs auf relevante Kundendaten für Microsoft in Supportszenarien
**Leitfaden**: Die Kunden-Lockbox wird für Azure DevTest Labs derzeit nicht unterstützt.

- [Unterstützte Dienste und Szenarios bei allgemeiner Verfügbarkeit](../security/fundamentals/customer-lockbox-overview.md#supported-services-and-scenarios-in-general-availability) 

**Azure Security Center-Überwachung:** Nicht verfügbar

**Verantwortlichkeit:** Kunde

## <a name="vulnerability-management"></a>Verwaltung von Sicherheitsrisiken
*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Verwaltung von Sicherheitsrisiken](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Ausführen automatisierter Scan-Tools für Sicherheitsrisiken
**Leitfaden**: Befolgen Sie die Empfehlungen im Azure Security Center zum Schützen Ihrer Azure DevTest Labs-Instanzen und damit verbundenen Ressourcen.

Microsoft führt die Verwaltung der Sicherheitsrisiken für die zugrunde liegenden Ressourcen aus, die Azure DevTest Labs unterstützen.

- [Grundlegendes zu Azure Security Center-Empfehlungen](../security-center/recommendations-reference.md) 

**Azure Security Center-Überwachung:** Ja

**Verantwortlichkeit:** Shared

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Bereitstellen der automatisierten Lösung für die Patchverwaltung von Betriebssystemen
**Leitfaden**: Verwenden Sie die Azure-Updateverwaltung, um sicherzustellen, dass auf Ihren Windows- und Linux-VMs, die in DevTest Labs gehostet werden, die aktuellen Sicherheitsupdates installiert sind. Stellen Sie bei virtuellen Windows-Computern sicher, dass Windows Update aktiviert wurde und auf „Automatisch Aktualisieren“ festgelegt ist. Diese Einstellung ist derzeit nicht für die Konfiguration über DevTest Labs verfügbar, aber der Lab-Administrator/Abonnementadministrator kann diese Einstellung auf den zugrunde liegenden Compute-VMs in seinem Abonnement konfigurieren. 

- [Konfigurieren der Updateverwaltung für VMs in Azure](../automation/automation-update-management.md)
- [Grundlegendes zu von Security Center überwachten Azure-Sicherheitsrichtlinien](../security-center/security-center-policy-definitions.md)

**Azure Security Center-Überwachung:** Nicht verfügbar

**Verantwortlichkeit:** Kunde

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3: Bereitstellen der automatisierten Lösung für die Patchverwaltung von Drittanbietersoftware
***Leitfaden:*** Als Lab-Administrator können Sie [DevTest Labs-Artefakte](add-artifact-vm.md) verwenden, um Updates für benutzerdefinierte Lab-Images (einschließlich Sicherheitspatches und anderer Updates) zu automatisieren. 

Erfahren Sie mehr über [DevTest Labs Image Factory](image-factory-create.md), eine Konfiguration-als-Code-Lösung, die Images automatisch in regelmäßigen Abständen mit allen gewünschten Konfigurationen erstellt und verteilt. 

Als Abonnementadministrator können Sie die Azure-Updateverwaltung verwenden, um Updates und Patches für DevTest Labs-VMs zu verwalten. In der Updateverwaltung wird das lokal konfigurierte Updaterepository verwendet, um unterstützte Windows-Systeme zu patchen. Mit Tools wie System Center Updates Publisher (Updates Publisher) können Sie benutzerdefinierte Updates in Windows Server Update Services (WSUS) veröffentlichen. Dadurch kann die Updateverwaltung Computer, auf denen Configuration Manager als Updaterepository verwendet wird, mit Software von Drittanbietern patchen.

- [Updateverwaltung in Azure](../automation/automation-update-management.md)
- [Verwalten von Updates und Patches für Ihre Azure-VMs](../automation/automation-tutorial-update-management.md)

**Azure Security Center-Überwachung:** Nicht verfügbar

**Verantwortlichkeit:** Kunde

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4 Vergleichen von kaskadierenden Sicherheitsrisikoscans
**Leitfaden**: Exportieren Sie die Scanergebnisse in regelmäßigen Abständen und vergleichen Sie die Ergebnisse, um zu überprüfen, ob die Sicherheitsrisiken behoben wurden. Wenn der Kunde die vom Azure Security Center vorgeschlagenen Empfehlungen zur Verwaltung von Sicherheitsrisiken verwendet, kann er in das Portal der ausgewählten Lösung wechseln, um den Verlauf der Scandaten anzuzeigen.

**Azure Security Center-Überwachung:** Nicht zutreffend

**Verantwortlichkeit:** Kunde

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Verwenden eines Risikobewertungsprozesses, um die Behebung von erkannten Sicherheitsrisiken zu priorisieren
**Leitfaden**: Verwenden Sie die Standardrisikobewertungen (Secure Score), die von Azure Security Center bereitgestellt werden.

- [Grundlegendes zum Secure Score von Azure Security Center](../security-center/secure-score-security-controls.md)

**Azure Security Center-Überwachung:** Ja

**Verantwortlichkeit:** Kunde

## <a name="inventory-and-asset-management"></a>Bestands- und Ressourcenverwaltung
*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Bestands- und Ressourcenverwaltung](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Verwenden der automatisierten Asset Discovery-Lösung
**Leitfaden**: Verwenden Sie Azure Resource Graph, um alle Ressourcen (einschließlich DevTest Labs-Ressourcen) in Ihren Abonnements abzufragen und zu ermitteln. Stellen Sie sicher, dass Sie über entsprechende (Lese-)Berechtigungen in Ihrem Mandanten verfügen und alle Azure-Abonnements und Ressourcen in Ihren Abonnements auflisten können.

- [Erstellen von Abfragen mit Azure Graph](../governance/resource-graph/first-query-portal.md)
- [Anzeigen Ihrer Azure-Abonnements](/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)
- [Grundlegendes zu Azure RBAC](../role-based-access-control/overview.md)

**Azure Security Center-Überwachung:** Nicht zutreffend

**Verantwortlichkeit:** Kunde

### <a name="62-maintain-asset-metadata"></a>6.2: Verwalten von Ressourcenmetadaten
**Leitfaden**: Wenden Sie Tags auf Ihre Azure-Ressourcen an, die Metadaten erzeugen, um sie einer Taxonomie entsprechend logisch zu organisieren.

- [Erstellen und Verwenden von Tags](../azure-resource-manager/management/tag-resources.md)
- [Konfigurieren von Tags für DevTest Labs](devtest-lab-add-tag.md)

**Azure Security Center-Überwachung:** Nicht verfügbar.

**Verantwortlichkeit:** Kunde

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Löschen nicht autorisierter Azure-Ressourcen
**Leitfaden**: Verwenden Sie nach Bedarf Tags, Verwaltungsgruppen und separate Abonnements sowie separate Labs, um Ihre Labs und die zugehörigen Lab-Ressourcen zu organisieren und nachzuverfolgen. Stimmen Sie den Bestand regelmäßig ab, und stellen Sie sicher, dass nicht autorisierte Ressourcen schnell aus dem Abonnement gelöscht werden.

- [Erstellen zusätzlicher Azure-Abonnements](../cost-management-billing/manage/create-subscription.md)
- [Erstellen von Verwaltungsgruppen](../governance/management-groups/create.md)
- [Erstellen eines Labs mithilfe von DevTest Labs](devtest-lab-create-lab.md)
- [Erstellen und Verwenden von Tags](../azure-resource-manager/management/tag-resources.md)
- [Konfigurieren von Tags für ein Lab](devtest-lab-add-tag.md)

**Azure Security Center-Überwachung:** Nicht zutreffend

**Verantwortlichkeit:** Kunde

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4: Definieren und Verwalten des Bestands an genehmigten Azure-Ressourcen
**Leitfaden**: Erstellen Sie einen Bestand an genehmigten Azure-Ressourcen und genehmigter Software für Computeressourcen entsprechend den Anforderungen Ihrer Organisation. Als Abonnementadministrator können Sie auch adaptive Anwendungssteuerungen verwenden, ein Feature von Azure Security Center, mit dem Sie eine Gruppe von Anwendungen definieren können, deren Ausführung auf konfigurierten Gruppen von Lab-Computern zulässig ist. Dieses Feature steht sowohl für Windows- (alle Versionen, klassisch oder Azure Resource Manager) und Linux-Computer und -Server mit oder ohne Azure zur Verfügung.

- [Aktivieren adaptiver Anwendungssteuerungen](../security-center/security-center-adaptive-application.md)
 
**Azure Security Center-Überwachung:** Nicht zutreffend **Verantwortlichkeit:** Kunde

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Überwachung auf nicht genehmigte Azure-Ressourcen
**Leitfaden**: Verwenden Sie eine Azure-Richtlinie, um Einschränkungen für den Typ der Ressourcen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie dazu die folgenden integrierten Richtliniendefinitionen:

- Not allowed resource types (Unzulässige Ressourcentypen)
- Zulässige Ressourcentypen

Verwenden Sie außerdem Azure Resource Graph, um Ressourcen in Ihren Abonnements abzufragen und zu ermitteln. Bei Umgebungen mit hohen Sicherheitsanforderungen wie Storage-Konten kann dies nützlich sein.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)
- [Erstellen von Abfragen mit Azure Graph](../governance/resource-graph/first-query-portal.md)

**Azure Security Center-Überwachung:** Nicht zutreffend

**Verantwortlichkeit:** Kunde

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Überwachen auf nicht genehmigte Softwareanwendungen innerhalb von Computeressourcen
**Leitfaden**: Mit Azure Automation haben Sie in Bezug auf die Bereitstellung, Abläufe und Außerbetriebnahme von Workloads und Ressourcen die volle Kontrolle. Als Abonnementadministrator können Sie den Azure Virtual Machine-Bestand verwenden, um die Erfassung von Informationen zur gesamten Software auf DevTest Labs-VMs in Ihrem Abonnement zu automatisieren. Der Softwarename, die Version, der Herausgeber und die Aktualisierungszeit sind im Azure-Portal verfügbar. Kunden müssen die Diagnose auf Gastebene aktivieren und die Windows-Ereignisprotokolle einem Log Analytics-Arbeitsbereich hinzufügen, um auf das Installationsdatum und andere Informationen zugreifen zu können.

Zusätzlich zur Änderungsnachverfolgung für die Überwachung von Softwareanwendungen verwenden adaptive Anwendungssteuerungen in Azure Security Center maschinelles Lernen, um die auf Ihren Computern ausgeführten Anwendungen zu analysieren und aus diesen Informationen eine Zulassungsliste zu erstellen. Dadurch können Sie Richtlinien für Anwendungszulassungsliste deutlich einfacher konfigurieren und verwalten. Auf diese Weise vermeiden Sie, dass in Ihrer Umgebung unerwünschte Software eingesetzt wird. Sie können den Überwachungsmodus oder den Erzwingungsmodus konfigurieren. Im Überwachungsmodus werden nur die Aktivitäten auf den geschützten VMs überwacht. Im Erzwingungsmodus werden die Regeln erzwungen, und es wird sichergestellt, dass Anwendungen, deren Ausführung unzulässig ist, blockiert werden. 

- [Einführung in Azure Automation](../automation/automation-intro.md)
- [Aktivieren des Azure-VM-Bestands](../automation/automation-tutorial-installed-software.md)
- [Grundlegendes zur adaptiven Anwendungssteuerung](../security-center/security-center-adaptive-application.md)

**Azure Security Center-Überwachung:** Nicht zutreffend

**Verantwortlichkeit:** Kunde


### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Entfernen nicht genehmigter Azure-Ressourcen und Softwareanwendungen
**Leitfaden**: Mit Azure Automation haben Sie in Bezug auf die Bereitstellung, Abläufe und Außerbetriebnahme von Workloads und Ressourcen die volle Kontrolle. Als Abonnementadministrator können Sie Änderungsnachverfolgung verwenden, um die gesamte Software zu identifizieren, die auf in DevTest Labs gehosteten VMs installiert ist. Sie können einen eigenen Prozess implementieren oder Azure Automation State Configuration verwenden, um nicht autorisierte Software zu entfernen.

- [Einführung in Azure Automation](../automation/automation-intro.md)
- [Nachverfolgen von Änderungen in Ihrer Umgebung mit der Änderungsnachverfolgung](../automation/change-tracking.md)
- [Übersicht über Azure Automation State Configuration](../automation/automation-dsc-overview.md)

**Azure Security Center-Überwachung:** Nicht verfügbar.

**Verantwortlichkeit:** Kunde

### <a name="68-use-only-approved-applications"></a>6.8: Ausschließliche Verwendung genehmigter Anwendungen
**Leitfaden**: Als Abonnementadministrator können Sie die adaptiven Anwendungssteuerungen in Azure Security Center nutzen, um sicherzustellen, dass nur autorisierte Software ausgeführt und die Ausführung jeglicher nicht autorisierter Software auf Azure-VMs blockiert wird, die in DevTest Labs gehostet werden.

- [Verwenden der adaptiven Anwendungssteuerungen in Azure Security Center](../security-center/security-center-adaptive-application.md)

**Azure Security Center-Überwachung:** Ja

**Verantwortlichkeit:** Kunde

### <a name="69-use-only-approved-azure-services"></a>6.9: Ausschließliche Verwendung genehmigter Azure-Dienste
**Leitfaden**: Verwenden Sie eine Azure-Richtlinie, um Einschränkungen für den Typ der Ressourcen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie dazu die folgenden integrierten Richtliniendefinitionen: 

- Not allowed resource types (Unzulässige Ressourcentypen)
- Zulässige Ressourcentypen

Weitere Informationen finden Sie in folgenden Artikeln: 
- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)
- [Ablehnen eines bestimmten Ressourcentyps mit Azure Policy](../governance/policy/samples/not-allowed-resource-types.md)

**Azure Security Center-Überwachung:** Ja

**Verantwortlichkeit:** Kunde


### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6.10: Verwalten eines Bestands an genehmigten Softwaretiteln
**Leitfaden**: Die adaptive Anwendungssteuerung ist eine intelligente, automatisierte End-to-End-Lösung von Azure Security Center, mit der Sie steuern können, welche Anwendungen auf Ihren Computern mit oder ohne Azure ausgeführt werden können (Windows und Linux), die in DevTest Labs gehostet werden. Beachten Sie, dass Sie ein Abonnementadministrator sein müssen, um diese Einstellung für die zugrunde liegenden Computeressourcen konfigurieren zu können, die in DevTest Labs gehostet werden. Implementieren Sie eine Drittanbieterlösung, wenn diese Einstellung die Anforderung Ihrer Organisation nicht erfüllt.

- [Verwenden der adaptiven Anwendungssteuerungen in Azure Security Center](../security-center/security-center-adaptive-application.md)

**Azure Security Center-Überwachung:** Nicht zutreffend

**Verantwortlichkeit:** Kunde

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Einschränken der Möglichkeiten von Benutzern zur Interaktion mit Azure Resource Manager
**Leitfaden**: Verwenden Sie den bedingten Azure-Zugriff, um die Möglichkeiten der Benutzer zur Interaktion mit Azure Resource Manager einzuschränken, indem Sie **Zugriff blockieren**** für die App zur **Verwaltung von Microsoft Azure** konfigurieren.

- [Verwalten des Zugriffs auf die Azure-Verwaltung mit bedingtem Zugriff](../role-based-access-control/conditional-access-azure-management.md)

**Azure Security Center-Überwachung:** Ja

**Verantwortlichkeit:** Kunde

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: Einschränken der Möglichkeiten der Benutzer, Skripte innerhalb von Computeressourcen auszuführen
**Leitfaden**: Abhängig vom Skripttyp können Sie betriebssystemspezifische Konfigurationen oder Ressourcen von Drittanbietern verwenden, um die Möglichkeit der Benutzer zur Skriptausführung innerhalb der VMs einzuschränken, die in DevTest Labs gehostet werden. Sie können auch die adaptiven Anwendungssteuerungen in Azure Security Center verwenden, um sicherzustellen, dass nur autorisierte Software ausgeführt und die Ausführung jeglicher nicht autorisierter Software auf den zugrunde liegenden Azure-VMs blockiert wird.

- [Steuern der PowerShell-Skriptausführung in Windows-Umgebungen](/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6)
- [Verwenden der adaptiven Anwendungssteuerungen in Azure Security Center](../security-center/security-center-adaptive-application.md)

**Azure Security Center-Überwachung:** Nicht verfügbar.

**Verantwortlichkeit:** Kunde


### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Physische oder logische Trennung von Anwendungen mit hohem Risiko
**Leitfaden**: Anwendungen mit hohem Risiko, die in ihrer Azure-Umgebung bereitgestellt werden, können mithilfe von virtuellen Netzwerken, Subnetzen, Abonnements, Verwaltungsgruppen usw. isoliert werden. Sie können durch eine Azure Firewall, eine Web Application Firewall (WAF) oder eine Netzwerksicherheitsgruppe (NSG) ausreichend geschützt werden.

- [Konfigurieren des virtuellen Netzwerks für DevTest Labs](devtest-lab-configure-vnet.md)
- [Übersicht über Azure Firewall](../firewall/overview.md)
- [Übersicht über Web Application Firewall](../web-application-firewall/overview.md)
- [Übersicht über die Netzwerksicherheit](../virtual-network/security-overview.md)
- [Übersicht über Azure Virtual Network]()
- [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../governance/management-groups/overview.md)
- [Leitfaden zur Entscheidungsfindung für Abonnements](/azure/cloud-adoption-framework/decision-guides/subscriptions/)

**Azure Security Center-Überwachung:** Nicht verfügbar.

**Verantwortlichkeit:** Kunde


## <a name="malware-defense"></a>Schutz vor Schadsoftware
*Weitere Informationen finden Sie unter Sicherheitskontrolle: Schutz vor Schadsoftware.*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1: Verwenden einer zentral verwalteten Antischadsoftware
**Leitfaden**: Verwenden Sie Microsoft Antimalware für Azure Cloud Services und Virtual Machines, um Ihre Ressourcen kontinuierlich zu überwachen und zu schützen. Verwenden Sie für Linux eine Antischadsoftware von Drittanbietern. Mithilfe der Bedrohungserkennung für Datendienste in Azure Security Center können Sie auch Schadsoftware erkennen, die in Speicherkonten hochgeladen wurde.

- Konfigurieren von Microsoft Antimalware für Cloud Services und Virtual Machines
- Bedrohungsschutz in Azure Security Center

**Azure Security Center-Überwachung:** Ja

**Verantwortlichkeit:** Kunde

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: Vorabprüfen von in computefremde Azure-Ressourcen hochzuladenden Dateien
**Leitfaden**: Microsoft Antimalware ist auf dem zugrunde liegenden Host für die Azure-Dienste (z. B. Azure App Service, gehostet in einem Lab) aktiviert, wird jedoch nicht für Ihre Inhalte ausgeführt.
Führen Sie eine Vorabprüfung aller Dateien durch, die in computefremde Azure-Ressourcen hochgeladen werden sollen, z. B. in App Service, Data Lake Storage, Blob Storage usw.

Mithilfe der Bedrohungserkennung für Datendienste in Azure Security Center können Sie Schadsoftware erkennen, die in Speicherkonten hochgeladen wurde.

- Grundlegendes zu Microsoft Antimalware für Azure Cloud Services und Virtual Machines
- Grundlegendes zur Bedrohungserkennung für Datendienste in Azure Security Center

**Azure Security Center-Überwachung:** Ja

**Verantwortlichkeit:** Nicht verfügbar


### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>Schritt 8.3: Sicherstellen der Aktualisierung von Antischadsoftware und Signaturen
**Leitfaden**: Mit einer bereitgestellten Microsoft Antimalware-Instanz werden standardmäßig die neuesten Signaturupdates, Plattformupdates und Engine-Updates automatisch installiert. Befolgen Sie die Empfehlungen in Azure Security Center: „Compute und Apps“, um sicherzustellen, dass alle Endpunkte für zugrunde liegende DevTest Labs-Computeressourcen mit den neuesten Signaturen auf dem aktuellen Stand sind. Das Windows-Betriebssystem kann durch zusätzliche Sicherheitsmaßnahmen noch besser geschützt werden, indem das Risiko durch Viren oder auf Schadsoftware basierende Angriffe mit dem Microsoft Defender Advanced Threat Protection-Dienst begrenzt wird, der mit Azure Security Center integriert ist.

- Bereitstellen von Microsoft Antimalware für Azure Cloud Services und Virtual Machines
- Microsoft Defender Advanced Threat Protection

**Azure Security Center-Überwachung:** Ja

**Verantwortlichkeit:** Kunde

## <a name="data-recovery"></a>Datenwiederherstellung
*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Datenwiederherstellung](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Sicherstellen regelmäßiger automatisierter Sicherungen
**Leitfaden**: Zurzeit unterstützt Azure DevTest Labs keine VM-Sicherungen und -Momentaufnahmen. Sie können jedoch Azure Backup für die zugrunde liegenden Azure-VMs aktivieren und konfigurieren, die in DevTest Labs gehostet werden. Außerdem können Sie die gewünschte Häufigkeit und den Aufbewahrungszeitraum für automatische Sicherungen konfigurieren, solange Sie über den richtigen Zugriff auf die zugrunde liegenden Computeressourcen verfügen. 

- [Übersicht über die Sicherung von Azure-VMs](../backup/backup-azure-vms-introduction.md)
- [Sichern einer Azure-VM über die VM-Einstellungen](../backup/backup-azure-vms-first-look-arm.md)

**Azure Security Center-Überwachung:** Ja

**Verantwortlichkeit:** Kunde

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Durchführen vollständiger Systemsicherungen und Sichern aller kundenseitig verwalteten Schlüssel
**Leitfaden**: Zurzeit unterstützt Azure DevTest Labs keine VM-Sicherungen und -Momentaufnahmen. Sie können jedoch mithilfe von PowerShell oder Rest-APIs Momentaufnahmen Ihrer zugrunde liegenden Azure-VMs erstellen, die in DevTest Labs gehostet werden, oder der verwalteten Datenträger, die mit diesen Instanzen verbunden sind, sofern Sie über den entsprechenden Zugriff auf die zugrunde liegenden Computeressourcen verfügen. Sie können auch alle kundenseitig verwalteten Schlüssel in Azure Key Vault sichern.

Aktivieren Sie Azure Backup auf Azure-Ziel-VMs, und geben Sie die gewünschte Häufigkeit und den Aufbewahrungszeitraum an. Dies umfasst eine vollständige Sicherung des Systemzustands. Wenn Sie Azure Disk Encryption verwenden, werden bei der Azure-VM-Sicherung automatisch auch kundenseitig verwaltete Schlüssel gesichert.

- [Sicherung auf Azure-VMs mit Verschlüsselung](../backup/backup-azure-vms-encryption.md)
- [Übersicht über das Sichern von Azure-VMs](../backup/backup-azure-vms-introduction.md)
- [Übersicht über die Sicherung von Azure-VMs](../backup/backup-azure-vms-introduction.md)
- [Sichern von Key Vault-Schlüsseln in Azure](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure Security Center-Überwachung:** Ja

**Verantwortlichkeit:** Kunde

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Überprüfen aller Sicherungen einschließlich kundenseitig verwalteter Schlüssel
**Leitfaden**: Stellen Sie regelmäßig sicher, dass die Inhalte in Azure Backup wiederhergestellt werden können. Testen Sie die Wiederherstellung von Inhalten ggf. in einem isolierten virtuellen Netzwerk oder Abonnement. Testen Sie außerdem die Wiederherstellung von gesicherten kundenseitig verwalteten Schlüsseln.

Wenn Sie Azure Disk Encryption verwenden, können Sie die Azure-VM mit den Verschlüsselungsschlüsseln für den Datenträger wiederherstellen. Bei Verwendung der Datenträgerverschlüsselung können Sie die Azure-VM mit den Verschlüsselungsschlüsseln für den Datenträger wiederherstellen.

- [Sicherung auf Azure-VMs mit Verschlüsselung](../backup/backup-azure-vms-encryption.md)
- [Wiederherstellen von Dateien aus einer Azure-VM-Sicherung](../backup/backup-azure-restore-files-from-vm.md)
- [Wiederherstellen von Schlüsseltresorschlüsseln in Azure](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)
- [Sichern und Wiederherstellen einer verschlüsselten VM](../backup/backup-azure-vms-encryption.md)

**Azure Security Center-Überwachung:** Nicht zutreffend

**Verantwortlichkeit:** Kunde

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Sicherstellen des Schutzes von Sicherungen und von kundenseitig verwalteten Schlüsseln
**Leitfaden**: Wenn Sie verwaltete Datenträger mit Azure Backup sichern, werden ruhende VMs mit der Speicherdienstverschlüsselung (SSE) verschlüsselt. Azure Backup kann auch virtuelle Azure-Computer sichern, die mit Azure Disk Encryption verschlüsselt wurden. Azure Disk Encryption kann in BitLocker-Verschlüsselungsschlüssel (BEK) integriert werden, die in einem Schlüsseltresor als Geheimnisse geschützt werden. Azure Disk Encryption kann auch in Azure Key Vault-Schlüssel für die Schlüsselverschlüsselung (Key Encryption Keys, KEK) integriert werden. Aktivieren Sie das vorläufige Löschen in Key Vault, um Schlüssel vor dem versehentlichen oder böswilligen Löschen zu schützen.

- [Vorläufiges Löschen für VMs](../backup/soft-delete-virtual-machines.md)
- [Übersicht über die Azure Key Vault-Funktion für vorläufiges Löschen](../key-vault/general/soft-delete-overview.md)

**Azure Security Center-Überwachung:** Ja

**Verantwortlichkeit:** Kunde

## <a name="incident-response"></a>Reaktion auf Vorfälle
*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Reaktion auf Vorfälle](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Erstellen eines Leitfadens für die Reaktion auf Vorfälle
**Leitfaden**: Erarbeiten Sie einen Leitfaden für die Reaktion auf Vorfälle für Ihre Organisation. Stellen Sie sicher, dass es schriftliche Pläne für die Reaktion auf Vorfälle gibt, in denen alle Rollen der Mitarbeiter sowie die Phasen der Bearbeitung und Verwaltung von Vorfällen von der Ermittlung bis zur abschließenden Überprüfung definiert sind.

- [Anleitung zum Entwickeln eines Prozesses für die Reaktion auf Sicherheitsvorfälle](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)
- [Struktur eines Vorfalls laut Microsoft Security Response Center](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)
- [Nutzen des Computer Security Incident Handling Guide des US-amerikanischen National Institute of Standards and Technology (NIST) als Hilfe bei der Entwicklung eines eigenen Plans zur Reaktion auf Vorfälle](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Azure Security Center-Überwachung:** Nicht verfügbar

**Verantwortlichkeit:** Kunde

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Erstellen eines Verfahrens zur Bewertung und Priorisierung von Vorfällen
**Leitfaden**: Azure Security Center weist jeder Warnung einen Schweregrad zu, damit Sie priorisieren können, welche Warnungen zuerst untersucht werden sollen. Der Schweregrad basiert darauf, wie zuversichtlich Security Center in Bezug auf den Befund oder die Analyse ist, die zum Auslösen der Warnung verwendet wird, sowie auf dem Zuverlässigkeitsgrad, dass hinter der Aktivität, die zu der Warnung führte, eine böswillige Absicht stand.

Kennzeichnen Sie außerdem die Abonnements (z. B. Produktion, Nicht-Produktion) mithilfe von Tags, und erstellen Sie ein Namenssystem, um Azure-Ressourcen eindeutig zu identifizieren und zu kategorisieren, insbesondere solche, die vertrauliche Daten verarbeiten. Die Priorisierung der Behebung von Warnungen basierend auf der Wichtigkeit der Azure-Ressourcen und der Umgebung, in der der Vorfall aufgetreten ist, liegt in Ihrer Verantwortung.

- [Sicherheitswarnungen in Azure Security Center](../security-center/security-center-alerts-overview.md)
- [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../azure-resource-manager/management/tag-resources.md)

**Azure Security Center-Überwachung:** Ja

**Verantwortlichkeit:** Kunde

### <a name="103-test-security-response-procedures"></a>10.3: Verfahren zum Testen der Reaktion auf Sicherheitsvorfälle
**Leitfaden**: Führen Sie in regelmäßigen Abständen Übungen durch, um die Reaktionsfähigkeit Ihrer Systeme zu testen und so Ihre Azure-Ressourcen zu schützen. Identifizieren Sie Schwachstellen und Lücken, und überarbeiten Sie den Plan bei Bedarf.

- [NIST-Veröffentlichung: Leitfaden zum Testen, Trainieren und Ausführen von Programmen für IT-Pläne und -Funktionen](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Azure Security Center-Überwachung:** Nicht verfügbar

**Verantwortlichkeit:** Kunde

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Angeben von Kontaktdaten für Sicherheitsvorfälle und Konfigurieren von Warnungsbenachrichtigungen für Sicherheitsvorfälle
**Leitfaden**: Microsoft wendet sich unter den für Sicherheitsvorfälle angegebenen Kontaktdaten an Sie, wenn das Microsoft Security Response Center (MSRC) feststellt, dass Personen unrechtmäßig oder unbefugt auf Ihre Daten zugegriffen haben. Überprüfen Sie die Vorfälle anschließend, um sicherzustellen, dass die Probleme behoben wurden.

- [Festlegen der Kontaktinformationen in Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Azure Security Center-Überwachung:** Nicht verfügbar

**Verantwortlichkeit:** Kunde

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Integrieren von Sicherheitswarnungen in das System zur Reaktion auf Vorfälle
**Leitfaden**: Exportieren Sie die Azure Security Center-Warnungen und -Empfehlungen über die Funktion „Fortlaufender Export“, um Risiken für Azure-Ressourcen zu ermitteln. Über „Fortlaufender Export“ können Sie Warnungen und Empfehlungen entweder manuell oder kontinuierlich exportieren. Sie können den Azure Security Center-Datenconnector verwenden, um die Warnungen an Azure Sentinel zu streamen.

- [Konfigurieren des fortlaufenden Exports](../security-center/continuous-export.md)
- [Streamen von Warnungen in Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Azure Security Center-Überwachung:** Nicht verfügbar

**Verantwortlichkeit:** Kunde

#### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatisieren der Reaktion auf Sicherheitswarnungen
**Leitfaden**: Verwenden Sie die Funktion „Workflowautomatisierung“ in Azure Security Center, um über Logic Apps automatisch Reaktionen auf Sicherheitswarnungen und -empfehlungen auszulösen und dadurch Ihre Azure-Ressourcen noch besser zu schützen.

- [Konfigurieren von Workflowautomatisierung und Logic Apps](../security-center/workflow-automation.md)
 
****Azure Security Center Überwachung:**** Nicht zutreffend

**Verantwortlichkeit:** Kunde


## <a name="penetration-tests-and-red-team-exercises"></a>Penetrationstests und Red Team-Übungen
*Weitere Informationen finden Sie unter Sicherheitskontrolle: [Penetrationstests und Red Team-Übungen](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*


### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11.1: Durchführen regelmäßiger Penetrationstests ihrer Azure-Ressourcen und Sicherstellen der Behebung aller kritischen Sicherheitsergebnisse innerhalb von 60 Tagen
**Leitfaden**: Befolgen Sie die Einsatzregeln von Microsoft, um sicherzustellen, dass Ihre Penetrationstests nicht gegen Microsoft-Richtlinien verstoßen. Nutzen Sie die Microsoft-Strategie und Durchführung von Red Team- und Livewebsite-Penetrationstests für von Microsoft verwaltete Cloudinfrastruktur, Dienste und Anwendungen.

- [Penetrationstests – Rules of Engagement](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)
- [Microsoft Cloud Red Teaming](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Security Center-Überwachung:** Nicht verfügbar

**Verantwortlichkeit:** Shared

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie den folgenden Artikel:

- [Sicherheitswarnungen für Umgebungen in Azure DevTest Labs](environment-security-alerts.md)
