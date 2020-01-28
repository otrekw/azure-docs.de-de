---
title: Wiederherstellen von Azure-Dateifreigaben
description: Erfahren Sie, wie Sie das Azure-Portal verwenden, um eine vollständige Dateifreigabe oder bestimmte Dateien von einem vom Azure Backup-Dienst erstellten Wiederherstellungspunkt wiederherstellen.
ms.topic: conceptual
ms.date: 01/12/2020
ms.openlocfilehash: b16eb4120ff2d269135ae8ae6555ef4fdbdbda5d
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294844"
---
# <a name="restore-azure-file-shares"></a>Wiederherstellen von Azure-Dateifreigaben

In diesem Artikel wird erläutert, wie Sie das Azure-Portal verwenden, um eine vollständige Dateifreigabe oder bestimmte Dateien von einem vom [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview)-Dienst erstellten Wiederherstellungspunkt wiederherstellen.

In diesem Leitfaden lernen Sie Folgendes:

* Wiederherstellen einer vollständigen Azure-Dateifreigabe
* Wiederherstellen einzelner Dateien oder Ordner
* Nachverfolgen des Status des Wiederherstellungsvorgangs

## <a name="steps-to-perform-restore"></a>Für eine Wiederherstellung auszuführende Schritte

### <a name="select-the-file-share-to-restore"></a>Auswählen der Dateifreigabe, die wiederhergestellt werden soll

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com/) den Recovery Services-Tresor, den Sie zur Konfiguration der Sicherung für die Dateifreigabe verwendet haben.

2. Klicken Sie auf dem Blatt **Übersicht** im Abschnitt **Geschützte Elemente** auf **Sicherungselemente**.

    ![Klicken Sie auf „Sicherungselemente“](./media/restore-afs/backup-items.png)

3. Sobald Sie auf **Sicherungselemente** geklickt haben, wird neben dem Blatt **Übersicht** angezeigt ein neues Blatt mit allen Sicherungsverwaltungstypen wie folgt angezeigt:

    ![Sicherungsverwaltungstypen](./media/restore-afs/backup-management.png)

4. Wählen Sie im Menü **Sicherungselemente** unter **Sicherungsverwaltungstyp** die Option **Azure Storage (Azure Files)** aus. Angezeigt wird eine Liste aller Dateifreigaben und der entsprechenden Speicherkonten, die mit diesem Tresor gesichert werden.

    ![Liste aller Dateifreigaben](./media/restore-afs/file-shares.png)

5. Wählen Sie aus der Liste der Azure-Dateifreigaben die gewünschte Dateifreigabe aus, für die Sie den Wiederherstellungsvorgang durchführen möchten.

### <a name="full-share-recovery"></a>Wiederherstellung der vollständigen Freigabe

Sie können diese Wiederherstellungsoption verwenden, um die vollständige Dateifreigabe am ursprünglichen oder einem alternativen Speicherort wiederherzustellen.

