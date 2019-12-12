---
title: Voraussetzungen für die Azure AD Connect-Cloudbereitstellung in Azure AD
description: In diesem Thema werden die Voraussetzungen und die Hardwareanforderungen für die Cloudbereitstellung beschrieben.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 155edf72a60e079a609853e953e3cf66024cc83c
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74795453"
---
# <a name="prerequisites-for-azure-ad-connect-cloud-provisioning"></a>Voraussetzungen für die Azure AD Connect-Cloudbereitstellung
Dieses Thema bietet einen Leitfaden zum Auswählen und Verwenden der Azure AD Connect-Cloudbereitstellung als Identitätslösung.



## <a name="cloud-provisioning-agent-requirements"></a>Anforderungen des Agents für die Cloudbereitstellung
Für die Verwendung der Azure AD Connect-Cloudbereitstellung benötigen Sie Folgendes:
    
- ein globales Administratorkonto für Ihren Azure AD-Mandanten
- ein lokaler Server für den Bereitstellungs-Agent mit Windows 2012 R2 oder höher
- Konfigurationen der lokalen Firewall

Im restlichen Dokument finden Sie Schrittanleitungen für diese Voraussetzungen.

### <a name="in-the-azure-active-directory-admin-center"></a>Im Azure Active Directory Admin Center

1. Erstellen Sie in Ihrem Azure AD-Mandanten ein auf die Cloud beschränktes globales Administratorkonto. Auf diese Weise können Sie die Konfiguration Ihres Mandanten verwalten, falls Ihre lokalen Dienste ausfallen oder nicht verfügbar sind. Erfahren Sie, wie Sie ein [rein cloudbasiertes Konto für den globalen Administrator hinzufügen](../active-directory-users-create-azure-portal.md). Die Ausführung dieses Schritts ist sehr wichtig, damit sichergestellt ist, dass Sie für Ihren Mandanten nicht gesperrt werden.
2. Fügen Sie Ihrem Azure AD-Mandanten mindestens einen [benutzerdefinierten Domänennamen](../active-directory-domains-add-azure-portal.md) hinzu. Ihre Benutzer können sich mit einem dieser Domänennamen anmelden.

### <a name="in-your-on-premises-environment"></a>In Ihrer lokalen Umgebung

1. Geben Sie einen in die Domäne eingebundenen Hostserver unter Windows Server 2012 R2 oder höher mit mindestens 4 GB RAM und .NET-Runtime (ab 4.7.1) an. 

2. Wenn zwischen Ihren Servern und Azure AD eine Firewall eingerichtet wurde, konfigurieren Sie die folgenden Elemente:
   - Stellen Sie sicher, dass Agents über die folgenden Ports *ausgehende* Anforderungen an Azure AD senden können:

     | Portnummer | Wie diese verwendet wird |
     | --- | --- |
     | **80** | Herunterladen der Zertifikatssperrlisten (CRL) bei der Überprüfung des SSL-Zertifikats |
     | **443** | Verarbeitung der gesamten ausgehende Kommunikation mit dem Dienst |
     | **8080** (optional) | Agents melden ihren Status alle zehn Minuten über den Port 8080, wenn der Port 443 nicht verfügbar ist. Dieser Status wird im Azure AD-Portal angezeigt. Port 8080 wird _nicht_ für Benutzeranmeldungen verwendet. |
     
     Wenn Ihre Firewall Regeln gemäß Ursprungsbenutzern erzwingt, öffnen Sie diese Ports für den Datenverkehr aus Windows-Diensten, die als Netzwerkdienst ausgeführt werden.
   - Wenn Ihre Firewall oder Ihr Proxy das Angeben sicherer Suffixe zulässt, fügen Sie Verbindungen zu **\*.msappproxy.net** und **\*.servicebus.windows.net** hinzu. Aktivieren Sie andernfalls den Zugriff auf die [IP-Adressbereiche für das Azure-Rechenzentrum](https://www.microsoft.com/download/details.aspx?id=41653), die wöchentlich aktualisiert werden.
   - Ihre Agents benötigen für die Erstregistrierung Zugriff auf **login.windows.net** und **login.microsoftonline.com**. Öffnen Sie Ihre Firewall auch für diese URLs.
   - Geben Sie für die Überprüfung des Zertifikats folgende URLs frei: **mscrl.microsoft.com:80**, **crl.microsoft.com:80**, **ocsp.msocsp.com:80** und **www\.microsoft.com:80**. Da diese URLs für die Überprüfung des Zertifikats in Verbindung mit anderen Microsoft-Produkten verwendet werden, haben Sie diese möglicherweise bereits freigegeben.

### <a name="verify-the-port"></a>Überprüfen des Ports
Sie können den folgenden Link verwenden, um zu überprüfen, ob Azure Port 443 überwacht und ob der Agent mit diesem kommunizieren kann:

https://aadap-portcheck.connectorporttest.msappproxy.net/ 

Mithilfe dieses Tests wird überprüft, ob Ihre Agents über Port 443 mit Azure kommunizieren können.  Öffnen Sie einen Browser, und navigieren Sie zu der obigen URL des Servers, auf dem der Agent installiert ist.
![Dienste](media/how-to-install/verify2.png)

### <a name="additional-requirements"></a>Weitere Anforderungen
- [Microsoft .NET Framework 4.7.1](https://www.microsoft.com/download/details.aspx?id=56116) 

#### <a name="tls-requirements"></a>TLS-Anforderungen

>[!NOTE]
>Bei der Transport Layer Security (TLS) handelt es sich um ein Protokoll, das für eine sichere Kommunikation sorgt.  Das Ändern der TLS-Einstellungen wirkt sich auf die Gesamtstruktur aus.  Weitere Informationen finden Sie unter [Update zur Aktivierung von TLS 1.1 und TLS 1.2 als sichere Standardprotokolle in WinHTTP unter Windows](https://support.microsoft.com/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-default-secure-protocols-in-wi).

Auf dem Windows-Server, auf dem der Agent für die Azure AD Connect-Cloudbereitstellung gehostet wird, muss TLS 1.2 aktiviert sein, bevor Sie diesen installieren.

So aktivieren Sie TLS 1.2

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

