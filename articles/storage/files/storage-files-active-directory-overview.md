---
title: 'Übersicht: identitätsbasierte Autorisierung mit Azure Files'
description: Azure Files unterstützt die identitätsbasierte Authentifizierung mit SMB (Server Message Block) über Azure Active Directory Domain Services (AD DS) und Active Directory. Ihre in die Domäne eingebundenen virtuellen Windows-Computer (VMs) können dann mit Azure AD-Anmeldeinformationen auf Azure-Dateifreigaben zugreifen.
author: roygara
ms.service: storage
ms.subservice: files
ms.topic: conceptual
ms.date: 02/21/2020
ms.author: rogarana
ms.openlocfilehash: 737cdfaddca3a5f7532620bdafd86149e4d61f9f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061071"
---
# <a name="overview-of-azure-files-identity-based-authentication-support-for-smb-access"></a>Übersicht über die Unterstützung der identitätsbasierten Authentifizierung mit Azure Files für den SMB-Zugriff
[!INCLUDE [storage-files-aad-auth-include](../../../includes/storage-files-aad-auth-include.md)]

Informationen zum Aktivieren der AD-Authentifizierung für Azure-Dateifreigaben finden Sie unter [Aktivieren der Active Directory-Authentifizierung über SMB für Azure-Dateifreigaben](storage-files-identity-auth-active-directory-enable.md).

Informationen zum Aktivieren der Azure AD DS-Authentifizierung für Azure-Dateifreigaben finden Sie unter [Aktivieren der Azure Active Directory Domain Services-Authentifizierung in Azure Files](storage-files-identity-auth-active-directory-domain-service-enable.md).

## <a name="glossary"></a>Glossar 
Sie sollten die folgenden Begriffe im Zusammenhang mit der Azure AD Domain Services-Authentifizierung über SMB für Azure-Dateifreigaben kennen:

