---
title: 'Azure VMware Solutions (AVS): Auswählen einer Lastenausgleichslösung für private AVS-Clouds'
description: Beschreibt die Lastenausgleichsoptionen beim Bereitstellen einer Anwendung in einer privaten AVS-Cloud
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 405bc9d95b8d82e2181e2fb828d6bcc00c8c4639
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77014877"
---
# <a name="choose-a-load-balancing-solution-for-avs-private-clouds"></a>Auswählen einer Lastenausgleichslösung für private AVS-Clouds

Wenn Sie eine Anwendung in einer privaten AVS-Cloud bereitstellen, können Sie eine von mehreren Optionen für den Lastenausgleich auswählen.

Sie können einen virtuellen oder softwarebasierten Lastenausgleich in Ihrer privaten AVS-Cloud auswählen oder sogar den Azure-L7-Lastenausgleich in Ihrem Azure-Abonnement als Front-End für die VMs auf Webebene verwenden, die in der privaten AVS-Cloud ausgeführt werden. Einige der Optionen, die verfügbar sind:

## <a name="virtual-load-balancers"></a>Virtueller Lastenausgleich

Sie können virtuelle Lastenausgleichsgeräte in Ihrer VMware-Umgebung über die vCenter-Schnittstelle bereitstellen und diese so konfigurieren, dass Sie als Front-End für Ihren Anwendungsdatenverkehr dienen.

Einige beliebte Anbieter sind: NginX: http://nginx.org/en/docs/http/load_balancing.html F5- BigIP: Traffic Manager: https://www.f5.com/products/big-ip-services/virtual-editions Citrix ADC: https://www.citrix.com/products/citrix-adc/

## <a name="azure-l7-load-balancer"></a>Azure L7-Lastenausgleich

Wenn Sie Azure Application Gateway als L7-Lastenausgleich für Ihre in einer privaten AVS-Cloud ausgeführte Anwendung verwenden, müssen Sie die Lastenausgleichssoftware nicht verwalten. Die Lastenausgleichssoftware wird von Azure verwaltet. Alle VMs auf Webebene in der privaten AVS-Cloud verwenden private IP-Adressen und erfordern keine zusätzlichen NAT-Regeln oder öffentlichen IP-Adressen zum Auflösen von Namen. VMs der Webschicht kommunizieren mit Azure Application Gateway über eine private Verbindung mit niedriger Latenz und hoher Bandbreite.

Weitere Informationen zum Konfigurieren dieser Lösung finden Sie im Lösungsleitfaden zum Verwenden von Azure Application Gateway als L7-Lastenausgleich.

## <a name="azure-internal-load-balancer"></a>Interner Azure-Lastenausgleich

Wenn Sie Ihre Anwendung in einer Hybridbereitstellung ausführen möchten, bei der die Web-Front-End-Ebene innerhalb eines Azure-VNET in Ihrem Azure-Abonnement und die DB-Ebene der Anwendung auf VMware-VMs in der privaten AVS-Cloud ausgeführt wird, können Sie den internen Lastenausgleich (L4-Lastenausgleich) von Azure vor Ihren VMs der DB-Ebene für die Datenverkehrsverwaltung verwenden.

Weitere Informationen finden Sie in der Azure-Dokumentation zum [internen Lastenausgleich](../load-balancer/concepts-limitations.md#internalloadbalancer).

## <a name="global-server-load-balancer"></a>Globaler Serverlastenausgleich

Wenn Sie nach einem DNS-basierten Lastenausgleich suchen, können Sie entweder Lösungen von Drittanbietern verwenden, die in Azure Marketplace verfügbar sind, oder die native Azure-Lösung nutzen.

Azure Traffic Manager ist ein DNS-basierter Lastenausgleich für Datenverkehr, der es Ihnen ermöglicht, den Datenverkehr optimal auf Dienste in den globalen Azure-Regionen und in der lokalen Umgebung zu verteilen, und gleichzeitig Hochverfügbarkeit und Reaktionsfähigkeit sicherzustellen. Weitere Informationen finden Sie in der Azure-Dokumentation zu [Traffic Manager](../traffic-manager/traffic-manager-configure-geographic-routing-method.md).
