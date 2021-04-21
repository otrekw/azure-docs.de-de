---
title: Azure-Sicherheitsbaseline für Key Vault
description: Die Key Vault-Sicherheitsbaseline enthält Anleitungen zu Verfahren und Ressourcen für die Implementierung der Sicherheitsempfehlungen, die im Azure-Sicherheitsvergleichstest angegeben sind.
author: msmbaldwin
ms.service: key-vault
ms.topic: conceptual
ms.date: 03/29/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 1f56de94df4fd5d4dd154ae8485edb9eed88364c
ms.sourcegitcommit: 6686a3d8d8b7c8a582d6c40b60232a33798067be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107753342"
---
# <a name="azure-security-baseline-for-key-vault"></a>Azure-Sicherheitsbaseline für Key Vault

Diese Sicherheitsbaseline wendet Empfehlungen des [Azure-Sicherheitsvergleichstests Version 1.0](../../security/benchmarks/overview-v1.md) auf Key Vault an. Der Azure-Sicherheitsvergleichstest enthält Empfehlungen zum Schutz Ihrer Cloudlösungen in Azure. Der Inhalt wird anhand der **Sicherheitskontrollen** gruppiert, die durch den Azure-Sicherheitsvergleichstest und die entsprechenden für Key Vault geltenden Empfehlungen definiert sind. Nicht auf Service Key Vault anwendbare **Kontrollen** oder Kontrollen, für die Microsoft die Verantwortung trägt, wurden ausgeschlossen.

Die vollständige Zuordnung von Key Vault zum Azure-Sicherheitsvergleichstest finden Sie in der [vollständigen Zuordnungsdatei der Key Vault -Sicherheitsbaseline](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Netzwerksicherheit

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Netzwerksicherheit](../../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Schützen von Azure-Ressourcen in virtuellen Netzwerken

**Anleitung:** Integrieren Sie Azure Key Vault in Azure Private Link. Mit dem Azure Private Link-Dienst können Sie über einen privaten Endpunkt in Ihrem virtuellen Netzwerk auf Azure-Dienste (z. B. Azure Key Vault) sowie auf in Azure gehostete Kunden-/Partnerdienste zugreifen.

Ein privater Endpunkt in Azure ist eine Netzwerkschnittstelle, die Sie privat und sicher mit einem von Azure Private Link betriebenen Dienst verbindet. Der private Endpunkt verwendet eine private IP-Adresse aus Ihrem VNET und bindet den Dienst dadurch in Ihr VNET ein. Der gesamte für den Dienst bestimmte Datenverkehr kann über den privaten Endpunkt geleitet werden. Es sind also keine Gateways, NAT-Geräte, ExpressRoute-/VPN-Verbindungen oder öffentlichen IP-Adressen erforderlich. Der Datenverkehr zwischen Ihrem virtuellen Netzwerk und dem Dienst wird über das Microsoft-Backbone-Netzwerk übertragen und dadurch vom öffentlichen Internet isoliert. Sie können eine Verbindung mit einer Instanz einer Azure-Ressource herstellen, was ein Höchstmaß an Granularität bei der Zugriffssteuerung ermöglicht.

- [Integrieren von Key Vault in Azure Private Link](/azure/key-vault/private-link-service)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/azure/governance/policy/samples/azure-security-benchmark) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/azure/security-center/security-center-recommendations). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/azure/security-center/azure-defender)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.KeyVault**:

[!INCLUDE [Resource Policy for Microsoft.KeyVault 1.1](../../../includes/policy/standards/asb/rp-controls/microsoft.keyvault-1-1.md)]

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2: Überwachen und Protokollieren der Konfiguration und des Datenverkehrs von virtuellen Netzwerken, Subnetzen und Netzwerkschnittstellen

**Anleitung:** Verwenden Sie Azure Security Center, und befolgen Sie die Empfehlungen für den Netzwerkschutz, um Ihre mit Key Vault konfigurierten Ressourcen in Azure zu schützen. 

