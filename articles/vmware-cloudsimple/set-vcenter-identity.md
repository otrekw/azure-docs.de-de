---
title: 'Azure VMware Solutions (AVS): Einrichten von vCenter-Identitätsquellen für die private AVS-Cloud'
description: Beschreibt, wie Sie für Ihre vCenter-Instanz Ihrer privaten AVS-Cloud die Authentifizierung mit Active Directory einrichten können, damit Ihre VMware-Administratoren auf vCenter zugreifen können
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: ad4a7b2bc67b7d50d9e9a5f8337a09dbe77366ea
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77014214"
---
# <a name="set-up-vcenter-identity-sources-to-use-active-directory"></a>Einrichten von vCenter-Identitätsquellen für die Verwendung von Active Directory

## <a name="about-vmware-vcenter-identity-sources"></a>Informationen zu VMware vCenter-Identitätsquellen

VMware vCenter unterstützt verschiedene Identitätsquellen für die Authentifizierung von Benutzern, die auf vCenter zugreifen. vCenter in Ihrer privaten AVS-Cloud kann für die Authentifizierung mit Active Directory eingerichtet werden, damit Ihre VMware-Administratoren auf vCenter zugreifen können. Wenn das Setup abgeschlossen ist, kann der **cloudowner**-Benutzer vCenter Benutzer aus der Identitätsquelle hinzufügen. 

Sie können Ihre Active Directory-Domäne und Domänencontroller auf eine der folgenden Arten einrichten:

* Lokal ausgeführte Active Directory-Domäne und -Domänencontroller
* In Azure als virtuelle Computer im Azure-Abonnement ausgeführte Active Directory-Domänen und -Domänencontroller
* Neue Active Directory-Domäne und -Domänencontroller, die in Ihrer privaten AVS-Cloud ausgeführt werden
* Azure Active Directory-Dienst

In diesem Leitfaden werden die Aufgaben zum Einrichten von Active Directory-Domänen- und -Domänencontrollern erläutert, die entweder lokal oder als virtuelle Computer in Ihren Abonnements ausgeführt werden. Wenn Sie Azure AD als Identitätsquelle verwenden möchten, finden Sie ausführliche Anweisungen zum Einrichten der Identitätsquelle unter [Verwenden von Azure AD als Identitätsanbieter für vCenter in der privaten AVS-Cloud](azure-ad.md).

