---
title: 'Tutorial: Wiederherstellen von Elementen unter Windows Server'
description: In diesem Tutorial erfahren Sie, wie Sie den MARS-Agent (Microsoft Azure Recovery Services-Agent) zum Wiederherstellen von Elementen aus Azure auf einem Windows-Server verwenden.
ms.topic: tutorial
ms.date: 02/14/2018
ms.custom: mvc
ms.openlocfilehash: 746c901747cf1c0b87612a31fbabcb657d5c4a0c
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "88263111"
---
# <a name="recover-files-from-azure-to-a-windows-server"></a>Wiederherstellen von Dateien aus Azure auf einer Windows Server-Instanz

Azure Backup ermöglicht die Wiederherstellung einzelner Elemente aus Sicherungen Ihrer Windows Server-Instanzen. Das Wiederherstellen einzelner Dateien ist nützlich, wenn Sie Dateien, die versehentlich gelöscht wurden, schnell wiederherstellen müssen. In diesem Tutorial erfahren Sie, wie Sie den Microsoft Azure Recovery Services-Agent (MARS) verwenden, um Elemente aus Sicherungen, die Sie zuvor in Azure erstellt haben, wiederherzustellen. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Initiieren der Wiederherstellung einzelner Elemente
> * Wählen Sie einen Wiederherstellungspunkt
> * Wiederherstellen von Elementen aus einem Wiederherstellungspunkt

In diesem Tutorial wird davon ausgegangen, dass Sie bereits die Schritte zum [Sichern einer Windows Server-Instanz in Azure](backup-windows-with-mars-agent.md) ausgeführt haben und über mindestens eine Sicherung Ihrer Windows Server-Dateien in Azure verfügen.

## <a name="initiate-recovery-of-individual-items"></a>Initiieren der Wiederherstellung einzelner Elemente

Zusammen mit dem Microsoft Azure Recovery Services-Agent (MARS) wird ein hilfreicher Assistent mit Benutzeroberfläche installiert: Microsoft Azure Backup. Der Microsoft Azure Backup-Assistent arbeitet mit dem Microsoft Azure Recovery Services-Agent (MARS) zusammen, um Sicherungsdaten aus Wiederherstellungspunkten, die in Azure gespeichert sind, abzurufen. Verwenden Sie den Microsoft Azure Backup-Assistenten zum Identifizieren der Dateien oder Ordner, die Sie unter Windows Server wiederherstellen möchten.

1. Öffnen Sie das Snap-In **Microsoft Azure Backup**. Den Agent finden Sie, indem Sie auf Ihrem Computer nach **Microsoft Azure Backup** suchen.

    ![Microsoft Azure Backup-Snap-In](./media/tutorial-backup-restore-files-windows-server/mars.png)

2. Wählen Sie im Assistenten im Bereich **Aktionen** der Agent-Konsole die Option **Daten wiederherstellen** aus, um den Assistenten **Daten wiederherstellen** zu starten.

    ![Auswählen von „Daten wiederherstellen“](./media/tutorial-backup-restore-files-windows-server/mars-recover-data.png)

3. Wählen Sie auf der Seite **Erste Schritte** die Option **Dieser Server (Servername)** und dann **Weiter** aus.

4. Wählen Sie auf der Seite **Wiederherstellungsmodus auswählen** die Option **Einzelne Dateien und Ordner** und dann **Weiter** aus, um den Auswahlprozess für den Wiederherstellungspunkt zu starten.

5. Wählen Sie auf der Seite **Volume und Datum auswählen** das Volume aus, das die wiederherzustellenden Dateien bzw. Ordner enthält, und klicken Sie dann auf **Einbinden**. Wählen Sie im Dropdownmenü ein Datum und einen Zeitpunkt für einen Wiederherstellungspunkt aus. Wenn Datumsangaben **fett** angezeigt werden, ist dies ein Hinweis darauf, dass mindestens ein Wiederherstellungszeitpunkt mit diesem Datum verfügbar ist.

    ![Auswählen von Volume und Datum](./media/tutorial-backup-restore-files-windows-server/mars-select-date.png)

    Beim Auswählen von **Einbinden** macht Azure Backup den Wiederherstellungspunkt als Datenträger verfügbar. Suchen Sie die Dateien auf dem Datenträger, und stellen Sie sie wieder her.

## <a name="restore-items-from-a-recovery-point"></a>Wiederherstellen von Elementen aus einem Wiederherstellungspunkt

1. Wählen Sie nach dem Einbinden des Wiederherstellungsvolumes **Durchsuchen** aus, um Windows-Explorer zu öffnen und nach den Dateien und Ordnern für die Wiederherstellung zu suchen.

    ![Auswählen von „Durchsuchen“](./media/tutorial-backup-restore-files-windows-server/mars-browse-recover.png)

    Sie können die Dateien direkt vom Wiederherstellungsvolume öffnen und überprüfen.

2. Kopieren Sie in Windows-Explorer die wiederherzustellenden Dateien und Ordner, und fügen Sie sie am gewünschten Speicherort auf dem Server ein.

    ![Kopieren der Dateien und Ordner](./media/tutorial-backup-restore-files-windows-server/mars-final.png)

3. Wählen Sie im Assistenten **Daten wiederherstellen** auf der Seite **Dateien suchen und wiederherstellen** die Option **Bereitstellung aufheben** aus, wenn Sie die Wiederherstellung der Dateien und Ordner abgeschlossen haben.

    ![Auswählen von „Bereitstellung aufheben“](./media/tutorial-backup-restore-files-windows-server/unmount-and-confirm.png)

4. Wählen Sie **Ja** aus, um zu bestätigen, dass Sie die Einbindung des Volumes aufheben möchten.

    Nachdem die Einbindung der Momentaufnahme aufgehoben wurde, wird in der Agent-Konsole im Bereich **Aufträge** der Status **Auftrag abgeschlossen** angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Dies schließt die Tutorials zum Sichern und Wiederherstellen von Windows Server-Daten in Azure ab. Informieren Sie sich über Azure Backup, und sehen Sie sich das PowerShell-Beispiel zum Sichern verschlüsselter virtueller Computer an.

> [!div class="nextstepaction"]
> [Sichern verschlüsselter virtueller Computer](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
