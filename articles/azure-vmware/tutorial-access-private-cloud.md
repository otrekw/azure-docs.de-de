---
title: 'Tutorial: Zugreifen auf Ihre private Cloud'
description: Hier erfahren Sie, wie Sie auf eine private Azure VMware Solution-Cloud zugreifen
ms.topic: tutorial
ms.date: 03/13/2021
ms.openlocfilehash: 81927e9ad0362ba340bb704d2d7e8b9c0927efbe
ms.sourcegitcommit: 3941df51ce4fca760797fa4e09216fcfb5d2d8f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2021
ms.locfileid: "114605406"
---
# <a name="tutorial-access-an-azure-vmware-solution-private-cloud"></a>Tutorial: Zugreifen auf eine private Azure VMware Solution-Cloud

Azure VMware Solution lässt die Verwaltung der privaten Cloud mit Ihrer lokalen vCenter-Instanz nicht zu. Sie müssen über eine Jumpbox eine Verbindung mit der Azure VMware Solution vCenter-Instanz herstellen. 

In diesem Tutorial erstellen Sie eine Jumpbox in der Ressourcengruppe, die Sie im [vorherigen Tutorial](tutorial-configure-networking.md) erstellt haben, und melden sich bei Azure VMware Solution vCenter an. Diese Jumpbox ist ein virtueller Windows-Computer im selben virtuellen Netzwerk, das Sie erstellt haben.  Sie ermöglicht den Zugriff auf vCenter und NSX Manager. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines virtuellen Windows-Computers für den Zugriff auf Azure VMware Solution vCenter
> * Anmelden bei vCenter über diesen virtuellen Computer

## <a name="create-a-new-windows-virtual-machine"></a>Erstellen eines neuen virtuellen Windows-Computers

1. Wählen Sie in der Ressourcengruppe **+ Hinzufügen** aus, suchen Sie nach **Microsoft Windows 10**, wählen Sie die Option aus, und wählen Sie anschließend **Erstellen** aus.

   :::image type="content" source="media/tutorial-access-private-cloud/ss8-azure-w10vm-create.png" alt-text="Screenshot: Hinzufügen eines neuen virtuellen Windows 10-Computers für eine Jumpbox":::

1. Geben Sie die erforderlichen Informationen in die Felder ein, und wählen Sie anschließend **Überprüfen + erstellen** aus. 

   Weitere Informationen zu den Feldern finden Sie in der folgenden Tabelle:

   | Feld | Wert |
   | --- | --- |
   | **Abonnement** | Dieses Feld ist bereits mit dem Abonnement ausgefüllt, das zur Ressourcengruppe gehört. |
   | **Ressourcengruppe** | Dieses Feld ist bereits mit dem Namen der aktuellen Ressourcengruppe ausgefüllt, die Sie im vorherigen Tutorial erstellt haben.  |
   | **Name des virtuellen Computers** | Geben Sie einen eindeutigen Namen für den virtuellen Computer ein. |
   | **Region** | Wählen Sie den geografischen Standort des virtuellen Computers aus. |
   | **Verfügbarkeitsoptionen** | Übernehmen Sie den Standardwert. |
   | **Image** | Wählen Sie das VM-Image aus. |
   | **Größe** | Übernehmen Sie die Standardgröße. |
   | **Authentifizierungstyp**  | Wählen Sie **Kennwort** aus. |
   | **Benutzername** | Geben Sie den Benutzernamen für die Anmeldung bei dem virtuellen Computer ein. |
   | **Kennwort** | Geben Sie das Kennwort für die Anmeldung bei dem virtuellen Computer ein. |
   | **Kennwort bestätigen** | Geben Sie das Kennwort für die Anmeldung bei dem virtuellen Computer ein. |
   | **Öffentliche Eingangsports** | Wählen Sie **Keine**. Wenn Sie „Keine“ auswählen, können Sie [JIT-Zugriff](../security-center/security-center-just-in-time.md#jit-configure) verwenden, um den Zugriff auf den virtuellen Computer nur dann zu steuern, wenn Sie darauf zugreifen möchten. Alternativ können Sie auch eine [Azure Bastion](../bastion/tutorial-create-host-portal.md)-Instanz verwenden, wenn Sie über das Internet sicher auf den Jumpboxserver zugreifen möchten, ohne Netzwerkports verfügbar zu machen.  |


1. Wählen Sie nach erfolgreicher Überprüfung die Option **Erstellen** aus, um den VM-Erstellungsprozess zu starten.

## <a name="connect-to-the-local-vcenter-of-your-private-cloud"></a>Herstellen einer Verbindung mit der lokalen vCenter-Instanz Ihrer privaten Cloud

1. Melden Sie sich über die Jumpbox mit einmaligem Anmelden von VMware vCenter beim vSphere-Client an. Verwenden Sie dazu einen Cloudadministrator-Benutzernamen, und vergewissern Sie sich, dass die Benutzeroberfläche angezeigt wird.

1. Wählen Sie im Azure-Portal Ihre private Cloud und anschließend **Verwalten** > **Identität** aus. 

   Die URLs und Benutzeranmeldeinformationen für vCenter und NSX-T Manager der privaten Cloud werden angezeigt.

   :::image type="content" source="media/tutorial-access-private-cloud/ss4-display-identity.png" alt-text="Screenshot: URLs und Anmeldeinformationen für vCenter und NSX-T Manager in der privaten Cloud" lightbox="media/tutorial-access-private-cloud/ss4-display-identity.png":::

1. Navigieren Sie zu dem virtuellen Computer, den Sie im vorherigen Schritt erstellt haben, und stellen Sie eine Verbindung mit dem virtuellen Computer her. 

   Wenn Sie Hilfe beim Herstellen einer Verbindung mit dem virtuellen Computer benötigen, finden Sie ausführliche Informationen unter [Verbinden mit dem virtuellen Computer](../virtual-machines/windows/connect-logon.md#connect-to-the-virtual-machine).

1. Öffnen Sie auf dem virtuellen Windows-Computer einen Browser, und navigieren Sie auf zwei Tabs zu den URLs für vCenter und NSX-T Manager. 

1. Geben Sie auf dem vCenter-Tab die Benutzeranmeldeinformationen `cloudadmin@vmcp.local` aus dem vorherigen Schritt ein.

   :::image type="content" source="media/tutorial-access-private-cloud/ss5-vcenter-login.png" alt-text="Screenshot: Anmeldeseite für VMware vSphere" border="true":::

   :::image type="content" source="media/tutorial-access-private-cloud/ss6-vsphere-client-home.png" alt-text="Screenshot: Zusammenfassung von Cluster-1 im vSphere-Client" border="true":::

1. Melden Sie sich auf dem zweiten Browsertab bei NSX-T Manager an.

   :::image type="content" source="media/tutorial-access-private-cloud/ss10-nsx-manager-home.png" alt-text="Screenshot: Übersicht über NSX-T Manager" border="true":::



## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines virtuellen Windows-Computers zum Herstellen der Verbindung mit vCenter
> * Anmelden bei vCenter über Ihren virtuellen Computer

Im nächsten Tutorial erfahren Sie, wie Sie im Rahmen der Einrichtung der lokalen Verwaltung für Ihre privaten Cloudcluster ein virtuelles Netzwerk erstellen.

> [!div class="nextstepaction"]
> [Erstellen eines virtuellen Netzwerks](tutorial-configure-networking.md)

