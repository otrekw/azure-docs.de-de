---
title: Steuerungen des Blaupausenbeispiels für den Vergleichstest für die Azure-Sicherheit
description: Steuerungszuordnung des Blaupausenbeispiels für den Vergleichstest für die Azure-Sicherheit zu Azure Policy
ms.date: 04/16/2020
ms.topic: sample
ms.openlocfilehash: f80b72f06532adef28bf5e5afd1eb94c2e34ee2d
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2020
ms.locfileid: "82691310"
---
# <a name="control-mapping-of-the-azure-security-benchmark-blueprint-sample"></a>Steuerungszuordnung des Blaupausenbeispiels für den Vergleichstest für die Azure-Sicherheit

Im folgenden Artikel wird erläutert, wie das Blaupausenbeispiel für den Vergleichstest für die Azure-Sicherheit von Azure Blueprints den Steuerungen des Vergleichstests für die Azure-Sicherheit zugeordnet wird. Weitere Informationen zu den Steuerungen finden Sie unter [Vergleichstest für die Azure-Sicherheit](../../../../security/benchmarks/overview.md).

Es gibt die folgenden Zuordnungen zu den Steuerungen im **Vergleichstest für die Azure-Sicherheit**. Über den rechten Navigationsbereich können Sie direkt zu einer bestimmten Steuerungszuordnung springen. Die zugeordneten Steuerungen werden mit einer [Azure Policy](../../../policy/overview.md)-Initiative implementiert. Zum Anzeigen der vollständigen Initiative öffnen Sie **Richtlinie** im Azure-Portal und wählen dann die Seite **Definitionen** aus. Suchen Sie anschließend die integrierte Richtlinieninitiative **\[Vorschauversion\]: Azure Security Benchmark-Empfehlungen überwachen und spezifische unterstützende VM-Erweiterungen bereitstellen**.

> [!IMPORTANT]
> Jede Steuerung unten ist einer oder mehreren [Azure Policy](../../../policy/overview.md)-Definitionen zugeordnet. Diese Richtlinien können Ihnen bei der [Konformitätsbewertung](../../../policy/how-to/get-compliance-data.md) mit der Steuerung helfen. Es gibt jedoch oft keine 1:1- oder vollständige Übereinstimmung zwischen einer Steuerung und einer bzw. mehreren Richtlinien. Daher bezieht sich **Konform** in Azure Policy nur auf die Richtlinien selbst und gewährleistet nicht die vollständige Konformität mit allen Anforderungen einer Steuerung. Außerdem kann der Kompatibilitätsstandard Steuerungen enthalten, die derzeit von keiner Azure Policy-Definition abgedeckt werden. Daher ist die Konformität in Azure Policy nur eine partielle Ansicht Ihres gesamten Konformitätsstatus. Die Zuordnungen zwischen Steuerungen und Azure Policy-Definitionen für dieses Konformitätsblaupausenbeispiel können sich im Laufe der Zeit ändern. Den Änderungsverlaufs finden Sie im [GitHub-Commit-Verlauf](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/azure-security-benchmark/control-mapping.md).

## <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1 Schützen von Ressourcen mithilfe von Netzwerksicherheitsgruppen oder Azure Firewall in Virtual Network

