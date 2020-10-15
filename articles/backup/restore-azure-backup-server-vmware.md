---
title: Wiederherstellen von VMware-VMs mit Azure Backup Server
description: Verwenden Sie Azure Backup Server (MABS), um VMware-VMs wiederherzustellen, die auf einem VMware vCenter-/ESXi-Server ausgeführt werden.
ms.topic: conceptual
ms.date: 08/18/2019
ms.openlocfilehash: b3f61aa828db39aeb11b1ce46a850d9a5b868653
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88263519"
---
# <a name="restore-vmware-virtual-machines"></a>Wiederherstellen von virtuellen VMware-Computern

In diesem Artikel wird beschrieben, wie Sie Microsoft Azure Backup Server (MABS) zum Wiederherstellen von VMware-VM-Wiederherstellungspunkten verwenden. Eine Übersicht über die Verwendung von MABS zum Wiederherstellen von Daten finden Sie unter [Wiederherstellen von Daten von Azure Backup Server](./backup-azure-alternate-dpm-server.md). In der MABS-Administratorkonsole haben Sie zwei Möglichkeiten, wiederherstellbare Daten zu ermitteln: Suchen und Durchsuchen. Beim Wiederherstellen von Daten kann es auch sein, dass Sie die Daten oder eine VM nicht an demselben Speicherort wiederherstellen möchten. Aus diesem Grund unterstützt MABS drei Wiederherstellungsoptionen für VMware-VM-Sicherungen:

* **Wiederherstellung am ursprünglichen Speicherort**: Verwenden Sie diese Wiederherstellungsoption, um eine geschützte VM an ihrem ursprünglichen Speicherort wiederherzustellen. Sie können einen virtuellen Computer nur dann am ursprünglichen Speicherort wiederherstellen, wenn seit der Durchführung des Sicherungsvorgangs keine Datenträger hinzugefügt oder gelöscht wurden. Wenn Datenträger hinzugefügt oder gelöscht wurden, müssen Sie einen anderen Ort für die Wiederherstellung verwenden.

* **Wiederherstellung an einem alternativen Speicherort**: Stellen Sie die VM an einem anderen Ort wieder her, falls die ursprüngliche VM fehlt oder Sie die ursprüngliche VM nicht beeinträchtigen möchten. Zum Wiederherstellen einer VM an einem anderen Speicherort müssen Sie den Speicherort eines ESXi-Hosts, Ressourcenpools, Ordners und des Speicherdatenspeichers und -pfads angeben. Zur Unterscheidung der wiederhergestellten VM von der ursprünglichen VM fügt MABS den Zusatz „-Recovered“ an den Namen der VM an.

* **Wiederherstellung einzelner Dateien an einem Speicherort**: Wenn es sich bei der geschützten VM um eine Windows Server-VM handelt, können einzelne Dateien/Ordner der VM mit dieser Option von MABS wiederhergestellt werden. Informationen zur Wiederherstellung einzelner Dateien finden Sie in den Schritten weiter unten in diesem Artikel.

## <a name="restore-a-recovery-point"></a>Wiederherstellen eines Wiederherstellungspunkts

1. Wählen Sie in der MABS-Administratorkonsole die Ansicht **Wiederherstellung** aus.

2. Suchen oder filtern Sie im Bereich „Durchsuchen“, um die wiederherzustellende VM zu ermitteln. Nachdem Sie eine VM oder einen Ordner ausgewählt haben, werden im entsprechenden Bereich die verfügbaren Wiederherstellungspunkte angezeigt.

    ![Verfügbare Wiederherstellungspunkte](./media/restore-azure-backup-server-vmware/recovery-points.png)

3. Verwenden Sie im Feld **Wiederherstellungspunkte für:** den Kalender und die Dropdownmenüs, um ein Datum auszuwählen, an dem ein Wiederherstellungspunkt erstellt wurde. Für fett formatierte Kalenderdaten sind Wiederherstellungspunkte verfügbar.

4. Wählen Sie im Menüband des Tools **Wiederherstellen** aus, um den **Wiederherstellungs-Assistenten** zu öffnen.

    ![Wiederherstellungs-Assistent, Wiederherstellungsauswahl überprüfen](./media/restore-azure-backup-server-vmware/recovery-wizard.png)

5. Wählen Sie **Weiter** aus, um zum Bildschirm **Wiederherstellungsoptionen angeben**  zu wechseln.

