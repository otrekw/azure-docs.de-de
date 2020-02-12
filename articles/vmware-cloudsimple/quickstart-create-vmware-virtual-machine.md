---
title: 'Schnellstart für Azure VMware Solutions (AVS): Erstellen von VMware-VMs in Azure'
description: Erfahren Sie, wie Sie VMware-VMs mit Azure VMware Solutions (AVS) über das Azure-Portal erstellen und konfigurieren.
titleSuffix: Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: dikamath
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 575d59d60ebfcfdbe4d234d608e8d988006773c2
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77019552"
---
# <a name="quickstart---create-vmware-vms-on-azure"></a>Schnellstart: Erstellen von VMware-VMs in Azure

Um einen virtuellen Computer im Azure-Portal zu erstellen, verwenden Sie VM-Vorlagen, die Ihr AVS-Administrator für Ihr Abonnement aktiviert hat. Die VM-Vorlagen finden Sie in der VMware-Infrastruktur.

## <a name="avs-vm-creation-on-azure-requires-a-vm-template"></a>Für die AVS-VM-Erstellung in Azure ist eine VM-Vorlage erforderlich.

Erstellen Sie einen virtuellen Computer über die vCenter-Benutzeroberfläche in Ihrer privaten AVS-Cloud. Um eine Vorlage zu erstellen, folgen Sie den Anweisungen unter [Klonen eines virtuellen Geräts in eine Vorlage im vSphere-Webclient](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-FE6DE4DF-FAD0-4BB0-A1FD-AFE9A40F4BFE.html). Speichern Sie die VM-Vorlage in vCenter in der privaten AVS-Cloud.

## <a name="create-a-virtual-machine-in-the-azure-portal"></a>Erstellen eines virtuellen Computers im Azure-Portal

1. Wählen Sie **Alle Dienste** aus.

2. Suchen Sie nach **AVS Virtual Machines** (Virtuelle AVS-Computer).

3. Klicken Sie auf **Hinzufügen**.

    ![Erstellen eines virtuellen AVS-Computers](media/create-cloudsimple-virtual-machine.png)

4. Geben Sie grundlegende Informationen ein, und klicken Sie auf **Next:Size**.

    ![Erstellen eines virtuellen AVS-Computers – Grundlagen](media/create-cloudsimple-virtual-machine-basic-info.png)

    | Feld | Beschreibung |
    | ------------ | ------------- |
    | Subscription | Das mit Ihrer privaten AVS-Cloud verknüpfte Azure-Abonnement |
    | Ressourcengruppe | Die Ressourcengruppe, der die VM zugewiesen wird. Wählen Sie eine vorhandene Gruppe auswählen oder eine neue erstellen. |
    | Name | Der Name zur Identifizierung der VM.  |
    | Location | Azure-Region, in der die VM gehostet wird.  |
    | Private AVS-Cloud | Die private AVS-Cloud, in der Sie den virtuellen Computer erstellen möchten. |
    | Ressourcenpool | Zugeordneter Ressourcenpool für die VM. Wählen Sie aus den verfügbaren Ressourcenpools aus. |
    | vSphere-Vorlage | vSphere-Vorlage für die VM.  |
    | Benutzername | Benutzername des VM-Administrators (für Windows-Vorlagen).|
    | Kennwort |  Kennwort des VM-Administrators (für Windows-Vorlagen). |
    | Kennwort bestätigen | Bestätigen Sie das Kennwort. |

5. Wählen Sie die Anzahl der Kerne und die Speicherkapazität für die VM und klicken Sie auf **Weiter:Konfigurationen**. Aktivieren Sie das Kontrollkästchen, wenn Sie die vollständige CPU-Virtualisierung für das Gastbetriebssystem bereitstellen möchten. Anwendungen, die eine Hardwarevirtualisierung erfordern, können auf virtuellen Computern ohne binäre Übersetzung oder Paravirtualisierung ausgeführt werden. Weitere Informationen finden Sie im VMware-Artikel <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html" target="_blank">Verfügbarmachen der hardwareunterstützten VMware-Virtualisierung</a>.

    ![Erstellen eines virtuellen AVS-Computers – Größe](media/create-cloudsimple-virtual-machine-size.png)

6. Konfigurieren Sie Netzwerkschnittstellen und Datenträger wie in den folgenden Tabellen beschrieben und klicken Sie auf **Überprüfen + Erstellen**.

    ![Erstellen eines virtuellen AVS-Computers – Konfigurationen](media/create-cloudsimple-virtual-machine-configurations.png)

    Klicken Sie bei Netzwerkschnittstellen auf **Netzwerkschnittstelle hinzufügen**, und konfigurieren Sie die folgenden Einstellungen.

    | Control | Beschreibung |
    | ------------ | ------------- |
    | Name | Geben Sie einen Namen zur Identifizierung der Schnittstelle ein.  |
    | Netzwerk | Wählen Sie in der Liste der konfigurierten verteilten Portgruppen in Ihrer privaten AVS-Cloud vSphere aus. |
    | Adapter | Wählen Sie einen vSphere-Adapter aus der Liste der verfügbaren Typen, die für die VM konfiguriert sind. Weitere Informationen finden Sie im VMware-Knowledge Base-Artikel <a href="https://kb.vmware.com/s/article/1001805" target="_blank">Auswählen eines Netzwerkadapters für Ihren virtuellen Computer</a>. |
    | Einschalten beim Starten | Wählen Sie, ob die NIC-Hardware beim Booten der VM aktiviert werden soll. Die Standardeinstellung ist **Aktiviert**. |

    Klicken Sie bei Datenträgern auf **Datenträger hinzufügen** und konfigurieren Sie die folgenden Einstellungen.

    | Element | Beschreibung |
    | ------------ | ------------- |
    | Name | Geben Sie einen Namen zur Identifizierung des Datenträgers ein. |
    | Size | Wählen Sie eine der verfügbaren Größen aus. |
    | SCSI-Controller | Wählen Sie einen SCSI-Controller für den Datenträger aus. |
    | Mode | Bestimmt, wie der Datenträger in Momentaufnahmen beteiligt ist. Wählen Sie eine der Optionen aus: <br> - Unabhängig dauerhaft: Alle Daten, die auf den Datenträger geschrieben werden, werden dauerhaft geschrieben.<br> - Unabhängig nicht dauerhaft: Auf dem Datenträger geschriebene Änderungen werden verworfen, wenn Sie den Computer ausschalten oder zurücksetzen. Der unabhängige, nicht dauerhafte Modus ermöglicht es Ihnen, die VM immer im gleichen Zustand neu zu starten. Weitere Informationen finden Sie in der <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html" target="_blank">VMware-Dokumentation</a>.

7. Wenn die Überprüfung abgeschlossen ist, überprüfen Sie die Einstellungen, und klicken Sie dann auf **Create** (Erstellen). Um Änderungen vorzunehmen, klicken Sie auf die Registerkarten oben.

    ![Erstellen eines virtuellen AVS-Computers – Überprüfen](media/create-cloudsimple-virtual-machine-review.png)

## <a name="next-steps"></a>Nächste Schritte

* [Anzeigen der Liste von virtuellen AVS-Computern](azure-create-vm.md#view-list-of-avs-virtual-machines)
* [Verwalten eines virtuellen AVS-Computers in Azure](azure-manage-vm.md)