1. Wählen Sie die Option **Freigabe wiederherstellen** auf dem Blatt **Sicherungselement** aus, das nach dem Auswählen der gewünschten Dateifreigabe für die Wiederherstellung in Schritt 5 des Abschnitts [Auswählen der Dateifreigabe, die wiederhergestellt werden soll](#select-the-file-share-to-restore) angezeigt wird.

   ![Wiederherzustellende Freigabe auswählen](./media/restore-afs/restore-share.png)

2. Sobald Sie auf **Freigabe wiederherstellen** geklickt haben, wird das Blatt **Wiederherstellen** mit einem Menü **Wiederherstellungspunkt** und einer Liste der für die ausgewählte Dateifreigabe verfügbaren Wiederherstellungspunkte anzeigt.

3. Wählen Sie den Wiederherstellungspunkt aus, den Sie für die Wiederherstellung verwenden möchten, und klicken Sie auf **OK**.

    ![Wiederherstellungspunkt auswählen](./media/restore-afs/restore-point.png)

4. Nachdem Sie auf „OK“ geklickt haben, wechselt das Menü „Wiederherstellen“ zu **Speicherort für die Wiederherstellung**. Geben Sie unter **Speicherort für die Wiederherstellung** an, wo (oder wie) die Daten wiederhergestellt werden sollen. Sie haben zwei Möglichkeiten:

    * **Ursprünglicher Speicherort**: Die gesamte Dateifreigabe wird am gleichen Speicherort wie die ursprüngliche Quelle wieder hergestellt.
    * **Alternativer Standort:** Die gesamte Dateifreigabe wird an einem alternativen Speicherort wiederhergestellt und die ursprüngliche Dateifreigabe **unverändert** beibehalten.

#### <a name="restore-to-original-location"></a>Wiederherstellen am ursprünglichen Speicherort

1. Wählen Sie **Ursprünglicher Speicherort** als **Wiederherstellungsziel** aus, und entscheiden Sie dann, ob Daten bei Konflikten übersprungen oder überschrieben werden sollen. Nachdem Sie die entsprechende Auswahl getroffen haben, klicken Sie auf **OK**.

    ![„Ursprünglicher Speicherort“ auswählen](./media/restore-afs/original-location.png)

2. Klicken Sie auf **Wiederherstellen**, um den Wiederherstellungsvorgang zu starten.

    ![Zum Starten auf „Wiederherstellen“ klicken](./media/restore-afs/click-restore.png)

#### <a name="restore-to-an-alternate-location"></a>Wiederherstellen an einem alternativen Speicherort

1. Wählen Sie **Alternativer Speicherort** als Wiederherstellungsziel aus.
2. Wählen Sie im Dropdownmenü des Felds **Speicherkonto** das Zielspeicherkonto aus, in dem Sie den gesicherten Inhalt wiederherstellen möchten.
3. Basierend auf dem in Schritt 2 ausgewählten Speicherkonto wird im Dropdownmenü **Dateifreigabe auswählen** die Liste der im ausgewählten Speicherkonto vorhandenen Dateifreigaben angezeigt. Wählen Sie die Dateifreigabe aus, in der Sie die gesicherten Inhalte wiederherstellen möchten.
4. Geben Sie im Feld **Ordnername** den Namen eines Ordners ein, den Sie in der Zieldateifreigabe mit den wiederhergestellten Inhalten erstellen möchten.
5. Wählen Sie aus, ob Daten bei Konflikten übersprungen oder überschrieben werden sollen.
6. Nachdem Sie die entsprechenden Werte in alle Felder eingegeben haben, klicken Sie auf **OK**.

    ![„Alternativer Speicherort“ auswählen](./media/restore-afs/alternate-location.png)

7. Klicken Sie auf „Wiederherstellen“, um den Wiederherstellungsvorgang zu starten.

    ![Zum Starten auf „Wiederherstellen“ klicken](./media/restore-afs/click-restore.png)

### <a name="item-level-recovery"></a>Wiederherstellung auf Elementebene

Sie können diese Wiederherstellungsoption verwenden, um einzelne Dateien oder Ordner am ursprünglichen oder einem alternativen Speicherort wiederherzustellen.

1. Wählen Sie die Option **Dateiwiederherstellung** auf dem Blatt **Sicherungselement** aus, das nach dem Auswählen der gewünschten Dateifreigabe für die Wiederherstellung in Schritt 5 des Abschnitts [Auswählen der Dateifreigabe, die wiederhergestellt werden soll](#select-the-file-share-to-restore) angezeigt wird.

    ![„Dateiwiederherstellung“ auswählen](./media/restore-afs/file-recovery.png)

2. Sobald Sie auf **Dateiwiederherstellung** geklickt haben, wird das Blatt **Wiederherstellen** mit einem Menü **Wiederherstellungspunkt** und einer Liste der für die ausgewählte Dateifreigabe verfügbaren Wiederherstellungspunkte anzeigt.

3. Wählen Sie den Wiederherstellungspunkt aus, den Sie für die Wiederherstellung verwenden möchten, und klicken Sie auf **OK**.

    ![Wiederherstellungspunkt auswählen](./media/restore-afs/restore-point.png)

4. Nachdem Sie auf „OK“ geklickt haben, wechselt das Menü „Wiederherstellen“ zu **Speicherort für die Wiederherstellung**. Geben Sie unter **Speicherort für die Wiederherstellung** an, wo (oder wie) die Daten wiederhergestellt werden sollen. Sie haben zwei Möglichkeiten:

    * **Ursprünglicher Speicherort**: Die ausgewählten Dateien/Ordner werden in derselben Dateifreigabe wie die ursprüngliche Quelle wiederhergestellt.
    * **Alternativer Standort:** Die ausgewählten Dateien/Ordner werden an einem alternativen Speicherort wiederhergestellt und der ursprüngliche Inhalt der Dateifreigabe bleibt **unverändert** erhalten.

#### <a name="restore-to-original-location"></a>An ursprünglichem Speicherort wiederherstellen

1. Wählen Sie **Ursprünglicher Speicherort** als **Wiederherstellungsziel** aus, und entscheiden Sie dann, ob Daten bei Konflikten übersprungen oder überschrieben werden sollen.

    ![Ursprünglicher Speicherort für Wiederherstellung auf Elementebene](./media/restore-afs/original-location-item-level.png)

2. Klicken Sie auf **Datei auswählen**, um die Dateien/Ordner auszuwählen, die Sie wiederherstellen möchten.

    ![Auf „Datei auswählen“ klicken](./media/restore-afs/select-file.png)

3. Wenn Sie auf **Datei auswählen** klicken, wird ein Dateifreigabeblatt mit dem Inhalt des für die Wiederherstellung ausgewählten Dateifreigabe-Wiederherstellungspunkts angezeigt.

4. Aktivieren Sie das Kontrollkästchen für die Datei bzw. den Ordner, die/den Sie wiederherstellen möchten, und klicken Sie auf **Auswählen**.

    ![Datei oder Ordner auswählen](./media/restore-afs/select-file-folder.png)

5. Wiederholen Sie die Schritte 2-4, um mehrere Dateien/Ordner für die Wiederherstellung auszuwählen.
6. Nachdem Sie alle Elemente ausgewählt haben, die Sie wiederherstellen möchten, klicken Sie auf **OK**.

    ![Nachdem Sie alle Elemente ausgewählt haben, klicken Sie auf OK](./media/restore-afs/after-selecting-items.png)

7. Klicken Sie auf „Wiederherstellen“, um den Wiederherstellungsvorgang zu starten.

    ![Zum Starten auf „Wiederherstellen“ klicken](./media/restore-afs/click-restore.png)

#### <a name="restore-to-an-alternate-location"></a>Wiederherstellen an einem alternativen Speicherort

1. Wählen Sie **Alternativer Speicherort** als Wiederherstellungsziel aus.
2. Wählen Sie im Dropdownmenü des Felds **Speicherkonto** das Zielspeicherkonto aus, in dem Sie den gesicherten Inhalt wiederherstellen möchten.
3. Basierend auf dem in Schritt 2 ausgewählten Speicherkonto wird im Dropdownmenü **Dateifreigabe auswählen** die Liste der im ausgewählten Speicherkonto vorhandenen Dateifreigaben angezeigt. Wählen Sie die Dateifreigabe aus, in der Sie die gesicherten Inhalte wiederherstellen möchten.
4. Geben Sie im Feld **Ordnername** den Namen eines Ordners ein, den Sie in der Zieldateifreigabe mit den wiederhergestellten Inhalten erstellen möchten.
5. Wählen Sie aus, ob Daten bei Konflikten übersprungen oder überschrieben werden sollen.
6. Klicken Sie auf **Datei auswählen**, um die Dateien/Ordner auszuwählen, die Sie wiederherstellen möchten.

    ![Elemente zum Wiederherstellen an einem alternativen Speicherort auswählen](./media/restore-afs/restore-to-alternate-location.png)

7. Wenn Sie auf **Datei auswählen** klicken, wird ein Dateifreigabeblatt mit dem Inhalt des für die Wiederherstellung ausgewählten Dateifreigabe-Wiederherstellungspunkts angezeigt.
8. Aktivieren Sie das Kontrollkästchen für die Datei bzw. den Ordner, die/den Sie wiederherstellen möchten, und klicken Sie auf **Auswählen**.

    ![Wiederherstellungsziel auswählen](./media/restore-afs/recovery-destination.png)

9. Wiederholen Sie die Schritte 6-8, um mehrere Dateien/Ordner für die Wiederherstellung auszuwählen.
10. Nachdem Sie alle Elemente ausgewählt haben, die Sie wiederherstellen möchten, klicken Sie auf **OK**.

    [Klicken Sie nach dem Auswählen aller Dateien auf „OK“.](./media/restore-afs/after-selecting-all-items.png)

11. Klicken Sie auf **Wiederherstellen**, um den Wiederherstellungsvorgang zu starten.

## <a name="track-restore-operation"></a>Nachverfolgen des Wiederherstellungsvorgangs

Nachdem Sie den Wiederherstellungsvorgang ausgelöst haben, erstellt der Sicherungsdienst einen Auftrag zum Nachverfolgen. Azure Backup zeigt Benachrichtigungen zum Auftrag im Portal an. Um die Vorgänge für den Auftrag anzuzeigen, klicken Sie auf den Hyperlink für Benachrichtigungen.

![Auf Benachrichtigungslink klicken](./media/restore-afs/notifications-link.png)

Sie können den Fortschritt der Wiederherstellung auch über den Recovery Services-Tresor überwachen. Hier finden Sie die Schritte zum Überprüfen des Status des Wiederherstellungsvorgangs:

1. Öffnen Sie den Recovery Services-Tresor, von dem aus Sie den Wiederherstellungsvorgang ausgelöst haben.
2. Klicken Sie unter dem **Überwachungsabschnitt** des Blatts **Übersicht** auf **Sicherungsaufträge**, um den Status der für verschiedene Workloads ausgeführten Vorgänge anzuzeigen.

    ![Auf „Sicherungsaufträge“ klicken](./media/restore-afs/backup-jobs.png)

3. Klicken Sie auf den Namen der Workload, die Ihrer Dateifreigabe entspricht, um weitere Details über den Wiederherstellungsvorgang anzuzeigen, wie z. B. übertragene Daten, Anzahl der wiederhergestellten Dateien usw.

    ![Details zur Wiederherstellung anzeigen](./media/restore-afs/restore-details.png)

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten der Sicherungen von Azure-Dateifreigaben](manage-afs-backup.md)
