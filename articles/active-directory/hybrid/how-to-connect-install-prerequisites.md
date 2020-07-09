---
title: 'Azure AD Connect: Voraussetzungen und Hardware | Microsoft-Dokumentation'
description: Dieses Thema beschreibt die Voraussetzungen und die Hardwareanforderungen für Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 91b88fda-bca6-49a8-898f-8d906a661f07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 02/27/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2bcf7b5b8791b813a28133d8a662d1736aacf35a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85358717"
---
# <a name="prerequisites-for-azure-ad-connect"></a>Voraussetzungen für Azure AD Connect
Dieses Thema beschreibt die Voraussetzungen und die Hardwareanforderungen für Azure AD Connect.

## <a name="before-you-install-azure-ad-connect"></a>Vor der Installation von Azure AD Connect
Vor der Installation von Azure AD Connect gibt es einige Dinge, die Sie benötigen.

### <a name="azure-ad"></a>Azure AD
* Einen Azure AD-Mandanten. Sie können diesen mit einer [kostenlosen Azure-Testversion erhalten](https://azure.microsoft.com/pricing/free-trial/). Sie können eines der folgenden Portale verwenden, um Azure AD Connect zu verwalten:
  * [Azure-Portal](https://portal.azure.com)
  * [Office-Portal](https://portal.office.com)  
* [Fügen Sie die Domäne hinzu](../active-directory-domains-add-azure-portal.md) , die Sie in Azure AD verwenden möchten, und überprüfen Sie sie. Wenn Sie beispielsweise planen, „contoso.com“ für Ihre Benutzer zu verwenden, sollten Sie sicherstellen, dass diese Domäne überprüft wurde und nicht nur die Standarddomäne „contoso.onmicrosoft.com“ verwendet wird.
* In einem Azure AD-Mandanten sind standardmäßig 50.000 Objekte zulässig. Wenn Sie Ihre Domäne verifizieren, wird der Grenzwert auf 300.000 Objekte erhöht. Wenn Sie noch mehr Objekte in Azure AD benötigen, müssen Sie eine Supportanfrage stellen, um den Grenzwert noch weiter zu erhöhen. Wenn Sie mehr als 500.000 Objekte verwalten müssen, benötigen Sie eine Lizenz, z.B. für Office 365, Azure AD Basic, Azure AD Premium oder Enterprise Mobility and Security.

### <a name="prepare-your-on-premises-data"></a>Vorbereiten Ihrer lokalen Daten
* Verwenden Sie [IdFix](https://support.office.com/article/Install-and-run-the-Office-365-IdFix-tool-f4bd2439-3e41-4169-99f6-3fabdfa326ac) zum Ermitteln von Fehlern wie Duplikaten und Formatierungsproblemen in Ihrem Verzeichnis, bevor Sie eine Synchronisierung mit Azure AD und Office 365 durchführen.
* Überprüfen Sie die [optionalen Synchronisierungsfunktionen, die Sie in Azure AD aktivieren können](how-to-connect-syncservice-features.md) , und bestimmen Sie, welche Funktionen Sie aktivieren sollten.

### <a name="on-premises-active-directory"></a>Lokales Active Directory
* Die AD-Schemaversion und Funktionsebene der Gesamtstruktur müssen Windows Server 2003 oder höher entsprechen. Die Domänencontroller können eine beliebige Version ausführen, solange die Anforderungen an Schema und Gesamtstrukturebene erfüllt werden.
* Wenn Sie das Feature **Kennwortrückschreiben** verwenden möchten, müssen die Domänencontroller unter Windows Server 2008 R2 oder höher ausgeführt werden.
* Der von Azure AD verwendete Domänencontroller darf nicht schreibgeschützt sein. Die Verwendung eines schreibgeschützten Domänencontrollers wird **nicht unterstützt**, und Azure AD Connect folgt keinen Umleitungen für Schreibvorgänge.
* Die Verwendung lokaler Gesamtstrukturen/Domänen mit NetBios-Namen, die einen Punkt (.) enthalten, wird **nicht unterstützt**.
* Es wird empfohlen, den [Active Directory-Papierkorb zu aktivieren](how-to-connect-sync-recycle-bin.md).

### <a name="azure-ad-connect-server"></a>Azure AD Connect-Server
>[!IMPORTANT]
>Der Azure AD Connect-Server enthält wichtige Identitätsdaten und sollte wie unter [Active Directory-Verwaltungsebenenmodell](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material) beschrieben als Komponente der Ebene 0 behandelt werden.

* Azure AD Connect kann nicht auf Small Business Server oder Windows Server Essentials vor 2019 (Windows Server Essentials 2019 wird unterstützt) installiert werden. Der Server muss Windows Server Standard oder höher verwenden.
* Vom Installieren von Azure AD Connect auf einem Domänencontroller wird aufgrund von Sicherheitsmaßnahmen und einschränkenden Einstellungen abgeraten, die die ordnungsgemäße Installation von Azure AD Connect verhindern können.
* Auf dem Azure AD Connect Server muss eine vollständige GUI installiert sein. Eine Installation unter Server Core wird **nicht unterstützt**.
>[!IMPORTANT]
>Die Installation von Azure AD Connect auf Small Business Servern, Server Essentials oder Server Core wird nicht unterstützt.

* Azure AD Connect muss unter Windows Server 2012 oder höher installiert werden. Dieser Server muss in die Domäne eingebunden werden und könnte ein Domänencontroller oder Mitgliedsserver sein.
* Auf dem Azure AD Connect-Server darf nicht die PowerShell-Aufzeichnung-Gruppenrichtlinie aktiviert sein, wenn Sie die ADFS-Konfiguration mit dem Azure AD Connect-Assistenten verwalten. Sie können die PowerShell-Aufzeichnung aktivieren, wenn Sie den Azure AD Connect-Assistenten zur Verwaltung der Synchronisierungskonfiguration verwenden.
* Wenn Active Directory-Verbunddienste bereitgestellt werden, müssen die Server, auf denen die Active Directory-Verbunddienste oder der Webanwendungsproxy installiert werden, Windows Server 2012 R2 oder höher ausführen. [Windows-Remoteverwaltung](#windows-remote-management) muss auf diesen Servern zur Remoteinstallation aktiviert werden.
* Wenn Active Directory-Verbunddienste bereitgestellt werden, benötigen Sie [TLS/SSL-Zertifikate](#tlsssl-certificate-requirements).
* Wenn Active Directory-Verbunddienste bereitgestellt werden, müssen Sie die [Namensauflösung](#name-resolution-for-federation-servers)konfigurieren.
* Wenn Ihre globalen Administratoren MFA aktiviert haben, muss die URL **https://secure.aadcdn.microsoftonline-p.com** in der Liste vertrauenswürdiger Websites enthalten sein. Sie werden aufgefordert, diese Website der Liste vertrauenswürdigen Websites hinzuzufügen, wenn Sie zu einem MFA-Captcha aufgefordert werden und diese zuvor noch nicht hinzugefügt wurde. Sie können dafür den Internet Explorer verwenden.
* Microsoft empfiehlt die Härtung des Azure AD Connect-Servers, um die Angriffsfläche für Sicherheitsangriffe für diese wichtige Komponente der IT-Umgebungen zu verringern.  Wenn Sie die unten genannten Empfehlungen befolgen, verringern Sie die Sicherheitsrisiken für Ihre Organisation.

* Stellen Sie Azure AD Connect auf einem mit der Domäne verbundenen Server bereit, und schränken Sie den administrativen Zugriff auf Domänenadministratoren oder andere streng kontrollierte Sicherheitsgruppen ein.

Weitere Informationen finden Sie unter: 

* [Sichern von Administratorgruppen](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/appendix-g--securing-administrators-groups-in-active-directory)

* [Sichern von integrierten Administratorkonten](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/appendix-d--securing-built-in-administrator-accounts-in-active-directory)

* [Verbesserung der Sicherheit und vorbeugenden Wartung durch Verringern von Angriffsoberflächen](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access#2-reduce-attack-surfaces )

* [Verringern der Active Directory-Angriffsfläche](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface)

### <a name="sql-server-used-by-azure-ad-connect"></a>Von Azure AD Connect verwendete SQL Server-Datenbank
* Azure AD Connect erfordert eine SQL Server-Datenbank zum Speichern von Identitätsdaten. Standardmäßig wird SQL Server 2012 Express LocalDB (eine einfache Version von SQL Server Express) installiert. Für SQL Server Express gilt ein 10-GB-Limit, mit dem Sie etwa 100.000 Objekte verwalten können. Wenn Sie eine höhere Anzahl von Verzeichnisobjekten verwalten möchten, müssen Sie im Installations-Assistenten auf eine andere Version von SQL Server verweisen. Der SQL Server-Installationstyp kann sich auf die [Leistung von Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-performance-factors#sql-database-factors) auswirken.
* Wenn Sie eine separate Installation von SQL Server verwenden, gelten die folgenden Anforderungen:
  * Azure AD Connect unterstützt sämtliche Versionen von Microsoft SQL Server von 2012 (mit dem neuesten Service Pack) bis SQL Server 2019. Microsoft Azure SQL-Datenbank wird als Datenbank **nicht unterstützt**.
  * Sie müssen eine SQL-Sortierung ohne Berücksichtigung der Groß- und Kleinschreibung verwenden. Diese Sortierungen werden durch „\_CI_“ in ihrem Namen bestimmt. Eine Sortierung mit Berücksichtigung der Groß- und Kleinschreibung und „\_CS_“ im Namen wird **nicht unterstützt**.
  * Sie können jeweils nur ein Synchronisierungsmodul pro SQL-Instanz verwenden. Die Freigabe einer SQL-Instanz mit FIM/MIM Sync, DirSync oder Azure AD Sync wird **nicht unterstützt** .

### <a name="accounts"></a>Konten
* Ein globales Azure AD-Administratorkonto für den Azure AD-Mandanten, in den die Integration erfolgen soll. Bei diesem Konto muss es sich um ein **Geschäfts-, Schul- oder Unikonto** handeln, und es darf kein **Microsoft-Konto** sein.
* Wenn Sie [Expresseinstellungen verwenden](reference-connect-accounts-permissions.md#express-settings-installation) oder ein Upgrade von DirSync durchführen, müssen Sie über ein Enterprise-Administratorkonto für Ihr lokales Active Directory verfügen.
* Wenn Sie den Installationspfad für benutzerdefinierte Einstellungen verwenden, haben Sie mehr Möglichkeiten. Weitere Informationen finden Sie unter [Einstellungen für die benutzerdefinierte Installation](reference-connect-accounts-permissions.md#custom-installation-settings).

### <a name="connectivity"></a>Konnektivität
* Der Azure AD Connect-Server benötigt die DNS-Auflösung sowohl für das Intranet als auch für das Internet. Der DNS-Server muss Namen sowohl zu Ihrem lokalen Active Directory als auch zu den Azure AD-Endpunkten auflösen können.
* Wenn Sie in Ihrem Intranet Firewalls verwenden und die Ports zwischen den Azure AD Connect-Servern und Ihren Domänencontrollern öffnen müssen, finden Sie Informationen hierzu unter [Azure AD Connect-Ports](reference-connect-ports.md).
* Wenn Ihr Proxy oder Ihre Firewall den Zugriff auf bestimmte URLs beschränkt, müssen die unter [URLs und IP-Adressbereiche von Office 365](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) dokumentierten URLs geöffnet werden.
  * Wenn Sie die Microsoft Cloud in Deutschland oder die Microsoft Azure Government-Cloud verwenden, finden Sie die URLs unter [Azure AD Connect: besondere Überlegungen zu Synchronisierungsdienstinstanzen](reference-connect-instances.md) .
* Azure AD Connect (Version 1.1.614.0 und höher) verwendet standardmäßig TLS 1.2 für die Verschlüsselung der Kommunikation zwischen dem Synchronisierungsmodul und Azure AD. Wenn TLS 1.2 auf dem zugrunde liegenden Betriebssystem nicht verfügbar ist, greift Azure AD Connect schrittweise auf älter Protokolle zurück (TLS 1.1 und TLS 1.0).
* Vor der Version 1.1.614.0 verwendet Azure AD Connect standardmäßig TLS 1.0 für die Verschlüsselung der Kommunikation zwischen dem Synchronisierungsmodul und Azure AD. Folgen Sie zur Änderung in TLS 1.2 den Schritten in [Aktivieren von TLS 1.2 für Azure AD Connect](#enable-tls-12-for-azure-ad-connect).
* Wenn Sie einen ausgehenden Proxy für die Verbindung mit dem Internet verwenden, muss die folgende Einstellung in der Datei **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** für den Installations-Assistenten und die Azure AD Connect-Synchronisierung hinzugefügt werden, um die Verbindung mit dem Internet und Azure AD zu ermöglichen. Dieser Text muss am Ende der Datei eingegeben werden. In diesem Codeabschnitt steht &lt;PROXYADDRESS&gt; für die tatsächliche Proxy-IP-Adresse oder den tatsächlichen Hostnamen.

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

* Wenn für den Proxyserver eine Authentifizierung erforderlich ist, muss sich das [Dienstkonto](reference-connect-accounts-permissions.md#adsync-service-account) in der Domäne befinden, und Sie müssen den Installationspfad aus den benutzerdefinierten Einstellungen zur Angabe eines [benutzerdefinierten Dienstkontos](how-to-connect-install-custom.md#install-required-components) verwenden. Außerdem müssen Sie in „machine.config“ eine weitere Änderung vornehmen. Durch diese Änderung in „machine.config“ antworten der Installations-Assistent und das Synchronisierungsmodul auf Authentifizierungsanfragen des Proxyservers. Auf allen Seiten des Installations-Assistenten mit Ausnahme der Seite **Konfigurieren** werden die Anmeldeinformationen des angemeldeten Benutzers verwendet. Auf der Seite **Konfigurieren** am Ende des Installations-Assistenten wird der Kontext in das von Ihnen erstellte [Dienstkonto](reference-connect-accounts-permissions.md#adsync-service-account) geändert. Im Abschnitt „machine.config“ sollte wie folgt aussehen.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

* Wenn Azure AD Connect im Rahmen der Verzeichnissynchronisierung eine Webanforderung an Azure AD sendet, kann es bis zu 5 Minuten dauern, bis Azure AD antwortet. In der Regel ist auf Proxyservern das Leerlauftimeout für Verbindungen konfiguriert. Stellen Sie sicher, dass die Konfiguration auf mindestens 6 Minuten festgelegt ist.

Weitere Informationen zum [defaultProxy](https://msdn.microsoft.com/library/kd3cf2ex.aspx)-Element finden Sie bei MSDN.  
Informationen zu Problemen mit der Konnektivität finden Sie unter [Beheben von Konnektivitätsproblemen](tshoot-connect-connectivity.md).

### <a name="other"></a>Andere
* Optional: Ein Testbenutzerkonto zum Überprüfen der Synchronisierung.

## <a name="component-prerequisites"></a>Voraussetzungen an Komponenten
### <a name="powershell-and-net-framework"></a>PowerShell und .NET Framework
Azure AD Connect ist abhängig von Microsoft PowerShell und .NET Framework 4.5.1. Auf dem Server muss diese Version oder eine neuere Version installiert sein. Führen Sie abhängig von Ihrer Windows Server-Version folgende Schritte aus:

* Windows Server 2012R2
  * Microsoft PowerShell ist standardmäßig installiert. Keine Aktion erforderlich.
  * .NET Framework 4.5.1 und neuere Versionen werden über Windows Update angeboten. Stellen Sie in der Systemsteuerung sicher, dass die neuesten Updates von Windows Server installiert sind.
* Windows Server 2012
  * Die neueste Version von Microsoft PowerShell steht im **Microsoft Download Center**unter [Windows Management Framework 4.0](https://www.microsoft.com/downloads)zur Verfügung.
  * .NET Framework 4.5.1 und neuere Versionen sind im [Microsoft Download Center](https://www.microsoft.com/downloads)verfügbar.


### <a name="enable-tls-12-for-azure-ad-connect"></a>Aktivieren von TLS 1.2 für Azure AD Connect
Vor der Version 1.1.614.0 verwendet Azure AD Connect standardmäßig TLS 1.0 für die Verschlüsselung der Kommunikation zwischen dem Server mit dem Synchronisierungsmodul und Azure AD. Sie können dies ändern, indem Sie Ihre .NET-Anwendungen auf dem Server für die standardmäßige Verwendung von TLS 1.2 konfigurieren. Weitere Informationen zu TLS 1.2 finden Sie in der [Microsoft-Sicherheitsempfehlung 2960358](https://technet.microsoft.com/security/advisory/2960358).

1.  Vergewissern Sie sich, dass der .NET 4.5.1-Hotfix für Ihr Betriebssystem installiert wurde. Informationen hierzu finden Sie in der [Microsoft-Sicherheitsempfehlung 2960358](https://technet.microsoft.com/security/advisory/2960358). Möglicherweise ist dieser Hotfix oder eine neuere Version bereits auf dem Server installiert.
    ```
2. For all operating systems, set this registry key and restart the server.
    ```
    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319 "SchUseStrongCrypto"=dword:00000001
    ```
4. If you also want to enable TLS 1.2 between the sync engine server and a remote SQL Server, then make sure you have the required versions installed for [TLS 1.2 support for Microsoft SQL Server](https://support.microsoft.com/kb/3135244).

## Prerequisites for federation installation and configuration
### Windows Remote Management
When using Azure AD Connect to deploy Active Directory Federation Services or the Web Application Proxy, check these requirements:

* If the target server is domain joined, then ensure that Windows Remote Managed is enabled
  * In an elevated PowerShell command window, use command `Enable-PSRemoting –force`
* If the target server is a non-domain joined WAP machine, then there are a couple of additional requirements
  * On the target machine (WAP machine):
    * Ensure the winrm (Windows Remote Management / WS-Management) service is running via the Services snap-in
    * In an elevated PowerShell command window, use command `Enable-PSRemoting –force`
  * On the machine on which the wizard is running (if the target machine is non-domain joined or untrusted domain):
    * In an elevated PowerShell command window, use the command `Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
    * In Server Manager:
      * add DMZ WAP host to machine pool (server manager -> Manage -> Add Servers...use DNS tab)
      * Server Manager All Servers tab: right click WAP server and choose Manage As..., enter local (not domain) creds for the WAP machine
      * To validate remote PowerShell connectivity, in the Server Manager All Servers tab: right click WAP server and choose Windows PowerShell. A remote PowerShell session should open to ensure remote PowerShell sessions can be established.

### TLS/SSL Certificate Requirements
* It's strongly recommended to use the same TLS/SSL certificate across all nodes of your AD FS farm and all Web Application proxy servers.
* The certificate must be an X509 certificate.
* You can use a self-signed certificate on federation servers in a test lab environment. However, for a production environment, we recommend that you obtain the certificate from a public CA.
  * If using a certificate that is not publicly trusted, ensure that the certificate installed on each Web Application Proxy server is trusted on both the local server and on all federation servers
* The identity of the certificate must match the federation service name (for example, sts.contoso.com).
  * The identity is either a subject alternative name (SAN) extension of type dNSName or, if there are no SAN entries, the subject name specified as a common name.  
  * Multiple SAN entries can be present in the certificate, provided one of them matches the federation service name.
  * If you are planning to use Workplace Join, an additional SAN is required with the value **enterpriseregistration.** followed by the User Principal Name (UPN) suffix of your organization, for example, **enterpriseregistration.contoso.com**.
* Certificates based on CryptoAPI next generation (CNG) keys and key storage providers are not supported. This means you must use a certificate based on a CSP (cryptographic service provider) and not a KSP (key storage provider).
* Wild-card certificates are supported.

### Name resolution for federation servers
* Set up DNS records for the AD FS federation service name (for example sts.contoso.com) for both the intranet (your internal DNS server) and the extranet (public DNS through your domain registrar). For the intranet DNS record, ensure that you use A records and not CNAME records. This is required for windows authentication to work correctly from your domain joined machine.
* If you are deploying more than one AD FS server or Web Application Proxy server, then ensure that you have configured your load balancer and that the DNS records for the AD FS federation service name (for example sts.contoso.com) point to the load balancer.
* For windows integrated authentication to work for browser applications using Internet Explorer in your intranet, ensure that the AD FS federation service name (for example sts.contoso.com) is added to the intranet zone in IE. This can be controlled via group policy and deployed to all your domain joined computers.

## Azure AD Connect supporting components
The following is a list of components that Azure AD Connect installs on the server where Azure AD Connect is installed. This list is for a basic Express installation. If you choose to use a different SQL Server on the Install synchronization services page, then SQL Express LocalDB is not installed locally.

* Azure AD Connect Health
* Microsoft SQL Server 2012 Command Line Utilities
* Microsoft SQL Server 2012 Express LocalDB
* Microsoft SQL Server 2012 Native Client
* Microsoft Visual C++ 2013 Redistribution Package

## Hardware requirements for Azure AD Connect
The table below shows the minimum requirements for the Azure AD Connect sync computer.

| Number of objects in Active Directory | CPU | Memory | Hard drive size |
| --- | --- | --- | --- |
| Fewer than 10,000 |1.6 GHz |4 GB |70 GB |
| 10,000–50,000 |1.6 GHz |4 GB |70 GB |
| 50,000–100,000 |1.6 GHz |16 GB |100 GB |
| For 100,000 or more objects the full version of SQL Server is required | | | |
| 100,000–300,000 |1.6 GHz |32 GB |300 GB |
| 300,000–600,000 |1.6 GHz |32 GB |450 GB |
| More than 600,000 |1.6 GHz |32 GB |500 GB |

The minimum requirements for computers running AD FS or Web Application Proxy Servers is the following:

* CPU: Dual core 1.6 GHz or higher
* MEMORY: 2 GB or higher
* Azure VM: A2 configuration or higher

## Next steps
Learn more about [Integrating your on-premises identities with Azure Active Directory](whatis-hybrid-identity.md).
