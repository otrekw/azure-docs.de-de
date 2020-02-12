---
title: Voraussetzungen für die Azure AD Connect-Cloudbereitstellung in Azure AD
description: In diesem Artikel werden die Voraussetzungen und Hardwareanforderungen beschrieben, die für die Cloudbereitstellung erforderlich sind.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 51e5c58d29f01cadcc3ea2e8ec48ae67e58c4180
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2020
ms.locfileid: "76909039"
---
# <a name="prerequisites-for-azure-ad-connect-cloud-provisioning"></a>Voraussetzungen für die Azure AD Connect-Cloudbereitstellung
Dieser Artikel enthält Anleitungen zur Auswahl und Verwendung der Azure Active Directory (Azure AD) Connect-Cloudbereitstellung als Identitätslösung.



## <a name="cloud-provisioning-agent-requirements"></a>Anforderungen des Agents für die Cloudbereitstellung
Für die Verwendung der Azure AD Connect-Cloudbereitstellung benötigen Sie Folgendes:
    
- ein globales Administratorkonto für Ihren Azure AD-Mandanten
- einen lokalen Server für den Bereitstellungs-Agent mit Windows 2012 R2 oder höher
- lokale Firewallkonfigurationen

>[!NOTE]
>Der Bereitstellungs-Agent kann derzeit nur auf englischsprachigen Servern installiert werden. Das Installieren eines englischen Sprachpakets auf einem nicht englischsprachigen Server ist als Problemumgehung ungültig und führt dazu, dass der Agent nicht installiert wird. 

Im Weiteren finden Sie schrittweise Anleitungen für diese Voraussetzungen.

### <a name="in-the-azure-active-directory-admin-center"></a>Im Azure Active Directory Admin Center

1. Erstellen Sie in Ihrem Azure AD-Mandanten ein auf die Cloud beschränktes globales Administratorkonto. Auf diese Weise können Sie die Konfiguration Ihres Mandanten verwalten, wenn Ihre lokalen Dienste ausfallen oder nicht verfügbar sind. Erfahren Sie, wie Sie ein [auf die Cloud beschränktes globales Administratorkonto hinzufügen](../active-directory-users-create-azure-portal.md). Die Ausführung dieses Schritts ist äußerst wichtig, damit sichergestellt ist, dass Sie für Ihren Mandanten nicht gesperrt werden.
1. Fügen Sie Ihrem Azure AD-Mandanten mindestens einen [benutzerdefinierten Domänennamen](../active-directory-domains-add-azure-portal.md) hinzu. Ihre Benutzer können sich mit einem dieser Domänennamen anmelden.

### <a name="in-your-on-premises-environment"></a>In Ihrer lokalen Umgebung

1. Geben Sie einen in die Domäne eingebundenen Hostserver unter Windows Server 2012 R2 oder höher mit mindestens 4 GB RAM und .NET 4.7.1 + Runtime an.

1. Wenn zwischen Ihren Servern und Azure AD eine Firewall eingerichtet wurde, konfigurieren Sie die folgenden Elemente:
   - Stellen Sie sicher, dass Agents über die folgenden Ports *ausgehende* Anforderungen an Azure AD senden können:

        | Portnummer | Wie diese verwendet wird |
        | --- | --- |
        | **80** | Herunterladen der Zertifikatsperrlisten (CRL) bei der Überprüfung des SSL-Zertifikats  |
        | **443** | Verarbeiten der gesamten ausgehenden Kommunikation mit dem Dienst |
        | **8080** (optional) | Agents melden ihren Status alle zehn Minuten über den Port 8080, wenn der Port 443 nicht verfügbar ist. Dieser Status wird im Azure AD-Portal angezeigt. |
     
   - Wenn Ihre Firewall Regeln gemäß Ursprungsbenutzern erzwingt, öffnen Sie diese Ports für den Datenverkehr aus Windows-Diensten, die als Netzwerkdienst ausgeführt werden.
   - Wenn Ihre Firewall oder Ihr Proxy das Angeben sicherer Suffixe zulässt, fügen Sie Verbindungen zu \*.msappproxy.net\* und „.servicebus.windows.net“ hinzu. Aktivieren Sie andernfalls den Zugriff auf die [IP-Adressbereiche für das Azure-Rechenzentrum](https://www.microsoft.com/download/details.aspx?id=41653), die wöchentlich aktualisiert werden.
   - Ihre Agents benötigen für die Erstregistrierung Zugriff auf „login.windows.net“ und „login.microsoftonline.com“. Öffnen Sie Ihre Firewall auch für diese URLs.
   - Geben Sie für die Zertifikatüberprüfung folgende URLs frei: „mscrl.microsoft.com:80“, „crl.microsoft.com:80“, „ocsp.msocsp.com:80“ und „www\.microsoft.com:80“. Da diese URLs für die Zertifikatüberprüfung in Verbindung mit anderen Microsoft-Produkten verwendet werden, haben Sie diese möglicherweise bereits freigegeben.

### <a name="verify-the-port"></a>Überprüfen des Ports
Mithilfe der folgenden URL können Sie überprüfen, ob Azure Port 443 überwacht und der Agent mit diesem kommunizieren kann:

https://aadap-portcheck.connectorporttest.msappproxy.net/ 

Mithilfe dieses Tests wird überprüft, ob Ihre Agents über Port 443 mit Azure kommunizieren können. Öffnen Sie einen Browser, und navigieren Sie von dem Server, auf dem der Agent installiert ist, zur obigen URL.

![Überprüfen der Porterreichbarkeit](media/how-to-install/verify2.png)

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


## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)

