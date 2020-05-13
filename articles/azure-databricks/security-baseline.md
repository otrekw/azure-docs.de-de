---
title: Azure-Sicherheitsbaseline für Azure Databricks
description: Azure-Sicherheitsbaseline für Azure Databricks
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 7d417f412592d87dec751b9d98a26bf786c719e5
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82793918"
---
# <a name="azure-security-baseline-for-azure-databricks"></a>Azure-Sicherheitsbaseline für Azure Databricks

Die Azure-Sicherheitsbaseline für Azure Databricks enthält Empfehlungen, mit deren Hilfe Sie den Sicherheitsstatus Ihrer Bereitstellung verbessern können.

Die Baseline für diesen Dienst wird von [Azure Security Benchmark-Version 1.0](https://docs.microsoft.com/azure/security/benchmarks/overview) abgeleitet, die Empfehlungen dazu enthält, wie Sie Ihre Cloudlösungen in Azure mithilfe unserer bewährten Methoden schützen können.

Weitere Informationen finden Sie in der [Übersicht über Azure-Sicherheitsbaselines](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Netzwerksicherheit

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Netzwerksicherheit](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1: Schützen von Ressourcen mithilfe von Netzwerksicherheitsgruppen oder Azure Firewall in Virtual Network

**Leitfaden**: Stellen Sie Azure Databricks in Ihrem eigenen virtuellen Azure-Netzwerk (VNET) bereit. Die standardmäßige Bereitstellung von Azure Databricks ist ein vollständig verwalteter Dienst in Azure: Alle Ressourcen der Datenebene (einschließlich eines VNET, dem alle Cluster zugeordnet werden) werden in einer gesperrten Ressourcengruppe bereitgestellt. Sollten Sie eine Netzwerkanpassung benötigen, können Sie die Datenebenenressourcen von Azure Databricks in Ihrem eigenen virtuellen Netzwerk bereitstellen (VNET-Einschleusung) und so benutzerdefinierte Netzwerkkonfigurationen implementieren. Sie können Ihre eigene Netzwerksicherheitsgruppe (NSG) mit benutzerdefinierten Regeln anwenden, um spezifische Einschränkungen für ausgehenden Datenverkehr zu erreichen.

Außerdem können Sie NSG-Regeln konfigurieren, um Einschränkungen für ausgehenden Datenverkehr für das Subnetz anzugeben, in dem Ihre Azure Databricks-Instanz bereitgestellt wird. Azure Firewall kann für Arbeitsbereiche mit eingeschleustem VNET konfiguriert werden.

* [Bereitstellen von Azure Databricks in Ihrem virtuellen Azure-Netzwerk (VNET-Einschleusung)](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject)

* [Erstellen, Ändern oder Löschen einer Netzwerksicherheitsgruppe](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2: Überwachen und Protokollieren der Konfiguration und des Datenverkehrs von VNETs, Subnetzen und Netzwerkkarten (NICs)

**Leitfaden**: Stellen Sie Azure Databricks in Ihrem eigenen virtuellen Azure-Netzwerk (VNET) bereit. Eine Netzwerksicherheitsgruppe (NSG) wird nicht automatisch erstellt. Sie müssen eine Baseline-NSG erstellen, die nur Azure-Standardregeln enthält. Wenn Sie einen Arbeitsbereich bereitstellen, werden die für Databricks erforderlichen Regeln hinzugefügt. Sie können auch mit einer leeren NSG beginnen. Die entsprechenden Regeln werden dann automatisch hinzugefügt. Aktivieren Sie NSG-Flussprotokolle, und senden Sie Protokolle an ein Speicherkonto für die Datenverkehrsüberwachung. Sie können auch Flussprotokolle an einen Log Analytics-Arbeitsbereich senden und mithilfe von Traffic Analytics Einblicke in den Datenverkehrsfluss Ihrer Azure-Cloud ermöglichen. Einige Vorteile von Traffic Analytics sind die Möglichkeit, die Netzwerkaktivität zu visualisieren und Hotspots zu erkennen, Sicherheitsbedrohungen zu identifizieren, Datenverkehrsflussmuster zu verstehen und Netzwerkfehlkonfigurationen zu ermitteln.

* [Bereitstellen von Azure Databricks in Ihrem virtuellen Azure-Netzwerk (VNET-Einschleusung)](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject)

* [Aktivieren der NSG-Flussprotokolle](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Aktivieren und Verwenden von Traffic Analytics](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

* [Aktivieren von Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-create)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="13-protect-critical-web-applications"></a>1.3: Schützen kritischer Webanwendungen

**Anleitung:** Nicht zutreffend. Diese Empfehlung ist für Webanwendungen gedacht, die in Azure App Service oder auf Computeressourcen ausgeführt werden.

**Azure Security Center-Überwachung:** Nicht verfügbar

**Verantwortlichkeit**: Nicht verfügbar

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Ablehnen der Kommunikation mit bekannten bösartigen IP-Adressen

**Leitfaden**: Aktivieren Sie Azure DDoS Protection Standard in den virtuellen Azure-Netzwerken, die Ihren Azure Databricks-Instanzen zugeordnet sind, um diese vor verteilten Denial-of-Service-Angriffen zu schützen. Verwenden Sie die in Azure Security Center integrierte Threat Intelligence, um die Kommunikation mit bekannten schädlichen oder nicht genutzten öffentlichen IP-Adressen zu verweigern.

* [Verwalten von Azure DDoS Protection Standard mithilfe des Azure-Portals](https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection)

* [Bedrohungsschutz in Azure Security Center](https://docs.microsoft.com/azure/security-center/threat-protection#threat-protection-for-azure-network-layer-)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="15-record-network-packets-and-flow-logs"></a>1.5: Aufzeichnen von Netzwerkpaketen und Flussprotokollen

**Leitfaden**: Stellen Sie Azure Databricks in Ihrem eigenen virtuellen Azure-Netzwerk (VNET) bereit. Eine Netzwerksicherheitsgruppe (NSG) wird nicht automatisch erstellt. Sie müssen eine Baseline-NSG erstellen, die nur Azure-Standardregeln enthält. Wenn Sie einen Arbeitsbereich bereitstellen, werden die für Databricks erforderlichen Regeln hinzugefügt. Aktivieren Sie NSG-Flussprotokolle, und senden Sie Protokolle an ein Speicherkonto für die Datenverkehrsüberwachung. Sie können auch Flussprotokolle an einen Log Analytics-Arbeitsbereich senden und mithilfe von Traffic Analytics Einblicke in den Datenverkehrsfluss Ihrer Azure-Cloud ermöglichen. Einige Vorteile von Traffic Analytics sind die Möglichkeit, die Netzwerkaktivität zu visualisieren und Hotspots zu erkennen, Sicherheitsbedrohungen zu identifizieren, Datenverkehrsflussmuster zu verstehen und Netzwerkfehlkonfigurationen zu ermitteln.

* [Bereitstellen von Azure Databricks in Ihrem virtuellen Azure-Netzwerk (VNET-Einschleusung)](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject)

* [Aktivieren der NSG-Flussprotokolle](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Aktivieren und Verwenden von Traffic Analytics](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

* [Aktivieren von Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-create)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Bereitstellen von netzwerkbasierten Angriffserkennungs-/Eindringschutzsystemen (IDS/IPS)

**Leitfaden**: Implementieren Sie ein IDP/IPS-fähiges virtuelles Netzwerkgerät (Network Virtual Appliance, NVA) aus dem Azure Marketplace, um einen Arbeitsbereich mit VNET-Integration zu erstellen, in dem alle Azure Databricks-Cluster über eine einzelne ausgehende IP-Adresse verfügen. Die einzelne IP-Adresse kann als zusätzliche Sicherheitsebene mit anderen Azure-Diensten und -Anwendungen verwendet werden, die den Zugriff basierend auf bestimmten IP-Adressen zulassen. Sie können eine Azure Firewall-Instanz oder andere Drittanbietertools (beispielsweise ein virtuelles Netzwerkgerät) verwenden, um ein- und ausgehenden Datenverkehr zu verwalten.

Wenn Angriffserkennung und/oder -verhinderung auf der Grundlage der Nutzlastüberprüfung keine Anforderung ist, kann Azure Firewall mit Threat Intelligence verwendet werden. Das Filtern auf Basis von Threat Intelligence mit Azure Firewall kann vor Datenverkehr von und zu bekannten schädlichen IP-Adressen oder Domänen warnen und diesen verweigern. Die IP-Adressen und Domänen stammen aus dem Microsoft Threat Intelligence-Feed.

* [How to Assign a Single Public IP for VNet-Injected Workspaces Using Azure Firewall](https://docs.microsoft.com/azure/databricks/kb/cloud/azure-vnet-single-ip) (Zuweisen einer einzelnen öffentlichen IP-Adresse für Arbeitsbereiche mit eingeschleustem VNET unter Verwendung von Azure Firewall)

* [Erstellen eines virtuellen Netzwerkgeräts](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-portal)

* [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="17-manage-traffic-to-web-applications"></a>1.7: Verwalten von Datenverkehr für Webanwendungen

**Anleitung:** Nicht zutreffend. Diese Empfehlung ist für Webanwendungen gedacht, die in Azure App Service oder auf Computeressourcen ausgeführt werden.

**Azure Security Center-Überwachung:** Nicht verfügbar

**Verantwortlichkeit**: Nicht verfügbar

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimieren der Komplexität und des Verwaltungsaufwands von Netzwerksicherheitsregeln

**Leitfaden**: Verwenden Sie Diensttags, um Netzwerkzugriffssteuerungen für Netzwerksicherheitsgruppen zu definieren, die an die Subnetze angefügt sind, mit denen Ihre Azure Databricks-Instanz verknüpft ist. Sie können Diensttags anstelle von spezifischen IP-Adressen nutzen, wenn Sie Sicherheitsregeln erstellen. Indem Sie den Diensttagnamen (z. B. „ApiManagement“) im entsprechenden Quell- oder Zielfeld einer Regel angeben, können Sie den Datenverkehr für den entsprechenden Dienst zulassen oder verweigern. Microsoft verwaltet die Adresspräfixe, für die das Diensttag gilt, und aktualisiert das Diensttag automatisch, wenn sich die Adressen ändern. Diensttags werden für Arbeitsbereiche ohne VNET-Einschleusung nicht unterstützt.

* [Diensttags in virtuellen Netzwerken](https://docs.microsoft.com/azure/virtual-network/service-tags-overview)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Beibehalten von Standardsicherheitskonfigurationen für Netzwerkgeräte

**Leitfaden**: Definieren und implementieren Sie Netzwerksicherheitskonfigurationen für Ihre Azure Databricks-Instanzen mit Azure Policy. Sie können Azure Policy-Aliase im Namespace „Microsoft.Databricks“ verwenden, um benutzerdefinierte Richtliniendefinitionen zu erstellen. Richtlinien gelten nicht für Ressourcen innerhalb der verwalteten Ressourcengruppe.

Sie können auch Azure Blueprints verwenden, um umfangreiche Azure-Bereitstellungen zu vereinfachen, indem wichtige Umgebungsartefakte wie Azure Resource Manager-Vorlagen, die rollenbasierte Zugriffssteuerung (RBAC) und Richtlinien in einer einzelnen Blaupausendefinition zusammengefasst werden. Sie können die Blaupause leicht auf neue Abonnements und Umgebungen anwenden und die Steuerung und Verwaltung durch die Versionsverwaltung optimieren.

* [Konfigurieren und Verwalten von Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Struktur von Azure Policy-Definitionen](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure)

* [Erstellen einer Azure-Blaupause](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="110-document-traffic-configuration-rules"></a>1.10: Dokumentieren von Datenverkehrskonfigurationsregeln

**Leitfaden**: Verwenden Sie Tags für NSGs und andere Ressourcen im Zusammenhang mit Netzwerksicherheit und Datenverkehrsfluss, die mit Ihrer Azure Databricks-Instanz verknüpft sind. Verwenden Sie für einzelne NSG-Regeln das Feld „Beschreibung“, um geschäftliche Anforderungen und/oder deren Dauer (usw.) für alle Regeln festzulegen, die Datenverkehr in ein bzw. aus einem Netzwerk zulassen.

* [Erstellen und Verwenden von Tags](httphttps://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Verwenden automatisierter Tools zum Überwachen von Netzwerkressourcenkonfigurationen und Erkennen von Änderungen

**Leitfaden**: Verwenden Sie das Azure-Aktivitätsprotokoll zum Überwachen der Konfigurationen von Netzwerkressourcen und zum Erkennen von Änderungen bei Netzwerkressourcen im Zusammenhang mit Ihren Azure Databricks-Instanzen. Erstellen Sie Warnungen in Azure Monitor, die bei Änderungen an wichtigen Netzwerkressourcen ausgelöst werden.

* [Anzeigen und Abrufen von Azure-Aktivitätsprotokollereignissen](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [Erstellen von Warnungen in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="logging-and-monitoring"></a>Protokollierung und Überwachung

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Protokollierung und Überwachung](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1: Verwenden von genehmigten Zeitsynchronisierungsquellen

**Leitfaden**: Microsoft verwaltet Zeitquellen für Azure-Ressourcen. Sie haben jedoch die Möglichkeit, die Zeitsynchronisierungseinstellungen für Ihre Computeressourcen zu verwalten. Da es sich bei Azure Databricks um einen PaaS-Dienst handelt, haben Sie keinen direkten Zugriff auf die virtuellen Computer in einem Azure Databricks-Cluster und können keine Einstellungen für die Zeitsynchronisierung einrichten.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2.2: Konfigurieren der zentralen Sicherheitsprotokollverwaltung

**Leitfaden**: Erfassen Sie Protokolle über Azure Monitor, um die von Azure Databricks generierten Sicherheitsdaten zu aggregieren. In Azure Monitor können Sie den Log Analytics-Arbeitsbereich abfragen, der für den Empfang Ihrer Databricks- und Diagnoseprotokolle konfiguriert ist. Verwenden Sie Azure-Speicherkonten, um Protokolle langfristig zu speichern bzw. zu archivieren, oder Event Hubs, um Daten in andere Systeme zu exportieren. Alternativ dazu können Sie auch Daten in Azure Sentinel oder einer Drittanbieter-Security Incident and Event Management-Lösung (SIEM) aktivieren und integrieren.

Hinweis: Für Azure Databricks-Diagnoseprotokolle ist der Azure Databricks Premium-Plan erforderlich.

* [Erstellen von Diagnoseeinstellungen im Azure-Portal](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings#create-diagnostic-settings-in-azure-portal)

* [Durchführen des Onboardings für Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

* [Erste Schritte mit Azure Monitor und der Integration einer SIEM-Drittanbieterlösung](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Aktivieren der Überwachungsprotokollierung für Azure-Ressourcen

**Leitfaden**: Aktivieren Sie die Diagnoseeinstellungen für das Azure-Aktivitätsprotokoll, und senden Sie die Protokolle zwecks Archivierung an einen Log Analytics-Arbeitsbereich, eine Azure Event Hub-Instanz oder ein Azure-Speicherkonto. Mit den Daten aus Azure-Aktivitätsprotokollen können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge (PUT, POST, DELETE) ermitteln, die auf Steuerungsebene für Ihre Azure-Ressourcen ausgeführt werden.

Für die Überwachungsprotokollierung werden von Azure Databricks umfassende End-to-End-Diagnoseprotokolle von Aktivitäten bereitgestellt, die von Azure Databricks-Benutzern ausgeführt wurden, was dem Unternehmen die Überwachung detaillierter Azure Databricks-Verwendungsmuster ermöglicht.

Hinweis: Für Azure Databricks-Diagnoseprotokolle ist der Azure Databricks Premium-Plan erforderlich.

* [Erfassen und Analysieren des Azure-Aktivitätsprotokolls in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Diagnoseprotokollierung in Azure Databricks](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs)

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: Erfassen von Sicherheitsprotokollen von Betriebssystemen

**Leitfaden**: Von Azure Databricks werden umfassende End-to-End-Diagnoseprotokolle von Aktivitäten bereitgestellt, die von Azure Databricks-Benutzern ausgeführt wurden, was dem Unternehmen die Überwachung detaillierter Azure Databricks-Verwendungsmuster ermöglicht.

Hinweis: Für Azure Databricks-Diagnoseprotokolle ist der Azure Databricks Premium-Plan erforderlich. Die Betriebssystem-Sicherheitsprotokollierung ist nicht verfügbar.

* [Erfassen und Analysieren des Azure-Aktivitätsprotokolls in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Diagnoseprotokollierung in Azure Databricks](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs)

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="25-configure-security-log-storage-retention"></a>2.5: Konfigurieren der Sicherheitsprotokoll-Aufbewahrungsdauer im Speicher

**Leitfaden**: Aktivieren Sie Diagnoseeinstellungen für Azure Databricks. Wenn Sie Protokolle in einem Log Analytics-Arbeitsbereich speichern, legen Sie den Aufbewahrungszeitraum Ihres Log Analytics-Arbeitsbereichs gemäß den Compliancebestimmungen Ihrer Organisation fest. Verwenden Sie Azure Storage-Konten für die langfristige/Archivierungsspeicherung. Sicherheitsbezogene Aktivitäten werden in Databricks-Überwachungsprotokollen nachverfolgt.

Hinweis: Für Azure Databricks-Diagnoseprotokolle ist der Azure Databricks Premium-Plan erforderlich.

* [Diagnoseprotokollierung in Azure Databricks](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs)

* [Ändern des Datenaufbewahrungszeitraums](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="26-monitor-and-review-logs"></a>2.6: Überwachen und Überprüfen von Protokollen

**Leitfaden**: Aktivieren Sie Diagnoseeinstellungen für Azure Databricks, und senden Sie Protokolle an einen Log Analytics-Arbeitsbereich. Verwenden Sie den Log Analytics-Arbeitsbereich, um Ihre Azure Databricks-Protokolle zu analysieren und auf anomales Verhalten zu überwachen, und prüfen Sie regelmäßig die Ergebnisse.

Alternativ dazu können Sie auch Daten in Azure Sentinel oder einer Drittanbieter-SIEM-Lösung aktivieren und integrieren.

Hinweis: Für Azure Databricks-Diagnoseprotokolle ist der Azure Databricks Premium-Plan erforderlich.

* [Diagnoseprotokollierung in Azure Databricks](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs)

* [Analysieren von Diagnoseprotokollen](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs#analyze-diagnostic-logs)

* [Durchführen des Onboardings für Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="27-enable-alerts-for-anomalous-activity"></a>2.7: Aktivieren von Warnungen für anormale Aktivitäten

**Leitfaden**: Konfigurieren Sie Diagnoseeinstellungen für Ihre Azure Databricks-Instanz, und senden Sie Protokolle an einen Log Analytics-Arbeitsbereich. Konfigurieren Sie in Ihrem Log Analytics-Arbeitsbereich Warnungen, die ausgelöst werden sollen, wenn vordefinierte Bedingungen eintreten.

Alternativ dazu können Sie auch Daten in Azure Sentinel oder einer Drittanbieter-SIEM-Lösung aktivieren und integrieren.

Hinweis: Für Azure Databricks-Diagnoseprotokolle ist der Azure Databricks Premium-Plan erforderlich.

* [Konfigurieren der Diagnoseprotokollübermittlung](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs#configure-diagnostic-log-delivery)

* [Erstellen, Anzeigen und Verwalten von Protokollwarnungen mithilfe von Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log)

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="28-centralize-anti-malware-logging"></a>2.8: Zentralisieren der Antischadsoftwareprotokollierung

**Leitfaden**: Nicht zutreffend

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Nicht verfügbar

### <a name="29-enable-dns-query-logging"></a>2.9: Aktivieren der DNS-Abfrageprotokollierung

**Leitfaden**: Nicht verfügbar. Von Azure Databricks werden keine DNS-bezogenen Protokolle verarbeitet oder erzeugt, auf die Benutzer zugreifen können.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Nicht verfügbar

### <a name="210-enable-command-line-audit-logging"></a>2.10: Aktivieren der Befehlszeilen-Überwachungsprotokollierung

**Leitfaden**: Nicht zutreffend. Diese Leitlinie ist für Computeressourcen vorgesehen.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Nicht verfügbar

## <a name="identity-and-access-control"></a>Identität und Zugriffssteuerung

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Identität und Zugriffssteuerung](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Verwalten eines Bestands von Administratorkonten

**Leitfaden**: Sie können Azure Databricks-SCIM-APIs verwenden, um Benutzer in einem Azure Databricks-Arbeitsbereich zu verwalten und bestimmten Benutzern Administratorrechte zu erteilen. Die Verwaltung ist aber auch über die Azure Databricks-Benutzeroberfläche möglich.

* [SCIM-API](https://docs.microsoft.com/azure/databricks/dev-tools/api/latest/scim/)

* [Verwalten von Benutzern](https://docs.microsoft.com/azure/databricks/administration-guide/users-groups/users)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Ändern von Standardkennwörtern bei Bedarf

**Leitfaden**: Von Azure Databricks wird Azure Active Directory (AD) verwendet, um Zugriff auf das Azure-Portal und auf die Azure Databricks-Verwaltungskonsole zu gewähren. Azure AD verfügt nicht über Standardkennwörter. Es liegt allerdings in Ihrer Verantwortung, Standardkennwörter für benutzerdefinierte Anwendungen oder Drittanbieteranwendungen zu ändern oder nicht zuzulassen.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Verwenden dedizierter Administratorkonten

**Leitfaden**: Sie können Azure Databricks-SCIM-APIs verwenden, um Benutzer mit Administratorberechtigungen in einer Azure Databricks-Instanz hinzuzufügen. Die Verwaltung ist aber auch über die Azure Databricks-Benutzeroberfläche möglich.

* [SCIM-API](https://docs.microsoft.com/azure/databricks/dev-tools/api/latest/scim/)

* [Verwalten von Benutzern](https://docs.microsoft.com/azure/databricks/administration-guide/users-groups/users)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4: Verwenden des einmaligen Anmeldens (Single Sign-On, SSO) mit Azure Active Directory

**Leitfaden**: Azure Databricks wird automatisch so eingerichtet, dass für die Benutzerauthentifizierung das einmalige Anmelden (Single Sign-On, SSO) mit Azure Active Directory verwendet wird. Benutzer außerhalb Ihrer Organisation müssen den Einladungsprozess durchlaufen und Ihrem Active Directory-Mandanten hinzugefügt werden, um sich per SSO bei Azure Databricks anmelden zu können. Sie können SCIM implementieren, um die Benutzerbereitstellung und die Aufhebung der Benutzerbereitstellung für Arbeitsbereiche zu automatisieren.

* [SCIM-API](https://docs.microsoft.com/azure/databricks/dev-tools/api/latest/scim/)

* [Einrichten des einmaligen Anmeldens](https://docs.microsoft.com/azure/databricks/administration-guide/users-groups/single-sign-on/)

* [Hinzufügen oder Entfernen von Rollenzuweisungen für externe Gastbenutzer mithilfe von Azure RBAC und dem Azure-Portal](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-external-users)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Microsoft

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Verwenden der mehrstufigen Authentifizierung für den gesamten Azure Active Directory-basierten Zugriff

**Leitfaden**: Aktivieren Sie Azure AD MFA, und befolgen Sie die Empfehlungen für die Identitäts- und Zugriffsverwaltung in Azure Security Center.

* [Aktivieren von MFA in Azure](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

* [Überwachen von Identität und Zugriff in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Verwenden dedizierter Computer (Arbeitsstationen mit privilegiertem Zugriff) für alle administrativen Aufgaben

**Leitfaden**: Verwenden Sie Arbeitsstationen mit privilegiertem Zugriff (Privileged Access Workstations, PAWs) mit MFA, die für die Anmeldung bei und die Konfiguration von Azure-Ressourcen konfiguriert sind.

* [Informationen zu Arbeitsstationen mit privilegiertem Zugriff](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)

* [Aktivieren von MFA in Azure](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7: Protokollieren und Warnen bei verdächtigen Aktivitäten in Administratorkonten

**Leitfaden**: Verwenden Sie Azure Active Directory-Sicherheitsberichte, um Protokolle und Warnungen bei verdächtigen oder sicherheitsrelevanten Aktivitäten in der Umgebung zu generieren. Verwenden Sie Azure Security Center zum Überwachen von identitäts- und zugriffsbezogenen Aktivitäten. Darüber hinaus können Sie die Azure Databricks-Diagnoseprotokolle nutzen.

* [Identifizieren von Azure AD-Benutzern, die aufgrund riskanter Aktivitäten gekennzeichnet wurden](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk)

* [Überwachen der identitäts- und zugriffsbezogenen Aktivitäten von Benutzern in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Verwalten von Azure-Ressourcen nur über genehmigte Standorte

**Leitfaden**: Verwenden Sie benannte Standorte mit bedingtem Zugriff, um den Zugriff nur über bestimmte logische Gruppierungen von IP-Adressbereichen oder Ländern/Regionen zuzulassen.

* [Konfigurieren benannter Standorte in Azure](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="39-use-azure-active-directory"></a>3.9: Verwenden von Azure Active Directory

**Leitfaden**: Azure Databricks wird automatisch so eingerichtet, dass für die Benutzerauthentifizierung das einmalige Anmelden (Single Sign-On, SSO) mit Azure Active Directory verwendet wird. Benutzer außerhalb Ihrer Organisation müssen den Einladungsprozess durchlaufen und Ihrem Active Directory-Mandanten hinzugefügt werden, um sich per SSO bei Azure Databricks anmelden zu können.

* [Einrichten des einmaligen Anmeldens](https://docs.microsoft.com/azure/databricks/administration-guide/users-groups/single-sign-on/)

* [Hinzufügen oder Entfernen von Rollenzuweisungen für externe Gastbenutzer mithilfe von Azure RBAC und dem Azure-Portal](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-external-users)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Microsoft

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Regelmäßiges Überprüfen und Abstimmen des Benutzerzugriffs

**Leitfaden**: Azure AD enthält Protokolle zum Ermitteln von veralteten Konten. Verwenden Sie zusätzlich Zugriffsüberprüfungen für Azure-Identitäten, um Gruppenmitgliedschaften, den Zugriff auf Unternehmensanwendungen sowie Rollenzuweisungen effizient zu verwalten. Der Benutzerzugriff kann regelmäßig überprüft werden, um sicherzustellen, dass nur die richtigen Benutzer weiterhin Zugriff haben. Sie können auch SCIM-APIs und Azure Databricks-Diagnoseprotokolle implementieren, um den Benutzerzugriff zu überprüfen. Sie können auch SCIM-APIs und Azure Databricks-Diagnoseprotokolle verwenden, um den Benutzerzugriff zu überprüfen.

Überprüfen und verwalten Sie außerdem regelmäßig den Benutzerzugriff in der Azure Databricks-Verwaltungskonsole.

* [Azure AD-Berichterstellung](https://docs.microsoft.com/azure/active-directory/reports-monitoring/)

* [Was sind Azure AD-Zugriffsüberprüfungen?](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

* [Verwalten von Benutzern](https://docs.microsoft.com/azure/databricks/administration-guide/users-groups/users)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11: Überwachen von Zugriffsversuchen auf deaktivierte Konten

**Leitfaden**: Sie haben Zugriff auf die Protokollquellen von Azure AD-Anmeldeaktivitäten und von Überwachungs- und Risikoereignissen, sodass die Integration in jedes SIEM- und Überwachungstool möglich ist. Darüber hinaus können Sie Azure Databricks-Diagnoseprotokolle verwenden, um Benutzerzugriffsaktivitäten zu überprüfen.

Sie können diesen Prozess optimieren, indem Sie Diagnoseeinstellungen für Azure Active Directory-Benutzerkonten erstellen und die Überwachungs- und Anmeldeprotokolle an einen Log Analytics-Arbeitsbereich senden. Sie können gewünschte Warnungen im Log Analytics-Arbeitsbereich konfigurieren.

* [SCIM-API](https://docs.microsoft.com/azure/databricks/dev-tools/api/latest/scim/)

* [Integrieren von Azure-Aktivitätsprotokollen in Azure Monitor](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Warnung bei abweichendem Verhalten bei der Kontoanmeldung

**Leitfaden**: Mit Azure AD-Funktionen zum Risiko- und Identitätsschutz können Sie automatische Antworten auf erkannte verdächtige Aktionen im Zusammenhang mit Benutzeridentitäten konfigurieren. Außerdem können Sie Daten zur weiteren Untersuchung in Azure Sentinel erfassen. Darüber hinaus können Sie Azure Databricks-Diagnoseprotokolle verwenden, um Benutzerzugriffsaktivitäten zu überprüfen.

* [Anzeigen riskanter Azure AD-Anmeldungen](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

* [Konfigurieren und Aktivieren von Risikorichtlinien für den Identitätsschutz](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies)

* [Durchführen des Onboardings für Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Ermöglichen des Zugriffs auf relevante Kundendaten für Microsoft in Supportszenarien

**Leitfaden**: Wenn Supporttickets offen sind, fordern Kundendienst und Supporttechniker die Einwilligung für den Zugriff auf relevante Kundendaten an.

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Zurzeit nicht verfügbar

## <a name="data-protection"></a>Schutz von Daten

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Datenschutz](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Verwalten eines Bestands an vertraulichen Informationen

**Leitfaden**: Verwenden Sie Tags für die Nachverfolgung von Azure Databricks-Instanzen, von denen vertrauliche Informationen gespeichert oder verarbeitet werden.

* [Erstellen und Verwenden von Tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Isolieren von Systemen, die vertrauliche Informationen speichern oder verarbeiten

**Leitfaden**: Implementieren Sie separate Abonnements und/oder Verwaltungsgruppen für Entwicklungs-, Test- und Produktionsumgebungen. Bei der Standardbereitstellung von Azure Databricks handelt es sich um einen vollständig verwalteten Dienst, der innerhalb seines eigenen virtuellen Netzwerks bereitgestellt wird. Sollten Sie eine Netzwerkanpassung benötigen, können Sie Azure Databricks in Ihrem eigenen virtuellen Netzwerk bereitstellen. Es empfiehlt sich, separate Azure Databricks-Arbeitsbereiche für verschiedene Unternehmensteams oder Abteilungen zu erstellen.

* [Bereitstellen von Azure Databricks in Ihrem virtuellen Azure-Netzwerk (VNET-Einschleusung)](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject)

* [Erstellen zusätzlicher Azure-Abonnements](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Erstellen von Verwaltungsgruppen](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Erstellen eines virtuellen Netzwerks](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Überwachen und Blockieren einer nicht autorisierten Übertragung vertraulicher Informationen.

**Leitfaden**: Orientieren Sie sich an der Databricks-Architektur zum Schutz vor Exfiltration, um das Datenexfiltrationsrisiko zu senken.

* [Schutz vor Datenexfiltration mit Azure Databricks](https://databricks.com/blog/2020/03/27/data-exfiltration-protection-with-azure-databricks.html)

Microsoft verwaltet die zugrunde liegende Infrastruktur für Azure Databricks und hat strenge Kontrollen implementiert, um den Verlust oder die Offenlegung von Kundendaten zu verhindern.

* [Grundlegendes zum Schutz von Kundendaten in Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Zurzeit nicht verfügbar

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Verschlüsseln aller vertraulichen Informationen während der Übertragung

**Leitfaden**: Von Microsoft wird standardmäßig TLS 1.2 ausgehandelt, wenn Ihre Azure Databricks-Instanz über das Azure-Portal oder über die Azure Databricks-Konsole verwaltet wird. Die Databricks-Web-App unterstützt TLS 1.3.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Microsoft

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Verwenden eines aktiven Ermittlungstools zur Bestimmung vertraulicher Daten

**Leitfaden**: Derzeit nicht verfügbar. Features für die Datenidentifizierung und -klassifizierung sowie zur Vermeidung von Datenverlusten sind derzeit für Azure Databricks nicht verfügbar. Versehen Sie Azure Databricks-Instanzen und zugehörige Ressourcen, von denen vertrauliche Informationen verarbeitet werden, mit Tags, und implementieren Sie bei Bedarf eine Drittanbieterlösung für Compliancezwecke.

Die Databricks-Plattform ist eine reine Computeplattform. Sämtliche Daten werden in anderen Azure-Datendiensten gespeichert. Für die zugrundeliegende Plattform, die von Microsoft verwaltet wird, behandelt Microsoft alle Kundeninhalte als vertraulich und unternimmt große Anstrengungen, um Kundendaten vor Verlust und Gefährdung zu schützen. Um die Sicherheit von Kundendaten innerhalb von Azure zu gewährleisten, hat Microsoft eine Reihe von robusten Datenschutzkontrollen und -funktionen implementiert und kümmert sich um deren Verwaltung.

* [Grundlegendes zum Schutz von Kundendaten in Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Zurzeit nicht verfügbar

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Verwenden von Azure RBAC zum Steuern des Zugriffs auf Ressourcen

**Leitfaden**: In Azure Databricks können Sie mithilfe von Zugriffssteuerungslisten (Access Control Lists, ACLs) Berechtigung für den Zugriff auf Datentabellen, Cluster, Pools, Aufträge und Arbeitsbereichsobjekte wie Notebooks, Experimente und Ordner konfigurieren. Neben allen Administratorbenutzern können auch Benutzer Zugriffssteuerungslisten verwalten, denen delegierte Berechtigungen zum Verwalten von Zugriffssteuerungslisten erteilt wurden. Berechtigungen für den Azure Databricks-Arbeitsbereich können per Azure RBAC festgelegt werden.

Hinweis: Die Zugriffssteuerung für Tabellen, Cluster, Pools, Aufträge und Arbeitsbereiche ist nur im Azure Databricks Premium-Plan verfügbar.

* [Aktivieren der Zugriffssteuerung](https://docs.microsoft.com/azure/databricks/administration-guide/access-control/)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Verwenden der hostbasierten Verhinderung von Datenverlusten zum Erzwingen der Zugriffssteuerung

**Leitfaden**: Nicht zutreffend. Diese Empfehlung ist für Computeressourcen vorgesehen.

Microsoft verwaltet die zugrunde liegende Infrastruktur für Azure Databricks und hat strenge Kontrollen implementiert, um den Verlust oder die Offenlegung von Kundendaten zu verhindern.

* [Grundlegendes zum Schutz von Kundendaten in Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Nicht verfügbar

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Verschlüsseln vertraulicher, ruhender Informationen

**Leitfaden**: Ein Azure Databricks-Arbeitsbereich umfasst eine Verwaltungsebene, die in einem von Azure Databricks verwalteten virtuellen Netzwerk gehostet wird, sowie eine Datenebene, die in einem kundenseitig verwalteten virtuellen Netzwerk bereitgestellt wird. Auf der Steuerungsebene werden die Notebooks aller Benutzer und die zugehörigen Notebookergebnisse in einer Datenbank gespeichert. Standardmäßig werden alle Notebooks und Ergebnisse im Ruhezustand mit einem anderen Verschlüsselungsschlüssel verschlüsselt.

Das Databricks-Dateisystem (Databricks File System, DBFS) ist ein verteiltes Dateisystem, das in einen Azure Databricks-Arbeitsbereich eingebunden und in Azure Databricks-Clustern verfügbar ist. DBFS wird als Speicherkonto in der verwalteten Ressourcengruppe Ihres Azure Databricks-Arbeitsbereichs implementiert. Das Speicherkonto wird standardmäßig mit von Microsoft verwalteten Schlüsseln verschlüsselt. Ihre Daten werden in den Azure-Datendiensten gespeichert, die Sie besitzen, und Sie haben die Möglichkeit, sie zu verschlüsseln. DBFS sollte nicht zum Speichern von Produktionsdaten verwendet werden.

Hinweis: Diese Features sind nicht für alle Azure Databricks-Abonnements verfügbar. Wenden Sie sich an den für Sie zuständigen Microsoft- oder Databricks-Kontobeauftragten, um Zugriff anzufordern.

* [Aktivieren von kundenseitig verwalteten Schlüsseln für Notebooks](https://docs.microsoft.com/azure/databricks/security/customer-managed-key-notebook)

* [Konfigurieren kundenseitig verwalteter Schlüssel für Standard-DBFS (Stamm)](https://docs.microsoft.com/azure/databricks/security/customer-managed-keys-dbfs)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Shared

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Protokollieren und Warnen bei Änderungen an wichtigen Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Monitor mit dem Azure-Aktivitätsprotokoll, um Warnungen zu erstellen, die bei Änderungen an wichtigen Azure Databricks-Arbeitsbereichen ausgegeben werden.

* [Erstellen von Warnungen für Ereignisse des Azure-Aktivitätsprotokolls](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="vulnerability-management"></a>Verwaltung von Sicherheitsrisiken

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Verwaltung von Sicherheitsrisiken](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Ausführen automatisierter Scan-Tools für Sicherheitsrisiken

**Leitfaden**: Implementieren Sie eine Drittanbieterlösung zur Verwaltung von Sicherheitsrisiken.

Wenn Sie über ein Abonnement für eine Plattform zur Verwaltung von Sicherheitsrisiken verfügen, können Sie Azure Databricks-Initialisierungsskripts verwenden, um Agents für die Sicherheitsrisikobewertung auf Ihren Azure Databricks-Clusterknoten zu installieren, und die Knoten über das entsprechende Portal verwalten. Beachten Sie, dass jede Drittanbieterlösung anders funktioniert.

* [Manuelles Installieren des Rapid7-Agents](https://insightagent.help.rapid7.com/docs/install)

* [Manuelles Installieren des Qualys-Agents](https://www.qualys.com/docs/qualys-cloud-agent-linux-install-guide.pdf)

* [Initialisierungsskripts für Clusterknoten](https://docs.microsoft.com/azure/databricks/clusters/init-scripts)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Bereitstellen der automatisierten Lösung für die Patchverwaltung von Betriebssystemen

**Leitfaden**: Von Microsoft werden zwar die Basisimages für Azure Databricks-Clusterknoten verwaltet, Sie müssen jedoch dafür sorgen, dass Ihre Clusterknoten stets gepatcht sind. Wenn Sie einem vorhandenen aktiven Cluster ein Wartungsupdate hinzufügen möchten, muss der Cluster neu gestartet werden.

* [Runtime-Wartungsupdates für Databricks](https://docs.microsoft.com/azure/databricks/release-notes/runtime/maintenance-updates)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3: Bereitstellen einer automatisierten Lösung für die Patchverwaltung von Drittanbietersoftware

**Leitfaden**: Von Microsoft werden zwar die Basisimages für Azure Databricks-Clusterknoten verwaltet, Sie müssen jedoch dafür sorgen, dass ggf. von Ihnen installierte Drittanbieteranwendungen stets gepatcht sind.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4 Vergleichen von kaskadierenden Sicherheitsrisikoscans

**Leitfaden**: Implementieren Sie eine Drittanbieterlösung für die Verwaltung von Sicherheitsrisiken, mit der Überprüfungen auf Sicherheitsrisiken im Zeitverlauf verglichen werden können. Falls Sie über ein Abonnement für eine Lösung zur Verwaltung von Sicherheitsrisiken verfügen, können Sie das Portal des Anbieters verwenden, um aufeinanderfolgende Überprüfungen auf Sicherheitsrisiken anzuzeigen und zu vergleichen. Beachten Sie, dass jede Drittanbieterlösung anders funktioniert.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Verwenden Sie einen Risikobewertungsprozess, um die Behebung von erkannten Sicherheitsrisiken zu priorisieren.

**Leitfaden**: Verwenden Sie ein gängiges Risikobewertungsprogramm (z. B. Common Vulnerability Scoring System) oder die von Ihrem Scantool eines Drittanbieters bereitgestellten Standardrisikobewertungen.

**Azure Security Center-Überwachung**: –

**Verantwortlichkeit**: Kunde

## <a name="inventory-and-asset-management"></a>Bestands- und Ressourcenverwaltung

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Bestands- und Ressourcenverwaltung](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-azure-asset-discovery"></a>6.1: Verwenden von Azure Asset Discovery

**Leitfaden**: Verwenden Sie Azure Resource Graph, um alle Ressourcen (z. B. Computeressourcen, Speicher, Netzwerke, Ports und Protokolle usw.) in ihren Abonnements abzufragen bzw. zu ermitteln. Stellen Sie sicher, dass in Ihrem Mandanten geeignete (Lese-)Berechtigungen vorhanden sind, und zählen Sie alle Azure-Abonnements sowie Ressourcen in Ihren Abonnements auf.

Obwohl klassische Azure-Ressourcen über das Resource Graph ermittelt werden können, wird dringend empfohlen, Azure Resource Manager-Ressourcen zu erstellen und zu verwenden.

* [Erstellen von Abfragen mit Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

* [Anzeigen Ihrer Azure-Abonnements](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

* [Was ist die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) für Azure-Ressourcen?](https://docs.microsoft.com/azure/role-based-access-control/overview)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="62-maintain-asset-metadata"></a>6.2: Verwalten von Ressourcenmetadaten

**Leitfaden**: Wenden Sie Tags auf Ihre Azure-Ressourcen an, die Metadaten erzeugen, um sie logisch in einer Taxonomie zu organisieren.

* [Erstellen und Verwenden von Tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Löschen nicht autorisierter Azure-Ressourcen

**Leitfaden**: Verwenden Sie Tagging, Verwaltungsgruppen und separate Abonnements nach Bedarf, um Ressourcen zu verwalten und nachzuverfolgen. Stimmen Sie den Bestand regelmäßig ab, und stellen Sie sicher, dass nicht autorisierte Ressourcen rechtzeitig aus dem Abonnement gelöscht werden.

Verwenden Sie darüber hinaus Azure Policy, um Einschränkungen für den Typ der Ressourcen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie dazu die folgenden integrierten Richtliniendefinitionen:
- Not allowed resource types (Unzulässige Ressourcentypen)
- Zulässige Ressourcentypen

* [Erstellen zusätzlicher Azure-Abonnements](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Erstellen von Verwaltungsgruppen](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Erstellen und Verwenden von Tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="64-maintain-inventory-of-approved-azure-resources-and-software-titles"></a>6.4: Verwalten eines Bestands genehmigter Azure-Ressourcen und Softwaretitel.

**Leitfaden**: Definieren Sie genehmigte Azure-Ressourcen und genehmigte Software für Computeressourcen.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Überwachung auf nicht genehmigte Azure-Ressourcen

**Leitfaden**: Verwenden Sie eine Azure-Richtlinie, um Einschränkungen für den Typ der Ressourcen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie dazu die folgenden integrierten Richtliniendefinitionen:
- Not allowed resource types (Unzulässige Ressourcentypen)
- Zulässige Ressourcentypen

Verwenden Sie Azure Resource Graph, um Ressourcen in ihren Abonnements abzufragen und zu ermitteln. Stellen Sie sicher, dass alle in der Umgebung vorhandenen Azure-Ressourcen genehmigt sind.

* [Konfigurieren und Verwalten von Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Erstellen von Abfragen mit Azure Graph](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Überwachen auf nicht genehmigte Softwareanwendungen innerhalb von Computeressourcen

**Leitfaden**: Nicht verfügbar. Bei Azure Databricks handelt es sich um einen PaaS-Dienst. Kunden haben keinen direkten Zugriff auf die virtuellen Computer in einem Azure Databricks-Cluster.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Entfernen nicht genehmigter Azure-Ressourcen und Softwareanwendungen

**Leitfaden**: Verwenden Sie Azure Resource Graph, um alle Ressourcen (z. B. Computeressourcen, Speicher, Netzwerk, Ports und Protokolle) in Ihren Abonnements abzufragen bzw. zu ermitteln (einschließlich Azure Databricks-Instanzen). Entfernen Sie alle nicht genehmigten Azure-Ressourcen, die Sie finden. Implementieren Sie für Azure Databricks-Cluster eine Drittanbieterlösung, um nicht genehmigte Software zu entfernen bzw. entsprechende Warnungen zu generieren.

* [Erstellen von Abfragen mit Azure Graph](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="68-use-only-approved-applications"></a>6.8: Ausschließliche Verwendung genehmigter Anwendungen

**Leitfaden**: Nicht verfügbar. Bei Azure Databricks handelt es sich um einen PaaS-Dienst. Sie haben keinen direkten Zugriff auf die virtuellen Computer in einem Azure Databricks-Cluster.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="69-use-only-approved-azure-services"></a>6.9: Ausschließliche Verwendung genehmigter Azure-Dienste

**Leitfaden**: Verwenden Sie Azure Policy, um Einschränkungen für den Typ der Ressourcen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie hierzu die folgenden integrierten Richtliniendefinitionen:
- Not allowed resource types (Unzulässige Ressourcentypen)
- Zulässige Ressourcentypen

* [Konfigurieren und Verwalten von Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Ablehnen eines bestimmten Ressourcentyps mit Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="610-implement-approved-application-list"></a>6.10: Implementieren einer Liste genehmigter Anwendungen

**Leitfaden**: Nicht verfügbar. Bei Azure Databricks handelt es sich um einen PaaS-Dienst. Sie haben keinen direkten Zugriff auf die virtuellen Computer in einem Azure Databricks-Cluster.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="611-divlimit-users-ability-to-interact-with-azure-resource-manager-via-scriptsdiv"></a>6.11: <div>Einschränken der Möglichkeiten der Benutzer zur Interaktion mit Azure Resource Manager über Skripts</div>

**Leitfaden**: Verwenden Sie den bedingten Azure-Zugriff, um die Möglichkeiten der Benutzer zur Interaktion mit Azure Resource Manager einzuschränken, indem Sie „Zugriff blockieren“ für die App zur „Verwaltung von Microsoft Azure“ konfigurieren.

* [Verwalten des Zugriffs auf die Azure-Verwaltung mit bedingtem Zugriff](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: Einschränken der Skriptausführung durch Benutzer innerhalb von Computeressourcen

**Leitfaden**: Nicht verfügbar. Dies ist für Azure Databricks nicht relevant, da Clusterbenutzer (ohne Administratorberechtigungen) keinen Zugriff auf die einzelnen Knoten benötigen, um Aufträge auszuführen. Der Clusteradministrator verfügt standardmäßig über root-Zugriff auf alle Clusterknoten.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Nicht verfügbar

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Physische oder logische Trennung von Anwendungen mit hohem Risiko

**Leitfaden**: Nicht anwendbar. Die Benchmark ist für Azure App Service- oder Computeressourcen vorgesehen, auf denen Webanwendungen gehostet werden.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Nicht verfügbar

## <a name="secure-configuration"></a>Sichere Konfiguration

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Sichere Konfiguration](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Einrichten sicherer Konfigurationen für alle Azure-Ressourcen

**Leitfaden**: Definieren und implementieren Sie Standardsicherheitskonfigurationen für Ihre Azure Databricks-Instanzen mit Azure Policy. Verwenden Sie Azure Policy-Aliase im Namespace „Microsoft.Databricks“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen der Konfiguration Ihrer Azure Databricks-Instanzen zu erstellen. Beachten Sie, dass die angewendeten Richtlinien für die verwaltete Databricks-Ressourcengruppe nicht funktionieren.

* [Anzeigen verfügbarer Azure Policy-Aliase](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [Konfigurieren und Verwalten von Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: Einrichten sicherer Betriebssystemkonfigurationen

**Leitfaden**: Nicht verfügbar. Bei Azure Databricks handelt es sich um einen PaaS-Dienst. Sie haben keinen direkten Zugriff auf die virtuellen Computer in einem Azure Databricks-Cluster.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Verwalten von sicheren Konfigurationen für Azure-Ressourcen

**Leitfaden**: Definieren und implementieren Sie Standardsicherheitskonfigurationen für Ihre Azure Databricks-Instanzen mit Azure Policy. Verwenden Sie Azure Policy-Aliase im Namespace „Microsoft.Databricks“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen der Konfiguration Ihrer Azure Databricks-Instanzen zu erstellen. Verwenden Sie die Azure-Richtlinien [Verweigern] und [Bereitstellen, falls nicht vorhanden], um sichere Einstellungen in den Azure-Ressourcen zu erzwingen.

* [Konfigurieren und Verwalten von Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Grundlegendes zu Azure Policy-Auswirkungen](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Verwalten sicherer Betriebssystemkonfigurationen

**Leitfaden**: Azure Databricks-Betriebssystemimages werden von Microsoft verwaltet und gepflegt. Sie sind verantwortlich für die Implementierung der Zustandskonfiguration auf Betriebssystemebene.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Shared

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Sicheres Speichern der Konfiguration von Azure-Ressourcen

**Leitfaden**: Wenn Sie benutzerdefinierte Azure-Richtliniendefinitionen verwenden, nutzen Sie Azure DevOps oder Azure Repos, um Code sicher zu speichern und zu verwalten.

* [Speichern von Code in Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Dokumentation zu Azure Repos](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: Sicheres Speichern von benutzerdefinierten Betriebssystemimages

**Leitfaden**: Sollten Sie benutzerdefinierte Images für Ihre Azure Databricks-Clusterknoten verwenden, pushen Sie Ihr benutzerdefiniertes Basisimage an eine Docker-Registrierung wie Azure Container Registry (ACR).

* [Anpassen von Containern mit Databricks-Containerdiensten](https://docs.microsoft.com/azure/databricks/clusters/custom-containers)

* [Einführung in private Docker-Containerregistrierungen in Azure](https://docs.microsoft.com/azure/container-registry/container-registry-intro)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="77-deploy-system-configuration-management-tools"></a>7.7: Bereitstellen von Verwaltungstools zur Systemkonfiguration

**Leitfaden**: Definieren und implementieren Sie Standardsicherheitskonfigurationen für Ihre Azure Databricks-Instanzen mit Azure Policy. Verwenden Sie Azure Policy-Aliase im Namespace „Microsoft.Databricks“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen der Konfiguration Ihrer Azure Databricks-Instanzen zu erstellen.

* [Konfigurieren und Verwalten von Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Anzeigen verfügbarer Azure Policy-Aliase](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [Konfigurieren und Verwalten von Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8: Bereitstellen von Verwaltungstools für Systemkonfigurationen für Betriebssysteme

**Leitfaden**: Nicht verfügbar. Bei Azure Databricks handelt es sich um einen PaaS-Dienst. Sie haben keinen direkten Zugriff auf die virtuellen Computer in einem Azure Databricks-Cluster.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9: Implementieren der automatisierten Konfigurationsüberwachung für Azure-Dienste

**Leitfaden**: Definieren und implementieren Sie Standardsicherheitskonfigurationen für Ihre Azure Databricks-Instanzen mit Azure Policy. Verwenden Sie Azure Policy-Aliase im Namespace „Microsoft.Databricks“, um benutzerdefinierte Richtlinien zum Überwachen oder Erzwingen der Konfiguration Ihrer Azure Databricks-Instanzen zu erstellen.

* [Anzeigen verfügbarer Azure Policy-Aliase](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [Konfigurieren und Verwalten von Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: Implementieren der automatisierten Konfigurationsüberwachung für Betriebssysteme

**Leitfaden**: Implementieren Sie eine Drittanbieterlösung, um den Zustand der Betriebssysteme Ihrer Azure Databricks-Clusterknoten zu überwachen.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="711-manage-azure-secrets-securely"></a>7.11: Sicheres Verwalten von Azure-Geheimnissen

**Leitfaden**: Verwenden Sie Azure Key Vault mit einem Azure Databricks-Geheimnisbereich, um Geheimnisse sicher zu verwalten und zu verwenden.

* [Tutorial: Zugreifen auf Azure Blob Storage über Azure Databricks unter Verwendung von Azure Key Vault](https://docs.microsoft.com/azure/azure-databricks/store-secrets-azure-key-vault)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Sicheres und automatisches Verwalten von Identitäten

**Leitfaden**: Derzeit nicht verfügbar. Für Azure Databricks stehen gegenwärtig keine verwalteten Identitäten zur Verfügung.

* [Dienste, die verwaltete Identitäten für Azure-Ressourcen unterstützen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities)

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Zurzeit nicht verfügbar

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Beheben der unbeabsichtigten Offenlegung von Anmeldeinformationen

**Anleitung:** Implementieren Sie Credential Scanner, um Anmeldeinformationen im Code zu identifizieren. In Credential Scanner wird auch das Verschieben von ermittelten Anmeldeinformationen an sicherere Speicherorte (z. B. Azure Key Vault) empfohlen.

* [Einrichten von Credential Scanner](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="malware-defense"></a>Schutz vor Schadsoftware

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Schutz vor Schadsoftware](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1: Verwenden einer zentral verwalteten Antischadsoftware

**Leitfaden**: Wenn Sie über ein Abonnement für eine Plattform zur Verwaltung von Sicherheitsrisiken verfügen, können Sie Azure Databricks-Initialisierungsskripts verwenden, um Agents für die Sicherheitsrisikobewertung auf Ihren Azure Databricks-Clusterknoten zu installieren, und die Knoten über das entsprechende Portal verwalten. Beachten Sie, dass jede Drittanbieterlösung anders funktioniert.

* [Manuelles Installieren des Rapid7-Agents](https://insightagent.help.rapid7.com/docs/install)

* [Manuelles Installieren des Qualys-Agents](https://www.qualys.com/docs/qualys-cloud-agent-linux-install-guide.pdf)

* [Initialisierungsskripts für Clusterknoten](https://docs.microsoft.com/azure/databricks/clusters/init-scripts)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: Vorabprüfen von in computefremde Azure-Ressourcen hochzuladenden Dateien

**Leitfaden**: Microsoft Antimalware ist auf dem zugrunde liegenden Host für die Azure-Dienste (z. B. Azure App Service) aktiviert, wird jedoch nicht für Ihre Inhalte ausgeführt.

Führen Sie eine Vorabprüfung aller Dateien durch, die in Ihre Azure Databricks-Clusterknoten oder in zugehörige Ressourcen hochgeladen werden.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>Schritt 8.3: Sicherstellen der Aktualisierung von Antischadsoftware und Signaturen

**Leitfaden**: Wenn Sie über ein Abonnement für eine Plattform zur Verwaltung von Sicherheitsrisiken verfügen, können Sie Azure Databricks-Initialisierungsskripts verwenden, um Agents für die Sicherheitsrisikobewertung auf Ihren Azure Databricks-Clusterknoten zu installieren, und die Knoten über das entsprechende Portal verwalten. Beachten Sie, dass jede Drittanbieterlösung anders funktioniert.

* [Manuelles Installieren des Rapid7-Agents](https://insightagent.help.rapid7.com/docs/install)

* [Manuelles Installieren des Qualys-Agents](https://www.qualys.com/docs/qualys-cloud-agent-linux-install-guide.pdf)

* [Initialisierungsskripts für Clusterknoten](https://docs.microsoft.com/azure/databricks/clusters/init-scripts)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="data-recovery"></a>Datenwiederherstellung

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Datenwiederherstellung](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Sicherstellen regelmäßiger automatisierter Sicherungen

**Leitfaden**: Stellen Sie sicher, dass Sie für Ihre Azure Databricks-Datenquellen eine für Ihren Anwendungsfall angemessene Datenredundanz konfiguriert haben. Wenn Sie also beispielsweise ein Azure Storage-Konto für Ihren Azure Databricks-Datenspeicher verwenden, wählen Sie die passende Redundanzoption (LRS, ZRS, GRS, RA-GRS) aus.

* [Datenquellen für Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/databricks-connect-to-data-sources#data-sources-for-azure-databricks)

* [Regionale Notfallwiederherstellung für Azure Databricks-Cluster](https://docs.microsoft.com/azure/azure-databricks/howto-regional-disaster-recovery)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Durchführen vollständiger Systemsicherungen und Sichern aller von Kunden verwalteten Schlüssel

**Leitfaden**: Sichern Sie kundenseitig verwaltete Schlüssel im Zusammenhang mit Ihren Azure Databricks-Implementierungen in Azure Key Vault. Sie können auch die REST-API und die Befehlszeilenschnittstelle verwenden, um eine tägliche Sicherung von Databricks-Konfigurationen zu erstellen. Sie können auch die REST-API/Befehlszeilenschnittstelle verwenden, um eine tägliche Sicherung von Databricks-Konfigurationen zu erstellen.

* [Sichern von Schlüsseltresorschlüsseln in Azure](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Überprüfen aller Sicherungen einschließlich der von Kunden verwalteten Schlüssel

**Leitfaden**: Testen Sie die Wiederherstellung gesicherter kundenseitig verwalteter Schlüssel im Zusammenhang mit Ihren Azure Databricks-Implementierungen.

* [Wiederherstellen von Schlüsseltresorschlüsseln in Azure](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Sicherstellen des Schutzes von Sicherungen und von kundenseitig verwalteten Schlüsseln

**Leitfaden**: Vergewissern Sie sich, dass das vorläufige Löschen in Key Vault aktiviert ist, um Schlüssel vor versehentlichem oder böswilligem Löschen zu schützen.

* [Vorläufiges Löschen für Blobspeicher](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

## <a name="incident-response"></a>Reaktion auf Vorfälle

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Reaktion auf Vorfälle](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Erstellen eines Leitfadens für die Reaktion auf Vorfälle

**Anleitung:** Erarbeiten Sie einen Leitfaden für die Reaktion auf Vorfälle für Ihre Organisation. Stellen Sie sicher, dass es schriftliche Pläne für die Reaktion auf Vorfälle gibt, in denen alle Rollen der Mitarbeiter sowie die Phasen der Bearbeitung und Verwaltung von Vorfällen von der Ermittlung bis zur abschließenden Überprüfung definiert sind.

* [Leitfaden zu Planung und Betrieb](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)

* [Anleitung zum Entwickeln eines Prozesses für die Reaktion auf Sicherheitsvorfälle](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Struktur eines Vorfalls laut Microsoft Security Response Center](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Sie können außerdem den Computer Security Incident Handling Guide des US-amerikanischen National Institute of Standards and Technology (NIST) nutzen, um einen Plan zur Reaktion auf Incidents auszuarbeiten.](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Erstellen eines Verfahrens zur Bewertung und Priorisierung von Vorfällen

**Anleitung:** Security Center weist jeder Warnung einen Schweregrad zu, damit Sie priorisieren können, welche Warnungen zuerst untersucht werden sollen. Der Schweregrad basiert darauf, wie zuversichtlich Security Center in Bezug auf den Befund oder die Analyse ist, die zum Auslösen der Warnung verwendet wird, sowie auf dem Zuverlässigkeitsgrad, dass hinter der Aktivität, die zu der Warnung führte, eine böswillige Absicht stand.

Kennzeichnen Sie außerdem die Abonnements (z. B. Produktion, Nicht-Produktion), und erstellen Sie ein Namenssystem, um Azure-Ressourcen eindeutig zu identifizieren und zu kategorisieren.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="103-test-security-response-procedures"></a>10.3: Verfahren zum Testen der Reaktion auf Sicherheitsvorfälle

**Leitfaden**: Führen Sie in regelmäßigen Abständen Tests zur Reaktionsfähigkeit Ihrer Systeme auf Vorfälle durch. Identifizieren Sie Schwachstellen und Lücken, und überarbeiten Sie den Plan bei Bedarf.

* [„Guide to Test, Training, and Exercise Programs for IT Plans and Capabilities“ des National Institute of Standards and Technology (NIST)](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Angeben von Kontaktdaten für Sicherheitsvorfälle und Konfigurieren von Warnungsbenachrichtigungen für Sicherheitsvorfälle

**Leitfaden**: Microsoft kontaktiert Sie unter den für Sicherheitsvorfälle angegebenen Kontaktdaten, wenn das Microsoft Security Response Center (MSRC) feststellt, dass Personen unrechtmäßig oder unbefugt auf die Kundendaten zugegriffen haben. Überprüfen Sie die Vorfälle anschließend, um sicherzustellen, dass die Probleme behoben wurden.

* [Festlegen der Kontaktinformationen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Integrieren von Sicherheitswarnungen in das System zur Reaktion auf Vorfälle

**Leitfaden**: Exportieren Sie die Azure Security Center-Warnungen und -Empfehlungen über die Funktion „Fortlaufender Export“. Über „Fortlaufender Export“ können Sie Warnungen und Empfehlungen entweder manuell oder kontinuierlich exportieren. Sie können den Azure Security Center-Datenconnector verwenden, um die Warnungen an Sentinel zu streamen.

* [Konfigurieren des fortlaufenden Exports](https://docs.microsoft.com/azure/security-center/continuous-export)

* [Streamen von Warnungen in Azure Sentinel](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatisieren der Reaktion auf Sicherheitswarnungen

**Leitfaden**: Verwenden Sie die Funktion „Workflowautomatisierung“ in Azure Security Center, um über „Logic Apps“ automatisch Reaktionen auf Sicherheitswarnungen und -empfehlungen auszulösen.

* [Konfigurieren von Workflowautomatisierung und Logic Apps](https://docs.microsoft.com/azure/security-center/workflow-automation)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="penetration-tests-and-red-team-exercises"></a>Penetrationstests und Red Team-Übungen

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Penetrationstests und Red Team-Übungen](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11.1: Führen Sie regelmäßige Penetrationstests für Ihre Azure-Ressourcen durch, und stellen Sie sicher, dass alle kritischen Sicherheitsergebnisse innerhalb von 60 Tagen behandelt werden.

**Leitfaden:** * Befolgen Sie die [Einsatzregeln für Penetrationstests](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1), damit diese alle Anforderungen der Microsoft-Richtlinien erfüllen.

* Weitere Informationen zur Microsoft-Strategie im Zusammenhang mit Red Team- und Livewebsite-Penetrationstests für von Microsoft verwaltete Cloudinfrastrukturen, Dienste und Anwendungen sowie zu deren Durchführung finden Sie [hier](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e).

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Shared

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie den [Vergleichstest für die Azure-Sicherheit](https://docs.microsoft.com/azure/security/benchmarks/overview).
- Erfahren Sie mehr über [Azure-Sicherheitsbaselines](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).