6. Wählen Sie auf dem Bildschirm **Wiederherstellungsoptionen angeben** die Option **Ändern** aus, wenn Sie die Drosselung der Netzwerkbandbreite aktivieren möchten. Wenn die Netzwerk-Bandbreiteneinschränkung deaktiviert bleiben soll, wählen Sie **Weiter** aus. Für VMware-VMs sind auf diesem Bildschirm des Assistenten keine weiteren Optionen verfügbar. Wenn Sie sich für die Änderung einer Drosselung der Netzwerkbandbreite entscheiden, müssen Sie dies aktivieren, indem Sie im Dialogfeld „Drosselung“ die Option **Netzwerk-Bandbreiteneinschränkung aktivieren** auswählen. Konfigurieren Sie nach der Aktivierung die Optionen **Einstellungen** und **Arbeitszeitplan**.

7. Wählen Sie auf dem Bildschirm **Wiederherstellungstyp auswählen** aus, ob die Wiederherstellung auf der ursprünglichen Instanz oder an einem anderen Speicherort durchgeführt werden soll. Wählen Sie **Weiter**aus.

     * Bei Auswahl von **In ursprünglicher Instanz wiederherstellen**  müssen Sie im Assistenten keine weiteren Angaben machen. Es werden die Daten für die ursprüngliche Instanz verwendet.

     * Wenn Sie **Als virtuellen Computer auf einem beliebigen Host wiederherstellen** auswählen, müssen Sie auf dem Bildschirm **Ziel angeben** die Informationen für **ESXi-Host, Ressourcenpool, Ordner** und **Pfad** angeben.

      ![Auswählen des Wiederherstellungstyps](./media/restore-azure-backup-server-vmware/recovery-type.png)

8. Überprüfen Sie Ihre Einstellungen auf dem Bildschirm **Zusammenfassung**, und wählen Sie **Wiederherstellen** aus, um den Wiederherstellungsprozess zu starten. Auf dem Bildschirm **Wiederherstellungsstatus** wird der Status des Wiederherstellungsvorgangs angezeigt.

## <a name="restore-an-individual-file-from-a-vm"></a>Wiederherstellen einer einzelnen Datei von einem virtuellen Computer

Sie können einzelne Dateien eines geschützten VM-Wiederherstellungspunkts wiederherstellen. Dieses Feature ist nur für Windows Server-VMs verfügbar. Die Wiederherstellung einzelner Dateien ähnelt der Wiederherstellung einer vollständigen VM. Allerdings müssen Sie zunächst zur VMDK navigieren und die gewünschten Dateien suchen, bevor Sie den Wiederherstellungsprozess starten. Gehen Sie zum Wiederherstellen einer einzelnen Datei oder zum Auswählen von Dateien von einer Windows Server-VM wie folgt vor:

>[!NOTE]
>Das Wiederherstellen einer einzelnen Datei von einem virtuellen Computer ist nur für Wiederherstellungspunkte virtueller Windows-Computer und -Datenträger verfügbar.

1. Wählen Sie in der MABS-Administratorkonsole die Ansicht **Wiederherstellung** aus.

2. Suchen oder filtern Sie im Bereich **Durchsuchen**, um die wiederherzustellende VM zu ermitteln. Nachdem Sie eine VM oder einen Ordner ausgewählt haben, werden im Bereich **Wiederherstellungspunkte für** die verfügbaren Wiederherstellungspunkte angezeigt.

    ![Bereich „Wiederherstellungspunkte für“](./media/restore-azure-backup-server-vmware/vmware-rp-disk.png)

3. Verwenden Sie im Bereich **Wiederherstellungspunkte für** den Kalender, um das Datum auszuwählen, das die gewünschten Wiederherstellungspunkte enthält. Je nach Konfiguration der Sicherungsrichtlinie können Daten ggf. über mehr als einen Wiederherstellungspunkt verfügen. Nachdem Sie den Tag ausgewählt haben, an dem der Wiederherstellungspunkt erstellt wurde, sollten Sie sicherstellen, dass Sie die richtige **Wiederherstellungszeit** angegeben haben. Falls das ausgewählte Datum über mehrere Wiederherstellungspunkte verfügt, sollten Sie Ihren Wiederherstellungspunkt über das Dropdownmenü „Wiederherstellungszeit“ auswählen. Nach Auswahl des Wiederherstellungspunkts wird die Liste mit den wiederherstellbaren Elementen im Bereich **Pfad:** angezeigt.

