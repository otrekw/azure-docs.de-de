---
title: Verwenden von Azure Application Gateway mit virtuellen VMware-Computern
description: Beschreibt, wie Sie mit Azure Application Gateway eingehenden Webdatenverkehr für Webserver verwalten, die auf virtuellen VMware-Computern in der privaten AVS-Cloudumgebung ausgeführt werden
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 94cc6e40b88fe631d525f41001034f5dada05397
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77015455"
---
# <a name="use-azure-application-gateway-with-vmware-virtual-machines-in-the-avs-private-cloud-environment"></a>Verwenden von Azure Application Gateway mit virtuellen VMware-Computern in der privaten AVS-Cloudumgebung

Sie können mit Azure Application Gateway eingehenden Webdatenverkehr für Webserver verwalten, die auf virtuellen VMware-Computern in Ihrer privaten AVS-Cloudumgebung ausgeführt werden.

Durch die Nutzung von Azure Application Gateway in einer öffentlich-privaten Hybridbereitstellung können Sie den Webdatenverkehr für Ihre Anwendungen verwalten, ein sicheres Front-End bereitstellen und die SSL-Verarbeitung für die in der VMware-Umgebung ausgeführten Dienste entlasten. Azure Application Gateway leitet eingehenden Webdatenverkehr an Back-End-Poolinstanzen weiter, die in VMware-Umgebungen gemäß konfigurierten Regeln und Integritätstests ausgeführt werden.

Diese Azure Application Gateway-Lösung erfordert Folgendes:

* Ein Azure-Abonnement
* Erstellen und Konfigurieren eines virtuellen Azure-Netzwerks und eines Subnetzes im virtuellen Netzwerk.
* Erstellen und Konfigurieren von NSG-Regeln und Einrichten des Peerings Ihres VNET mit Ihrer privaten AVS-Cloud mithilfe von ExpressRoute.
* Erstellen und Konfigurieren Ihrer privaten AVS-Cloud.
* Erstellen und konfigurieren Sie Ihr Azure Application Gateway.

## <a name="azure-application-gateway-deployment-scenario"></a>Bereitstellungsszenario für Azure Application Gateway

In diesem Szenario wird das Azure Application Gateway in Ihrem virtuellen Azure-Netzwerk ausgeführt. Das virtuelle Netzwerk ist über eine ExpressRoute-Leitung mit Ihrer privaten AVS-Cloud verbunden. Alle Subnetze in der privaten AVS-Cloud sind per IP aus den Subnetzen des virtuellen Netzwerks erreichbar.

![Azure-Lastenausgleich im virtuellen Azure-Netzwerk](media/load-balancer-use-case.png)

## <a name="how-to-deploy-the-solution"></a>Bereitstellen der Lösung

Der Bereitstellungsvorgang besteht aus den folgenden Aufgaben:

1. [Sicherstellen, dass alle Voraussetzungen erfüllt sind](#1-verify-prerequisites)
2. [Herstellen einer Verbindung zwischen dem virtuellen Azure-Netzwerk und der privaten AVS-Cloud](#2-connect-your-azure-virtual-network-to-your-avs-private-cloud)
3. [Bereitstellen eines Azure Application Gateways](#3-deploy-an-azure-application-gateway)
4. [Erstellen und Konfigurieren eines Webserver-VM-Pools in Ihrer privaten AVS-Cloud](#4-create-and-configure-a-web-server-vm-pool-in-your-avs-private-cloud)

## <a name="1-verify-prerequisites"></a>1. Überprüfen der Voraussetzungen

Vergewissern Sie sich, dass die folgenden Voraussetzungen erfüllt sind:

* Ein Azure Resource Manager und ein virtuelles Netzwerk wurden bereits erstellt.
* Ein dediziertes Subnetz (für Application Gateway) in Ihrem virtuellen Azure-Netzwerk wurde bereits erstellt.
* Es wurde bereits eine private AVS-Cloud erstellt.
* Zwischen IP-Subnetzen im virtuellen Netzwerk und Subnetzen in der privaten Cloud besteht kein IP-Konflikt.

## <a name="2-connect-your-azure-virtual-network-to-your-avs-private-cloud"></a>2. Herstellen einer Verbindung Ihres virtuellen Azure-Netzwerks mit der privaten AVS-Cloud

Um Ihr virtuelles Azure-Netzwerk mit Ihrer privaten AVS-Cloud zu verbinden, gehen Sie wie folgt vor.

1. [Kopieren Sie im AVS-Portal die Informationen zum ExpressRoute-Peering](virtual-network-connection.md).

2. [Konfigurieren Sie ein virtuelles Netzwerkgateway für Ihr virtuelles Azure-Netzwerk](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md).

3. [Verknüpfen Sie Ihr virtuelles Netzwerk mit der AVS-ExpressRoute-Leitung](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md#connect-a-vnet-to-a-circuit---different-subscription).

4. [Verwenden Sie die von Ihnen kopierten Peeringinformationen, um Ihr virtuelles Netzwerk mit der ExpressRoute-Leitung zu verbinden](virtual-network-connection.md).

## <a name="3-deploy-an-azure-application-gateway"></a>3. Bereitstellen eines Azure Application Gateways

Ausführlich Anleitungen dazu finden Sie unter [Erstellen eines Anwendungsgateways mit pfadbasierten Routingregeln mithilfe des Azure-Portals](../application-gateway/create-url-route-portal.md). Dies ist eine Zusammenfassung der erforderlichen Schritte:

1. Erstellen Sie ein virtuelles Netzwerk in Ihrem Abonnement und eine Ressourcengruppe.
2. Erstellen Sie ein Subnetz (das als dediziertes Subnetz verwendet wird) in Ihrem virtuellen Netzwerk.
3. Erstellen Sie ein Standardanwendungsgateway (aktivieren Sie optional WAF): Klicken Sie auf der Homepage des Azure-Portals oben links auf der Seite auf **Resource (Ressource)**  > **Networking (Netzwerk)**  > **Application Gateway (Anwendungsgateway)** . Wählen Sie die Standard-SKU und die Größe aus, und geben Sie das Azure-Abonnement, die Ressourcengruppe und die Standortinformationen an. Erstellen Sie bei Bedarf eine neue öffentliche IP-Adresse für dieses Anwendungsgateway, und geben Sie Details zum virtuellen Netzwerk und zum dedizierten Subnetz für das Anwendungsgateway an.
4. Fügen Sie einen Back-End-Pool mit virtuellen Computern hinzu, und fügen Sie diesen Ihrem Anwendungsgateway hinzu.

## <a name="4-create-and-configure-a-web-server-vm-pool-in-your-avs-private-cloud"></a>4. Erstellen und Konfigurieren eines Webserver-VM-Pools in Ihrer privaten AVS-Cloud

Erstellen Sie in vCenter VMs mit dem Betriebssystem und Webserver Ihrer Wahl (z.B. Windows/IIS oder Linux/Apache). Wählen Sie ein Subnetz/VLAN aus, das für die Webschicht in Ihrer privaten AVS-Cloud reserviert ist. Vergewissern Sie sich, dass sich mindestens ein vNIC der Webserver-VMs im Subnetz der Webschicht befindet.