- Subnetze sollten einer Netzwerksicherheitsgruppe zugeordnet werden
- Auf virtuelle Computer mit Internetzugang müssen Empfehlungen zur adaptiven Netzwerkhärtung angewendet werden.
- Virtuelle Computer müssen mit einem genehmigten virtuellen Netzwerk verbunden sein.
- Virtuelle Computer mit Internetzugang sollten über Netzwerksicherheitsgruppen geschützt werden.
- Service Bus sollte einen VNET-Dienstendpunkt verwenden.
- App Service sollte einen VNET-Dienstendpunkt verwenden.
- SQL Server sollte einen VNET-Dienstendpunkt verwenden.
- Event Hub sollte einen VNET-Dienstendpunkt verwenden
- Cosmos DB sollte einen VNET-Dienstendpunkt verwenden
- Key Vault sollte einen VNET-Dienstendpunkt verwenden.
- Nicht eingeschränkten Netzwerkzugriff auf Speicherkonten überwachen
- Speicherkonten sollten einen VNET-Dienstendpunkt verwenden
- Container Registry sollte einen VNET-Dienstendpunkt verwenden.
- Virtuelle Netzwerke sollten ein angegebenes Gateway für virtuelle Netzwerke verwenden.
- In Kubernetes Services sollten autorisierte IP-Adressbereiche definiert werden.
- \[Vorschau\]: IP-Weiterleitung für Ihre VM muss deaktiviert sein
- Virtuelle Computer mit Internetzugang sollten über Netzwerksicherheitsgruppen geschützt werden.
- Die Just-In-Time-Netzwerkzugriffssteuerung sollte auf virtuelle Computer angewendet werden.
- Verwaltungsports sollten auf Ihren virtuellen Computern geschlossen werden.

## <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2 Überwachen und Protokollieren der Konfiguration und des Datenverkehrs von VNETs, Subnetzen und Netzwerkkarten (NICs)

- Network Watcher muss aktiviert sein

## <a name="13-protect-critical-web-applications"></a>1.3 Schützen kritischer Webanwendungen

- Stellen Sie sicher, dass „Clientzertifikate (eingehende Clientzertifikate)“ für die Web-App auf „Ein“ festgelegt ist.
- CORS sollte nicht jeder Ressource Zugriff auf Ihre Webanwendungen erteilen.
- CORS sollte nicht jeder Ressource Zugriff auf Ihre Funktions-App erteilen.
- Nicht jeder Ressource den Zugriff auf Ihre API-APP über CORS gestatten
- Remotedebuggen sollte für Webanwendungen deaktiviert sein.
- Remotedebuggen sollte für Funktions-Apps deaktiviert sein.
- Remotedebuggen sollte für API-Apps deaktiviert sein.

## <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4 Ablehnen der Kommunikation mit bekannten bösartigen IP-Adressen

- DDoS Protection Standard sollte aktiviert sein.
- Die Just-In-Time-Netzwerkzugriffssteuerung sollte auf virtuelle Computer angewendet werden.
- Auf virtuelle Computer mit Internetzugang müssen Empfehlungen zur adaptiven Netzwerkhärtung angewendet werden.

## <a name="15-record-network-packets-and-flow-logs"></a>1.5 Aufzeichnen von Netzwerkpaketen und Flussprotokollen

- Network Watcher muss aktiviert sein

## <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11 Verwenden automatisierter Tools zum Überwachen von Netzwerkressourcenkonfigurationen und Erkennen von Änderungen

- Voraussetzungen für die Überwachung von Windows-VM-Konfigurationen in „Sicherheitsoptionen – Netzwerkzugriff“ bereitstellen
- Überwachungsergebnisse aus Windows-VM-Konfigurationen in „Sicherheitsoptionen – Microsoft-Netzwerk (Client)“ anzeigen
- Voraussetzungen für die Überwachung von Windows-VM-Konfigurationen in „Sicherheitsoptionen – Netzwerksicherheit“ bereitstellen
- Überwachungsergebnisse aus Windows-VM-Konfigurationen in „Sicherheitsoptionen – Netzwerksicherheit“ anzeigen
- Voraussetzungen für die Überwachung von Windows-VM-Konfigurationen in „Sicherheitsoptionen – Microsoft-Netzwerk (Server)“ bereitstellen
- Überwachungsergebnisse aus Windows-VM-Konfigurationen in „Sicherheitsoptionen – Microsoft-Netzwerk (Server)“ anzeigen
- Voraussetzungen für die Überwachung von Windows-VM-Konfigurationen in „Administrative Vorlagen – Netzwerk“ bereitstellen
- Überwachungsergebnisse aus Windows-VM-Konfigurationen in „Administrative Vorlagen – Netzwerk“ anzeigen

## <a name="22-configure-central-security-log-management"></a>2.2 Konfigurieren der zentralen Sicherheitsprotokollverwaltung

