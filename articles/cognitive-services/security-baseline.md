---
title: Azure-Sicherheitsbaseline für Cognitive Services
description: Die Cognitive Services-Sicherheitsbaseline enthält Schrittanleitungen und Ressourcen für die Implementierung der Sicherheitsempfehlungen, die im Vergleichstest für die Azure-Sicherheit angegeben sind.
author: msmbaldwin
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: b243fa18b17fdd15f3c39545b7d81f5796bd8429
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101699860"
---
# <a name="azure-security-baseline-for-cognitive-services"></a>Azure-Sicherheitsbaseline für Cognitive Services

Diese Sicherheitsbaseline wendet Empfehlungen der [Version 1.0 des Vergleichstests für die Azure-Sicherheit](../security/benchmarks/overview-v1.md) auf Cognitive Services an. Der Azure-Sicherheitsvergleichstest enthält Empfehlungen zum Schutz Ihrer Cloudlösungen in Azure.
Der Inhalt wird anhand der **Sicherheitskontrollen** gruppiert, die durch den Azure-Sicherheitsvergleichstest und die entsprechenden für Cognitive Services geltenden Empfehlungen definiert werden. Nicht auf Cognitive Services anwendbare **Kontrollen** wurden ausgeschlossen.

 
Die vollständige Zuordnung von Cognitive Services zum Azure-Sicherheitsvergleichstest finden Sie in der [vollständigen Zuordnungsdatei der Cognitive Services-Sicherheitsbaseline](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Netzwerksicherheit

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Netzwerksicherheit](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Schützen von Azure-Ressourcen in virtuellen Netzwerken

**Leitfaden**: Microsoft Azure Cognitive Services bietet ein mehrschichtiges Sicherheitsmodell. Dank dieses Modells können Sie Ihre Cognitive Services-Konten für eine bestimmte Teilmenge von Netzwerken schützen. Wenn Netzwerkregeln konfiguriert wurden, können nur Anwendungen, die Daten über die angegebene Gruppe von Netzwerken anfordern, auf das Konto zugreifen. Sie können den Zugriff auf Ihre Ressourcen mithilfe von Anforderungsfilterung einschränken und nur Anforderungen zulassen, die aus angegebenen IP-Adressen, IP-Adressbereichen oder einer Liste von Subnetzen in virtuellen Azure-Netzwerken stammen.

Die Unterstützung von virtuellen Netzwerken und Dienstendpunkten für Cognitive Services ist auf eine bestimmte Gruppe von Regionen beschränkt.

- [Konfigurieren von virtuellen Netzwerken für Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-virtual-networks?tabs=portal)

- [Übersicht über virtuelle Azure-Netzwerke](../virtual-network/virtual-networks-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2: Überwachen und Protokollieren der Konfiguration und des Datenverkehrs von virtuellen Netzwerken, Subnetzen und Netzwerkschnittstellen

**Leitfaden**: Wenn virtuelle Computer im gleichen virtuellen Netzwerk wie Ihr Cognitive Services-Container bereitgestellt werden, können Sie Netzwerksicherheitsgruppen (NSG) verwenden, um das Risiko von Datenexfiltration zu verringern. Aktivieren Sie Netzwerksicherheitsgruppen-Datenflussprotokolle, und senden Sie Protokolle an ein Azure Storage-Konto, um den Datenverkehr zu überwachen. Senden Sie Datenflussprotokolle von Netzwerksicherheitsgruppen an einen Log Analytics-Arbeitsbereich, und verwenden Sie Traffic Analytics, um Einblicke in den Datenverkehrsfluss in Ihrer Azure-Cloud zu erhalten. Einige Vorteile von Traffic Analytics sind die Möglichkeit, die Netzwerkaktivität zu visualisieren und Hotspots zu erkennen, Sicherheitsbedrohungen zu identifizieren, Datenverkehrsflussmuster zu verstehen und Netzwerkfehlkonfigurationen zu ermitteln.

- [Aktivieren der NSG-Flussprotokolle](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Aktivieren und Verwenden von Traffic Analytics](../network-watcher/traffic-analytics.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="13-protect-critical-web-applications"></a>1.3: Schützen kritischer Webanwendungen

**Leitfaden**: Wenn Sie Cognitive Services in einem Container verwenden, können Sie Ihre Containerbereitstellung mit einer Front-End-Firewall-Lösung für Webanwendungen verbessern, die schädlichen Datenverkehr filtert und die End-to-End-TLS-Verschlüsselung unterstützt, sodass der Containerendpunkt privat und sicher bleibt. 

Beachten Sie, dass Cognitive Services-Container erforderlich sind, um Messungsinformationen zu Abrechnungszwecken zu übermitteln. Die einzige Ausnahme sind Offlinecontainer, da für sie eine abweichende Abrechnungsmethode gilt. Wenn Sie die verschiedenen Netzwerkkanäle, auf die sich die Cognitive Services-Container stützen, nicht in einer Zulassungsliste aufführen, funktionieren die Container nicht. Der Host sollte Port 443 und die folgenden Domänen in seiner Zulassungsliste aufführen:

- *.cognitive.microsoft.com
- *.cognitiveservices.azure.com

Beachten Sie außerdem, dass Sie die eingehende Paketuntersuchung für Ihre Firewall in den sicheren Kanälen, die von den Cognitive Services-Containern zu Microsoft-Servern erstellt werden, deaktivieren müssen. Andernfalls funktionieren die Container nicht ordnungsgemäß.

- [Grundlegendes zur Containersicherheit in Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support#azure-cognitive-services-container-security)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Ablehnen der Kommunikation mit bekannten schädlichen IP-Adressen

**Leitfaden**: Wenn virtuelle Computer im gleichen virtuellen Netzwerk wie Ihr Cognitive Services-Container bereitgestellt werden, definieren und implementieren Sie mit Azure Policy Standardsicherheitskonfigurationen für verknüpfte Netzwerkressourcen. Verwenden Sie Azure Policy-Aliase in den Namespaces „Microsoft.CognitiveServices“ und „Microsoft.Network“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen der Netzwerkkonfiguration Ihrer Azure Cache for Redis-Instanzen zu erstellen. Sie können auch integrierte Richtliniendefinitionen verwenden, z. B.:

- DDoS Protection Standard sollte aktiviert sein.

Verwenden Sie Azure Blueprints, um umfangreiche Azure-Bereitstellungen zu vereinfachen, indem wichtige Umgebungsartefakte wie Azure Resource Manager-Vorlagen, die rollenbasierte Zugriffssteuerung in Azure (Azure RBAC) und Richtlinien gemeinsam in einer Blaupausendefinition zusammengefasst werden. Sie können die Blaupause leicht auf neue Abonnements und Umgebungen anwenden und die Steuerung und Verwaltung durch die Versionsverwaltung optimieren.

Wenn Sie Cognitive Services in einem Container verwenden, können Sie Ihre Containerbereitstellung mit einer Front-End-Firewall-Lösung für Webanwendungen verbessern, die schädlichen Datenverkehr filtert und die End-to-End-TLS-Verschlüsselung unterstützt, sodass der Containerendpunkt privat und sicher bleibt. 

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Erstellen einer Azure-Blaupause](../governance/blueprints/create-blueprint-portal.md)

- [Grundlegendes zur Containersicherheit in Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support#azure-cognitive-services-container-security)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="15-record-network-packets"></a>1.5: Aufzeichnen von Netzwerkpaketen

**Leitfaden**: Wenn virtuelle Computer im gleichen virtuellen Netzwerk wie Ihr Cognitive Services-Container bereitgestellt werden, können Sie Netzwerksicherheitsgruppen (NSG) verwenden, um das Risiko von Datenexfiltration zu verringern. Aktivieren Sie Netzwerksicherheitsgruppen-Datenflussprotokolle, und senden Sie Protokolle an ein Azure Storage-Konto, um den Datenverkehr zu überwachen. Senden Sie Datenflussprotokolle von Netzwerksicherheitsgruppen an einen Log Analytics-Arbeitsbereich, und verwenden Sie Traffic Analytics, um Einblicke in den Datenverkehrsfluss in Ihrer Azure-Cloud zu erhalten. Einige Vorteile von Traffic Analytics sind die Möglichkeit, die Netzwerkaktivität zu visualisieren und Hotspots zu erkennen, Sicherheitsbedrohungen zu identifizieren, Datenverkehrsflussmuster zu verstehen und Netzwerkfehlkonfigurationen zu ermitteln.

- [Aktivieren der NSG-Flussprotokolle](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Aktivieren und Verwenden von Traffic Analytics](../network-watcher/traffic-analytics.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Bereitstellen von netzwerkbasierten Angriffserkennungs-/Eindringschutzsystemen (Intrusion Detection/Intrusion Prevention Systems, IDS/IPS)

**Leitfaden**: Wenn Sie Cognitive Services in einem Container verwenden, können Sie Ihre Containerbereitstellung mit einer Front-End-Firewall-Lösung für Webanwendungen verbessern, die schädlichen Datenverkehr filtert und die End-to-End-TLS-Verschlüsselung unterstützt, sodass der Containerendpunkt privat und sicher bleibt.  Sie können ein Angebot aus dem Azure Marketplace auswählen, das IDS-/IPS-Funktionen mit Funktionalität zum Deaktivieren der Nutzlastuntersuchung unterstützt.

Beachten Sie, dass Cognitive Services-Container erforderlich sind, um Messungsinformationen zu Abrechnungszwecken zu übermitteln. Die einzige Ausnahme bilden Offlinecontainer, da sie einer anderen Abrechnungsmethode unterliegen. Wenn Sie die verschiedenen Netzwerkkanäle, auf die sich die Cognitive Services-Container stützen, nicht in einer Zulassungsliste aufführen, funktionieren die Container nicht. Der Host sollte Port 443 und die folgenden Domänen in seiner Zulassungsliste aufführen:

- *.cognitive.microsoft.com
- *.cognitiveservices.azure.com

Beachten Sie außerdem, dass Sie die eingehende Paketuntersuchung für Ihre Firewall in den sicheren Kanälen, die von den Cognitive Services-Containern zu Microsoft-Servern erstellt werden, deaktivieren müssen. Andernfalls funktionieren die Container nicht ordnungsgemäß.

- [Grundlegendes zur Containersicherheit in Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support#azure-cognitive-services-container-security)

- [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="17-manage-traffic-to-web-applications"></a>1.7: Verwalten von Datenverkehr für Webanwendungen

**Leitfaden**: Wenn Sie Cognitive Services in einem Container verwenden, können Sie Ihre Containerbereitstellung mit einer Front-End-Firewall-Lösung für Webanwendungen verbessern, die schädlichen Datenverkehr filtert und die End-to-End-TLS-Verschlüsselung unterstützt, sodass der Containerendpunkt privat und sicher bleibt. 

Beachten Sie, dass Cognitive Services-Container erforderlich sind, um Messungsinformationen zu Abrechnungszwecken zu übermitteln. Die einzige Ausnahme sind Offlinecontainer, da für sie eine abweichende Abrechnungsmethode gilt. Wenn Sie die verschiedenen Netzwerkkanäle, auf die sich die Cognitive Services-Container stützen, nicht in einer Zulassungsliste aufführen, funktionieren die Container nicht. Der Host sollte Port 443 und die folgenden Domänen in seiner Zulassungsliste aufführen:

- *.cognitive.microsoft.com
- *.cognitiveservices.azure.com

Beachten Sie außerdem, dass Sie die eingehende Paketuntersuchung für Ihre Firewall in den sicheren Kanälen, die von den Cognitive Services-Containern zu Microsoft-Servern erstellt werden, deaktivieren müssen. Andernfalls funktionieren die Container nicht ordnungsgemäß.

- [Grundlegendes zur Containersicherheit in Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support#azure-cognitive-services-container-security)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimieren der Komplexität und des Verwaltungsaufwands von Netzwerksicherheitsregeln

**Leitfaden**: Verwenden Sie Diensttags in virtuellen Netzwerken, um Netzwerkzugriffssteuerungen für Netzwerksicherheitsgruppen oder Azure Firewall zu definieren. Sie können Diensttags anstelle von spezifischen IP-Adressen nutzen, wenn Sie Sicherheitsregeln erstellen. Wenn Sie den Diensttagnamen (z. B. ApiManagement) im entsprechenden Feld Quelle oder Ziel einer Regel angeben, können Sie den Datenverkehr für den entsprechenden Dienst zulassen oder verweigern. Microsoft verwaltet die Adresspräfixe, für die das Diensttag gilt, und aktualisiert das Diensttag automatisch, wenn sich die Adressen ändern.

Sie können auch Anwendungssicherheitsgruppen verwenden, um eine komplexe Sicherheitskonfiguration zu vereinfachen. Mit Anwendungssicherheitsgruppen können Sie die Netzwerksicherheit als natürliche Erweiterung einer Anwendungsstruktur konfigurieren und virtuelle Computer gruppieren sowie auf der Grundlage dieser Gruppen Netzwerksicherheitsrichtlinien definieren.

- [Diensttags für virtuelle Netzwerke](../virtual-network/service-tags-overview.md)

- [Anwendungssicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/network-security-groups-overview#application-security-groups)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Beibehalten von Standardsicherheitskonfigurationen für Netzwerkgeräte

**Leitfaden**: Definieren und implementieren Sie mit Azure Policy Standardsicherheitskonfigurationen für Netzwerkressourcen, die Ihrem Cognitive Services-Container zugeordnet sind. Verwenden Sie Azure Policy-Aliase in den Namespaces „Microsoft.CognitiveServices“ und „Microsoft.Network“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen der Netzwerkkonfiguration Ihrer Azure Cache for Redis-Instanzen zu erstellen.

Sie können auch Azure Blueprints verwenden, um umfangreiche Azure-Bereitstellungen zu vereinfachen, indem wichtige Umgebungsartefakte, z. B. Azure Resource Manager-Vorlagen, rollenbasierte Zugriffssteuerung (Azure RBAC) und Richtlinien, gemeinsam in einer Blaupausendefinition gepackt werden. Sie können die Blaupause leicht auf neue Abonnements und Umgebungen anwenden und die Steuerung und Verwaltung durch die Versionsverwaltung optimieren.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Erstellen einer Azure-Blaupause](../governance/blueprints/create-blueprint-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="110-document-traffic-configuration-rules"></a>1.10: Dokumentieren von Datenverkehrskonfigurationsregeln

**Leitfaden**: Verwenden Sie Tags für Netzwerkressourcen, die Ihrem Cognitive Services-Container zugeordnet sind, um sie in einer Taxonomie logisch zu organisieren.

- [Erstellen und Verwenden von Tags](../azure-resource-manager/management/tag-resources.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Verwenden automatisierter Tools zum Überwachen von Netzwerkressourcenkonfigurationen und Erkennen von Änderungen

**Leitfaden**: Nutzen Sie das Azure-Aktivitätsprotokoll zum Überwachen der Konfigurationen von Netzwerkressourcen und zum Erkennen von Änderungen bei Netzwerkressourcen im Zusammenhang mit Ihrem Cognitive Services-Container. Erstellen Sie Warnungen in Azure Monitor, die bei Änderungen an wichtigen Netzwerkressourcen ausgelöst werden.

- [Anzeigen und Abrufen von Azure-Aktivitätsprotokollereignissen](/azure/azure-monitor/platform/activity-log#view-the-activity-log)

- [Erstellen von Warnungen in Azure Monitor](/azure/azure-monitor/platform/alerts-activity-log)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="logging-and-monitoring"></a>Protokollierung und Überwachung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Protokollierung und Überwachung](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Konfigurieren der zentralen Sicherheitsprotokollverwaltung

**Leitfaden**: Aktivieren Sie die Diagnoseeinstellungen für das Azure-Aktivitätsprotokoll, und senden Sie die Protokolle zwecks Archivierung an einen Log Analytics-Arbeitsbereich, eine Azure Event Hub-Instanz oder ein Azure-Speicherkonto. Aktivitätsprotokolle geben Einblick in die Vorgänge, die für Ihren Cognitive Services-Container auf Steuerungsebene erfolgen. Mit Aktivitätsprotokollen können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge (PUT, POST, DELETE) auf Steuerungsebene ermitteln, die für Ihre Azure Cache for Redis-Instanzen stattfinden.

- [Erfassen und Analysieren des Azure-Aktivitätsprotokolls in Azure Monitor](/azure/azure-monitor/platform/activity-log)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Aktivieren der Überwachungsprotokollierung für Azure-Ressourcen

**Leitfaden**: Aktivieren Sie für die Überwachungsprotokollierung auf der Steuerungsebene die Diagnoseeinstellungen für das Azure-Aktivitätsprotokoll, und senden Sie die Protokolle zwecks Archivierung an einen Log Analytics-Arbeitsbereich, eine Azure Event Hubs-Instanz oder ein Azure-Speicherkonto. Mit den Daten aus Azure-Aktivitätsprotokollen können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge (PUT, POST, DELETE) ermitteln, die auf Steuerungsebene für Ihre Azure-Ressourcen ausgeführt werden.

Außerdem sendet Cognitive Services Diagnoseereignisse, die gesammelt und dann für die Analyse, Warnung und Berichterstellung verwendet werden können. Diagnoseereignisse für einen Cognitive Services-Container lassen sich im Azure-Portal konfigurieren. Sie können ein oder mehrere Diagnoseereignisse an ein Speicherkonto, eine Event Hubs-Instanz oder einen Log Analytics-Arbeitsbereich senden.

- [Erfassen und Analysieren des Azure-Aktivitätsprotokolls in Azure Monitor](/azure/azure-monitor/platform/diagnostic-settings-legacy)

- [Verwenden von Diagnoseeinstellungen für Azure Cognitive Services](diagnostic-logging.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="25-configure-security-log-storage-retention"></a>2.5: Konfigurieren der Sicherheitsprotokoll-Aufbewahrungsdauer im Speicher

**Leitfaden**: Legen Sie in Azure Monitor den Aufbewahrungszeitraum Ihres Log Analytics-Arbeitsbereichs gemäß den Compliancevorschriften Ihrer Organisation fest. Verwenden Sie Azure Storage-Konten für die langfristige Speicherung oder Archivierung.

- [Ändern des Datenaufbewahrungszeitraums](/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="26-monitor-and-review-logs"></a>2.6: Überwachen und Überprüfen von Protokollen

**Leitfaden**: Aktivieren Sie die Diagnoseeinstellungen für das Azure-Aktivitätsprotokoll, und senden Sie die Protokolle an einen Log Analytics-Arbeitsbereich. Diese Protokolle stellen umfassende, häufig verwendete Daten zu den Vorgängen einer Ressource bereit, die für die Ermittlung von Problemen und das Debuggen verwendet werden. Führen Sie Abfragen in Log Analytics durch, um Suchbegriffe zu ermitteln, Trends auszumachen, Muster zu analysieren und viele weitere Erkenntnisse auf Grundlage der Aktivitätsprotokolldaten zu gewinnen, die möglicherweise für Azure Cognitive Services gesammelt wurden.

- [Erfassen und Analysieren des Azure-Aktivitätsprotokolls in Azure Monitor](/azure/azure-monitor/platform/activity-log)

- [Erfassen und Analysieren des Azure-Aktivitätsprotokolls in Azure Monitor](/azure/azure-monitor/platform/activity-log)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Aktivieren von Warnungen bei anomalen Aktivitäten

**Leitfaden:** Sie können Warnungen zu unterstützten Metriken in Cognitive Services einstellen, indem Sie in Azure Monitor zum Abschnitt „Warnungen und Metriken“ navigieren.

Konfigurieren Sie die Diagnoseeinstellungen für Ihren Cognitive Services-Container, und senden Sie Protokolle an einen Log Analytics-Arbeitsbereich. Konfigurieren Sie in Ihrem Log Analytics-Arbeitsbereich Warnungen, die ausgelöst werden sollen, wenn vordefinierte Bedingungen eintreten. Alternativ dazu können Sie auch Daten in Azure Sentinel oder einer Drittanbieter-SIEM-Lösung aktivieren und integrieren.

- [Durchführen des Onboardings für Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Erstellen, Anzeigen und Verwalten von Protokollwarnungen mithilfe von Azure Monitor](/azure/azure-monitor/platform/alerts-log)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="identity-and-access-control"></a>Identität und Zugriffssteuerung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Identität und Zugriffssteuerung](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Verwalten eines Bestands von Administratorkonten

**Leitfaden**: Azure Active Directory (Azure AD) umfasst integrierte Rollen, die explizit zugewiesen werden müssen und abgefragt werden können. Verwenden Sie das Azure AD PowerShell-Modul, um Ad-hoc-Abfragen zum Ermitteln von Konten auszuführen, die Mitglieder von administrativen Gruppen sind.

- [Abrufen einer Verzeichnisrolle in Azure AD mit PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0&amp;preserve-view=true)

- [Abrufen von Mitgliedern einer Verzeichnisrolle in Azure AD mit PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0&amp;preserve-view=true)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Ändern von Standardkennwörtern bei Bedarf

**Leitfaden**: Der Zugriff auf Steuerungsebene auf Cognitive Services wird mittels Azure Active Directory (AD) gesteuert. Azure AD verfügt nicht über das Konzept von Standardkennwörtern.

Der Zugriff auf Datenebene auf Cognitive Services wird mittels Zugriffsschlüsseln gesteuert. Diese Schlüssel werden von den Clients verwendet, die eine Verbindung mit Ihrem Cache herstellen, und können jederzeit neu generiert werden.

Es wird nicht empfohlen, Standardkennwörter in Ihre Anwendung zu integrieren. Stattdessen können Sie Ihre Kennwörter in Azure Key Vault speichern und sie über Azure AD abrufen.

- [Neugenerieren von Zugriffsschlüsseln für Azure Cognitive Services](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-configure#settings)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Verwenden dedizierter Administratorkonten

**Leitfaden**: Erstellen Sie Standardbetriebsvorgänge für die Verwendung dedizierter Administratorkonten. Verwenden Sie die Identitäts- und Zugriffsverwaltung in Security Center, um die Anzahl der Administratorkonten zu überwachen.

Um den Überblick über dedizierte Verwaltungskonten zu behalten, können Sie zusätzlich Empfehlungen von Security Center oder integrierte Azure-Richtlinien befolgen, etwa:

- Ihrem Abonnement sollte mehr als ein Besitzer zugewiesen sein.
- Veraltete Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.
- Externe Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.

- [Überwachen der Identität und des Zugriffs](../security-center/security-center-identity-access.md)

- [Tutorial: Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung](../governance/policy/tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4: Verwenden des einmaligen Anmeldens (SSO) in Azure Active Directory

**Leitfaden:** Cognitive Services verwendet Zugriffsschlüssel zur Benutzerauthentifizierung und unterstützt kein einmaliges Anmelden (Single Sign-On, SSO) auf Datenebene. Der Zugriff auf die Steuerungsebene für Cognitive Services ist über die REST-API verfügbar und unterstützt SSO. Legen Sie zum Authentifizieren den Autorisierungsheader für Ihre Anforderungen auf ein JSON-Webtoken (JavaScript Object Notation) fest, das Sie aus Azure Active Directory (Azure AD) abrufen.

- [Grundlegendes zur REST-API von Azure Cognitive Services](/rest/api/cognitiveservices/)

- [Grundlegendes zu SSO mit Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Verwenden der mehrstufigen Authentifizierung für den gesamten Azure Active Directory-basierten Zugriff

**Leitfaden:** Aktivieren Sie die mehrstufige Authentifizierung für Azure Active Directory (Azure AD), und befolgen Sie die Empfehlungen für die Identitäts- und Zugriffsverwaltung in Security Center.

- [Planen einer Bereitstellung von Azure AD Multi-Factor Authentication](../active-directory/authentication/howto-mfa-getstarted.md)

- [Überwachen von Identität und Zugriff in Azure Security Center](../security-center/security-center-identity-access.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3.6: Verwenden von sicheren, von Azure verwalteten Arbeitsstationen für Verwaltungsaufgaben

**Leitfaden**: Verwenden Sie Privileged Access Workstations (PAW) mit mehrstufiger Authentifizierung, die für die Anmeldung und Konfiguration von Azure-Ressourcen konfiguriert sind. 

- [Informationen zu Arbeitsstationen mit privilegiertem Zugriff](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Planen einer Bereitstellung von Azure AD Multi-Factor Authentication](../active-directory/authentication/howto-mfa-getstarted.md)

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

**Leitfaden**: Konfigurieren Sie in der Azure Active Directory-Funktion (Azure AD) „Bedingter Zugriff“ benannte Standorte, um den Zugriff nur bestimmten logischen Gruppierungen von IP-Adressbereichen oder Ländern und Regionen zu gestatten.

- [Konfigurieren benannter Standorte in Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="39-use-azure-active-directory"></a>3.9: Verwenden von Azure Active Directory

**Leitfaden**: Verwenden Sie Azure Active Directory (Azure AD) als zentrales Authentifizierungs- und Autorisierungssystem. Azure AD schützt Daten mithilfe von starker Verschlüsselung für Daten im Ruhezustand und in der Übertragung. Außerdem werden Salts, Hashes und sichere Speicher für Benutzeranmeldeinformationen verwendet. Wenn Ihr Anwendungsfall die AD-Authentifizierung unterstützt, verwenden Sie Azure AD, um Anforderungen an Ihre Cognitive Services-API zu authentifizieren. 

Derzeit unterstützen nur Maschinelles Sehen-API, Gesichtserkennungs-API, Textanalyse-API, Plastischer Reader, Formularerkennung, Anomalieerkennung und alle Bing-Dienste mit Ausnahme der benutzerdefinierten Bing-Suche die Authentifizierung mit Azure AD.

- [Authentifizieren von Anforderungen für Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-azure-active-directory)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Regelmäßiges Überprüfen und Abstimmen des Benutzerzugriffs

**Leitfaden**: Azure Active Directory (Azure AD) enthält Protokolle zum Ermitteln von veralteten Konten. Darüber hinaus können Kunden Zugriffsüberprüfungen für Azure-Identitäten verwenden, um Gruppenmitgliedschaften, den Zugriff auf Unternehmensanwendungen und Rollenzuweisungen effizient zu verwalten. Der Benutzerzugriff kann regelmäßig überprüft werden, um sicherzustellen, dass nur aktive Benutzer weiterhin Zugriff besitzen.

Kunden können einen Bestand an Benutzerkonten für API Management verwalten und den Zugriff nach Bedarf abstimmen. In API Management sind Entwickler die Benutzer der APIs, die Sie mit API Management verfügbar machen. Neu erstellte Entwicklerkonten sind standardmäßig „Aktiv“ und werden der Gruppe „Entwickler“ zugewiesen. Entwicklerkonten mit dem Status „Aktiv“ können sämtliche APIs aufrufen, die sie abonniert haben.

- [Verwalten von Benutzerkonten in Azure API Management](../api-management/api-management-howto-create-or-invite-developers.md)

- [Abrufen einer Liste mit API Management-Benutzern](https://docs.microsoft.com/powershell/module/az.apimanagement/get-azapimanagementuser?view=azps-4.8.0&amp;preserve-view=true)

- [Verwenden von Zugriffsüberprüfungen für Azure-Identitäten](../active-directory/governance/access-reviews-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Überwachen von Zugriffsversuchen auf deaktivierte Anmeldeinformationen

**Leitfaden**: Sie haben Zugriff auf Azure AD-Anmeldeaktivitäten (Azure Active Directory) sowie Quellen für Überwachungs- und Risikoereignisprotokolle, wodurch die Integration in Azure Sentinel oder ein SIEM-Tool eines Drittanbieters möglich ist.

Sie können diesen Prozess optimieren, indem Sie Diagnoseeinstellungen für Azure AD-Benutzerkonten erstellen und die Überwachungs- und Anmeldeprotokolle an einen Log Analytics-Arbeitsbereich senden. Sie können gewünschte Warnungen in Log Analytics konfigurieren.

- [Integrieren von Azure-Aktivitätsprotokollen in Azure Monitor](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

- [Schnellstart: Ausführen des Onboardings für Azure Sentinel](../sentinel/quickstart-onboard.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12: Warnung bei abweichendem Verhalten bei der Kontoanmeldung

**Leitfaden**: Verwenden Sie für abweichendes Verhalten bei der Kontoanmeldung auf Steuerungsebene Azure Active Directory (Azure AD) Identity Protection und Risikoerkennungsfunktionen, um automatische Reaktionen auf erkannte verdächtige Aktionen im Zusammenhang mit Benutzeridentitäten zu konfigurieren. Außerdem können Sie Daten zur weiteren Untersuchung in Azure Sentinel erfassen.

- [Anzeigen riskanter Azure AD-Anmeldungen](../active-directory/identity-protection/overview-identity-protection.md)

- [Konfigurieren und Aktivieren von Risikorichtlinien für den Identitätsschutz](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Durchführen des Onboardings für Azure Sentinel](../sentinel/quickstart-onboard.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Ermöglichen des Zugriffs auf relevante Kundendaten für Microsoft in Supportszenarien

**Leitfaden:** Nicht für Cognitive Services verfügbar. Kunden-Lockbox wird für Cognitive Services noch nicht unterstützt.

- [Unterstützte Dienste und Szenarios bei allgemeiner Verfügbarkeit](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability)

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

**Leitfaden:** Implementieren Sie getrennte Abonnements oder Verwaltungsgruppen für Entwicklungs-, Test- und Produktionsumgebungen. Ressourcen sollten durch virtuelle Netzwerke oder Subnetze getrennt, entsprechend gekennzeichnet und durch eine Netzwerksicherheitsgruppe oder Azure Firewall gesichert werden. Ressourcen, die vertrauliche Daten speichern oder verarbeiten, müssen ausreichend isoliert werden. Implementieren Sie für virtuelle Computer, auf denen vertrauliche Informationen gespeichert oder verarbeitet werden, Richtlinien und Verfahren, mit denen Sie sie bei Nichtverwendung deaktivieren können.

- [Erstellen zusätzlicher Azure-Abonnements](../cost-management-billing/manage/create-subscription.md)

- [Erstellen von Verwaltungsgruppen](../governance/management-groups/create-management-group-portal.md)

- [Erstellen und Verwenden von Tags](../azure-resource-manager/management/tag-resources.md)

- [Erstellen eines virtuellen Netzwerks](../virtual-network/quick-create-portal.md)

- [Erstellen einer NSG mit einer Sicherheitskonfiguration](../virtual-network/tutorial-filter-network-traffic.md)

- [Bereitstellen von Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md)

- [Konfigurieren von „Warnen“ oder „Warnen und ablehnen“ mit Azure Firewall](../firewall/threat-intel.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Überwachen und Blockieren einer nicht autorisierten Übertragung vertraulicher Informationen

**Leitfaden:** Noch nicht verfügbar. Funktionen für Datenidentifizierung, -klassifizierung und -verlust sind für Cognitive Services noch nicht verfügbar.

Microsoft verwaltet die zugrunde liegende Infrastruktur für Cognitive Services und hat strenge Kontrollen implementiert, um den Verlust oder die Offenlegung von Kundendaten zu verhindern.

- [Grundlegendes zum Schutz von Kundendaten in Azure](../security/fundamentals/protection-customer-data.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Verschlüsseln aller vertraulichen Informationen während der Übertragung

**Leitfaden**: Bei allen Cognitive Services-Endpunkten, die über HTTP verfügbar gemacht werden, wird TLS 1.2 erzwungen. Aufgrund des erzwungenen Sicherheitsprotokolls gelten für Consumer, die einen Cognitive Services-Endpunkt aufrufen möchten, folgende Richtlinien:

- Das Clientbetriebssystem (Operating System, OS) muss TLS 1.2 unterstützen.
- Die für den HTTP-Aufruf verwendete Sprache (und Plattform) muss TLS 1.2 als Teil der Anforderung angeben. Je nach Sprache und Plattform erfolgt die Angabe von TLS entweder implizit oder explizit.

- [Grundlegendes zu Transport Layer Security für Azure Cognitive Services](cognitive-services-security.md)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Verwenden eines aktiven Ermittlungstools zur Bestimmung vertraulicher Daten

**Leitfaden:** Funktionen für Datenidentifizierung, -klassifizierung und -verlust sind für Cognitive Services noch nicht verfügbar. Versehen Sie Instanzen mit vertraulichen Informationen mit entsprechenden Tags, und implementieren Sie eine Drittanbieterlösung, wenn dies für Compliancezwecke erforderlich ist.

Microsoft verwaltet die zugrunde liegende Plattform, behandelt alle Kundeninhalte vertraulich und unternimmt große Anstrengungen, um Kundendaten vor Verlust und Offenlegung zu schützen. Um die Sicherheit von Kundendaten innerhalb von Azure zu gewährleisten, hat Microsoft eine Reihe von robusten Datenschutzkontrollen und -funktionen implementiert und kümmert sich um deren Verwaltung.

- [Grundlegendes zum Schutz von Kundendaten in Azure](../security/fundamentals/protection-customer-data.md)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Verwenden von Azure RBAC zum Steuern des Zugriffs auf Ressourcen 

**Leitfaden**: Verwenden Sie die rollenbasierte Zugriffssteuerung (Azure RBAC) in Azure, um den Zugriff auf die Steuerungsebene von Cognitive Services (d. h. das Azure-Portal) zu steuern. 

- [Konfigurieren von Azure RBAC](../role-based-access-control/role-assignments-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Verschlüsseln vertraulicher, ruhender Informationen

**Leitfaden**: Verschlüsselung im Ruhezustand für Cognitive Services hängt vom spezifischen Dienst ab, der verwendet wird. In den meisten Fällen werden Daten mittels FIPS 140-2-konformer 256-Bit-AES-Verschlüsselung ver- und entschlüsselt. Verschlüsselung und Entschlüsselung sind transparent, was bedeutet, dass die Verschlüsselung und der Zugriff für die Kunden von Microsoft verwaltet werden. Kundendaten werden standardmäßig geschützt, und Kunden müssen weder Code noch Anwendungen ändern, um die Verschlüsselung nutzen zu können.

Sie können außerdem Azure Key Vault zum Speichern Ihrer vom Kunden verwalteten Schlüssel verwenden. Sie können entweder Ihre eigenen Schlüssel erstellen und in einem Schlüsseltresor speichern oder mit den Azure Key Vault-APIs Schlüssel generieren.

- [Liste der Dienste, die Informationen im Ruhezustand verschlüsseln](/azure/cognitive-services/encryption/cognitive-services-encryption-keys-portal)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Protokollieren und Warnen bei Änderungen an wichtigen Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Monitor mit dem Azure-Aktivitätsprotokoll, um Warnungen für den Fall zu erstellen, dass Änderungen an Produktionsinstanzen von Cognitive Services und anderen kritischen bzw. verbundenen Ressourcen vorgenommen werden.

- [Erstellen von Warnungen für Ereignisse des Azure-Aktivitätsprotokolls](/azure/azure-monitor/platform/alerts-activity-log)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="inventory-and-asset-management"></a>Bestands- und Ressourcenverwaltung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Bestands- und Ressourcenverwaltung](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Verwenden der automatisierten Asset Discovery-Lösung

**Leitfaden**: Verwenden Sie Azure Resource Graph, um alle Ressourcen (etwa Computeressourcen, Speicher, Netzwerke, Ports, Protokolle usw.) in Ihren Abonnements abzufragen oder zu ermitteln. Stellen Sie entsprechende (Lese-) Berechtigungen in Ihrem Mandanten sicher, und zählen Sie alle Azure-Abonnements sowie Ressourcen in ihren Abonnements auf.

Obwohl klassische Azure-Ressourcen über das Resource Graph ermittelt werden können, wird dringend empfohlen, Azure Resource Manager-Ressourcen zu erstellen und zu verwenden.

- [Erstellen von Abfragen mit Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

- [Anzeigen Ihrer Azure-Abonnements](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-4.8.0&amp;preserve-view=true)

- [Grundlegendes zu Azure RBAC](../role-based-access-control/overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="62-maintain-asset-metadata"></a>6.2: Verwalten von Ressourcenmetadaten

**Leitfaden**: Wenden Sie Tags auf Ihre Azure-Ressourcen an, die Metadaten erzeugen, um sie logisch in einer Taxonomie zu organisieren.

- [Erstellen und Verwenden von Tags](../azure-resource-manager/management/tag-resources.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Löschen nicht autorisierter Azure-Ressourcen

**Leitfaden**: Verwenden Sie nach Bedarf Tagging, Verwaltungsgruppen und separate Abonnements zum Organisieren und Nachverfolgen von Azure Cache for Redis-Instanzen und damit verbundenen Ressourcen. Stimmen Sie den Bestand regelmäßig ab, und stellen Sie sicher, dass nicht autorisierte Ressourcen rechtzeitig aus dem Abonnement gelöscht werden.

Verwenden Sie darüber hinaus Azure Policy, um Einschränkungen für die Ressourcentypen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie dazu die folgenden integrierten Richtliniendefinitionen:

- Not allowed resource types (Unzulässige Ressourcentypen)
- Zulässige Ressourcentypen

- [Erstellen zusätzlicher Azure-Abonnements](../cost-management-billing/manage/create-subscription.md)

- [Erstellen von Verwaltungsgruppen](../governance/management-groups/create-management-group-portal.md)

- [Erstellen und Verwenden von Tags](../azure-resource-manager/management/tag-resources.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Überwachung auf nicht genehmigte Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy, um Einschränkungen für den Typ der Ressourcen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie dazu die folgenden integrierten Richtliniendefinitionen:

- Not allowed resource types (Unzulässige Ressourcentypen)
- Zulässige Ressourcentypen

Verwenden Sie darüber hinaus Azure Resource Graph, um Ressourcen in Ihren Abonnements abzufragen oder zu ermitteln.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Erstellen von Abfragen mit Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="69-use-only-approved-azure-services"></a>6.9: Ausschließliche Verwendung genehmigter Azure-Dienste

**Leitfaden**: Verwenden Sie Azure Policy, um Einschränkungen für den Typ der Ressourcen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie dazu die folgenden integrierten Richtliniendefinitionen:
- Not allowed resource types (Unzulässige Ressourcentypen)
- Zulässige Ressourcentypen

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Ablehnen eines bestimmten Ressourcentyps mit Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#general)

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

**Leitfaden**: Definieren und implementieren Sie mit Azure Policy Standardsicherheitskonfigurationen für Ihre Cognitive Services-Container. Verwenden Sie Azure Policy-Aliase im Namespace „Microsoft.CognitiveServices“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen der Konfiguration Ihrer Azure Cache for Redis-Instanzen zu erstellen.

- [Anzeigen verfügbarer Azure Policy-Aliase](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-4.8.0&amp;preserve-view=true)

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Verwalten von sicheren Konfigurationen für Azure-Ressourcen

**Leitfaden**: Verwenden Sie die Azure Policy-Einstellungen [deny] (Verweigern) und [deploy if not exist] (Bereitstellen, falls nicht vorhanden), um durchgängig sichere Einstellungen für Ihre Azure-Ressourcen zu erzwingen.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Grundlegendes zu Azure Policy-Auswirkungen](../governance/policy/concepts/effects.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Sicheres Speichern der Konfiguration von Azure-Ressourcen

**Leitfaden**: Wenn Sie benutzerdefinierte Azure Policy-Definitionen oder Azure Resource Manager-Vorlagen für Ihre Cognitive Services-Container und damit verbundene Ressourcen verwenden, nutzen Sie Azure Repos, um Code sicher zu speichern und zu verwalten.

- [Speichern von Code in Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops&amp;preserve-view=true)

- [Dokumentation zu Azure Repos](https://docs.microsoft.com/azure/devops/repos/?view=azure-devops&amp;preserve-view=true)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Bereitstellen von Konfigurationsverwaltungstools für Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy-Aliase im Namespace „Microsoft.Cache“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen von Systemkonfigurationen zu erstellen und Warnungen dazu zu erhalten. Entwickeln Sie außerdem einen Prozess und eine Pipeline zum Verwalten von Richtlinienausnahmen.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementieren der automatisierten Konfigurationsüberwachung für Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy-Aliase im Namespace „Microsoft.CognitiveServices“, um benutzerdefinierte Azure Policy-Definitionen zum Benachrichtigen, Überprüfen und Erzwingen von Systemkonfigurationen zu verwenden. Verwenden Sie die Azure Policy-Effekte [audit] (Überwachen), [deny] (Verweigern) und [deploy if not existent] (Bereitstellen, falls nicht vorhanden), um Konfigurationen für Ihre Azure Cache for Redis-Instanzen und damit verbundene Ressourcen automatisch zu erzwingen.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="711-manage-azure-secrets-securely"></a>7.11: Sicheres Verwalten von Azure-Geheimnissen

**Leitfaden**: Wenn Azure-VMs oder Webanwendungen, die in Azure App Service ausgeführt werden, für den Zugriff auf Ihre Cognitive Services-API verwendet werden, nutzen Sie die verwaltete Dienstidentität in Verbindung mit Azure Key Vault, um die Schlüsselverwaltung für Cognitive Services zu vereinfachen und zu schützen. Stellen Sie sicher, dass das vorläufige Löschen für Azure Key Vault aktiviert ist.

- [Integrieren mit verwalteten Azure-Identitäten](../azure-app-configuration/howto-integrate-azure-managed-service-identity.md)

- [Erstellen einer Key Vault-Instanz](/azure/key-vault/quick-create-portal)

- [Authentifizieren bei Key Vault](../key-vault/general/authentication.md)

- [Zuweisen einer Key Vault-Zugriffsrichtlinie](../key-vault/general/assign-access-policy-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Sicheres und automatisches Verwalten von Identitäten

**Leitfaden**: Wenn Azure-VMs oder Webanwendungen, die in Azure App Service ausgeführt werden, für den Zugriff auf Ihre Cognitive Services-API verwendet werden, nutzen Sie die verwaltete Dienstidentität in Verbindung mit Azure Key Vault, um die Schlüsselverwaltung für Cognitive Services zu vereinfachen und zu schützen. Stellen Sie sicher, dass das vorläufige Löschen für Azure Key Vault aktiviert ist.

Verwenden Sie verwaltete Identitäten, um Azure-Dienste mit einer automatisch verwalteten Identität in Azure Active Directory (AAD) bereitzustellen. Mit verwalteten Identitäten können Sie die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt, einschließlich Azure Key Vault. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein.

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

**Leitfaden**: Microsoft-Antischadsoftware ist auf dem zugrunde liegenden Host für die Azure-Dienste (z. B. Azure Cache for Redis) aktiviert, wird aber nicht für Kundeninhalte ausgeführt.

Führen Sie eine Vorabprüfung aller Inhalte durch, die in Azure-Ressourcen ohne Computeaufgaben hochgeladen werden sollen, z. B. App Service, Data Lake Storage, Blob Storage, Azure Database for PostgreSQL usw. Microsoft kann in diesen Fällen nicht auf Ihre Daten zugreifen.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="data-recovery"></a>Datenwiederherstellung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Datenwiederherstellung](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Sicherstellen regelmäßiger automatisierter Sicherungen

**Leitfaden**: Die Daten in Ihrem Microsoft Azure-Speicherkonto werden stets automatisch repliziert, um Beständigkeit und Hochverfügbarkeit sicherzustellen. Azure Storage kopiert Ihre Daten so, dass sie vor geplanten und ungeplanten Ereignissen geschützt sind – von vorübergehend auftretenden Hardwarefehlern und Netzwerk- oder Stromausfällen bis zu schweren Naturkatastrophen usw. Sie können Ihre Daten wahlweise im selben Rechenzentrum, Rechenzentren in derselben Region und sogar übergreifend über geografisch getrennte Regionen replizieren. 

Sie können außerdem ein Lebenszyklusverwaltungsfeature verwenden, um Daten auf der Archivebene zu sichern. Darüber hinaus können Sie das vorläufige Löschen für Ihre Sicherungen aktivieren, die im Speicherkonto gespeichert werden.

- [Grundlegendes zu Azure Storage-Redundanz und Vereinbarungen zum Servicelevel](../storage/common/storage-redundancy.md)

- [Verwalten des Azure Blob Storage-Lebenszyklus](../storage/blobs/storage-lifecycle-management-concepts.md)

- [Vorläufiges Löschen für Azure Storage-Blobs](../storage/blobs/soft-delete-blob-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Durchführen vollständiger Systemsicherungen und Sichern aller kundenseitig verwalteten Schlüssel

**Leitfaden**: Verwenden Sie Azure Resource Manager, um Cognitive Services und zugehörige Ressourcen bereitzustellen. Azure Resource Manager bietet die Möglichkeit zum Exportieren von Vorlagen, mit denen Sie Ihre Lösung während des Entwicklungslebenszyklus erneut bereitstellen und zugleich sichergehen können, dass Ihre Ressourcen in einem konsistenten Zustand bereitgestellt werden. Verwenden Sie Azure Automation, um die API zum Export von Azure Resource Manager-Vorlagen regelmäßig aufzurufen. Sichern Sie vorinstallierte Schlüssel in Azure Key Vault.

- [Übersicht über Azure Resource Manager](../azure-resource-manager/management/overview.md)

- [Erstellen einer Cognitive Services-Ressource mithilfe einer Azure Resource Manager-Vorlage](https://docs.microsoft.com/azure/cognitive-services/resource-manager-template?tabs=portal)

- [Exportieren von einzelnen oder mehreren Ressourcen in eine Vorlage im Azure-Portal](../azure-resource-manager/templates/export-template-portal.md)

- [Ressourcengruppen: Vorlage exportieren](/rest/api/resources/resourcegroups/exporttemplate)

- [Einführung in Azure Automation](../automation/automation-intro.md)

- [Sichern von Schlüsseltresorschlüsseln in Azure](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey?view=azps-4.8.0&amp;preserve-view=true)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Überprüfen aller Sicherungen einschließlich kundenseitig verwalteter Schlüssel

**Leitfaden**: Sicherstellen der Fähigkeit, die Bereitstellung von Azure Resource Manager-Vorlagen bei Bedarf regelmäßig in einem isolierten Abonnement auszuführen. Testen der Wiederherstellung von gesicherten vordefinierten Schlüsseln.

- [Bereitstellen von Ressourcen mit ARM-Vorlagen und dem Azure-Portal](../azure-resource-manager/templates/deploy-portal.md)

- [Wiederherstellen von Schlüsseltresorschlüsseln in Azure](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultkey?view=azps-4.8.0&amp;preserve-view=true)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Sicherstellen des Schutzes von Sicherungen und von kundenseitig verwalteten Schlüsseln

**Leitfaden**: Verwenden von Azure DevOps zum Speichern und Verwalten Ihrer Azure Resource Manager-Vorlagen. Um die Ressourcen, die Sie in Azure DevOps verwalten, zu schützen, können Sie bestimmten Benutzern, integrierten Sicherheitsgruppen oder in Azure Active Directory (Azure AD) definierten Gruppen (bei Integration in Azure DevOps) oder in Active Directory definierten Gruppen (bei Integration in Team Foundation Server) Berechtigungen erteilen oder verweigern.

Verwenden Sie die rollenbasierte Zugriffssteuerung in Azure, um kundenseitig verwaltete Schlüssel zu schützen. Aktivieren Sie vorläufiges Löschen und den Löschschutz in Key Vault, um Schlüssel vor versehentlichem oder böswilligem Löschen zu schützen. 

- [Speichern von Code in Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops&amp;preserve-view=true)

- [Informationen über Berechtigungen und Gruppen in Azure DevOps](/azure/devops/organizations/security/about-permissions)

- [Vorläufiges Löschen für Azure Storage-Blobs](../storage/blobs/soft-delete-blob-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="incident-response"></a>Reaktion auf Vorfälle

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Reaktion auf Vorfälle](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Erstellen eines Leitfadens für die Reaktion auf Vorfälle

**Anleitung:** Erarbeiten Sie einen Leitfaden für die Reaktion auf Vorfälle für Ihre Organisation. Stellen Sie sicher, dass es schriftliche Pläne für die Reaktion auf Vorfälle gibt, in denen alle Rollen der Mitarbeiter sowie die Phasen der Bearbeitung und Verwaltung von Vorfällen von der Ermittlung bis zur abschließenden Überprüfung definiert sind.

- [Leitfaden zu Planung und Betrieb](../security-center/security-center-planning-and-operations-guide.md)

- [Anleitung zum Entwickeln eines Prozesses für die Reaktion auf Sicherheitsvorfälle](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process)

- [„Computer Security Incident Handling Guide“ des National Institute of Standards and Technology (NIST)](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

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

**Leitfaden**: Exportieren Sie Ihre Security Center-Warnungen und -Empfehlungen mithilfe des Features „Fortlaufender Export“. Über „Fortlaufender Export“ können Sie Warnungen und Empfehlungen entweder manuell oder kontinuierlich exportieren. Sie können den Security Center-Datenconnector verwenden, um die Warnungen an Azure Sentinel zu streamen.

- [Konfigurieren des fortlaufenden Exports](../security-center/continuous-export.md)

- [Streamen von Warnungen in Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatisieren der Reaktion auf Sicherheitswarnungen

**Leitfaden**: Verwenden Sie die Funktion „Workflowautomatisierung“ in Security Center, um über „Logic Apps“ automatisch Reaktionen auf Sicherheitswarnungen und -empfehlungen auszulösen.

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
