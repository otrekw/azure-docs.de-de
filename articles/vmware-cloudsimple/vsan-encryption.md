---
title: 'Azure VMware Solutions (AVS): Konfigurieren von vSAN-Verschlüsselung für eine private AVS-Cloud'
description: Beschreibt das Konfigurieren des Features für die vSAN-Softwareverschlüsselung, damit die private AVS-Cloud mit einem in Ihrem virtuellen Azure-Netzwerk ausgeführten Schlüsselverwaltungsserver zusammenarbeiten kann
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 056c05701a3915610fb17a7e8c04feb743e38286
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77020640"
---
# <a name="configure-vsan-encryption-for-avs-private-cloud"></a>Konfigurieren der vSAN-Verschlüsselung für die private AVS-Cloud

Sie können das Feature für die vSAN-Softwareverschlüsselung konfigurieren, damit Ihre private AVS-Cloud mit einem in Ihrem virtuellen Azure-Netzwerk ausgeführten Schlüsselverwaltungsserver zusammenarbeiten kann.

Für VMware ist die Verwendung eines externen KMIP 1.1-konformen Schlüsselverwaltungsserver-Tools (Key Management Server, KMS) eines Drittanbieters erforderlich, wenn die vSAN-Verschlüsselung verwendet wird. Sie können alle unterstützten KMS-Tools nutzen, die für VMware zertifiziert und für Azure verfügbar sind.

In diesem Leitfaden wird beschrieben, wie Sie das KMS-Tool „HyTrust KeyControl“ in einem virtuellen Azure-Netzwerk verwenden. Einen ähnlichen Ansatz können Sie auch für alle anderen zertifizierten KMS-Lösungen von Drittanbietern für vSAN verwenden.

Für diese KMS-Lösung ist Folgendes erforderlich:

* Installieren, Konfigurieren und Verwalten eines für VMware zertifizierten KMS-Drittanbietertools in Ihrem virtuellen Azure-Netzwerk.
* Bereitstellen Ihrer eigenen Lizenzen für das KMS-Tool
* Konfigurieren und Verwalten der vSAN-Verschlüsselung in Ihrer privaten AVS-Cloud mit dem Drittanbietertool KMS in Ihrem virtuellen Azure-Netzwerk

## <a name="kms-deployment-scenario"></a>KMS-Bereitstellungsszenario

Der KMS-Servercluster wird in Ihrem virtuellen Azure-Netzwerk ausgeführt und ist über die konfigurierte Azure ExpressRoute-Verbindung per IP aus vCenter in der privaten AVS-Cloud erreichbar.

![../media/KMS-Cluster im virtuellen Azure-Netzwerk](media/vsan-kms-cluster.png)

## <a name="how-to-deploy-the-solution"></a>Bereitstellen der Lösung

Der Bereitstellungsprozess umfasst die folgenden Schritte:

1. [Sicherstellen, dass alle Voraussetzungen erfüllt sind](#verify-prerequisites-are-met)
2. [AVS-Portal: Abrufen von ExpressRoute-Peeringinformationen](#avs-portal-obtain-expressroute-peering-information)
3. [Azure-Portal: Herstellen einer Verbindung Ihres virtuellen Netzwerks mit der privaten AVS-Cloud](#azure-portal-connect-your-virtual-network-to-the-avs-private-cloud)
4. [Azure-Portal: Bereitstellen eines HyTrust KeyControl-Clusters in Ihrem virtuellen Netzwerk](#azure-portal-deploy-a-hytrust-keycontrol-cluster-in-the-azure-resource-manager-in-your-virtual-network)
5. [HyTrust WebUI: Konfigurieren des KMIP-Servers](#hytrust-webui-configure-the-kmip-server)
6. [vCenter-Benutzeroberfläche: Konfigurieren der vSAN-Verschlüsselung für die Verwendung des KMS-Clusters in Ihrem virtuellen Azure-Netzwerk](#vcenter-ui-configure-vsan-encryption-to-use-kms-cluster-in-your-azure-virtual-network)

### <a name="verify-prerequisites-are-met"></a>Überprüfen der Erfüllung von Voraussetzungen

Überprüfen Sie vor der Bereitstellung Folgendes:

* Der ausgewählte KMS-Anbieter, das Tool und die Version sind in der vSAN-Kompatibilitätsliste enthalten.
* Der ausgewählte Anbieter unterstützt für eine Version des Tools die Ausführung in Azure.
* Die Azure-Version des KMS-Tools ist KMIP 1.1-konform.
* Eine Azure Resource Manager-Instanz und ein virtuelles Netzwerk wurden bereits erstellt.
* Es wurde bereits eine private AVS-Cloud erstellt.

### <a name="avs-portal-obtain-expressroute-peering-information"></a>AVS-Portal: Abrufen von ExpressRoute-Peeringinformationen

Um das Setup fortzusetzen, benötigen Sie den Autorisierungsschlüssel und den Peer-Verbindungs-URI für ExpressRoute sowie Zugriff auf Ihr Azure-Abonnement. Diese Informationen sind im AVS-Portal auf der Seite „Virtual Network Connection“ (Virtuelle Netzwerkverbindung) verfügbar. Eine Anleitung finden Sie unter [Einrichten einer VNET-Verbindung mit der privaten AVS-Cloud](virtual-network-connection.md). Erstellen Sie eine [Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest), falls Sie Probleme beim Abrufen dieser Informationen haben.

### <a name="azure-portal-connect-your-virtual-network-to-the-avs-private-cloud"></a>Azure-Portal: Herstellen einer Verbindung Ihres virtuellen Netzwerks mit der privaten AVS-Cloud

1. Erstellen Sie für Ihr VNET ein Gateway für virtuelle Netzwerke, indem Sie die Anleitung unter [Konfigurieren eines virtuellen Netzwerkgateways für ExpressRoute mit dem Azure-Portal](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md) befolgen.
2. Verknüpfen Sie Ihr virtuelles Netzwerk mit der AVS-ExpressRoute-Leitung anhand der Anweisungen unter [Verbinden eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung mithilfe des Portals](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).
3. Verwenden Sie die Informationen zur AVS-ExpressRoute-Leitung aus Ihrer Begrüßungs-E-Mail von AVS, um Ihr virtuelles Netzwerk mit der AVS-ExpressRoute-Leitung in Azure zu verknüpfen.
4. Geben Sie den Autorisierungsschlüssel und den Peerleitungs-URI ein, geben Sie der Verbindung einen Namen, und klicken Sie auf **OK**.

![Angeben des CS-Peerleitungs-URI beim Erstellen des virtuellen Netzwerks](media/vsan-azureportal01.png) 

### <a name="azure-portal-deploy-a-hytrust-keycontrol-cluster-in-the-azure-resource-manager-in-your-virtual-network"></a>Azure-Portal: Bereitstellen eines HyTrust KeyControl-Clusters in der Azure Resource Manager-Instanz Ihres virtuellen Netzwerks

Führen Sie die folgenden Aufgaben durch, um einen HyTrust KeyControl-Cluster in der Azure Resource Manager-Instanz in Ihrem virtuellen Netzwerk bereitzustellen. Details hierzu finden Sie in der [HyTrust-Dokumentation](https://docs.hytrust.com/DataControl/Admin_Guide-4.0/Default.htm#OLH-Files/Azure.htm%3FTocPath%3DHyTrust%2520DataControl%2520and%2520Microsoft%2520Azure%7C_____0).

1. Erstellen Sie eine Azure-Netzwerksicherheitsgruppe (nsg-hytrust) mit angegebenen Eingangsregeln, indem Sie die Anleitung in der HyTrust-Dokumentation befolgen.
2. Generieren Sie ein SSH-Schlüsselpaar in Azure.
3. Stellen Sie den anfänglichen KeyControl-Knoten über das Azure Marketplace-Image bereit. Verwenden Sie den öffentlichen Schlüssel des generierten Schlüsselpaars, und wählen Sie **nsg-hytrust** als Netzwerksicherheitsgruppe für den KeyControl-Knoten aus.
4. Konvertieren Sie die private IP-Adresse von KeyControl in eine statische IP-Adresse.
5. Stellen Sie eine SSH-Verbindung mit der KeyControl-VM her, indem Sie ihre öffentliche IP-Adresse und den privaten Schlüssel des zuvor erwähnten Schlüsselpaars verwenden.
6. Wählen Sie bei entsprechender Aufforderung in der Secure Shell die Option `No` aus, um den Knoten als anfänglichen KeyControl-Knoten festzulegen.
7. Fügen Sie zusätzliche KeyControl-Knoten hinzu, indem Sie die Schritte 3 bis 5 dieses Verfahrens erneut ausführen und `Yes` auswählen, wenn die Aufforderung zum Hinzufügen zu einem vorhandenen Cluster angezeigt wird.

### <a name="hytrust-webui-configure-the-kmip-server"></a>HyTrust WebUI: Konfigurieren des KMIP-Servers

Navigieren Sie zu „https://*public-ip*“, wobei *public-ip* für die öffentliche IP-Adresse der KeyControl-Knoten-VM steht. Führen Sie die Schritte aus, die in der [HyTrust-Dokumentation](https://docs.hytrust.com/DataControl/Admin_Guide-4.0/Default.htm#OLH-Files/Azure.htm%3FTocPath%3DHyTrust%2520DataControl%2520and%2520Microsoft%2520Azure%7C_____0) beschrieben sind.

1. [Configuring a KMIP server](https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/configuring-kmip-server.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____2) (Konfigurieren eines KMIP-Servers)
2. [Creating a Certificate Bundle for VMware Encryption](https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/creating-user-for-vmcrypt.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____3) (Erstellen eines Zertifikatpakets für die VMware-Verschlüsselung)

### <a name="vcenter-ui-configure-vsan-encryption-to-use-kms-cluster-in-your-azure-virtual-network"></a>vCenter-Benutzeroberfläche: Konfigurieren der vSAN-Verschlüsselung für die Verwendung des KMS-Clusters in Ihrem virtuellen Azure-Netzwerk

Befolgen Sie die HyTrust-Anleitung zum [Erstellen eines KMS-Clusters in vCenter](https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/creating-KMS-Cluster.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____4).

![Hinzufügen von KMS-Clusterdetails in vCenter](media/vsan-config01.png)

Navigieren Sie in vCenter zu **Cluster > Konfigurieren**, und wählen Sie für vSAN die Option **Allgemein** aus. Aktivieren Sie die Verschlüsselung, und wählen Sie den KMS-Cluster aus, der vCenter zuvor hinzugefügt wurde.

![Aktivieren der vSAN-Verschlüsselung und Konfigurieren des KMS-Clusters in vCenter](media/vsan-config02.png)

## <a name="references"></a>References

### <a name="azure"></a>Azure

[Konfigurieren eines virtuellen Netzwerkgateways für ExpressRoute mit dem Azure-Portal](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md)

[Verbinden eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung mithilfe des Portals](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md)

### <a name="hytrust"></a>HyTrust

[HyTrust DataControl and Microsoft Azure](https://docs.hytrust.com/DataControl/Admin_Guide-4.0/Default.htm#OLH-Files/Azure.htm%3FTocPath%3DHyTrust%2520DataControl%2520and%2520Microsoft%2520Azure%7C_____0) (HyTrust DataControl und Microsoft Azure)

[Configuring a KMPI Server](https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/configuring-kmip-server.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____2) (Konfigurieren eines KMPI-Servers)

[Creating a Certificate Bundle for VMware Encryption](https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/creating-user-for-vmcrypt.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____3) (Erstellen eines Zertifikatpakets für die VMware-Verschlüsselung)

[Creating the KMS Cluster in vSphere](https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/creating-KMS-Cluster.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____4) (Erstellen des KMS-Clusters in vSphere)
