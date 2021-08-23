---
title: Konfigurieren und Verwalten von DHCP in Azure VMware Solution
description: Erfahren Sie, wie Sie DHCP für Ihre private Azure VMware Solution-Cloud einrichten und verwalten.
ms.topic: how-to
ms.custom: contperf-fy21q2
ms.date: 05/17/2021
ms.openlocfilehash: 28fb995cbc91d6e350e8d295c1f57d62c706e59e
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110104051"
---
# <a name="configure-and-manage-dhcp-in-azure-vmware-solution"></a>Konfigurieren und Verwalten von DHCP in Azure VMware Solution

Anwendungen und Workloads, die in einer privaten Cloudumgebung ausgeführt werden, erfordern DHCP-Dienste für die IP-Adresszuweisung.  In diesem Artikel erfahren Sie, welche beiden Möglichkeiten zum Erstellen und Verwalten von DHCP in Azure VMware Solution zur Verfügung stehen:

- Bei Verwendung von NSX-T zum Hosten Ihres DHCP-Servers müssen Sie [einen DHCP-Server erstellen](#create-a-dhcp-server) und eine [Weiterleitung zu diesem Server einrichten](#create-dhcp-relay-service). Wenn Sie den DHCP-Server erstellen, fügen Sie auch ein Netzwerksegment hinzu und geben den IP-Adressbereich für DHCP an.   

- Wenn Sie einen externen DHCP-Server eines Drittanbieters in Ihrem Netzwerk verwenden, müssen Sie [einen DHCP-Relaydienst erstellen](#create-dhcp-relay-service). Wenn Sie einen Relaydienst zu einem DHCP-Server erstellen, müssen Sie den DHCP-IP-Adressbereich angeben, unabhängig davon, ob NSX-T oder einen Drittanbieterdienst zum Hosten des DHCP-Servers verwendet wird.

>[!IMPORTANT]
>DHCP kann für VMs in einem gestreckten VMware HCX L2-Netzwerk nicht verwendet werden, wenn der DHCP-Server sich in einem lokalen Rechenzentrum befindet.  NSX blockiert standardmäßig den Durchlauf aller DHCP-Anforderungen durch das gestreckte L2-Netzwerk. Die Lösung finden Sie in der Vorgehensweise zum [Konfigurieren von DHCP in gestreckten VMware HCX L2-Netzwerken](configure-l2-stretched-vmware-hcx-networks.md).


## <a name="create-a-dhcp-server"></a>Erstellen eines DHCP-Servers

Wenn Sie NSX-T zum Hosten Ihres DHCP-Servers verwenden möchten, erstellen Sie einen DHCP-Server. Fügen Sie dann ein Netzwerksegment hinzu, und geben Sie den DHCP-IP-Adressbereich an.

1. Wählen Sie in NSX-T Manager die Optionen **Networking** > **DHCP** (Netzwerk > DHCP) und dann **Add Server** (Server hinzufügen) aus.

1. Wählen Sie **DHCP** unter **Server Type** (Servertyp) aus, geben Sie den Servernamen und die IP-Adresse an, und wählen Sie dann **Save** (Speichern) aus.

   :::image type="content" source="./media/manage-dhcp/dhcp-server-settings.png" alt-text="DHCP-Server hinzufügen" border="true":::

1. Wählen Sie **Tier-1 Gateways** (Gateways der Ebene 1), dann die vertikalen Auslassungspunkte für das Tier-1 Gateway (Gateway der Ebene 1) und anschließend **Edit** (Bearbeiten) aus.

   :::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway.png" alt-text="Das zu verwendende Gateway auswählen" border="true":::

1. Wählen Sie **No IP Allocation Set** (Kein IP-Zuteilungssatz) aus, um ein Subnetz hinzuzufügen.

   :::image type="content" source="./media/manage-dhcp/add-subnet.png" alt-text="Hinzufügen eines Subnetzes" border="true":::

1. Wählen Sie unter **Type** (Typ) die Option **DHCP Local Server** (Lokaler DHCP-Server) aus. 
   
1. Wählen Sie **Default DHCP** (Standard-DHCP) für **DHCP Server** und dann **Save** (Speichern) aus.

1. Wählen Sie erneut **Save** (Speichern) und dann **Close Editing** (Bearbeitung beenden) aus.

### <a name="add-a-network-segment"></a>Hinzufügen eines Netzwerksegments

[!INCLUDE [add-network-segment-steps](includes/add-network-segment-steps.md)]


## <a name="create-dhcp-relay-service"></a>Erstellen eines DHCP-Relaydiensts

Wenn Sie einen externen DHCP-Server eines Drittanbieters verwenden möchten, müssen Sie einen DHCP-Relaydienst erstellen. Außerdem geben Sie den DHCP-IP-Adressbereich in NSX-T Manager an. 

1. Wählen Sie in NSX-T Manager die Optionen **Networking** > **DHCP** (Netzwerk > DHCP) und dann **Add Server** (Server hinzufügen) aus.

1. Wählen Sie **DHCP Relay** unter **Server Type** (Servertyp) aus, geben Sie den Servernamen und die IP-Adresse an, und wählen Sie dann **Save** (Speichern) aus.

   :::image type="content" source="./media/manage-dhcp/create-dhcp-relay.png" alt-text="Erstellen eines DHCP-Relaydiensts" border="true":::

1. Wählen Sie **Tier-1 Gateways** (Gateways der Ebene 1), dann die vertikalen Auslassungspunkte für das Tier-1 Gateway (Gateway der Ebene 1) und anschließend **Edit** (Bearbeiten) aus.

   :::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway-relay.png" alt-text="Gateway der Ebene 1 bearbeiten" border="true":::

1. Wählen Sie **No IP Allocation Set** (Keine IP-Zuteilung festgelegt) aus, um die Zuteilung von IP-Adressen zu definieren.

   :::image type="content" source="./media/manage-dhcp/edit-ip-address-allocation.png" alt-text="Zuteilung von IP-Adressen bearbeiten" border="true":::

1. Wählen Sie unter **Type** (Typ) die Option **DHCP Server** aus. 
   
1. Wählen Sie **DHCP Relay** für **DHCP Server** und dann **Save** (Speichern) aus.

1. Wählen Sie erneut **Save** (Speichern) und dann **Close Editing** (Bearbeitung beenden) aus.


## <a name="specify-the-dhcp-ip-address-range"></a>Angeben des DHCP-IP-Adressbereichs

1. Wählen Sie in NSX-T Manager die Optionen **Networking** > **Segments** (Netzwerk > Segmente) aus. 
   
1. Wählen Sie die vertikalen Auslassungspunkte für den Segmentnamen und dann **Edit** (Bearbeiten) aus.
   
1. Wählen Sie **Set Subnets** (Subnetze festlegen) aus, um die DHCP-IP-Adresse für das Subnetz anzugeben. 
   
   :::image type="content" source="./media/manage-dhcp/network-segments.png" alt-text="Netzwerksegmente" border="true":::
      
1. Ändern Sie ggf. die Gateway-IP-Adresse, und geben Sie den IP-Adressbereich für DHCP ein. 
      
   :::image type="content" source="./media/manage-dhcp/edit-subnet.png" alt-text="Subnetze bearbeiten" border="true":::
      
1. Wählen Sie **Apply** (Übernehmen) und dann **Save** (Speichern) aus. Das Segment wird einem DHCP-Serverpool zugewiesen.
      
   :::image type="content" source="./media/manage-dhcp/assigned-to-segment.png" alt-text="Einem Segment zugewiesener DHCP-Serverpool" border="true":::



## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie ein neues Sicherheitssegmentprofil, wenn Sie DHCP-Anforderungen von Ihren Azure VMware Solution-VMs an einen Nicht-NSX-T-DHCP-Server senden möchten. Die Lösung finden Sie in der Vorgehensweise zum [Konfigurieren von DHCP in gestreckten VMware HCX L2-Netzwerken](configure-l2-stretched-vmware-hcx-networks.md).

