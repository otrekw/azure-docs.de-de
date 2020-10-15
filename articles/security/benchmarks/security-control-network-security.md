---
title: Azure-Sicherheitskontrolle – Netzwerksicherheit
description: Azure-Sicherheitskontrolle – Netzwerksicherheit
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: dad01212be3589af7167082ff22c624fa776772a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "82193121"
---
# <a name="security-control-network-security"></a>Sicherheitskontrolle: Netzwerksicherheit

Bei den Netzwerksicherheitsempfehlungen liegt der Schwerpunkt auf der Angabe, welchen Netzwerkprotokollen, TCP/UDP-Ports und mit dem Netzwerk verbundenen Diensten der Zugriff auf Azure-Dienste erlaubt oder verweigert wird.

## <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Schützen von Azure-Ressourcen in virtuellen Netzwerken

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.1 | 9.2, 9.4, 14.1, 14.2, 14.3 | Kunde |

Stellen Sie sicher, dass auf alle Virtual Network-Subnetzbereitstellungen eine Netzwerksicherheitsgruppe mit Netzwerkzugriffssteuerungen angewendet wurde, die für die vertrauenswürdigen Ports und Quellen der Anwendung spezifisch sind. Verwenden Sie bei Verfügbarkeit private Endpunkte mit Private Link, um Ihre Azure-Dienstressourcen in Ihrem virtuellen Netzwerk zu schützen, indem Sie die VNet-Identität auf den Dienst ausweiten. Wenn private Endpunkte und Private Link nicht verfügbar sind, verwenden Sie Dienstendpunkte. Informationen zu dienstspezifischen Anforderungen finden Sie in der Sicherheitsempfehlung für den jeweiligen Dienst. 

Sollte Ihr Anwendungsfall speziell sein, können die Anforderungen alternativ auch durch Implementieren von Azure Firewall erfüllt werden.

- [Grundlegendes zu VNET-Dienstendpunkten](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview)

