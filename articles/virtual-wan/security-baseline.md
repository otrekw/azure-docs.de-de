---
title: Azure-Sicherheitsbaseline für Virtual WAN
description: Die Sicherheitsbaseline für Virtual WAN enthält Schritt-für-Schritt-Anleitungen und Ressourcen für die Implementierung der Sicherheitsempfehlungen aus dem Vergleichstest für die Azure-Sicherheit.
author: msmbaldwin
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 11/03/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 409c8d163b52e56a1eef68a23e4db79956a68722
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93328620"
---
# <a name="azure-security-baseline-for-virtual-wan"></a>Azure-Sicherheitsbaseline für Virtual WAN

Diese Sicherheitsbaseline wendet Empfehlungen des [Azure-Sicherheitsvergleichstests Version 2.0](../security/benchmarks/overview.md) auf Microsoft Azure Virtual WAN an. Der Azure-Sicherheitsvergleichstest enthält Empfehlungen zum Schutz Ihrer Cloudlösungen in Azure. Der Inhalt wird anhand der **Sicherheitskontrollen** gruppiert, die durch den Azure-Sicherheitsvergleichstest und die entsprechenden für Virtual WAN geltenden Empfehlungen definiert werden. Nicht auf Virtual WAN anwendbare **Kontrollen** wurden ausgeschlossen.

Um die vollständige Zuordnung von Virtual WAN zum Azure-Sicherheitsvergleichstest anzuzeigen, sehen Sie sich die [vollständige Zuordnungsdatei der Virtual WAN-Sicherheitsbaseline](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines) an.

## <a name="network-security"></a>Netzwerksicherheit

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Netzwerksicherheit](/azure/security/benchmarks/security-controls-v2-network-security).*

### <a name="ns-1-implement-security-for-internal-traffic"></a>NS-1: Implementieren der Sicherheit für internen Datenverkehr

**Leitfaden** : Azure Virtual WAN unterstützt nicht die direkte Bereitstellung in einem virtuellen Netzwerk. Sie können jedoch weiterhin Regeln von Netzwerksicherheitsgruppen auf Spoke-Ressourcen anwenden, die Schutzmechanismen von Azure Firewall verwenden oder benutzerdefinierte Routingtabellen erstellen, um privaten Datenverkehr zu verhindern oder zuzulassen.

- [Benutzerdefinierte Routingszenarien](scenario-any-to-any.md)

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Kunde

### <a name="ns-2-connect-private-networks-together"></a>NS-2: Verbinden privater Netzwerke 

**Leitfaden** : Stellen Sie mit Azure ExpressRoute oder dem virtuellen privaten Azure-Netzwerk (VPN) private Verbindungen mit Azure-Rechenzentren und lokaler Infrastruktur in einer Housingumgebung her. ExpressRoute-Verbindungen werden nicht über das öffentliche Internet hergestellt, bieten mehr Zuverlässigkeit, eine höhere Geschwindigkeit und weniger Latenz als herkömmliche Internetverbindungen. 

Für Point-to-Site-VPN und Site-to-Site-VPN verbinden Sie lokale Geräte oder Netzwerke mit einem virtuellen Netzwerk, indem Sie eine beliebige Kombination dieser VPN-Optionen und ExpressRoute verwenden. Stellen Sie eine Verbindung von zwei oder mehr virtuellen Netzwerken in Azure mittels Peering virtueller Netzwerke her. Der Netzwerkdatenverkehr zwischen mittels Peering verbundenen virtuellen Netzwerken ist privat und wird im Azure-Backbone-Netzwerk verwaltet.

- [ExpressRoute in Virtual WAN](virtual-wan-expressroute-portal.md)

- [Übersicht über Site-to-Site-VPN](virtual-wan-site-to-site-portal.md)

- [Übersicht über Point-to-Site-VPN](virtual-wan-point-to-site-portal.md)

- [Private Link](howto-private-link.md)

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Shared

### <a name="ns-3-establish-private-network-access-to-azure-services"></a>NS-3: Einrichten des Zugriffs über das private Netzwerk auf Azure-Dienste

**Leitfaden** : Ermöglichen Sie mit Azure Private Link von Ihren virtuellen Netzwerken aus privaten Zugriff auf Azure Virtual WAN, ohne über das Internet zu gehen. Der private Zugriff ist neben der Authentifizierung und der von Azure-Diensten gebotenen Datenverkehrssicherheit eine weitere Verteidigungsmaßnahme.

- [Grundlegendes zu Azure Private Link](../private-link/private-link-overview.md)

- [Private Link für Virtual WAN](howto-private-link.md)

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Kunde

### <a name="ns-4-protect-applications-and-services-from-external-network-attacks"></a>NS-4: Schützen von Anwendungen und Diensten vor externen Netzwerkangriffen.

**Leitfaden** : Schützen Sie Ihre Azure Virtual WAN-Ressourcen vor Angriffen aus externen Netzwerken einschließlich verteilter Denial-of-Service-Angriffe (DDoS), anwendungsspezifischer Angriffe und unerwünschtem und potenziell schädlichem Internetdatenverkehr. 

Schützen Sie mit der Azure-Firewall Anwendungen und Dienste vor potenziell schädlichem Datenverkehr aus dem Internet und von anderen externen Standorten. Wählen Sie Azure DDoS Protection für Ihre Ressourcen zum Schutz vor Angriffen auf Ihre virtuellen Azure-Netzwerke. Verwenden Sie Azure Security Center, um Risiken durch Fehlkonfigurationen zu erkennen, die mit Ihren netzwerkbezogenen Ressourcen zusammenhängen.

- [Azure Firewall-Dokumentation](/azure/firewall)

- [Verwalten von Azure DDoS Protection Standard mithilfe des Azure-Portals](/azure/virtual-network/manage-ddos-protection) 

