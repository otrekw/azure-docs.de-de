---
title: Azure-Sicherheitskontrolle – Netzwerksicherheit
description: Sicherheitskontrolle – Netzwerksicherheit
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: d052226470042d374544de0b7e1ced4ca0f48a14
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2019
ms.locfileid: "75563956"
---
# <a name="security-control-network-security"></a>Sicherheitskontrolle: Netzwerksicherheit

Bei den Netzwerksicherheitsempfehlungen liegt der Schwerpunkt auf der Angabe, welchen Netzwerkprotokollen, TCP/UDP-Ports und mit dem Netzwerk verbundenen Diensten der Zugriff auf Azure-Dienste erlaubt oder verweigert wird.

## <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1: Schützen von Ressourcen mithilfe von Netzwerksicherheitsgruppen oder Azure Firewall in Virtual Network

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.1 | 9.2, 9.4, 14.1-14.3 | Kunde |

Stellen Sie sicher, dass auf alle Virtual Network-Subnetzbereitstellungen eine Netzwerksicherheitsgruppe mit Netzwerkzugriffssteuerungen angewendet wurde, die für die vertrauenswürdigen Ports und Quellen der Anwendung spezifisch sind. Verwenden Sie Azure-Dienste, bei denen Private Link aktiviert ist, stellen Sie den Dienst in Ihrem VNET bereit, oder stellen Sie eine private Verbindung über private Endpunkte her. Informationen zu dienstspezifischen Anforderungen finden Sie in der Sicherheitsempfehlung für den jeweiligen Dienst.

Wenn Ihr Anwendungsfall speziell ist, können die Anforderungen alternativ auch durch Implementieren von Azure Firewall erfüllt werden.

Allgemeine Informationen über Private Link: https://docs.microsoft.com/azure/private-link/private-link-overview

Erstellen eines virtuellen Netzwerks:

https://docs.microsoft.com/azure/virtual-network/quick-create-portal

Erstellen einer NSG mit einer Sicherheitskonfiguration:

https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

Bereitstellen und Konfigurieren von Azure Firewall:

https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

## <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2: Überwachen und Protokollieren der Konfiguration und des Datenverkehrs von VNETs, Subnetzen und Netzwerkkarten (NICs)

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.2 | 9.3, 12.2 | Kunde |

Verwenden Sie Azure Security Center, und befolgen Sie die Empfehlungen für den Netzwerkschutz, um Ihre Netzwerkressourcen in Azure abzusichern. Aktivieren Sie NSG-Flussprotokolle und senden Sie Protokolle an ein Speicherkonto für die Datenverkehrsüberwachung.

Aktivieren der NSG-Flussprotokolle:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Grundlegendes zur Netzwerksicherheit, die von Azure Security Center bereitgestellt wird:

https://docs.microsoft.com/azure/security-center/security-center-network-recommendations

## <a name="13-protect-critical-web-applications"></a>1.3: Schützen kritischer Webanwendungen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.3 | 9.5 | Kunde |

Stellen Sie die Azure Web Application Firewall (WAF) vor kritischen Webanwendungen bereit, um den eingehenden Datenverkehr weiteren Überprüfungen zu unterziehen. Aktivieren Sie die Diagnoseeinstellung für WAF, und erfassen Sie Protokolle in ein Speicherkonto, einen Event Hub oder einen Log Analytics-Arbeitsbereich.

Bereitstellen von Azure WAF:

https://docs.microsoft.com/azure/web-application-firewall/ag/create-waf-policy-ag

## <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Ablehnen der Kommunikation mit bekannten bösartigen IP-Adressen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.4 | 12,3 | Kunde |

Aktivieren Sie den DDoS-Standardschutz in Ihren virtuellen Azure-Netzwerken, um sie vor DDoS-Angriffe zu schützen. Verwenden Sie die integrierten Informationen zu Bedrohungen in Azure Security Center, um die Kommunikation mit bekannten schädlichen IP-Adressen zu verweigern.

Stellen Sie die Azure Firewall an allen Netzwerkgrenzen der Organisation bereit, aktiviert Sie Threat Intelligence, und konfigurieren Sie das Feature so, dass schädlicher Netzwerkdatenverkehr &quot;mit Warnungen gemeldet und verweigert&quot; wird.

Verwenden Sie den Just-in-Time-Netzwerkzugriff in Azure Security Center, um NSGs so zu konfigurieren, dass die Exposition von Endpunkten auf genehmigte IP-Adressen und einen begrenzten Zeitraum eingeschränkt wird.

Verwenden Sie die adaptive Netzwerkhärtung in Azure Security Center, um NSG-Konfigurationen zu empfehlen, die Ports und Quell-IPs basierend auf tatsächlichem Datenverkehr und Threat Intelligence einschränken.

Konfigurieren von DDoS-Schutz:

https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection

Bereitstellen von Azure Firewall:

https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

Grundlegendes zu integrierten Informationen zu Bedrohungen in Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer

Grundlegendes zur adaptiven Netzwerkhärtung in Azure Security Center

https://docs.microsoft.com/azure/security-center/security-center-adaptive-network-hardening

Grundlegendes zur Just-in-Time-Netzwerkzugriffssteuerung in Azure Security Center

https://docs.microsoft.com/azure/security-center/security-center-just-in-time

