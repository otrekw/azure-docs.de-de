---
title: Erstellen und Verwalten von DHCP
description: In diesem Artikel wird beschrieben, wie Sie DHCP in Azure VMware Solution verwalten.
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 3fc3de228179925afdf1b7c1015c577fd9c4c924
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2020
ms.locfileid: "88752159"
---
# <a name="how-to-create-and-manage-dhcp-in-azure-vmware-solution"></a>Erstellen und Verwalten von DHCP in Azure VMware Solution

NSX-T bietet die Möglichkeit, DHCP für Ihre private Cloud zu konfigurieren. Wenn Sie vorhaben, NSX-T zum Hosten Ihres DHCP-Servers zu verwenden, finden Sie weitere Informationen unter [Erstellen eines DHCP-Servers](#create-dhcp-server). Wenn Sie hingegen einen externen DHCP-Server eines Drittanbieters in Ihrem Netzwerk betreiben und Anforderungen an diesen weiterleiten möchten, finden Sie weitere Informationen unter [Erstellen eines DHCP-Relaydiensts](#create-dhcp-relay-service).

## <a name="create-dhcp-server"></a>Erstellen des DHCP-Servers

Führen Sie die folgenden Schritte aus, um einen DHCP-Server in NSX-T zu konfigurieren.

Navigieren Sie im NSX-Manager zur Registerkarte **Networking** (Netzwerk), und wählen Sie unter **IP Management** (IP-Verwaltung) **DHCP** aus. Wählen Sie die Schaltfläche **ADD SERVER** (Server hinzufügen) aus. Geben Sie dann den Namen und die IP-Adresse des Servers an. Wählen Sie anschließend **Save** (Speichern) aus.

:::image type="content" source="./media/manage-dhcp/dhcp-server-settings.png" alt-text="DHCP-Server hinzufügen" border="true":::

### <a name="connect-dhcp-server-to-the-tier-1-gateway"></a>Verbinden Sie den DHCP-Server mit dem Gateway der Ebene 1.

1. Wählen Sie **Tier-1-Gateways** (Gateways der Ebene 1), anschließend das Gateway und schließlich **Edit** (Bearbeiten) aus.

   :::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway.png" alt-text="Das zu verwendende Gateway auswählen" border="true":::

1. Fügen Sie ein Subnetz hinzu, indem Sie **No IP Allocation Set** (Keine IP-Zuteilung festgelegt) auswählen.

   :::image type="content" source="./media/manage-dhcp/add-subnet.png" alt-text="Hinzufügen eines Subnetzes" border="true":::

1. Wählen Sie auf dem nächsten Bildschirm **DHCP Local Server** (Lokaler DHCP-Server) in der Dropdownliste **Type** (Typ) aus. Wählen Sie für **DHCP Server** die Option **Default DHCP** (Standard-DHCP) und dann **Save** (Speichern) aus.

   :::image type="content" source="./media/manage-dhcp/set-ip-address-management.png" alt-text="Optionen für DHCP-Server auswählen" border="true":::

1. Wählen Sie im Fenster **Tier-1-Gateway** (Gateway der Ebene 1) die Option **Save** (Speichern) aus. Auf dem nächsten Bildschirm sehen Sie **Changes Saved** (Änderungen gespeichert). Wählen Sie **Close Editing** (Bearbeitung beenden) aus.

### <a name="add-a-network-segment"></a>Hinzufügen eines Netzwerksegments

Nachdem Sie den DHCP-Server erstellt haben, müssen Sie ihm Netzwerksegmente hinzufügen.

1. Wählen Sie in NSX-T auf der Registerkarte **Networking** (Netzwerk) unter **Connectivity** (Konnektivität) **Segments** (Segmente) aus. Wählen Sie **ADD SEGMENT** (Segment hinzufügen) aus. Benennen Sie das Segment und die Verbindung mit dem Gateway der Ebene 1. Wählen Sie als Nächstes **Set Subnets** (Subnetze festlegen) aus, um ein neues Subnetz zu konfigurieren. 

   :::image type="content" source="./media/manage-dhcp/add-segment.png" alt-text="Neues Netzwerksegment hinzufügen" border="true":::

1. Wählen Sie im Fenster **Set Subnets** (Subnetze festlegen) **ADD SUBNET** (Subnetz hinzufügen) aus. Geben Sie die IP-Adresse des Gateways und den DHCP-Bereich ein, und wählen Sie **Add** (Hinzufügen) und dann **Apply** (Übernehmen) aus.

   :::image type="content" source="./media/manage-dhcp/add-subnet-segment.png" alt-text="Netzwerksegment hinzufügen" border="true":::

1. Wählen Sie anschließend **Save** (Speichern) aus, um das Hinzufügen eines Netzwerksegments abzuschließen.

   :::image type="content" source="./media/manage-dhcp/segments-complete.png" alt-text="Segmente vollständig" border="true":::

## <a name="create-dhcp-relay-service"></a>Erstellen eines DHCP-Relaydiensts

1. Navigieren Sie im Fenster „NXT-T“ zur Registerkarte **Networking** (Netzwerk), und wählen Sie unter **IP Management** (IP-Verwaltung) **DHCP** aus. Wählen Sie **ADD SERVER** (Server hinzufügen) aus. Wählen Sie „DHCP Relay“ als **Servertyp**, und geben Sie den Namen und die IP-Adresse des Relayservers ein. Wählen Sie **Speichern**, um Ihre Änderungen zu speichern.

   :::image type="content" source="./media/manage-dhcp/create-dhcp-relay.png" alt-text="DHCP-Relayserver erstellen" border="true":::

1. Wählen Sie unter **Connectivity** (Konnektivität) **Tier-1 Gateways** (Gateways der Ebene 1) aus. Wählen Sie das vertikale Auslassungszeichen für das Gateway der Ebene 1 und dann **Edit** (Bearbeiten) aus.

   :::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway-relay.png" alt-text="Gateway der Ebene 1 bearbeiten" border="true":::

1. Wählen Sie **No IP Allocation Set** (Keine IP-Zuteilung festgelegt) aus, um die Zuteilung von IP-Adressen zu definieren.

   :::image type="content" source="./media/manage-dhcp/edit-ip-address-allocation.png" alt-text="Zuteilung von IP-Adressen bearbeiten" border="true":::

1. Wählen Sie im Dialogfeld für **Type** (Typ) **DHCP Relay Server** (DHCP-Relayserver) aus. Wählen Sie in der Dropdownliste **DHCP Relay** Ihren DHCP-Relayserver aus. Wenn Sie fertig sind, wählen Sie **Save**  (Speichern) aus.

   :::image type="content" source="./media/manage-dhcp/set-ip-address-management-relay.png" alt-text="IP-Adressverwaltung festlegen" border="true":::

## <a name="specify-a-dhcp-range-ip-on-segment"></a>Angeben eines IP-Adressbereichs für DHCP für „Segment“

> [!NOTE]
> Diese Konfiguration ist erforderlich, um die DHCP-Relayfunktionalität für das DHCP-Clientsegment zu implementieren. 

1. Wählen Sie unter **Connectivity** (Konnektivität) **Segments** (Segmente) aus. Wählen Sie die vertikalen Auslassungszeichen und dann **Edit** (Bearbeiten) aus. Wenn Sie stattdessen ein neues Segment hinzufügen möchten, können Sie **Segment hinzufügen** wählen, um ein neues Segment zu erstellen.

   :::image type="content" source="./media/manage-dhcp/edit-segments.png" alt-text="Subnetz des Netzwerks bearbeiten" border="true":::

1. Fügen Sie Details zum Segment hinzu. Wählen Sie den Wert unter **Subnets** (Subnetze) oder **Set Subnets** (Subnetze festlegen) aus, um das Subnetz hinzuzufügen oder zu ändern.

   :::image type="content" source="./media/manage-dhcp/network-segments.png" alt-text="Netzwerksegmente" border="true":::

1. Wählen Sie die vertikalen Auslassungszeichen und dann **Edit** (Bearbeiten) aus. Wenn Sie ein neues Subnetz erstellen müssen, wählen Sie **Add Subnet** (Subnetz hinzufügen) aus, um ein Gateway zu erstellen und einen DHCP-Bereich zu konfigurieren. Geben Sie den Bereich des IP-Adressenpools an. Wählen Sie **Apply** (Übernehmen) und dann **Save** (Speichern) aus.

   :::image type="content" source="./media/manage-dhcp/edit-subnet.png" alt-text="Subnetze bearbeiten" border="true":::

1. Nun ist dem Segment ein DHCP-Serverpool zugewiesen.

   :::image type="content" source="./media/manage-dhcp/assigned-to-segment.png" alt-text="Einem Segment zugewiesener DHCP-Serverpool" border="true":::