Eskalieren Sie vor dem [Hinzufügen einer Identitätsquelle](#add-an-identity-source-on-vcenter) vorübergehend Ihre [vCenter-Berechtigungen](escalate-private-cloud-privileges.md).

> [!CAUTION]
> Neue Benutzer müssen lediglich *Cloud-Owner-Group*, *Cloud-Global-Cluster-Admin-Group*, *Cloud-Global-Storage-Admin-Group*, *Cloud-Global-Network-Admin-Group* oder *Cloud-Global-VM-Admin-Group* hinzugefügt werden.  Benutzer, die der Gruppe *Administratoren* hinzugefügt wurden, werden automatisch entfernt.  Nur Dienstkonten dürfen der Gruppe *Administratoren* hinzugefügt werden, und Dienstkonten dürfen nicht für die Anmeldung bei der vSphere-Webbenutzeroberfläche verwendet werden.   


## <a name="identity-source-options"></a>Optionen für Identitätsquellen

* [Hinzufügen eines lokalen Active Directory als SSO-Identitätsquelle](#add-on-premises-active-directory-as-a-single-sign-on-identity-source)
* [Einrichten eines neuen Active Directory für eine private AVS-Cloud](#set-up-new-active-directory-on-an-avs-private-cloud)
* [Einrichten von Active Directory in Azure](#set-up-active-directory-on-azure)

## <a name="add-on-premises-active-directory-as-a-single-sign-on-identity-source"></a>Hinzufügen eines lokalen Active Directory als SSO-Identitätsquelle

Um Ihr lokales Active Directory als SSO-Identitätsquelle einzurichten, benötigen Sie Folgendes:

* [Site-to-Site-VPN-Verbindung](vpn-gateway.md#set-up-a-site-to-site-vpn-gateway) zwischen Ihrem lokalen Rechenzentrum und Ihrer privaten AVS-Cloud.
* Die IP-Adresse des lokalen DNS-Servers wurde vCenter und PSC (Platform Services Controller) hinzugefügt.

Verwenden Sie die Informationen in der folgenden Tabelle, wenn Sie Ihre Active Directory-Domäne einrichten.

| **Option** | **Beschreibung** |
|------------|-----------------|
| **Name** | Der Name der Identitätsquelle. |
| **Basis-DN für Benutzer** | Der Distinguished Basis-Name für Benutzer. |
| **Domänenname** | Der FDQN der Domäne, z.B. „example.com“. Geben Sie in diesem Textfeld keine IP-Adresse an. |
| **Domänenalias** | Der NetBIOS-Name der Domäne. Fügen Sie den NetBIOS-Namen der Active Directory-Domäne als Alias der Identitätsquelle hinzu, wenn Sie SSPI-Authentifizierung verwenden. |
| **Basis-DN für Gruppen** | Der Distinguished Basis-Name für Gruppen. |
| **URL des primären Servers** | LDAP-Server des primären Domänencontrollers für die Domäne.<br><br>Verwenden Sie das Format  `ldap://hostname:port`  oder  `ldaps://hostname:port`. Der Port ist in der Regel 389 für LDAP-Verbindungen und 636 für LDAPS-Verbindungen. Für Active Directory-Bereitstellungen mit mehreren Domänencontrollern ist der Port in der Regel 3268 für LDAP und 3269 für LDAPS.<br><br>Ein Zertifikat, das eine Vertrauensstellung für den LDAPS-Endpunkt des Active Directory-Servers einrichtet, ist erforderlich, wenn Sie  `ldaps://`  in der primären oder sekundären LDAP-URL verwenden. |
| **URL des sekundären Servers** | Die Adresse eines LDAP-Servers für einen sekundären Domänencontroller, der für Failover verwendet wird. |
| **Zertifikat auswählen** | Wenn Sie LDAPS mit dem Active Directory LDAP-Server oder der OpenLDAP-Server-Identitätsquelle verwenden möchten, wird eine Schaltfläche „Zertifikat auswählen“ nach dem Eingeben von  `ldaps://`  in das URL-Textfeld angezeigt. Eine sekundäre URL ist nicht erforderlich. |
| **Benutzername** | Die ID eines Benutzers in der Domäne, der mindestens über Lesezugriff auf den Basis-DN für Benutzer und Gruppen verfügt. |
| **Kennwort** | Das Kennwort des Benutzers, der durch „Benutzername“ angegeben wird. |

Wenn Sie über die Informationen aus der Tabelle oben verfügen, können Sie Ihr lokales Active Directory als SSO-Identitätsquelle (einmaliges Anmelden) in vCenter hinzufügen.

> [!TIP]
> Weitere Informationen zu SSO-Identitätsquellen finden Sie auf der [VMware-Dokumentationsseite](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.psc.doc/GUID-B23B1360-8838-4FF2-B074-71643C4CB040.html).

## <a name="set-up-new-active-directory-on-an-avs-private-cloud"></a>Einrichten eines neuen Active Directory für eine private AVS-Cloud

Sie können eine neue Active Directory-Domäne für Ihre private AVS-Cloud einrichten und als SSO-Identitätsquelle (einmaliges Anmelden) verwenden. Die Active Directory-Domäne kann Teil einer vorhandenen Active Directory-Gesamtstruktur sein oder als unabhängige Gesamtstruktur eingerichtet werden.

### <a name="new-active-directory-forest-and-domain"></a>Neue Active Directory-Gesamtstruktur und -Domäne

Zum Einrichten einer neuen Active Directory-Gesamtstruktur und -Domäne benötigen Sie Folgendes:

* Mindestens einen virtueller Computer, auf dem Microsoft Windows Server ausgeführt wird und der als Domänencontroller für die neue Active Directory-Gesamtstruktur und -Domäne verwendet werden soll.
* Mindestens ein virtueller Computer, auf dem der DNS-Dienst für Namensauflösung ausgeführt wird.

Ausführliche Schritte finden Sie unter [Installieren einer neuen Windows Server 2012 Active Directory-Gesamtstruktur](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/install-a-new-windows-server-2012-active-directory-forest--level-200-).

> [!TIP]
> Für Hochverfügbarkeit von Diensten empfiehlt es sich, mehrere Domänencontroller und DNS-Server einzurichten.

Nachdem Sie die Active Directory-Gesamtstruktur und die Domäne eingerichtet haben, können Sie [eine Identitätsquelle für vCenter](#add-an-identity-source-on-vcenter) für Ihr neues Active Directory hinzufügen.

### <a name="new-active-directory-domain-in-an-existing-active-directory-forest"></a>Neue Active Directory-Domäne in einer vorhandenen Active Directory-Gesamtstruktur

Um eine neue Active Directory-Domäne in einer vorhandenen Active Directory-Gesamtstruktur einzurichten, benötigen Sie Folgendes:

* Site-to-Site-VPN-Verbindung mit dem Standort Ihrer Active Directory-Gesamtstruktur.
* DNS-Server, um den Namen der vorhandenen Active Directory-Gesamtstruktur aufzulösen.

Ausführliche Schritte finden Sie unter [Installieren einer neuen untergeordneten Windows Server 2012-Active Directory-Domäne oder -Strukturdomäne](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/install-a-new-windows-server-2012-active-directory-child-or-tree-domain--level-200-).

Nachdem Sie die Active Directory-Domäne eingerichtet haben, können Sie [eine Identitätsquelle für vCenter](#add-an-identity-source-on-vcenter) für Ihr neues Active Directory hinzufügen.

## <a name="set-up-active-directory-on-azure"></a>Einrichten von Active Directory in Azure

Active Directory in Azure ähnelt der lokalen Ausführung von Active Directory. Um Active Directory in Azure als SSO-Identitätsquelle (einmaliges Anmelden) für vCenter einzurichten, müssen der vCenter-Server und der PSC über eine Netzwerkverbindung mit dem Azure Virtual Network verfügen, in dem Active Directory-Dienste ausgeführt werden. Sie können diese Verbindung über eine [Azure Virtual Network-Verbindung unter Verwendung von ExpressRoute](azure-expressroute-connection.md) aus dem virtuellen Azure-Netzwerk herstellen, in dem Active Directory-Dienste in der privaten AVS-Cloud ausgeführt werden.

Nachdem die Netzwerkverbindung hergestellt wurde, führen Sie die Schritte unter [Hinzufügen eines lokalen Active Directory als SSO-Identitätsquelle](#add-on-premises-active-directory-as-a-single-sign-on-identity-source) aus, um es als Identitätsquelle hinzuzufügen. 

## <a name="add-an-identity-source-on-vcenter"></a>Hinzufügen einer Identitätsquelle für vCenter

1. [Eskalieren Sie die Berechtigungen](escalate-private-cloud-privileges.md) für Ihre private AVS-Cloud.

2. Melden Sie sich bei vCenter für Ihre private AVS-Cloud an.

3. Wählen **Sie Home > Verwaltung** aus.

    ![Verwaltung](media/OnPremAD01.png)

4. Wählen Sie **Einmaliges Anmelden > Konfiguration** aus.

    ![Einmaliges Anmelden](media/OnPremAD02.png)

5. Öffnen Sie die Registerkarte **Identitätsquellen**, und klicken Sie auf **+** , um eine neue Identitätsquelle hinzuzufügen.

    ![Identitätsquellen](media/OnPremAD03.png)

6. Wählen Sie **Active Directory als LDAP-Server** aus, und klicken Sie auf **Weiter**.

    ![Active Directory](media/OnPremAD04.png)

7. Geben Sie die Identitätsquellenparameter für Ihre Umgebung aus, und klicken Sie auf **Weiter**.

    ![Active Directory](media/OnPremAD05.png)

8. Überprüfen Sie die Einstellungen, und klicken Sie auf **Fertig stellen**.
