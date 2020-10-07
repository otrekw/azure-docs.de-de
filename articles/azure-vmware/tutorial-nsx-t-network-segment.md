---
title: 'Tutorial: Erstellen eines NSX-T-Netzwerksegments in Azure VMware Solution'
description: Hier erfahren Sie, wie Sie die NSX-T-Netzwerksegmente erstellen, die für virtuelle Computer in vCenter verwendet werden.
ms.topic: tutorial
ms.date: 09/21/2020
ms.openlocfilehash: 780cac15efc043b9ae44b77af1234adca3fec5a1
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91254532"
---
# <a name="tutorial-create-an-nsx-t-network-segment-in-azure-vmware-solution"></a>Tutorial: Erstellen eines NSX-T-Netzwerksegments in Azure VMware Solution

In NSX-T Manager erstellte Netzwerksegmente werden als Netzwerke für virtuelle Computer (VMs) in vCenter verwendet. Die in vCenter erstellten virtuellen Computer werden in den in NSX-T erstellten Netzwerksegmenten platziert und sind in vCenter sichtbar.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Navigieren in NSX-T Manager zum Hinzufügen von Netzwerksegmenten
> * Hinzufügen eines neuen Netzwerksegments
> * Beobachten des neuen Netzwerksegments in vCenter

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial sind eine private Azure VMware Solution-Cloud mit Zugriff auf die vCenter- und NSX-T Manager-Verwaltungsschnittstellen erforderlich. Weitere Informationen finden Sie unter [Tutorial: Konfigurieren des Netzwerks für Ihre private VMware-Cloud in Azure](tutorial-configure-networking.md).

## <a name="provision-a-network-segment-in-nsx-t"></a>Bereitstellen eines Netzwerksegments in NSX-T

1. Wählen Sie in der vCenter-Instanz für Ihre private Cloud **SDDC-Datacenter > Networks** (SDDC-Datacenter > Netzwerke) aus. Beachten Sie, dass noch keine Netzwerke vorhanden sind.

   :::image type="content" source="media/nsxt/vcenter-without-ls01.png" alt-text="Wählen Sie in der vCenter-Instanz für Ihre private Cloud „SDDC-Datacenter > Networks“ (SDDC-Datacenter > Netzwerke) aus. Beachten Sie, dass noch keine Netzwerke vorhanden sind.":::

1. Wählen Sie in NSX-T Manager für Ihre private Cloud **Networking** (Netzwerk) aus.

   :::image type="content" source="media/nsxt/nsxt-network-overview.png" alt-text="Wählen Sie in der vCenter-Instanz für Ihre private Cloud „SDDC-Datacenter > Networks“ (SDDC-Datacenter > Netzwerke) aus. Beachten Sie, dass noch keine Netzwerke vorhanden sind.":::

1. Wählen Sie **Segments** (Segmente) aus.

   :::image type="content" source="media/nsxt/nsxt-select-segments.png" alt-text="Wählen Sie in der vCenter-Instanz für Ihre private Cloud „SDDC-Datacenter > Networks“ (SDDC-Datacenter > Netzwerke) aus. Beachten Sie, dass noch keine Netzwerke vorhanden sind.":::

1. Wählen Sie auf der Übersichtsseite der NSX-T-Segmente die Option **ADD SEGMENT** (SEGMENT HINZUFÜGEN) aus. Im Rahmen der Bereitstellung der privaten Cloud werden drei Segmente erstellt. Diese können nicht für virtuelle Computer verwendet werden.  Für virtuelle Computer muss ein neues Netzwerksegment hinzugefügt werden.

   :::image type="content" source="media/nsxt/nsxt-segments-overview.png" alt-text="Wählen Sie in der vCenter-Instanz für Ihre private Cloud „SDDC-Datacenter > Networks“ (SDDC-Datacenter > Netzwerke) aus. Beachten Sie, dass noch keine Netzwerke vorhanden sind.":::

1. Geben Sie einen Namen für das Segment ein, wählen Sie unter **Connected Gateway** (Verbundenes Gateway) das vorkonfigurierte Gateway der Ebene 1 (TNTxx-T1) aus, und übernehmen Sie für **Type** (Typ) die Option „Flex“. Wählen Sie unter **Transport Zone** (Transportzone) die vorkonfigurierte Überlagerung (TNTxx-OVERLAY-TZ) und anschließend „Set Subnets“ (Subnetze festlegen) aus. Für alle anderen Einstellungen in diesem Abschnitt sowie für die Einstellungen unter **PORTS** und **SEGMENT PROFILES** (SEGMENTPROFILE) können die Standardwerte übernommen werden.

   :::image type="content" source="media/nsxt/nsxt-create-segment-specs.png" alt-text="Wählen Sie in der vCenter-Instanz für Ihre private Cloud „SDDC-Datacenter > Networks“ (SDDC-Datacenter > Netzwerke) aus. Beachten Sie, dass noch keine Netzwerke vorhanden sind.":::