- [Weitere Informationen zu der von Azure Security Center bereitgestellten Netzwerksicherheit](../../security-center/security-center-network-recommendations.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Ablehnen der Kommunikation mit bekannten schädlichen IP-Adressen

**Anleitung:** Aktivieren Sie Azure DDoS Protection Standard in den virtuellen Azure-Netzwerken, die Ihren Key Vault-Instanzen zugeordnet sind, um diese vor verteilten Denial-of-Service-Angriffen zu schützen. Verwenden Sie die in Azure Security Center integrierte Threat Intelligence, um die Kommunikation mit bekannten schädlichen oder nicht genutzten IP-Adressen zu verweigern.

 
- [Verwalten von Azure DDoS Protection Standard mithilfe des Azure-Portals](/azure/virtual-network/manage-ddos-protection)

- [Bedrohungserkennung für die Azure-Dienstebene in Azure Security Center](/azure/security-center/security-center-alerts-service-layer)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Bereitstellen von netzwerkbasierten Angriffserkennungs-/Eindringschutzsystemen (Intrusion Detection/Intrusion Prevention Systems, IDS/IPS)

**Anleitung:** Diese Anforderung kann durch Konfigurieren von Advanced Threat Protection (ATP) für Azure Key Vault erfüllt werden. ATP bietet zusätzliche Sicherheitsanalysen für Daten. Dieses Tool erkennt potenziell schädliche Versuche, auf Azure Key Vault-Konten zuzugreifen oder diese missbräuchlich zu nutzen.

Wenn Azure Security Center anomale Aktivitäten erkennt, werden Warnungen angezeigt. Außerdem wird der Administrator des Abonnements per E-Mail über die Details der verdächtigen Aktivität informiert und erhält Empfehlungen, wie er die identifizierten Bedrohungen untersuchen und beheben kann.

- [Einrichten von Advanced Threat Protection für Azure Key Vault](/azure/security-center/advanced-threat-protection-key-vault)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimieren der Komplexität und des Verwaltungsaufwands von Netzwerksicherheitsregeln

**Anleitung:** Verwenden Sie für Ressourcen, die Zugriff auf Ihre Azure Key Vault-Instanzen benötigen, Azure-Diensttags für Azure Key Vault, um Netzwerkzugriffssteuerungen für Netzwerksicherheitsgruppen oder Azure Firewall zu definieren. Sie können Diensttags anstelle von spezifischen IP-Adressen nutzen, wenn Sie Sicherheitsregeln erstellen. Indem Sie den Diensttagnamen (z. B. „ApiManagement“) im entsprechenden Quell- oder Zielfeld einer Regel angeben, können Sie den Datenverkehr für den entsprechenden Dienst zulassen oder verweigern. Microsoft verwaltet die Adresspräfixe, für die das Diensttag gilt, und aktualisiert das Diensttag automatisch, wenn sich die Adressen ändern.

- [Übersicht über Azure-Diensttags](../../virtual-network/service-tags-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Beibehalten von Standardsicherheitskonfigurationen für Netzwerkgeräte

**Anleitung:** Definieren und implementieren Sie mit Azure Policy Standardsicherheitskonfigurationen für Netzwerkressourcen, die Ihren Azure Key Vault-Instanzen zugeordnet sind. Verwenden Sie Azure Policy-Aliase in den Namespaces „Microsoft.KeyVault“ und „Microsoft.Network“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen der Netzwerkkonfiguration Ihrer Azure Key Vault-Instanzen zu erstellen. Sie können auch die integrierten Richtliniendefinitionen für Azure Key Vault verwenden, z. B.:
- Key Vault sollte einen VNET-Dienstendpunkt verwenden.

Weitere Informationen finden Sie in den folgenden Referenzen:

- [Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung](../../governance/policy/tutorials/create-and-manage.md)

- [Azure Policy-Beispiele](/azure/governance/policy/samples)

- [Definieren und Zuweisen einer Blaupause im Portal](../../governance/blueprints/create-blueprint-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="110-document-traffic-configuration-rules"></a>1.10: Dokumentieren von Datenverkehrskonfigurationsregeln

**Anleitung:** Verwenden Sie Tags für Ressourcen im Zusammenhang mit der Netzwerksicherheit und dem Datenfluss für Ihre Azure Key Vault-Instanzen, um Metadaten und eine logische Organisation bereitzustellen.

Verwenden Sie die integrierten Azure-Richtliniendefinitionen in Bezug auf das Tagging, z. B. „Tag und zugehörigen Wert erzwingen“, um sicherzustellen, dass alle Ressourcen mit Tags erstellt werden, und um Sie über nicht getaggte Ressourcen zu informieren.

Sie können Azure PowerShell oder die Azure-Befehlszeilenschnittstelle verwenden, um Ressourcen basierend auf ihren Tags zu suchen oder Aktionen für diese Ressourcen auszuführen.

- [Verwenden von Tags zum Organisieren von Azure-Ressourcen](/azure/azure-resource-manager/resource-group-using-tags)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Verwenden automatisierter Tools zum Überwachen von Netzwerkressourcenkonfigurationen und Erkennen von Änderungen

**Anleitung:** Verwenden Sie das Azure-Aktivitätsprotokoll zum Überwachen der Konfigurationen von Netzwerkressourcen und zum Erkennen von Änderungen bei Netzwerkressourcen, die sich auf Ihre Azure Key Vault-Instanzen beziehen. Erstellen Sie Warnungen in Azure Monitor, die bei Änderungen an wichtigen Netzwerkressourcen ausgelöst werden.

- [Anzeigen und Abrufen von Azure-Aktivitätsprotokollereignissen](/azure/azure-monitor/platform/activity-log-view)

- [Erstellen, Anzeigen und Verwalten von Aktivitätsprotokollwarnungen mit Azure Monitor](/azure/azure-monitor/platform/alerts-activity-log)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="logging-and-monitoring"></a>Protokollierung und Überwachung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Protokollierung und Überwachung](../../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Konfigurieren der zentralen Sicherheitsprotokollverwaltung

**Anleitung:** Erfassen Sie Protokolle über Azure Monitor, um die von Azure Key Vault generierten Sicherheitsdaten zu aggregieren. Verwenden Sie Azure Log Analytics-Arbeitsbereiche in Azure Monitor, um Analysen abzufragen und auszuführen, und verwenden Sie Azure Storage-Konten für die langfristige Speicherung/Archivierung. Alternativ dazu können Sie auch Daten in Azure Sentinel oder einer Drittanbieter-SIEM-Lösung aktivieren und integrieren. 

- [Azure Key Vault-Protokollierung](/azure/key-vault/key-vault-logging)

- [Schnellstart: Ausführen des Onboardings für Azure Sentinel](../../sentinel/quickstart-onboard.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Aktivieren der Überwachungsprotokollierung für Azure-Ressourcen

**Anleitung:** Aktivieren Sie Diagnoseeinstellungen für Ihre Azure Key Vault-Instanzen für den Zugriff auf Überwachungs-, Sicherheits- und Diagnoseprotokolle. Aktivitätsprotokolle, die automatisch verfügbar sind, enthalten Ereignisquelle, Datum, Benutzer, Zeitstempel, Quelladressen, Zieladressen und andere nützliche Elemente.

- [Azure Key Vault-Protokollierung](/azure/key-vault/key-vault-logging)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/azure/governance/policy/samples/azure-security-benchmark) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/azure/security-center/security-center-recommendations). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/azure/security-center/azure-defender)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.KeyVault**:

[!INCLUDE [Resource Policy for Microsoft.KeyVault 2.3](../../../includes/policy/standards/asb/rp-controls/microsoft.keyvault-2-3.md)]

### <a name="25-configure-security-log-storage-retention"></a>2.5: Konfigurieren der Sicherheitsprotokoll-Aufbewahrungsdauer im Speicher

**Anleitung:** Legen Sie in Azure Monitor den Aufbewahrungszeitraum des Log Analytics-Arbeitsbereichs, der für die Azure Key Vault-Protokolle verwendet wird, gemäß den Compliancevorschriften Ihrer Organisation fest. Verwenden Sie Azure Storage-Konten für die langfristige/Archivierungsspeicherung.

- [Ändern des Datenaufbewahrungszeitraums](/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="26-monitor-and-review-logs"></a>2.6: Überwachen und Überprüfen von Protokollen

**Anleitung:** Analysieren und überwachen Sie Protokolle auf anomales Verhalten, und prüfen Sie die Ergebnisse für Ihre durch Azure Key Vault geschützten Ressourcen regelmäßig. Verwenden Sie den Log Analytics-Arbeitsbereich von Azure Monitor, um Protokolle zu überprüfen und Abfragen nach Protokolldaten auszuführen. Alternativ dazu können Sie auch Daten in Azure Sentinel oder einer Drittanbieter-SIEM-Lösung aktivieren und integrieren. 

- [Schnellstart: Ausführen des Onboardings für Azure Sentinel](../../sentinel/quickstart-onboard.md)

- [Erste Schritte mit Log Analytics in Azure Monitor](/azure/azure-monitor/log-query/get-started-portal)

- [Erste Schritte mit Protokollabfragen in Azure Monitor](/azure/azure-monitor/log-query/get-started-queries)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Aktivieren von Warnungen bei anomalen Aktivitäten

**Anleitung:** Aktivieren Sie Advanced Threat Protection (ATP) für Key Vault in Azure Security Center. Aktivieren Sie die Diagnoseeinstellungen in Azure Key Vault, und senden Sie Protokolle an einen Log Analytics-Arbeitsbereich. Integrieren Sie Ihren Log Analytics-Arbeitsbereich in Azure Sentinel, da er eine Lösung für die Sicherheitsorchestrierung mit automatisierter Reaktion (Security Orchestration Automated Response, SOAR) bereitstellt. Dadurch können Playbooks (automatisierte Lösungen) erstellt und zum Beheben von Sicherheitsproblemen verwendet werden.

- [Schnellstart: Ausführen des Onboardings für Azure Sentinel](../../sentinel/quickstart-onboard.md)

- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](../../security-center/security-center-managing-and-responding-alerts.md)

- [Reagieren auf Ereignisse mit Azure Monitor-Warnungen](/azure/azure-monitor/learn/tutorial-response)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="identity-and-access-control"></a>Identität und Zugriffssteuerung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Identität und Zugriffssteuerung](../../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Verwalten eines Bestands von Administratorkonten

**Anleitung:** Verwalten Sie den Bestand Ihrer in Azure Active Directory (Azure AD) registrierten Anwendungen sowie alle Benutzerkonten, die Zugriff auf Ihre Azure Key Vault-Schlüssel, -Geheimnisse und -Zertifikate haben. Sie können das Azure-Portal oder PowerShell verwenden, um den Key Vault Zugriff abzufragen und abzustimmen. Verwenden Sie den folgenden Befehl, um den Zugriff in PowerShell anzuzeigen:

(Get-AzResource -ResourceId [KeyVaultResourceID]).Properties.AccessPolicies

- [Registrierung einer Anwendung mit Azure AD](/azure/key-vault/key-vault-manage-with-cli2#registering-an-application-with-azure-active-directory)

- [Sicherer Zugriff auf einen Schlüsseltresor](security-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Verwenden dedizierter Administratorkonten

**Anleitung:** Erstellen Sie Standardbetriebsvorgänge für die Verwendung dedizierter Administratorkonten, die Zugriff auf Ihre Azure Key Vault-Instanzen haben. Verwenden Sie die Identitäts- und Zugriffsverwaltung in Azure Security Center (derzeit in der Vorschauphase), um die Anzahl der aktiven Administratorkonten zu überwachen.

- [Identität und Zugriff überwachen (Vorschau)](../../security-center/security-center-identity-access.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4: Verwenden des einmaligen Anmeldens (SSO) in Azure Active Directory

**Anleitung:** Verwenden Sie einen Azure-Dienstprinzipal in Verbindung mit AppId, TenantID und ClientSecret, um Ihre Anwendung nahtlos zu authentifizieren und das Token abzurufen, das für den Zugriff auf Ihre Azure Key Vault-Geheimnisse verwendet wird.

- [Dienst-zu-Dienst-Authentifizierung zu Azure Key Vault mit .NET](/azure/key-vault/service-to-service-authentication)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Verwenden der mehrstufigen Authentifizierung für den gesamten Azure Active Directory-basierten Zugriff

**Anleitung:** Aktivieren Sie Azure Active Directory (Azure AD) Multi-Factor Authentication, und befolgen Sie die Empfehlungen zur Identitäts- und Zugriffsverwaltung von Azure Security Center (derzeit in der Vorschauphase), um Ihre Event Hub-fähigen Ressourcen zu schützen.

- [Planen einer Bereitstellung von Azure AD Multi-Factor Authentication](../../active-directory/authentication/howto-mfa-getstarted.md)

- [Identität und Zugriff überwachen (Vorschau)](../../security-center/security-center-identity-access.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3.6: Verwenden von sicheren, von Azure verwalteten Arbeitsstationen für Verwaltungsaufgaben

**Leitfaden**: Verwenden Sie eine Arbeitsstation mit privilegiertem Zugriff (Privileged Access Workstation, PAW) mit Azure AD Multi-Factor Authentication, die für die Anmeldung und Konfiguration Key Vault-fähiger Ressourcen konfiguriert ist.

- [Privileged Access Workstations](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/) 

- [Planen einer cloudbasierten Azure AD Multi-Factor Authentication-Bereitstellung](../../active-directory/authentication/howto-mfa-getstarted.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Protokollieren von und Warnen bei verdächtigen Aktivitäten in Administratorkonten

**Leitfaden**: Verwenden Sie Azure Active Directory Privileged Identity Management (Azure AD/PIM) für die Generierung von Protokollen und Warnungen bei verdächtigen oder sicherheitsrelevanten Aktivitäten in der Umgebung. Verwenden Sie Azure AD-Risikoerkennungen, um Warnungen und Berichte zum Verhalten von Risikobenutzern anzuzeigen. Wenn Sie eine zusätzliche Protokollierung benötigen, senden Sie die Risikoerkennungswarnungen von Azure Security Center an Azure Monitor, und konfigurieren Sie mithilfe von Aktionsgruppen benutzerdefinierte Warnungen/Benachrichtigungen.

Aktivieren Sie Advanced Threat Protection (ATP) für Azure Key Vault, um Warnungen für verdächtige Aktivitäten zu generieren.

- [Bereitstellen von Azure AD Privileged Identity Management (PIM)](../../active-directory/privileged-identity-management/pim-deployment-plan.md)

- [Einrichten von erweitertem Bedrohungsschutz für Azure Key Vault (Vorschau)](/azure/security-center/advanced-threat-protection-key-vault)

- [Warnungen für Azure Key Vault (Vorschau)](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-azurekv)

- [Azure AD-Risikoerkennungen](/azure/active-directory/reports-monitoring/concept-risk-events)

- [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](/azure/azure-monitor/platform/action-groups)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Verwalten von Azure-Ressourcen nur über genehmigte Standorte

**Anleitung:** Konfigurieren Sie die Standortbedingung einer Richtlinie für bedingten Zugriff, und verwalten Sie Ihre benannten Standorte. Mithilfe von benannten Standorten können Sie logische Gruppierungen von IP-Adressbereichen oder Ländern und Regionen erstellen. Sie können den Zugriff auf vertrauliche Ressourcen, z. B. Ihre Key Vault-Geheimnisse, auf die konfigurierten benannten Standorte beschränken.

- [Was sind Standortbedingungen beim bedingten Zugriff in Azure Active (Azure AD) Directory?](../../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="39-use-azure-active-directory"></a>3.9: Verwenden von Azure Active Directory

**Leitfaden**: Verwenden Sie Azure Active Directory (Azure AD) als zentrales Authentifizierungs- und Autorisierungssystem für Azure-Ressourcen wie Key Vault. Dies ermöglicht die Verwendung der rollenbasierten Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC) zum Verwalten vertraulicher Ressourcen.

- [Erstellen eines neuen Tenants in Azure AD](../../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Regelmäßiges Überprüfen und Abstimmen des Benutzerzugriffs

**Leitfaden**: Überprüfen Sie die Azure Active Directory-Protokolle (Azure AD), um veraltete Konten mit administrativen Azure Key Vault-Rollen zu ermitteln. Verwenden Sie zusätzlich Azure AD-Zugriffsüberprüfungen, um Gruppenmitgliedschaften, den Zugriff auf Unternehmensanwendungen, die für den Zugriff auf Azure Key Vault verwendet werden könnten, und Rollenzuweisungen effizient zu verwalten. Der Benutzerzugriff sollte regelmäßig überprüft werden (z. B. alle 90 Tage), um sicherzustellen, dass nur die richtigen Benutzer weiterhin Zugriff haben.

- [Azure AD-Berichte und Überwachungsdokumentation](/azure/active-directory/reports-monitoring/)

- [Was sind Azure AD-Zugriffsüberprüfungen?](../../active-directory/governance/access-reviews-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Überwachen von Zugriffsversuchen auf deaktivierte Anmeldeinformationen

**Anleitung:** Aktivieren Sie die Diagnoseeinstellungen für Azure Key Vault und Azure Active Directory, (Azure AD) und senden Sie alle Protokolle an einen Log Analytics-Arbeitsbereich. Konfigurieren Sie die gewünschten Warnungen (z. B. Versuche, auf deaktivierte Geheimnisse zuzugreifen) in Log Analytics.

- [Integrieren von Azure AD-Protokollen mit Azure Monitor-Protokollen](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

- [Migrieren von der alten Key Vault-Lösung](/azure/azure-monitor/insights/azure-key-vault#migrating-from-the-old-key-vault-solution)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12: Warnung bei abweichendem Verhalten bei der Kontoanmeldung

**Anleitung:** Verwenden Sie die Features Azure Active Directory (Azure AD) Identity Protection und Risikoerkennung, um automatisierte Reaktionen auf erkannte verdächtige Aktionen im Zusammenhang mit Ihren durch Azure Key Vault geschützten Ressourcen zu konfigurieren. Sie sollten automatisierte Antworten über Azure Sentinel aktivieren, um die Sicherheitsmaßnahmen Ihrer Organisation zu implementieren.

- [Bericht „Riskante Anmeldungen“ im Azure Active Directory-Portal](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Vorgehensweise: Konfigurieren und Aktivieren von Risikorichtlinien](../../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Durchführen des Onboardings für Azure Sentinel](../../sentinel/quickstart-onboard.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="data-protection"></a>Datenschutz

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Schutz von Daten](../../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Verwalten eines Bestands an vertraulichen Informationen

**Anleitung:** Verwenden Sie Tags für die Nachverfolgung von Azure-Ressourcen, die vertrauliche Informationen zu Ressourcen, für die Azure Key Vault aktiviert wurde, speichern oder verarbeiten. 

- [Verwenden von Tags zum Organisieren von Azure-Ressourcen](/azure/azure-resource-manager/resource-group-using-tags)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Isolieren von Systemen, die vertrauliche Informationen speichern oder verarbeiten

**Anleitung:** Sie können den Zugriff auf Azure Key Vault schützen, indem Sie VNET-Dienstendpunkte verwenden, die zum Einschränken des Zugriffs auf bestimmte Subnetze konfiguriert sind.

Nachdem Firewallregeln in Kraft sind, können Sie Vorgänge auf der Datenebene von Azure Key Vault nur ausführen, wenn Ihre Anforderungen aus zulässigen Subnetzen oder IP-Adressbereichen stammen. Dies gilt auch für den Zugriff auf Azure Key Vault aus dem Azure-Portal. Obwohl Sie im Azure-Portal zu einem Schlüsseltresor navigieren können, können Sie möglicherweise keine Schlüssel, Geheimnisse oder Zertifikate auflisten, wenn Ihr Clientcomputer nicht in der Zulassungsliste enthalten ist. Dies wirkt sich auch auf die Azure Key Vault-Auswahl und andere Azure-Dienste aus. Sie können möglicherweise eine Liste von Schlüsseltresoren anzeigen, aber keine Schlüssel auflisten, wenn Firewallregeln Ihren Clientcomputer blockieren.

- [Konfigurieren von Azure Key Vault-Firewalls und virtuellen Netzwerken](/azure/key-vault/key-vault-network-security)

- [VNET-Dienstendpunkte für Azure Key Vault](/azure/key-vault/key-vault-overview-vnet-service-endpoints)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Überwachen und Blockieren einer nicht autorisierten Übertragung vertraulicher Informationen

**Anleitung:** Alle in Azure Key Vault gespeicherte Daten werden als vertraulich eingestuft. Verwenden Sie Zugriffssteuerungen für die Azure Key Vault-Datenebene, um den Zugriff auf Azure Key Vault-Geheimnisse zu steuern. Sie können auch die integrierte Firewall von Key Vault verwenden, um den Zugriff auf Netzwerkebene zu steuern. Aktivieren Sie zum Überwachen des Zugriffs auf Azure Key Vault die Key Vault-Diagnoseeinstellungen, und senden Sie die Protokolle an ein Azure Storage-Konto oder einen Log Analytics-Arbeitsbereich.

- [Sicherer Zugriff auf einen Schlüsseltresor](security-overview.md)

- [Konfigurieren von Azure Key Vault-Firewalls und virtuellen Netzwerken](/azure/key-vault/key-vault-network-security)

- [Azure Key Vault-Protokollierung](/azure/key-vault/key-vault-logging)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="46-use-azure-rbac-to-manage-access-to-resources"></a>4.6: Verwenden von Azure RBAC zum Verwalten des Zugriffs auf Ressourcen

**Leitfaden**: Schützen Sie den Zugriff auf die Verwaltungs- und Datenebene Ihrer Azure Key Vault-Instanzen.

- [Sicherer Zugriff auf einen Schlüsseltresor](security-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Protokollieren und Warnen bei Änderungen an wichtigen Azure-Ressourcen

**Anleitung:** Sie können die Azure Key Vault-Analyse-Lösung in Azure Monitor verwenden, um Ereignisprotokolle der Azure Key Vault-Überwachung zu überprüfen.

- [Azure Key Vault-Analyse-Lösung in Azure Monitor](/azure/azure-monitor/insights/azure-key-vault)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="vulnerability-management"></a>Verwaltung von Sicherheitsrisiken

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Verwaltung von Sicherheitsrisiken](../../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Verwenden eines Risikobewertungsprozesses, um die Behebung von erkannten Sicherheitsrisiken zu priorisieren

**Leitfaden**: Verwenden Sie die Standardrisikobewertungen (Secure Score), die von Azure Security Center bereitgestellt werden.

- [Verbessern des Secure Score in Azure Security Center](/azure/security-center/security-center-secure-score)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="inventory-and-asset-management"></a>Bestands- und Ressourcenverwaltung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Bestands- und Ressourcenverwaltung](../../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Verwenden der automatisierten Asset Discovery-Lösung

**Anleitung:** Verwenden Sie Azure Resource Graph, um alle Ressourcen (einschließlich Azure Key Vault-Instanzen) innerhalb Ihres Abonnements abzufragen und zu ermitteln. Stellen Sie entsprechende (Lese-) Berechtigungen in Ihrem Mandanten sicher, und durchlaufen Sie alle Azure-Abonnements sowie Ressourcen in Ihren Abonnements.

- [Ausführen Ihrer ersten Resource Graph-Abfrage mithilfe des Azure Resource Graph-Explorers](../../governance/resource-graph/first-query-portal.md)

- [Abrufen von Abonnements, auf die das aktuelle Konto Zugriff hat](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-4.8.0&amp;preserve-view=true)

- [Was ist die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) für Azure-Ressourcen?](../../role-based-access-control/overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="62-maintain-asset-metadata"></a>6.2: Verwalten von Ressourcenmetadaten

**Anleitung:** Wenden Sie Tags auf Ihre Azure Key Vault-Ressourcen an, die Metadaten erzeugen, um sie logisch in einer Taxonomie zu organisieren.

- [Erstellen und Verwenden von Tags](/azure/azure-resource-manager/resource-group-using-tags)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Löschen nicht autorisierter Azure-Ressourcen

**Anleitung:** Verwenden Sie nach Bedarf Tagging, Verwaltungsgruppen und separate Abonnements zum Organisieren und Nachverfolgen von Azure Key Vault-Instanzen und zugehörigen Ressourcen. Stimmen Sie den Bestand regelmäßig ab, und stellen Sie sicher, dass nicht autorisierte Ressourcen rechtzeitig aus dem Abonnement gelöscht werden.

- [Erstellen eines zusätzlichen Azure-Abonnements](/azure/billing/billing-create-subscription)

- [Erstellen von Verwaltungsgruppen zum Organisieren und Verwalten von Ressourcen](/azure/governance/management-groups/create)

- [Verwenden von Tags zum Organisieren von Azure-Ressourcen](/azure/azure-resource-manager/resource-group-using-tags)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Überwachung auf nicht genehmigte Azure-Ressourcen

**Anleitung:** Verwenden Sie Azure-Richtlinien, um Einschränkungen für den Typ der Ressourcen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie dazu die folgenden integrierten Richtliniendefinitionen:

- Not allowed resource types (Unzulässige Ressourcentypen)

- Zulässige Ressourcentypen

Verwenden Sie darüber hinaus Azure Resource Graph, um Ressourcen in Ihren Abonnements abzufragen und zu ermitteln.

- [Tutorial: Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung](../../governance/policy/tutorials/create-and-manage.md)

- [Schnellstart: Ausführen Ihrer ersten Resource Graph-Abfrage mithilfe des Azure Resource Graph-Explorers](../../governance/resource-graph/first-query-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="69-use-only-approved-azure-services"></a>6.9: Ausschließliche Verwendung genehmigter Azure-Dienste

**Anleitung:** Verwenden Sie Azure-Richtlinien, um Einschränkungen für den Typ der Ressourcen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie dazu die folgenden integrierten Richtliniendefinitionen:

- Not allowed resource types (Unzulässige Ressourcentypen)

- Zulässige Ressourcentypen

Weitere Informationen finden Sie in den folgenden Referenzen:

- [Tutorial: Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung](../../governance/policy/tutorials/create-and-manage.md)

- [Azure Policy-Beispiele](/azure/governance/policy/samples/built-in-policies#general)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Einschränken der Möglichkeiten von Benutzern zur Interaktion mit Azure Resource Manager

**Leitfaden**: Verwenden Sie den bedingten Azure-Zugriff, um die Möglichkeiten der Benutzer zur Interaktion mit Azure Resource Manager einzuschränken, indem Sie „Zugriff blockieren“ für die App zur Verwaltung von Microsoft Azure konfigurieren. Dies kann das Erstellen und Ändern von Ressourcen innerhalb einer Hochsicherheitsumgebung (z. B. mit Key Vault-Konfiguration) verhindern.

- [Verwalten des Zugriffs auf die Azure-Verwaltung mit bedingtem Zugriff](../../role-based-access-control/conditional-access-azure-management.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="secure-configuration"></a>Sichere Konfiguration

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Sichere Konfiguration](../../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Einrichten sicherer Konfigurationen für alle Azure-Ressourcen

**Anleitung:** Verwenden Sie Azure Policy-Aliase im Namespace „Microsoft.KeyVault“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen der Konfiguration Ihrer Azure Key Vault-Instanzen zu erstellen. Sie können auch integrierte Azure Policy-Definitionen für Azure Key Vault verwenden, z. B.:

- Key Vault-Objekte müssen wiederherstellbar sein

- Bereitstellen von Diagnoseeinstellungen für Key Vault im Log Analytics-Arbeitsbereich

- Diagnoseprotokolle in Key Vault sollten aktiviert sein.

- Key Vault sollte einen VNET-Dienstendpunkt verwenden.

- Bereitstellen von Diagnoseeinstellungen für Key Vault in Event Hub

- Nutzen Sie die Empfehlungen von Azure Security Center als Konfigurationsbaseline für den Schutz Ihrer Azure Key Vault-Instanzen.

Weitere Informationen finden Sie in den folgenden Referenzen:

- [Anzeigen verfügbarer Azure Policy-Aliase](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-4.8.0&amp;preserve-view=true)

- [Tutorial: Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung](../../governance/policy/tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Verwalten von sicheren Konfigurationen für Azure-Ressourcen

**Leitfaden**: Verwenden Sie die Azure-Richtlinien [Verweigern] und [Bereitstellen, falls nicht vorhanden], um sichere Einstellungen in den Ressourcen mit aktiviertem Azure Key Vault zu erzwingen. 

- [Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung](../../governance/policy/tutorials/create-and-manage.md)

  
- [Grundlegendes zu Azure Policy-Auswirkungen](../../governance/policy/concepts/effects.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Sicheres Speichern der Konfiguration von Azure-Ressourcen

**Leitfaden**: Wenn Sie für Ihre Ressourcen mit aktiviertem Azure Key Vault benutzerdefinierte Azure Policy-Definitionen verwenden, nutzen Sie Azure Repos, um Code sicher zu speichern und zu verwalten.

- [Speichern von Code in Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops&amp;preserve-view=true)

- [Dokumentation zu Azure Repos](https://docs.microsoft.com/azure/devops/repos/?view=azure-devops&amp;preserve-view=true)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Bereitstellen von Konfigurationsverwaltungstools für Azure-Ressourcen

**Anleitung:** Verwenden Sie Azure Policy-Aliase im Namespace „Microsoft.KeyVault“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen von Systemkonfigurationen zu erstellen und Warnungen zu diesen zu erhalten. Entwickeln Sie außerdem einen Prozess und eine Pipeline zum Verwalten von Richtlinienausnahmen.

- [Konfigurieren und Verwalten von Azure Policy](../../governance/policy/tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementieren der automatisierten Konfigurationsüberwachung für Azure-Ressourcen

**Anleitung:** Verwenden Sie Azure Security Center, um Baselineüberprüfungen der mit Azure Key Vault geschützten Ressourcen durchzuführen.

- [Umsetzen von Empfehlungen in Azure Security Center](../../security-center/security-center-remediate-recommendations.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="711-manage-azure-secrets-securely"></a>7.11: Sicheres Verwalten von Azure-Geheimnissen

**Anleitung:** Verwenden Sie die verwaltete Dienstidentität in Verbindung mit Azure Key Vault, um die Verwaltung von Geheimnissen für Cloudanwendungen zu vereinfachen und zu schützen. Stellen Sie sicher, dass das vorläufige Löschen für Azure Key Vault aktiviert ist.

- [Integrieren mit verwalteten Azure-Identitäten](../../azure-app-configuration/howto-integrate-azure-managed-service-identity.md)

- [Erstellen einer Key Vault-Instanz](quick-create-portal.md)

- [Authentifizieren bei Key Vault](authentication.md)

- [Zuweisen einer Key Vault-Zugriffsrichtlinie](assign-access-policy-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/azure/governance/policy/samples/azure-security-benchmark) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/azure/security-center/security-center-recommendations). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/azure/security-center/azure-defender)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.KeyVault**:

[!INCLUDE [Resource Policy for Microsoft.KeyVault 7.11](../../../includes/policy/standards/asb/rp-controls/microsoft.keyvault-7-11.md)]

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Sicheres und automatisches Verwalten von Identitäten

**Anleitung:** Verwenden Sie die verwaltete Dienstidentität in Verbindung mit Azure Key Vault, um die Verwaltung von Geheimnissen für Cloudanwendungen zu vereinfachen und zu schützen.

  

- [Integrieren mit verwalteten Azure-Identitäten](../../azure-app-configuration/howto-integrate-azure-managed-service-identity.md) 

- [Erstellen einer Key Vault-Instanz](quick-create-portal.md)    

- [Authentifizieren bei Key Vault](authentication.md)

- [Zuweisen einer Key Vault-Zugriffsrichtlinie](assign-access-policy-portal.md)

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

**Anleitung:** Microsoft Antimalware ist auf dem zugrunde liegenden Host für die Azure-Dienste (z. B. Azure Key Vault) aktiviert, wird jedoch nicht für Kundeninhalte ausgeführt.

Inhalte, die in nicht für Compute ausgelegte Azure-Ressourcen wie Azure Key Vault hochgeladen oder an diese gesendet werden, sollten im Voraus überprüft werden. Microsoft kann in diesen Fällen nicht auf Ihre Daten zugreifen.

- [Grundlegendes zu Microsoft Antimalware für Azure Cloud Services und Virtual Machines](../../security/fundamentals/antimalware.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="data-recovery"></a>Datenwiederherstellung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Datenwiederherstellung](../../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Sicherstellen regelmäßiger automatisierter Sicherungen

**Anleitung:** Stellen Sie mithilfe der folgenden PowerShell-Befehle sicher, dass regelmäßig automatisierte Sicherungen Ihrer Key Vault-Zertifikate, -Schlüssel und -Geheimnisse und der verwalteten Speicherkonten erstellt werden:

- Backup-AzKeyVaultCertificate

- Backup-AzKeyVaultKey

- Backup-AzKeyVaultManagedStorageAccount

- Backup-AzKeyVaultSecret

Optional können Sie Ihre Key Vault-Sicherungen in Azure Backup speichern.

- [Sichern von Key Vault-Zertifikaten](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultcertificate?view=azps-5.3.0&amp;preserve-view=true)

- [Sichern von Key Vault-Schlüsseln](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey?view=azps-5.3.0&amp;preserve-view=true)

- [Sichern von verwalteten Key Vault-Speicherkonten](/powershell/module/az.keyvault/add-azkeyvaultmanagedstorageaccount)

- [Sichern von Key Vault-Geheimnissen](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultsecret?view=azps-5.3.0&amp;preserve-view=true)

- [Aktivieren von Azure Backup](/azure/backup)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Durchführen vollständiger Systemsicherungen und Sichern aller kundenseitig verwalteten Schlüssel

**Anleitung:** Erstellen Sie mithilfe der folgenden PowerShell-Befehle Sicherungen Ihrer Key Vault-Zertifikate, -Schlüssel und -Geheimnisse und der verwalteten Speicherkonten:

- Backup-AzKeyVaultCertificate

- Backup-AzKeyVaultKey

- Backup-AzKeyVaultManagedStorageAccount

- Backup-AzKeyVaultSecret

Optional können Sie Ihre Key Vault-Sicherungen in Azure Backup speichern.

- [Sichern von Key Vault-Zertifikaten](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultcertificate?view=azps-5.3.0&amp;preserve-view=true)

- [Sichern von Key Vault-Schlüsseln](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey?view=azps-5.3.0&amp;preserve-view=true)

- [Sichern von verwalteten Key Vault-Speicherkonten](/powershell/module/az.keyvault/add-azkeyvaultmanagedstorageaccount)

- [Sichern von Key Vault-Geheimnissen](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultsecret?view=azps-5.3.0&amp;preserve-view=true)

- [Aktivieren von Azure Backup](/azure/backup)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Überprüfen aller Sicherungen einschließlich kundenseitig verwalteter Schlüssel

**Anleitung:** Führen Sie mithilfe der folgenden PowerShell-Befehle regelmäßig Datenwiederherstellungen Ihrer Key Vault-Zertifikate, -Schlüssel und -Geheimnisse und der verwalteten Speicherkonten durch:

- Restore-AzKeyVaultCertificate

- Restore-AzKeyVaultKey

- Restore-AzKeyVaultManagedStorageAccount

- Restore-AzKeyVaultSecret

Weitere Informationen finden Sie in den folgenden Referenzen:

- [Wiederherstellen von Key Vault-Zertifikaten](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultcertificate?view=azps-5.3.0&amp;preserve-view=true)

- [Wiederherstellen von Key Vault-Schlüsseln](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultkey?view=azps-5.3.0&amp;preserve-view=true)

- [Wiederherstellen von per Key Vault verwalteten Storage-Konten](/powershell/module/az.keyvault/backup-azkeyvaultmanagedstorageaccount)

- [Wiederherstellen von Key Vault-Geheimnissen](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultsecret?view=azps-5.3.0&amp;preserve-view=true)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Sicherstellen des Schutzes von Sicherungen und von kundenseitig verwalteten Schlüsseln

**Anleitung:** Stellen Sie sicher, dass das vorläufige Löschen für Azure Key Vault aktiviert ist. Vorläufiges Löschen ermöglicht die Wiederherstellung gelöschter Schlüsseltresore und Tresorobjekte wie Schlüssel, Geheimnisse und Zertifikate. 

- [Verwenden des vorläufigen Löschens in Azure Key Vault](/azure/key-vault/key-vault-soft-delete-powershell)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/azure/governance/policy/samples/azure-security-benchmark) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/azure/security-center/security-center-recommendations). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/azure/security-center/azure-defender)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.KeyVault**:

[!INCLUDE [Resource Policy for Microsoft.KeyVault 9.4](../../../includes/policy/standards/asb/rp-controls/microsoft.keyvault-9-4.md)]

## <a name="incident-response"></a>Reaktion auf Vorfälle

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Reaktion auf Vorfälle](../../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Erstellen eines Leitfadens für die Reaktion auf Vorfälle

**Anleitung:** Erarbeiten Sie einen Leitfaden für die Reaktion auf Vorfälle für Ihre Organisation. Stellen Sie sicher, dass es schriftliche Pläne für die Reaktion auf Vorfälle gibt, in denen alle Rollen der Mitarbeiter sowie die Phasen der Bearbeitung und Verwaltung von Vorfällen von der Ermittlung bis zur abschließenden Überprüfung definiert sind. Ziel dieser Prozesse sollte der Schutz sensibler Systeme sein, z. B. solcher, die Key Vault-Geheimnisse nutzen.

- [Leitfaden zu Planung und Betrieb](../../security-center/security-center-planning-and-operations-guide.md)   

- [Anleitung zum Entwickeln eines Prozesses für die Reaktion auf Sicherheitsvorfälle](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Struktur eines Vorfalls laut Microsoft Security Response Center](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process)   

- [Kunden können außerdem den Computer Security Incident Handling Guide des US-amerikanischen National Institute of Standards and Technology (NIST) nutzen, um einen Plan zur Reaktion auf Incidents auszuarbeiten.](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Erstellen eines Verfahrens zur Bewertung und Priorisierung von Vorfällen

**Anleitung:** Security Center weist jeder Warnung einen Schweregrad zu, damit Sie priorisieren können, welche Warnungen zuerst untersucht werden sollen. Der Schweregrad basiert darauf, wie sicher Security Center in Bezug auf den, die Warnung auslösenden Befund oder Metrik ist. Darüber hinaus spielt die Verlässlichkeit der Beurteilung, ob hinter der Aktivität, die zu der Warnung führte, eine böswillige Absicht stand, eine Rolle. Kennzeichnen Sie außerdem die Abonnements (z. B. Produktion, Nicht-Produktion), und erstellen Sie ein Namenssystem, um Azure-Ressourcen eindeutig zu identifizieren und zu kategorisieren. Dies gilt insbesondere für solche, die vertrauliche Daten wie Azure Key Vault-Geheimnisse verarbeiten.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="103-test-security-response-procedures"></a>10.3: Verfahren zum Testen der Reaktion auf Sicherheitsvorfälle

**Anleitung:** Führen Sie in regelmäßigen Abständen Übungen durch, um die Reaktionsfähigkeit Ihrer Systeme zu testen und so Ihre Azure Key Vault-Instanzen und die zugehörigen Ressourcen zu schützen. Identifizieren Sie Schwachstellen und Lücken, und überarbeiten Sie den Plan bei Bedarf.

- [„Guide to Test, Training, and Exercise Programs for IT Plans and Capabilities“ des National Institute of Standards and Technology (NIST)](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Angeben von Kontaktdaten für Sicherheitsvorfälle und Konfigurieren von Warnungsbenachrichtigungen für Sicherheitsvorfälle

**Leitfaden**: Microsoft kontaktiert Sie unter den für Sicherheitsvorfälle angegebenen Kontaktdaten, wenn das Microsoft Security Response Center (MSRC) feststellt, dass Personen unrechtmäßig oder unbefugt auf Ihre Daten zugegriffen haben.  Überprüfen Sie die Vorfälle anschließend, um sicherzustellen, dass die Probleme behoben wurden.

- [Festlegen der Kontaktinformationen in Azure Security Center](../../security-center/security-center-provide-security-contact-details.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Integrieren von Sicherheitswarnungen in das System zur Reaktion auf Vorfälle

**Anleitung:** Exportieren Sie die Azure Security Center-Warnungen und -Empfehlungen über die Funktion „Fortlaufender Export“, um Risiken bei Ressourcen zu identifizieren, die Azure Key Vault nutzen. Über „Fortlaufender Export“ können Sie Warnungen und Empfehlungen entweder manuell oder kontinuierlich exportieren.  Sie können den Azure Security Center-Datenconnector verwenden, um die Warnungen an Azure Sentinel zu streamen. 

 

- [Konfigurieren des fortlaufenden Exports](../../security-center/continuous-export.md) 

  

- [Streamen von Warnungen in Azure Sentinel](../../sentinel/connect-azure-security-center.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatisieren der Reaktion auf Sicherheitswarnungen

**Anleitung:** Verwenden Sie die Funktion „Workflowautomatisierung“ in Azure Security Center, um über Logic Apps automatisch Reaktionen auf Sicherheitswarnungen und -empfehlungen auszulösen und dadurch Ihre Ressourcen mit Azure Key Vault noch besser zu schützen. 

 

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
