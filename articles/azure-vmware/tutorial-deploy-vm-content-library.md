---
title: 'Tutorial: Erstellen einer Inhaltsbibliothek zum Bereitstellen von VMs in Azure VMware Solution (AVS)'
description: In diesem Tutorial zu Azure VMware Solution (AVS) erstellen Sie eine Inhaltsbibliothek zum Bereitstellen einer VM in einer privaten AVS-Cloud.
ms.topic: tutorial
ms.date: 07/10/2020
ms.openlocfilehash: bdea4304baa92fadc4d6c3d0060f99cf37af5f77
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259417"
---
# <a name="tutorial-create-a-content-library-to-deploy-vms-in-azure-vmware-solution-avs"></a>Tutorial: Erstellen einer Inhaltsbibliothek zum Bereitstellen von VMs in Azure VMware Solution (AVS)

In einer Inhaltsbibliothek werden Inhalte in Form von Bibliothekselementen gespeichert und verwaltet. Ein einzelnes Bibliothekselement besteht aus einer oder mehreren Dateien, die Sie zum Bereitstellen virtueller Computer (VMs) verwenden. 
 
In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen einer Inhaltsbibliothek
> * Hochladen eines ISO-Images in die Inhaltsbibliothek
> * Bereitstellen einer VM mithilfe eines ISO-Images in der Inhaltsbibliothek

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial sind ein logisches NSX-T-Switchsegment und ein verwalteter DHCP-Dienst erforderlich.  Ausführliche Informationen dazu finden Sie unter [Verwalten von DHCP in der Azure-VMware-Lösung (Vorschauversion)](manage-dhcp.md).

## <a name="create-a-content-library"></a>Erstellen einer Inhaltsbibliothek

1. Wählen Sie auf dem lokalen vSphere-Client **Menü > Inhaltsbibliotheken** aus.

   ![Auswählen von „Menü“ -> „Inhaltsbibliotheken“](./media/content-library/vsphere-menu-content-libraries.png)

1. Erstellen Sie über die Schaltfläche **Hinzufügen** eine neue Inhaltsbibliothek.

   ![Erstellen Sie über die Schaltfläche „Hinzufügen“ eine neue Inhaltsbibliothek.](./media/content-library/create-new-content-library.png)

1. Geben Sie einen Namen an, und bestätigen Sie die IP-Adresse des vCenter-Servers. Wählen Sie dann **Weiter** aus.

   ![Geben Sie einen Namen und Hinweise Ihrer Wahl an, und wählen Sie „Weiter“ aus.](./media/content-library/new-content-library-step1.png)

1. Wählen Sie **Lokale Inhaltsbibliothek** und dann **Weiter** aus.

   ![In diesem Beispiel wird eine lokale Inhaltsbibliothek erstellt. Wählen Sie „Weiter“ aus.](./media/content-library/new-content-library-step2.png)

1. Wählen Sie den Datenspeicher zum Speichern der Inhaltsbibliothek und dann **Weiter** aus.

   ![Wählen Sie den gewünschten Datenspeicher zum Hosten der Inhaltsbibliothek und dann „Weiter“ aus.](./media/content-library/new-content-library-step3.png)

1. Überprüfen Sie die Einstellungen der Inhaltsbibliothek, und wählen Sie dann **Fertig stellen** aus.

   ![Überprüfen Sie die Einstellungen, und wählen Sie „Fertig stellen“ aus.](./media/content-library/new-content-library-step4.png)

## <a name="upload-an-iso-image-to-the-content-library"></a>Hochladen eines ISO-Images in die Inhaltsbibliothek

Nachdem Sie die Inhaltsbibliothek erstellt haben, können Sie ein ISO-Image hinzufügen, um eine VM in einem privaten Cloudcluster bereitzustellen. 

1. Wählen Sie auf dem vSphere-Client **Menü > Inhaltsbibliotheken** aus.

1. Klicken Sie mit der rechten Maustaste auf die Inhaltsbibliothek, die Sie für das neue ISO-Image verwenden möchten, und wählen Sie **Element importieren** aus.

1. Importieren Sie ein Bibliothekselement für die Quelle mithilfe einer der folgenden Aktionen, und wählen Sie dann **Importieren** aus:
   1. Wählen Sie „URL“ aus, und geben Sie eine URL zum Herunterladen eines ISO-Images an.

   1. Wählen Sie die **Lokale Datei** aus, die vom lokalen System hochgeladen werden soll.

   > [!TIP]
   > Optional können Sie einen benutzerdefinierten Elementnamen und Hinweise für das Ziel definieren.

1. Öffnen Sie die Bibliothek, und wählen Sie die Registerkarte **Andere Typen** aus, um zu überprüfen, ob Ihr ISO-Image erfolgreich hochgeladen wurde.


## <a name="deploy-a-vm-to-a-private-cloud-cluster"></a>Bereitstellen einer VM in einem privaten Cloudcluster

1. Wählen Sie auf dem vSphere-Client **Menü > Hosts und Cluster** aus.

1. Erweitern Sie die Struktur im linken Bereich, und wählen Sie einen Cluster aus.

1. Wählen Sie **Aktionen > Neuer virtueller Computer** aus.

1. Durchlaufen Sie den Assistenten, und ändern Sie die gewünschten Einstellungen.

1. Wählen Sie **Neues CD-/DVD-Laufwerk > Clientgerät > ISO-Datei der Inhaltsbibliothek** aus.

1. Wählen Sie das im vorherigen Abschnitt hochgeladene ISO-Image und dann **OK** aus.

1. Aktivieren Sie das Kontrollkästchen **Verbinden**, damit das ISO-Image beim Einschalten bereitgestellt wird.

1. Wählen Sie **Neues Netzwerk > Dropdownliste auswählen > Durchsuchen** aus.

1. Wählen Sie den **logischen Switch (Segment)** und dann **OK** aus.

1. Ändern Sie beliebige andere Hardwareeinstellungen, und wählen Sie **Weiter** aus.

1. Überprüfen Sie die Einstellungen, und wählen Sie **Fertig stellen** aus.


## <a name="next-steps"></a>Nächste Schritte

Wenn Sie beabsichtigen, Hybrid Cloud Extension (HCX) zum Migrieren von VM-Workloads zu Ihrer privaten Cloud zu verwenden, führen Sie das Verfahren unter [Installieren von HCX für eine Azure-VMware-Lösung](hybrid-cloud-extension-installation.md) aus.

<!-- LINKS - external-->

<!-- LINKS - internal -->