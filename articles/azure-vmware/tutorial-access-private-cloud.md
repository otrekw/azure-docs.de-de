---
title: 'Tutorial: Zugreifen auf Ihre private Cloud'
description: Hier erfahren Sie, wie Sie auf eine private Azure VMware Solution-Cloud zugreifen
ms.topic: tutorial
ms.date: 02/22/2021
ms.openlocfilehash: 456767a9edd78a70a0aba45c7b44a2150a2217a1
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102045002"
---
# <a name="tutorial-access-an-azure-vmware-solution-private-cloud"></a>Tutorial: Zugreifen auf eine private Azure VMware Solution-Cloud

Azure VMware Solution lässt die Verwaltung der privaten Cloud mit Ihrer lokalen vCenter-Instanz nicht zu. Daher sind zusätzliche Einrichtungsschritte erforderlich, und es muss über eine Jumpbox eine Verbindung mit einer lokalen vCenter-Instanz hergestellt werden. 

In diesem Tutorial erstellen Sie eine Jumpbox in der Ressourcengruppe, die Sie im [vorherigen Tutorial](tutorial-configure-networking.md) erstellt haben, und melden sich bei vCenter an. Die Jumpbox ist ein virtueller Windows-Computer im selben virtuellen Netzwerk, das Sie erstellt haben.  Sie ermöglicht den Zugriff auf vCenter und NSX Manager. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines virtuellen Windows-Computers zum Herstellen der Verbindung mit vCenter
> * Anmelden bei vCenter über Ihren virtuellen Computer

## <a name="create-a-new-windows-virtual-machine"></a>Erstellen eines neuen virtuellen Windows-Computers

[!INCLUDE [create-avs-jump-box-steps](includes/create-jump-box-steps.md)]

## <a name="connect-to-the-local-vcenter-of-your-private-cloud"></a>Herstellen einer Verbindung mit der lokalen vCenter-Instanz Ihrer privaten Cloud

1. Melden Sie sich über die Jumpbox mit einmaligem Anmelden von VMware vCenter beim vSphere-Client an. Verwenden Sie dazu einen Cloudadministrator-Benutzernamen, und vergewissern Sie sich, dass die Benutzeroberfläche angezeigt wird.

1. Wählen Sie im Azure-Portal Ihre private Cloud und anschließend **Verwalten** > **Identität** aus. 

   Die URLs und Benutzeranmeldeinformationen für vCenter und NSX-T Manager der privaten Cloud werden angezeigt.

   >[!TIP]
   >Wählen Sie **Neues Kennwort generieren** aus, um neue vCenter- und NSX-T-Kennwörter zu generieren.

   :::image type="content" source="media/tutorial-access-private-cloud/ss4-display-identity.png" alt-text="Anzeigen der URLs und Anmeldeinformationen für vCenter und NSX Manager der privaten Cloud" border="true" lightbox="media/tutorial-access-private-cloud/ss4-display-identity.png":::

1. Navigieren Sie zu dem virtuellen Computer, den Sie im vorherigen Schritt erstellt haben, und stellen Sie eine Verbindung mit dem virtuellen Computer her. 

   Wenn Sie Hilfe beim Herstellen einer Verbindung mit dem virtuellen Computer benötigen, finden Sie ausführliche Informationen unter [Verbinden mit dem virtuellen Computer](../virtual-machines/windows/connect-logon.md#connect-to-the-virtual-machine).

1. Öffnen Sie auf dem virtuellen Windows-Computer einen Browser, und navigieren Sie auf zwei Tabs zu den URLs für vCenter und NSX-T Manager. 

1. Geben Sie auf dem vCenter-Tab die Benutzeranmeldeinformationen `cloudadmin@vmcp.local` aus dem vorherigen Schritt ein.

   :::image type="content" source="media/tutorial-access-private-cloud/ss5-vcenter-login.png" alt-text="Anmelden bei der privaten Cloud: vCenter" border="true":::

   :::image type="content" source="media/tutorial-access-private-cloud/ss6-vsphere-client-home.png" alt-text="vCenter-Portal" border="true":::

1. Melden Sie sich auf dem zweiten Browsertab bei NSX-T Manager an.

   :::image type="content" source="media/tutorial-access-private-cloud/ss10-nsx-manager-home.png" alt-text="Melden Sie sich auf dem zweiten Browsertab bei NSX-T Manager an." border="true":::



## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines virtuellen Windows-Computers zum Herstellen der Verbindung mit vCenter
> * Anmelden bei vCenter über Ihren virtuellen Computer

Im nächsten Tutorial erfahren Sie, wie Sie im Rahmen der Einrichtung der lokalen Verwaltung für Ihre privaten Cloudcluster ein virtuelles Netzwerk erstellen.

> [!div class="nextstepaction"]
> [Erstellen eines virtuellen Netzwerks](tutorial-configure-networking.md)


