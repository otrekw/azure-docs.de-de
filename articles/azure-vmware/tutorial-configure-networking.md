---
title: 'Tutorial: Konfigurieren des Netzwerks für Ihre private VMware-Cloud in Azure'
description: Hier erfahren Sie, wie Sie das erforderliche Netzwerk zum Bereitstellen Ihrer privaten Cloud in Azure erstellen und konfigurieren.
ms.topic: tutorial
ms.date: 09/21/2020
ms.openlocfilehash: 6aff39284f3ea786080055552ac001ac5dd7b394
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91578348"
---
# <a name="tutorial-configure-networking-for-your-vmware-private-cloud-in-azure"></a>Tutorial: Konfigurieren des Netzwerks für Ihre private VMware-Cloud in Azure

Eine private Azure VMware Solution-Cloud erfordert ein virtuelles Azure-Netzwerk. Da Azure VMware Solution Ihre lokale vCenter-Instanz nicht unterstützt, müssen Sie zusätzliche Schritte für die Integration in Ihre lokale Umgebung durchführen. Die Einrichtung einer ExpressRoute-Leitung und eines Gateways für virtuelle Netzwerke ist ebenfalls erforderlich.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks
> * Erstellen eines Gateways für das virtuelle Netzwerk
> * Verbinden der ExpressRoute-Leitung mit dem Gateway
> * Ermitteln der URLs für vCenter und NSX Manager

## <a name="prerequisites"></a>Voraussetzungen 
Ein virtuelles Netzwerk, das Sie für eine [private Azure VMware Solution-Cloud](tutorial-create-private-cloud.md) erstellt haben. 

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

1. Geben Sie auf der Registerkarte „Grundlagen“ der Seite **Gateway für virtuelle Netzwerke erstellen** Werte für die Felder an, und wählen Sie dann **Überprüfen und erstellen** aus. 

   | Feld | Wert |
   | --- | --- |
   | **Abonnement** | Vorgegebener Wert mit dem Abonnement, zu dem die Ressourcengruppe gehört. |
   | **Ressourcengruppe** | Vorgegebener Wert für die aktuelle Ressourcengruppe. Bei dem Wert sollte es sich die Ressourcengruppe handeln, die Sie bei einem vorherigen Test erstellt haben. |
   | **Name** | Geben Sie einen eindeutigen Namen für das Gateway des virtuellen Netzwerks ein. |
   | **Region** | Wählen Sie den geografischen Standort des Gateways des virtuellen Netzwerks aus. |
   | **Gatewaytyp** | Wählen Sie **ExpressRoute** aus. |
   | **SKU** | Übernehmen Sie den Standardwert : **Standard**. |
   | **Virtuelles Netzwerk** | Wählen Sie das zuvor erstellte virtuelle Netzwerk aus. Sollte das virtuelle Netzwerk nicht angezeigt werden, vergewissern Sie sich, dass die Region des Gateways mit der Ihres virtuellen Netzwerks übereinstimmt. |
   | **Adressbereich für Gatewaysubnetz** | Dieser Wert wird aufgefüllt, wenn Sie das virtuelle Netzwerk auswählen. Lassen Sie den Standardwert unverändert. |
   | **Öffentliche IP-Adresse** | Wählen Sie **Neu erstellen**. |

   :::image type="content" source="./media/tutorial-configure-networking/create-virtual-network-gateway.png" alt-text="Auswählen von „Überprüfen + erstellen“" border="true":::

1. Überprüfen Sie die Details auf ihre Richtigkeit, und wählen Sie dann **Erstellen** aus, um die Bereitstellung des Gateways für virtuelle Netzwerke zu starten. 
1. Fahren Sie nach Abschluss der Bereitstellung mit dem nächsten Abschnitt fort, um Ihre ExpressRoute-Verbindung mit dem Gateway für virtuelle Netzwerke zu verbinden, das Ihre private Azure VMware Solution-Cloud enthält.

## <a name="connect-expressroute-to-the-virtual-network-gateway"></a>Verbinden von ExpressRoute mit dem Gateway für virtuelle Netzwerke

Sie haben ein Gateway für virtuelle Netzwerke bereitgestellt und fügen nun eine Verbindung zwischen dem Gateway und Ihrer privaten Azure VMware Solution-Cloud hinzu.

[!INCLUDE [connect-expressroute-to-vnet](includes/connect-expressroute-vnet.md)]


## <a name="locate-the-urls-for-vcenter-and-nsx-manager"></a>Ermitteln der URLs für vCenter und NSX Manager

Damit Sie sich bei vCenter und NSX Manager anmelden können, benötigen Sie die URLs des vCenter-Webclients und der NSX-T Manager-Website. 

Navigieren Sie zu Ihrer privaten Azure VMware Solution-Cloud, und wählen Sie unter **Verwalten** die Option **Identität** aus. Dort finden Sie die benötigten Informationen.

:::image type="content" source="./media/tutorial-configure-networking/locate-urls.png" alt-text="Auswählen von „Überprüfen + erstellen“" border="true":::

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks
> * Erstellen eines Gateways für das virtuelle Netzwerk
> * Verbinden der ExpressRoute-Leitung mit dem Gateway
> * Ermitteln der URLs für vCenter und NSX Manager

Im nächsten Tutorial erfahren Sie, wie Sie die NSX-T-Netzwerksegmente erstellen, die für virtuelle Computer in vCenter verwendet werden.

> [!div class="nextstepaction"]
> [Tutorial: Erstellen eines NSX-T-Netzwerksegments in Azure VMware Solution](tutorial-nsx-t-network-segment.md)