-   **Kerberos-Authentifizierung**

    Kerberos ist ein Authentifizierungsprotokoll, das zum Überprüfen der Identität eines Benutzers oder Hosts verwendet wird. Weitere Informationen zu Kerberos finden Sie unter [Kerberos-Authentifizierung: Übersicht](https://docs.microsoft.com/windows-server/security/kerberos/kerberos-authentication-overview).

-  **Server Message Block-Protokoll (SMB)**

    SMB ist ein Netzwerkprotokoll zur Dateifreigabe nach Industriestandard. SMB ist auch bekannt als Common Internet File System oder CIFS. Weitere Informationen zu SMB finden Sie unter [Microsoft SMB-Protokoll und CIFS-Protokoll (Übersicht)](https://docs.microsoft.com/windows/desktop/FileIO/microsoft-smb-protocol-and-cifs-protocol-overview).

-   **Azure Active Directory (Azure AD)**

    Azure Active Directory (Azure AD) ist der mehrinstanzenfähige cloudbasierte Verzeichnis- und Identitätsverwaltungsdienst von Microsoft. Azure AD kombiniert grundlegende Verwaltungsdienste, Zugriffsverwaltung für Anwendungen und Identitätsgovernance in einer einzigen Lösung. Dank Azure AD können Ihre in die Domäne eingebundenen virtuellen Windows-Computer (VMs) mithilfe Ihrer Azure AD-Anmeldeinformationen auf Azure-Dateifreigaben zugreifen. Weitere Informationen finden Sie unter [Was ist Azure Active Directory?](../../active-directory/fundamentals/active-directory-whatis.md).

-   **Azure AD Domain Services (Azure AD DS)**

    Azure AD Domain Services (allgemeine Verfügbarkeit) stellen verwaltete Domänendienste bereit, z. B. Domänenbeitritt, Gruppenrichtlinie, LDAP und Kerberos-/NTLM-Authentifizierung. Diese Dienste sind vollständig kompatibel mit Windows Server Active Directory. Weitere Informationen finden Sie unter [Azure Active Directory-Domänendienste (AD)](../../active-directory-domain-services/overview.md).

- **Active Directory Domain Services (AD DS, auch als AD bezeichnet)**

    Active Directory (AD) (Vorschauversion) stellt die Methoden zum Speichern von Verzeichnisdaten bereit und macht diese Daten für Netzwerkbenutzer und Administratoren verfügbar. Sicherheit ist in Active Directory über die Anmeldeauthentifizierung und die Zugriffssteuerung für Objekte im Verzeichnis integriert. Mit einer einzigen Netzwerkanmeldung können Administratoren Verzeichnisdaten und die Organisation im gesamten Netzwerk verwalten, und autorisierte Netzwerkbenutzer können überall im Netzwerk auf Ressourcen zugreifen. AD wird häufig von Unternehmen lokal eingeführt und in Form der AD-Anmeldeinformationen als Identität für die Zugriffssteuerung verwendet. Weitere Informationen finden Sie unter [Active Directory Domain Services – Übersicht](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview).

-   **Rollenbasierte Zugriffssteuerung in Azure (Role-Based Access Control, RBAC)**

    Die rollenbasierte Access Control in Azure (RBAC) ermöglicht eine präzise Zugriffsverwaltung für Azure. Mit RBAC können Sie den Zugriff auf Ressourcen verwalten, indem Sie Benutzern die Mindestberechtigungen zum Ausführen ihrer Aufgaben erteilen. Weitere Informationen zu rollenbasierter Zugriffssteuerung finden Sie unter [Was ist die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) in Azure?](../../role-based-access-control/overview.md).

## <a name="common-use-cases"></a>Gängige Anwendungsfälle

Die identitätsbasierte Authentifizierung und Unterstützung für Windows-ACLs in Azure Files eignet sich besonders für die folgenden Anwendungsfälle:

### <a name="replace-on-premises-file-servers"></a>Ersatz für lokale Dateiserver

Das als veraltet Markieren und Ersetzen weit verteilter lokaler Dateiserver ist ein häufiges Problem, auf das jedes Unternehmen bei der IT-Modernisierung stößt. Die Authentifizierung von Azure-Dateifreigaben mit AD (Vorschauversion) ist hierfür am besten geeignet, sofern Sie die Daten zu Azure Files migrieren können. Eine komplette Migration ermöglicht Ihnen, die Vorteile der Hochverfügbarkeit und Skalierbarkeit zu nutzen und gleichzeitig clientseitige Änderungen zu minimieren, insbesondere bei einer komplizierten AD-Domäneninfrastruktur. Sie bietet eine nahtlose Migration für Endbenutzer, sodass sie weiterhin mit denselben Anmeldeinformationen und über ihre vorhandenen, in die Domäne eingebundenen Computer auf ihre Daten zugreifen können.

### <a name="lift-and-shift-applications-to-azure"></a>Lift & Shift von Anwendungen in Azure

Wenn Sie Anwendungen per Lift & Shift in die Cloud verlagern, soll das Authentifizierungsmodell für Ihre Daten beibehalten werden. Da wir die identitätsbasierte Zugriffssteuerung auf Azure-Dateifreigaben ausweiten, entfällt die Notwendigkeit, Ihre Anwendung auf moderne Authentifizierungsmethoden umzustellen und die Cloudeinführung zu beschleunigen. Azure-Dateifreigaben bieten die Möglichkeit, Azure AD DS (allgemein verfügbar) oder AD (Vorschauversion) für die Authentifizierung zu integrieren. Wenn Sie vorhaben, vollumfänglich cloudnativ zu werden, und minimalen Aufwand für die Verwaltung von Cloudinfrastrukturen betreiben möchten, ist Azure AD DS als vollständig verwalteter Domänendienst besser geeignet. Wenn Sie vollständige Kompatibilität mit den AD DS-Funktionen (allgemein verfügbar) benötigen, sollten Sie in Erwägung ziehen, Ihre AD-Umgebung durch eigenes Hosting von Domänencontrollern auf VMs in die Cloud zu erweitern. In beiden Fällen bieten wir Ihnen die Flexibilität, die Domänendienste entsprechend Ihren Geschäftsanforderungen auszuwählen.

### <a name="backup-and-disaster-recovery-dr"></a>Sicherung und Notfallwiederherstellung

Wenn Sie Ihren primären Dateispeicher lokal betreiben, können Azure-Dateifreigaben einen idealen Speicher für die Sicherung oder Notfallwiederherstellung darstellen, um die Geschäftskontinuität zu verbessern. Sie können Azure-Dateifreigaben verwenden, um Ihre Daten von vorhandenen Dateiservern zu sichern und dabei Windows-DACLs beizubehalten. Für Notfallwiederherstellungsszenarien können Sie eine Authentifizierungsoption konfigurieren, um bei einem Failover die ordnungsgemäße Durchsetzung der Zugriffssteuerung zu unterstützen.

## <a name="supported-scenarios"></a>Unterstützte Szenarios

In der folgenden Tabelle werden die unterstützten Authentifizierungsszenarien für Azure-Dateifreigaben für Azure AD DS (allgemein verfügbar) und AD (Vorschauversion) zusammengefasst. Es wird empfohlen, den Domänendienst auszuwählen, den Sie für Ihre Clientumgebung für die Integration mit Azure Files eingeführt haben. Wenn Sie AD (Vorschauversion) bereits lokal oder in Azure eingerichtet haben und Ihre Geräte in die AD-Domäne eingebunden sind, sollten Sie sich für die Authentifizierung von Azure-Dateifreigaben mit AD (Vorschauversion) entscheiden. Wenn Sie Azure AD DS (allgemein verfügbar) bereits eingeführt haben, sollten Sie dies auch für die Authentifizierung von Azure-Dateifreigaben verwenden.


|Authentifizierung mit Azure AD DS (allgemein verfügbar)  |Authentifizierung mit AD (Vorschauversion)  |
|---------|---------|
|Ihre mit Azure AD DS in eine Domäne eingebundenen Windows-Computer können mit Azure AD-Anmeldeinformationen über SMB auf Azure-Dateifreigaben zugreifen.     |Mit AD in eine Domäne eingebundene Windows-Computer können mit AD-Anmeldeinformationen, die über SMB mit Azure AD synchronisiert werden, auf Azure-Dateifreigaben zugreifen.         |

### <a name="unsupported-scenarios"></a>Nicht unterstützte Szenarien

- Für die Authentifizierung mit Azure AD DS (allgemein verfügbar) und AD (Vorschauversion) werden keine Computerkonten unterstützt. Stattdessen können Sie die Verwendung eines Dienstanmeldekontos in Erwägung ziehen.
- Für die Authentifizierung mit Azure AD DS (allgemein verfügbar) und AD (Vorschauversion) werden keine über Azure AD mit der Cloud verbundenen Geräte unterstützt.

## <a name="advantages-of-identity-based-authentication"></a>Vorteile der identitätsbasierten Authentifizierung
Die identitätsbasierte Authentifizierung für Azure Files bietet mehrere Vorteile gegenüber der Authentifizierung mit einem gemeinsam verwendeten Schlüssel:

-   **Erweitern des herkömmlichen Zugriffs auf Dateifreigaben auf Identitätsbasis in die Cloud mit AD und Azure AD DS**  
    Wenn Sie Ihre Anwendung per Lift & Shift in die Cloud migrieren und dabei herkömmliche Dateiserver durch Azure-Dateifreigaben ersetzen möchten, sollten Sie Ihre Anwendung mit AD- oder Azure AD DS-Anmeldeinformationen für den Zugriff auf Dateidaten authentifizieren. Azure Files unterstützt sowohl AD- als auch Azure AD DS-Anmeldeinformationen für den Zugriff auf Azure-Dateifreigaben über SMB von VMs, die in eine AD- oder Azure AD DS-Domäne eingebunden sind.

-   **Erzwingen einer differenzierten Zugriffssteuerung auf Azure-Dateifreigaben**  
    Sie können einer bestimmten Identität Berechtigungen auf der Freigabe-, Verzeichnis- oder Dateiebene zuweisen. Nehmen wir beispielsweise an, dass Sie über mehrere Teams verfügen, die eine einzelne Azure-Dateifreigabe für die Projektzusammenarbeit verwenden. Sie können allen Teams Zugriff auf nicht vertrauliche Verzeichnisse erteilen und dabei den Zugriff auf Verzeichnisse, die vertrauliche finanzielle Daten enthalten, auf Ihr Finanzteam beschränken. 

-   **Sichern von Windows-ACLs (auch als NTFS bezeichnet) zusammen mit Ihren Daten**  
    Sie können Azure-Dateifreigaben verwenden, um Ihre vorhandenen lokalen Dateifreigaben zu sichern. Azure Files behält Ihre ACLs zusammen mit Ihren Daten bei, wenn Sie eine Dateifreigabe auf Azure-Dateifreigaben über SMB sichern.

## <a name="how-it-works"></a>Funktionsweise
Azure-Dateifreigaben unterstützen die Kerberos-Authentifizierung für die Integration mit Azure AD DS (allgemein verfügbar) oder AD (Vorschauversion). Bevor Sie die Authentifizierung für Azure-Dateifreigaben aktivieren können, müssen Sie zuerst Ihre Domänenumgebung einrichten. Bei der Azure AD DS-Authentifizierung (allgemein verfügbar) sollten Sie Azure AD Domain Services aktivieren und die VMs, von denen aus auf Dateidaten zugegriffen werden soll, in die Domäne einbinden. Ihre in die Domäne eingebundene VM muss sich im selben virtuellen Netzwerk (VNET) wie Azure AD Domain Services befinden. Für die AD-Authentifizierung (Vorschauversion) müssen Sie entsprechend Ihre Active Directory-Domänencontroller einrichten und Ihre Computer oder VMs in die Domäne einbinden.

Wenn eine Identität, die mit einer auf einer VM ausgeführten Anwendung verknüpft ist, versucht, auf Daten in Azure-Dateifreigaben zuzugreifen, wird die Anforderung zur Authentifizierung der Identität an Azure AD Domain Services gesendet. Wenn die Authentifizierung erfolgreich ist, gibt Azure AD Domain Services ein Kerberos-Token zurück. Die Anwendung sendet eine Anforderung mit dem Kerberos-Token, und dieses Token wird von den Azure-Dateifreigaben zur Autorisierung der Anforderung verwendet. Die Azure-Dateifreigaben empfangen nur die Token und behalten die Azure AD-Anmeldeinformationen nicht bei. Die AD-Authentifizierung funktioniert auf ähnliche Weise, wobei AD das Kerberos-Token bereitstellt.

![Screenshot mit Diagramm der Azure AD-Authentifizierung über SMB](media/storage-files-active-directory-overview/azure-active-directory-over-smb-for-files-overview.png)

### <a name="enable-identity-based-authentication"></a>Aktivieren der identitätsbasierten Authentifizierung

Sie können die identitätsbasierte Authentifizierung mit Azure AD DS (allgemein verfügbar) oder AD (Vorschauversion) für Azure-Dateifreigaben in Ihren neuen und vorhandenen Speicherkonten aktivieren. Nur ein Domänendienst kann für die Authentifizierung des Dateizugriffs im Speicherkonto verwendet werden. Dies betrifft dann alle Dateifreigaben im Konto. Eine ausführliche Anleitung zum Einrichten Ihrer Dateifreigaben für die Authentifizierung mit Azure AD DS (allgemein verfügbar) finden Sie in unserem Artikel [Aktivieren der Azure Active Directory Domain Services-Authentifizierung über Azure Files](storage-files-identity-auth-active-directory-domain-service-enable.md), und eine Anleitung für AD (Vorschauversion) finden Sie in unserem Artikel [Aktivieren der Active Directory-Authentifizierung über SMB für Azure-Dateifreigaben](storage-files-identity-auth-active-directory-enable.md).

### <a name="configure-share-level-permissions-for-azure-files"></a>Konfigurieren von Freigabeberechtigungen für Azure Files

Sobald die Authentifizierung mit Azure AD DS (allgemein verfügbar) oder AD (Vorschauversion) aktiviert ist, können Sie integrierte RBAC-Rollen für Azure AD-Identitäten verwenden oder benutzerdefinierte Rollen konfigurieren und allen Dateifreigaben im Speicherkonto Zugriffsrechte zuweisen. Mit der zugewiesenen Berechtigung kann die gewährte Identität ausschließlich auf die Freigabe zugreifen, aber nicht auf andere Elemente – auch nicht auf das Stammverzeichnis. Sie müssen immer noch separat Berechtigungen auf Verzeichnis- oder Dateiebene für Azure-Dateifreigaben konfigurieren.

### <a name="configure-directory-or-file-level-permissions-for-azure-files"></a>Konfigurieren von Berechtigungen auf Verzeichnis- oder Dateiebene für Azure Files

Azure-Dateifreigaben erzwingen Windows-Standarddateiberechtigungen sowohl auf Verzeichnis- als auch auf Dateiebene, einschließlich des Stammverzeichnisses. Die Konfiguration von Berechtigungen auf Verzeichnis- oder Dateiebene wird über SMB und REST unterstützt. Binden Sie die gewünschte Dateifreigabe von Ihrer VM aus ein, und konfigurieren Sie Berechtigungen mit dem Datei-Explorer unter Windows mit Windows-[icacls](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls) oder mit dem Befehl [Set-ACL](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-acl?view=powershell-6).

### <a name="use-the-storage-account-key-for-superuser-permissions"></a>Verwenden des Speicherkontoschlüssels für Superuserberechtigungen

Ein Benutzer, der den Speicherkontoschlüssel besitzt, kann mit Superuserberechtigungen auf Azure-Dateifreigaben zugreifen. Superuserberechtigungen umgehen alle Einschränkungen der Zugriffssteuerung.

> [!IMPORTANT]
> Die empfohlene bewährte Sicherheitsmaßnahme besteht darin, Ihre Speicherkontoschlüssel nicht weiterzugeben und nach Möglichkeit die identitätsbasierte Authentifizierung zu nutzen.

### <a name="preserve-directory-and-file-acls-when-importing-data-to-azure-file-shares"></a>Beibehalten von Verzeichnis- und Datei-Zugriffssteuerungslisten beim Importieren von Daten in Azure-Dateifreigaben

Azure Files unterstützt das Beibehalten von Verzeichnis- oder Datei-Zugriffssteuerungslisten beim Kopieren von Daten in Azure-Dateifreigaben. Sie können Zugriffssteuerungslisten für ein Verzeichnis oder eine Datei mit der Azure-Dateisynchronisierung oder gängigen Dateiverschiebungs-Toolsets in Azure-Dateifreigaben kopieren. Beispielsweise können Sie [robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) mit dem Flag `/copy:s` verwenden, um sowohl die Daten als auch die Zugriffssteuerungslisten in eine Azure-Dateifreigabe zu kopieren. Zugriffssteuerungslisten werden standardmäßig beibehalten. Sie müssen die identitätsbasierte Authentifizierung für Ihr Speicherkonto dazu nicht aktivieren.

## <a name="pricing"></a>Preise
Für das Aktivieren der identitätsbasierten Authentifizierung über SMB in Ihrem Speicherkonto fällt keine zusätzliche Servicegebühr an. Weitere Informationen zu den Preisen finden Sie auf den Seiten [Azure Files – Preise](https://azure.microsoft.com/pricing/details/storage/files/) und [Azure Active Directory Domain Services – Preise ](https://azure.microsoft.com/pricing/details/active-directory-ds/).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Azure Files und zur identitätsbasierten Authentifizierung über SMB finden Sie in diesen Ressourcen:

- [Planung für eine Azure Files-Bereitstellung](storage-files-planning.md)
- [Aktivieren der Active Directory-Authentifizierung über SMB für Azure-Dateifreigaben](storage-files-identity-auth-active-directory-enable.md)
- [Aktivieren der Azure Active Directory Domain Services-Authentifizierung in Azure Files](storage-files-identity-auth-active-directory-domain-service-enable.md)
- [Häufig gestellte Fragen](storage-files-faq.md)
