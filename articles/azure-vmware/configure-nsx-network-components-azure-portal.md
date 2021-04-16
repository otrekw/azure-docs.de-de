---
title: Konfigurieren von NSX-Netzwerkkomponenten in Azure VMware Solution
description: Erfahren Sie, wie Sie mit der Azure VMware Solution-Konsole NSX-T-Netzwerksegmente konfigurieren können.
ms.topic: how-to
ms.date: 02/16/2021
ms.openlocfilehash: a4702efe8ea26234d074f421d2e6eab871eba5f5
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105733096"
---
# <a name="configure-nsx-network-components-in-azure-vmware-solution"></a>Konfigurieren von NSX-Netzwerkkomponenten in Azure VMware Solution

Eine private Azure VMware Solution-Cloud ist standardmäßig als softwaredefiniertes Netzwerk (SDDC) in NSX-T enthalten. Sie wird vorab mit einem NSX-T Tier-0-Gateway im **Aktiv/Aktiv-Modus** und einem standardmäßigen NSX-T Tier-1-Gateway im Aktiv/Standby-Modus bereitgestellt.  Mit diesen Gateways können Sie eine Verbindung der Segmente herstellen (logische Switches) und Ost-West- und Nord-Süd-Konnektivität bereitstellen. 

Nach Bereitstellen der privaten Azure VMware Solution-Cloud können Sie die erforderlichen NSX-T-Objekte über die Azure VMware Solution-Konsole unter **Workloadnetzwerk** konfigurieren.  Die Konsole zeigt die vereinfachte Ansicht von NSX-T-Vorgängen an, die ein VMware-Administrator täglich benötigt, und die für Benutzer bestimmt ist, die mit NSX-T nicht vertraut sind.  

Sie haben vier Optionen zum Konfigurieren von NSX-T-Komponenten in der Azure VMware Solution-Konsole:
- **Segmente**: Erstellen Sie Segmente, die im NSX-T Manager und in vCenter angezeigt werden.
- **DHCP**: Erstellen Sie einen DHCP-Server oder ein DHCP-Relay, wenn Sie DHCP verwenden möchten.
- **Portspiegelung**: Erstellen Sie eine Portspiegelung, um Netzwerkprobleme zu beheben.
- **DNS**: Erstellen Sie eine DNS-Weiterleitung, um DNS-Anforderungen zur Auflösung an einen bestimmten DNS-Server zu senden.  

>[!NOTE]
>Sie haben weiterhin Zugriff auf die NSX-T Manager-Konsole, in der Sie die erwähnten erweiterten Einstellungen und andere NSX-T-Features verwenden können. 
 
:::image type="content" source="media/configure-nsx-network-components-azure-portal/nsxt-workload-networking.png" alt-text="Screenshot mit vier Optionen zum Konfigurieren von NSX-T in der Azure VMware Solution-Konsole.":::

## <a name="prerequisites"></a>Voraussetzungen
Virtuelle Computer (Virtual Machines, VMs), die in der privaten Azure VMware Solution-Cloud erstellt oder dorthin migriert werden, sollten einem Segment angefügt werden. 

## <a name="create-an-nsx-t-segment-in-the-azure-portal"></a>Erstellen eines NSX-T-Segments im Azure-Portal
Sie können ein NSX-T-Segment über die Azure VMware Solution-Konsole im Azure-Portal erstellen und konfigurieren.  Diese Segmente sind mit dem standardmäßigen Tier-1-Gateway verbunden, und die Workloads in diesen Segmenten erhalten Ost-West und Nord-Süd-Konnektivität. Nachdem Sie das Segment erstellt haben, wird es im NSX-T Manager und in vCenter angezeigt.