- [Grundlegendes zu Azure Private Link](https://docs.microsoft.com/azure/private-link/private-link-overview)

- [Erstellen eines virtuellen Netzwerks](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

- [Erstellen einer NSG mit einer Sicherheitskonfiguration](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

- [Bereitstellen und Konfigurieren von Azure Firewall](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

## <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1.2: Überwachen und Protokollieren der Konfiguration und des Datenverkehrs von virtuellen Netzwerken, Subnetzen und Netzwerkkarten (NICs)

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.2 | 9.3, 12.2, 12.8 | Kunde |

Verwenden Sie Azure Security Center, und befolgen Sie die Empfehlungen für den Netzwerkschutz, um Ihre Netzwerkressourcen in Azure abzusichern. Aktivieren Sie NSG-Flussprotokolle und senden Sie Protokolle an ein Speicherkonto für die Datenverkehrsüberwachung. Sie können auch NSG-Flussprotokolle an einen Log Analytics-Arbeitsbereich senden und mithilfe von Traffic Analytics Einblicke in den Datenverkehrsfluss in Ihrer Azure-Cloud ermöglichen. Einige Vorteile von Traffic Analytics sind die Möglichkeit, die Netzwerkaktivität zu visualisieren und Hotspots zu erkennen, Sicherheitsbedrohungen zu identifizieren, Datenverkehrsflussmuster zu verstehen und Netzwerkfehlkonfigurationen zu ermitteln.

- [Aktivieren der NSG-Flussprotokolle](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

- [Aktivieren und Verwenden von Traffic Analytics](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

- [Grundlegendes zu der von Azure Security Center bereitgestellten Netzwerksicherheit](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)

## <a name="13-protect-critical-web-applications"></a>1.3: Schützen kritischer Webanwendungen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.3 | 9.5 | Kunde |

Stellen Sie die Azure Web Application Firewall (WAF) vor kritischen Webanwendungen bereit, um den eingehenden Datenverkehr weiteren Überprüfungen zu unterziehen. Aktivieren Sie die Diagnoseeinstellung für WAF, und erfassen Sie Protokolle in ein Speicherkonto, einen Event Hub oder einen Log Analytics-Arbeitsbereich.

- [Bereitstellen von Azure WAF](https://docs.microsoft.com/azure/web-application-firewall/ag/create-waf-policy-ag)

## <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Ablehnen der Kommunikation mit bekannten bösartigen IP-Adressen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.4 | 12,3 | Kunde |

Aktivieren Sie den DDoS-Standardschutz in Ihren virtuellen Azure-Netzwerken, um sie vor DDoS-Angriffe zu schützen. Verwenden Sie die integrierten Informationen zu Bedrohungen in Azure Security Center, um die Kommunikation mit bekannten schädlichen IP-Adressen zu verweigern.

Stellen Sie die Azure Firewall an allen Netzwerkgrenzen der Organisation bereit, aktiviert Sie Threat Intelligence, und konfigurieren Sie das Feature so, dass schädlicher Netzwerkdatenverkehr „mit Warnungen gemeldet und verweigert“ wird.

Verwenden Sie den Just-in-Time-Netzwerkzugriff in Azure Security Center, um NSGs so zu konfigurieren, dass die Exposition von Endpunkten auf genehmigte IP-Adressen und einen begrenzten Zeitraum eingeschränkt wird.

Verwenden Sie die adaptive Netzwerkhärtung in Azure Security Center, um NSG-Konfigurationen zu empfehlen, die Ports und Quell-IPs basierend auf tatsächlichem Datenverkehr und Threat Intelligence einschränken.

- [Konfigurieren von DDoS-Schutz](https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection)

- [Bereitstellen von Azure Firewall](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

- [Grundlegendes zur integrierten Threat Intelligence in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer)

- [Grundlegendes zur adaptiven Netzwerkhärtung von Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-adaptive-network-hardening)

- [Grundlegendes zur Just-in-Time-Netzwerkzugriffssteuerung in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)

## <a name="15-record-network-packets"></a>1.5: Aufzeichnen von Netzwerkpaketen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.5 | 12,5 | Kunde |

Aktivieren Sie die Network Watcher-Paketerfassung, um anomale Aktivitäten zu untersuchen.

- [Aktivieren von Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-create)

## <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Bereitstellen von netzwerkbasierten Angriffserkennungs-/Eindringschutzsystemen (IDS/IPS)

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.6 | 12.6, 12.7 | Kunde |

Wählen Sie ein Angebot aus Azure Marketplace aus, das IDS-/IPS-Funktionen mit Funktionen zur Nutzlastuntersuchung unterstützt.  Wenn Angriffserkennung und/oder -verhinderung auf der Grundlage der Nutzlastüberprüfung keine Anforderung ist, kann Azure Firewall mit Threat Intelligence verwendet werden. Das Filtern auf Basis von Threat Intelligence mit Azure Firewall kann vor Datenverkehr von und zu bekannten schädlichen IP-Adressen oder Domänen warnen und diesen verweigern. Die IP-Adressen und Domänen stammen aus dem Microsoft Threat Intelligence-Feed.

Stellen Sie die Firewalllösung Ihrer Wahl an allen Netzwerkgrenzen Ihrer Organisation bereit, um schädlichen Datenverkehr zu erkennen und/oder zu verweigern.

- [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

- [Bereitstellen von Azure Firewall](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

- [Konfigurieren von Warnungen mit Azure Firewall](https://docs.microsoft.com/azure/firewall/threat-intel)

## <a name="17-manage-traffic-to-web-applications"></a>1.7: Verwalten von Datenverkehr für Webanwendungen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.7 | 12.9, 12.10 | Kunde |

Stellen Sie Azure Application Gateway für Webanwendungen mit aktiviertem HTTPS/TLS für vertrauenswürdige Zertifikate bereit.

- [Bereitstellen von Application Gateway](https://docs.microsoft.com/azure/application-gateway/quick-create-portal)

- [Konfigurieren von Application Gateway für die Verwendung von HTTPS](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal)

- [Grundlegendes zum Lastenausgleich der Ebene 7 mit Azure-Webanwendungsgateways](https://docs.microsoft.com/azure/application-gateway/overview)

## <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimieren der Komplexität und des Verwaltungsaufwands von Netzwerksicherheitsregeln

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.8 | 1.5 | Kunde |

Verwenden Sie Virtual Network-Diensttags, um Netzwerkzugriffssteuerungen in Netzwerksicherheitsgruppen oder Azure Firewall zu definieren. Sie können Diensttags anstelle von spezifischen IP-Adressen nutzen, wenn Sie Sicherheitsregeln erstellen. Indem Sie den Diensttagnamen (z. B. „ApiManagement“) im entsprechenden Quell- oder Zielfeld einer Regel angeben, können Sie den Datenverkehr für den entsprechenden Dienst zulassen oder verweigern. Microsoft verwaltet die Adresspräfixe, für die das Diensttag gilt, und aktualisiert das Diensttag automatisch, wenn sich die Adressen ändern.

Sie können auch Anwendungssicherheitsgruppen verwenden, um eine komplexe Sicherheitskonfiguration zu vereinfachen. Mit Anwendungssicherheitsgruppen können Sie die Netzwerksicherheit als natürliche Erweiterung einer Anwendungsstruktur konfigurieren und virtuelle Computer gruppieren sowie auf der Grundlage dieser Gruppen Netzwerksicherheitsrichtlinien definieren.

- [Grundlegendes zu Diensttags und deren Verwendung](https://docs.microsoft.com/azure/virtual-network/service-tags-overview)

- [Grundlegendes zu Anwendungssicherheitsgruppen und deren Verwendung](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups)

## <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Beibehalten von Standardsicherheitskonfigurationen für Netzwerkgeräte

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.9 | 11,1 | Kunde |

Definieren und implementieren Sie Standardsicherheitskonfigurationen für Netzwerkressourcen mit Azure Policy.

Sie können umfangreiche Azure-Bereitstellungen auch mithilfe von Azure Blueprints vereinfachen, indem Sie wichtige Umgebungsartefakte wie Azure Resource Manager-Vorlagen, RBAC-Steuerelemente und Richtlinien in einer einzigen Blaupausendefinition verpacken. Sie können die Blaupause auf neue Abonnements anwenden sowie die Steuerung und Verwaltung durch die Versionsverwaltung optimieren.

- [Konfigurieren und Verwalten von Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Azure Policy-Beispiele für Netzwerke](https://docs.microsoft.com/azure/governance/policy/samples/#network)

- [Erstellen einer Azure-Blaupause](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

## <a name="110-document-traffic-configuration-rules"></a>1.10: Dokumentieren von Datenverkehrskonfigurationsregeln

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.10 | 11.2 | Kunde |

Verwenden Sie Tags für NSGs und andere Ressourcen im Zusammenhang mit Netzwerksicherheit und Datenverkehrsfluss. Verwenden Sie für einzelne NSG-Regeln das Feld „Beschreibung“, um geschäftliche Anforderungen und/oder deren Dauer (usw.) für alle Regeln festzulegen, die Datenverkehr in ein bzw. aus einem Netzwerk zulassen.

Verwenden Sie eine der integrierten Azure Policy-Definitionen zum Tagging, z. B. „Tag und zugehörigen Wert erzwingen“, um sicherzustellen, dass alle Ressourcen mit Tags erstellt werden und Sie über vorhandene nicht markierte Ressourcen benachrichtigt werden.

Sie können Azure PowerShell oder die Azure-Befehlszeilenschnittstelle verwenden, um Ressourcen basierend auf ihren Tags zu suchen oder Aktionen auszuführen.

- [Erstellen und Verwenden von Tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

- [Erstellen eines virtuellen Netzwerks](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

- [Erstellen einer NSG mit einer Sicherheitskonfiguration](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

## <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Verwenden automatisierter Tools zum Überwachen von Netzwerkressourcenkonfigurationen und Erkennen von Änderungen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.11 | 11,3 | Kunde |

Verwenden Sie das Azure-Aktivitätsprotokoll zum Überwachen von Ressourcenkonfigurationen und Erkennen von Änderungen an Ihren Azure-Ressourcen. Erstellen Sie in Azure Monitor Warnungen, die bei Änderungen an wichtigen Ressourcen ausgelöst werden.

- [Anzeigen und Abrufen von Azure-Aktivitätsprotokollereignissen](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

- [Erstellen von Warnungen in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie in der nächsten Sicherheitskontrolle: [Protokollierung und Überwachung](security-control-logging-monitoring.md)