- Der Log Analytics-Agent sollte auf virtuellen Computern installiert werden.
- Der Log Analytics-Agent sollte in Virtual Machine Scale Sets installiert sein.
- Voraussetzungen für die Überwachung von Windows-VMs bereitstellen, auf denen der Log Analytics-Agent nicht wie erwartet verbunden ist
- Überwachungsergebnisse von Windows-VMs anzeigen, auf denen der Log Analytics-Agent nicht wie erwartet verbunden ist
- Das Azure Monitor-Protokollprofil muss Protokolle für die Kategorien „write“, „delete“ und „action“ erfassen
- Azure Monitor muss Aktivitätsprotokolle aus allen Regionen erfassen
- Für Ihr Abonnement muss die automatische Bereitstellung des Log Analytics-Überwachungs-Agents aktiviert sein

## <a name="23-enable-audit-logging-for-azure-resources"></a>2.3 Aktivieren der Überwachungsprotokollierung für Azure-Ressourcen

- In Azure Data Lake Store sollten Diagnoseprotokolle aktiviert sein.
- In Logic Apps müssen Diagnoseprotokolle aktiviert sein
- In IoT Hub sollten Diagnoseprotokolle aktiviert sein.
- Diagnoseprotokolle in Batch-Konten sollten aktiviert sein.
- In Virtual Machine Scale Sets sollten Diagnoseprotokolle aktiviert sein.
- In Event Hub sollten Diagnoseprotokolle aktiviert sein
- In den Suchdiensten müssen Diagnoseprotokolle aktiviert sein
- In App Services sollten Diagnoseprotokolle aktiviert sein.
- In Data Lake Analytics sollten Diagnoseprotokolle aktiviert sein.
- Diagnoseprotokolle in Key Vault sollten aktiviert sein.
- In Service Bus sollten Diagnoseprotokolle aktiviert sein.
- Diagnoseprotokolle in Azure Stream Analytics sollten aktiviert werden.
- Die Überwachung in SQL Server muss aktiviert werden.
- Überwachen der Diagnoseeinstellung

## <a name="24-collect-security-logs-from-operating-systems"></a>2.4 Erfassen von Sicherheitsprotokollen von Betriebssystemen

- Für Ihr Abonnement muss die automatische Bereitstellung des Log Analytics-Überwachungs-Agents aktiviert sein
- Der Log Analytics-Agent sollte auf virtuellen Computern installiert werden.
- Der Log Analytics-Agent sollte in Virtual Machine Scale Sets installiert sein.
- Voraussetzungen für die Überwachung von Windows-VMs bereitstellen, auf denen der Log Analytics-Agent nicht wie erwartet verbunden ist
- Überwachungsergebnisse von Windows-VMs anzeigen, auf denen der Log Analytics-Agent nicht wie erwartet verbunden ist

## <a name="27-enable-alerts-for-anomalous-activity"></a>2.7 Aktivieren von Warnungen für anormale Aktivitäten

- Standard-Tarif von Security Center muss ausgewählt sein
- Advanced Data Security muss für Ihre SQL-Server aktiviert werden.
- Advanced Data Security muss für Ihre verwalteten SQL-Instanzen aktiviert werden.
- Advanced Threat Protection-Typen sollten in den Advanced Data Security-Einstellungen für SQL Server auf „Alle“ festgelegt werden.
- Advanced Threat Protection-Typen sollten in den Advanced Data Security-Einstellungen für verwaltete SQL-Instanzen auf „Alle“ festgelegt werden.

## <a name="28-centralize-anti-malware-logging"></a>2.8 Zentralisieren der Antischadsoftwareprotokollierung

- Microsoft Antimalware für Azure sollte für die automatische Aktualisierung von Schutzsignaturen konfiguriert sein.
- Fehlenden Endpoint Protection-Schutz in Azure Security Center überwachen
- Die Endpoint Protection-Lösung sollte für VM-Skalierungsgruppen installiert sein.

## <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1 Verwalten eines Bestands von Administratorkonten

