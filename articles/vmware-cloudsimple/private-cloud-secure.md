---
title: 'Azure VMware Solutions (AVS): Schützen einer privaten AVS-Cloud'
description: Informationen zum Schützen einer privaten Cloud in Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: b053f1493c2380153711176a4748ebf90b479a6c
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77020079"
---
# <a name="how-to-secure-your-avs-private-cloud-environment"></a>Schützen Ihrer privaten AVS-Cloudumgebung

Definieren Sie in Azure die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) für den AVS-Dienst, das AVS-Portal und die private AVS-Cloud. Benutzer, Gruppen und Rollen für den Zugriff auf vCenter in der privaten AVS-Cloud werden mithilfe von VMware-SSO angegeben. 

## <a name="rbac-for-avs-service"></a>RBAC für den AVS-Dienst

Die Erstellung des AVS-Diensts erfordert eine der Rollen **Besitzer** oder **Mitwirkender** im Azure-Abonnement. Standardmäßig können alle Besitzer und Mitwirkenden einen AVS-Dienst erstellen und auf das AVS-Portal zugreifen, um private AVS-Clouds zu erstellen und zu verwalten. Pro Region kann nur ein AVS-Dienst erstellt werden. Führen Sie die folgenden Schritte aus, um den Zugriff auf bestimmte Administratoren zu beschränken.

1. Erstellen eines AVS-Diensts in einer neuen **Ressourcengruppe** über das Azure-Portal
2. Geben Sie die RBAC für die Ressourcengruppe an.
3. Erwerben Sie Knoten, und verwenden Sie dieselbe Ressourcengruppe wie für den AVS-Dienst.

Nur Benutzer mit den Berechtigungen **Besitzer** oder **Mitwirkender** für die Ressourcengruppe können den AVS-Dienst anzeigen und das AVS-Portal aufrufen.

Weitere Informationen dazu finden Sie unter [Was ist die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) für Azure-Ressourcen?](../role-based-access-control/overview.md).

## <a name="rbac-for-avs-private-cloud-vcenter"></a>RBAC für vCenter in der privaten AVS-Cloud

Der Standardbenutzer `CloudOwner@AVS.local` wird in der vCenter-SSO-Domäne erstellt, wenn eine private AVS-Cloud erstellt wird. Der Benutzer „CloudOwner“ hat Berechtigungen zum Verwalten von vCenter. vCenter SSO werden zusätzliche Identitätsquellen hinzugefügt, um verschiedenen Benutzern Zugriff zu gewähren. In vCenter werden vordefinierte Rollen und Gruppen eingerichtet, mit deren Hilfe weitere Benutzer hinzugefügt werden können.

### <a name="add-new-users-to-vcenter"></a>Hinzufügen neuer Benutzer zu vCenter

1. [Erhöhen Sie Berechtigungen](escalate-private-cloud-privileges.md) für den Benutzer **CloudOwner@AVS.local** in der privaten AVS-Cloud.
2. Melden Sie sich bei vCenter mithilfe von **CloudOwner@AVS.local** an.
3. [Fügen Sie vCenter SSO-Benutzer hinzu](https://docs.vmware.com/en/VMware-vSphere/5.5/com.vmware.vsphere.security.doc/GUID-72BFF98C-C530-4C50-BF31-B5779D2A4BBB.html).
4. Fügen Sie Benutzer zu [SSO-Gruppen in vCenter](https://docs.vmware.com/en/VMware-vSphere/5.5/com.vmware.vsphere.security.doc/GUID-CDEA6F32-7581-4615-8572-E0B44C11D80D.html) hinzu.

Weitere Informationen zu vordefinierten Rollen und Gruppen finden Sie unter [Berechtigungsmodell der privaten AVS-Cloud von VMware vCenter](learn-private-cloud-permissions.md).

### <a name="add-new-identity-sources"></a>Hinzufügen neuer Identitätsquellen

Sie können der vCenter-SSO-Domäne Ihrer privaten AVS-Cloud zusätzliche Identitätsanbieter hinzufügen. Identitätsanbieter ermöglichen die Authentifizierung, und SSO-Gruppen von vCenter die Autorisierung der Benutzer.

* [Verwenden Sie Active Directory als Identitätsanbieter](set-vcenter-identity.md) für vCenter in der privaten AVS-Cloud.
* [Verwenden von Azure AD als Identitätsanbieter](azure-ad.md) für vCenter in der privaten AVS-Cloud

1. [Erhöhen Sie Berechtigungen](escalate-private-cloud-privileges.md) für den Benutzer **CloudOwner@AVS.local** in der privaten AVS-Cloud.
2. Melden Sie sich bei vCenter mithilfe von **CloudOwner@AVS.local** an.
3. Fügen Sie Benutzer aus dem Identitätsanbieter zu [SSO-Gruppen für vCenter](https://docs.vmware.com/en/VMware-vSphere/5.5/com.vmware.vsphere.security.doc/GUID-CDEA6F32-7581-4615-8572-E0B44C11D80D.html) hinzu.

## <a name="secure-network-on-your-avs-private-cloud-environment"></a>Schützen des Netzwerks in Ihrer privaten AVS-Cloudumgebung

Die Netzwerksicherheit der privaten AVS-Cloudumgebung wird durch die Absicherung des Netzwerkzugriffs und die Steuerung des Netzwerkdatenverkehrs zwischen Ressourcen gewährleistet.

### <a name="access-to-avs-private-cloud-resources"></a>Zugreifen auf Ressourcen in der privaten AVS-Cloud

Der Zugriff auf vCenter und Ressourcen in der privaten AVS-Cloud erfolgt über eine sichere Netzwerkverbindung:

* **[ExpressRoute-Verbindung](on-premises-connection.md)** . ExpressRoute bietet eine sichere, breitbandige und latenzarme Verbindung mit Ihrer lokalen Umgebung. Mithilfe der Verbindung können Ihre lokalen Dienste, Netzwerke und Benutzer auf vCenter in Ihrer privaten AVS-Cloud zugreifen.
* **[Site-to-Site-VPN-Gateway](vpn-gateway.md)** . Ein Site-to-Site-VPN ermöglicht den lokalen Zugriff auf die Ressourcen in Ihrer privaten AVS-Cloud über einen sicheren Tunnel. Sie legen fest, welche lokalen Netzwerke Datenverkehr an Ihre private AVS-Cloud senden und von dieser empfangen können.
* **[Point-to-Site-VPN Gateway](vpn-gateway.md#set-up-a-site-to-site-vpn-gateway)** . Verwenden Sie eine Point-to-Site-VPN-Verbindung für den schnellen Remotezugriff auf vCenter in Ihrer privaten AVS-Cloud.

### <a name="control-network-traffic-in-avs-private-cloud"></a>Steuern des Netzwerkdatenverkehrs in der privaten AVS-Cloud

Firewalltabellen und -regeln steuern den Netzwerkdatenverkehr in der privaten AVS-Cloud. Die Firewalltabelle ermöglicht die Steuerung des Netzwerkdatenverkehrs zwischen einem Quellnetzwerk oder einer IP-Adresse und einem Zielnetzwerk oder einer IP-Adresse basierend auf der in der Tabelle definierten Kombination von Regeln.

1. Erstellen Sie eine [Firewalltabelle](firewall.md#add-a-new-firewall-table).
2. Fügen Sie der [Firewalltabelle Regeln hinzu](firewall.md#create-a-firewall-rule).
3. [Anfügen einer Firewalltabelle an ein VLAN/Subnetz] ((firewall.md#attach-vlanssubnet)
