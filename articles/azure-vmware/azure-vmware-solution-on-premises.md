---
title: Verbinden von Azure VMware Solution mit Ihrer lokalen Umgebung
description: Es wird beschrieben, wie Sie Azure VMware Solution mit Ihrer lokalen Umgebung verbinden.
ms.topic: tutorial
ms.date: 10/02/2020
ms.openlocfilehash: 2a0cb641df00f3e580e87e38aff382d8e8101fc7
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2020
ms.locfileid: "91578763"
---
# <a name="connect-azure-vmware-solution-to-your-on-premises-environment"></a>Verbinden von Azure VMware Solution mit Ihrer lokalen Umgebung

In diesem Artikel verwenden Sie weiterhin die [während der Planung gesammelten Informationen](production-ready-deployment-steps.md), um für Azure VMware Solution eine Verbindung mit Ihrer lokalen Umgebung herzustellen.

Zunächst müssen zwei Voraussetzungen für die Verbindungsherstellung zwischen Azure VMware Solution und Ihrer lokalen Umgebung erfüllt werden:

- Eine ExpressRoute-Leitung von Ihrer lokalen Umgebung zu Azure.
- Ein Netzwerkadressblock vom Typ „/29“ ohne Überschneidungen für das ExpressRoute Global Reach-Peering, das Sie während der [Planungsphase](production-ready-deployment-steps.md) definiert haben.

>[!NOTE]
> Sie können auch eine Verbindung per VPN herstellen, aber die Beschreibung der Vorgehensweise würde den Rahmen dieser Schnellstartanleitung sprengen.

## <a name="establish-an-expressroute-global-reach-connection"></a>Einrichten einer ExpressRoute Global Reach-Verbindung

Befolgen Sie die Anleitung im Tutorial[Zusammenfassen lokaler Umgebungen mittels Peering zu einer privaten Cloud](tutorial-expressroute-global-reach-private-cloud.md), um die lokale Konnektivität mit Ihrer privaten Azure VMware Solution-Cloud per ExpressRoute Global Reach herzustellen.



## <a name="verify-on-premises-network-connectivity"></a>Überprüfen der Netzwerkkonnektivität für die lokale Umgebung

Sie sollten nun auf Ihrem **lokalen Edgerouter** sehen können, wie die NSX-T-Netzwerksegmente und die Azure VMware Solution-Verwaltungssegmente per ExpressRoute verbunden sind.

>[!NOTE]
>Die Umgebungen von Unternehmen unterscheiden sich, und in einigen Fällen kann es erforderlich sein, dass für diese Routen die Weitergabe zurück an das lokale Netzwerk zugelassen wird.  

In einigen Umgebungen ist die ExpressRoute-Leitung durch Firewalls geschützt.  Falls keine Firewalls vorhanden sind und keine Routenbereinigung durchgeführt wird, können Sie aus Ihrer lokalen Umgebung einen Ping an Ihren VCenter Server von Azure VMware Solution oder an eine [VM](deploy-azure-vmware-solution.md#add-a-vm-on-the-nsx-t-network-segment) im NSX-T-Segment senden.

Darüber hinaus können Sie von der VM im NSX-T-Segment aus auch Ressourcen in Ihrer lokalen Umgebung erreichen.

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem nächsten Abschnitt fort, in dem es um das Bereitstellen und Konfigurieren von VMware HCX geht.

> [!div class="nextstepaction"]
> [Bereitstellen und Konfigurieren von VMware HCX](tutorial-deploy-vmware-hcx.md)