- [Azure Security Center-Empfehlungen](../security-center/recommendations-reference.md#recs-network)

**Azure Security Center-Überwachung** : Ja

**Verantwortlichkeit** : Shared

### <a name="ns-5-deploy-intrusion-detectionintrusion-prevention-systems-idsips"></a>NS-5: Bereitstellen von Angriffserkennungs-/Eindringschutzsystemen (Intrusion Detection/Intrusion Prevention Systems, IDS/IPS)

**Leitfaden** : Die beste Bereitstellung von Intrusion-Detection- oder Intrusion-Prevention-Systemen in Azure Virtual WAN ist die Verwendung eines virtuellen Netzwerkgeräts in den Spoke-Netzwerken, die an den virtuellen Hub angefügt sind.  Weitere Informationen zu den Routingszenarien finden Sie unter den angegebenen Links. 

- [Szenario: Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät](scenario-route-through-nva.md)

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Kunde

### <a name="ns-6-simplify-network-security-rules"></a>NS-6: Vereinfachen von Netzwerksicherheitsregeln

**Leitfaden** : Verwenden Sie Azure Virtual Network-Diensttags, um Netzwerkzugriffssteuerungen in Azure-Netzwerksicherheitsgruppen oder Azure Firewall zu definieren, die für Ihre Virtual WAN-Ressourcen konfiguriert sind. 

Verwenden Sie Diensttags anstelle von spezifischen IP-Adressen, wenn Sie Sicherheitsregeln erstellen. Durch Angabe des Diensttagnamens (z. B.: {VirtualWAN: Virtual Network}) im entsprechenden Quell- oder Zielfeld einer Regel können Sie den Datenverkehr für den entsprechenden Dienst zulassen oder verweigern. Microsoft verwaltet die Adresspräfixe, für die das Diensttag gilt, und aktualisiert das Diensttag automatisch, wenn sich die Adressen ändern.

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Kunde

### <a name="ns-7-secure-domain-name-service-dns"></a>NS-7: Secure Domain Name Service (DNS)

**Leitfaden** : Azure Virtual WAN bietet benutzerdefinierte DNS-Server für Point-to-Site-VPN Gateways. 

Befolgen Sie die bewährten Methoden für die DNS-Sicherheit, um häufige Angriffe wie verwaistes DNS, DNS-Verstärkungsangriffe, DNS-Vergiftungen und -Spoofing usw. zu vermeiden.

Wenn Azure DNS als autorisierender DNS-Dienst verwendet wird, stellen Sie sicher, dass DNS-Zonen und -Einträge vor versehentlicher oder böswilliger Änderung mit rollenbasierter Zugriffssteuerung in Azure (Azure RBAC) und Ressourcensperren geschützt werden.

- [Azure DNS – Übersicht](../dns/dns-overview.md) 

- [Secure Domain Name System (DNS) Deployment Guide](https://csrc.nist.gov/publications/detail/sp/800-81/2/final) (Bereitstellungshandbuch für Secure Domain Name System) 

- [Verhindern verwaister DNS-Einträge und Vermeiden von Unterdomänenübernahmen](../security/fundamentals/subdomain-takeover.md)

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Kunde

## <a name="identity-management"></a>Identitätsverwaltung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Identitätsverwaltung](/azure/security/benchmarks/security-controls-v2-identity-management).*

### <a name="im-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>IM-1: Standardisieren von Azure Active Directory als zentrales Identitäts- und Authentifizierungssystem

**Leitfaden** : Das Point-to-Site-VPN von Azure Virtual WAN ist mit Azure Active Directory (Azure AD) integriert, dem standardmäßigen Identitäts- und Zugriffsverwaltungsdienst von Azure. Sie sollten Azure AD als Standard für die Identitäts- und Zugriffsverwaltung Ihrer Organisation verwenden:

- Microsoft-Cloudressourcen, wie z. B. das Azure-Portal, Azure Storage, Azure Virtual Machines (Linux und Windows), Azure Key Vault, PaaS- und SaaS-Anwendungen.
- Die Ressourcen Ihrer Organisation, z. B. Anwendungen in Azure oder Ihre Unternehmensnetzwerkressourcen.

Sichern Sie Azure AD bei den Methoden Ihrer Organisation im Zusammenhang mit Cloudsicherheit mit hoher Priorität. Bewerten Sie Ihren Identitäts- und Sicherheitsstatus mit dem Feature zur Sicherheitsbewertung von Azure Security Center, um festzustellen, wie gut Ihre Konfiguration den Empfehlungen bewährter Methoden entspricht. Implementieren Sie Empfehlungen von Microsoft für bewährte Methoden zur Verbesserung Ihres Sicherheitsstatus.

Azure AD unterstützt auch externe Identitäten, die es Benutzern ohne Microsoft-Konto ermöglichen, sich mit ihrer externen Identität bei ihren Anwendungen und Ressourcen anzumelden. Lesen Sie Informationen zur Verwendung von Azure AD in Point-to-Site-VPN-Szenarien unter den angegebenen Links.

- [Erstellen eines Azure Active Directory (AD)-Mandanten für Verbindungen mit dem P2S OpenVPN-Protokoll](openvpn-azure-ad-tenant-multi-app.md)

- [Konfigurieren der Azure Active Directory-Authentifizierung für ein Benutzer-VPN](virtual-wan-point-to-site-azure-ad.md)

- [Mandanten in Azure Active Directory](../active-directory/develop/single-and-multi-tenant-apps.md) 

- [Erstellen und Konfigurieren einer Azure AD-Instanz](../active-directory/fundamentals/active-directory-access-create-new-tenant.md) 

- [Verwenden externer Identitätsanbieter für eine Anwendung](/azure/active-directory/b2b/identity-providers)

**Azure Security Center-Überwachung** : Ja

**Verantwortlichkeit** : Shared

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>IM-4: Verwenden stärkerer Authentifizierungssteuerungen für den gesamten Azure Active Directory-basierten Zugriff

**Leitfaden** : Das Point-to-Site-VPN von Azure Virtual WAN ist mit Azure Active Directory (Azure AD) integriert, das starke Authentifizierungssteuerungen mit mehrstufiger Authentifizierung und starke kennwortlose Methoden unterstützt.

- Mehrstufige Authentifizierung: Aktivieren Sie die mehrstufige Azure AD-Authentifizierung, und befolgen Sie die Empfehlungen der Identitäts- und Zugriffsverwaltung von Azure Security Center hinsichtlich bewährter Methoden für Ihr Setup mit mehrstufiger Authentifizierung. Die mehrstufige Authentifizierung kann für alle Benutzer, für ausgewählte Benutzer oder auf Benutzerebene auf der Grundlage von Anmeldebedingungen und Risikofaktoren durchgesetzt werden.

- Kennwortlose Authentifizierung: Es stehen drei Optionen für die kennwortlose Authentifizierung zur Verfügung. Dazu gehören Windows Hello for Business, die Microsoft Authenticator-App und lokale Authentifizierungsmethoden wie Smartcards.

Stellen Sie für Administratoren und privilegierte Benutzer sicher, dass die höchste Stufe der starken Authentifizierungsmethode durch die Einführung einer für andere Benutzer geeigneten starken Authentifizierungsrichtlinie verwendet wird.

- [Aktivieren der mehrstufigen Authentifizierung in Point-to-Site-VPN für Virtual WAN](openvpn-azure-ad-mfa.md)

**Azure Security Center-Überwachung** : Ja

**Verantwortlichkeit** : Shared

### <a name="im-6-restrict-azure-resource-access-based-on-conditions"></a>IM-6: Einschränken des Zugriffs auf Azure-Ressourcen basierend auf Bedingungen

**Leitfaden** : Das Point-to-Site-VPN von Azure Virtual WAN unterstützt den bedingten Azure AD-Zugriff für eine differenziertere Zugriffssteuerung auf der Grundlage benutzerdefinierter Bedingungen. So müssen z. B. Benutzeranmeldungen aus bestimmten IP-Bereichen für die Anmeldung eine mehrstufige Authentifizierung verwenden. Die Sitzungsverwaltungsrichtlinie für die differenzierte Authentifizierung kann ebenfalls für verschiedene Anwendungsfälle verwendet werden.

- [Allgemeine Richtlinien für bedingten Zugriff](../active-directory/conditional-access/concept-conditional-access-policy-common.md) 

- [Konfigurieren der Verwaltung von Authentifizierungssitzungen mit bedingtem Zugriff](../active-directory/conditional-access/howto-conditional-access-session-lifetime.md)

- [Bedingter Zugriff für Point-to-Site-VPN in Virtual WAN](openvpn-azure-ad-mfa.md#conditional)

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Kunde

### <a name="im-7-eliminate-unintended-credential-exposure"></a>IM-7: Beheben der unbeabsichtigten Offenlegung von Anmeldeinformationen

**Anleitung:** Implementieren Sie Credential Scanner, um Anmeldeinformationen im Code zu identifizieren. In Credential Scanner wird auch das Verschieben von ermittelten Anmeldeinformationen an sicherere Speicherorte (z. B. Azure Key Vault) empfohlen.

Für GitHub können Sie das native Feature zur geheimen Überprüfung verwenden, um Anmeldeinformationen oder eine andere Form von Geheimnissen im Code zu identifizieren.

- [Einrichten von Credential Scanner](https://secdevtools.azurewebsites.net/helpcredscan.html) 

- [Geheime Überprüfung in GitHub](https://docs.github.com/github/administering-a-repository/about-secret-scanning)

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Kunde

## <a name="privileged-access"></a>Privilegierter Zugriff

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Privilegierter Zugriff](/azure/security/benchmarks/security-controls-v2-privileged-access).*

### <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2: Einschränken des administrativen Zugriffs auf unternehmenskritische Systeme

**Leitfaden** : Azure Virtual WAN verwendet die rollenbasierte Zugriffssteuerung in Azure (Azure RBAC), um den Zugriff auf unternehmenskritische Systeme zu isolieren, indem die Konten begrenzt werden, denen privilegierter Zugriff auf die Abonnements und Verwaltungsgruppen gewährt wird, in denen sie sich befinden.

Schränken Sie auch den Zugriff auf die Verwaltungs-, Identitäts- und Sicherheitssysteme ein, die über Administratorzugriff für Ihren unternehmenskritischen Zugriff verfügen, z. B. Active Directory-Domänencontroller, Sicherheitstools und Systemverwaltungstools mit Agents, die auf unternehmenskritischen Systemen installiert sind. Angreifer, die diese Verwaltungs- und Sicherheitssysteme kompromittieren, können diese sofort ausnutzen, um geschäftskritische Ressourcen zu gefährden.

Alle Zugriffssteuerungstypen sollten an der Segmentierungsstrategie des Unternehmens ausgerichtet werden, um eine konsistente Zugriffssteuerung sicherzustellen.

- [Azure-Komponenten und -Referenzmodell](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151) 

- [Zugriff auf die Verwaltungsgruppe](../governance/management-groups/overview.md#management-group-access) 

- [Azure-Abonnementadministratoren](../cost-management-billing/manage/add-change-subscription-administrator.md)

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Kunde

### <a name="pa-6-use-privileged-access-workstations"></a>PA-6: Verwenden von Privileged Access Workstations

**Leitfaden** : Gesicherte, isolierte Arbeitsstationen sind von entscheidender Bedeutung für die Sicherheit sensibler Rollen wie Administratoren, Entwickler und Operatoren kritischer Dienste. Verwenden Sie stark gesicherte Benutzerworkstations oder Azure Bastion für administrative Aufgaben. Verwenden Sie Azure Active Directory (Azure AD), Microsoft Defender Advanced Threat Protection (ATP) oder Microsoft Intune, um eine sichere und verwaltete Benutzerworkstation für administrative Aufgaben einzurichten. Die gesicherten Workstations können zentral verwaltet werden, um eine gesicherte Konfiguration einschließlich starker Authentifizierung, Software- und Hardwarebaselines, eingeschränktem logischen und Netzwerkzugang durchzusetzen.

- [Informationen zu sicheren, von Azure verwalteten Arbeitsstationen](../active-directory/devices/concept-azure-managed-workstation.md)

- [Bereitstellen einer sicheren, von Azure verwalteten Arbeitsstation](../active-directory/devices/howto-azure-managed-workstation.md)

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Kunde

## <a name="data-protection"></a>Datenschutz

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Schutz von Daten](/azure/security/benchmarks/security-controls-v2-data-protection).*

### <a name="dp-4-encrypt-sensitive-information-in-transit"></a>DP-4: Verschlüsseln vertraulicher Informationen während der Übertragung

**Leitfaden** : Die Verschlüsselung ist entscheidend für den Datenverkehr in externen und öffentlichen Netzwerken.

- Durch die Verwendung von Zugriffssteuerungen sollten die Daten während der Übertragung durch Verschlüsselung vor Out-of-Band-Angriffen (z. B. Erfassung des Datenverkehrs) geschützt werden, um sicherzustellen, dass Angreifer die Daten nicht einfach lesen oder ändern können.

- Stellen Sie für den HTTP-Datenverkehr sicher, dass alle Clients, die Verbindungen mit Ihren Azure-Ressourcen herstellen, TLS 1.2 oder höher aushandeln können.

- Verwenden Sie für die Remoteverwaltung SSH (für Linux) bzw. RDP/TLS (für Windows) anstelle eines unverschlüsselten Protokolls. Veraltete SSL-/TLS-/SSH-Versionen und -Protokolle sowie schwache Verschlüsselungsverfahren sollten deaktiviert werden.

- In der zugrunde liegenden Infrastruktur stellt Azure für den Datenverkehr zwischen Azure-Rechenzentren standardmäßig die Verschlüsselung von Daten während der Übertragung zur Verfügung.

Im Allgemeinen stellen wir die Verschlüsselung auf dem sicheren Microsoft-Backbone und Möglichkeiten zur Verschlüsselung des Datenverkehrs in Ihrem Site-to-Site-VPN, Site-to-Site-VPN über Azure ExpressRoute und Point-to-Site-VPN (Benutzer) bereit.

- [Grundlegendes zur Verschlüsselung während der Übertragung mit Azure](../security/fundamentals/encryption-overview.md#encryption-of-data-in-transit)

- [Informationen zur TLS-Sicherheit](/security/engineering/solving-tls1-problem)

- [Doppelte Verschlüsselung für Azure-Daten während der Übertragung](../security/fundamentals/double-encryption.md#data-in-transit)

**Azure Security Center-Überwachung** : Ja

**Verantwortlichkeit** : Shared

## <a name="asset-management"></a>Ressourcenverwaltung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Ressourcenverwaltung](/azure/security/benchmarks/security-controls-v2-asset-management).*

### <a name="am-2-ensure-security-team-has-access-to-asset-inventory-and-metadata"></a>AM-2: Sicherstellen, dass das Sicherheitsteam Zugriff auf den Ressourcenbestand und die Metadaten hat

**Leitfaden** : Wenden Sie Tags auf Ihre Ressourcen, Ressourcengruppen und Abonnements in Azure an, um sie logisch in einer Taxonomie zu strukturieren. Jedes Tag besteht aus einem Paar mit einem Namen und einem Wert. So können Sie beispielsweise den Namen „Umgebung“ und den Wert „Produktion“ auf alle Ressourcen in der Produktion anwenden.

Azure Virtual WAN unterstützt auch auf Azure Resource Manager basierende Bereitstellungen von Ressourcen und Abfragen von Azure Resource Graph. 

- [Weitere Informationen zum Taggen von Ressourcen finden Sie im „Leitfaden zur Entscheidungsfindung für Ressourcenbenennung und -markierung“.](https://docs.microsoft.com/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=/azure/azure-resource-manager/management/toc.json)

- [Schnellstart: Ausführen Ihrer ersten Resource Graph-Abfrage mithilfe des Azure Resource Graph-Explorers](../governance/resource-graph/first-query-portal.md) 

- [Verwalten des Ressourcenbestands in Azure Security Center](../security-center/asset-inventory.md)

**Azure Security Center-Überwachung** : Ja

**Verantwortlichkeit** : Kunde

### <a name="am-3-use-only-approved-azure-services"></a>AM-3: Ausschließliche Verwendung genehmigter Azure-Dienste

**Leitfaden** : Verwenden Sie Azure Policy, um Dienste einzuschränken, die in Ihrer Umgebung bereitgestellt werden können. Suchen und ermitteln Sie Ressourcen mit Azure Resource Graph innerhalb Ihrer Abonnements, und verwenden Sie Azure Monitor, um Regeln zu erstellen, die Warnungen auslösen, wenn ein nicht genehmigter Dienst erkannt wird.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Ablehnen eines bestimmten Ressourcentyps mit Azure Policy](../governance/policy/samples/built-in-policies.md#general)

- [Schnellstart: Ausführen Ihrer ersten Resource Graph-Abfrage mithilfe des Azure Resource Graph-Explorers](../governance/resource-graph/first-query-portal.md)

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Kunde

### <a name="am-5-limit-users-ability-to-interact-with-azure-resource-manager"></a>AM-5: Einschränken der Möglichkeiten von Benutzern zur Interaktion mit Azure Resource Manager

**Leitfaden** : Verwenden Sie den bedingten Azure-Zugriff, um die Möglichkeiten von Benutzern zur Interaktion mit Azure Resource Manager einzuschränken, indem Sie „Zugriff blockieren“ für die App zur „Verwaltung von Microsoft Azure“ konfigurieren.

- [Verwalten des Zugriffs auf die Azure-Verwaltung mit bedingtem Zugriff](../role-based-access-control/conditional-access-azure-management.md)

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Kunde

## <a name="logging-and-threat-detection"></a>Protokollierung und Bedrohungserkennung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Protokollierung und Bedrohungserkennung](/azure/security/benchmarks/security-controls-v2-logging-threat-protection).*

### <a name="lt-2-enable-threat-detection-for-azure-identity-and-access-management"></a>LT-2: Aktivieren der Bedrohungserkennung für die Identitäts- und Zugriffsverwaltung in Azure

**Leitfaden** : Azure Active Directory (Azure AD) stellt die folgenden Benutzerprotokolle bereit, die Sie in der Azure AD-Berichterstellung anzeigen oder mit Azure Monitor, Azure Sentinel, SIEM- oder Überwachungstools integrieren können, um komplexere Anwendungsfälle für Überwachung und Analyse zu erhalten. Dies sind:

- Anmeldungen: Der Bericht „Anmeldungen“ enthält Informationen zur Nutzung von verwalteten Anwendungen und Aktivitäten der Benutzeranmeldung.
- Überwachungsprotokolle: Ermöglichen die Nachverfolgung sämtlicher Änderungen, die von verschiedenen Features in Azure AD vorgenommen wurden. Hierzu zählen unter anderem Änderungen an Ressourcen in Azure AD, z. B. das Hinzufügen oder Entfernen von Benutzern, Apps, Gruppen, Rollen und Richtlinien.
- Riskante Anmeldungen: Eine riskante Anmeldung ist ein Indikator für einen Anmeldeversuch von einem Benutzer, der nicht der rechtmäßige Besitzer eines Benutzerkontos ist.
- Benutzer mit Risikomarkierung: Ein Benutzer mit Risikomarkierung ist ein Indikator für ein ggf. kompromittiertes Benutzerkonto.

Verwenden Sie Azure Security Center zum Erstellen von Warnungen zu bestimmten verdächtigen Aktivitäten, z. B. eine übermäßige Anzahl fehlgeschlagener Authentifizierungsversuche, einschließlich veralteter Konten im Abonnement. Verwenden Sie zusätzlich zur grundlegenden Sicherheitsüberwachung das Threat Protection-Modul von Security Center, um ausführlichere Sicherheitswarnungen von einzelnen Azure-Computeressourcen (virtuelle Computer, Container, App Service), Datenressourcen (SQL-Datenbank und SQL-Speicher) und Azure-Dienstebenen zu sammeln. So erhalten Sie Einblick in Kontoanomalien innerhalb einzelner Ressourcen.

- [Berichte zu Überwachungsaktivitäten in Azure Active Directory](../active-directory/reports-monitoring/concept-audit-logs.md) 

- [Aktivieren von Azure Identity Protection](../active-directory/identity-protection/overview-identity-protection.md)

**Azure Security Center-Überwachung** : Ja

**Verantwortlichkeit** : Shared

### <a name="lt-3-enable-logging-for-azure-network-activities"></a>LT-3: Aktivieren der Protokollierung für Azure-Netzwerkaktivitäten

**Leitfaden** : Verwenden Sie Tools zur VPN-Paketerfassung, um Netzwerkpakete aufzuzeichnen, die zwischen Ihren Azure Virtual WAN-Ressourcen ausgetauscht werden. Dies könnte beim Debuggen in Bezug auf Ihr Hybridnetzwerk hilfreich sein. Sie können eine Netzwerksicherheitsgruppe zwar nicht in Virtual WAN, aber auf Ihren virtuellen Spoke-Netzwerkressourcen bereitstellen.

Aktivieren Sie Datenflussprotokolle der Netzwerksicherheitsgruppe in Ihren Netzwerksicherheitsgruppen für die Überwachung des Datenverkehrs.

Aktivieren Sie das Feature Azure Traffic Analytics, um Datenflussprotokolle zu verarbeiten, die im Speicherkonto aufbewahrt werden, und senden Sie sie dann an einen Log Analytics-Arbeitsbereich. Traffic Analytics bietet zusätzliche Einblicke in den Datenverkehrsfluss für Ihre Azure-Netzwerke. Einige Vorteile von Traffic Analytics sind die Möglichkeit, die Netzwerkaktivität zu visualisieren und Hotspots zu erkennen, Sicherheitsbedrohungen zu identifizieren, Datenverkehrsflussmuster zu verstehen und Netzwerkfehlkonfigurationen zu ermitteln.

- [Aktivieren von Datenflussprotokollen für Netzwerksicherheitsgruppen](../network-watcher/network-watcher-nsg-flow-logging-portal.md) 

- [Aktivieren und Verwenden von Traffic Analytics](../network-watcher/traffic-analytics.md) 

Virtual WAN produziert oder verarbeitet keine DNS-Abfrageprotokolle, die aktiviert werden müssten.

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Kunde

### <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4: Aktivieren der Protokollierung für Azure-Ressourcen

**Leitfaden** : Azure-Aktivitätsprotokolle, die automatisch aktiviert werden, enthalten alle Schreibvorgänge (PUT, POST, DELETE) für Ihre Azure Virtual WAN-Ressourcen, mit Ausnahme von Lesevorgängen (GET). Mit Aktivitätsprotokollen können Fehler ermittelt werden, und es kann nachverfolgt werden, welche Änderungen an einer Ressource ein Benutzer in Ihrer Organisation vorgenommen hat. Virtual WAN erzeugt jedoch keine Azure-Ressourcenprotokolle.

- [Sammeln von Plattformprotokollen und -metriken mit Azure Monitor](../azure-monitor/platform/diagnostic-settings.md) 
- [Grundlegendes zur Protokollierung und zu verschiedenen Protokolltypen in Azure](../azure-monitor/platform/platform-logs-overview.md)

**Azure Security Center-Überwachung** : Ja

**Verantwortlichkeit** : Shared

### <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5: Zentralisieren der Verwaltung und Analyse von Sicherheitsprotokollen

**Leitfaden** : Zentralisieren Sie die Speicherung und Analyse von Protokollen, um eine Korrelation zu ermöglichen. Stellen Sie sicher, dass jeder Protokollquelle ein Datenbesitzer, eine Zugriffsanleitung, ein Speicherort, die für die Verarbeitung und den Zugriff verwendeten Tools sowie Anforderungen zur Datenaufbewahrung zugewiesen werden. Stellen Sie sicher, dass die Azure-Aktivitätsprotokolle in Ihre zentralen Protokollierungssysteme integriert werden. 

Erfassen von Protokollen über Azure Monitor zum Aggregieren von Sicherheitsdaten, die von Endpunktgeräten, Netzwerkressourcen und anderen Sicherheitssystemen generiert werden. Verwenden Sie in Azure Monitor Log Analytics-Arbeitsbereiche, um Analysen abzufragen und auszuführen, und verwenden Sie Azure-Speicherkonten für langfristige Speicherung und Archivierung. Zusätzlich können Sie auch Daten in Azure Sentinel oder der SIEM-Lösung eines Drittanbieters aktivieren und integrieren.

Viele Unternehmen verwenden Azure Sentinel für „heiße“ Daten, die häufig verwendet werden, und Azure Storage für seltener verwendete „kalte“ Daten.

- [Sammeln von Plattformprotokollen und -metriken mit Azure Monitor](../azure-monitor/platform/diagnostic-settings.md) 

- [Durchführen des Onboardings für Azure Sentinel](../sentinel/quickstart-onboard.md)

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Shared

## <a name="incident-response"></a>Reaktion auf Vorfälle

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Reaktion auf Vorfälle](/azure/security/benchmarks/security-controls-v2-incident-response).*

### <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>IR-1: Vorbereitung – Aktualisieren des Prozesses zur Reaktion auf Vorfälle für Azure

**Leitfaden** : Stellen Sie sicher, dass Ihre Organisation über Prozesse verfügt, um auf Sicherheitsvorfälle zu reagieren, dass diese Prozesse für Azure aktualisiert wurden und dass sie regelmäßig trainiert werden, um die Bereitschaft zu gewährleisten. Stellen Sie sicher, dass Ihr Dienstangebot ggf. in den Prozess zur Reaktion auf Vorfälle einbezogen wird.

- [Implement security across the enterprise environment](https://aka.ms/AzSec4) (Implementieren von Sicherheit in der gesamten Unternehmensumgebung) 
- [Incident Response Reference Guide](https://aka.ms/IRRG) (Referenzleitfaden für die Reaktion auf Vorfälle)

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Kunde

### <a name="ir-2-preparation--setup-incident-notification"></a>IR-2: Vorbereitung – Einrichten von Ereignisbenachrichtigungen

**Leitfaden** : Richten Sie Kontaktinformationen für Sicherheitsvorfälle im Azure Security Center ein. Microsoft wendet sich unter den angegebenen Kontaktdaten an Sie, wenn das Microsoft Security Response Center (MSRC) feststellt, dass Personen unrechtmäßig oder unbefugt auf Ihre Daten zugegriffen haben. Sie haben auch die Möglichkeit, die Warnungen und Benachrichtigungen bei Vorfällen in verschiedenen Azure-Diensten entsprechend Ihrer Anforderungen bezüglich der Reaktion auf Vorfälle anzupassen.

Microsoft wendet sich unter den für Sicherheitsvorfälle angegebenen Kontaktdaten an Sie, wenn das Microsoft Security Response Center (MSRC) feststellt, dass Personen unrechtmäßig oder unbefugt auf Ihre Daten zugegriffen haben. Überprüfen Sie die Vorfälle anschließend, um sicherzustellen, dass die Probleme vollständig behoben wurden.

- [Festlegen der Kontaktinformationen in Azure Security Center](../security-center/security-center-provide-security-contact-details.md) 

- [Festlegen der Kontaktinformationen in Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Azure Security Center-Überwachung** : Ja

**Verantwortlichkeit** : Kunde

### <a name="ir-3-detection-and-analysis--create-incidents-based-on-high-quality-alerts"></a>IR-3: Erkennung und Analyse – Erstellen von Vorfällen basierend auf Warnungen mit hoher Qualität

**Leitfaden** : Azure Security Center weist jeder Warnung einen Schweregrad zu, damit Sie priorisieren können, welche Warnungen zuerst untersucht werden sollen. Der Schweregrad basiert darauf, wie zuversichtlich Security Center in Bezug auf den Befund oder die Analyse ist, die zum Auslösen der Warnung verwendet wird, sowie auf dem Zuverlässigkeitsgrad, dass hinter der Aktivität, die zu der Warnung führte, eine böswillige Absicht stand.

Markieren Sie außerdem Abonnements aussagekräftig (z. B. „Produktion“, „Nicht-Produktion“), und erstellen Sie ein Benennungssystem, um Azure-Ressourcen eindeutig zu identifizieren und zu kategorisieren.

- [Sicherheitswarnungen in Azure Security Center](../security-center/security-center-alerts-overview.md) 

- [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../azure-resource-manager/management/tag-resources.md)

**Azure Security Center-Überwachung** : Ja

**Verantwortlichkeit** : Kunde

### <a name="ir-4-detection-and-analysis--investigate-an-incident"></a>IR-4: Erkennung und Analyse – Untersuchen eines Vorfalls

**Anleitung:** Erarbeiten Sie einen Leitfaden für die Reaktion auf Vorfälle für Ihre Organisation. Führen Sie in regelmäßigen Abständen Tests zur Reaktionsfähigkeit Ihrer Systeme auf Vorfälle durch. Identifizieren Sie Schwachstellen und Lücken, und überarbeiten Sie den Plan bei Bedarf.

Stellen Sie sicher, dass es schriftliche Pläne für die Reaktion auf Vorfälle gibt, in denen alle Rollen der Mitarbeiter sowie die Phasen der Bearbeitung und Verwaltung von Vorfällen von der Ermittlung bis zur abschließenden Überprüfung definiert sind.

- [Leitfaden zu Planung und Betrieb](../security-center/security-center-planning-and-operations-guide.md) 

- [Anleitung zum Entwickeln eines Prozesses für die Reaktion auf Sicherheitsvorfälle](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process) 

- [Struktur eines Vorfalls laut Microsoft Security Response Center](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process) 

- [„Guide to Test, Training, and Exercise Programs for IT Plans and Capabilities“ des National Institute of Standards and Technology (NIST)](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Kunde

### <a name="ir-5-detection-and-analysis--prioritize-incidents"></a>IR-5: Erkennung und Analyse – Priorisieren von Vorfällen

**Leitfaden** : Azure Security Center weist jeder Warnung einen Schweregrad zu, damit Sie priorisieren können, welche Warnungen zuerst untersucht werden sollen. Der Schweregrad basiert darauf, wie zuversichtlich Security Center in Bezug auf den Befund oder die Analyse ist, die zum Auslösen der Warnung verwendet wird, sowie auf dem Zuverlässigkeitsgrad, dass hinter der Aktivität, die zu der Warnung führte, eine böswillige Absicht stand.

Markieren Sie außerdem Abonnements aussagekräftig (z. B. „Produktion“, „Nicht-Produktion“), und erstellen Sie ein Benennungssystem, um Azure-Ressourcen eindeutig zu identifizieren und zu kategorisieren.

- [Sicherheitswarnungen in Azure Security Center](../security-center/security-center-alerts-overview.md) 

- [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../azure-resource-manager/management/tag-resources.md)

**Azure Security Center-Überwachung** : Ja

**Verantwortlichkeit** : Shared

### <a name="ir-6-containment-eradication-and-recovery--automate-the-incident-handling"></a>IR-6: Eindämmung, Ausmerzung und Wiederherstellung – Automatisieren der Behandlung von Vorfällen

**Leitfaden** : Verwenden Sie die Funktion „Workflowautomatisierung“ in Azure Security Center, um über „Azure Logic Apps“ automatisch Reaktionen auf Sicherheitswarnungen und -empfehlungen auszulösen.

- [Konfigurieren von Workflowautomatisierung und Logic Apps](../security-center/workflow-automation.md)

**Azure Security Center-Überwachung** : Ja

**Verantwortlichkeit** : Kunde

## <a name="posture-and-vulnerability-management"></a>Status- und Sicherheitsrisikoverwaltung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Status- und Sicherheitsrisikoverwaltung](/azure/security/benchmarks/security-controls-v2-vulnerability-management).*

### <a name="pv-8-conduct-regular-attack-simulation"></a>PV-8: Regelmäßiges Durchführen einer Angriffssimulation

**Leitfaden** : Führen Sie nach Bedarf Penetrationstests oder Red Team-Aktivitäten für Ihre Azure-Ressourcen durch, und stellen Sie sicher, dass alle kritischen Sicherheitsergebnisse behandelt werden.

Befolgen Sie die Einsatzregeln für Penetrationstests für die Microsoft Cloud, um sicherzustellen, dass die Penetrationstests nicht gegen Microsoft-Richtlinien verstoßen. Nutzen Sie die Microsoft-Strategie und die Red Teaming- und Livewebsite-Penetrationstests für von Microsoft verwaltete Cloudinfrastrukturen, Dienste und Anwendungen.

- [Penetrationstests in Azure](../security/fundamentals/pen-testing.md)

- [Penetrationstests – Rules of Engagement](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Cloud Red Teaming](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Security Center-Überwachung** : Nicht verfügbar

**Verantwortlichkeit** : Kunde

## <a name="endpoint-security"></a>Endpunktsicherheit

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Endpunktsicherheit](/azure/security/benchmarks/security-controls-v2-endpoint-security).*

### <a name="es-1-use-endpoint-detection-and-response-edr"></a>ES-1: Verwenden von Endpunkterkennung und -antwort (Endpoint Detection and Response, EDR)

**Leitfaden** : Kunden ist es nicht ausdrücklich gestattet, Einstellungen für die Endpunkterkennung und -antwort zu konfigurieren. Die im Azure Virtual WAN-Angebot verwendeten virtuellen Computer verwenden jedoch diese Funktionen. Weitere Informationen zu diesen allgemeinen Funktionen finden Sie unter den angegebenen Links. 

- [Microsoft Defender Advanced Threat Protection](/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection)

- [Durchführen des Onboardings von Windows-Servern für den Microsoft Defender ATP-Dienst](/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints)

- [Durchführen des Onboardings für Windows-fremde Server](/windows/security/threat-protection/microsoft-defender-atp/configure-endpoints-non-windows)

**Azure Security Center-Überwachung** : Ja

**Verantwortlichkeit** : Shared

## <a name="governance-and-strategy"></a>Governance und Strategie

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Governance und Strategie](/azure/security/benchmarks/security-controls-v2-governance-strategy).*

### <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>GS-1: Definieren der Strategie für Ressourcenverwaltung und Datenschutz 

**Leitfaden** : Stellen Sie sicher, dass Sie eine klare Strategie für die kontinuierliche Überwachung und den Schutz von Systemen und Daten dokumentieren und kommunizieren. Priorisieren Sie die Ermittlung, die Bewertung, den Schutz und die Überwachung unternehmenskritischer Daten und Systeme. 

Diese Strategie sollte dokumentierte Anleitungen, Richtlinien und Standards für die folgenden Elemente umfassen: 

- Datenklassifizierungsstandard in Übereinstimmung mit den Geschäftsrisiken
- Sicherheitsorganisationseinblick in Risiken und Ressourcenbestand 
- Genehmigung durch die Sicherheitsorganisation für die Nutzung der Azure-Dienste 
- Sicherheit von Ressourcen durch ihren gesamten Lebenszyklus
- Erforderliche Zugriffssteuerungsstrategie in Übereinstimmung mit der Organisationsdatenklassifizierung
- Verwendung von nativen Azure- und Drittanbieter-Funktionen für den Datenschutz
- Datenverschlüsselungsanforderungen für Anwendungsfälle während der Übertragung und für ruhende Anwendungsfälle
- Geeignete Verschlüsselungsstandards

Lesen Sie zusätzliche Informationen, die unter den angegebenen Links verfügbar sind.

- [Empfehlung für eine Azure-Sicherheitsarchitektur: Speicher, Daten, Verschlüsselung](https://docs.microsoft.com/azure/architecture/framework/security/storage-data-encryption?toc=/security/compass/toc.json&amp;bc=/security/compass/breadcrumb/toc.json)

- [Grundlegende Azure-Sicherheitsinformationen: Sicherheit, Verschlüsselung und Speicherung von Azure-Daten](../security/fundamentals/encryption-overview.md)

- [Cloud Adoption Framework: Bewährte Methoden für Datensicherheit und Datenverschlüsselung in Azure](https://docs.microsoft.com/azure/security/fundamentals/data-encryption-best-practices?toc=/azure/cloud-adoption-framework/toc.json&amp;bc=/azure/cloud-adoption-framework/_bread/toc.json)

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Kunde

### <a name="gs-2-define-enterprise-segmentation-strategy"></a>GS-2: Definieren der Strategie für Unternehmenssegmentierung 

**Leitfaden** : Erstellen Sie eine unternehmensweite Strategie zum Segmentieren des Zugriffs auf Ressourcen durch eine Kombination aus Identität, Netzwerk, Anwendung, Abonnement, Verwaltungsgruppe und anderen Steuerelementen. Wägen Sie die Notwendigkeit einer Sicherheitstrennung sorgfältig gegen die Notwendigkeit ab, den täglichen Betrieb der Systeme zu ermöglichen, die miteinander kommunizieren und auf Daten zugreifen müssen.

Stellen Sie sicher, dass die Segmentierungsstrategie einheitlich für alle Steuerelementtypen implementiert wird, einschließlich Netzwerksicherheit, Identitäts- und Zugriffsmodelle, Berechtigungs- oder Zugriffsmodelle für Anwendungen sowie Steuerelemente für Benutzerprozesse.

- [Leitfaden für die Segmentierungsstrategie in Azure (Video)](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Leitfaden für die Segmentierungsstrategie in Azure (Dokument)](/security/compass/governance#enterprise-segmentation-strategy)

- [Ausrichten der Netzwerksegmentierung an der Segmentierungsstrategie des Unternehmens](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Kunde

### <a name="gs-3-define-security-posture-management-strategy"></a>GS-3: Definieren der Strategie für die Sicherheitsstatusverwaltung

**Leitfaden** : Führen Sie kontinuierliche Messungen durch, und Mindern Sie die Risiken für Ihre individuellen Ressourcen und die Umgebung, in der diese gehostet werden. Priorisieren Sie Ressourcen mit hohem Wert und hoch exponierte Angriffsflächen, wie z. B. veröffentlichte Anwendungen, Eingangs- und Ausgangspunkte für Netzwerke, Benutzer- und Administratorendpunkte usw.

- [Azure-Sicherheitsvergleichstest: Verwaltung von Status und Sicherheitsrisiken](/azure/security/benchmarks/security-benchmark-v2-posture-vulnerability-management)

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Kunde

### <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>GS-4: Ausrichten von Organisationsrollen, Zuständigkeiten und Verantwortlichkeiten

**Leitfaden** : Stellen Sie sicher, dass Sie eine klare Strategie für Rollen und Zuständigkeiten in ihrer Sicherheitsorganisation dokumentieren und kommunizieren. Priorisieren Sie Ihre Bemühungen auf der Grundlage klarer Verantwortlichkeiten für Sicherheitsentscheidungen, Schulung aller Beteiligten für das Modell der gemeinsamen Verantwortung und Schulung von technischen Teams hinsichtlich der Technologie zum Sichern der Cloud.

- [Azure Security Best Practice 1 – People: Educate Teams on Cloud Security Journey](https://aka.ms/AzSec1) (Bewährte Methoden für die Sicherheit von Azure 1 – Personen: Schulen von Teams auf dem Weg zur Cloudsicherheit)

- [Azure Security Best Practice 2 – People: Educate Teams on Cloud Security Technology](https://aka.ms/AzSec2) (Bewährte Methoden für die Sicherheit von Azure 1 – Personen: Schulen der Teams in Cloudsicherheitstechnologie)

- [Azure Security Best Practice 3 – Process: Assign Accountability for Cloud Security Decisions](https://aka.ms/AzSec3) (Bewährte Methoden für die Sicherheit von Azure 1 – Prozess: Zuweisen von Verantwortlichkeit für Cloudsicherheitsentscheidungen)

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Kunde

### <a name="gs-5-define-network-security-strategy"></a>GS-5: Definieren einer Netzwerksicherheitsstrategie

**Leitfaden** : Richten Sie einen Ansatz für die Azure-Netzwerksicherheit im Rahmen der Gesamtstrategie Ihrer Organisation für die Sicherheitszugriffssteuerung ein. Diese Strategie sollte dokumentierte Anleitungen, Richtlinien und Standards für die folgenden Elemente umfassen: 

- Zentralisieren der Verantwortlichkeit für Netzwerkverwaltung und -sicherheit
- Segmentierungsmodell für virtuelle Netzwerke, das auf die Segmentierungsstrategie des Unternehmens abgestimmt ist
- Wiederherstellungsstrategie in verschiedenen Bedrohungs- und Angriffsszenarien
- Strategie für Internet-Randpunkte, -Eingangspunkte und -Ausgangspunkte
- Strategie für Konnektivität zwischen Hybrid Cloud und lokalen Systemen
- Aktuelle Netzwerksicherheitsartefakte (z. B. Netzwerkdiagramme, Referenznetzwerkarchitektur)

Lesen Sie zusätzliche Informationen, die unter den angegebenen Links verfügbar sind.

- [Azure Security Best Practice 11 – Architecture. Single unified security strategy](https://aka.ms/AzSec11) (Bewährte Methoden für die Sicherheit von Azure 11 – Architektur: Zentralisierte einheitliche Sicherheitsstrategie)

- [Azure-Sicherheitsvergleichstest: Netzwerksicherheit](/azure/security/benchmarks/security-benchmark-v2-network-security)

- [Die Netzwerksicherheit in Azure in der Übersicht](../security/fundamentals/network-overview.md)

- [Strategie für die Unternehmensnetzwerkarchitektur](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Kunde

### <a name="gs-6-define-identity-and-privileged-access-strategy"></a>GS-6: Definieren der Strategie für Identität und privilegierten Zugriff

**Leitfaden** : Richten Sie einen Ansatz für Azure-Identität und privilegierten Zugriff im Rahmen der Gesamtstrategie Ihrer Organisation für die Sicherheitszugriffssteuerung ein. Diese Strategie sollte dokumentierte Anleitungen, Richtlinien und Standards für die folgenden Elemente umfassen: 

- Zentralisiertes Identitäts- und Authentifizierungssystem mit Interkonnektivität zu anderen internen und externen Identitätssystemen
- Starke Authentifizierungsmethoden in verschiedenen Anwendungsfällen und Bedingungen
- Schutz stark privilegierter Benutzer
- Überwachung und Behandlung von Anomalien bei Benutzeraktivitäten  
- Benutzeridentität und Zugriffsüberprüfung und Abstimmungsprozess

Lesen Sie zusätzliche Informationen, die unter den angegebenen Links verfügbar sind.

- [Azure-Sicherheitsvergleichstest: Identitätsverwaltung](/azure/security/benchmarks/security-benchmark-v2-identity-management)

- [Azure-Sicherheitsvergleichstest: Privilegierter Zugriff](/azure/security/benchmarks/security-benchmark-v2-privileged-access)

- [Azure Security Best Practice 11 – Architecture. Single unified security strategy](https://aka.ms/AzSec11) (Bewährte Methoden für die Sicherheit von Azure 11 – Architektur: Zentralisierte einheitliche Sicherheitsstrategie)

- [Übersicht über die Sicherheit der Azure-Identitätsverwaltung](../security/fundamentals/identity-management-overview.md)

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Kunde

### <a name="gs-7-define-logging-and-threat-response-strategy"></a>GS-7: Definieren einer Strategie für Protokollierung und Reaktion auf Bedrohungen

**Leitfaden** : Richten Sie eine Strategie für Protokollierung und Reaktion auf Bedrohungen zur schnellen Erkennung und Behebung von Bedrohungen bei gleichzeitiger Erfüllung von Complianceanforderungen ein. Priorisieren Sie, indem Sie Analysten qualitativ hochwertige Warnungen und nahtlose Umgebungen bereitstellen, sodass sie sich auf Bedrohungen konzentrieren können und sich nicht um Integration und manuelle Schritte kümmern müssen. 

Diese Strategie sollte dokumentierte Anleitungen, Richtlinien und Standards für die folgenden Elemente umfassen: 

- Rolle und Verantwortlichkeiten der Organisation für Sicherheitsvorgänge (SecOps)
- Ein klar definierter Prozess zur Reaktion auf Vorfälle gemäß NIST oder einem anderen Branchenframework
- Erfassung und Aufbewahrung von Protokollen zur Unterstützung von Bedrohungserkennung, Reaktion auf Vorfälle und Complianceanforderungen
- Zentralisierte Sichtbarkeit von und Korrelationsinformationen zu Bedrohungen unter Verwendung von SIEM, nativen Azure-Funktionen und anderen Quellen. Kommunikations- und Benachrichtigungsplan mit Ihren Kunden, Lieferanten und öffentlichen Interessengruppen.
- Verwendung von nativen Azure-Plattformen und Plattformen von Drittanbietern für die Behandlung von Vorfällen, wie z. B. Protokollierung und Bedrohungserkennung, Forensik sowie Reduzierung und Bekämpfung von Angriffen
- Prozesse für den Umgang mit Vorfällen und Aktivitäten nach Vorfällen, wie z. B. Erkenntnisse und Beweissicherung

Lesen Sie zusätzliche Informationen, die unter den angegebenen Links verfügbar sind.
- [Azure-Sicherheitsvergleichstest: Protokollierung und Bedrohungserkennung](/azure/security/benchmarks/security-benchmark-v2-logging-threat-detection)

- [Azure-Sicherheitsvergleichstest: Reaktion auf Vorfälle](/azure/security/benchmarks/security-benchmark-v2-incident-response)

- [Azure Security Best Practice 4 – Process. Update Incident Response Processes for Cloud](https://aka.ms/AzSec4) (Bewährte Methoden für die Sicherheit von Azure 4 – Prozess: Aktualisieren der Prozesse zur Reaktion auf Vorfälle für die Cloud)

- [Azure Adoption Framework, Entscheidungshilfe für Protokollierung und Berichterstattung](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

**Azure Security Center-Überwachung** : Zurzeit nicht verfügbar

**Verantwortlichkeit** : Kunde

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die [Übersicht über Version 2 des Azure-Sicherheitsvergleichstests](/azure/security/benchmarks/overview) an.
- Erfahren Sie mehr über [Azure-Sicherheitsbaselines](/azure/security/benchmarks/security-baselines-overview).
