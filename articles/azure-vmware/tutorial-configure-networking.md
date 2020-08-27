---
title: 'Tutorial: Konfigurieren des Netzwerks für Ihre private VMware-Cloud in Azure'
description: Hier erfahren Sie, wie Sie das erforderliche Netzwerk zum Bereitstellen Ihrer privaten Cloud in Azure erstellen und konfigurieren.
ms.topic: tutorial
ms.date: 07/22/2020
ms.openlocfilehash: ff071e0d6eaf1552634433a76e4eade530c603b6
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2020
ms.locfileid: "88750501"
---
# <a name="tutorial-configure-networking-for-your-vmware-private-cloud-in-azure"></a>Tutorial: Konfigurieren des Netzwerks für Ihre private VMware-Cloud in Azure

Eine private Azure VMware Solution-Cloud erfordert ein virtuelles Azure-Netzwerk. Da Azure VMware Solution Ihre lokale vCenter-Instanz in der Vorschauversion nicht unterstützt, müssen Sie zusätzliche Schritte für die Integration in Ihre lokale Umgebung durchführen. Die Einrichtung einer ExpressRoute-Leitung und eines Gateways für das virtuelle Netzwerk ist ebenfalls erforderlich und wird in diesem Tutorial behandelt.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks
> * Erstellen eines Gateways für das virtuelle Netzwerk
> * Verbinden der ExpressRoute-Leitung mit dem Gateway
> * Ermitteln der URLs für vCenter und NSX Manager

## <a name="prerequisites"></a>Voraussetzungen 
Vergewissern Sie sich, dass Sie eine [private Azure VMware Solution-Cloud](tutorial-create-private-cloud.md) erstellt haben, damit Sie ein virtuelles Netzwerk erstellen können. 

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Navigieren Sie zu der Ressourcengruppe, die Sie im [Tutorial zum Erstellen einer privaten Cloud](tutorial-create-private-cloud.md) erstellt haben, und wählen Sie **+ Hinzufügen** aus, um eine neue Ressource zu definieren. 

1. Geben Sie **Virtuelles Netzwerk** in das Feld **Marketplace durchsuchen** ein. Suchen Sie nach der VNET-Ressource.

1. Wählen Sie auf der Seite **Virtuelles Netzwerk** die Option **Erstellen** aus, um Ihr virtuelles Netzwerk für die private Cloud einzurichten.

1. Geben Sie auf der Seite **Virtuelles Netzwerk erstellen** die Details für Ihr virtuelles Netzwerk ein.

1. Geben Sie auf der Registerkarte **Grundlagen** einen Namen für das virtuelle Netzwerk ein, wählen Sie die entsprechende Region und dann **Weiter: IP-Adressen**.

1. Geben Sie auf der Registerkarte **IP-Adressen** unter **IPv4-Adressraum** den Adressraum ein, den Sie im vorherigen Tutorial erstellt haben.

   > [!IMPORTANT]
   > Sie müssen einen Adressraum verwenden, der sich **nicht** mit dem Adressraum überlappt, den Sie im vorherigen Tutorial beim Erstellen der privaten Cloud verwendet haben.

1. Wählen Sie **+ Subnetz hinzufügen** aus, und geben Sie auf der Seite **Subnetz hinzufügen** einen Namen und einen entsprechenden Adressbereich für das Subnetz an. Wählen Sie zum Abschluss **Hinzufügen** aus.

1. Klicken Sie auf **Überprüfen + erstellen**.

   :::image type="content" source="./media/tutorial-configure-networking/create-virtual-network.png" alt-text="Auswählen von „Überprüfen + erstellen“" border="true":::

1. Überprüfen Sie die Angaben, und wählen Sie dann **Erstellen** aus. Nach Abschluss der Bereitstellung wird Ihr virtuelles Netzwerk in der Ressourcengruppe angezeigt.

## <a name="create-a-virtual-network-gateway"></a>Erstellen eines Gateways für das virtuelle Netzwerk

Sie haben ein virtuelles Netzwerk erstellt und erstellen nun ein Gateway für das virtuelle Netzwerk.

1. Wählen Sie in Ihrer Ressourcengruppe **+ Hinzufügen** aus, um eine neue Ressource hinzuzufügen.

1. Geben Sie **Gateway für virtuelle Netzwerke** in das Feld **Marketplace durchsuchen** ein. Suchen Sie nach der VNET-Ressource.

1. Wählen Sie auf der Seite **Gateway für virtuelle Netzwerke** die Option **Erstellen** aus.

1. Geben Sie auf der Registerkarte „Grundlagen“ der Seite **Gateway für virtuelle Netzwerke erstellen** Werte für die Felder an, und wählen Sie dann **Überprüfen + erstellen** aus. 

   | Feld | Wert |
   | --- | --- |
   | **Abonnement** | Dieser Wert ist bereits mit dem Abonnement aufgefüllt, zu dem die Ressourcengruppe gehört. |
   | **Ressourcengruppe** | Dieser Wert ist bereits für die aktuelle Ressourcengruppe aufgefüllt. Es sollte die Ressourcengruppe eingetragen sein, die Sie bei einem vorherigen Test erstellt haben. |
   | **Name** | Geben Sie einen eindeutigen Namen für das Gateway des virtuellen Netzwerks ein. |
   | **Region** | Wählen Sie den geografischen Standort des Gateways des virtuellen Netzwerks aus. |
   | **Gatewaytyp** | Wählen Sie **ExpressRoute** aus. |
   | **SKU** | Übernehmen Sie den Standardwert : **Standard**. |
   | **Virtuelles Netzwerk** | Wählen Sie das zuvor erstellte virtuelle Netzwerk aus. Falls das virtuelle Netzwerk nicht angezeigt wird, vergewissern Sie sich, dass die Region des Gateways mit der Ihres virtuellen Netzwerks übereinstimmt. |
   | **Adressbereich für Gatewaysubnetz** | Dieser Wert wird aufgefüllt, wenn Sie das virtuelle Netzwerk auswählen. Lassen Sie den Standardwert unverändert. |
   | **Öffentliche IP-Adresse** | Wählen Sie **Neu erstellen**. |

   :::image type="content" source="./media/tutorial-configure-networking/create-virtual-network-gateway.png" alt-text="Angeben von Werten für die Felder auf der Registerkarte „Grundlagen“ der Seite „Gateway für virtuelle Netzwerke erstellen“ und Auswählen von „Überprüfen + erstellen“" border="true":::

