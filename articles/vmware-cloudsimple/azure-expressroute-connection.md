---
title: 'Azure VMware Solutions (AVS): Verbinden der privaten AVS-Cloud mit dem Azure-Netzwerk mithilfe von ExpressRoute'
description: Beschreibt, wie Ihre private AVS-Cloudumgebung über ExpressRoute mit dem virtuellen Azure-Netzwerk verbunden wird
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 3d487794e219f63150142db8df4b0c1abf112947
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77015234"
---
# <a name="connect-your-avs-private-cloud-environment-to-the-azure-virtual-network-using-expressroute"></a>Verbinden Ihrer privaten AVS-Cloudumgebung mit dem virtuellen Azure-Netzwerk über ExpressRoute

Ihre private AVS-Cloud kann über ExpressRoute mit dem virtuellen Azure-Netzwerk verbunden werden. Diese Verbindung mit hoher Bandbreite und geringer Latenz ermöglicht Ihnen den Zugriff auf Dienste, die in Ihrem Azure-Abonnement in Ihrer privaten AVS-Cloudumgebung ausgeführt werden.

Eine virtuelle Netzwerkverbindung ermöglicht Ihnen Folgendes:

* Verwenden von Azure als Sicherungsziel für virtuelle Computer in Ihrer privaten AVS-Cloud
* Bereitstellen von KMS-Servern in Ihrem Azure-Abonnement, um den vSAN-Datenspeicher Ihrer privaten AVS-Cloud zu verschlüsseln
* Verwenden von Hybridanwendungen, bei denen die Webebene der jeweiligen Anwendung in der öffentlichen Cloud ausgeführt wird, während die Anwendungs- und die Datenbankebene in Ihrer privaten AVS-Cloud ausgeführt werden

![Azure ExpressRoute-Verbindung mit virtuellem Netzwerk](media/cloudsimple-azure-network-connection.png)

## <a name="set-up-a-virtual-network-connection"></a>Einrichten einer Verbindung mit einem virtuellen Netzwerk

Um die Verbindung des virtuellen Netzwerks mit Ihrer privaten AVS-Cloud einzurichten, benötigen Sie Ihren Autorisierungsschlüssel, Ihren Peerleitungs-URI sowie Zugriff auf Ihr Azure-Abonnement. Diese Informationen sind im AVS-Portal auf der Seite „Virtual Network Connection“ (Virtuelle Netzwerkverbindung) verfügbar. Anweisungen finden Sie unter [Abrufen von Peeringinformationen für eine Verbindung zwischen dem virtuellen Azure-Netzwerk und AVS](virtual-network-connection.md). Wenn Sie Probleme haben, diese Informationen abzurufen, übermitteln Sie eine <a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">Supportanfrage</a>.

> [!TIP]
> Wenn Sie bereits über ein virtuelles Azure-Netzwerk, ein Gatewaysubnetz und ein Gateway für virtuelle Netzwerke verfügen, können Sie mit Schritt 4 fortfahren.

1. Erstellen Sie ein virtuelles Netzwerk in Ihrem Azure-Abonnement, und vergewissern Sie sich, dass sich der ausgewählte Adressraum vom Adressraum Ihrer privaten AVS-Cloud unterscheidet. Wenn Sie bereits über ein virtuelles Azure-Netzwerk verfügen, können Sie das vorhandene Netzwerk verwenden. Weitere Informationen finden Sie unter [Erstellen eines virtuellen Netzwerks im Azure-Portal](../virtual-network/quick-create-portal.md).
2. Erstellen Sie das Gatewaysubnetz in Ihrem virtuellen Azure-Netzwerk. Wenn Sie bereits über ein Gatewaysubnetz in Ihrem virtuellen Azure-Netzwerk verfügen, können Sie das vorhandene Subnetz verwenden. Weitere Informationen finden Sie unter [Erstellen des Gatewaysubnetzes](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md#create-the-gateway-subnet).
3. Erstellen Sie das virtuelle Netzwerkgateway in Ihrem virtuellen Netzwerk. Wenn bereits ein virtuelles Netzwerkgateway vorhanden ist, können Sie dieses verwenden. Weitere Informationen finden Sie unter [Erstellen des Gateways für virtuelle Netzwerke](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md#create-the-virtual-network-gateway).
4. Erstellen Sie die Verbindung zwischen Ihrem virtuellen Netzwerk und Ihrer privaten AVS-Cloud, indem Sie den Autorisierungsschlüssel einlösen, wie unter [Verbinden eines virtuellen Netzwerks mit einer anderen Verbindung: anderes Abonnement](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md#connect-a-vnet-to-a-circuit---different-subscription) beschrieben.

> [!WARNING]
> Wenn Sie ein vorhandenes Gateway eines virtuellen Netzwerks verwenden und dieses über eine ExpressRoute-Verbindung mit demselben Standort wie die AVS-ExpressRoute-Leitung verfügt, wird die Verbindung nicht hergestellt. Erstellen Sie ein neues virtuelles Netzwerk, und führen Sie die oben genannten Schritte aus.

## <a name="test-the-virtual-network-connection"></a>Testen der Verbindung mit dem virtuellen Netzwerk

Nachdem die Verbindung hergestellt wurde, können Sie den Status der Verbindung überprüfen, indem Sie **Eigenschaften** unter **Einstellungen** auswählen. Der Status und der Bereitstellungszustand sollten als **Erfolgreich** angezeigt werden.

![Verbindungsstatus](media/azure-expressroute-connection.png)

So testen Sie die Verbindung des virtuellen Netzwerks:

1. Erstellen Sie einen virtuellen Computer in Ihrem Azure-Abonnement.
2. Suchen Sie nach der IP-Adresse von vCenter in Ihrer privaten AVS-Cloud (siehe Begrüßungs-E-Mail).
3. Pingen Sie Ihr Cloud-vCenter von dem virtuellen Computer, der in Ihrem virtuellen Azure-Netzwerk erstellt wurde.
4. Pingen Sie Ihren virtuellen Azure-Computer über einen virtuellen Computer, der in vCenter in Ihrer privaten AVS-Cloud ausgeführt wird.

Wenn beim Herstellen der Verbindung Probleme auftreten, übermitteln Sie eine <a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">Supportanfrage</a>.
