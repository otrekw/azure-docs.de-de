---
title: Azure-Sicherheitsbaseline für Azure Stack Edge
description: Die Azure Stack Edge-Sicherheitsbaseline enthält Anweisungen zur Vorgehensweise und Ressourcen für die Implementierung der Sicherheitsempfehlungen im Azure-Sicherheitsvergleichstest.
author: msmbaldwin
ms.service: databox
ms.topic: conceptual
ms.date: 12/18/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 1d7cd07ee8ae8b35d51fbdd25b34602c2e799c75
ms.sourcegitcommit: a055089dd6195fde2555b27a84ae052b668a18c7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98787263"
---
# <a name="azure-security-baseline-for-azure-stack-edge"></a>Azure-Sicherheitsbaseline für Azure Stack Edge

Diese Sicherheitsbaseline wendet Empfehlungen des [Azure-Sicherheitsvergleichstests Version 2.0](../security/benchmarks/overview.md) auf Microsoft Azure Stack Edge an. Der Azure-Sicherheitsvergleichstest enthält Empfehlungen zum Schutz Ihrer Cloudlösungen in Azure. Der Inhalt wird nach den **Sicherheitskontrollen** gruppiert, die durch den Azure-Sicherheitsvergleichstest und die entsprechenden für Azure Stack Edge geltenden Empfehlungen definiert sind. Nicht auf Azure Stack Edge anwendbare **Kontrollen** wurden ausgeschlossen.

Die vollständige Zuordnung von Azure Stack Edge zum Azure-Sicherheitsvergleichstest finden Sie in der [vollständigen Zuordnungsdatei der Azure Stack Edge-Sicherheitsbaseline](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Netzwerksicherheit

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Netzwerksicherheit](../security/benchmarks/security-controls-v2-network-security.md).*

### <a name="ns-1-implement-security-for-internal-traffic"></a>NS-1: Implementieren der Sicherheit für internen Datenverkehr

**Leitfaden**: Kunden stellen ein von Microsoft bereitgestelltes, physisches Azure Stack Edge-Gerät in Ihrem privaten Netzwerk für den internen Zugriff bereit und haben die Möglichkeit, es stärker zu schützen. Beispielsweise kann auf das Azure Stack Edge-Gerät über das interne Netzwerk des Kunden zugegriffen werden, und es erfordert eine vom Kunden konfigurierte IP-Adresse. Außerdem wählt der Kunde ein Kennwort für den Zugriff auf die Benutzeroberfläche des Geräts aus. 

Der interne Datenverkehr wird folgendermaßen weiter geschützt:

- TLS Version 1.2 (Transport Layer Security) ist für die Verwaltung des Azure Stack Edge-Geräts über das Azure-Portal oder das SDK erforderlich.

- Jeder Clientzugriff auf das Gerät erfolgt über die lokale Webbenutzeroberfläche, wobei als Standardsicherheitsprotokoll TLS 1.2 verwendet wird.

- Dem im Azure-Abonnement des Kunden erstellten Azure Stack Edge-Dienst kann nur ein autorisiertes Azure Stack Edge Pro-Gerät hinzugefügt werden.

Zusätzliche Informationen finden Sie unter den aufgeführten Links.
 
- [Konfigurieren von TLS 1.2 auf Windows-Clients mit Zugriff auf ein Azure Stack Edge Pro-GPU-Gerät](azure-stack-edge-j-series-configure-tls-settings.md)

- [Schnellstart: Erste Schritte mit Azure Stack Edge Pro mit GPU](azure-stack-edge-gpu-quickstart.md)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="ns-2-connect-private-networks-together"></a>NS-2: Verbinden privater Netzwerke

**Leitfaden**: Kunden können ein Point-to-Site-VPN (virtuelles privates Netzwerk) auswählen, um ein Azure Stack Edge-Gerät aus dem lokalen privaten Netzwerk mit dem Azure-Netzwerk zu verbinden. Das VPN bietet eine zweite Verschlüsselungsebene für Daten, während diese über TLS vom Kundengerät zu Azure übertragen werden. 

Kunden können über das Azure-Portal oder Azure PowerShell ein VPN auf ihrem Azure Stack Edge-Gerät konfigurieren.

- [Konfigurieren eines Azure-VPN über ein Azure PowerShell-Skript für Azure Stack Edge Pro R und Azure Stack Edge Mini R](azure-stack-edge-mini-r-configure-vpn-powershell.md)

- [Konfigurieren von TLS 1.2 auf Windows-Clients mit Zugriff auf ein Azure Stack Edge Pro-GPU-Gerät](azure-stack-edge-j-series-configure-tls-settings.md)

- [Tutorial: Konfigurieren von Zertifikaten für Ihr Azure Stack Edge Pro R-Gerät](azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption.md)

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="ns-3-establish-private-network-access-to-azure-services"></a>NS-3: Einrichten des Zugriffs über das private Netzwerk auf Azure-Dienste

**Leitfaden**: Kunden können ein Point-to-Site-VPN (virtuelles privates Netzwerk) auswählen, um ein Azure Stack Edge-Gerät aus dem lokalen privaten Netzwerk mit dem Azure-Netzwerk zu verbinden. Das VPN bietet eine zweite Verschlüsselungsebene für Daten, während diese über TLS vom Kundengerät zu Azure übertragen werden. 

- [Konfigurieren eines Azure-VPN über ein Azure PowerShell-Skript für Azure Stack Edge Pro R und Azure Stack Edge Mini R](azure-stack-edge-mini-r-configure-vpn-powershell.md)

- [Konfigurieren von TLS 1.2 auf Windows-Clients mit Zugriff auf ein Azure Stack Edge Pro-GPU-Gerät](azure-stack-edge-j-series-configure-tls-settings.md)

- [Tutorial: Konfigurieren von Zertifikaten für Ihr Azure Stack Edge Pro R-Gerät](azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption.md)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="ns-4-protect-applications-and-services-from-external-network-attacks"></a>NS-4: Schützen von Anwendungen und Diensten vor externen Netzwerkangriffen.

**Leitfaden**: Das Azure Stack Edge-Gerät nutzt Standardfeatures von Windows Server für die Netzwerksicherheit, die vom Kunden nicht konfiguriert werden können.

Kunden haben die Möglichkeit, ihr privates Netzwerk, das mit dem Azure Stack Edge-Gerät verbunden ist, mithilfe eines virtuellen Netzwerkgeräts wie z. B. einer Firewall mit erweitertem DDoS-Schutz (Distributed Denial-of-Service) vor externen Angriffen zu schützen.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Shared

### <a name="ns-5-deploy-intrusion-detectionintrusion-prevention-systems-idsips"></a>NS-5: Bereitstellen von Angriffserkennungs-/Eindringschutzsystemen (Intrusion Detection/Intrusion Prevention Systems, IDS/IPS)

**Leitfaden**: Die vom Azure Stack Edge-Gerät verwendeten Endpunkte werden alle von Microsoft verwaltet. Die Kunden sind für alle zusätzlichen Steuerfunktionen verantwortlich, die sie für ihre lokalen Systeme bereitstellen möchten.

Das Azure Stack Edge-Gerät nutzt eigene Angriffserkennungsfeatures, um den Dienst zu schützen. 

