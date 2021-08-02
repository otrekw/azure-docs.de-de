---
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 06/02/2021
ms.author: cherylmc
ms.openlocfilehash: cf11d4614cd4b6c3c387ef83b63da506a1702afc
ms.sourcegitcommit: c385af80989f6555ef3dadc17117a78764f83963
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111411904"
---
### <a name="i-am-a-network-appliance-partner-and-want-to-get-our-nva-in-the-hub-can-i-join-this-partner-program"></a>Ich bin ein Partner für Netzwerkgeräte und möchte die Bereitstellung unseres NVA im Hub ermöglichen. Kann ich an diesem Partnerprogramm teilnehmen?

Leider haben wir zurzeit keine Kapazität für neue Partnerangebote. Versuchen Sie es später noch einmal.

### <a name="can-i-deploy-any-nva-from-azure-marketplace-into-the-virtual-wan-hub"></a>Kann ich ein beliebiges NVA aus dem Azure Marketplace im Virtual WAN-Hub bereitstellen?

Zurzeit sind nur [Barracuda CloudGen WAN](https://aka.ms/BarracudaMarketPlaceOffer), [Cisco Cloud vWAN Application](https://azuremarketplace.microsoft.com/marketplace/apps/cisco.cisco_cloud_vwan_app?tab=Overview) und [VMware SD-WAN](https://aka.ms/vmwareMarketplaceLink) zur Bereitstellung im Virtual WAN-Hub verfügbar.

### <a name="what-is-the-cost-of-the-nva"></a>Welche Kosten fallen für das NVA an?

Sie müssen eine Lizenz für das virtuelle Netzwerkgerät (NVA) vom NVA-Anbieter erwerben. Informationen zur Lizenz für Ihr Barracuda CloudGen WAN-NVA von Barracuda finden Sie auf der Seite zum [Barracuda CloudGen WAN](https://www.barracuda.com/products/cloudgenwan). Cisco bietet zurzeit nur das BYOL-Lizenzierungsmodell (Bring Your Own License, Verwendung Ihrer eigenen Lizenz) an, das direkt von Cisco erhalten werden muss. Außerdem stellt Ihnen Microsoft Gebühren für die NVA-Infrastruktureinheiten und anderen Ressourcen in Rechnung, die Sie nutzen. Weitere Informationen finden Sie unter [Informationen zu den Preisen von Virtual WAN](../articles/virtual-wan/pricing-concepts.md).

### <a name="can-i-deploy-an-nva-to-a-basic-hub"></a>Kann ich ein NVA in einem Basic-Hub bereitstellen?

Nein. Sie müssen einen Standard-Hub verwenden, wenn Sie ein NVA bereitstellen möchten.

### <a name="can-i-deploy-an-nva-into-a-secure-hub"></a>Kann ich ein NVA in einem geschützten Hub bereitstellen?

Ja. Partner-NVAs können in einem Hub mit Azure Firewall bereitgestellt werden.

### <a name="can-i-connect-any-cpe-device-in-my-branch-office-to-barracuda-cloudgen-wan-nva-in-the-hub"></a>Kann ich ein beliebiges CPE-Gerät in meiner Zweigstelle mit dem Barracuda CloudGen WAN-NVA im Hub verbinden?

Nein. Barracuda CloudGen WAN ist nur mit CPE-Geräten von Barracuda kompatibel. Weitere Informationen zu den Anforderungen von CloudGen WAN finden Sie auf der [Seite zu CloudGen WAN von Barracuda](https://www.barracuda.com/products/cloudgenwan). Für Cisco gibt es mehrere kompatible SD-WAN-CPE-Geräte. Weitere Informationen zu kompatiblen CPEs finden Sie in der Dokumentation zu [Cisco Cloud OnRamp for Multi-Cloud](https://www.cisco.com/c/en/us/td/docs/routers/sdwan/configuration/cloudonramp/ios-xe-17/cloud-onramp-book-xe/cloud-onramp-multi-cloud.html#Cisco_Concept.dita_c61e0e7a-fff8-4080-afee-47b81e8df701).

### <a name="what-routing-scenarios-are-supported-with-nva-in-the-hub"></a>Welche Routingszenarien werden mit NVAs im Hub unterstützt?

Alle von Virtual WAN unterstützten Routingszenarien werden mit NVAs im Hub unterstützt.
