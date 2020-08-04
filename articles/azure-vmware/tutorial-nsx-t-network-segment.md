---
title: 'Tutorial: Erstellen eines NSX-T-Netzwerksegments in Azure VMware Solution (AVS)'
description: In diesem Tutorial haben Sie die NSX-T-Netzwerksegmente erstellt, die für virtuelle Computer in vCenter verwendet werden.
ms.topic: tutorial
ms.date: 07/16/2020
ms.openlocfilehash: 5654fbb6a063d4dfeb541c20407f9a09dff1509f
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87093584"
---
# <a name="tutorial-create-an-nsx-t-network-segment-in-azure-vmware-solution-avs"></a>Tutorial: Erstellen eines NSX-T-Netzwerksegments in Azure VMware Solution (AVS)

In NSX-T Manager erstellte Netzwerksegmente werden als Netzwerke für virtuelle Computer (VMs) in vCenter verwendet. Die in vCenter erstellten virtuellen Computer werden in den in NSX-T erstellten Netzwerksegmenten platziert und sind in vCenter sichtbar.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Navigieren in NSX-T Manager zum Hinzufügen von Netzwerksegmenten
> * Hinzufügen eines neuen Netzwerksegments
> * Beobachten des neuen Netzwerksegments in vCenter

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial sind eine private AVS-Cloud mit Zugriff auf die vCenter- und NSX-T Manager-Verwaltungsschnittstellen erforderlich. Eine exemplarische Vorgehensweise zu diesem Beispiel-App-Code finden Sie im [Tutorial: Konfigurieren des Netzwerks für Ihre private VMware-Cloud in Azure](tutorial-configure-networking.md).

## <a name="provision-a-network-segment-in-nsx-t"></a>Bereitstellen eines Netzwerksegments in NSX-T

1. Wählen Sie in der vCenter-Instanz für Ihre private Cloud **SDDC-Datacenter > Networks** (SDDC-Datacenter > Netzwerke) aus. Beachten Sie, dass noch keine Netzwerke vorhanden sind.

   :::image type="content" source="media/nsxt/vcenter-without-ls01.png" alt-text="Auswählen von „SDDC-Datacenter > Networks“ (SDDC-Datacenter > Netzwerke)":::

1. Wählen Sie in NSX-T Manager für Ihre private Cloud **Networking** (Netzwerk) aus.

   :::image type="content" source="media/nsxt/nsxt-network-overview.png" alt-text="Auswählen von „Networking“ (Netzwerk), um zur Übersicht von NSX-T Manager zu navigieren":::

1. Wählen Sie **Segments** (Segmente) aus.

   :::image type="content" source="media/nsxt/nsxt-select-segments.png" alt-text="Auswählen von „Segments“ (Segmente) in der Netzwerkübersicht":::

1. Wählen Sie auf der Übersichtsseite der NSX-T-Segmente die Option **ADD SEGMENT** (SEGMENT HINZUFÜGEN) aus. Im Rahmen der Bereitstellung der privaten Cloud werden drei Segmente erstellt. Diese können nicht für virtuelle Computer verwendet werden.  Für virtuelle Computer muss ein neues Netzwerksegment hinzugefügt werden.

   :::image type="content" source="media/nsxt/nsxt-segments-overview.png" alt-text="Auswählen von „Add Segment“ (Segment hinzufügen) auf der Übersichtsseite der Netzwerksegmente":::

1. Geben Sie einen Namen für das Segment ein, wählen Sie unter **Connected Gateway** (Verbundenes Gateway) das vorkonfigurierte Gateway der Ebene 1 (TNTxx-T1) aus, und übernehmen Sie für **Type** (Typ) die Option „Flex“. Wählen Sie unter **Transport Zone** (Transportzone) die vorkonfigurierte Überlagerung (TNTxx-OVERLAY-TZ) und anschließend „Set Subnets“ (Subnetze festlegen) aus. Für alle anderen Einstellungen in diesem Abschnitt sowie für die Einstellungen unter **PORTS** und **SEGMENT PROFILES** (SEGMENTPROFILE) können die Standardwerte übernommen werden.

   :::image type="content" source="media/nsxt/nsxt-create-segment-specs.png" alt-text="Festlegen von Segmentname, verbundenem Gateway und Typ sowie Transportzone und Auswählen von „Set Subnets“ (Subnetze festlegen)":::

1. Legen Sie die IP-Adresse des Gateways für das neue Segment fest, und wählen Sie dann **ADD** (HINZUFÜGEN) aus. Die von Ihnen verwendete IP-Adresse muss Teil eines nicht überlappenden RFC1918-Adressblocks sein. Dadurch wird sichergestellt, dass Sie eine Verbindung mit den virtuellen Computern im neuen Segment herstellen können.

   :::image type="content" source="media/nsxt/nsxt-create-segment-gateway.png" alt-text="Angeben der IP-Adresse des Segmentgateways in CIDR-Notation und Auswählen von „ADD“ (HINZUFÜGEN)":::

1. Wenden Sie das neue Netzwerksegment an, indem Sie **APPLY** (ANWENDEN) auswählen und die Konfiguration mithilfe der Option **SAVE** (SPEICHERN) speichern.

   :::image type="content" source="media/nsxt/nsxt-create-segment-apply.png" alt-text="Anwenden des neuen Netzwerksegments in der NSX-T-Konfiguration mit „APPLY“ (ANWENDEN)":::

   :::image type="content" source="media/nsxt/nsxt-create-segment-save.png" alt-text="Speichern des neuen Netzwerksegments in der NSX-T-Konfiguration mit „SAVE“ (SPEICHERN)":::

1. Das neue Netzwerksegment wurde nun erstellt, und Sie können die Option zum Fortsetzen der Konfiguration des Segments ablehnen, indem Sie **NO** (NEIN) auswählen.

   :::image type="content" source="media/nsxt/nsxt-create-segment-continue-no.png" alt-text="Ablehnen der weiteren Konfiguration des neu erstellten Netzwerksegments durch Auswählen von „NO“ (NEIN)":::

1. Vergewissern Sie sich, dass das neue Netzwerksegment in NSX-T vorhanden ist, indem Sie **Networking > Segments** (Netzwerk > Segmente) auswählen und überprüfen, ob sich das neue Segment in der Liste befindet (in diesem Fall „ls01“).

   :::image type="content" source="media/nsxt/nsxt-new-segment-overview-2.png" alt-text="Überprüfen, ob das neue Netzwerksegment in NSX-T vorhanden ist":::

1. Vergewissern Sie sich, dass das neue Netzwerksegment in vCenter vorhanden ist, indem Sie **Networking > SDDC-Datacenter** (Netzwerk > SDDC-Datacenter) auswählen und überprüfen, ob sich das neue Segment in der Liste befindet (in diesem Fall „ls01“).

   :::image type="content" source="media/nsxt/vcenter-with-ls01-2.png" alt-text="Überprüfen, ob das neue Netzwerksegment in vCenter vorhanden ist":::

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die NSX-T-Netzwerksegmente erstellt, die für virtuelle Computer in vCenter verwendet werden. Sie können nun die Informationen unter [Tutorial: Erstellen einer Inhaltsbibliothek zum Bereitstellen von VMs in Azure VMware Solution (AVS)](tutorial-deploy-vm-content-library.md) lesen, um eine Inhaltsbibliothek in vCenter zu erstellen und anschließend einen virtuellen Computer in dem Netzwerk bereitzustellen, das Sie in diesem Tutorial erstellt haben.

<!-- LINKS - external-->

<!-- LINKS - internal -->