- [Grundlegendes zur Azure Stack Edge-Sicherheit](azure-stack-edge-security.md)

- [Portinformationen für Azure Stack Edge](azure-stack-edge-gpu-system-requirements.md)

- [Abrufen von Protokollen zur Angriffserkennung bei Hardware und Software](azure-stack-edge-gpu-troubleshoot.md#gather-advanced-security-logs)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Shared

## <a name="identity-management"></a>Identitätsverwaltung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Identitätsverwaltung](../security/benchmarks/security-controls-v2-identity-management.md).*

### <a name="im-2-manage-application-identities-securely-and-automatically"></a>IM-2: Sicheres und automatisches Verwalten von Anwendungsidentitäten

**Leitfaden**: Alle Azure Stack Edge-Geräte verfügen in Azure Active Directory (Azure AD) automatisch über eine vom System zugewiesene verwaltete Identität. Derzeit wird die verwaltete Identität für die Cloudverwaltung von VMs verwendet, die auf Azure Stack Edge gehostet werden.

Azure Stack Edge-Geräte sind nach dem Start für den lokalen Zugriff gesperrt. Für das lokale Geräteadministratorkonto müssen Sie über die lokale Webbenutzeroberfläche oder die PowerShell-Schnittstelle eine Verbindung mit Ihrem Gerät herstellen und ein sicheres Kennwort festlegen. Speichern und verwalten Sie die Anmeldeinformationen für den Geräteadministrator an einem sicheren Ort, z. B. in Azure Key Vault, und rotieren Sie das Administratorkennwort gemäß den Vorgaben Ihrer Organisation.

- [Schützen von Azure Stack Edge-Geräten mit einem Kennwort](azure-stack-edge-security.md#protect-the-device-via-password)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Shared

### <a name="im-3-use-azure-ad-single-sign-on-sso-for-application-access"></a>IM-3: Verwenden von Azure AD Single Sign-on (SSO) für den Anwendungszugriff

**Leitfaden**: Einmaliges Anmelden wird für Azure Stack Edge-Endpunktgeräte nicht unterstützt. Sie können jedoch das auf Azure Active Directory (Azure AD) basierende standardmäßige einmalige Anmelden aktivieren, um den Zugriff auf Ihre Azure-Cloudressourcen zu schützen.

- [Grundlegendes zu Anwendungs-SSO mit Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>IM-4: Verwenden stärkerer Authentifizierungssteuerungen für den gesamten Azure Active Directory-basierten Zugriff

**Leitfaden**: Die mehrstufige Authentifizierung stellt eine starke Authentifizierungsmöglichkeit dar, ist für Benutzer des Azure Stack Edge-Diensts aber optional. Sie kann in unterstützten Szenarien genutzt werden, z. B. der Verwaltung von Azure Stack Edge-Geräten am Edge über das Azure-Portal. Beachten Sie, dass für den lokalen Zugriff auf die Azure Stack Edge-Geräte keine mehrstufige Authentifizierung unterstützt wird.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="im-5-monitor-and-alert-on-account-anomalies"></a>IM-5: Überwachen und Warnen bei Kontoanomalien

**Leitfaden**: Aktivieren Sie Azure Monitor, um Geräte- oder Containerprotokolle zu Ihrem Azure Stack Edge-Dienst zu erfassen. Überwachen Sie Container, z. B. solche, in denen mehrere Computeanwendungen im Kubernetes-Cluster auf Ihrem Gerät ausgeführt werden.

Darüber hinaus können Sie über die lokale Webbenutzeroberfläche des Azure Stack Edge-Geräts ein Protokollpaket abrufen, das Überwachungsprotokolle enthält. Beachten Sie, dass das Protokollpaket nicht über das Azure-Portal verfügbar ist.
 
- [Aktivieren von Azure Monitor auf Ihrem Azure Stack Edge Pro-Gerät](azure-stack-edge-gpu-enable-azure-monitor.md) 

- [Erfassen eines Protokollpakets](azure-stack-edge-gpu-troubleshoot.md#collect-support-package)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="im-6-restrict-azure-resource-access-based-on-conditions"></a>IM-6: Einschränken des Zugriffs auf Azure-Ressourcen basierend auf Bedingungen

**Leitfaden**: Der bedingte Zugriff von Azure Active Directory (Azure AD) ist eine optionale Funktion für die Authentifizierung beim Azure Stack Edge-Dienst. Der Azure Stack Edge-Dienst ist eine Ressource im Azure-Portal, mit der Sie ein Azure Stack Edge Pro-Gerät von verschiedenen geografischen Standorten aus verwalten können. 

- [Was ist bedingter Zugriff?](../active-directory/conditional-access/overview.md)

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="im-7-eliminate-unintended-credential-exposure"></a>IM-7: Beheben der unbeabsichtigten Offenlegung von Anmeldeinformationen

**Leitfaden**: Befolgen Sie die bewährten Methoden zum Schutz von Anmeldeinformationen, z. B.:

- Aktivierungsschlüssel zum Aktivieren des Geräts mit der Azure Stack Edge-Ressource in Azure
- Anmeldeinformationen für den Zugriff auf ein Azure Stack Edge-Gerät
- Schlüsseldateien für eine einfachere Wiederherstellung des Azure Stack Edge-Geräts
- Kanalverschlüsselungsschlüssel

Rotieren und synchronisieren Sie Ihre Speicherkontoschlüssel regelmäßig, um zu vermeiden, dass nicht autorisierte Benutzer auf Ihr Speicherkonto zugreifen können.

- [Azure Stack Edge Pro: Sicherheit und Schutz von Daten](azure-stack-edge-security.md)

- [Synchronisieren von Speicherschlüsseln](azure-stack-edge-manage-shares.md#sync-storage-keys)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="privileged-access"></a>Privilegierter Zugriff

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Privilegierter Zugriff](../security/benchmarks/security-controls-v2-privileged-access.md).*

### <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2: Einschränken des administrativen Zugriffs auf unternehmenskritische Systeme

**Leitfaden**: Die Azure Stack Edge-Lösung bietet mehrere Komponenten mit strikter Zugriffssteuerung, um den Zugriff auf unternehmenskritische Geräte einzuschränken. Ihre Organisation benötigt einen Konzernvertrag (Enterprise Agreement, EA) oder ein CSP- (Cloud Solution Provider) oder ein Microsoft Azure Sponsorship-Abonnement, um das Gerät zu konfigurieren und zu verwalten: 

Da Azure Stack Edge als Verwaltungsdienst in Azure gehostet wird, ist er durch die Azure-Sicherheitsfeatures geschützt. Sie können den Verschlüsselungsschlüssel für Ihre Ressource in den Geräteeigenschaften für alle SDK-Verwaltungsvorgänge abrufen. Anzeigen können Sie den Verschlüsselungsschlüssel jedoch nur, wenn Sie über die entsprechenden Berechtigungen für die Resource Graph-API verfügen.

Das Azure Stack Edge Pro-Gerät ist ein lokales Gerät, das Sie bei der Transformation Ihrer Daten unterstützt. Hierzu werden die Daten lokal verarbeitet und dann an Azure gesendet. Für Ihr Gerät gilt Folgendes:

- Es benötigt einen Aktivierungsschlüssel für den Zugriff auf den Azure Stack Edge-Dienst.
- Es ist jederzeit durch ein Gerätekennwort geschützt.
- Sicherer Start ist aktiviert.

- Es ist ein gesperrtes Gerät. Der Baseboard-Verwaltungscontroller (Baseboard Management Controller, BMC) und das BIOS des Geräts sind kennwortgeschützt. Das BIOS ist durch eingeschränkten Benutzerzugriff geschützt.
- Windows Defender Device Guard wird darauf ausgeführt. Device Guard stellt sicher, dass nur vertrauenswürdige Anwendungen ausgeführt werden können, die Sie in Ihren Codeintegritätsrichtlinien definieren.

Befolgen Sie die bewährten Methoden, um alle Anmeldeinformationen und Geheimnisse für den Zugriff auf Azure Stack Edge zu schützen. 

- [Bewährte Methoden für Kennwörter](azure-stack-edge-security.md#protect-the-device-via-password)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="pa-3-review-and-reconcile-user-access-regularly"></a>PA-3: Regelmäßiges Überprüfen und Abstimmen des Benutzerzugriffs

**Leitfaden**: Azure Stack Edge weist einen Benutzer mit dem Namen „EdgeUser“ auf, über den das Gerät konfiguriert werden kann. Außerdem gibt es den Azure Resource Manager-Benutzer „EdgeArmUser“ für die lokalen Azure Resource Manager-Features auf dem Gerät. 

Nutzen Sie für den Schutz von Folgendem die bewährten Methoden:

- Anmeldeinformationen für den Zugriff auf das lokale Gerät

- Anmeldeinformationen von SMB-Freigaben

- Zugriff auf Clientsysteme, die für die Verwendung von NFS-Freigaben konfiguriert wurden

- Schlüssel für den Zugriff auf lokale Speicherkonten bei Verwendung der Blob-REST-API

Weitere Informationen finden Sie unter dem aufgeführten Link.

- [Informationen zur Sicherheit für Ihre Azure Stack Edge-Geräte](azure-stack-edge-security.md)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="pa-6-use-privileged-access-workstations"></a>PA-6: Verwenden von Privileged Access Workstations

**Leitfaden**: Geschützte und isolierte Arbeitsstationen sind von entscheidender Bedeutung für die Sicherheit sensibler Rollen wie Administratoren, Entwickler und Operatoren kritischer Dienste. Verwenden Sie stark geschützte Benutzerarbeitsstationen mit oder ohne Azure Bastion für Verwaltungsaufgaben. Verwenden Sie Azure Active Directory (Azure AD), Microsoft Defender Advanced Threat Protection (ATP) und Microsoft Intune, um eine sichere und verwaltete Benutzerarbeitsstation für administrative Aufgaben einzurichten. 

Die gesicherten Workstations können zentral verwaltet werden, um eine gesicherte Konfiguration einschließlich starker Authentifizierung, Software- und Hardwarebaselines, eingeschränktem logischen und Netzwerkzugang durchzusetzen.

- [Informationen zu sicheren, von Azure verwalteten Arbeitsstationen](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/) 

- [Bereitstellen einer sicheren, von Azure verwalteten Arbeitsstation](/security/compass/privileged-access-deployment)

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="data-protection"></a>Datenschutz

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Schutz von Daten](../security/benchmarks/security-controls-v2-data-protection.md).*

### <a name="dp-2-protect-sensitive-data"></a>DP-2: Schützen von vertraulichen Daten

**Leitfaden**: Azure Stack Edge behandelt alle Daten während der Interaktion als vertraulich und gewährt nur autorisierten Benutzern den Zugriff darauf. Befolgen Sie die bewährten Methoden, um die Anmeldeinformationen für den Zugriff auf den Azure Stack Edge-Dienst zu schützen.

- [Azure Stack Edge Pro: Sicherheit und Schutz von Daten](azure-stack-edge-security.md#protect-the-device-via-password)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Shared

### <a name="dp-4-encrypt-sensitive-information-in-transit"></a>DP-4: Verschlüsseln vertraulicher Informationen während der Übertragung

**Leitfaden**: Azure Stack Edge verwendet sichere Kanäle für Daten während der Übertragung. Dies sind:

- Standardmäßiges TLS 1.2 wird für Daten verwendet, die zwischen dem Gerät und der Azure-Cloud übertragen werden. Es erfolgt kein Fallback auf TLS 1.1 oder auf ältere Versionen. Agent-Kommunikation wird blockiert, wenn TLS 1.2 nicht unterstützt wird. TLS 1.2 ist auch für die Verwaltung über das Azure-Portal und ein SDK (Software Development Kit) erforderlich.

- Wenn Clients über die lokale Webbenutzeroberfläche eines Browsers auf Ihr Gerät zugreifen, wird standardmäßiges TLS 1.2 als Standardsicherheitsprotokoll verwendet.

Es empfiehlt sich, Ihren Browser für die Verwendung von TLS 1.2 zu konfigurieren. Verwenden Sie SMB 3.0 mit Verschlüsselung, um Daten zu schützen, wenn Sie diese von Ihren Datenservern kopieren.

- [Bewährte Methoden zum Schützen von Daten während der Übertragung](azure-stack-edge-security.md#protect-data-in-flight)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Shared

## <a name="asset-management"></a>Ressourcenverwaltung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Ressourcenverwaltung](../security/benchmarks/security-controls-v2-asset-management.md).*

### <a name="am-1-ensure-security-team-has-visibility-into-risks-for-assets"></a>AM-1: Sicherstellen, dass das Sicherheitsteam Risiken für Ressourcen einsehen kann

**Leitfaden**: Stellen Sie sicher, dass Sicherheitsteams die Berechtigungen „Sicherheitsleseberechtigter“ in Ihrem Azure-Mandanten und Ihren Abonnements erhalten, damit sie mit Azure Security Center Sicherheitsrisiken überwachen können. 

Abhängig davon, wie die Verantwortungsbereiche des Sicherheitsteams strukturiert sind, kann die Überwachung auf Sicherheitsrisiken unter die Verantwortung eines zentralen Sicherheitsteams oder eines lokalen Teams fallen. Aus diesem Grund müssen Sicherheitserkenntnisse und -risiken immer innerhalb einer Organisation zentral aggregiert werden. 

Die Berechtigungen der Rolle „Sicherheitsleseberechtigter“ können weit gefasst auf einen gesamten Mandanten (Stammverwaltungsgruppe) angewandt oder auf Verwaltungsgruppen oder bestimmte Abonnements beschränkt werden. 

Beachten Sie, dass möglicherweise zusätzliche Berechtigungen erforderlich sind, um Einblick in Workloads und Dienste zu erhalten. 

- [Übersicht über die Rolle „Sicherheitsleseberechtigter“](../role-based-access-control/built-in-roles.md#security-reader)

- [Übersicht über Azure-Verwaltungsgruppen](../governance/management-groups/overview.md)

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="am-5-limit-users-ability-to-interact-with-azure-resource-manager"></a>AM-5: Einschränken der Möglichkeiten von Benutzern zur Interaktion mit Azure Resource Manager

**Leitfaden**: Nur autorisierte Benutzer (z. B. „EdgeArmUser“) können über die lokale Azure Resource Manager-Instanz auf die Azure Stack Edge-Geräte-APIs zugreifen. Kennwörter für Benutzerkonten können nur über das Azure-Portal verwaltet werden. 

- [Festlegen des Azure Resource Manager-Kennworts](azure-stack-edge-j-series-set-azure-resource-manager-password.md)

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="am-6-use-only-approved-applications-in-compute-resources"></a>AM-6: Ausschließliche Verwendung genehmigter Anwendungen in Computeressourcen

**Leitfaden**: Sie können eigene Anwendungen auf allen lokal erstellten VMs ausführen. Verwenden Sie PowerShell-Skripts zum Erstellen lokaler Compute-VMs auf dem Stack Edge-Gerät. Es wird dringend empfohlen, auf den lokalen VMs nur vertrauenswürdige Anwendungen zu aktivieren. 

- [Steuern der PowerShell-Skriptausführung in einer Windows-Umgebung](/powershell/module/microsoft.powershell.security/set-executionpolicy?preserve-view=true&amp;viewFallbackFrom=powershell-6&view=powershell-7.1)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="logging-and-threat-detection"></a>Protokollierung und Bedrohungserkennung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Protokollierung und Bedrohungserkennung](../security/benchmarks/security-controls-v2-logging-threat-detection.md).*

### <a name="lt-1-enable-threat-detection-for-azure-resources"></a>LT-1: Aktivieren der Bedrohungserkennung für Azure-Ressourcen

**Leitfaden**: Azure Stack Edge bietet keine Funktionen zur Bedrohungserkennung. Kunden können jedoch Überwachungsprotokolle in einem Protokollpaket erfassen, um die Bedrohungserkennung und -analyse offline durchzuführen. 

- [Erfassen eines Protokollpakets für ein Azure Stack Edge-Gerät](azure-stack-edge-gpu-troubleshoot.md#collect-support-package)

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="lt-3-enable-logging-for-azure-network-activities"></a>LT-3: Aktivieren der Protokollierung für Azure-Netzwerkaktivitäten

**Leitfaden**: In Azure Stack Edge werden mit dem herunterladbaren Protokollpaket auch Netzwerküberwachungsprotokolle aktiviert. Über eine Analyse dieser Protokolle können Sie eine Überwachung nahezu in Echtzeit für Ihre Azure Stack Edge-Geräte implementieren.

- [Erfassen eines Protokollpakets für ein Azure Stack Edge-Gerät](azure-stack-edge-gpu-troubleshoot.md#collect-support-package)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4: Aktivieren der Protokollierung für Azure-Ressourcen

**Leitfaden**: Die Echtzeitüberwachung mit Protokollen wird derzeit nicht für Azure Stack Edge unterstützt. Es gibt eine Option zum Erfassen eines Protokollpakets, die es Ihnen ermöglicht, die verschiedenen darin enthaltenen Protokolle zu analysieren. Dies umfasst z. B. Protokolle für Firewall-, Software-, Hardware- und Systemereignisse für Ihr Azure Stack Edge Pro-Gerät. Beachten Sie, dass die Protokolle zu softwarebezogenen Angriffen oder die standardmäßigen Firewallprotokolle für ein- und ausgehenden Datenverkehr gesammelt werden.

- [Erfassen eines Protokollpakets für ein Azure Stack Edge-Gerät](azure-stack-edge-gpu-troubleshoot.md#collect-support-package)

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5: Zentralisieren der Verwaltung und Analyse von Sicherheitsprotokollen

**Leitfaden**: Die Echtzeitüberwachung mit Protokollen wird derzeit nicht für Azure Stack Edge unterstützt. Sie können jedoch ein Protokollpaket erfassen, mit dem Sie die verschiedenen darin enthaltenen Protokolle analysieren können.  Das Protokollpaket wird komprimiert und in den gewünschten Pfad heruntergeladen. Sie können das Paket entzippen und sich die enthaltenen Systemprotokolldateien ansehen. Sie können diese Protokolle auch zur weiteren Analyse an ein SIEM-Tool (Security Information & Event Management) oder einen anderen zentralen Speicherort senden.

- [Erfassen eines Protokollpakets für ein Azure Stack Edge-Gerät](azure-stack-edge-gpu-troubleshoot.md#collect-support-package)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="lt-6-configure-log-storage-retention"></a>LT-6: Konfigurieren der Aufbewahrungsdauer von Protokollen im Speicher

**Leitfaden**: Der Aufbewahrungszeitraum für Protokolle kann auf dem Azure Stack Edge-Gerät nicht geändert werden. Ältere Protokolle werden bei Bedarf endgültig gelöscht. Sie können in regelmäßigen Abständen Protokollpakete für alle Anforderungen vom Gerät erfassen, um diese Protokolle für einen längeren Zeitraum aufzubewahren. 

- [Erfassen eines Protokollpakets für ein Azure Stack Edge-Gerät](azure-stack-edge-gpu-troubleshoot.md#collect-support-package)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="lt-7-use-approved-time-synchronization-sources"></a>LT-7: Verwenden von genehmigten Zeitsynchronisierungsquellen

**Leitfaden**: Azure Stack Edge verwendet time.windows.com, einen Netzwerk-Zeitanbieterserver von Microsoft.  Außerdem ermöglicht Azure Stack Edge Kunden, einen beliebigen Zeitprotokollserver für das Netzwerk zu konfigurieren.

- [Konfigurieren der Zeiteinstellungen des Azure Stack Edge-Geräts](azure-stack-edge-gpu-deploy-set-up-device-update-time.md#configure-time)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="incident-response"></a>Reaktion auf Vorfälle

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Reaktion auf Vorfälle](../security/benchmarks/security-controls-v2-incident-response.md).*

### <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>IR-1: Vorbereitung – Aktualisieren des Prozesses zur Reaktion auf Vorfälle für Azure

**Leitfaden**: Stellen Sie für den Plan zur Reaktion auf Vorfälle in Ihrer Organisation Folgendes sicher: 

- Er umfasst Prozesse zum Reagieren auf Sicherheitsvorfälle.

- Die Prozesse wurden an die Azure-Cloud angepasst.
 
- Die Prozesse werden regelmäßig getestet, damit sie im Ernstfall einsatzbereit sind.

Es wird empfohlen, die Sicherheit für die gesamte Unternehmensumgebung mithilfe standardisierter Prozesse zur Reaktion auf Vorfälle zu implementieren.

- [Implement security across the enterprise environment](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud) (Implementieren von Sicherheit in der gesamten Unternehmensumgebung)

- [Referenzleitfaden für die Reaktion auf Vorfälle](/microsoft-365/downloads/IR-Reference-Guide.pdf)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="ir-2-preparation--setup-incident-notification"></a>IR-2: Vorbereitung – Einrichten von Ereignisbenachrichtigungen

**Leitfaden**: Richten Sie Kontaktinformationen für Sicherheitsvorfälle im Azure Security Center ein. Microsoft wendet sich unter den angegebenen Kontaktdaten an Sie, wenn das Microsoft Security Response Center (MSRC) feststellt, dass Personen unrechtmäßig oder unbefugt auf Ihre Daten zugegriffen haben. Sie haben auch die Möglichkeit, die Warnungen und Benachrichtigungen bei Vorfällen in verschiedenen Azure-Diensten entsprechend Ihrer Anforderungen bezüglich der Reaktion auf Vorfälle anzupassen. 

- [Festlegen der Kontaktinformationen in Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Azure Security Center-Überwachung**: Ja

**Verantwortlichkeit**: Kunde

### <a name="ir-3-detection-and-analysis--create-incidents-based-on-high-quality-alerts"></a>IR-3: Erkennung und Analyse – Erstellen von Vorfällen basierend auf Warnungen mit hoher Qualität 

**Leitfaden**: Stellen Sie sicher, dass Sie über einen Prozess zum Erstellen hochwertiger Warnungen und zum Messen ihrer Qualität verfügen. Auf diese Weise können Sie Lehren aus vergangenen Vorfällen ziehen und Warnungen für Analysten priorisieren, damit diese keine Zeit mit falsch positiven Warnungen verschwenden. Qualitativ hochwertige Warnungen können anhand von Erfahrungen aus früheren Vorfällen, validierten Communityquellen und Tools zur Generierung und Bereinigung von Warnmeldungen durch Protokollierung und Korrelation verschiedener Warnungsquellen erstellt werden. 

Azure Security Center bietet qualitativ hochwertige Warnungen für viele Azure-Ressourcen. Sie können den Security Center-Datenconnector verwenden, um die Warnungen an Azure Sentinel zu streamen. Mit Azure Sentinel können Sie erweiterte Warnungsregeln erstellen, um Vorfälle automatisch für eine Untersuchung zu generieren. 

Exportieren Sie Ihre Security Center-Warnungen und -Empfehlungen über das Exportfeature in Azure Sentinel, um Risiken für Azure-Ressourcen zu ermitteln. Sie sollten auch einen Prozess zum automatischen Exportieren von Warnungen und Empfehlungen einrichten, um die Sicherheit kontinuierlich zu überwachen.

- [Konfigurieren des Exports](../security-center/continuous-export.md)

- [Streamen von Warnungen in Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="ir-4-detection-and-analysis--investigate-an-incident"></a>IR-4: Erkennung und Analyse – Untersuchen eines Vorfalls

**Leitfaden**: Stellen Sie sicher, dass Analysten bei der Untersuchung potenzieller Vorfälle unterschiedlichste Datenquellen abfragen und verwenden können, um eine vollständige Übersicht über die Aktivität zu erhalten. Es empfiehlt sich, verschiedene Protokolltypen zu erfassen, um die Aktivitäten eines potenziellen Angreifers über das gesamte Kill Chain-Spektrum hinweg nachzuverfolgen und Schwachpunkte zu vermeiden.  Stellen Sie auch sicher, dass Erkenntnisse und Erfahrungen als Referenzen festgehalten werden.  

Zu den zu untersuchenden Datenquellen gehören die zentralisierten Protokollierungsquellen, die bereits von den zum Umfang gehörige Diensten und ausgeführten Systemen gesammelt werden, aber auch andere:

- Netzwerkdaten: Verwenden Sie die Datenflussprotokolle von Netzwerksicherheitsgruppen sowie Azure Network Watcher und Azure Monitor, um Netzwerkflussprotokolle und andere Analyseinformationen zu erfassen. 

- Momentaufnahmen von laufenden Systemen: 

    - Verwenden Sie die Momentaufnahmenfunktion des virtuellen Azure-Computers, um eine Momentaufnahme der Festplatte des laufenden Systems zu erstellen. 

    - Nutzen Sie die native Speicherabbildfunktion des Betriebssystems, um eine Momentaufnahme des Arbeitsspeichers des laufenden Systems zu erstellen.

    - Verwenden Sie das Momentaufnahmefeature der Azure-Dienste oder Funktionen einer Drittanbietersoftware, um Momentaufnahmen der laufenden Systeme zu erstellen.

Azure Sentinel bietet umfangreiche Datenanalysen über praktisch jede Protokollquelle sowie ein Fallverwaltungsportal zum Verwalten des gesamten Lebenszyklus von Vorfällen. Intelligenceinformationen während einer Untersuchung können zu Verfolgungs- und Berichtszwecken mit einem Vorfall verknüpft werden. 

- [Momentaufnahme des Datenträgers eines Windows-Computers](../virtual-machines/windows/snapshot-copy-managed-disk.md)

- [Momentaufnahme des Datenträgers eines Linux-Computers](../virtual-machines/linux/snapshot-copy-managed-disk.md)

- [Microsoft Azure-Supportdiagnoseinformationen und Speicherabbilderfassung](https://azure.microsoft.com/support/legal/support-diagnostic-information-collection/) 

- Lesen Sie [Untersuchen von Incidents mit Azure Sentinel](../sentinel/tutorial-investigate-cases.md).

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="ir-5-detection-and-analysis--prioritize-incidents"></a>IR-5: Erkennung und Analyse – Priorisieren von Vorfällen

**Leitfaden**: Stellen Sie den Analysten Kontext bereit, auf welche Vorfälle sie sich zuerst konzentrieren sollten, je nach Schweregrad und Ressourcensensitivität. Azure Security Center weist jeder Warnung einen Schweregrad zu. Legen Sie damit fest, welche Warnungen zuerst untersucht werden sollen. Der Schweregrad basiert darauf, wie zuversichtlich Security Center in Bezug auf den Befund oder die Analyse ist, die zum Auslösen der Warnung verwendet wird, sowie auf dem Zuverlässigkeitsgrad, dass hinter der Aktivität, die zu der Warnung führte, eine böswillige Absicht stand.

Markieren Sie Ressourcen außerdem mithilfe von Tags, und erstellen Sie ein Benennungssystem, um Azure-Ressourcen eindeutig zu identifizieren und zu kategorisieren – insbesondere solche, von denen vertrauliche Daten verarbeitet werden.  Die Priorisierung der Behebung von Warnungen basierend auf der Wichtigkeit der Azure-Ressourcen und der Umgebung, in der der Vorfall aufgetreten ist, liegt in Ihrer Verantwortung.

- [Sicherheitswarnungen in Azure Security Center](../security-center/security-center-alerts-overview.md)

- [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../azure-resource-manager/management/tag-resources.md)

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="ir-6-containment-eradication-and-recovery--automate-the-incident-handling"></a>IR-6: Eindämmung, Ausmerzung und Wiederherstellung – Automatisieren der Behandlung von Vorfällen

**Leitfaden**: Automatisieren Sie manuelle, sich wiederholende Aufgaben, um die Antwortzeit zu beschleunigen und die Belastung der Analysten zu verringern. Manuelle Aufgaben dauern länger, verlangsamen jeden Vorfall und reduzieren die Anzahl der Vorfälle, die ein Analyst bewältigen kann. Manuelle Aufgaben erhöhen auch die Ermüdung der Analytiker. Dadurch erhöht sich das Risiko menschlicher Fehler, die zu Verzögerungen führen, und es verschlechtert sich die Fähigkeit der Analytiker, sich effektiv auf komplexe Aufgaben zu konzentrieren. Verwenden Sie die Workflowautomatisierungsfeatures in Azure Security Center und Azure Sentinel, um automatisch Aktionen auszulösen oder ein Playbook auszuführen, um auf eingehende Sicherheitswarnungen zu reagieren. Das Playbook führt Aktionen aus, wie das Versenden von Benachrichtigungen, das Deaktivieren von Konten und das Isolieren problematischer Netzwerke. 

- [Konfigurieren der Workflowautomatisierung in Security Center](../security-center/workflow-automation.md)

- [Einrichten automatisierter Reaktionen auf Bedrohungen in Azure Security Center](../security-center/tutorial-security-incident.md#triage-security-alerts)

- Machen Sie sich mit dem [Einrichten automatisierter Reaktionen auf Bedrohungen in Azure Sentinel](../sentinel/tutorial-respond-threats-playbook.md) vertraut.

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="posture-and-vulnerability-management"></a>Status- und Sicherheitsrisikoverwaltung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Status- und Sicherheitsrisikoverwaltung](../security/benchmarks/security-controls-v2-posture-vulnerability-management.md).*

### <a name="pv-3-establish-secure-configurations-for-compute-resources"></a>PV-3: Einrichten sicherer Konfigurationen für Computeressourcen

**Leitfaden**: Azure Stack Edge bietet Unterstützung für die Erstellung sicherer Konfigurationen für lokale VMs, die von den Kunden erstellt werden. Es wird dringend empfohlen, die von Microsoft bereitgestellten Richtlinien anzuwenden, um beim Erstellen lokaler VMs Sicherheitsbaselines einzurichten.

- [Herunterladen der im Security Compliance Toolkit enthaltenen Sicherheitsbaselines](/windows/security/threat-protection/windows-security-baselines)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="pv-4-sustain-secure-configurations-for-compute-resources"></a>PV-4: Aufrechterhalten sicherer Konfigurationen für Computeressourcen

**Leitfaden**: Azure Stack Edge bietet keine Unterstützung für die Beibehaltung sicherer Konfigurationen für lokale VMs, die von Kunden erstellt wurden. Es wird Kunden dringend empfohlen, die sicheren Konfigurationen ihrer Computeressourcen mithilfe der Security Compliance Toolkits (SCT) beizubehalten.

Für die von Azure Stack Edge verwalteten Hostbetriebssysteme und VMs werden die Sicherheitskonfigurationen beibehalten. 

- [Windows-Sicherheitsbaselines](/windows/security/threat-protection/windows-security-baselines#using-security-baselines-in-your-organization)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Shared

### <a name="pv-5-securely-store-custom-operating-system-and-container-images"></a>PV-5: Sicheres Speichern von benutzerdefinierten Betriebssystem- und Containerimages

**Leitfaden**: Von Azure Stack Edge verwaltete Hostbetriebssysteme und VMs werden sicher gespeichert. 

Kunden können auch eigene VMs und Containerimages nutzen, sind dann aber selbst für deren sichere Verwaltung verantwortlich.

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="pv-7-rapidly-and-automatically-remediate-software-vulnerabilities"></a>PV-7: Schnelles und automatisches Beheben von Softwaresicherheitsrisiken

**Leitfaden**: Azure Stack Edge stellt regelmäßig Patches und Updates bereit und benachrichtigt Kunden, wenn Updates verfügbar sind, die Sicherheitsrisiken beheben. Es liegt in der Verantwortung der Kunden, die eigenen Geräte und (selbst erstellten) VMs mit aktuellen Patches auf dem neuesten Stand zu halten.

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="pv-8-conduct-regular-attack-simulation"></a>PV-8: Regelmäßiges Durchführen einer Angriffssimulation

**Leitfaden**: Führen Sie nach Bedarf Penetrationstests oder Red Team-Aktivitäten für Ihre Azure-Ressourcen durch, und stellen Sie sicher, dass alle kritischen Sicherheitsergebnisse behandelt werden.
Befolgen Sie die Einsatzregeln für Penetrationstests für die Microsoft Cloud, um sicherzustellen, dass die Penetrationstests nicht gegen Microsoft-Richtlinien verstoßen. Nutzen Sie die Microsoft-Strategie und Durchführung von Red Team- und Livewebsite-Penetrationstests für von Microsoft verwaltete Cloudinfrastruktur, Dienste und Anwendungen.

- [Penetrationstests in Azure](../security/fundamentals/pen-testing.md)

- [Penetrationstests – Rules of Engagement](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Cloud Red Teaming](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Shared

## <a name="endpoint-security"></a>Endpunktsicherheit

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Endpunktsicherheit](../security/benchmarks/security-controls-v2-endpoint-security.md).*

### <a name="es-1-use-endpoint-detection-and-response-edr"></a>ES-1: Verwenden von Endpunkterkennung und -antwort (Endpoint Detection and Response, EDR)

**Leitfaden**: Azure Stack Edge unterstützt Endpunkterkennung und -reaktion (Endpoint Detection and Response, EDR) nicht direkt. Sie können jedoch ein Protokollpaket erfassen und Überwachungsprotokolle abrufen. Diese Protokolle können dann analysiert werden, um anomale Aktivitäten zu ermitteln. 

- [Erfassen eines Protokollpakets für ein Azure Stack Edge-Gerät](azure-stack-edge-gpu-troubleshoot.md#collect-support-package)

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="es-2-use-centrally-managed-modern-anti-malware-software"></a>ES-2: Verwenden von moderner, zentral verwalteter Antischadsoftware

**Leitfaden**: Azure Stack Edge verwendet Windows Defender Application Control (WDAC) mit einer Richtlinie für strikte Codeintegrität (CI), die nur die Ausführung vorab festgelegter Anwendungen und Skripts erlaubt. Die Antischadsoftware Windows Defender-Echtzeitschutz (Real Time Protection, RTP) ist ebenfalls aktiviert. Der Kunde kann Antischadsoftware auf den selbst erstellten Compute-VMs ausführen, um sie lokal auf dem Azure Stack Edge-Gerät auszuführen.

**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="backup-and-recovery"></a>Sicherung und Wiederherstellung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Sicherung und Wiederherstellung](../security/benchmarks/security-controls-v2-backup-recovery.md).*

### <a name="br-1-ensure-regular-automated-backups"></a>BR-1: Sicherstellen regelmäßiger automatisierter Sicherungen

**Leitfaden**: Regelmäßige Sicherungen Ihres Azure Stack Edge-Geräts werden dringend empfohlen, um die auf den auf dem Gerät bereitgestellten VMs enthaltenen Daten zu schützen. Sie können dafür Azure Backup oder Sicherheitslösungen von Drittanbietern verwenden. 

Datenschutzlösungen von Drittanbietern wie Cohesity, CommVault und Veritas können auch als Sicherungslösung für die Daten in lokalen SMB- oder NFS-Freigaben dienen. 

Zusätzliche Informationen finden Sie unter den aufgeführten Links.

- [Vorbereitung auf Gerätefehler](azure-stack-edge-gpu-prepare-device-failure.md)

- [Schützen von Dateien und Ordnern auf VMs](azure-stack-edge-gpu-prepare-device-failure.md#protect-files-and-folders-on-vms)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="br-2-encrypt-backup-data"></a>BR-2: Verschlüsseln von Sicherungsdaten

**Leitfaden**: Stellen Sie sicher, dass Ihre Sicherungen vor Angriffen geschützt sind. Dazu gehört auch die Verschlüsselung der Sicherungen zum Schutz vor dem Verlust der Vertraulichkeit.  Weitere Informationen finden Sie in der Sicherungslösung Ihrer Wahl.

- [Schützen von Daten in lokalen Edge-Freigaben](azure-stack-edge-gpu-prepare-device-failure.md#protect-data-in-edge-local-shares)

- [Schützen von Dateien und Ordnern auf VMs](azure-stack-edge-gpu-prepare-device-failure.md#protect-files-and-folders-on-vms)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="br-3-validate-all-backups-including-customer-managed-keys"></a>BR-3: Überprüfen aller Sicherungen einschließlich kundenseitig verwalteter Schlüssel

**Leitfaden**: Führen Sie regelmäßige Datenwiederherstellungen Ihrer Sicherungen aus. 

- [Wiederherstellungsverfahren für Azure Stack Edge](azure-stack-edge-gpu-recover-device-failure.md)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="br-4-mitigate-risk-of-lost-keys"></a>BR-4: Minimieren der Auswirkungen verlorener Schlüssel

**Leitfaden**: Stellen Sie sicher, dass alle Azure Stack Edge-Sicherungen (einschließlich kundenseitig verwalteter Schlüssel) gemäß den bewährten Methoden Ihrer Organisation geschützt werden. Ihr Azure Stack Edge-Gerät ist einem Azure Storage-Konto zugeordnet, das als Zielrepository für Ihre Daten in Azure verwendet wird. 

Der Zugriff auf das Azure Storage-Konto wird über das Azure-Abonnement und die beiden 512-Bit-Zugriffsschlüssel gesteuert, die dem Speicherkonto zugeordnet sind. Einer der Zugriffsschlüssel dient zur Authentifizierung, wenn das Azure Stack Edge-Gerät auf das Speicherkonto zugreift. Der andere Schlüssel dient als Reserve für die regelmäßige Schlüsselrotation. Stellen Sie sicher, dass bei der Schlüsselrotation die bewährten Sicherheitsmethoden angewandt werden.

- [Schützen von Daten auf Azure Stack Edge-Geräten in Azure Storage-Konten](azure-stack-edge-pro-r-security.md#protect-data-in-storage-accounts)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="governance-and-strategy"></a>Governance und Strategie

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Governance und Strategie](../security/benchmarks/security-controls-v2-governance-strategy.md).*

### <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>GS-1: Definieren der Strategie für Ressourcenverwaltung und Datenschutz 

**Leitfaden**: Stellen Sie sicher, dass Sie eine klare Strategie für die kontinuierliche Überwachung und den Schutz von Systemen und Daten dokumentieren und kommunizieren. Priorisieren Sie die Ermittlung, die Bewertung, den Schutz und die Überwachung unternehmenskritischer Daten und Systeme. 

Diese Strategie sollte dokumentierte Anleitungen, Richtlinien und Standards für die folgenden Elemente umfassen: 

-   Datenklassifizierungsstandard in Übereinstimmung mit den Geschäftsrisiken

-   Sicherheitsorganisationseinblick in Risiken und Ressourcenbestand 

-   Genehmigung durch die Sicherheitsorganisation für die Nutzung der Azure-Dienste 

-   Sicherheit von Ressourcen durch ihren gesamten Lebenszyklus

-   Erforderliche Zugriffssteuerungsstrategie in Übereinstimmung mit der Organisationsdatenklassifizierung

-   Verwendung von nativen Azure- und Drittanbieterfunktionen für den Datenschutz

-   Datenverschlüsselungsanforderungen für Anwendungsfälle während der Übertragung und für ruhende Anwendungsfälle

-   Geeignete Verschlüsselungsstandards

Weitere Informationen finden Sie in den folgenden Referenzen:
- [Empfehlung für eine Azure-Sicherheitsarchitektur: Speicher, Daten, Verschlüsselung](/azure/architecture/framework/security/storage-data-encryption?bc=%2fsecurity%2fcompass%2fbreadcrumb%2ftoc.json&toc=%2fsecurity%2fcompass%2ftoc.json)

- [Grundlegende Azure-Sicherheitsinformationen: Sicherheit, Verschlüsselung und Speicherung von Azure-Daten](../security/fundamentals/encryption-overview.md)

- [Cloud Adoption Framework: Bewährte Methoden für Datensicherheit und Datenverschlüsselung in Azure](../security/fundamentals/data-encryption-best-practices.md?bc=%2fazure%2fcloud-adoption-framework%2f_bread%2ftoc.json&toc=%2fazure%2fcloud-adoption-framework%2ftoc.json)

- [Azure-Sicherheitsvergleichstest: Ressorcenverwaltung](../security/benchmarks/security-controls-v2-asset-management.md)

- [Azure-Sicherheitsvergleichstest: Datenschutz](../security/benchmarks/security-controls-v2-data-protection.md)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="gs-2-define-enterprise-segmentation-strategy"></a>GS-2: Definieren der Strategie für Unternehmenssegmentierung 

**Leitfaden**: Erstellen Sie eine unternehmensweite Strategie zum Segmentieren des Zugriffs auf Ressourcen durch eine Kombination aus Identität, Netzwerk, Anwendung, Abonnement, Verwaltungsgruppe und anderen Steuerelementen.

Wägen Sie die Notwendigkeit einer Sicherheitstrennung sorgfältig gegen die Notwendigkeit ab, den täglichen Betrieb der Systeme zu ermöglichen, die miteinander kommunizieren und auf Daten zugreifen müssen.

Stellen Sie sicher, dass die Segmentierungsstrategie einheitlich für alle Steuerelementtypen implementiert wird, einschließlich Netzwerksicherheit, Identitäts- und Zugriffsmodelle, Berechtigungs-/Zugriffsmodelle für Anwendungen sowie Steuerelemente für Benutzerprozesse.

- [Leitfaden für die Segmentierungsstrategie in Azure (Video)](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Leitfaden für die Segmentierungsstrategie in Azure (Dokument)](/security/compass/governance#enterprise-segmentation-strategy)

- [Ausrichten der Netzwerksegmentierung an der Segmentierungsstrategie des Unternehmens](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="gs-3-define-security-posture-management-strategy"></a>GS-3: Definieren der Strategie für die Sicherheitsstatusverwaltung

**Leitfaden**: Führen Sie kontinuierliche Messungen durch, und Mindern Sie die Risiken für Ihre individuellen Ressourcen und die Umgebung, in der diese gehostet werden. Priorisieren Sie Ressourcen mit hohem Wert und hoch exponierte Angriffsflächen, wie z. B. veröffentlichte Anwendungen, Eingangs- und Ausgangspunkte für Netzwerke, Benutzer- und Administratorendpunkte usw.

- [Azure-Sicherheitsvergleichstest: Verwaltung von Status und Sicherheitsrisiken](../security/benchmarks/security-controls-v2-posture-vulnerability-management.md)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>GS-4: Ausrichten von Organisationsrollen, Zuständigkeiten und Verantwortlichkeiten

**Leitfaden**: Stellen Sie sicher, dass Sie eine klare Strategie für Rollen und Zuständigkeiten in ihrer Sicherheitsorganisation dokumentieren und kommunizieren. Priorisieren Sie durch Zuweisung klarer Verantwortlichkeiten für Sicherheitsentscheidungen, Schulung aller Beteiligten für das Modell der gemeinsamen Verantwortung und Schulung von technischen Teams hinsichtlich der Technologie zum Sichern der Cloud.

- [Azure Security Best Practice 1 – People: Educate Teams on Cloud Security Journey](/azure/cloud-adoption-framework/security/security-top-10#1-people-educate-teams-about-the-cloud-security-journey) (Bewährte Methoden für die Sicherheit von Azure 1 – Personen: Schulen von Teams auf dem Weg zur Cloudsicherheit)

- [Azure Security Best Practice 2 – People: Educate Teams on Cloud Security Technology](/azure/cloud-adoption-framework/security/security-top-10#2-people-educate-teams-on-cloud-security-technology) (Bewährte Methoden für die Sicherheit von Azure 1 – Personen: Schulen der Teams in Cloudsicherheitstechnologie)

- [Azure Security Best Practice 3 – Process: Assign Accountability for Cloud Security Decisions](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud) (Bewährte Methoden für die Sicherheit von Azure 1 – Prozess: Zuweisen von Verantwortlichkeit für Cloudsicherheitsentscheidungen)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="gs-5-define-network-security-strategy"></a>GS-5: Definieren einer Netzwerksicherheitsstrategie

**Leitfaden**: Richten Sie einen Ansatz für die Azure-Netzwerksicherheit im Rahmen der Gesamtstrategie Ihrer Organisation für die Sicherheitszugriffssteuerung ein.  

Diese Strategie sollte dokumentierte Anleitungen, Richtlinien und Standards für die folgenden Elemente umfassen: 

-   Zentralisieren der Verantwortlichkeit für Netzwerkverwaltung und -sicherheit

-   Segmentierungsmodell für virtuelle Netzwerke, das auf die Segmentierungsstrategie des Unternehmens abgestimmt ist

-   Wiederherstellungsstrategie in verschiedenen Bedrohungs- und Angriffsszenarien

-   Strategie für Internet-Randpunkte, -Eingangspunkte und -Ausgangspunkte

-   Strategie für Konnektivität zwischen Hybrid Cloud und lokalen Systemen

-   Aktuelle Netzwerksicherheitsartefakte (z. B. Netzwerkdiagramme, Referenznetzwerkarchitektur)

Weitere Informationen finden Sie in den folgenden Referenzen:
- [Azure Security Best Practice 11 – Architecture. Single unified security strategy](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy) (Bewährte Methoden für die Sicherheit von Azure 11 – Architektur: Zentralisierte einheitliche Sicherheitsstrategie)

- [Azure-Sicherheitsvergleichstest: Netzwerksicherheit](../security/benchmarks/index.yml)

- [Die Netzwerksicherheit in Azure in der Übersicht](../security/fundamentals/network-overview.md)

- [Strategie für die Unternehmensnetzwerkarchitektur](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="gs-6-define-identity-and-privileged-access-strategy"></a>GS-6: Definieren der Strategie für Identität und privilegierten Zugriff

**Leitfaden**: Richten Sie einen Ansatz für Azure-Identität und privilegierten Zugriff im Rahmen der Gesamtstrategie Ihrer Organisation für die Sicherheitszugriffssteuerung ein.  

Diese Strategie sollte dokumentierte Anleitungen, Richtlinien und Standards für die folgenden Elemente umfassen: 

-   Zentralisiertes Identitäts- und Authentifizierungssystem mit Interkonnektivität zu anderen internen und externen Identitätssystemen

-   Starke Authentifizierungsmethoden in verschiedenen Anwendungsfällen und Bedingungen

-   Schutz stark privilegierter Benutzer

-   Überwachung und Behandlung von Anomalien bei Benutzeraktivitäten  

-   Benutzeridentität und Zugriffsüberprüfung und Abstimmungsprozess

Weitere Informationen finden Sie in den folgenden Referenzen:

- [Azure-Sicherheitsvergleichstest: Identitätsverwaltung](../security/benchmarks/security-controls-v2-identity-management.md)

- [Azure-Sicherheitsvergleichstest: Privilegierter Zugriff](../security/benchmarks/security-controls-v2-privileged-access.md)

- [Azure Security Best Practice 11 – Architecture. Single unified security strategy](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy) (Bewährte Methoden für die Sicherheit von Azure 11 – Architektur: Zentralisierte einheitliche Sicherheitsstrategie)

- [Übersicht über die Sicherheit der Azure-Identitätsverwaltung](../security/fundamentals/identity-management-overview.md)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="gs-7-define-logging-and-threat-response-strategy"></a>GS-7: Definieren einer Strategie für Protokollierung und Reaktion auf Bedrohungen

**Leitfaden**: Richten Sie eine Strategie für Protokollierung und Reaktion auf Bedrohungen zur schnellen Erkennung und Behebung von Bedrohungen bei gleichzeitiger Erfüllung von Complianceanforderungen ein. Priorisieren Sie die Bereitstellung wertvoller Warnungen und nahtloser Funktionen für Analysten, damit sie sich auf Bedrohungen konzentrieren können, anstatt mit der Integration und der Ausführung manueller Schritte. 

Diese Strategie sollte dokumentierte Anleitungen, Richtlinien und Standards für die folgenden Elemente umfassen: 

-   Rolle und Verantwortlichkeiten der Organisation für Sicherheitsvorgänge (SecOps) 

-   Ein klar definierter Prozess zur Reaktion auf Vorfälle gemäß NIST oder einem anderen Branchenframework 

-   Erfassung und Aufbewahrung von Protokollen zur Unterstützung von Bedrohungserkennung, Reaktion auf Vorfälle und Complianceanforderungen

-   Zentralisierte Sichtbarkeit von und Korrelationsinformationen zu Bedrohungen unter Verwendung von SIEM, nativen Azure-Funktionen und anderen Quellen 

-   Kommunikations- und Benachrichtigungsplan mit Ihren Kunden, Lieferanten und öffentlichen Interessengruppen

-   Verwendung von nativen Azure-Plattformen und Plattformen von Drittanbietern für die Behandlung von Vorfällen, wie z. B. Protokollierung und Bedrohungserkennung, Forensik sowie Reduzierung und Bekämpfung von Angriffen

-   Prozesse für den Umgang mit Vorfällen und Aktivitäten nach Vorfällen, wie z. B. Erkenntnisse und Beweissicherung

Weitere Informationen finden Sie in den folgenden Referenzen:

- [Azure-Sicherheitsvergleichstest: Protokollierung und Bedrohungserkennung](../security/benchmarks/security-controls-v2-logging-threat-detection.md)

- [Azure-Sicherheitsvergleichstest: Reaktion auf Vorfälle](../security/benchmarks/security-controls-v2-incident-response.md)

- [Azure Security Best Practice 4 – Process. Update Incident Response Processes for Cloud](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud) (Bewährte Methoden für die Sicherheit von Azure 4 – Prozess: Aktualisieren der Prozesse zur Reaktion auf Vorfälle für die Cloud)

- [Azure Adoption Framework, Entscheidungshilfe für Protokollierung und Berichterstattung](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die [Übersicht über Version 2 des Azure-Sicherheitsvergleichstests](../security/benchmarks/overview.md) an.
- Erfahren Sie mehr über [Azure-Sicherheitsbaselines](../security/benchmarks/security-baselines-overview.md).
