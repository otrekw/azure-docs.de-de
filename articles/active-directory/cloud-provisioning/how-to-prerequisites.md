---
title: Voraussetzungen für die Azure AD Connect-Cloudbereitstellung in Azure AD
description: In diesem Artikel werden die Voraussetzungen und Hardwareanforderungen beschrieben, die für die Cloudbereitstellung erforderlich sind.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/06/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6dbdd5153186ee47e37856637eac16d6d450cc5a
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2020
ms.locfileid: "94695179"
---
# <a name="prerequisites-for-azure-ad-connect-cloud-provisioning"></a>Voraussetzungen für die Azure AD Connect-Cloudbereitstellung
Dieser Artikel enthält Anleitungen zur Auswahl und Verwendung der Azure Active Directory (Azure AD) Connect-Cloudbereitstellung als Identitätslösung.



## <a name="cloud-provisioning-agent-requirements"></a>Anforderungen des Agents für die Cloudbereitstellung
Für die Verwendung der Azure AD Connect-Cloudbereitstellung benötigen Sie Folgendes:
    
- Ein Hybrididentität-Administratorkonto für Ihren Azure AD-Mandanten, das kein Gastbenutzer ist.
- einen lokalen Server für den Bereitstellungs-Agent mit Windows 2012 R2 oder höher  Bei diesem Server sollte es sich um einen Server der Ebene 0 im [Active Directory-Verwaltungsebenenmodell](/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material) handeln.
- lokale Firewallkonfigurationen

>[!NOTE]
>Der Bereitstellungs-Agent kann derzeit nur auf englischsprachigen Servern installiert werden. Das Installieren eines englischen Sprachpakets auf einem nicht englischsprachigen Server ist als Problemumgehung ungültig und führt dazu, dass der Agent nicht installiert wird. 

Im Weiteren finden Sie schrittweise Anleitungen für diese Voraussetzungen.

### <a name="in-the-azure-active-directory-admin-center"></a>Im Azure Active Directory Admin Center

1. Erstellen Sie in Ihrem Azure AD-Mandanten ein auf die Cloud beschränktes Hybrididentität-Administratorkonto. Auf diese Weise können Sie die Konfiguration Ihres Mandanten verwalten, wenn Ihre lokalen Dienste ausfallen oder nicht verfügbar sind. Erfahren Sie, wie Sie ein [auf die Cloud beschränktes Hybrididentität-Administratorkonto hinzufügen](../fundamentals/add-users-azure-active-directory.md). Die Ausführung dieses Schritts ist äußerst wichtig, damit sichergestellt ist, dass Sie für Ihren Mandanten nicht gesperrt werden.
1. Fügen Sie Ihrem Azure AD-Mandanten mindestens einen [benutzerdefinierten Domänennamen](../fundamentals/add-custom-domain.md) hinzu. Ihre Benutzer können sich mit einem dieser Domänennamen anmelden.

### <a name="in-your-directory-in-active-directory"></a>In Ihrem Verzeichnis in Active Directory

Führen Sie das [IdFix-Tool](/office365/enterprise/prepare-directory-attributes-for-synch-with-idfix) aus, um die Verzeichnisattribute für die Synchronisierung vorzubereiten.

### <a name="in-your-on-premises-environment"></a>In Ihrer lokalen Umgebung

1. Geben Sie einen in die Domäne eingebundenen Hostserver unter Windows Server 2012 R2 oder höher mit mindestens 4 GB RAM und .NET 4.7.1 + Runtime an.

1. Die PowerShell-Ausführungsrichtlinie auf dem lokalen Server muss auf „Nicht definiert“ oder „RemoteSigned“ festgelegt werden.

