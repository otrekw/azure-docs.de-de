---
title: Azure-Sicherheitsbaseline für Azure Load Balancer
description: Die Azure Load Balancer-Sicherheitsbaseline enthält Anweisungen zur Vorgehensweise und Ressourcen für die Implementierung der Sicherheitsempfehlungen im Vergleichstest für die Azure-Sicherheit.
author: msmbaldwin
ms.service: load-balancer
ms.topic: conceptual
ms.date: 03/16/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: bffc9eb3e75dda2b04ad4118d1f599f85a0013c2
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104590139"
---
# <a name="azure-security-baseline-for-azure-load-balancer"></a>Azure-Sicherheitsbaseline für Azure Load Balancer

Diese Sicherheitsbaseline wendet Empfehlungen aus [Version 1.0 des Azure-Sicherheitsvergleichstests](../security/benchmarks/overview-v1.md) auf Microsoft Azure Load Balancer an. Der Azure-Sicherheitsvergleichstest enthält Empfehlungen zum Schutz Ihrer Cloudlösungen in Azure.
Der Inhalt ist anhand der **Sicherheitskontrollen** gruppiert, die durch den Azure-Sicherheitsvergleichstest und die entsprechenden für Azure Load Balancer geltenden Empfehlungen definiert werden. Nicht auf Azure Load Balancer anwendbare **Kontrollen** wurden ausgeschlossen.

 
Informationen zur vollständigen Zuordnung von Azure Load Balancer zum Azure-Sicherheitsvergleichstest finden Sie in der [vollständigen Zuordnungsdatei für die Azure Load Balancer-Sicherheitsbaseline](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Netzwerksicherheit

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Netzwerksicherheit](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Schützen von Azure-Ressourcen in virtuellen Netzwerken

**Leitfaden**: Verwenden interner Azure Load Balancer, um Datenverkehr zu Back-End-Ressourcen nur aus bestimmten virtuellen Netzwerken oder virtuellen Peeringnetzwerken ohne Gefährdung durch das Internet zuzulassen. Implementieren Sie eine externe Load Balancer-Instanz mit SNAT

(Source Network Address Translation), um die IP-Adressen von Back-End-Ressourcen zum Schutz vor direkter Gefährdung durch das Internet zu tarnen.

Azure bietet zwei Typen von Load Balancern: Standard und Basic. Verwenden Sie Load Balancer Standard für alle Produktionsworkloads. Implementieren Sie Netzwerksicherheitsgruppen, und gewähren Sie nur den Zugriff auf die vertrauenswürdigen Ports und IP-Adressbereiche Ihrer Anwendung. In Fällen, in denen dem Back-End-Subnetz oder der NIC der virtuellen Back-End-Computer keine Netzwerksicherheitsgruppe zugewiesen ist, ist Datenverkehr für diese Ressourcen vom Load Balancer nicht zulässig. Stellen Sie mit Load Balancer Standard Ausgangsregeln bereit, um ausgehende NAT mit einer Netzwerksicherheitsgruppe zu definieren. Überprüfen Sie diese Ausgangsregeln, um das Verhalten ausgehender Verbindungen zu optimieren.

Die Verwendung einer Load Balancer Standard-Instanz wird für Ihre Produktionsworkloads empfohlen, und in der Regel wird Load Balancer Basic nur für Tests verwendet, da der Basistyp für Verbindungen aus dem Internet standardmäßig geöffnet ist und keine Netzwerksicherheitsgruppen für den Betrieb benötigt.

- [Ausgehende Verbindungen in Azure](load-balancer-outbound-connections.md)

- [Upgraden einer öffentlichen Azure Load Balancer-Instanz](upgrade-basic-standard.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.Network**:

[!INCLUDE [Resource Policy for Microsoft.Network 1.1](../../includes/policy/standards/asb/rp-controls/microsoft.network-1-1.md)]

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1.2: Überwachen und Protokollieren der Konfiguration und des Datenverkehrs von virtuellen Netzwerken, Subnetzen und Netzwerkkarten (NICs)

**Leitfaden**: Load Balancer ist ein Pass-Through-Dienst, da er die Regeln der Netzwerksicherheitsgruppen verwendet, die auf Back-End-Ressourcen angewendet werden, sowie auf die konfigurierten Ausgangsregeln, um den Internetzugang zu steuern.

Überprüfen Sie die für Ihre Load Balancer Standard-Instanz konfigurierten Regeln für ausgehenden Datenverkehr auf dem Blatt „Ausgangsregeln“ Ihrer Load Balancer-Instanz und dem Blatt „Lastenausgleichsregeln“, auf dem Sie möglicherweise implizite Regeln für ausgehenden Datenverkehr aktiviert haben.

Überwachen Sie die Anzahl der ausgehenden Verbindungen, um nachzuverfolgen, wie oft ihre Ressourcen auf das Internet zugreifen. 

Verwenden Sie Security Center, und befolgen Sie die Empfehlungen für den Netzwerkschutz, um Ihre Azure-Netzwerkressourcen abzusichern.

Befolgen Sie die Sicherheitsempfehlungen für Ihre Back-End-Ressourcen, und aktivieren Sie Netzwerksicherheitsgruppen-Datenflussprotokolle. Senden Sie Protokolle zur Überwachung an ein Azure Storage-Konto.

Senden Sie die Datenflussprotokolle auch an einen Log Analytics-Arbeitsbereich, und verwenden Sie dann Traffic Analytics, um Einblicke in die Datenverkehrsmuster in Ihrer Azure-Cloud zu ermöglichen. Vorteile von Traffic Analytics beinhalten die Möglichkeit, die Netzwerkaktivität zu visualisieren, Hotspots und Sicherheitsbedrohungen zu erkennen, Datenverkehrsflussmuster zu verstehen und Netzwerkfehlkonfigurationen zu ermitteln.

- [Aktivieren von Datenflussprotokollen für Netzwerksicherheitsgruppen](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Aktivieren und Verwenden von Traffic Analytics](../network-watcher/traffic-analytics.md)

- [Grundlegendes zu der von Azure Security Center bereitgestellten Netzwerksicherheit](../security-center/security-center-network-recommendations.md)

- [Wie überprüfe ich meine Statistiken für ausgehende Verbindungen?](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics#how-do-i-check-my-outbound-connection-statistics)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.Network**:

[!INCLUDE [Resource Policy for Microsoft.Network 1.2](../../includes/policy/standards/asb/rp-controls/microsoft.network-1-2.md)]

### <a name="13-protect-critical-web-applications"></a>1.3: Schützen kritischer Webanwendungen

**Leitfaden**: Definieren Sie mit Ihrer Load Balancer-Instanz explizit Internet-Konnektivität und gültige IP-Quelladressen durch Ausgangsregeln und Netzwerksicherheitsgruppen, um Threat Intelligence von Microsoft zum Schutz Ihrer Webanwendungen zu nutzen.

- [Integrieren von Azure Firewall](../firewall/integrate-lb.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Ablehnen der Kommunikation mit bekannten bösartigen IP-Adressen

**Leitfaden**: Aktivieren Sie den Azure DDoS-Standardschutz (Distributed Denial of Service) in den virtuellen Azure-Netzwerken, um sie vor DDoS-Angriffen zu schützen. 

Stellen Sie die Azure Firewall an allen Netzwerkgrenzen der Organisation mit aktivierter, auf Threat Intelligence basierender Filterung bereit, und konfigurieren Sie das Feature so, dass schädlicher Netzwerkdatenverkehr „mit Warnungen gemeldet und verweigert“ wird.

 

Verwenden Sie Azure Security Center-Bedrohungsschutz, um die Kommunikation mit bekannten schädlichen IP-Adressen zu erkennen. 

Load Balancer Standard ist standardmäßig sicher und Bestandteil eines privaten und isolierten Virtual Network. Er ist für eingehende Datenflüsse geschlossen, es sei denn, er wird von Netzwerksicherheitsgruppen geöffnet, um zulässigen Datenverkehr explizit zuzulassen und bekannte schädliche IP-Adressen zu verweigern. Wenn keine Netzwerksicherheitsgruppe in einem Subnetz oder einer NIC Ihrer virtuellen Computerressource hinter der Load Balancer-Instanz vorhanden ist, kann Datenverkehr diese Ressource nicht erreichen. 

Stellen Sie Azure Firewall an allen Netzwerkgrenzen der Organisation mit aktivierter, auf Threat Intelligence basierender Filterung bereit, und konfigurieren Sie das Feature so, dass schädlicher Netzwerkdatenverkehr „mit Warnungen gemeldet und verweigert“ wird, um Angriffe von schädlichen IP-Adressen zu verhindern. 

Implementieren Sie Anleitungen zum Integrieren von Azure Firewall in Ihre Load Balancer-Instanz.

Verwenden Sie Bedrohungsschutzfunktionen von Azure Security Center, um die Kommunikation mit bekannten schädlichen IP-Adressen zu erkennen. 

Security Center (Standardtarif) bietet Just-in-Time-Zugriff auf virtuelle Computer und konfiguriert zulässige IP-Quelladressen, um Zugriff nur von genehmigten IP-Adressen/Bereichen zuzulassen.
 

Verwenden Sie die Funktion für adaptive Netzwerkhärtung in Azure Security Center, um Netzwerksicherheitsgruppenkonfigurationen zu empfehlen, die Ports und IP-Quelladressen basierend auf tatsächlichem Datenverkehr und Threat Intelligence einschränken.
 

- [Verwalten von Azure DDoS Protection Standard mithilfe des Azure-Portals](../ddos-protection/manage-ddos-protection.md)

- [Threat Intelligence-gestütztes Filtern für Azure Firewall](../firewall/threat-intel.md)

- [Bedrohungsschutz in Azure Security Center](../security-center/azure-defender.md)

- [Sichern Ihrer Verwaltungsports mit Just-in-Time-Zugriff (JIT)](../security-center/security-center-just-in-time.md)

- [Adaptive Netzwerkhärtung in Azure Security Center](../security-center/security-center-adaptive-network-hardening.md)

- [Integrieren von Azure Firewall in Ihre Load Balancer-Instanz](../firewall/overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.Network**:

[!INCLUDE [Resource Policy for Microsoft.Network 1.4](../../includes/policy/standards/asb/rp-controls/microsoft.network-1-4.md)]

### <a name="15-record-network-packets"></a>1.5: Aufzeichnen von Netzwerkpaketen

**Leitfaden**: Aktivieren Sie die Network Watcher-Paketerfassung, um anomale Aktivitäten zu untersuchen.

- [Erstellen einer Network Watcher-Instanz](../network-watcher/network-watcher-create.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.Network**:

[!INCLUDE [Resource Policy for Microsoft.Network 1.5](../../includes/policy/standards/asb/rp-controls/microsoft.network-1-5.md)]

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Bereitstellen von netzwerkbasierten Angriffserkennungs-/Eindringschutzsystemen (Intrusion Detection/Intrusion Prevention Systems, IDS/IPS)

**Leitfaden**: Implementieren eines Angebots aus Azure Marketplace, das IDS-/IPS-Funktionen mit Funktionen zur Nutzlastuntersuchung für die Umgebung Ihres Load Balancers unterstützt. 

Wenn Nutzlastuntersuchung keine Anforderung ist, verwenden Sie die Threat Intelligence von Azure Firewall. Die auf Azure Firewall Threat Intelligence basierende Filterung wird verwendet, um bei bekannten böswilligen IP-Adressen und Domänen zu warnen und/oder Datenverkehr zu und von diesen Adressen oder Domänen zu blockieren. Die IP-Adressen und Domänen stammen aus dem Microsoft Threat Intelligence-Feed.

Stellen Sie die Firewalllösung Ihrer Wahl an allen Netzwerkgrenzen Ihrer Organisation bereit, um schädlichen Datenverkehr zu erkennen und/oder zu blockieren.

- [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

- [Bereitstellen von Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md)

- [Konfigurieren von Warnungen mit Azure Firewall](../firewall/threat-intel.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="17-manage-traffic-to-web-applications"></a>1.7: Verwalten von Datenverkehr für Webanwendungen

**Leitfaden**: Definieren Sie mit Ihrer Load Balancer-Instanz explizit Internet-Konnektivität und gültige IP-Quelladressen durch Ausgangsregeln und Netzwerksicherheitsgruppen, um Threat Intelligence-Funktionen von Microsoft zum Schutz Ihrer Webanwendungen zu nutzen.

- [Integrieren von Azure Firewall](../firewall/integrate-lb.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimieren der Komplexität und des Verwaltungsaufwands von Netzwerksicherheitsregeln

**Leitfaden**: Verwenden Sie Diensttags anstelle von spezifischen IP-Adressen, wenn Sie Sicherheitsregeln erstellen. Indem Sie den Diensttagnamen im Quell- oder Zielfeld einer Regel angeben, können Sie den Datenverkehr für den entsprechenden Dienst zulassen oder verweigern. 

Microsoft verwaltet die Adresspräfixe, für die das Diensttag gilt, und aktualisiert das Diensttag automatisch, wenn sich die Adressen ändern. 

Standardmäßig enthält jede Netzwerksicherheitsgruppe das Diensttag „AzureLoadBalancer“, um Datenverkehr im Rahmen von Integritätstests zulassen. 

Alle Diensttags, die zur Verwendung in Regeln für Netzwerksicherheitsgruppen verfügbar sind, finden Sie in der Azure-Dokumentation.

- [Verfügbare Diensttags](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#available-service-tags)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Beibehalten von Standardsicherheitskonfigurationen für Netzwerkgeräte

**Leitfaden**: Definieren und implementieren Sie Standardsicherheitskonfigurationen für Netzwerkressourcen mit Azure Policy.

Zum Vereinfachen umfangreicher Azure-Bereitstellungen verwenden Sie Azure Blueprints, indem Sie wichtige Umgebungsartefakte wie Azure Resource Manager-Vorlagen, Azure RBAC-Kontrollen und Richtlinien in einer einzigen Blaupausendefinition packen. 

Wenden Sie die Blaupause auf neue Abonnements an, und optimieren Sie die Steuerung und Verwaltung durch Versionsverwaltung.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Azure Policy-Beispiele für Netzwerke](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#network)

- [Erstellen einer Azure-Blaupause](../governance/blueprints/create-blueprint-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="110-document-traffic-configuration-rules"></a>1.10: Dokumentieren von Datenverkehrskonfigurationsregeln

**Leitfaden**: Verwenden Sie Ressourcentags für Netzwerksicherheitsgruppen und andere Ressourcen im Zusammenhang mit Netzwerksicherheit und Datenverkehrsfluss. 

Für einzelne Netzwerksicherheitsgruppen-Regeln verwenden Sie das Feld „Beschreibung“, um die Regeln zu dokumentieren, die Datenverkehr aus/zu einem Netzwerk zulassen.

Implementieren Sie beliebige der integrierten taggingbezogenen Azure Policy-Definitionen (beispielsweise „Tag und zugehörigen Wert erzwingen“), um sicherzustellen, dass alle Ressourcen mit Tags erstellt und Sie auf nicht markierte Ressourcen aufmerksam gemacht werden.

Verwenden Sie Azure PowerShell oder die Azure CLI, um nach Ressourcen basierend auf ihren Tags zu suchen oder Aktionen für diese Ressourcen auszuführen.

- [Erstellen und Verwenden von Tags](../azure-resource-manager/management/tag-resources.md)

- [Erstellen eines virtuellen Azure-Netzwerks](../virtual-network/quick-create-portal.md)

- [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen-Regeln](../virtual-network/tutorial-filter-network-traffic.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Verwenden automatisierter Tools zum Überwachen von Netzwerkressourcenkonfigurationen und Erkennen von Änderungen

**Leitfaden**: Verwenden Sie das Azure-Aktivitätsprotokoll zum Überwachen von Ressourcenkonfigurationen und Erkennen von Änderungen an Ihren Azure-Ressourcen. 

Erstellen Sie in Azure Monitor Warnungen, mit denen Sie benachrichtigt werden, wenn sich wichtige Ressourcen ändern.

- [Anzeigen und Abrufen von Azure-Aktivitätsprotokollereignissen](https://docs.microsoft.com/azure/azure-monitor/essentials/activity-log#view-the-activity-log)

- [Erstellen von Warnungen in Azure Monitor](../azure-monitor/alerts/alerts-activity-log.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="logging-and-monitoring"></a>Protokollierung und Überwachung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Protokollierung und Überwachung](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Konfigurieren der zentralen Sicherheitsprotokollverwaltung

**Leitfaden**: Überprüfen Sie Änderungen an Ihren Ausgangsregeln und Netzwerksicherheitsgruppen für Ihre Load Balancer-Instanzen, indem Sie das Aktivitätsprotokoll in Ihren Abonnements anzeigen. 

Zeigen Sie die internen Hostprotokolle an, um sicherzustellen, dass Ihre Back-End-Ressourcen sicher sind.

Exportieren Sie diese Protokolle in Log Analytics oder eine andere Speicherplattform. Verwenden Sie in Azure Monitor Log Analytics-Arbeitsbereiche, um Analysen abzufragen und auszuführen, und verwenden Sie Azure Storage-Konten für langfristige Speicherung und Archivierung.

Sie sollten die Daten für Azure Sentinel oder eine SIEM-Lösung eines Drittanbieters auf Grundlage der Geschäftsanforderungen Ihrer Organisation aktivieren und ein Onboarding durchführen.

- [Durchführen des Onboardings für Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Sammeln von Plattformprotokollen und -metriken mit Azure Monitor](../azure-monitor/essentials/diagnostic-settings.md)

- [Sammeln von internen Azure Virtual Machine-Hostprotokollen mit Azure Monitor](../azure-monitor/vm/quick-collect-azurevm.md)

- [Erste Schritte mit Azure Monitor und der Integration einer SIEM-Drittanbieterlösung](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

- [Plattformaktivitätsprotokolle](../azure-monitor/essentials/activity-log.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Aktivieren der Überwachungsprotokollierung für Azure-Ressourcen

**Leitfaden**: Überprüfen der Protokollierung der Steuerungs- und Verwaltungsebene und Überwachungen der Informationen, die mit Aktivitätsprotokollen für Load Balancer Basic erfasst wurden. Diese Erfassungseinstellungen sind standardmäßig aktiviert. 

Verwenden Sie Aktivitätsprotokolle zum Überwachen von Aktionen für Ressourcen, um alle Aktivitäten und deren Status anzuzeigen. 

Ermitteln Sie mit Aktivitätsprotokollen, welche Vorgänge für die Ressourcen in Ihrem Abonnement ausgeführt wurden: 

- Den Benutzer oder das System, von dem der Vorgang initiiert wurde
- Wann der Vorgang abgeschlossen wurde
- Den Status des Vorgangs

- Die Werte anderer Eigenschaften, mit denen Sie den Vorgang eventuell untersuchen können

Rufen Sie Informationen aus den Aktivitätsprotokollen über Azure PowerShell, die Azure-Befehlszeilenschnittstelle, die Azure REST-API oder über das Azure-Portal ab. 

Implementieren Sie mit Azure Monitor mehrdimensionale Diagnosefunktionen für die Konfigurationsmöglichkeiten von Load Balancer Standard.  Hierzu gehören verfügbare Metriken für Sicherheit, z. B. Informationen zu SNAT-Verbindungen (Source Network Address Translation, Quellnetzwerk-Adressenübersetzung) und Ports. Weitere Metriken zu SYN-Paketen (Synchronisieren) und Paketleistungsindikatoren sind ebenfalls verfügbar. 

Rufen Sie mehrdimensionale Metriken programmgesteuert über APIs ab, und schreiben Sie diese über die Option „Alle Metriken“ in ein Speicherkonto.

Verwenden Sie Azure Audit Logs Content Pack mit Microsoft Power BI, um Ihre Daten mit vorkonfigurierten Dashboards zu analysieren oder Ansichten an Ihre Anforderungen anzupassen.

Streamen Sie Protokolle an einen Event Hub oder einen Log Analytics-Arbeitsbereich. Sie können auch aus Azure Blob Storage extrahiert und in anderen Tools wie Excel und Power BI angezeigt werden. 

Sie sollten die Daten für Azure Sentinel oder eine SIEM-Lösung eines Drittanbieters auf Grundlage der Geschäftsanforderungen aktivieren und ein Onboarding durchführen.

- [Lesen Sie diesen Artikel mit schrittweisen Anleitungen für die einzelnen Methoden unter „Überwachen von Vorgängen mit Resource Manager“.](../azure-resource-manager/management/view-activity-logs.md)

- [Azure Monitor-Protokolle für öffentlichen Load Balancer vom Typ „Basic“](load-balancer-monitor-log.md)

- [Anzeigen von Aktivitätsprotokollen zur Überwachung von Aktionen in Ressourcen](../azure-resource-manager/management/view-activity-logs.md)

- [Programmgesteuertes Abrufen von mehrdimensionalen Metriken über APIs](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics#retrieve-multi-dimensional-metrics-programmatically-via-apis)

- [Erste Schritte mit Azure Monitor und der Integration einer SIEM-Drittanbieterlösung](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="25-configure-security-log-storage-retention"></a>2.5: Konfigurieren der Sicherheitsprotokoll-Aufbewahrungsdauer im Speicher

**Leitfaden**: Das Aktivitätsprotokoll ist standardmäßig aktiviert und wird für 90 Tage im Ereignisprotokollspeicher von Azure beibehalten. Legen Sie in Azure Monitor den Aufbewahrungszeitraum Ihres Log Analytics-Arbeitsbereichs gemäß den Compliancevorschriften Ihrer Organisation fest. Verwenden Sie Azure Storage-Konten für die langfristige Speicherung und Archivierung.

- [Artikel „Anzeigen von Aktivitätsprotokollen zur Überwachung von Aktionen in Ressourcen“](../azure-resource-manager/management/view-activity-logs.md)

- [Ändern des Datenaufbewahrungszeitraums in Protokollanalyse](https://docs.microsoft.com/azure/azure-monitor/logs/manage-cost-storage#change-the-data-retention-period)

- [Konfigurieren der Aufbewahrungsrichtlinie für Azure Storage-Kontoprotokolle](https://docs.microsoft.com/azure/storage/common/manage-storage-analytics-logs#configure-logging)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="26-monitor-and-review-logs"></a>2.6: Überwachen und Überprüfen von Protokollen

**Leitfaden**: Überwachen, Verwalten und Problembehandlung von Load Balancer Standard-Ressourcen unter Verwendung der Seite „Load Balancer“ im Azure-Portal und der Seite „Resource Health“ in Azure Monitor. Dazu gehören verfügbare Metriken für Sicherheit, z. B. Informationen zu SNAT-Verbindungen (Source Network Address Translation, Quellnetzwerk-Adressenübersetzung) und Ports. Außerdem sind Metriken zu SYN-Paketen (Synchronisieren) und Paketleistungsindikatoren verfügbar. 

Verwenden Sie Azure Monitor, um den Status des Endpunktintegritätstests mit mehrdimensionalen Metriken für externe und interne Load Balancer-Instanzen und Load Balancer Standard zu überprüfen. Erfassen Sie diese Metriken programmgesteuert über APIs, und schreiben Sie diese über die Option „Alle Metriken“ in ein Speicherkonto.

Azure Monitor-Protokolle stehen für interne Load Balancer Basic-Instanzen nicht zur Verfügung. 

Verwenden Sie Azure Monitor, um den zusammengefassten Integritätsteststatus pro Back-End-Pool für den externen Load Balancer Standard anzuzeigen, z. B. die Anzahl der Instanzen in Ihrem Back-End-Pool, die aufgrund von Integritätstestfehlern keine Anforderungen vom Load Balancer empfangen. 

Implementieren Sie Protokollierung mit Power BI oder Azure Operational Insights, um Statistiken zum Integritätsstatus der Load Balancer-Instanz bereitzustellen. 

Verwenden Sie die Aktivitätsprotokolle, um Warnungen anzuzeigen und Aktionen für Ressourcen und deren Status in Ihren Azure-Abonnements zu überwachen. Aktivitätsprotokolle sind standardmäßig aktiviert und können im Azure-Portal angezeigt werden. 

Verwenden Sie Microsoft Power BI mit dem Azure Audit Logs Content Pack, und analysieren Sie Ihre Daten mit vorkonfigurierten Dashboards. Passen Sie Ansichten entsprechend den Anforderungen Ihres Unternehmens an. 

Streamen Sie Protokolle an einen Event Hub oder einen Log Analytics-Arbeitsbereich. Sie können auch aus Azure Blob Storage extrahiert und in anderen Tools wie Excel und Power BI angezeigt werden. Sie können Daten in Azure Sentinel oder in einer SIEM-Drittanbieterlösung aktivieren und ein Onboarding durchführen.

- [Lastenausgleichs-Integritätstests](load-balancer-custom-probe-overview.md)

- [Azure Monitor-REST-API](/rest/api/monitor)

- [Abrufen von Metriken über die Rest-API](/rest/api/monitor/metrics/list)

- [Load Balancer Standard-Diagnose mit Metriken, Warnungen und Ressourcenintegrität](load-balancer-standard-diagnostics.md)

- [Azure Monitor-Protokolle für öffentlichen Load Balancer vom Typ „Basic“](load-balancer-monitor-log.md)

- [Anzeigen Ihrer Load Balancer-Metriken im Azure-Portal](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics#view-your-load-balancer-metrics-in-the-azure-portal)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Aktivieren von Warnungen bei anomalen Aktivitäten

**Leitfaden**: Verwenden Sie Security Center mit einem Log Analytics-Arbeitsbereich für die Überwachung und Warnung bei anomalen Aktivitäten, sie sich auf Load Balancer beziehen und die in Sicherheitsprotokollen und Ereignissen gefunden werden.

Aktivieren Sie Daten in Azure Sentinel oder in einer SIEM-Drittanbieterlösung, und führen Sie ein Onboarding durch.

- [Durchführen des Onboardings für Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Verwalten von Warnungen in Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md)

- [Warnungen bei Log Analytics-Protokolldaten](../azure-monitor/alerts/tutorial-response.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="identity-and-access-control"></a>Identität und Zugriffssteuerung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Identität und Zugriffssteuerung](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Verwalten eines Bestands von Administratorkonten

**Leitfaden**: Mit der rollenbasierten Zugriffssteuerung in Azure (Azure RBAC) kann der Zugriff auf Azure-Ressourcen wie Ihre Load Balancer-Instanz über Rollenzuweisungen verwaltet werden. Weisen Sie diese Rollen Benutzern, Gruppen, Dienstprinzipalen und verwalteten Identitäten zu.

Verwenden Sie Tools wie die Azure CLI, Azure PowerShell oder das Azure-Portal, um vordefinierte integrierte Rollen für bestimmte Ressourcen zu inventarisieren oder abzufragen.

- [Abrufen einer Verzeichnisrolle in Azure Active Directory (Azure AD) mit PowerShell](/powershell/module/azuread/get-azureaddirectoryrole)

- [Abrufen von Mitgliedern einer Verzeichnisrolle in Azure AD mit PowerShell](/powershell/module/azuread/get-azureaddirectoryrolemember)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Verwenden der mehrstufigen Authentifizierung für den gesamten Azure Active Directory-basierten Zugriff

**Hinweis:** Aktivieren Sie die Azure Active Directory Multifactor Authentication (Azure AD), und befolgen Sie die Empfehlungen für die Identitäts- und Zugriffsverwaltung in Security Center.

- [Planen einer Bereitstellung von Azure AD Multi-Factor Authentication](../active-directory/authentication/howto-mfa-getstarted.md) 

- [Überwachen von Identität und Zugriff in Azure Security Center](../security-center/security-center-identity-access.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Verwenden dedizierter Computer (Arbeitsstationen mit privilegiertem Zugriff) für alle administrativen Aufgaben

**Hinweis:** Verwenden Sie Privileged Access Workstations (PAW) mit Multifactor Authentication, die für die Verwaltung von und den Zugriff auf Azure-Netzwerkressourcen konfiguriert sind. 

- [Informationen zu Arbeitsstationen mit privilegiertem Zugriff](/security/compass/privileged-access-devices)

- [Planen einer Bereitstellung von Azure AD Multi-Factor Authentication](../active-directory/authentication/howto-mfa-getstarted.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="38-manage-azure-resources-only-from-approved-locations"></a>3.8: Verwalten von Azure-Ressourcen nur über genehmigte Standorte

**Leitfaden**: Verwenden Sie benannte Standorte mit bedingtem Zugriff, um den Zugriff nur über bestimmte logische Gruppierungen von IP-Adressbereichen oder Ländern/Regionen zuzulassen.

- [Konfigurieren benannter Standorte in Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="39-use-azure-active-directory"></a>3.9: Verwenden von Azure Active Directory

**Leitfaden**: Verwenden Sie Azure Active Directory (Azure AD) als zentrales Authentifizierungs- und Autorisierungssystem für Ihre Dienste. Azure AD schützt Daten mithilfe von starker Verschlüsselung für Daten im Ruhezustand und in der Übertragung. Außerdem werden Salts, Hashes und sichere Speicher für Benutzeranmeldeinformationen verwendet.  

- [Erstellen und Konfigurieren einer Azure AD-Instanz](../active-directory-domain-services/tutorial-create-instance.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Regelmäßiges Überprüfen und Abstimmen des Benutzerzugriffs

**Leitfaden**: Verwenden Sie Azure Active Directory (Azure AD), um Protokolle zum Ermitteln von veralteten Konten bereitzustellen. 

Zugriffsüberprüfungen für Azure-Identitäten können durchgeführt werden, um Gruppenmitgliedschaften, den Zugriff auf Unternehmensanwendungen sowie Rollenzuweisungen effizient zu verwalten. Der Benutzerzugriff sollte regelmäßig überprüft werden, um sicherzustellen, dass nur aktive Benutzer weiterhin Zugriff haben.

- [Grundlegendes zur Azure AD-Berichterstellung](/azure/active-directory/reports-monitoring/)

- [Verwenden von Zugriffsüberprüfungen für Azure-Identitäten](../active-directory/governance/access-reviews-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Überwachen von Zugriffsversuchen auf deaktivierte Anmeldeinformationen

**Leitfaden**: Integrieren Sie ausgehend von Ihrem Zugriff Protokollquellen für Azure AD-Anmeldeaktivitäten (Azure Active Directory), Überwachung und Risikoereignisse in beliebige SIEM- oder Überwachungstools.

Optimieren Sie diesen Prozess, indem Sie Diagnoseeinstellungen für Azure AD-Benutzerkonten erstellen und die Überwachungs- und Anmeldeprotokolle an einen Log Analytics-Arbeitsbereich senden. Im Log Analytics-Arbeitsbereich können alle gewünschten Benachrichtigungen konfiguriert werden.

- [Integrieren von Azure-Aktivitätsprotokollen in Azure Monitor](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Warnung bei abweichendem Verhalten bei der Kontoanmeldung

**Hinweis**: Mit Azure AD-Features zum Risiko- und Identitätsschutz können Sie automatische Antworten auf erkannte verdächtige Aktionen im Zusammenhang mit Benutzeridentitäten konfigurieren. Erfassen Sie Daten zur weiteren Untersuchung in Azure Sentinel.

- [Anzeigen riskanter Azure AD-Anmeldungen](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Konfigurieren und Aktivieren von Risikorichtlinien für den Identitätsschutz](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Durchführen des Onboardings für Azure Sentinel](../sentinel/quickstart-onboard.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="data-protection"></a>Datenschutz

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Schutz von Daten](../security/benchmarks/security-control-data-protection.md).*

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Verschlüsseln aller vertraulichen Informationen während der Übertragung

**Hinweis:** Stellen Sie sicher, dass alle Clients, die sich mit Ihren Azure-Ressourcen verbinden, TLS 1.2 oder höher aushandeln können.

Befolgen Sie ggf. die Empfehlungen von Azure Security Center zur Verschlüsselung ruhender Daten und zur Verschlüsselung während der Übertragung.

- [Grundlegendes zur Verschlüsselung während der Übertragung mit Azure](https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="46-use-azure-rbac-to-manage-access-to-resources"></a>4.6: Verwenden von Azure RBAC zum Verwalten des Zugriffs auf Ressourcen

**Leitfaden**: Verwenden von Azure RBAC zum Steuern des Zugriffs auf Ihre Load Balancer-Ressourcen.

- [Konfigurieren von Azure RBAC](../role-based-access-control/role-assignments-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Verwenden der hostbasierten Verhinderung von Datenverlusten zum Erzwingen der Zugriffssteuerung

**Leitfaden**: Load Balancer ist ein Pass-Through-Dienst, der keine Kundendaten speichert. Er ist ein Teil der zugrunde liegenden Plattform, die von Microsoft verwaltet wird. 

Microsoft behandelt alle Kundeninhalte vertraulich und unternimmt große Anstrengungen, um Kundendaten vor Verlust und Offenlegung zu schützen. 

Um die Sicherheit von Kundendaten innerhalb von Azure zu gewährleisten, hat Microsoft eine Reihe von robusten Datenschutzkontrollen und -funktionen implementiert und kümmert sich um deren Verwaltung. 

- [Grundlegendes zum Schutz von Kundendaten in Azure](../security/fundamentals/protection-customer-data.md)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Protokollieren und Warnen bei Änderungen an wichtigen Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Monitor mit dem Azure-Aktivitätsprotokoll, um Warnungen zu erstellen, die bei Änderungen an wichtigen Azure-Ressourcen ausgegeben werden, z. B. an Load Balancern, die für wichtige Produktionsworkloads verwendet werden.

- [Erstellen von Warnungen für Azure-Aktivitätsprotokollereignisse](../azure-monitor/alerts/alerts-activity-log.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="inventory-and-asset-management"></a>Bestands- und Ressourcenverwaltung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Bestands- und Ressourcenverwaltung](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Verwenden der automatisierten Asset Discovery-Lösung

**Leitfaden**: Verwenden Sie Azure Resource Graph, um alle Ressourcen (z. B. Computeressourcen, Speicher, Netzwerke, Ports und Protokolle usw.) in Ihren Abonnements abzufragen und zu ermitteln. Azure Resource Manager wird empfohlen, um aktuelle Ressourcen zu erstellen und zu verwenden.

Sorgen Sie für entsprechende Berechtigungen (Leseberechtigungen) auf Ihrem Mandanten, und zählen Sie alle Azure-Abonnements sowie Ressourcen in Ihren Abonnements auf.

- [Schnellstart: Ausführen Ihrer ersten Resource Graph-Abfrage mithilfe des Azure Resource Graph-Explorers](../governance/resource-graph/first-query-portal.md)

- [Anzeigen Ihrer Azure-Abonnements](/powershell/module/az.accounts/get-azsubscription)

- [Grundlegendes zu Azure RBAC](../role-based-access-control/overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="62-maintain-asset-metadata"></a>6.2: Verwalten von Ressourcenmetadaten

**Leitfaden**: Wenden Sie Tags auf Ihre Azure-Ressourcen mit Metadaten an, die Ressourcen einer Taxonomie entsprechend logisch zu organisieren.

- [Erstellen und Verwenden von Tags](/azure/azure-resource-manager/resource-group-using-tags)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Löschen nicht autorisierter Azure-Ressourcen

**Leitfaden**: Verwenden Sie nach Bedarf Tagging, Verwaltungsgruppen und separate Abonnements zum Organisieren und Nachverfolgen von Ressourcen. 

Stimmen Sie den Bestand regelmäßig ab, und stellen Sie sicher, dass nicht autorisierte Ressourcen rechtzeitig aus Ihrem Abonnement gelöscht werden.

- [Erstellen zusätzlicher Azure-Abonnements](../cost-management-billing/manage/create-subscription.md)

- [Erstellen von Verwaltungsgruppen](../governance/management-groups/create-management-group-portal.md)

- [Erstellen und Verwenden von Tags](../azure-resource-manager/management/tag-resources.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6.4: Definieren und Verwalten eines Bestands an genehmigten Azure-Ressourcen

**Leitfaden**: Erstellen Sie eine Liste der genehmigten Azure-Ressourcen gemäß den Anforderungen Ihrer Organisation, die Sie als Zulassungslistenmechanismus nutzen können. Dadurch kann Ihre Organisation ein Onboarding neu verfügbarer Azure-Dienste durchführen, nachdem sie formal überprüft und durch die üblichen Sicherheitsbewertungsprozesse Ihres Unternehmens genehmigt wurden.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Überwachung auf nicht genehmigte Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy, um Einschränkungen für den Typ der Ressourcen festzulegen, die in Ihren Abonnements erstellt werden können.

Verwenden Sie Azure Resource Graph, um Ressourcen in Ihren eigenen Abonnements abzufragen und zu ermitteln. 

Stellen Sie sicher, dass alle in der Umgebung vorhandenen Azure-Ressourcen genehmigt sind.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Schnellstart: Ausführen Ihrer ersten Resource Graph-Abfrage mithilfe des Azure Resource Graph-Explorers](../governance/resource-graph/first-query-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="69-use-only-approved-azure-services"></a>6.9: Ausschließliche Verwendung genehmigter Azure-Dienste

**Leitfaden**: Verwenden Sie Azure Policy, um Einschränkungen für den Typ der Ressourcen anzugeben, die in Kundenabonnements erstellt werden können. Nutzen Sie dazu die folgenden integrierten Richtliniendefinitionen:
- Not allowed resource types (Unzulässige Ressourcentypen)
- Zulässige Ressourcentypen

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Ablehnen eines bestimmten Ressourcentyps mit Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#general)

- [Integrierte Azure Policy-Beispiele für virtuelle Netzwerke](/azure/virtual-network/policy-samples)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Einschränken der Möglichkeiten von Benutzern zur Interaktion mit Azure Resource Manager

**Hinweis**: Verwenden Sie bedingten Azure AD-Zugriff (Azure Active Directory), um die Möglichkeiten von Benutzern zur Interaktion mit Azure Resource Manager einzuschränken, indem Sie „Zugriff blockieren“ für die Microsoft Azure Management-App konfigurieren.

- [Konfigurieren des bedingten Zugriffs, um den Zugriff auf Azure Resource Manager zu blockieren](../role-based-access-control/conditional-access-azure-management.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Physische oder logische Trennung von Anwendungen mit hohem Risiko

**Leitfaden**: Software, die für den Geschäftsbetrieb erforderlich ist, aber ein höheres Risiko für das Unternehmen darstellen kann, sollte innerhalb des eigenen virtuellen Computers und/oder virtuellen Netzwerks isoliert und entweder mit einer Azure Firewall oder einer Netzwerksicherheitsgruppe ausreichend gesichert werden.

- [Erstellen eines virtuellen Netzwerks](../virtual-network/quick-create-portal.md)

- [Erstellen einer Netzwerksicherheitsgruppe mit einer Sicherheitskonfiguration](../virtual-network/tutorial-filter-network-traffic.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="secure-configuration"></a>Sichere Konfiguration

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Sichere Konfiguration](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Einrichten sicherer Konfigurationen für alle Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy-Aliase, um benutzerdefinierte Richtlinien zur Überprüfung oder Durchsetzung der Konfiguration Ihrer Azure-Ressourcen zu erstellen. Verwenden Sie integrierte Azure Policy-Definitionen.

Mit Azure Resource Manager ist es möglich, die Vorlage in JSON-Code (JavaScript Object Notation) zu exportieren. Dieser sollte überprüft werden, um sicherzustellen, dass die Konfigurationen die Sicherheitsanforderungen für Ihre Organisation erfüllen.

Exportieren Sie Azure Resource Manager-Vorlagen in JSON-Formate(JavaScript Object Notation), und überprüfen Sie diese regelmäßig, um sicherzustellen, dass die Konfigurationen den Sicherheitsanforderungen Ihrer Organisation entsprechen.

Implementieren Sie Empfehlungen von Security Center als sichere Konfigurationsbaseline für Ihre Azure-Ressourcen.

- [Anzeigen verfügbarer Azure Policy-Aliase](/powershell/module/az.resources/get-azpolicyalias)

- [Tutorial: Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung](../governance/policy/tutorials/create-and-manage.md)

- [Exportieren von einzelnen oder mehreren Ressourcen in eine Vorlage im Azure-Portal](../azure-resource-manager/templates/export-template-portal.md)

- [Sicherheitsempfehlungen: Referenzhandbuch](../security-center/recommendations-reference.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Verwalten von sicheren Konfigurationen für Azure-Ressourcen

**Leitfaden**: Verwenden Sie die Azure Policy-Auswirkungen [Deny] und [DeployIfNotExists], um sichere Einstellungen für alle Ihre Azure-Ressourcen zu erzwingen.  Sie können auch Azure Resource Manager-Vorlagen verwenden, um die Sicherheitskonfiguration der für Ihre Organisation erforderlichen Azure-Ressourcen zu verwalten. 

- [Grundlegendes zu Azure Policy-Auswirkungen](../governance/policy/concepts/effects.md)

- [Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung](../governance/policy/tutorials/create-and-manage.md)

- [Übersicht über Azure Resource Manager-Vorlagen](../azure-resource-manager/templates/overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Sicheres Speichern der Konfiguration von Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure DevOps, um Ihren Code sicher zu speichern und zu verwalten. Dazu zählen z. B. benutzerdefinierte Azure Policy-Richtlinien, Azure Resource Manager-Vorlagen und Desired State Configuration-Skripts.

Erteilen oder verweigern Sie bestimmten Benutzern, integrierten Sicherheitsgruppen oder in Azure Active Directory (Azure AD) definierten Gruppen (bei Integration mit Azure DevOps oder TFS) Berechtigungen.

- [Speichern von Code in Azure DevOps](/azure/devops/repos/git/gitworkflow)

- [Informationen über Berechtigungen und Gruppen in Azure DevOps](/azure/devops/organizations/security/about-permissions)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Bereitstellen von Konfigurationsverwaltungstools für Azure-Ressourcen

**Leitfaden**: Definieren und implementieren Sie Standardsicherheitskonfigurationen für Azure-Ressourcen mit Azure Policy.  Verwenden Sie Azure Policy-Aliase, um benutzerdefinierte Richtlinien zur Überprüfung oder Durchsetzung der Netzwerkkonfiguration Ihrer Azure-Ressourcen zu erstellen. Implementieren Sie integrierte Richtliniendefinitionen, die sich auf Ihre spezifischen Load Balancer-Ressourcen beziehen.  Verwenden Sie außerdem Azure Automation, um Konfigurationsänderungen für Ihre Load Balancer-Ressourcen bereitzustellen.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Verwenden von Aliasen](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#aliases)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementieren der automatisierten Konfigurationsüberwachung für Azure-Ressourcen

**Leitfaden**: Verwenden Sie Security Center, um Baselinescans für Ihre Azure-Ressourcen auszuführen, und Azure Policy zum Warnen und zum Überwachen von Ressourcenkonfigurationen.

- [Umsetzen von Empfehlungen in Azure Security Center](../security-center/security-center-remediate-recommendations.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="incident-response"></a>Reaktion auf Vorfälle

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Reaktion auf Vorfälle](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Erstellen eines Leitfadens für die Reaktion auf Vorfälle

**Anleitung:** Erarbeiten Sie einen Leitfaden für die Reaktion auf Vorfälle für Ihre Organisation. Stellen Sie sicher, dass es schriftliche Pläne für die Reaktion auf Vorfälle gibt, in denen alle Rollen der Mitarbeiter sowie die Phasen der Bearbeitung und Verwaltung von Vorfällen von der Ermittlung bis zur abschließenden Überprüfung definiert sind. 

- [Leitfaden zu Planung und Betrieb](../security-center/security-center-planning-and-operations-guide.md) 

- [Anleitung zum Entwickeln eines Prozesses für die Reaktion auf Sicherheitsvorfälle](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/) 

- [Struktur eines Vorfalls laut Microsoft Security Response Center](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/) 

- [Kunden können außerdem den Computer Security Incident Handling Guide des US-amerikanischen National Institute of Standards and Technology (NIST) nutzen, um einen Plan zur Reaktion auf Incidents auszuarbeiten.](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Erstellen eines Verfahrens zur Bewertung und Priorisierung von Vorfällen

**Anleitung:** Security Center weist jeder Warnung einen Schweregrad zu, damit Sie priorisieren können, welche Warnungen zuerst untersucht werden sollen. 

Der Schweregrad basiert darauf, wie zuversichtlich Security Center in Bezug auf den Befund oder die Analyse ist, die zum Auslösen der Warnung verwendet wird, sowie auf dem Zuverlässigkeitsgrad, dass hinter der Aktivität, die zu der Warnung führte, eine böswillige Absicht stand.

Markieren Sie Abonnements mithilfe von Tags, und erstellen Sie ein Benennungssystem, um Azure-Ressourcen eindeutig zu identifizieren und zu kategorisieren, insbesondere solche, die vertrauliche Daten verarbeiten.  

Die Priorisierung der Behebung von Warnungen basierend auf der Wichtigkeit der Azure-Ressourcen und der Umgebung, in der der Vorfall aufgetreten ist, liegt in Ihrer Verantwortung.

- [Sicherheitswarnungen in Azure Security Center](../security-center/security-center-alerts-overview.md)

- [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../azure-resource-manager/management/tag-resources.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="103-test-security-response-procedures"></a>10.3: Verfahren zum Testen der Reaktion auf Sicherheitsvorfälle

**Leitfaden**: Führen Sie in regelmäßigen Abständen Übungen durch, um die Reaktionsfähigkeit Ihrer Systeme zu testen und so Ihre Azure-Ressourcen zu schützen. Ermitteln Sie Schwachpunkte und Lücken, und passen Sie Ihren Reaktionsplan dann entsprechend an. 

- [NIST-Veröffentlichung: Leitfaden zum Testen, Trainieren und Ausführen von Programmen für IT-Pläne und -Funktionen](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Angeben von Kontaktdaten für Sicherheitsvorfälle und Konfigurieren von Warnungsbenachrichtigungen für Sicherheitsvorfälle

**Leitfaden**: Microsoft kontaktiert Sie unter den für Sicherheitsvorfälle angegebenen Kontaktdaten, wenn das Microsoft Security Response Center (MSRC) feststellt, dass Personen unrechtmäßig oder unbefugt auf Ihre Daten zugegriffen haben. Überprüfen Sie die Vorfälle anschließend, um sicherzustellen, dass die Probleme behoben wurden. 

- [Festlegen der Kontaktinformationen in Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Integrieren von Sicherheitswarnungen in das System zur Reaktion auf Vorfälle

**Leitfaden**: Exportieren Sie Ihre Warnungen und Empfehlungen von Security Center mithilfe des fortlaufenden Exportfeatures, um Risiken für Azure-Ressourcen zu identifizieren. 

Über die Funktion „Fortlaufender Export“ in Security Center können Sie Warnungen und Empfehlungen entweder manuell oder kontinuierlich exportieren. 

Verwenden Sie den Azure Security Center-Datenconnector, um die Warnungen an Azure Sentinel zu streamen.

- [Konfigurieren des fortlaufenden Exports](../security-center/continuous-export.md)

- [Streamen von Warnungen in Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatisieren der Reaktion auf Sicherheitswarnungen

**Leitfaden**: Verwenden Sie die Funktion „Workflowautomatisierung“ in Security Center, um automatisch Reaktionen auf Sicherheitswarnungen und -empfehlungen auszulösen und Ihre Azure-Ressourcen noch besser zu schützen.

- [Konfigurieren von Workflowautomatisierung in Security Center](../security-center/workflow-automation.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="penetration-tests-and-red-team-exercises"></a>Penetrationstests und Red Team-Übungen

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Penetrationstests und Red Team-Übungen](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1: Durchführen regelmäßiger Penetrationstests Ihrer Azure-Ressourcen und Sicherstellen der Behebung aller kritischen Sicherheitsergebnissen

**Leitfaden**: Befolgen Sie in dem Fall die Einsatzregeln für Penetrationstests von Microsoft Cloud, um sicherzustellen, dass die Penetrationstests nicht gegen Microsoft-Richtlinien verstoßen. Nutzen Sie die Microsoft-Strategie und Durchführung von Red Team- und Livewebsite-Penetrationstests für von Microsoft verwaltete Cloudinfrastruktur, Dienste und Anwendungen. 

- [Penetrationstests – Rules of Engagement](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Cloud Red Teaming](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die [Übersicht über Version 2 des Azure-Sicherheitsvergleichstests](/azure/security/benchmarks/overview) an.
- Erfahren Sie mehr über [Azure-Sicherheitsbaselines](/azure/security/benchmarks/security-baselines-overview).