- Maximal 3 Besitzer sollten für Ihr Abonnement festgelegt sein.
- Ihrem Abonnement sollte mehr als ein Besitzer zugewiesen sein.
- Externe Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.
- Veraltete Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.

## <a name="33-use-dedicated-administrative-accounts"></a>3.3 Verwenden dedizierter Administratorkonten

- Voraussetzungen für die Überwachung von Windows-VMs bereitstellen, bei denen die Gruppe „Administratoren“ nicht nur die angegebenen Mitglieder enthält
- Überwachungsergebnisse von Windows-VMs anzeigen, bei denen die Gruppe „Administratoren“ nicht nur die angegebenen Mitglieder enthält
- Erforderliche Komponenten zum Überwachen von Windows-VMs bereitstellen, bei denen die Gruppe „Administratoren“ nicht alle der angegebenen Mitglieder enthält
- Überwachungsergebnisse von Windows-VMs anzeigen, bei denen die Gruppe „Administratoren“ nicht alle der angegebenen Mitglieder enthält
- Voraussetzungen für die Überwachung von Windows-VMs bereitstellen, bei denen die Gruppe „Administratoren“ beliebige der angegebenen Mitglieder enthält
- Überwachungsergebnisse von Windows-VMs anzeigen, bei denen die Gruppe „Administratoren“ beliebige der angegebenen Mitglieder enthält
- Maximal 3 Besitzer sollten für Ihr Abonnement festgelegt sein.
- Ihrem Abonnement sollte mehr als ein Besitzer zugewiesen sein.

## <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5 Verwenden der mehrstufigen Authentifizierung für den gesamten Azure Active Directory-basierten Zugriff

- MFA sollte für Konten mit Besitzerberechtigungen in Ihrem Abonnement aktiviert sein.
- MFA sollte für Konten mit Schreibrechten für Ihr Abonnement aktiviert werden
- MFA sollte für Ihre Abonnementkonten mit Leseberechtigungen aktiviert sein

## <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7 Protokollieren und Warnen bei verdächtigen Aktivitäten in Administratorkonten

- Standard-Tarif von Security Center muss ausgewählt sein

## <a name="39-use-azure-active-directory"></a>3.9 Verwenden von Azure Active Directory

- Ein Azure Active Directory-Administrator sollte für SQL-Server-Instanzen bereitgestellt werden
- Service Fabric-Cluster sollten nur Azure Active Directory für die Clientauthentifizierung verwenden.
- Stellen Sie sicher, dass „Registrierung bei Azure Active Directory“ für die API-App aktiviert ist.
- Stellen Sie sicher, dass „Registrierung bei Azure Active Directory“ für die Web-App aktiviert ist.
- Stellen Sie sicher, dass „Registrierung bei Azure Active Directory“ für die Funktions-App aktiviert ist.

## <a name="310-regularly-review-and-reconcile-user-access"></a>3.10 Regelmäßiges Überprüfen und Abstimmen des Benutzerzugriffs

- Veraltete Konten sollten aus Ihrem Abonnement entfernt werden.
- Veraltete Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.
- Externe Konten mit Leseberechtigungen sollten aus Ihrem Abonnement entfernt werden
- Externe Konten mit Schreibberechtigungen sollten aus Ihrem Abonnement entfernt werden.
- Externe Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.

## <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1 Verwalten eines Bestands an vertraulichen Informationen

- Sensible Daten in Ihren SQL-Datenbanken sollten klassifiziert werden.

## <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4 Verschlüsseln aller vertraulichen Informationen während der Übertragung

- Sichere Übertragung in Speicherkonten sollte aktiviert werden.
- In Ihrer API-App sollte die neueste TLS-Version verwendet werden.
- In Ihrer Web-App sollte die neueste TLS-Version verwendet werden.
- In Ihrer Funktions-App sollte die neueste TLS-Version verwendet werden.
- Zugriff auf Funktions-App nur über HTTPS gestatten
- Zugriff auf Webanwendung nur über HTTPS gestatten
- Auf API-Apps sollte nur über HTTPS zugegriffen werden können
- Erzwingen einer SSL-Verbindung muss für MySQL-Datenbankserver aktiviert sein
- Erzwingen einer SSL-Verbindung muss für PostgreSQL-Datenbankserver aktiviert sein
- Nur sichere Verbindungen mit Ihrer Redis Cache-Instanz sollten aktiviert werden