1. Überprüfen Sie die Details auf ihre Richtigkeit, und wählen Sie dann **Erstellen** aus, um die Bereitstellung des Gateways für virtuelle Netzwerke zu starten. 
1. Fahren Sie nach Abschluss der Bereitstellung mit dem nächsten Abschnitt fort, um Ihre ExpressRoute-Verbindung mit dem Gateway für virtuelle Netzwerke zu verbinden, das Ihre private Azure VMware Solution-Cloud enthält.

## <a name="connect-expressroute-to-the-virtual-network-gateway"></a>Verbinden von ExpressRoute mit dem Gateway für virtuelle Netzwerke

Sie haben ein Gateway für virtuelle Netzwerke bereitgestellt und fügen nun eine Verbindung zwischen dem Gateway und Ihrer privaten Azure VMware Solution-Cloud hinzu.

1. Navigieren Sie zu der privaten Cloud, die Sie im vorherigen Tutorial erstellt haben, wählen Sie unter **Verwalten** die Option **Konnektivität** und dann die Registerkarte **ExpressRoute** aus.

1. Kopieren Sie den Autorisierungsschlüssel. Falls kein Autorisierungsschlüssel angezeigt wird, müssen Sie einen erstellen, indem Sie **+ Autorisierungsschlüssel anfordern** auswählen.

   :::image type="content" source="./media/tutorial-configure-networking/request-auth-key.png" alt-text="Kopieren Sie den Autorisierungsschlüssel. Falls kein Autorisierungsschlüssel angezeigt wird, müssen Sie einen erstellen, indem Sie „Autorisierungsschlüssel anfordern“ auswählen." border="true":::

1. Navigieren Sie zu dem Gateway des virtuellen Netzwerks, das Sie im vorherigen Schritt erstellt haben, und wählen Sie unter **Einstellungen** die Option **Verbindungen** aus. Wählen Sie auf der Seite **Verbindungen** die Option **+ Hinzufügen** aus.

1. Geben Sie auf der Seite **Verbindung hinzufügen** Werte für die Felder an, und wählen Sie **OK** aus. 

   | Feld | Wert |
   | --- | --- |
   | **Name**  | Geben Sie einen Namen für die Verbindung ein.  |
   | **Verbindungstyp**  | Wählen Sie **ExpressRoute** aus.  |
   | **Autorisierung einlösen**  | Stellen Sie sicher, dass dieses Kontrollkästchen aktiviert ist.  |
   | **Gateway für virtuelle Netzwerke** | Das Gateway für virtuelle Netzwerke, das Sie zuvor erstellt haben  |
   | **Autorisierungsschlüssel**  | Kopieren Sie den Autorisierungsschlüssel von der Registerkarte „ExpressRoute“ für Ihre Ressourcengruppe, und fügen Sie ihn ein. |
   | **Peerleitungs-URI**  | Kopieren Sie die ExpressRoute-ID von der Registerkarte „ExpressRoute“ für Ihre Ressourcengruppe, und fügen Sie sie ein.  |

   :::image type="content" source="./media/tutorial-configure-networking/add-connection.png" alt-text="Geben Sie auf der Seite „Verbindung hinzufügen“ Werte für die Felder an, und wählen Sie „OK“ aus." border="true":::

Die Verbindung zwischen Ihrer ExpressRoute-Leitung und Ihrem virtuellen Netzwerk wird erstellt.



## <a name="locate-the-urls-for-vcenter-and-nsx-manager"></a>Ermitteln der URLs für vCenter und NSX Manager

Damit Sie sich bei vCenter und NSX Manager anmelden können, benötigen Sie die URLs des vCenter-Webclients und der NSX-T Manager-Website. 

Navigieren Sie zu Ihrer privaten Azure VMware Solution-Cloud, und wählen Sie unter **Verwalten** die Option **Identität** aus. Dort finden Sie die benötigten Informationen.

:::image type="content" source="./media/tutorial-configure-networking/locate-urls.png" alt-text="Navigieren Sie zu Ihrer privaten Azure VMware Solution-Cloud, und wählen Sie unter „Verwalten“ die Option „Identität“ aus. Dort finden Sie die benötigten Informationen." border="true":::

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks
> * Erstellen eines Gateways für das virtuelle Netzwerk
> * Verbinden der ExpressRoute-Leitung mit dem Gateway
> * Ermitteln der URLs für vCenter und NSX Manager

Im nächsten Tutorial erfahren Sie, wie Sie eine Jumpbox zum Herstellen einer Verbindung mit Ihrer Umgebung erstellen, damit Sie Ihre private Cloud lokal verwalten können.

> [!div class="nextstepaction"]
> [Zugreifen auf die private Cloud](tutorial-access-private-cloud.md)
