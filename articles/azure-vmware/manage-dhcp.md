---
title: Erstellen und Verwalten von DHCP
description: In diesem Artikel wird beschrieben, wie Sie DHCP in Azure VMware Solution verwalten.
ms.topic: conceptual
ms.date: 10/22/2020
ms.openlocfilehash: cb74ed4474cc14081e59142f2f60915fccd47559
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2020
ms.locfileid: "92461055"
---
# <a name="how-to-create-and-manage-dhcp-in-azure-vmware-solution"></a>Erstellen und Verwalten von DHCP in Azure VMware Solution

NSX-T bietet die Möglichkeit, DHCP für Ihre private Cloud zu konfigurieren. Wenn Sie NSX-T zum Hosten Ihres DHCP-Servers verwenden, finden Sie weitere Informationen unter [Erstellen eines DHCP-Servers](#create-dhcp-server). Wenn Sie in Ihrem Netzwerk einen externen DHCP-Server eines Drittanbieters haben, finden Sie weitere Informationen unter [Erstellen eines DHCP-Relaydiensts](#create-dhcp-relay-service).

## <a name="create-dhcp-server"></a>Erstellen des DHCP-Servers

Führen Sie die folgenden Schritte aus, um einen DHCP-Server in NSX-T zu konfigurieren.

1. Navigieren Sie in NSX Manager zur Registerkarte **Networking** (Netzwerk), und wählen Sie **DHCP** aus. 
1. Wählen Sie **ADD SERVER** (Server hinzufügen) aus, und geben Sie den Servernamen und die IP-Adresse an. 
1. Wählen Sie **Speichern** aus.

:::image type="content" source="./media/manage-dhcp/dhcp-server-settings.png" alt-text="DHCP-Server hinzufügen" border="true":::

### <a name="connect-dhcp-server-to-the-tier-1-gateway"></a>Verbinden Sie den DHCP-Server mit dem Gateway der Ebene 1.

1. Wählen Sie **Tier-1-Gateways** (Gateways der Ebene 1), das Gateway in der Liste und dann **Edit** (Bearbeiten) aus.

   :::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway.png" alt-text="Das zu verwendende Gateway auswählen" border="true":::

1. Wählen Sie **No IP Allocation Set** (Kein IP-Zuteilungssatz) aus, um ein Subnetz hinzuzufügen.

   :::image type="content" source="./media/manage-dhcp/add-subnet.png" alt-text="Hinzufügen eines Subnetzes" border="true":::

1. Wählen Sie für **Type** (Typ) **DHCP Local Server** (Lokaler DHCP-Server) aus. 
1. Wählen Sie **Default DHCP** (Standard-DHCP) für **DHCP Server** und dann **Save** (Speichern) aus.


1. Wählen Sie im Fenster **Tier-1-Gateway** (Gateway der Ebene 1) die Option **Save** (Speichern) aus. 
1. Wählen Sie abschließend **Close Editing** (Bearbeitung beenden) aus.

### <a name="add-a-network-segment"></a>Hinzufügen eines Netzwerksegments

Nachdem Sie den DHCP-Server erstellt haben, müssen Sie ihm Netzwerksegmente hinzufügen.

1. Wählen Sie in NSX-T auf der Registerkarte **Networking** (Netzwerk) unter **Connectivity** (Konnektivität) **Segments** (Segmente) aus. 
1. Wählen Sie **ADD SEGMENT** (Segment hinzufügen) aus, und benennen Sie das Segment und die Verbindung mit dem Tier-1 Gateway. 
1. Wählen Sie **Set Subnets** (Subnetze festlegen) aus, um ein neues Subnetz zu konfigurieren. 

   :::image type="content" source="./media/manage-dhcp/add-segment.png" alt-text="Neues Netzwerksegment hinzufügen" border="true":::

1. Wählen Sie im Fenster **Set Subnets** (Subnetze festlegen) **ADD SUBNET** (Subnetz hinzufügen) aus. 
1. Geben Sie die IP-Adresse des Gateways und den DHCP-Bereich ein, und wählen Sie **Add** (Hinzufügen) und dann **Apply** (Übernehmen) aus.

1. Wählen Sie **Save** (Speichern) aus, um das neue Netzwerksegment hinzuzufügen.

## <a name="create-dhcp-relay-service"></a>Erstellen eines DHCP-Relaydiensts

1. Navigieren Sie die Registerkarte **Networking** (Netzwerk) und unter **IP Management** (IP-Verwaltung) **DHCP** aus. 
1. Wählen Sie **ADD SERVER** (Server hinzufügen) aus. 
1. Wählen Sie „DHCP Relay“ als **Server Type** (Servertyp), und geben Sie den Namen und die IP-Adresse des Relayservers ein. 
1. Wählen Sie **Speichern** aus.

   :::image type="content" source="./media/manage-dhcp/create-dhcp-relay.png" alt-text="DHCP-Relayserver erstellen" border="true":::

1. Wählen Sie unter **Connectivity** (Konnektivität) **Tier-1 Gateways** (Gateways der Ebene 1) aus. 
1. Wählen Sie das vertikale Auslassungszeichen für das Tier-1 Gateway (Gateway der Ebene 1) und dann **Edit** (Bearbeiten) aus.

   :::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway-relay.png" alt-text="Gateway der Ebene 1 bearbeiten" border="true":::

1. Wählen Sie **No IP Allocation Set** (Keine IP-Zuteilung festgelegt) aus, um die Zuteilung von IP-Adressen zu definieren.

   :::image type="content" source="./media/manage-dhcp/edit-ip-address-allocation.png" alt-text="Zuteilung von IP-Adressen bearbeiten" border="true":::

1. Wählen Sie **DHCP Relay Server** für **Type** aus.
1. Wählen Sie Ihren DHCP-Relayserver für **DHCP Relay** aus. 
1. Wählen Sie **Speichern** aus.


## <a name="specify-a-dhcp-range-ip-on-a-segment"></a>Angeben eines IP-Adressbereichs für DHCP für ein Segment

> [!NOTE]
> Diese Konfiguration ist erforderlich, um die DHCP-Relayfunktionalität für das DHCP-Clientsegment zu implementieren. 

1. Wählen Sie unter **Connectivity** (Konnektivität) **Segments** (Segmente) aus. 
1. Wählen Sie die vertikalen Auslassungszeichen und dann **Edit** (Bearbeiten) aus. 

   >[!TIP]
   >Wenn Sie ein neues Segment hinzufügen möchten, wählen Sie **Add Segment** (Segment hinzufügen) aus.

1. Fügen Sie Details zum Segment hinzu. 
1. Wählen Sie den Wert unter **Set Subnets** (Subnetze festlegen) aus, um das Subnetz hinzuzufügen oder zu ändern.

   :::image type="content" source="./media/manage-dhcp/network-segments.png" alt-text="Netzwerksegmente" border="true":::

1. Wählen Sie die vertikalen Auslassungszeichen und dann **Edit** (Bearbeiten) aus. Wenn Sie ein neues Subnetz erstellen müssen, wählen Sie **Add Subnet** (Subnetz hinzufügen) aus, um ein Gateway zu erstellen und einen DHCP-Bereich zu konfigurieren. 
1. Geben Sie den Bereich des IP-Adressenpools an. Wählen Sie **Apply** (Übernehmen) und dann **Save** (Speichern) aus.

   :::image type="content" source="./media/manage-dhcp/edit-subnet.png" alt-text="Subnetze bearbeiten" border="true":::

   Dem Segment ist ein DHCP-Serverpool zugewiesen.

   :::image type="content" source="./media/manage-dhcp/assigned-to-segment.png" alt-text="Einem Segment zugewiesener DHCP-Serverpool" border="true":::
