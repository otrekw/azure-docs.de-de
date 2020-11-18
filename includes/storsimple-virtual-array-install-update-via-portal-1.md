---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: e64db93347794ccfc85252205c6d92a68e904635
ms.sourcegitcommit: 6a902230296a78da21fbc68c365698709c579093
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2020
ms.locfileid: "93375978"
---
#### <a name="to-install-updates-via-the-azure-portal"></a>So installieren Sie Updates über das Azure-Portal

1. Wechseln Sie zu Ihrem StorSimple-Geräte-Manager, und wählen Sie **Devices** (Geräte) aus. Wählen Sie aus der Liste der mit dem Dienst verbundenen Geräte das Gerät aus, das Sie aktualisieren möchten, und klicken Sie darauf.

    ![Gerät aktualisieren](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate1m.png) 

2. Klicken Sie auf dem Blatt **Settings** (Einstellungen) auf **Device updates** (Geräteupdates).

    ![Aktualisieren des Geräts 2](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate2m.png)  

3. Wenn Softwareupdates verfügbar sind, wird eine Meldung angezeigt. Sie können auch auf **Scan** (Überprüfen) klicken, um nach Updates zu suchen. Notieren Sie sich die Softwareversion, die Sie ausführen. 

    ![Aktualisieren des Geräts 3](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate3m1.png)

    Sie werden jeweils benachrichtigt, wenn die Überprüfung beginnt und erfolgreich abgeschlossen wurde.

    ![Aktualisieren des Geräts 4](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate5m.png)

4. Wenn die Updatesuche abgeschlossen wurde, klicken Sie auf **Download updates** (Updates herunterladen).

    ![Aktualisieren des Geräts 5](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate6m.png)

5. Im Blatt **Neue Updates**, überprüfen Sie die Anmerkungen zu dieser Version. Beachten Sie, dass Sie, nachdem die Updates heruntergeladen wurden, die Installation bestätigen müssen. Klicken Sie auf **OK**.

    ![Aktualisieren des Geräts 6](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate7m.png)

6. Sie werden jeweils benachrichtigt, wenn das Hochladen beginnt und erfolgreich abgeschlossen wurde.

     ![Aktualisieren des Geräts 7](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate8m.png)

5. Klicken Sie im Blatt **Device updates** auf **Install** (Installieren).

     ![Aktualisieren des Geräts 8](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate11m1.png)

6. Auf dem Blatt **New updates** (Neue Updates) werden Sie gewarnt, dass das Update zu einer Unterbrechung führt. Da es sich bei Virtual Array um ein Gerät mit einem Einzelknoten handelt, wird das Gerät nach dem Update neu gestartet. Dadurch werden alle laufenden E/A-Vorgänge unterbrochen. Klicken Sie auf **OK**, um die Updates zu installieren.

    ![Aktualisieren des Geräts 9](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate12m.png)

7. Sie werden benachrichtigt, wenn der Installationsauftrag beginnt.

    ![Aktualisieren des Geräts 10](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate13m.png)

8.  Klicken Sie nach erfolgreichem Beendigen des Installationsauftrags im Blatt **Device updates** (Geräteupdates) auf den Link **View Job** (Auftrag anzeigen), um die Installation zu überwachen. 

    ![Aktualisieren des Geräts 11](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate15m1.png)

    Durch diese Aktion gelangen Sie auf das Blatt **Install Updates** (Updates installieren). Hier finden Sie ausführliche Informationen zum Auftrag.

    ![Aktualisieren des Geräts 12](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate16m1.png)

9. Wenn Sie mit einem virtuellen Array mit Softwareversion Update 0.6 (10.0.10293.0) begonnen haben, führen Sie jetzt Update 1 aus, danach sind Sie fertig. Sie können die verbleibenden Schritte überspringen. Wenn Sie mit einem virtuellen Array mit einer älteren Softwareversion als Update 0.6 (10.0.10293.0) gestartet sind, werden Sie jetzt auf Update 0.6 aktualisiert und sind fertig. Daraufhin wird eine andere Meldung angezeigt, die angibt, dass Updates verfügbar sind. Wiederholen Sie die Schritte 4 bis 8, um Update 1 zu installieren.

    ![Aktualisieren des Geräts 13](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate17.png)