1. Legen Sie die IP-Adresse des Gateways für das neue Segment fest, und wählen Sie dann **ADD** (HINZUFÜGEN) aus. Die von Ihnen verwendete IP-Adresse muss Teil eines nicht überlappenden RFC1918-Adressblocks sein. Dadurch wird sichergestellt, dass Sie eine Verbindung mit den virtuellen Computern im neuen Segment herstellen können.

   :::image type="content" source="media/nsxt/nsxt-create-segment-gateway.png" alt-text="Wählen Sie in der vCenter-Instanz für Ihre private Cloud „SDDC-Datacenter > Networks“ (SDDC-Datacenter > Netzwerke) aus. Beachten Sie, dass noch keine Netzwerke vorhanden sind.":::

1. Wenden Sie das neue Netzwerksegment an, indem Sie **APPLY** (ANWENDEN) auswählen und die Konfiguration mithilfe der Option **SAVE** (SPEICHERN) speichern.

   :::image type="content" source="media/nsxt/nsxt-create-segment-apply.png" alt-text="Wählen Sie in der vCenter-Instanz für Ihre private Cloud „SDDC-Datacenter > Networks“ (SDDC-Datacenter > Netzwerke) aus. Beachten Sie, dass noch keine Netzwerke vorhanden sind.":::

   :::image type="content" source="media/nsxt/nsxt-create-segment-save.png" alt-text="Wählen Sie in der vCenter-Instanz für Ihre private Cloud „SDDC-Datacenter > Networks“ (SDDC-Datacenter > Netzwerke) aus. Beachten Sie, dass noch keine Netzwerke vorhanden sind.":::

1. Das neue Netzwerksegment wurde nun erstellt, und Sie können die Option zum Fortsetzen der Konfiguration des Segments ablehnen, indem Sie **NO** (NEIN) auswählen.

   :::image type="content" source="media/nsxt/nsxt-create-segment-continue-no.png" alt-text="Wählen Sie in der vCenter-Instanz für Ihre private Cloud „SDDC-Datacenter > Networks“ (SDDC-Datacenter > Netzwerke) aus. Beachten Sie, dass noch keine Netzwerke vorhanden sind.":::

1. Vergewissern Sie sich, dass das neue Netzwerksegment in NSX-T vorhanden ist, indem Sie **Networking > Segments** (Netzwerk > Segmente) auswählen und überprüfen, ob sich das neue Segment in der Liste befindet (in diesem Fall „ls01“).

   :::image type="content" source="media/nsxt/nsxt-new-segment-overview-2.png" alt-text="Wählen Sie in der vCenter-Instanz für Ihre private Cloud „SDDC-Datacenter > Networks“ (SDDC-Datacenter > Netzwerke) aus. Beachten Sie, dass noch keine Netzwerke vorhanden sind.":::

1. Vergewissern Sie sich, dass das neue Netzwerksegment in vCenter vorhanden ist, indem Sie **Networking > SDDC-Datacenter** (Netzwerk > SDDC-Datacenter) auswählen und überprüfen, ob sich das neue Segment in der Liste befindet (in diesem Fall „ls01“).

   :::image type="content" source="media/nsxt/vcenter-with-ls01-2.png" alt-text="Wählen Sie in der vCenter-Instanz für Ihre private Cloud „SDDC-Datacenter > Networks“ (SDDC-Datacenter > Netzwerke) aus. Beachten Sie, dass noch keine Netzwerke vorhanden sind.":::

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die NSX-T-Netzwerksegmente erstellt, die für virtuelle Computer in vCenter verwendet werden. Sie können nun eine [Inhaltsbibliothek zum Bereitstellen von virtuellen Computern in Azure VMware Solution erstellen](deploy-vm-content-library.md) und anschließend einen virtuellen Computer in dem Netzwerk bereitstellen, das Sie in diesem Tutorial erstellt haben.

Fahren Sie andernfalls mit dem nächsten Tutorial fort, um zu erfahren, wie Sie ExpressRoute Global Reach-Peering mit einer privaten Cloud in einer Azure VMware Solution-Instanz erstellen.

> [!div class="nextstepaction"]
> [Zusammenfassen lokaler Umgebungen mittels Peering zu einer privaten Cloud](tutorial-expressroute-global-reach-private-cloud.md)

<!-- LINKS - external-->

<!-- LINKS - internal -->