4. Um die Dateien zu finden, die wiederhergestellt werden sollen, doppelklicken Sie im Bereich **Pfad** auf das Element in der Spalte **Wiederherstellbares Element**, um es zu öffnen. Wählen Sie die Dateien oder Ordner aus, die Sie wiederherstellen möchten. Um mehrere Elemente auszuwählen, drücken Sie die **STRG**-Taste, während Sie die einzelnen Elemente auswählen. Durchsuchen Sie im Bereich **Pfad** die Liste der Dateien oder Ordnern, die in der Spalte **Wiederherstellbares Element** angezeigt werden. Bei der Option **Liste unten durchsuchen** werden die Unterordner nicht in die Suche einbezogen. Doppelklicken Sie auf den Ordner, um die Unterordner zu durchsuchen. Verwenden Sie die Schaltfläche **Nach oben**, um von einem untergeordneten Ordner in den übergeordneten Ordner zu wechseln. Sie können mehrere Elemente (Dateien und Ordner) auswählen, aber diese müssen in demselben übergeordneten Ordner enthalten sein. Es ist nicht möglich, in einem einzigen Wiederherstellungsauftrag Elemente aus mehreren Ordnern wiederherzustellen.

    ![Wiederherstellungsauswahl prüfen](./media/restore-azure-backup-server-vmware/vmware-rp-disk-ilr-2.png)

5. Wählen Sie nach dem Auswählen der wiederherzustellenden Elemente im Menüband der Administratorkonsole **Wiederherstellen** aus, um den **Wiederherstellungs-Assistenten** zu öffnen. Im Wiederherstellungs-Assistenten werden auf dem Bildschirm **Wiederherstellungsauswahl überprüfen** die zur Wiederherstellung ausgewählten Elemente angezeigt.

6. Wählen Sie auf dem Bildschirm **Wiederherstellungsoptionen angeben** die Option **Ändern** aus, wenn Sie die Drosselung der Netzwerkbandbreite aktivieren möchten. Wenn die Netzwerk-Bandbreiteneinschränkung deaktiviert bleiben soll, wählen Sie **Weiter** aus. Keine anderen Optionen auf dieser Seite des Assistenten sind für VMware-VMs verfügbar. Wenn Sie die Netzwerk-Bandbreiteneinschränkung ändern möchten, klicken Sie im Dialogfeld „Drosselung“ auf **Netzwerk-Bandbreiteneinschränkung aktivieren**, um die Option zu aktivieren. Konfigurieren Sie nach der Aktivierung die Optionen **Einstellungen** und **Arbeitszeitplan**.
7. Wählen Sie auf dem Bildschirm **Wiederherstellungstyp auswählen** die Option **Weiter** aus. Sie können die Dateien oder Ordner nur in einem Netzwerkordner wiederherstellen.
8. Wählen Sie auf dem Bildschirm **Ziel angeben** die Option **Durchsuchen** aus, um eine Netzwerkadresse für Ihre Dateien bzw. Ordner auszuwählen. MABS erstellt einen Ordner, in den alle wiederhergestellten Elemente kopiert werden. Der Name des Ordners enthält das Präfix „MABS_day-month-year“. Wenn Sie einen Speicherort für die wiederhergestellten Dateien oder den Ordner auswählen, werden die Details für diesen Speicherort angegeben (Ziel, Zielpfad und verfügbarer Speicherplatz).

    ![Angeben des Speicherorts zum Wiederherstellen von Dateien](./media/restore-azure-backup-server-vmware/specify-destination.png)

9. Wählen Sie auf dem Bildschirm **Wiederherstellungsoptionen angeben** aus, welche Sicherheitseinstellung angewendet werden soll. Sie können sich für eine Änderung der Drosselung der Netzwerkbandbreite entscheiden, aber die Drosselung ist standardmäßig deaktiviert. Auch die Optionen **SAN-Wiederherstellung** und **Benachrichtigung** sind nicht aktiviert.
10. Überprüfen Sie Ihre Einstellungen auf dem Bildschirm **Zusammenfassung**, und wählen Sie **Wiederherstellen** aus, um den Wiederherstellungsprozess zu starten. Auf dem Bildschirm **Wiederherstellungsstatus** wird der Status des Wiederherstellungsvorgangs angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie den [Leitfaden zur Problembehandlung für Azure Backup Server](./backup-azure-mabs-troubleshoot.md). Darin finden Sie Informationen zur Behandlung von Problemen bei der Verwendung von Azure Backup Server.