1. Wenn zwischen Ihren Servern und Azure AD eine Firewall eingerichtet wurde, konfigurieren Sie die folgenden Elemente:
   - Stellen Sie sicher, dass Agents über die folgenden Ports *ausgehende* Anforderungen an Azure AD senden können:

        | Portnummer | Wie diese verwendet wird |
        | --- | --- |
        | **80** | Herunterladen der Zertifikatsperrlisten (CRLs) bei der Überprüfung des TLS/SSL-Zertifikats.  |
        | **443** | Verarbeiten der gesamten ausgehenden Kommunikation mit dem Dienst |
        | **8080** (optional) | Agents melden ihren Status alle zehn Minuten über den Port 8080, wenn der Port 443 nicht verfügbar ist. Dieser Status wird im Azure AD-Portal angezeigt. |
     
   - Wenn Ihre Firewall Regeln gemäß Ursprungsbenutzern erzwingt, öffnen Sie diese Ports für den Datenverkehr aus Windows-Diensten, die als Netzwerkdienst ausgeführt werden.
   - Wenn Ihre Firewall oder Ihr Proxy das Angeben sicherer Suffixe zulässt, fügen Sie Verbindungen zu \*.msappproxy.net\* und „.servicebus.windows.net“ hinzu. Aktivieren Sie andernfalls den Zugriff auf die [IP-Adressbereiche für das Azure-Rechenzentrum](https://www.microsoft.com/download/details.aspx?id=41653), die wöchentlich aktualisiert werden.
   - Ihre Agents benötigen für die Erstregistrierung Zugriff auf „login.windows.net“ und „login.microsoftonline.com“. Öffnen Sie Ihre Firewall auch für diese URLs.
   - Geben Sie für die Zertifikatüberprüfung folgende URLs frei: „mscrl.microsoft.com:80“, „crl.microsoft.com:80“, „ocsp.msocsp.com:80“ und „www\.microsoft.com:80“. Da diese URLs für die Zertifikatüberprüfung in Verbindung mit anderen Microsoft-Produkten verwendet werden, haben Sie diese möglicherweise bereits freigegeben.

>[!NOTE]
> Die Installation des Agents für die Cloudbereitstellung unter Windows Server Core wird nicht unterstützt.


### <a name="additional-requirements"></a>Zusätzliche Anforderungen
- [Microsoft .NET Framework 4.7.1](https://www.microsoft.com/download/details.aspx?id=56116) 

#### <a name="tls-requirements"></a>TLS-Anforderungen

>[!NOTE]
>Transport Layer Security (TLS) ist ein Protokoll, das für eine sichere Kommunikation sorgt. Das Ändern der TLS-Einstellungen wirkt sich auf die Gesamtstruktur aus. Weitere Informationen finden Sie unter [Update zur Aktivierung von TLS 1.1 und TLS 1.2 als sichere Standardprotokolle in WinHTTP unter Windows](https://support.microsoft.com/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-default-secure-protocols-in-wi).

Auf dem Windows-Server, auf dem der Agent für die Azure AD Connect-Cloudbereitstellung gehostet wird, muss TLS 1.2 aktiviert sein, bevor Sie den Agent installieren.

Führen Sie diese Schritte aus, um TLS 1.2 zu aktivieren.

1. Legen Sie die folgenden Registrierungsschlüssel fest:
    
    ```
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001
    ```

1. Starten Sie den Server neu.

## <a name="known-limitations"></a>Bekannte Einschränkungen
Es gelten die folgenden bekannten Einschränkungen:

### <a name="delta-synchronization"></a>Deltasynchronisierung

- Bei der Gruppenbereichsfilterung für die Deltasynchronisierung werden nicht mehr als 1.500 Mitglieder unterstützt.
- Wenn Sie eine Gruppe löschen, die für einen Gruppenbereichsfilter verwendet wird, werden Benutzer, die Mitglieder der Gruppe sind, nicht gelöscht. 
- Wenn Sie die im Bereich befindliche Organisationseinheit oder Gruppe umbenennen, werden die Benutzer bei der Deltasynchronisierung nicht entfernt.

### <a name="provisioning-logs"></a>Bereitstellungsprotokolle
- In den Bereitstellungsprotokollen wird nicht eindeutig zwischen Erstellungs- und Aktualisierungsvorgängen unterschieden.  Es kann sein, dass ein Erstellungsvorgang für eine Aktualisierung und ein Aktualisierungsvorgang für eine Erstellung angezeigt wird.

### <a name="cross-domain-references"></a>Domänenübergreifende Verweise
- Falls Sie über Benutzer mit Mitgliedsverweisen einer anderen Domäne verfügen, werden diese im Rahmen der aktuellen Domänensynchronisierung für diesen Benutzer nicht synchronisiert. 
- (Beispiel: Ein Manager des zu synchronisierenden Benutzers befindet sich in Domäne B, und der Benutzer in Domäne A. Wenn Sie die Synchronisierung für Domäne A und Domäne B durchführen, ist dieser Vorgang erfolgreich, aber der Manager des Benutzers wird nicht übernommen.)

### <a name="group-re-naming-or-ou-re-naming"></a>Umbenennen von Gruppen oder Organisationseinheiten
- Wenn Sie eine Gruppe oder Organisationseinheit in AD umbenennen, die sich im Bereich einer bestimmten Konfiguration befindet, wird die Namensänderung in AD vom Cloudbereitstellungsauftrag nicht erkannt. Der Auftrag wird nicht unter Quarantäne gestellt und bleibt im fehlerfreien Zustand.



## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)