## <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5 Verwenden eines aktiven Ermittlungstools zur Bestimmung vertraulicher Daten

- Sensible Daten in Ihren SQL-Datenbanken sollten klassifiziert werden.
- Advanced Data Security muss für Ihre SQL-Server aktiviert werden.
- Advanced Data Security muss für Ihre verwalteten SQL-Instanzen aktiviert werden.

## <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6 Verwenden von Azure RBAC zum Steuern des Zugriffs auf Ressourcen

- Die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) sollte für Kubernetes Service verwendet werden.
- Verwendung benutzerdefinierter RBAC-Regeln überwachen

## <a name="48-encrypt-sensitive-information-at-rest"></a>4.8 Verschlüsseln vertraulicher, ruhender Informationen

- Transparent Data Encryption für SQL-Datenbanken sollte aktiviert werden.
- Die Datenträgerverschlüsselung sollte auf virtuelle Computer angewendet werden.
- Nicht angefügte Datenträger müssen verschlüsselt werden
- Die TDE-Schutzvorrichtung in SQL Server sollte mit Ihrem eigenen Schlüssel verschlüsselt werden.
- Die TDE-Schutzvorrichtung verwalteter SQL-Instanzen sollte mit Ihrem eigenen Schlüssel verschlüsselt werden.
- Automation-Kontovariablen sollten verschlüsselt werden.
- Service Fabric Cluster sollten die Eigenschaft ClusterProtectionLevel auf EncryptAndSign setzen

## <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9 Protokollieren und Warnen bei Änderungen an wichtigen Azure-Ressourcen

- Azure Monitor muss Aktivitätsprotokolle aus allen Regionen erfassen

## <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1 Ausführen automatisierter Scantools für Sicherheitsrisiken

- Für Ihre SQL Server-Instanzen muss eine Sicherheitsrisikobewertung aktiviert sein.
- Bewertung von Sicherheitsrisiken für verwaltete SQL-Instanzen aktivieren
- \[Vorschau\] Sicherheitsrisikobewertung sollte auf VMs aktiviert sein.
- Die Einstellungen für die Sicherheitsrisikobewertung für SQL Server sollten eine E-Mail-Adresse für den Empfang von Überprüfungsberichten enthalten.

## <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2 Bereitstellen der automatisierten Lösung für die Patchverwaltung von Betriebssystemen

- Systemupdates sollten auf Ihren Computern installiert sein.
- Systemupdates für VM-Skalierungsgruppen sollten installiert werden.
- Sicherstellen, dass die neueste .NET Framework-Version angegeben ist, wenn sie als Teil der Funktions-App verwendet wird
- Sicherstellen, dass die neueste .NET Framework-Version angegeben ist, wenn sie als Teil der Web-App verwendet wird
- Sicherstellen, dass die neueste .NET Framework-Version angegeben ist, wenn sie als Teil der API-App verwendet wird

## <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3 Bereitstellen der automatisierten Lösung für die Patchverwaltung von Drittanbietersoftware

- Stellen Sie sicher, dass die neueste PHP-Version angegeben ist, wenn sie als Teil der API-App verwendet wird.
- Stellen Sie sicher, dass die neueste PHP-Version angegeben ist, wenn sie als Teil der Web-App verwendet wird.
- Stellen Sie sicher, dass die neueste PHP-Version angegeben ist, wenn sie als Teil der Funktions-App verwendet wird.
- Stellen Sie sicher, dass die neueste Java-Version angegeben ist, wenn sie als Teil der Web-App verwendet wird.
- Sicherstellen, dass die neueste Java-Version angegeben ist, wenn sie als Teil der Funktions-App verwendet wird
- Stellen Sie sicher, dass die neueste Java-Version angegeben ist, wenn sie als Teil der API-App verwendet wird.
- Stellen Sie sicher, dass die neueste Python-Version angegeben ist, wenn sie als Teil der Web-App verwendet wird.
- Stellen Sie sicher, dass die neueste Python-Version angegeben ist, wenn sie als Teil der Funktions-App verwendet wird.
- Stellen Sie sicher, dass die neueste Python-Version angegeben ist, wenn sie als Teil der API-App verwendet wird.
- Für Kubernetes Service muss ein Upgrade auf eine Kubernetes-Version ohne Sicherheitsrisiko durchgeführt werden.

