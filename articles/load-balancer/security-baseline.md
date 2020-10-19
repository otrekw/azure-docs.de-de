---
title: Azure-Sicherheitsbaseline für Azure Load Balancer
description: Die Azure Load Balancer-Sicherheitsbaseline enthält Anweisungen zur Vorgehensweise und Ressourcen für die Implementierung der Sicherheitsempfehlungen im Vergleichstest für die Azure-Sicherheit.
author: msmbaldwin
ms.service: load-balancer
ms.topic: conceptual
ms.date: 09/28/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 6d03f0057e657933f4b0f38c48f2a2cdbe1ba2a4
ms.sourcegitcommit: 541bb46e38ce21829a056da880c1619954678586
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2020
ms.locfileid: "91940326"
---
# <a name="azure-security-baseline-for-azure-load-balancer"></a>Azure-Sicherheitsbaseline für Azure Load Balancer

Die Azure-Sicherheitsbaseline für Microsoft Azure Load Balancer enthält Empfehlungen, mit deren Hilfe Sie den Sicherheitsstatus Ihrer Bereitstellung verbessern können. Die Baseline für diesen Dienst wird von [Azure Security Benchmark-Version 1.0](https://docs.microsoft.com/azure/security/benchmarks/overview) abgeleitet, die Empfehlungen dazu enthält, wie Sie Ihre Cloudlösungen in Azure mithilfe unserer bewährten Methoden schützen können. Weitere Informationen finden Sie unter [Übersicht über Azure-Sicherheitsbaselines](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Netzwerksicherheit

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Netzwerksicherheit](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Schützen von Azure-Ressourcen in virtuellen Netzwerken

**Leitfaden**: Verwenden interner Azure Load Balancer, um Datenverkehr zu Back-End-Ressourcen nur aus bestimmten virtuellen Netzwerken oder virtuellen Peeringnetzwerken ohne Gefährdung durch das Internet zuzulassen. Implementieren Sie einen externen Load Balancer mit SNAT (Source Network Address Translation), um die IP-Adressen von Back-End-Ressourcen zum Schutz vor direkter Gefährdung durch das Internet zu maskieren.

Azure bietet zwei Typen von Load Balancern: Standard und Basic. Verwenden Sie Load Balancer Standard für alle Produktionsworkloads. Implementieren Sie Netzwerksicherheitsgruppen, und gewähren Sie nur den Zugriff auf die vertrauenswürdigen Ports und IP-Adressbereiche Ihrer Anwendung. In Fällen, in denen dem Back-End-Subnetz oder der NIC der virtuellen Back-End-Computer keine Netzwerksicherheitsgruppe zugewiesen ist, ist Datenverkehr für diese Ressourcen vom Load Balancer nicht zulässig. Stellen Sie mit Load Balancer Standard Ausgangsregeln bereit, um ausgehende NAT mit einer Netzwerksicherheitsgruppe zu definieren. Überprüfen Sie diese Ausgangsregeln, um das Verhalten ausgehender Verbindungen zu optimieren. 

Die Verwendung einer Load Balancer Standard-Instanz wird für Ihre Produktionsworkloads empfohlen, und in der Regel wird Load Balancer Basic nur für Tests verwendet, da der Basistyp für Verbindungen aus dem Internet standardmäßig geöffnet ist und keine Netzwerksicherheitsgruppen für den Betrieb benötigt. 

- [Ausgehende Verbindungen in Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#outboundrule)

- [Upgraden einer öffentlichen Azure Load Balancer-Instanz](https://docs.microsoft.com/azure/load-balancer/upgrade-basic-standard)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1.2: Überwachen und Protokollieren der Konfiguration und des Datenverkehrs von virtuellen Netzwerken, Subnetzen und Netzwerkkarten (NICs)

**Leitfaden**: Load Balancer ist ein Pass-Through-Dienst, da er die Regeln der Netzwerksicherheitsgruppen verwendet, die auf Back-End-Ressourcen angewendet werden, sowie auf die konfigurierten Ausgangsregeln, um den Internetzugang zu steuern.

Überprüfen Sie die für Ihre Load Balancer Standard-Instanz konfigurierten Regeln für ausgehenden Datenverkehr auf dem Blatt „Ausgangsregeln“ Ihrer Load Balancer-Instanz und dem Blatt „Lastenausgleichsregeln“, auf dem Sie möglicherweise implizite Regeln für ausgehenden Datenverkehr aktiviert haben.

Überwachen Sie die Anzahl der ausgehenden Verbindungen, um nachzuverfolgen, wie oft ihre Ressourcen auf das Internet zugreifen. 

Verwenden Sie Security Center, und befolgen Sie die Empfehlungen für den Netzwerkschutz, um Ihre Azure-Netzwerkressourcen abzusichern.

Befolgen Sie die Sicherheitsempfehlungen für Ihre Back-End-Ressourcen, und aktivieren Sie Netzwerksicherheitsgruppen-Datenflussprotokolle. Senden Sie Protokolle zur Überwachung an ein Azure Storage-Konto.

Senden Sie die Datenflussprotokolle auch an einen Log Analytics-Arbeitsbereich, und verwenden Sie dann Traffic Analytics, um Einblicke in die Datenverkehrsmuster in Ihrer Azure-Cloud zu ermöglichen. Vorteile von Traffic Analytics beinhalten die Möglichkeit, die Netzwerkaktivität zu visualisieren, Hotspots und Sicherheitsbedrohungen zu erkennen, Datenverkehrsflussmuster zu verstehen und Netzwerkfehlkonfigurationen zu ermitteln.

- [Aktivieren von Datenflussprotokollen für Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

- [Aktivieren und Verwenden von Traffic Analytics](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

- [Grundlegendes zu der von Azure Security Center bereitgestellten Netzwerksicherheit](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)

- [Wie überprüfe ich meine Statistiken für ausgehende Verbindungen?](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics#how-do-i-check-my-outbound-connection-statistics)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="13-protect-critical-web-applications"></a>1.3: Schützen kritischer Webanwendungen

**Leitfaden**: Definieren Sie mit Ihrer Load Balancer-Instanz explizit Internet-Konnektivität und gültige IP-Quelladressen durch Ausgangsregeln und Netzwerksicherheitsgruppen, um Threat Intelligence von Microsoft zum Schutz Ihrer Webanwendungen zu nutzen.

- [Integrieren von Azure Firewall](https://docs.microsoft.com/azure/firewall/integrate-lb)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

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
 

- [Verwalten von Azure DDoS Protection Standard mithilfe des Azure-Portals](https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection)

- [Threat Intelligence-gestütztes Filtern für Azure Firewall](https://docs.microsoft.com/azure/firewall/threat-intel)

- [Bedrohungsschutz in Azure Security Center](https://docs.microsoft.com/azure/security-center/threat-protection)

- [Sichern Ihrer Verwaltungsports mit Just-in-Time-Zugriff (JIT)](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)

- [Adaptive Netzwerkhärtung in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-adaptive-network-hardening)

- [Integrieren von Azure Firewall in Ihre Load Balancer-Instanz](https://docs.microsoft.com/azure/firewall/overview)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="15-record-network-packets"></a>1.5: Aufzeichnen von Netzwerkpaketen

**Leitfaden**: Aktivieren Sie die Network Watcher-Paketerfassung, um anomale Aktivitäten zu untersuchen.

- [Erstellen einer Network Watcher-Instanz](https://docs.microsoft.com/azure/network-watcher/network-watcher-create)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Bereitstellen von netzwerkbasierten Angriffserkennungs-/Eindringschutzsystemen (Intrusion Detection/Intrusion Prevention Systems, IDS/IPS)

**Leitfaden**: Implementieren eines Angebots aus Azure Marketplace, das IDS-/IPS-Funktionen mit Funktionen zur Nutzlastuntersuchung für die Umgebung Ihres Load Balancers unterstützt. 

Wenn Nutzlastuntersuchung keine Anforderung ist, verwenden Sie Azure Firewall Threat Intelligence. Die auf Azure Firewall Threat Intelligence basierende Filterung wird verwendet, um bei bekannten böswilligen IP-Adressen und Domänen zu warnen und/oder Datenverkehr zu und von diesen Adressen oder Domänen zu blockieren. Die IP-Adressen und Domänen stammen aus dem Microsoft Threat Intelligence-Feed.

Stellen Sie die Firewalllösung Ihrer Wahl an allen Netzwerkgrenzen Ihrer Organisation bereit, um schädlichen Datenverkehr zu erkennen und/oder zu blockieren.

- [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

- [Bereitstellen von Azure Firewall](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

- [Konfigurieren von Warnungen mit Azure Firewall](https://docs.microsoft.com/azure/firewall/threat-intel)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="17-manage-traffic-to-web-applications"></a>1.7: Verwalten von Datenverkehr für Webanwendungen

**Leitfaden**: Definieren Sie mit Ihrer Load Balancer-Instanz explizit Internet-Konnektivität und gültige IP-Quelladressen durch Ausgangsregeln und Netzwerksicherheitsgruppen, um Threat Intelligence-Funktionen von Microsoft zum Schutz Ihrer Webanwendungen zu nutzen.

- [Integrieren von Azure Firewall](https://docs.microsoft.com/azure/firewall/integrate-lb)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimieren der Komplexität und des Verwaltungsaufwands von Netzwerksicherheitsregeln

**Leitfaden**: Verwenden Sie Diensttags anstelle von spezifischen IP-Adressen, wenn Sie Sicherheitsregeln erstellen. Indem Sie den Diensttagnamen im Quell- oder Zielfeld einer Regel angeben, können Sie den Datenverkehr für den entsprechenden Dienst zulassen oder verweigern. 

Microsoft verwaltet die Adresspräfixe, für die das Diensttag gilt, und aktualisiert das Diensttag automatisch, wenn sich die Adressen ändern. 

Standardmäßig enthält jede Netzwerksicherheitsgruppe das Diensttag „AzureLoadBalancer“, um Datenverkehr im Rahmen von Integritätstests zulassen. 

Alle Diensttags, die zur Verwendung in Regeln für Netzwerksicherheitsgruppen verfügbar sind, finden Sie in der Azure-Dokumentation.

- [Verfügbare Diensttags](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#available-service-tags)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Beibehalten von Standardsicherheitskonfigurationen für Netzwerkgeräte

**Leitfaden**: Definieren und implementieren Sie Standardsicherheitskonfigurationen für Netzwerkressourcen mit Azure Policy.

Zum Vereinfachen umfangreicher Azure-Bereitstellungen verwenden Sie Azure Blueprints, indem Sie wichtige Umgebungsartefakte wie Azure Resource Manager-Vorlagen, Azure RBAC-Kontrollen und Richtlinien in einer einzigen Blaupausendefinition packen. 

Wenden Sie die Blaupause auf neue Abonnements an, und optimieren Sie die Steuerung und Verwaltung durch Versionsverwaltung.

- [Konfigurieren und Verwalten von Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Azure Policy-Beispiele für Netzwerke](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#network)

- [Erstellen einer Azure-Blaupause](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="110-document-traffic-configuration-rules"></a>1.10: Dokumentieren von Datenverkehrskonfigurationsregeln

**Leitfaden**: Verwenden Sie Ressourcentags für Netzwerksicherheitsgruppen und andere Ressourcen im Zusammenhang mit Netzwerksicherheit und Datenverkehrsfluss. 

Für einzelne Netzwerksicherheitsgruppen-Regeln verwenden Sie das Feld „Beschreibung“, um die Regeln zu dokumentieren, die Datenverkehr aus/zu einem Netzwerk zulassen.

Implementieren Sie beliebige der integrierten taggingbezogenen Azure Policy-Definitionen (beispielsweise „Tag und zugehörigen Wert erzwingen“), um sicherzustellen, dass alle Ressourcen mit Tags erstellt und Sie auf nicht markierte Ressourcen aufmerksam gemacht werden.

Verwenden Sie Azure PowerShell oder die Azure CLI, um nach Ressourcen basierend auf ihren Tags zu suchen oder Aktionen für diese Ressourcen auszuführen.

- [Erstellen und Verwenden von Tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

- [Erstellen eines virtuellen Azure-Netzwerks](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

- [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen-Regeln](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Verwenden automatisierter Tools zum Überwachen von Netzwerkressourcenkonfigurationen und Erkennen von Änderungen

**Leitfaden**: Verwenden Sie das Azure-Aktivitätsprotokoll zum Überwachen von Ressourcenkonfigurationen und Erkennen von Änderungen an Ihren Azure-Ressourcen. 

Erstellen Sie in Azure Monitor Warnungen, mit denen Sie benachrichtigt werden, wenn sich wichtige Ressourcen ändern.

- [Anzeigen und Abrufen von Azure-Aktivitätsprotokollereignissen](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

- [Erstellen von Warnungen in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

## <a name="logging-and-monitoring"></a>Protokollierung und Überwachung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Protokollierung und Überwachung](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="22-configure-central-security-log-management"></a>2.2: Konfigurieren der zentralen Sicherheitsprotokollverwaltung

**Leitfaden**: Überprüfen Sie Änderungen an Ihren Ausgangsregeln und Netzwerksicherheitsgruppen für Ihre Load Balancer-Instanzen, indem Sie das Aktivitätsprotokoll in Ihren Abonnements anzeigen. 

Zeigen Sie die internen Hostprotokolle an, um sicherzustellen, dass Ihre Back-End-Ressourcen sicher sind.

Exportieren Sie diese Protokolle in Log Analytics oder eine andere Speicherplattform. Verwenden Sie in Azure Monitor Log Analytics-Arbeitsbereiche, um Analysen abzufragen und auszuführen, und verwenden Sie Azure Storage-Konten für langfristige Speicherung und Archivierung.

Sie sollten die Daten für Azure Sentinel oder eine SIEM-Lösung eines Drittanbieters auf Grundlage der Geschäftsanforderungen Ihrer Organisation aktivieren und ein Onboarding durchführen.

- [Durchführen des Onboardings für Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

- [Sammeln von Plattformprotokollen und -metriken mit Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings)

- [Sammeln von internen Azure Virtual Machine-Hostprotokollen mit Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-azurevm)

- [Erste Schritte mit Azure Monitor und der Integration einer SIEM-Drittanbieterlösung](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

- [Plattformaktivitätsprotokolle](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

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

- [Lesen Sie diesen Artikel mit schrittweisen Anleitungen für die einzelnen Methoden unter „Überwachen von Vorgängen mit Resource Manager“.](https://docs.microsoft.com/azure/azure-resource-manager/management/view-activity-logs)

- [Azure Monitor-Protokolle für öffentlichen Load Balancer vom Typ „Basic“](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log)

- [Anzeigen von Aktivitätsprotokollen zur Überwachung von Aktionen in Ressourcen](https://docs.microsoft.com/azure/azure-resource-manager/management/view-activity-logs)

- [Programmgesteuertes Abrufen von mehrdimensionalen Metriken über APIs](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics#retrieve-multi-dimensional-metrics-programmatically-via-apis)

- [Erste Schritte mit Azure Monitor und der Integration einer SIEM-Drittanbieterlösung](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="25-configure-security-log-storage-retention"></a>2.5: Konfigurieren der Sicherheitsprotokoll-Aufbewahrungsdauer im Speicher

**Leitfaden**: Das Aktivitätsprotokoll ist standardmäßig aktiviert und wird für 90 Tage im Ereignisprotokollspeicher von Azure beibehalten. Legen Sie in Azure Monitor den Aufbewahrungszeitraum Ihres Log Analytics-Arbeitsbereichs gemäß den Compliancevorschriften Ihrer Organisation fest. Verwenden Sie Azure Storage-Konten für die langfristige Speicherung und Archivierung.

- [Artikel „Anzeigen von Aktivitätsprotokollen zur Überwachung von Aktionen in Ressourcen“](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit)

- [Ändern des Datenaufbewahrungszeitraums in Protokollanalyse](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

- [Konfigurieren der Aufbewahrungsrichtlinie für Azure Storage-Kontoprotokolle](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account#configure-logging)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="26-monitor-and-review-logs"></a>2.6: Überwachen und Überprüfen von Protokollen

**Leitfaden**: Überwachen, Verwalten und Problembehandlung von Load Balancer Standard-Ressourcen unter Verwendung der Seite „Load Balancer“ im Azure-Portal und der Seite „Resource Health“ in Azure Monitor. Dazu gehören verfügbare Metriken für Sicherheit, z. B. Informationen zu SNAT-Verbindungen (Source Network Address Translation, Quellnetzwerk-Adressenübersetzung) und Ports. Außerdem sind Metriken zu SYN-Paketen (Synchronisieren) und Paketleistungsindikatoren verfügbar. 

Verwenden Sie Azure Monitor, um den Status des Endpunktintegritätstests mit mehrdimensionalen Metriken für externe und interne Load Balancer-Instanzen und Load Balancer Standard zu überprüfen. Erfassen Sie diese Metriken programmgesteuert über APIs, und schreiben Sie diese über die Option „Alle Metriken“ in ein Speicherkonto.

Azure Monitor-Protokolle stehen für interne Load Balancer Basic-Instanzen nicht zur Verfügung. 

Verwenden Sie Azure Monitor, um den zusammengefassten Integritätsteststatus pro Back-End-Pool für den externen Load Balancer Standard anzuzeigen, z. B. die Anzahl der Instanzen in Ihrem Back-End-Pool, die aufgrund von Integritätstestfehlern keine Anforderungen vom Load Balancer empfangen. 

Implementieren Sie Protokollierung mit Power BI oder Azure Operational Insights, um Statistiken zum Integritätsstatus der Load Balancer-Instanz bereitzustellen. 

Verwenden Sie die Aktivitätsprotokolle, um Warnungen anzuzeigen und Aktionen für Ressourcen und deren Status in Ihren Azure-Abonnements zu überwachen. Aktivitätsprotokolle sind standardmäßig aktiviert und können im Azure-Portal angezeigt werden. 

Verwenden Sie Microsoft Power BI mit dem Azure Audit Logs Content Pack, und analysieren Sie Ihre Daten mit vorkonfigurierten Dashboards. Passen Sie Ansichten entsprechend den Anforderungen Ihres Unternehmens an. 

Streamen Sie Protokolle an einen Event Hub oder einen Log Analytics-Arbeitsbereich. Sie können auch aus Azure Blob Storage extrahiert und in anderen Tools wie Excel und Power BI angezeigt werden. Sie können Daten in Azure Sentinel oder in einer SIEM-Drittanbieterlösung aktivieren und ein Onboarding durchführen.

- [Lastenausgleichs-Integritätstests](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)

- [Azure Monitor-REST-API](https://docs.microsoft.com/rest/api/monitor)

- [Abrufen von Metriken über die Rest-API](https://docs.microsoft.com/rest/api/monitor/metrics/list)

- [Load Balancer Standard-Diagnose mit Metriken, Warnungen und Ressourcenintegrität](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics)

- [Azure Monitor-Protokolle für öffentlichen Load Balancer vom Typ „Basic“](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log)

- [Anzeigen Ihrer Load Balancer-Metriken im Azure-Portal](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics#view-your-load-balancer-metrics-in-the-azure-portal)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Aktivieren von Warnungen bei anomalen Aktivitäten

**Leitfaden**: Verwenden Sie Security Center mit einem Log Analytics-Arbeitsbereich für die Überwachung und Warnung bei anomalen Aktivitäten, sie sich auf Load Balancer beziehen und die in Sicherheitsprotokollen und Ereignissen gefunden werden.

Aktivieren Sie Daten in Azure Sentinel oder in einer SIEM-Drittanbieterlösung, und führen Sie ein Onboarding durch.

- [Durchführen des Onboardings für Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

- [Verwalten von Warnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)

- [Warnungen bei Log Analytics-Protokolldaten](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="28-centralize-anti-malware-logging"></a>2.8: Zentralisieren der Antischadsoftwareprotokollierung

**Leitfaden**: Gilt nicht für Azure Load Balancer. Diese Empfehlung ist für Computeressourcen vorgesehen.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="29-enable-dns-query-logging"></a>2.9: Aktivieren der DNS-Abfrageprotokollierung

**Leitfaden**: Nicht zutreffend, da Azure Load Balancer ein Kernnetzwerkdienst ist, der keine DNS-Abfragen durchführt.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="210-enable-command-line-audit-logging"></a>2.10: Aktivieren der Befehlszeilen-Überwachungsprotokollierung

**Leitfaden**: Gilt nicht für Azure Load Balancer, da diese Empfehlung für Computeressourcen gilt.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="identity-and-access-control"></a>Identität und Zugriffssteuerung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Identität und Zugriffssteuerung](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Verwalten eines Bestands von Administratorkonten

**Leitfaden**: Mit der rollenbasierten Zugriffssteuerung in Azure (Azure RBAC) kann der Zugriff auf Azure-Ressourcen wie Ihre Load Balancer-Instanz über Rollenzuweisungen verwaltet werden. Weisen Sie diese Rollen Benutzern, Gruppen, Dienstprinzipalen und verwalteten Identitäten zu. 

Verwenden Sie Tools wie die Azure CLI, Azure PowerShell oder das Azure-Portal, um vordefinierte integrierte Rollen für bestimmte Ressourcen zu inventarisieren oder abzufragen.

- [Abrufen einer Verzeichnisrolle in Azure AD mit PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

- [Abrufen von Mitgliedern einer Verzeichnisrolle in Azure AD mit PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

## <a name="data-protection"></a>Schutz von Daten

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Datenschutz](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="46-use-azure-rbac-to-manage-access-to-resources"></a>4.6: Verwenden von Azure RBAC zum Verwalten des Zugriffs auf Ressourcen

**Leitfaden**: Verwenden von Azure RBAC zum Steuern des Zugriffs auf Ihre Load Balancer-Ressourcen.

- [Konfigurieren von RBAC in Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Verwenden der hostbasierten Verhinderung von Datenverlusten zum Erzwingen der Zugriffssteuerung

**Leitfaden**: Load Balancer ist ein Pass-Through-Dienst, der keine Kundendaten speichert. Er ist ein Teil der zugrunde liegenden Plattform, die von Microsoft verwaltet wird. 

Microsoft behandelt alle Kundeninhalte vertraulich und unternimmt große Anstrengungen, um Kundendaten vor Verlust und Offenlegung zu schützen. 

Um die Sicherheit von Kundendaten innerhalb von Azure zu gewährleisten, hat Microsoft eine Reihe von robusten Datenschutzkontrollen und -funktionen implementiert und kümmert sich um deren Verwaltung. 

- [Grundlegendes zum Schutz von Kundendaten in Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Shared

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Protokollieren und Warnen bei Änderungen an wichtigen Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Monitor mit dem Azure-Aktivitätsprotokoll, um Warnungen zu erstellen, die bei Änderungen an wichtigen Azure-Ressourcen ausgegeben werden, z. B. an Load Balancern, die für wichtige Produktionsworkloads verwendet werden.

- [Erstellen von Warnungen für Azure-Aktivitätsprotokollereignisse](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

## <a name="inventory-and-asset-management"></a>Bestands- und Ressourcenverwaltung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Bestands- und Ressourcenverwaltung](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Verwenden der automatisierten Asset Discovery-Lösung

**Leitfaden**: Verwenden Sie Azure Resource Graph, um alle Ressourcen (z. B. Computeressourcen, Speicher, Netzwerke, Ports und Protokolle usw.) in Ihren Abonnements abzufragen und zu ermitteln. Azure Resource Manager wird empfohlen, um aktuelle Ressourcen zu erstellen und zu verwenden. 

Sorgen Sie für entsprechende Berechtigungen (Leseberechtigungen) auf Ihrem Mandanten, und zählen Sie alle Azure-Abonnements sowie Ressourcen in Ihren Abonnements auf.

- [Schnellstart: Ausführen Ihrer ersten Resource Graph-Abfrage mithilfe des Azure Resource Graph-Explorers](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

- [Anzeigen Ihrer Azure-Abonnements](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

- [Grundlegendes zu Azure RBAC](https://docs.microsoft.com/azure/role-based-access-control/overview)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="62-maintain-asset-metadata"></a>6.2: Verwalten von Ressourcenmetadaten

**Leitfaden**: Wenden Sie Tags auf Ihre Azure-Ressourcen mit Metadaten an, die Ressourcen einer Taxonomie entsprechend logisch zu organisieren.

- [Erstellen und Verwenden von Tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Löschen nicht autorisierter Azure-Ressourcen

**Leitfaden**: Verwenden Sie nach Bedarf Tagging, Verwaltungsgruppen und separate Abonnements zum Organisieren und Nachverfolgen von Ressourcen. 

Stimmen Sie den Bestand regelmäßig ab, und stellen Sie sicher, dass nicht autorisierte Ressourcen rechtzeitig aus Ihrem Abonnement gelöscht werden.

- [Erstellen zusätzlicher Azure-Abonnements](https://docs.microsoft.com/azure/billing/billing-create-subscription)

- [Erstellen von Verwaltungsgruppen](https://docs.microsoft.com/azure/governance/management-groups/create)

- [Erstellen und Verwenden von Tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6.4: Definieren und Verwalten eines Bestands an genehmigten Azure-Ressourcen

**Leitfaden**: Erstellen Sie eine Liste der genehmigten Azure-Ressourcen gemäß den Anforderungen Ihrer Organisation, die Sie als Zulassungslistenmechanismus nutzen können. Dadurch kann Ihre Organisation ein Onboarding neu verfügbarer Azure-Dienste durchführen, nachdem sie formal überprüft und durch die üblichen Sicherheitsbewertungsprozesse Ihres Unternehmens genehmigt wurden.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Überwachung auf nicht genehmigte Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy, um Einschränkungen für den Typ der Ressourcen festzulegen, die in Ihren Abonnements erstellt werden können.

Verwenden Sie Azure Resource Graph, um Ressourcen in Ihren eigenen Abonnements abzufragen und zu ermitteln. 

Stellen Sie sicher, dass alle in der Umgebung vorhandenen Azure-Ressourcen genehmigt sind.

- [Konfigurieren und Verwalten von Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Schnellstart: Ausführen Ihrer ersten Resource Graph-Abfrage mithilfe des Azure Resource Graph-Explorers](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Einschränken der Möglichkeiten von Benutzern zur Interaktion mit Azure Resource Manager

**Leitfaden**: Mithilfe des bedingten Azure AD-Zugriffs können Sie die Möglichkeiten von Benutzern zur Interaktion mit Azure Resource Manager einschränken, indem Sie „Zugriff blockieren“ für die App zur „Verwaltung von Microsoft Azure“ konfigurieren.

- [Konfigurieren des bedingten Zugriffs, um den Zugriff auf Azure Resource Manager zu blockieren](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Physische oder logische Trennung von Anwendungen mit hohem Risiko

**Leitfaden**: Software, die für den Geschäftsbetrieb erforderlich ist, aber ein höheres Risiko für das Unternehmen darstellen kann, sollte innerhalb des eigenen virtuellen Computers und/oder virtuellen Netzwerks isoliert und entweder mit einer Azure Firewall oder einer Netzwerksicherheitsgruppe ausreichend gesichert werden.

- [Erstellen eines virtuellen Netzwerks](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

- [Erstellen einer Netzwerksicherheitsgruppe mit einer Sicherheitskonfiguration](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="secure-configuration"></a>Sichere Konfiguration

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Sichere Konfiguration](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Einrichten sicherer Konfigurationen für alle Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy-Aliase, um benutzerdefinierte Richtlinien zur Überprüfung oder Durchsetzung der Konfiguration Ihrer Azure-Ressourcen zu erstellen. Verwenden Sie integrierte Azure Policy-Definitionen.

Mit Azure Resource Manager ist es möglich, die Vorlage in JSON-Code (JavaScript Object Notation) zu exportieren. Dieser sollte überprüft werden, um sicherzustellen, dass die Konfigurationen die Sicherheitsanforderungen für Ihre Organisation erfüllen.

Exportieren Sie Azure Resource Manager-Vorlagen in JSON-Formate(JavaScript Object Notation), und überprüfen Sie diese regelmäßig, um sicherzustellen, dass die Konfigurationen den Sicherheitsanforderungen Ihrer Organisation entsprechen. 

Implementieren Sie Empfehlungen von Security Center als sichere Konfigurationsbaseline für Ihre Azure-Ressourcen. 

- [Anzeigen verfügbarer Azure Policy-Aliase](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

- [Tutorial: Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Exportieren von einzelnen oder mehreren Ressourcen in eine Vorlage im Azure-Portal](https://docs.microsoft.com/azure/azure-resource-manager/templates/export-template-portal)

- [Sicherheitsempfehlungen: Referenzhandbuch](https://docs.microsoft.com/azure/security-center/recommendations-reference)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Verwalten von sicheren Konfigurationen für Azure-Ressourcen

**Leitfaden**: Verwenden Sie die Azure Policy-Auswirkungen [Deny] und [DeployIfNotExists], um sichere Einstellungen für alle Ihre Azure-Ressourcen zu erzwingen.  Sie können auch Azure Resource Manager-Vorlagen verwenden, um die Sicherheitskonfiguration der für Ihre Organisation erforderlichen Azure-Ressourcen zu verwalten. 

- [Grundlegendes zu Azure Policy-Auswirkungen](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

- [Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Übersicht über Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/templates/overview)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Sicheres Speichern der Konfiguration von Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure DevOps, um Ihren Code sicher zu speichern und zu verwalten. Dazu zählen z. B. benutzerdefinierte Azure Policy-Richtlinien, Azure Resource Manager-Vorlagen und Desired State Configuration-Skripts. 

Erteilen oder verweigern Sie bestimmten Benutzern, integrierten Sicherheitsgruppen oder in Azure Active Directory (Azure AD) definierten Gruppen (bei Integration mit Azure DevOps) oder in Active Directory definierten Gruppen (bei Integration mit TFS) Berechtigungen.

- [Speichern von Code in Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Informationen über Berechtigungen und Gruppen in Azure DevOps](https://docs.microsoft.com/azure/devops/organizations/security/about-permissions)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Bereitstellen von Konfigurationsverwaltungstools für Azure-Ressourcen

**Leitfaden**: Definieren und implementieren Sie Standardsicherheitskonfigurationen für Azure-Ressourcen mit Azure Policy.  Verwenden Sie Azure Policy-Aliase, um benutzerdefinierte Richtlinien zur Überprüfung oder Durchsetzung der Netzwerkkonfiguration Ihrer Azure-Ressourcen zu erstellen. Implementieren Sie integrierte Richtliniendefinitionen, die sich auf Ihre spezifischen Load Balancer-Ressourcen beziehen.  Verwenden Sie außerdem Azure Automation, um Konfigurationsänderungen für Ihre Load Balancer-Ressourcen bereitzustellen.

- [Konfigurieren und Verwalten von Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Verwenden von Aliasen](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#aliases)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementieren der automatisierten Konfigurationsüberwachung für Azure-Ressourcen

**Leitfaden**: Verwenden Sie Security Center, um Baselinescans für Ihre Azure-Ressourcen auszuführen, und Azure Policy zum Warnen und zum Überwachen von Ressourcenkonfigurationen.

- [Umsetzen von Empfehlungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

## <a name="incident-response"></a>Reaktion auf Vorfälle

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Reaktion auf Vorfälle](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Erstellen eines Verfahrens zur Bewertung und Priorisierung von Vorfällen

**Anleitung:** Security Center weist jeder Warnung einen Schweregrad zu, damit Sie priorisieren können, welche Warnungen zuerst untersucht werden sollen. 

Der Schweregrad basiert darauf, wie zuversichtlich Security Center in Bezug auf den Befund oder die Analyse ist, die zum Auslösen der Warnung verwendet wird, sowie auf dem Zuverlässigkeitsgrad, dass hinter der Aktivität, die zu der Warnung führte, eine böswillige Absicht stand.

Markieren Sie Abonnements mithilfe von Tags, und erstellen Sie ein Benennungssystem, um Azure-Ressourcen eindeutig zu identifizieren und zu kategorisieren, insbesondere solche, die vertrauliche Daten verarbeiten.  

Die Priorisierung der Behebung von Warnungen basierend auf der Wichtigkeit der Azure-Ressourcen und der Umgebung, in der der Vorfall aufgetreten ist, liegt in Ihrer Verantwortung.

- [Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-overview)

- [Verwenden von Tags zum Organisieren von Azure-Ressourcen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Integrieren von Sicherheitswarnungen in das System zur Reaktion auf Vorfälle

**Leitfaden**: Exportieren Sie Ihre Warnungen und Empfehlungen von Security Center mithilfe des fortlaufenden Exportfeatures, um Risiken für Azure-Ressourcen zu identifizieren. 

Über die Funktion „Fortlaufender Export“ in Security Center können Sie Warnungen und Empfehlungen entweder manuell oder kontinuierlich exportieren. 

Verwenden Sie den Azure Security Center-Datenconnector, um die Warnungen an Azure Sentinel zu streamen.

- [Konfigurieren des fortlaufenden Exports](https://docs.microsoft.com/azure/security-center/continuous-export)

- [Streamen von Warnungen in Azure Sentinel](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatisieren der Reaktion auf Sicherheitswarnungen

**Leitfaden**: Verwenden Sie die Funktion „Workflowautomatisierung“ in Security Center, um automatisch Reaktionen auf Sicherheitswarnungen und -empfehlungen auszulösen und Ihre Azure-Ressourcen noch besser zu schützen.

- [Konfigurieren von Workflowautomatisierung in Security Center](https://docs.microsoft.com/azure/security-center/workflow-automation)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

## <a name="penetration-tests-and-red-team-exercises"></a>Penetrationstests und Red Team-Übungen

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Penetrationstests und Red Team-Übungen](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1: Durchführen regelmäßiger Penetrationstests Ihrer Azure-Ressourcen und Sicherstellen der Behebung aller kritischen Sicherheitsergebnissen

**Leitfaden**: Befolgen Sie in dem Fall die Einsatzregeln für Penetrationstests von Microsoft Cloud, um sicherzustellen, dass die Penetrationstests nicht gegen Microsoft-Richtlinien verstoßen. Nutzen Sie die Microsoft-Strategie und Durchführung von Red Team- und Livewebsite-Penetrationstests für von Microsoft verwaltete Cloudinfrastruktur, Dienste und Anwendungen. 

- [Penetrationstests – Rules of Engagement](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Cloud Red Teaming](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Shared

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie den [Vergleichstest für die Azure-Sicherheit](/azure/security/benchmarks/overview).
- Erfahren Sie mehr über [Azure-Sicherheitsbaselines](/azure/security/benchmarks/security-baselines-overview).