## <a name="15-record-network-packets-and-flow-logs"></a>1.5: Aufzeichnen von Netzwerkpaketen und Flussprotokollen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.5 | 12.5, 15.8 | Kunde |

Aufzeichnen von NSG-Flussprotokollen in einem Speicherkonto, um Flussdatensätze zu generieren. Aktivieren Sie Network Watcher-Paketerfassung, falls dies für die Untersuchung anomaler Aktivitäten erforderlich ist.

Aktivieren der NSG-Flussprotokolle: https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Aktivieren von Network Watcher: https://docs.microsoft.com/azure/network-watcher/network-watcher-create

## <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Bereitstellen von netzwerkbasierten Angriffserkennungs-/Eindringschutzsystemen (IDS/IPS)

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.6 | 12.6, 12.7 | Kunde |

Stellen Sie die Azure Firewall an allen Netzwerkgrenzen der Organisation bereit, aktiviert Sie Threat Intelligence, und konfigurieren Sie das Feature so, dass schädlicher Netzwerkdatenverkehr &quot;mit Warnungen gemeldet und verweigert&quot; wird.

Bereitstellen von Azure Firewall: https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

Konfigurieren von Warnungen mit Azure Firewall: https://docs.microsoft.com/azure/firewall/threat-intel

## <a name="17-manage-traffic-to-web-applications"></a>1.7: Verwalten von Datenverkehr für Webanwendungen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.7 | 12.9, 12.10 | Kunde |

Stellen Sie Azure Application Gateway für Webanwendungen mit aktiviertem HTTPS/SSL für vertrauenswürdige Zertifikate bereit.

Bereitstellen von Application Gateway: https://docs.microsoft.com/azure/application-gateway/quick-create-portal

Konfigurieren von Application Gateway für die Verwendung von HTTPS: https://docs.microsoft.com/azure/application-gateway/create-ssl-portal

Grundlegendes zum Lastenausgleich der Ebene 7 mit Azure-Webanwendungsgateways: https://docs.microsoft.com/azure/application-gateway/overview

## <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimieren der Komplexität und des Verwaltungsaufwands von Netzwerksicherheitsregeln

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.8 | 1.5 | Kunde |

Verwenden Sie Virtual Network-Diensttags, um Netzwerkzugriffssteuerungen in Netzwerksicherheitsgruppen oder Azure Firewall zu definieren. Sie können Diensttags anstelle von spezifischen IP-Adressen nutzen, wenn Sie Sicherheitsregeln erstellen. Indem Sie den Diensttagnamen (z. B. „ApiManagement“) im entsprechenden Quell- oder Zielfeld einer Regel angeben, können Sie den Datenverkehr für den entsprechenden Dienst zulassen oder verweigern. Microsoft verwaltet die Adresspräfixe, für die das Diensttag gilt, und aktualisiert das Diensttag automatisch, wenn sich die Adressen ändern.

Grundlegendes zu und Verwenden von Diensttags: https://docs.microsoft.com/azure/virtual-network/service-tags-overview

## <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Beibehalten von Standardsicherheitskonfigurationen für Netzwerkgeräte

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.9 | 11,1 | Kunde |

Definieren und implementieren Sie Standardsicherheitskonfigurationen für Netzwerkressourcen mit Azure Policy.

Sie können auch Azure Blueprints verwenden, um umfangreiche Azure-Bereitstellungen zu vereinfachen, indem wichtige Umgebungsartefakte wie Azure Resource Manager-Vorlagen, RBAC-Steuerelemente und Richtlinien in einer einzigen Blaupausendefinition verpackt werden. Sie können die Blaupause auf neue Abonnements und Umgebungen anwenden und die Steuerung und Verwaltung durch die Versionsverwaltung optimieren.

Konfigurieren und Verwalten von Azure Policy:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Policy-Beispiele für Netzwerke:

https://docs.microsoft.com/azure/governance/policy/samples/#network

Erstellen einer Azure-Blaupause:

https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal

## <a name="110-document-traffic-configuration-rules"></a>1.10: Dokumentieren von Datenverkehrskonfigurationsregeln

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.1 | 11.2 | Kunde |

Verwenden Sie Tags für NSGs und andere Ressourcen im Zusammenhang mit Netzwerksicherheit und Datenverkehrsfluss. Verwenden Sie für einzelne NSG-Regeln das Feld &quot;Beschreibung&quot;, um geschäftliche Anforderungen und/oder deren Dauer (usw.) für alle Regeln festzulegen, die Datenverkehr in das bzw. aus einem Netzwerk zulassen.

Erstellen und Verwenden von Tags:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Erstellen eines virtuellen Netzwerks:

https://docs.microsoft.com/azure/virtual-network/quick-create-portal

Erstellen einer NSG mit einer Sicherheitskonfiguration:

https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

## <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Verwenden automatisierter Tools zum Überwachen von Netzwerkressourcenkonfigurationen und Erkennen von Änderungen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 1.11 | 11,3 | Kunde |

Verwenden Sie Azure Policy, um die Konfiguration für Netzwerkressourcen zu überprüfen (und/oder zu korrigieren).

Konfigurieren und Verwalten von Azure Policy:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Policy-Beispiele für Netzwerke:

https://docs.microsoft.com/azure/governance/policy/samples/#network

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie in der nächsten Sicherheitskontrolle: [Protokollierung und Überwachung](security-control-logging-monitoring.md)