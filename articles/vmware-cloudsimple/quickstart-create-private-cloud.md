---
title: 'Schnellstart für Azure VMware Solutions (AVS): Erstellen einer privaten AVS-Cloud'
description: Erfahren Sie, wie Sie mit Azure VMware Solutions (AVS) eine private AVS-Cloud erstellen und konfigurieren.
titleSuffix: Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: dikamath
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: cafcf04dac0542f1506980d8b9484b82b558e100
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77018566"
---
# <a name="quickstart---configure-an-avs-private-cloud-environment"></a>Schnellstart: Konfigurieren einer privaten AVS-Cloudumgebung

In diesem Artikel erfahren Sie, wie Sie eine private AVS-Cloud erstellen und Ihre private AVS-Cloudumgebung einrichten.

## <a name="before-you-begin"></a>Voraussetzungen

Lesen Sie [Voraussetzungen für Netzwerke](cloudsimple-network-checklist.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-an-avs-private-cloud"></a>Erstellen einer privaten AVS-Cloud

Eine private AVS-Cloud ist ein isolierter VMware-Stapel, der ESXi-Hosts, vCenter, vSAN und NSX unterstützt.

Private AVS-Clouds werden über das AVS-Portal verwaltet. Sie verfügen über einen eigenen vCenter-Server in einer eigenen Verwaltungsdomäne. Der Stapel wird auf dedizierten Knoten und isolierten Bare-Metal-Hardwareknoten ausgeführt.

1. Wählen Sie **Alle Dienste** aus.
2. Suchen Sie nach **AVS Services** (AVS-Dienste).
3. Wählen Sie den AVS-Dienst aus, über den Sie Ihre private AVS-Cloud erstellen möchten.
4. Klicken Sie unter **Übersicht** auf **Create AVS Private Cloud** (Private AVS-Cloud erstellen), um eine neue Browserregisterkarte für das AVS-Portal zu öffnen. Melden Sie sich nach Aufforderung mit Ihren Azure-Anmeldeinformationen an. 

    ![Erstellen einer privaten AVS-Cloud in Azure](media/create-private-cloud-from-azure.png)

5. Geben Sie im AVS-Portal einen Namen für Ihre private AVS-Cloud an.
6. Wählen Sie den **Standort** Ihrer privaten AVS-Cloud aus.
7. Wählen Sie den **Knotentyp** aus, der mit dem in Azure bereitgestellten Knotentyp übereinstimmt.
8. Geben Sie die **Knotenanzahl** an. Für das Erstellen einer privaten AVS-Cloud sind mindestens drei Knoten erforderlich.

    ![Erstellen einer privaten AVS-Cloud: grundlegende Informationen](media/create-private-cloud-basic-info.png)

9. Klicken Sie auf **Weiter: Erweiterte Optionen**.
10. Geben Sie den CIDR-Bereich für vSphere/vSAN-Subnetze ein. Stellen Sie sicher, dass sich der CIDR-Bereich nicht mit einem Ihrer lokalen Subnetze, mit anderen Azure-Subnetzen (virtuelle Netzwerke) oder dem Gatewaysubnetz überlappt.

    **Mögliche CIDR-Bereiche:** /24, /23, /22 oder /21. Der CIDR-Bereich /24 unterstützt bis zu 26 Knoten, der Bereich /23 bis zu 58 Knoten und die Bereiche /22 und /21 bis zu 64 Knoten (die maximale Anzahl von Knoten in einer privaten AVS-Cloud). Weitere Informationen und VLANs und Subnetze finden Sie unter [Übersicht über VLANs und Subnetze](cloudsimple-vlans-subnets.md).

      > [!IMPORTANT]
      > IP-Adressen im vSphere/vSAN-CIDR-Bereich sind für die Verwendung durch die private AVS-Cloudinfrastruktur reserviert. Verwenden Sie keine IP-Adresse dieses Bereichs auf einem virtuellen Computer.

11. Klicken Sie auf **Weiter: Überprüfen und erstellen**.
12. Überprüfen Sie die Einstellungen. Wenn Sie irgendeine der Einstellungen ändern müssen, klicken Sie auf **Zurück**.
13. Klicken Sie auf **Erstellen**.

Die Bereitstellung der privaten AVS-Cloud wird gestartet. Es kann bis zu zwei Stunden dauern, bis die private AVS-Cloud bereitgestellt wurde.

## <a name="launch-avs-portal"></a>Starten des AVS-Portals

Sie können aus dem Azure-Portal auf das AVS-Portal zugreifen. Das AVS-Portal wird mit Ihren Azure-Anmeldeinformationen über einmaliges Anmelden (Single Sign-On, SSO) gestartet. Damit Sie auf das AVS-Portal zugreifen können, müssen Sie die Anwendung **AVS Service Authorization** autorisieren. Weitere Informationen zum Erteilen von Berechtigungen finden Sie unter [Einwilligen für die Anwendung AVS Service Authorization](access-cloudsimple-portal.md#consent-to-avs-service-authorization-application).

1. Wählen Sie **Alle Dienste** aus.
2. Suchen Sie nach **AVS Services** (AVS-Dienste).
3. Wählen Sie den AVS-Dienst aus, über den Sie Ihre private AVS-Cloud erstellen möchten.
4. Klicken Sie in der Übersicht auf **Go to the AVS portal** (Zum AVS-Portal wechseln), um eine neue Browserregisterkarte für das AVS-Portal zu öffnen. Melden Sie sich nach Aufforderung mit Ihren Azure-Anmeldeinformationen an. 

    ![Starten des AVS-Portals](media/launch-cloudsimple-portal.png)

## <a name="create-point-to-site-vpn"></a>Erstellen einer Point-to-Site-VPN-Verbindung

Eine Point-to-Site-VPN-Verbindung ist die einfachste Möglichkeit, auf Ihrem Computer eine Verbindung mit Ihrer privaten AVS-Cloud herzustellen. Verwenden Sie eine Point-to-Site-VPN-Verbindung, wenn Sie eine Remoteverbindung mit der privaten AVS-Cloud herstellen. Führen Sie für schnellen Zugriff auf Ihre private AVS-Cloud die nachstehenden Schritte aus. Der Zugriff auf die AVS-Region kann aus Ihrem lokalen Netzwerk über [Site-to-Site-VPN](vpn-gateway.md) oder [Azure ExpressRoute](on-premises-connection.md) erfolgen.

### <a name="create-gateway"></a>Erstellen des Gateways

1. Starten Sie das AVS-Portal, und wählen Sie **Network** (Netzwerk) aus.
2. Wählen Sie **VPN Gateway** aus.
3. Klicken Sie auf **New VPN Gateway**.

    ![Erstellen eines VPN-Gateways](media/create-vpn-gateway.png)

4. Geben Sie für **Gateway configuration** die folgenden Einstellungen an, und klicken Sie auf **Weiter**.

    * Wählen Sie **Point-to-Site VPN** als Gatewaytyp aus.
    * Geben Sie einen Namen ein, um das Gateway zu bezeichnen.
    * Wählen Sie den Azure-Standort aus, an dem der AVS-Dienst bereitgestellt wird.
    * Geben Sie das Clientsubnetz für das Point-to-Site-Gateway an. DHCP-Adressen werden aus diesem Subnetz angegeben, wenn Sie eine Verbindung herstellen.

5. Geben Sie für **Connection/User** die folgenden Einstellungen an, und klicken Sie auf **Weiter**.

    * Um allen aktuellen und zukünftigen Benutzern automatisch Zugriff auf die private AVS-Cloud über dieses Point-to-Site-Gateway zu gestatten, wählen Sie **Automatically add all users** (Alle Benutzer automatisch hinzufügen) aus. Wenn Sie diese Option auswählen, werden automatisch alle Benutzer in der Benutzerliste ausgewählt. Sie können die automatische Option außer Kraft setzen, indem Sie einzelne Benutzer in der Liste deaktivieren.
    * Um nur einzelne Benutzer auszuwählen, klicken Sie auf die Kontrollkästchen in der Benutzerliste.

6. Im Abschnitt „VLANs/Subnets“ können Sie Verwaltungs- und Benutzer-VLANs/Subnetze für das Gateway und für Verbindungen angeben.

    * Mit den **Automatically add**-Optionen wird die globale Richtlinie für dieses Gateway festgelegt. Die Einstellungen gelten für das aktuelle Gateway. Die Einstellungen können im **Auswahl**bereich überschrieben werden.
    * Wählen Sie **Add management VLANs/Subnets of AVS Private Clouds** (Verwaltungs-VLANs/Subnetze privater AVS-Clouds hinzufügen) aus.
    * Um alle benutzerdefinierten VLANs/Subnetze hinzuzufügen, klicken Sie auf **Add user-defined VLANs/Subnets**.
    * Die **Auswahl**-Einstellungen überschreiben die globalen Einstellungen unter **Automatically add**.

7. Klicken Sie auf **Weiter**, um die Einstellungen zu überprüfen. Klicken Sie auf die Bearbeiten-Symbole, um jegliche Änderungen vorzunehmen.
8. Klicken Sie auf **Create**, um das VPN-Gateway zu erstellen.

### <a name="connect-to-avs-using-point-to-site-vpn"></a>Herstellen einer Verbindung mit AVS über ein Point-to-Site-VPN

Es ist ein VPN-Client erforderlich, um von Ihrem Computer aus eine Verbindung mit AVS herzustellen. Laden Sie den [OpenVPN-Client](https://openvpn.net/community-downloads/) für Windows oder [Viscosity](https://www.sparklabs.com/viscosity/download/) für Mac OS und OS X herunter.

1. Starten Sie das AVS-Portal, und wählen Sie **Network** (Netzwerk) aus.
2. Wählen Sie **VPN Gateway** aus.
3. Klicken Sie in der Liste der VPN-Gateways auf das Point-to-Site-VPN-Gateway.
4. Wählen Sie **Benutzer** aus.
5. Klicken Sie auf **Download my VPN configuration** (Meine VPN-Konfiguration herunterladen).

    ![Herunterladen der VPN-Konfiguration](media/download-p2s-vpn-configuration.png)

6. Importieren Sie die Konfiguration in Ihren VPN-Client.

    * Anweisungen zum [Importieren der Konfiguration auf einem Windows-Client](https://openvpn.net/vpn-server-resources/connecting-to-access-server-with-windows/#openvpn-open-source-openvpn-gui-program)
    * Anweisungen zum [Importieren der Konfiguration unter macOS oder OS X](https://www.sparklabs.com/support/kb/article/getting-started-with-viscosity-mac/#creating-your-first-connection)

7. Stellen Sie eine Verbindung mit AVS her.

## <a name="create-a-vlan-for-your-workload-vms"></a>Erstellen eines VLANs für Ihre Workload-VMs

Erstellen Sie nach dem Erstellen einer privaten AVS-Cloud ein VLAN, in dem Sie Ihre Workload-/Anwendungs-VMs bereitstellen möchten.

1. Wählen Sie im AVS-Portal **Network** (Netzwerk) aus.
2. Klicken Sie auf **VLAN/Subnets**.
3. Klicken Sie auf **Create VLAN/Subnet** (VLAN/Subnetz erstellen).

    ![VLAN/Subnetz erstellen](media/create-new-vlan-subnet.png)

4. Wählen Sie die **AVS Private Cloud** (Private AVS-Cloud) für das neue VLAN/Subnetz aus.
5. Wählen Sie eine VLAN-ID in der Liste aus. 
6. Geben Sie einen Subnetznamen ein, um das Subnetz zu bezeichnen.
7. Geben Sie den CIDR-Bereich und die Maske des Subnetzes an. Dieser Bereich darf sich nicht mit irgendeinem vorhandenen Subnetz überlappen.
8. Klicken Sie auf **Submit**(Senden).

    ![Details zum Erstellen des VLANs/Subnetzes](media/create-new-vlan-subnet-details.png)

Die VLAN/Subnetz wird erstellt. Sie können jetzt diese VLAN-ID verwenden, um eine verteilten Portgruppe in der vCenter-Instanz Ihrer privaten AVS-Cloud zu erstellen.

## <a name="connect-your-environment-to-an-azure-virtual-network"></a>Verbinden Ihrer Umgebung mit einem virtuellen Azure-Netzwerk

AVS stellt eine ExpressRoute-Leitung für Ihre private AVS-Cloud bereit. Sie können Ihr virtuelles Netzwerk in Azure mit der ExpressRoute-Verbindung verbinden. Ausführliche Informationen zum Einrichten der Verbindung finden Sie in den Schritten unter [Verbindung mit dem virtuellen Azure-Netzwerk mithilfe von ExpressRoute](https://docs.azure.cloudsimple.com/cloudsimple-azure-network-connection/).

## <a name="sign-in-to-vcenter"></a>Anmelden bei vCenter

Sie können sich jetzt bei vCenter anmelden, um virtuelle Computer sowie Richtlinien einzurichten.

1. Um auf vCenter zuzugreifen, beginnen Sie im AVS-Portal. Klicken Sie auf der Startseite unter **Common Tasks** auf **Launch vSphere Client**. Wählen Sie die private AVS-Cloud aus, und klicken Sie dann auf **Launch vSphere Client** (vSphere-Client starten) für die private AVS-Cloud.

    ![VSphere-Client starten](media/launch-vcenter-from-cloudsimple-portal.png)

2. Wählen Sie Ihren bevorzugten vSphere-Client aus, über den Sie auf vCenter zugreifen möchten, und melden Sie sich mit Ihrem Benutzernamen und Kennwort an. Die Standardwerte sind:
    * Benutzername: **CloudOwner@AVS.local**
    * Password (Kennwort): **AVS123!**  

Die vCenter-Bildschirme in den nächsten Schritten stammen vom vSphere-Client für HTML5.

## <a name="change-your-vcenter-password"></a>Ändern Ihres vCenter-Kennworts

AVS empfiehlt, Ihr Kennwort zu ändern, wenn Sie sich erstmals bei vCenter anmelden. Das von Ihnen festgelegte Kennwort muss die folgenden Anforderungen erfüllen:

* Maximale Lebensdauer: Das Kennwort muss alle 365 Tage geändert werden.
* Einschränkung für Wiederverwendung: Benutzer können keines der fünf vorherigen Kennwörter wiederverwenden.
* Länge: 8–20 Zeichen
* Sonderzeichen: mindestens ein Sonderzeichen
* Alphabetische Zeichen: mindestens ein Großbuchstabe (A-Z) und mindestens ein Kleinbuchstabe (a-z)
* Zahlen: mindestens ein numerisches Zeichen (0-9)
* Maximale Anzahl identischer aufeinanderfolgender Zeichen: drei

    Beispiel: CC oder CCC ist als Teil eines Kennworts zulässig, CCCC dagegen nicht.

Wenn Sie ein Kennwort festlegen, das die Anforderungen nicht erfüllt:

* Wenn Sie den vSphere-Flash-Client verwenden, meldet dieser einen Fehler.
* Wenn Sie den HTML5-Client verwendet, wird kein Fehler gemeldet. Der Client akzeptiert die Änderung nicht, und das alte Kennwort funktioniert weiterhin.

## <a name="access-nsx-manager"></a>Zugreifen auf NSX Manager

NSX Manager wird mit einem Standardkennwort bereitgestellt. 

* Benutzername: **admin**
* Password (Kennwort): **AVS123!**

Sie finden den vollqualifizierten Domänennamen (FQDN) und die IP-Adresse von NSX Manager im AVS-Portal.

1. Starten Sie das AVS-Portal, und wählen Sie **Resources** (Ressourcen) aus.
2. Klicken Sie auf die private AVS-Cloud, die Sie verwenden möchten.
3. Wählen Sie **vSphere management network** aus.
4. Verwenden Sie den FQDN oder die IP-Adresse von **NSX Manager**, und stellen Sie über einen Webbrowser eine Verbindung her.

    ![Nach dem FQDN von NSX Manager suchen](media/private-cloud-nsx-manager-fqdn.png)

## <a name="create-a-port-group"></a>Erstellen einer Portgruppe

So erstellen eine verteilte Portgruppe in vSphere:

1. Führen Sie die Anweisungen aus, die im [vSphere-Netzwerkleitfaden](https://docs.vmware.com/en/VMware-vSphere/6.5/vsphere-esxi-vcenter-server-65-networking-guide.pdf) unter „Add a distributed port group“ (Hinzufügen einer verteilen Portgruppe) beschrieben werden.
2. Stellen Sie beim Einrichten der verteilten Portgruppe die VLAN-ID bereit, die Sie in [Erstellen eines VLANs für Ihre Workload-VMs](#create-a-vlan-for-your-workload-vms) erstellt haben.

## <a name="next-steps"></a>Nächste Schritte

* [Nutzen von virtuellen VMware-Computern in Azure](quickstart-create-vmware-virtual-machine.md)
* [Connect from on-premises to CloudSimple using ExpressRoute](on-premises-connection.md)
* [Einrichten eines Site-to-Site-VPN-Gateways aus der lokalen Umgebung](vpn-gateway.md)