## <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5 Verwenden eines Risikobewertungsprozesses, um die Behebung von erkannten Sicherheitsrisiken zu priorisieren

- Sicherheitsrisiken sollten durch eine Lösung zur Sicherheitsrisikobewertung beseitigt werden.
- Sicherheitsrisiken in der Sicherheitskonfiguration für Ihre Computer sollten beseitigt werden.
- Sicherheitsrisiken in der Sicherheitskonfiguration von VM-Skalierungsgruppen sollten beseitigt werden.
- Sicherheitsrisiken in Containersicherheitskonfigurationen sollten behoben werden.
- Sicherheitsrisiken in SQL-Datenbanken sollten beseitigt werden.

## <a name="68-use-only-approved-applications"></a>6.8 Ausschließliche Verwendung genehmigter Anwendungen

- Standard-Tarif von Security Center muss ausgewählt sein
- Die adaptive Anwendungssteuerung sollte auf virtuellen Computern aktiviert werden.

## <a name="69-use-only-approved-azure-services"></a>6.9 Ausschließliche Verwendung genehmigter Azure-Dienste

- VMs sollten zu neuen Azure Resource Manager-Ressourcen migriert werden
- Speicherkonten sollten zu neuen Azure Resource Manager-Ressourcen migriert werden.

## <a name="610-implement-approved-application-list"></a>6.10 Implementieren einer Liste genehmigter Anwendungen

- Standard-Tarif von Security Center muss ausgewählt sein
- Die adaptive Anwendungssteuerung sollte auf virtuellen Computern aktiviert werden.

## <a name="73-maintain-secure-azure-resource-configurations"></a>7.3 Verwalten von sicheren Konfigurationen für Azure-Ressourcen

- \[Vorschau\]: Podsicherheitsrichtlinien sollten in Kubernetes Services definiert sein.

## <a name="74-maintain-secure-operating-system-configurations"></a>7.4 Verwalten sicherer Betriebssystemkonfigurationen

- Sicherheitsrisiken in der Sicherheitskonfiguration für Ihre Computer sollten beseitigt werden.
- Sicherheitsrisiken in Containersicherheitskonfigurationen sollten behoben werden.
- Sicherheitsrisiken in der Sicherheitskonfiguration von VM-Skalierungsgruppen sollten beseitigt werden.

## <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9 Implementieren der automatisierten Konfigurationsüberwachung für Azure-Dienste

- \[Vorschau\]: Podsicherheitsrichtlinien sollten in Kubernetes Services definiert sein.

## <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10 Implementieren der automatisierten Konfigurationsüberwachung für Betriebssysteme

- Sicherheitsrisiken in der Sicherheitskonfiguration für Ihre Computer sollten beseitigt werden.
- Sicherheitsrisiken in Containersicherheitskonfigurationen sollten behoben werden.
- Sicherheitsrisiken in der Sicherheitskonfiguration von VM-Skalierungsgruppen sollten beseitigt werden.

## <a name="711-manage-azure-secrets-securely"></a>7.11 Sicheres Verwalten von Azure-Geheimnissen

- Key Vault-Objekte müssen wiederherstellbar sein

## <a name="712-manage-identities-securely-and-automatically"></a>7.12 Sicheres und automatisches Verwalten von Identitäten

- Funktions-sollte muss verwaltete Identität verwenden.
- Web-App sollte verwaltete Identität verwenden.
- API-App sollte verwaltete Identität verwenden.

## <a name="81-use-centrally-managed-anti-malware-software"></a>8.1 Verwenden einer zentral verwalteten Antischadsoftware

