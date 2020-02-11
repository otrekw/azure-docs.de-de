---
title: 'Azure VMware Solution (AVS): Verwenden der Private Cloud-Website von AVS zum Hosten einer virtuellen Desktopinfrastruktur mithilfe von VMware Horizon'
description: Erfahren Sie, wie Sie die Private Cloud-Website von AVS zum Hosten einer virtuellen Desktopinfrastruktur mithilfe von VMware Horizon verwenden.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: f909ce297344e80ed6004631d5218d6a7fcd5085
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025247"
---
# <a name="use-avs-private-cloud-site-to-host-a-virtual-desktop-infrastructure-using-vmware-horizon"></a>Verwenden der Private Cloud-Website von AVS zum Hosten einer virtuellen Desktopinfrastruktur mithilfe von VMware Horizon

Sie können die Private Cloud-Website von AVS zum Hosten einer virtuellen Desktopinfrastruktur (VDI) mithilfe von VMware Horizon 7.x verwenden. Die folgende Abbildung zeigt die logische Lösungsarchitektur der VDI.

![Horizon-Bereitstellung](media/horizon-deployment.png)

Mit dieser Lösung haben Sie die vollständige Kontrolle über den Horizon View-Manager und App Volumes. Die vertraute Benutzeroberfläche, API und CLI ermöglichen die Verwendung der vorhandenen Skripts und Tools.

Die AVS-Lösung erfordert, dass Sie wie folgt vorgehen:

* Installieren, Konfigurieren und Verwalten von VMware Horizon 7.x in Ihrer privaten AVS-Cloud
* Bereitstellen eigener Horizon-Lizenzen

## <a name="deploy-the-solution"></a>Bereitstellen der Lösung

In den folgenden Abschnitten wird beschrieben, wie Sie mit Horizon eine VDI-Lösung in Ihrer privaten AVS-Cloud bereitstellen.