>[!NOTE]
>Wenn Sie DHCP verwenden möchten, müssen Sie [einen DHCP-Server oder ein DHCP-Relay](#create-a-dhcp-server-or-dhcp-relay-in-the-azure-portal) konfigurieren, bevor Sie ein NSX-T-Segment erstellen und konfigurieren können.

1. Wählen Sie in Ihrer privaten Azure VMware Solution-Cloud unter **Workloadnetzwerk** die Option **Segmente** > **Hinzufügen** aus. Geben Sie die Details für das neue logische Segment an, und klicken Sie auf **OK**.

   :::image type="content" source="media/configure-nsx-network-components-azure-portal/add-new-nsxt-segment.png" alt-text="Screenshot: Hinzufügen eines neuen Segments.":::
   
   - **Segmentname**: der Name des logischen Switches, der in vCenter sichtbar ist.
   - **Subnetzgateway**: Gateway-IP-Adresse für das Subnetz des logischen Switches mit einer Subnetzmaske. Virtuelle Computer werden einem logischen Switch angefügt, und alle VMs, die eine Verbindung mit diesem Switch herstellen, gehören zum selben Subnetz.  Außerdem müssen alle VMs, die diesem logischen Segment angefügt sind, eine IP-Adresse aus dem selben Segment enthalten.
   - **DHCP** (optional): DHCP-Bereiche für ein logisches Segment. Ein [DHCP-Server oder DHCP-Relay](#create-a-dhcp-server-or-dhcp-relay-in-the-azure-portal) muss für die Nutzung von DHCP in Segmenten konfiguriert werden.  
   - **Verbundenes Gateway**: - *Standardmäßig ausgewählt und schreibgeschützt.*  Tier-1-Gateway und Typ der Segmentinformationen. 
      - **T1**: Name des Tier-1-Gateways im NSX-T Manager. Eine private Azure VMware Solution-Cloud mit einem NSX-T Tier-0-Gateway im Aktiv/Aktiv-Modus und einem standardmäßigen NSX-T Tier-1-Gateway im Aktiv/Standby-Modus.  Segmente, die über die Azure VMware Solution-Konsole erstellt werden, stellen nur eine Verbindung mit dem standardmäßigen NSX-T Tier-1-Gateway her, und die Workloads dieser Segmente erhalten Ost-West- und Nord-Süd-Konnektivität. Sie können nur über den NSX-T Manager weitere Tier-1-Gateways erstellen. Von der NSX-T Manager-Konsole erstellte Tier-1-Gateways sind in der Azure VMware Solution-Konsole nicht sichtbar. 
      - **Typ**: Überlagerungssegment, das von Azure VMware Solution unterstützt wird.

   Das Segment ist nun in der Azure VMware Solution-Konsole, im NSX-T Manager und in vCenter sichtbar.

## <a name="create-a-dhcp-server-or-dhcp-relay-in-the-azure-portal"></a>Erstellen eines DHCP-Servers oder DHCP-Relays im Azure-Portal
Sie können einen DHCP-Server oder ein -Relay direkt über die Azure VMware Solution-Konsole im Azure-Portal erstellen. Der DHCP-Server oder das -Relay stellt eine Verbindung mit dem Tier-1-Gateway her, das beim Bereitstellen der Azure VMware Solution-Instanz erstellt wird. Alle Segmente, in denen Sie DHCP-Bereiche vergeben haben, werden Teil dieses DHCPs.  Nachdem Sie einen DHCP-Server oder ein DHCP-Relay erstellt haben, müssen Sie zu dessen Verwendung ein Subnetz oder einen Bereich auf Segmentebene definieren.

1. Wählen Sie in Ihrer privaten Azure VMware Solution-Cloud unter **Workloadnetzwerk** die Option **DHCP** > **Hinzufügen** aus.

2. Wählen Sie entweder **DHCP-Server** oder **DHCP-Relay** aus, und geben Sie dann einen Namen für den Server oder das Relay und drei IP-Adressen an. 

   >[!NOTE]
   >Bei einem DHCP-Relay ist für eine erfolgreiche Konfiguration nur eine IP-Adresse erforderlich.

   :::image type="content" source="media/configure-nsx-network-components-azure-portal/add-dhcp-server-relay.png" alt-text="Screenshot, der das Hinzufügen eines DHCP-Servers oder -Relays in Azure VMware Solution zeigt.":::

4. Vervollständigen Sie die DHCP-Konfiguration, indem Sie [DHCP-Bereiche in den logischen Segmenten bereitstellen](#create-an-nsx-t-segment-in-the-azure-portal), und wählen Sie dann **OK**.
 
## <a name="configure-port-mirroring-in-the-azure-portal"></a>Konfigurieren der Portspiegelung im Azure-Portal
Sie können die Portspiegelung zum Überwachen des Netzwerkdatenverkehrs konfigurieren, was die Weiterleitung einer Kopie jedes Pakets von einem Netzwerkswitchport an einen anderen einschließt. Mit dieser Option wird ein Protokollanalysetool auf dem Port platziert, das die gespiegelten Daten empfängt. Es analysiert den Datenverkehr, der von einer Quelle, einem virtuellen Computer oder einer Gruppe von VMs an ein definiertes Ziel gesendet wird. 

Zum Einrichten der Portspiegelung in der Azure VMware Solution-Konsole gehen Sie wie folgt vor:

* [Schritt 1. Erstellen von Quell- und Ziel-VMs oder -VM-Gruppen](#step-1-create-source-and-destination-vms-or-vm-groups): Die Quellgruppe verfügt über eine einzelne VM oder mehrere virtuelle Computer, auf denen der Datenverkehr gespiegelt wird.

* [Schritt 2. Erstellen eines Portspiegelungsprofils](#step-2-create-a-port-mirroring-profile): Sie definieren die Richtung des Datenverkehrs für die Quell- und Ziel-VM-Gruppen.

### <a name="step-1-create-source-and-destination-vms-or-vm-groups"></a>Schritt 1: Erstellen von Quell- und Ziel-VMs oder -VM-Gruppen

In diesem Schritt erstellen Sie eine Quell-VM-Gruppe und eine Ziel-VM-Gruppe.

1. Wählen Sie in Ihrer privaten Azure VMware Solution-Cloud unter **Workloadnetzwerk** die Option **Portspiegelung** > **VM-Gruppen** > **Hinzufügen** aus.

   :::image type="content" source="media/configure-nsx-network-components-azure-portal/add-port-mirroring-vm-groups.png" alt-text="Screenshot, der das Erstellen einer VM-Gruppe für die Portspiegelung zeigt.":::

1. Geben Sie einen Namen für die neue VM-Gruppe an, wählen Sie die gewünschten VMs aus der Liste aus, und klicken Sie dann auf **OK**.

   :::image type="content" source="media/configure-nsx-network-components-azure-portal/add-vm-group.png" alt-text="Screenshot, der die Liste der VMs zeigt, die der VM-Gruppe hinzugefügt werden sollen.":::

1. Wiederholen Sie diese Schritte, um die Ziel-VM-Gruppe zu erstellen.

### <a name="step-2-create-a-port-mirroring-profile"></a>Schritt 2: Erstellen eines Portspiegelungsprofils

In diesem Schritt definieren Sie ein Profil für die Datenverkehrsrichtung der Quell- und Ziel-VM-Gruppe.

>[!NOTE]
>Stellen Sie sicher, dass die Quell- und Ziel-VM-Gruppe erstellt wurden.

1. Wählen Sie **Portspiegelung** > **Hinzufügen** aus, und geben Sie Folgendes an:

   :::image type="content" source="media/configure-nsx-network-components-azure-portal/add-port-mirroring-profile.png" alt-text="Screenshot der Informationen, die für das Portspiegelungsprofil erforderlich sind.":::

   - **Name der Portspiegelung**: beschreibender Name für das Profil.
   - **Richtung**: Wählen Sie zwischen „Eingehend“, „Ausgehend“ und „Bidirektional“.
   - **Quelle**: Wählen Sie die Quell-VM-Gruppe aus.
   - **Ziel**: Wählen Sie die Ziel-VM-Gruppe aus.
   - **Beschreibung**: Geben Sie eine Beschreibung für die Portspiegelung ein.

1. Wählen Sie **OK** aus, um das Profil abzuschließen. 

   Das Profil und die VM-Gruppen werden in der Azure VMware Solution-Konsole angezeigt.

## <a name="configure-a-dns-forwarder-in-the-azure-portal"></a>Konfigurieren einer DNS-Weiterleitung im Azure-Portal
Sie konfigurieren eine DNS-Weiterleitung, bei der bestimmte DNS-Anforderungen zur Auflösung an einen bestimmten DNS-Server weitergeleitet werden.  Eine DNS-Weiterleitung wird einer **standardmäßigen DNS-Zone** und bis zu drei **FQDN-Zonen** zugeordnet.

>[!TIP]
>Sie können auch die [NSX-T Manager-Konsole verwenden, um eine DNS-Weiterleitung zu konfigurieren](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/2.5/administration/GUID-A0172881-BB25-4992-A499-14F9BE3BE7F2.html).

Zum Einrichten einer DNS-Weiterleitung in der Azure VMware Solution-Konsole gehen Sie wie folgt vor:

* [Schritt 1. Konfigurieren einer DNS-Standard Zone und FQDN-Zone](#step-1-configure-a-default-dns-zone-and-fqdn-zone): Wenn eine DNS-Abfrage empfangen wird, vergleicht eine DNS-Weiterleitung den Domänennamen mit den Domänennamen in der FQDN-DNS-Zone. 

* [Schritt 2. Konfigurieren des DNS-Diensts](#step-2-configure-dns-service): Sie konfigurieren den DNS-Weiterleitungsdienst.

### <a name="step-1-configure-a-default-dns-zone-and-fqdn-zone"></a>Schritt 1: Konfigurieren einer DNS-Standard Zone und FQDN-Zone
Sie konfigurieren eine Standard-DNS-Zone und FQDN-Zone, um DNS-Abfragen an den Upstreamserver zu senden.  Bei Empfang einer DNS-Abfrage vergleicht die DNS-Weiterleitung den Domänennamen in der Abfrage mit den FQDN-DNS-Zonen-Domänennamen. Wenn eine Entsprechung gefunden wird, wird die Abfrage an die DNS-Server weitergeleitet, die in der FQDN-DNS-Zone angegeben sind. Wenn keine Entsprechung gefunden wird, wird die Abfrage an die DNS-Server weitergeleitet, die in der Standard-DNS-Zone angegeben sind.

>[!NOTE]
>Eine Standard-DNS-Zone muss definiert werden, bevor Sie eine FQDN-Zone konfigurieren. 

1. Wählen Sie in Ihrer privaten Azure VMware Solution-Cloud unter **Workloadnetzwerk** die Option **DNS** > **DNS-Zonen** > **Hinzufügen** aus.

   :::image type="content" source="media/configure-nsx-network-components-azure-portal/nsxt-workload-networking-dns-zones.png" alt-text="Screenshot, der zeigt, wie DNS-Zonen und ein DNS-Dienst hinzugefügt werden.":::

1. Wählen Sie **Standard-DNS-Zone** auswählen aus, und geben Sie Folgendes an:

   :::image type="content" source="media/configure-nsx-network-components-azure-portal/nsxt-workload-networking-configure-dns-zones.png" alt-text="Screenshot, der zeigt, wie Sie eine Standard-DNS-Zone hinzufügen.":::

   1. Ein Name für die DNS-Zone.

   1. Bis zu drei IP-Adressen des DNS-Servers im Format **8.8.8.8**.

1. Wählen Sie **FQDN-Zone** aus, und geben Sie Folgendes an:

   :::image type="content" source="media/configure-nsx-network-components-azure-portal/nsxt-workload-networking-configure-fqdn-zone.png" alt-text="Screenshot: Hinzufügen einer FQDN-Zone.":::

   1. Einen Namen für die DNS-Zone.

   1. Die FQDN-Domäne.

   1. Bis zu drei IP-Adressen des DNS-Servers im Format **8.8.8.8**.

1. Wählen Sie **OK** aus, um die Standard-DNS-Zone und den DNS-Dienst hinzuzufügen.

### <a name="step-2-configure-dns-service"></a>Schritt 2: Konfigurieren des DNS-Diensts

1. Klicken Sie auf die Registerkarte **DNS service** (DNS-Dienst) und dann auf **Hinzufügen**. Geben Sie die Details ein, und klicken Sie auf **OK**.

   :::image type="content" source="media/configure-nsx-network-components-azure-portal/nsxt-workload-networking-configure-dns-service.png" alt-text="Screenshot der Informationen, die für den DNS-Dienst erforderlich sind.":::

   >[!TIP]
   >**Tier-1-Gateway** ist standardmäßig ausgewählt und spiegelt das Gateway wider, das beim Bereitstellen der Azure VMware Solution-Instanz erstellt wurde.

   Der DNS-Dienst wurde erfolgreich hinzugefügt.

   :::image type="content" source="media/configure-nsx-network-components-azure-portal/nsxt-workload-networking-configure-dns-service-success.png" alt-text="Screenshot, der das erfolgreiche Hinzufügen des DNS-Diensts anzeigt.":::

