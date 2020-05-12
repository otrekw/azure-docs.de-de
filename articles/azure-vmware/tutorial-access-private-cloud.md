---
title: 'Tutorial: Zugreifen auf Ihre private Cloud'
description: Hier erfahren Sie, wie Sie auf eine private AVS-Cloud (Azure VMware Solution, Azure-VMware-Lösung) zugreifen.
ms.topic: tutorial
ms.date: 05/04/2020
ms.openlocfilehash: fcee332818c89d9c8b00795dca9e74f68260eefd
ms.sourcegitcommit: d9cd51c3a7ac46f256db575c1dfe1303b6460d04
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2020
ms.locfileid: "82739585"
---
# <a name="tutorial-learn-how-to-access-an-azure-vmware-solution-avs-private-cloud"></a>Tutorial: Hier erfahren Sie, wie Sie auf eine private AVS-Cloud (Azure VMware Solution, Azure-VMware-Lösung) zugreifen.

Während der Vorschauphase können Sie Ihre private Cloud nicht mit Ihrer lokalen vCenter-Instanz verwalten. Daher sind zusätzliche Einrichtungsschritte erforderlich, und es muss über eine Jumpbox eine Verbindung mit einer lokalen vCenter-Instanz hergestellt werden. 

In diesem Tutorial erstellen Sie einen virtuellen Windows-Computer für eine Jumpbox in der Ressourcengruppe, die Sie im vorherigen Tutorial ([Tutorial: Konfigurieren des Netzwerks für Ihre private VMware-Cloud in Azure](tutorial-configure-networking.md)) erstellt haben, und melden sich bei vCenter an. Dieser virtuelle Computer befindet sich im von Ihnen erstellten virtuellen Netzwerk und ermöglicht den Zugriff auf vCenter und NSX Manager. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines virtuellen Windows-Computers zum Herstellen der Verbindung mit vCenter
> * Anmelden bei vCenter über Ihren virtuellen Computer

## <a name="create-a-new-windows-virtual-machine"></a>Erstellen eines neuen virtuellen Windows-Computers

Wählen Sie in der Ressourcengruppe **+ Hinzufügen** aus, suchen Sie nach **Microsoft Windows 10**, wählen Sie die Option aus, und klicken Sie anschließend auf **Erstellen**.

:::image type="content" source="./media/tutorial-access-private-cloud/ss8-azure-w10vm-create.png" alt-text="Hinzufügen eines neuen virtuellen Windows 10-Computers für eine Jumpbox" border="true":::

Geben Sie die erforderlichen Informationen in die Felder ein, und wählen Sie anschließend **Überprüfen + erstellen** aus. Weitere Informationen zu den Feldern finden Sie in der folgenden Tabelle:

| Feld | Wert |
| --- | --- |
| **Abonnement** | Dieser Wert ist bereits mit dem Abonnement aufgefüllt, zu dem die Ressourcengruppe gehört. |
| **Ressourcengruppe** | Dieser Wert ist bereits für die aktuelle Ressourcengruppe aufgefüllt. Hierbei sollte es sich um die Ressourcengruppe handeln, die Sie in einem vorherigen Tutorial erstellt haben. |
| **Name des virtuellen Computers** | Geben Sie einen eindeutigen Namen für den virtuellen Computer ein. |
| **Region** | Wählen Sie den geografischen Standort des virtuellen Computers aus. |
| **Verfügbarkeitsoptionen** | Übernehmen Sie den Standardwert. |
| **Image** | Wählen Sie das VM-Image aus. |
| **Größe** | Übernehmen Sie die Standardgröße. |
| **Authentifizierungstyp**  | Wählen Sie **Kennwort** aus. |
| **Benutzername** | Geben Sie den Benutzernamen für die Anmeldung bei dem virtuellen Computer ein. |
| **Kennwort** | Geben Sie das Kennwort für die Anmeldung bei dem virtuellen Computer ein. |
| **Kennwort bestätigen** | Geben Sie das Kennwort für die Anmeldung bei dem virtuellen Computer ein. |
| **Öffentliche Eingangsports** | Wählen Sie **Keine**. Wenn Sie „Keine“ auswählen, können Sie [JIT-Zugriff](../security-center/security-center-just-in-time.md#configure-jit-access-from-an-azure-vms-page-) verwenden, um den Zugriff auf den virtuellen Computer nur dann zu steuern, wenn Sie darauf zugreifen möchten.  |

Klicken Sie nach Angabe der erforderlichen Informationen auf **Überprüfen + erstellen**. Wählen Sie nach erfolgreicher Überprüfung die Option **Erstellen** aus, um den VM-Erstellungsprozess zu starten.

:::image type="content" source="./media/tutorial-access-private-cloud/ss11-review-create-wjb01.png" alt-text="Erstellen eines neuen virtuellen Windows 10-Computers für eine Jumpbox" border="true":::

## <a name="connect-to-the-local-vcenter-of-your-private-cloud"></a>Herstellen einer Verbindung mit der lokalen vCenter-Instanz Ihrer privaten Cloud

Melden Sie sich von der Jumpbox aus mit VMware vCenter-SSO beim vSphere-Client an. Melden Sie sich mit einem Cloudadministrator-Benutzernamen beim vSphere-Client an. Sollte eine Warnung mit einem Hinweis auf ein potenzielles Sicherheitsrisiko angezeigt werden, akzeptieren Sie das Sicherheitsrisiko. Melden Sie sich bei VMware vCenter mit Anmeldeinformationen für einmaliges Anmelden an, und vergewissern Sie sich, dass die Benutzeroberfläche erfolgreich angezeigt wird.

Wählen Sie im Azure-Portal Ihre private Cloud aus, und navigieren Sie in der Ansicht **Übersicht** zu **Identität > Standard**. Die URLs und Anmeldeinformationen für vCenter und NSX-T Manager der privaten Cloud werden angezeigt.

:::image type="content" source="./media/tutorial-access-private-cloud/ss4-display-identity.png" alt-text="Anzeigen der URLs und Anmeldeinformationen für vCenter und NSX Manager der privaten Cloud" border="true":::

Navigieren Sie zu dem virtuellen Computer, den Sie im vorherigen Schritt erstellt haben, und stellen Sie eine Verbindung mit dem virtuellen Computer her. Eine ausführliche Anleitung zum Herstellen der Verbindung mit dem virtuellen Computer finden Sie unter [Verbinden mit dem virtuellen Computer](../virtual-machines/windows/connect-logon.md#connect-to-the-virtual-machine).

Öffnen Sie auf dem virtuellen Windows-Computer einen Browser, und navigieren Sie auf zwei Tabs zu den URLs für vCenter und NSX-T Manager. Geben Sie auf dem vCenter-Tab die Benutzeranmeldeinformationen `cloudadmin@vmcp.local` aus dem vorherigen Schritt ein.

:::image type="content" source="./media/tutorial-access-private-cloud/ss5-vcenter-login.png" alt-text="Anmelden bei der privaten Cloud: vCenter" border="true":::

:::image type="content" source="./media/tutorial-access-private-cloud/ss6-vsphere-client-home.png" alt-text="vCenter-Portal" border="true":::

Melden Sie sich auf dem zweiten Browsertab bei NSX-T Manager an.

:::image type="content" source="./media/tutorial-access-private-cloud/ss10-nsx-manager-home.png" alt-text="Lokale private Cloud: Startseite von NSX Manager" border="true":::

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines virtuellen Windows-Computers zum Herstellen der Verbindung mit vCenter
> * Anmelden bei vCenter über Ihren virtuellen Computer

Im nächsten Tutorial erfahren Sie, wie Sie Ihre private AVS-Cloud skalieren.

> [!div class="nextstepaction"]
> [Tutorial: Skalieren einer privaten AVS-Cloud (Vorschauversion)](tutorial-scale-private-cloud.md)