1. [Überprüfen, ob die VMware-Produktversionen kompatibel sind](#verify-that-vmware-product-versions-are-compatible)
2. [Schätzen der Größe der Desktopumgebung](#estimate-the-size-of-your-desktop-environment)
3. [Erstellen einer privaten AVS-Cloud für Ihre Umgebung](#create-an-avs-private-cloud-for-your-environment)
4. [Installieren von VMware Horizon in Ihrer privaten AVS-Cloud](#install-vmware-horizon-in-your-avs-private-cloud)

### <a name="verify-that-vmware-product-versions-are-compatible"></a>Überprüfen, ob die VMware-Produktversionen kompatibel sind

* Vergewissern Sie sich, dass Ihre aktuellen und geplanten Versionen von Horizon, App Volumes, Unified Access Gateway und User Environment Manager miteinander und mit vCenter und PSC in der privaten AVS-Cloud kompatibel sind. Informationen zur Kompatibilität finden Sie unter [VMware Product Interoperability Matrices](https://www.vmware.com/resources/compatibility/sim/interop_matrix.php#interop&260=2877&0=) (VMware-Produktinteroperabilitätsmatrix).
* Um die aktuellen Versionen von vCenter und PSC in Ihrer privaten AVS-Cloud zu ermitteln, wechseln Sie im **AVS-Portal** zu [Resources](access-cloudsimple-portal.md) (Ressourcen), wählen Ihre private AVS-Cloud aus und klicken dann auf die Registerkarte **vSphere Management Network** (vSphere-Verwaltungsnetzwerk).

![vCenter- und PSC-Versionen](media/private-cloud-vsphere-versions.png)

### <a name="estimate-the-size-of-your-desktop-environment"></a>Schätzen der Größe der Desktopumgebung

* Überprüfen Sie, ob die ermittelte Konfiguration innerhalb der VMware-Betriebslimits liegt.
* Schätzen Sie die Ressourcen, die für alle Ihre Desktops und ihre Horizont-Verwaltungskomponenten benötigt werden.

### <a name="create-an-avs-private-cloud-for-your-environment"></a>Erstellen einer privaten AVS-Cloud für Ihre Umgebung

1. Erstellen Sie über das AVS-Portal eine private AVS-Cloud, indem Sie die Anweisungen unter [Erstellen einer privaten AVS-Cloudumgebung](quickstart-create-private-cloud.md) befolgen. AVS erstellt in jeder neu erstellten privaten AVS-Cloud einen vCenter-Standardbenutzer mit dem Namen „cloudowner“. Ausführliche Informationen zum Standardbenutzer und Berechtigungsmodell für die private AVS-Cloud finden Sie unter [Kennenlernen des Berechtigungsmodells für private AVS-Clouds](learn-private-cloud-permissions.md).
2. Erstellen Sie ein VLAN in der privaten AVS-Cloud für die Verwaltungsebene von Horizon, und weisen Sie dafür einen Subnetz-CIDR-Wert zu. Anweisungen dazu finden Sie unter [Erstellen und Verwalten von VLANs/Subnetzen](create-vlan-subnet.md). Dabei handelt es sich um das Netzwerk, in dem alle Lösungskomponenten (Unified Access Gateway, Connection Server, App Volume Server und User Environment Manager Server) installiert werden.
3. Entscheiden Sie, ob Sie in Ihrer privaten vCenter-AVS-Cloud einen externen Identitätsanbieter verwenden möchten. Wenn ja, wählen Sie eine der folgenden Optionen aus:
    * Verwenden Sie das lokale Active Directory als externen Identitätsanbieter. Anweisungen hierzu finden Sie unter [vCenter-Identitätsquellen](set-vcenter-identity.md).
    * Richten Sie einen Active Directory-Server in der privaten AVS-Cloud im VLAN der Verwaltungsebene von Horizon ein, den Sie als externen Identitätsanbieter verwenden möchten. Anweisungen hierzu finden Sie unter [vCenter-Identitätsquellen](set-vcenter-identity.md).
    * Richten Sie einen DHCP- und DNS-Server im VLAN der Verwaltungsebene von Horizon in der privaten AVS-Cloud ein. Anweisungen hierzu finden Sie unter [Einrichten von DNS- und DHCP-Anwendungen und -Workloads in der privaten AVS-Cloud](dns-dhcp-setup.md).
4. Konfigurieren Sie die DNS-Weiterleitung auf dem DNS-Server, der in der privaten AVS-Cloud installiert ist. Anweisungen hierzu finden Sie unter [Erstellen einer bedingten Weiterleitung](on-premises-dns-setup.md#create-a-conditional-forwarder).

### <a name="install-vmware-horizon-in-your-avs-private-cloud"></a>Installieren von VMware Horizon in Ihrer privaten AVS-Cloud

Das folgende Bereitstellungsdiagramm zeigt eine in einer privaten AVS-Cloud bereitgestellte Horizon-Lösung. Unified Access Gateway, AD/DC, View und App Volume Server werden im vom Benutzer erstellten VLAN 234 installiert. Unified Access Gateway verfügt über eine zugewiesene öffentliche IP-Adresse, die über das Internet erreichbar ist. Die virtuellen Computer des Desktoppools von Horizon werden in VLAN 235 bereitgestellt, um zusätzliche Isolation und Sicherheit zu gewährleisten.

![Horizon-Bereitstellung in der privaten AVS-Cloud](media/horizon-private-cloud.png)

In den folgenden Abschnitten werden die Anweisungen zum Einrichten einer Bereitstellung beschrieben, die der in der Abbildung dargestellten ähnelt. Vergewissern Sie sich zunächst, dass Sie über Folgendes verfügen:

* Eine private AVS-Cloud, die mithilfe des AVS-Portals mit ausreichender Kapazität zum Ausführen Ihrer Desktoppools erstellt wurde
* Ausreichende Bandbreite zwischen Ihrer lokalen Umgebung und der privaten AVS-Cloudumgebung, um den Netzwerkdatenverkehr für Ihre Desktops zu unterstützen
* Einen Site-to-Site-VPN-Tunnel, der zwischen Ihrem lokalen Rechenzentrum und der privaten AVS-Cloud eingerichtet ist
* Erreichbarkeit der IP-Adressen im Subnetz der privaten AVS-Cloud aus den Endbenutzersubnetzen in der lokalen Umgebung
* Installation von AD/DHCP/DNS für Ihre private AVS-Cloud

#### <a name="avs-portal-create-a-dedicated-vlansubnet-for-desktop-pools"></a>AVS-Portal: Erstellen eines dedizierten VLAN/Subnetzes für Desktoppools

Erstellen Sie ein VLAN für die Horizon-Desktoppools, und weisen Sie ihm einen Subnetz-CIDR-Wert zu. Anweisungen dazu finden Sie unter [Erstellen und Verwalten von VLANs/Subnetzen](create-vlan-subnet.md). Dabei handelt es sich um das Netzwerk, in dem alle virtuellen Desktopcomputer ausgeführt werden.

Befolgen Sie die bewährten Standardsicherheitsmethoden, um Ihre Horizon-Bereitstellung zu schützen:

* Lassen Sie nur Desktop-RDP-Datenverkehr/SSH-Datenverkehr zu Ihren Desktop-VMS zu.
* Lassen Sie nur Verwaltungsdatenverkehr zwischen dem VLAN der Verwaltungsebene von Horizon und dem VLAN mit dem Desktoppool zu.
* Lassen Sie nur Verwaltungsdatenverkehr aus dem lokalen Netzwerk zu.

Sie können diese bewährten Methoden durch das Konfigurieren von [Firewallregeln](firewall.md) über das AVS-Portal erzwingen.

#### <a name="avs-portal-configure-firewall-rules-to-secure-horizon-management-plane"></a>AVS-Portal: Konfigurieren von Firewallregeln zum Schutz der Verwaltungsebene von Horizon

Richten Sie im AVS-Portal die folgenden Regeln ein. Anweisungen dazu finden Sie unter [Einrichten von Firewalltabellen und -regeln](firewall.md).

1. Konfigurieren Sie in der N-S-Firewall von AVS Firewallregeln, um die Kommunikation zwischen lokalen Subnetzen und dem VLAN der Verwaltungsebene von Horizon zuzulassen, sodass nur die Netzwerkports zugelassen werden, die im VMware-Dokument mit der [Horizon-Portliste](https://docs.vmware.com/en/VMware-Horizon-7/7.1/com.vmware.horizon-client-agent.security.doc/GUID-52807839-6BB0-4727-A9C7-EA73DE61ADAB.html) aufgeführt sind.

2. Erstellen Sie E-W-Firewallregeln zwischen dem Verwaltungs-VLAN von Horizon und dem Desktoppool-VLAN in der privaten AVS-Cloud.

#### <a name="avs-portal-create-a-public-ip-address-for-unified-access-gateway"></a>AVS-Portal: Erstellen einer öffentlichen IP-Adresse für Unified Access Gateway

Erstellen Sie eine öffentliche IP-Adresse für die Unified Access Gateway-Appliance, um Desktopclientverbindungen aus dem Internet zu ermöglichen. Anweisungen hierzu finden Sie unter [Zuweisen von öffentlichen IP-Adressen](public-ips.md).

Nach Abschluss der Einrichtung wird die öffentliche IP-Adresse zugewiesen und auf der Seite mit den öffentlichen IP-Adressen aufgelistet.

#### <a name="avs-portal-escalate-privileges"></a>AVS-Portal: Eskalieren von Berechtigungen

Der Standardbenutzer „cloudowner“ verfügt nicht über ausreichende Berechtigungen im vCenter der privaten AVS-Cloud für die Installation von Horizon, sodass die vCenter-Berechtigungen des Benutzers ausgeweitet werden müssen. Weitere Informationen finden Sie unter [Eskalieren von Berechtigungen](escalate-private-cloud-privileges.md).

#### <a name="vcenter-ui-create-a-user-in-avs-private-cloud-for-horizon-installation"></a>vCenter-Benutzeroberfläche: Erstellen eines Benutzers in der privaten AVS-Cloud für die Installation von Horizon

1. Melden Sie sich bei vCenter mit den Benutzeranmeldeinformationen von „cloudowner“ an.
2. Erstellen Sie in vCenter den neuen Benutzer „horizon-soln-admin“, und fügen Sie ihn der Administratorengruppe in vCenter hinzu.
3. Melden Sie sich bei vCenter als Benutzer „cloudowner“ ab, und melden Sie sich als Benutzer „horizon-soln-admin“ an.

#### <a name="vcenter-ui-install-vmware-horizon"></a>vCenter-Benutzeroberfläche: Installieren von VMware Horizon

Wie bereits im Abschnitt oben zur logischen Architektur erwähnt, umfasst die Horizon-Lösung die folgenden Komponenten:

* VMware Horizon View
* VMware Unified Access Gateway
* VMware App Volume Manager
* VMware User Environment Manager

Installieren Sie die Komponenten wie folgt:

1. Installieren und konfigurieren Sie Unified Access Gateway anhand der Anweisungen im VMware-Dokument [Deploying and Configuring VMware Unified Access Gateway](https://docs.vmware.com/en/Unified-Access-Gateway/3.3.1/com.vmware.uag-331-deploy-config.doc/GUID-F5CE0D5E-BE85-4FA5-BBCF-0F86C9AB8A70.html) (Bereitstellen und Konfigurieren von VMware Unified Access Gateway).

2. Installieren Sie Horizon View in der privaten AVS-Cloud anhand der Anweisungen unter [Installieren von View](https://docs.vmware.com/en/VMware-Horizon-7/7.4/horizon-installation/GUID-37D39B4F-5870-4188-8B11-B6C41AE9133C.html).

3. Installieren Sie App Volume Manager anhand der Anweisungen unter [Install and Configure VMware App Volumes](https://docs.vmware.com/en/VMware-App-Volumes/2.10/com.vmware.appvolumes.user.doc/GUID-5E8BAF8C-F5A6-412C-9424-266BA7109BA4.html) (Installieren und Konfigurieren von VMware App Volumes).

4. Installieren und konfigurieren Sie User Environment Manager anhand der Anweisungen unter [About Installing and Configuring VMware User Environment Manager](https://docs.vmware.com/en/VMware-User-Environment-Manager/9.4/com.vmware.user.environment.manager-install-config/GUID-DBBC82E4-483F-4B28-9D49-4D28E08715BC.html) (Installieren und Konfigurieren von VMware User Environment Manager).

#### <a name="file-a-support-request-to-upload-vmware-horizon-pre-packaged-app-volumes"></a>Erstellen einer Supportanfrage zum Hochladen von vorab gepackten App-Volumes für VMware Horizon

Im Rahmen des Installationsvorgangs verwendet App Volume Manager vorab gepackte Volumes, um App-Stapel und beschreibbare Volumes bereitzustellen. Diese Volumes dienen als Vorlagen für App-Stapel und beschreibbare Volumes.

Zum Hochladen der Volumes in den Datenspeicher der privaten AVS-Cloud ist das ESXi-Root-Kennwort erforderlich. Wenn Sie Hilfe benötigen, erstellen Sie [eine Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). Fügen Sie das AppVolumes-Installationspaket an, damit die Supportmitarbeiter von AVS die Vorlagen in Ihre private AVS-Cloudumgebung hochladen können.

#### <a name="avs-portal-de-escalate-privileges"></a>AVS-Portal: Einschränken von Berechtigungen

Sie können jetzt die Berechtigungen des Benutzers „cloudowner“ [herabstufen](escalate-private-cloud-privileges.md#de-escalate-privileges).

## <a name="ongoing-management-of-your-horizon-solution"></a>Fortlaufende Verwaltung Ihrer Horizon-Lösung

Sie haben die vollständige Kontrolle über die Horizon- und App Volume Manager-Software in der privaten AVS-Cloudumgebung und müssen die erforderliche Verwaltung des Softwarelebenszyklus durchführen. Stellen Sie sicher, dass jede neue Version der Software mit vCenter und PSC in der privaten AVS-Cloud kompatibel ist, bevor Sie ein Update oder Upgrade von Horizon oder App Volume durchführen.
