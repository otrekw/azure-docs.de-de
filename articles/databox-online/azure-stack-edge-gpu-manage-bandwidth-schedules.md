---
title: 'Azure Stack Edge Pro-GPU: Verwalten von Bandbreitenzeitplänen | Microsoft-Dokumentation'
description: In diesem Artikel wird beschrieben, wie Sie Bandbreitenzeitpläne für Ihr Azure Stack Edge Pro-GPU-Gerät über das Azure-Portal verwalten.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 02/22/2021
ms.author: alkohli
ms.openlocfilehash: 7b091ff1ec825ac2292345183eb77bc37f02f6e0
ms.sourcegitcommit: b572ce40f979ebfb75e1039b95cea7fce1a83452
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "102637141"
---
# <a name="use-the-azure-portal-to-manage-bandwidth-schedules-on-your-azure-stack-edge-pro-gpu"></a>Verwalten von Bandbreitenzeitplänen für Ihr Azure Stack Edge Pro-GPU-Gerät über das Azure-Portal 

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

In diesem Artikel wird beschrieben, wie Sie Bandbreitenzeitpläne auf Ihrem Azure Stack Edge Pro-Gerät verwalten. Mithilfe von Bandbreitenzeitplänen können Sie die Nutzung der Netzwerkbandbreite durch mehrere tageszeitbasierte Zeitpläne konfigurieren. Diese Zeitpläne können auf die Upload- und Downloadvorgänge zwischen Ihrem Gerät und der Cloud angewendet werden.

Sie können die Bandbreitenzeitpläne für Ihr Azure Stack Edge Pro-Gerät über das Azure-Portal hinzufügen, ändern oder löschen.

In diesem Artikel werden folgende Vorgehensweisen behandelt: 

> [!div class="checklist"]
> * Hinzufügen eines Zeitplans
> * Ändern eines Zeitplans
> * Löschen eines Zeitplans


## <a name="add-a-schedule"></a>Hinzufügen eines Zeitplans

Gehen Sie im Azure-Portal wie folgt vor, um einen Zeitplan hinzuzufügen:

1. Wechseln Sie im Azure-Portal für Ihre Azure Stack Edge-Ressource zu **Bandbreite**.
2. Wählen Sie im rechten Bereich die Option **+ Zeitplan hinzufügen**.

    ![Auswählen der Bandbreite](media/azure-stack-edge-gpu-manage-bandwidth-schedules/add-schedule-1.png)

3. Gehen Sie unter **Zeitplan hinzufügen** wie folgt vor:

   1. Geben Sie den **Starttag**, den **Endtag**, die **Startzeit** und die **Endzeit** des Zeitplans an.
   2. Aktivieren Sie die Option **Ganztägig**, wenn dieser Zeitplan den ganzen Tag lang ausgeführt werden soll.
   3. **Bandbreitenrate** ist die Bandbreite in Megabit pro Sekunde (MBit/s), die Ihr Gerät bei Cloudvorgängen (Uploads und Downloads) verwendet. Geben Sie eine Zahl zwischen 64 und 2.147.483.647 in dieses Feld ein.
   4. Aktivieren Sie die Option **Unbegrenzt**, wenn Sie Uploads und Downloads nicht drosseln möchten.
   5. Wählen Sie **Hinzufügen**.

      ![Hinzufügen eines Zeitplans](media/azure-stack-edge-gpu-manage-bandwidth-schedules/add-schedule-2.png)

3. Ein Zeitplan mit den angegebenen Parametern wird erstellt. Dieser Zeitplan wird dann im Portal in der Liste mit den Bandbreitenzeitplänen angezeigt.

    ![Aktualisierte Liste der Bandbreitenzeitpläne](media/azure-stack-edge-gpu-manage-bandwidth-schedules/add-schedule-3.png)

## <a name="edit-schedule"></a>Bearbeiten des Zeitplans

Führen Sie die folgenden Schritte aus, um einen Bandbreitenzeitplan zu bearbeiten:

1. Wechseln Sie im Azure-Portal zu Ihrer Azure Stack Edge-Ressource und dann zu **Bandbreite**.
2. Wählen Sie in der Liste mit den Bandbreitenzeitplänen einen Zeitplan aus, den Sie ändern möchten.

   ![Auswählen eines Bandbreitenzeitplans](media/azure-stack-edge-gpu-manage-bandwidth-schedules/modify-schedule-1.png)

3. Nehmen Sie die gewünschten Änderungen vor, und speichern Sie sie.

    ![Ändern eines Benutzers](media/azure-stack-edge-gpu-manage-bandwidth-schedules/modify-schedule-2.png)

4. Nach der Änderung des Zeitplans wird die Liste mit den Zeitplänen aktualisiert, um den geänderten Zeitplan widerzuspiegeln.

    ![Ändern von Benutzer 2](media/azure-stack-edge-gpu-manage-bandwidth-schedules/modify-schedule-3.png)


## <a name="delete-a-schedule"></a>Löschen eines Zeitplans

Führen Sie die folgenden Schritte aus, um einen mit Ihrem Azure Stack Edge Pro-Gerät verknüpften Bandbreitenzeitplan zu löschen.

1. Wechseln Sie im Azure-Portal zu Ihrer Azure Stack Edge-Ressource und dann zu **Bandbreite**.  

2. Wählen Sie in der Liste mit den Bandbreitenzeitplänen einen Zeitplan aus, den Sie löschen möchten. Wählen Sie im Dialogfeld **Zeitplan bearbeiten** die Option **Löschen** aus. Wählen Sie **Ja** aus, wenn Sie zur Bestätigung aufgefordert werden.

   ![Löschen eines Benutzers](media/azure-stack-edge-gpu-manage-bandwidth-schedules/delete-schedule-2.png)

3. Nach der Löschung des Zeitplans wird die Liste mit den Zeitplänen aktualisiert.


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Verwaltung von Freigaben](azure-stack-edge-gpu-manage-shares.md).