- Fehlenden Endpoint Protection-Schutz in Azure Security Center überwachen
- Die Endpoint Protection-Lösung sollte für VM-Skalierungsgruppen installiert sein.

## <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2 Vorabprüfen von in computefremde Azure-Ressourcen hochzuladenden Dateien

- Standard-Tarif von Security Center muss ausgewählt sein

## <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3 Sicherstellen der Aktualisierung von Antischadsoftware und Signaturen

- Microsoft Antimalware für Azure sollte für die automatische Aktualisierung von Schutzsignaturen konfiguriert sein.

## <a name="91-ensure-regular-automated-back-ups"></a>9.1 Sicherstellen regelmäßiger automatisierter Sicherungen

- Langfristige georedundante Sicherung muss für Azure SQL-Datenbank aktiviert sein
- Georedundante Sicherung muss für Azure Database for MySQL aktiviert sein
- Georedundante Sicherung muss für Azure Database for PostgreSQL aktiviert sein
- Georedundante Sicherung muss für Azure Database for MariaDB aktiviert sein
- Azure Backup muss für Virtual Machines aktiviert sein.

## <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2 Durchführen vollständiger Systemsicherungen und Sichern aller von Kunden verwalteten Schlüssel

- Langfristige georedundante Sicherung muss für Azure SQL-Datenbank aktiviert sein
- Georedundante Sicherung muss für Azure Database for MySQL aktiviert sein
- Georedundante Sicherung muss für Azure Database for PostgreSQL aktiviert sein
- Georedundante Sicherung muss für Azure Database for MariaDB aktiviert sein
- Azure Backup muss für Virtual Machines aktiviert sein.

## <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4 Sicherstellen des Schutzes von Sicherungen und von kundenseitig verwalteten Schlüsseln

- Key Vault-Objekte müssen wiederherstellbar sein

## <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2 Erstellen eines Verfahrens zur Bewertung und Priorisierung von Vorfällen

- Standard-Tarif von Security Center muss ausgewählt sein

## <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4 Angeben von Kontaktdaten für Sicherheitsvorfälle und Konfigurieren von Warnungsbenachrichtigungen für Sicherheitsvorfälle

- Für Ihr Abonnement muss eine E-Mail-Adresse als Sicherheitskontakt angegeben sein
- Für Ihr Abonnement muss eine Telefonnummer für den Sicherheitskontakt angegeben werden
- Advanced Data Security-Einstellungen für SQL Server sollten eine E-Mail-Adresse für den Empfang von Sicherheitswarnungen enthalten.
- Advanced Data Security-Einstellungen für verwaltete SQL-Instanzen sollten eine E-Mail-Adresse für den Empfang von Sicherheitswarnungen enthalten.
- E-Mail-Benachrichtigungen an Administratoren und Abonnementbesitzer sollten in den erweiterten Einstellungen für Datensicherheit in SQL Server aktiviert werden.
- In den Advanced Data Security-Einstellungen für die verwaltete SQL-Instanz müssen E-Mail-Benachrichtigungen an Administratoren und Abonnementbesitzer aktiviert sein

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Steuerungszuordnung der Blaupause für den Vergleichstest für die Azure-Sicherheit überprüft haben, rufen Sie Azure Policy im Azure-Portal auf, um die Initiative zuzuweisen:

> [!div class="nextstepaction"]
> [Azure Policy](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyMenuBlade/Definitions)

Weitere Artikel zu Blaupausen und ihrer Nutzung:

- Erfahren Sie mehr über den [Lebenszyklus von Blaupausen](../../concepts/lifecycle.md).
- Machen Sie sich mit der Verwendung [statischer und dynamischer Parameter](../../concepts/parameters.md) vertraut.
- Erfahren Sie, wie Sie die [Abfolge von Blaupausen](../../concepts/sequencing-order.md) anpassen können.
- Erfahren Sie, wie Sie [Ressourcen in Blaupausen sperren](../../concepts/resource-locking.md) können.
- Lernen Sie, wie Sie [vorhandene Zuweisungen aktualisieren](../../how-to/update-existing-assignments.md).