---
title: Wiederherstellen aller Dateien auf einem Volume mit MARS
description: Erfahren Sie, wie Sie alle Dateien eines Volumes mit dem MARS-Agent wiederherstellen können.
ms.topic: conceptual
ms.date: 01/17/2021
ms.openlocfilehash: 1d04e9f77b9f92594def9381f973c999e96b2cb2
ms.sourcegitcommit: db925ea0af071d2c81b7f0ae89464214f8167505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2021
ms.locfileid: "107516500"
---
# <a name="restore-all-the-files-in-a-volume-using-the-mars-agent"></a>Wiederherstellen aller Dateien auf einem Volume unter Verwendung des MARS-Agents

In diesem Artikel wird erklärt, wie Sie alle gesicherten Dateien eines gesamten Volumes unter Verwendung des Datenwiederherstellungs-Assistenten im Microsoft Azure Recovery Services-Agent (MARS) wiederherstellen können. Ihre Möglichkeiten:

- Wiederherstellen aller gesicherten Dateien eines Volumes auf dem gleichen Computer, auf dem die Sicherungen erstellt wurden.
- Wiederherstellen aller gesicherten Dateien eines Volumes auf einem alternativen Computer.

>[!TIP]
>Die Option **Volume** stellt alle gesicherten Daten auf einem angegebenen Volume wieder her. Mit dieser Option erreichen Sie höhere Übertragungsgeschwindigkeiten (bis zu 40 MBit/s), daher wird sie für das Wiederherstellen großer Daten oder ganzer Volumes empfohlen.
>
>Die Option **Einzelne Dateien und Ordner** ermöglicht den schnellen Zugriff auf Daten des Wiederherstellungspunkts. Sie eignet sich für die Wiederherstellung einzelner Dateien und wird für eine Gesamtgröße von unter 80 GB empfohlen. Sie bietet Übertragungs- oder Kopiergeschwindigkeiten von bis zu 6 MBit/s während der Wiederherstellung.

## <a name="volume-level-restore-to-the-same-machine"></a>Volumeebenen-Wiederherstellung auf demselben Computer

Mit den folgenden Schritten können Sie alle gesicherten Dateien eines Volumes wiederherstellen:

1. Öffnen Sie das Snap-In **Microsoft Azure Backup**. Falls Sie nicht wissen, wo das Snap-In installiert wurde, können Sie den für **Microsoft Azure Backup** verwendeten Computer oder Server durchsuchen. Die Desktop-App sollten in den Suchergebnissen angezeigt werden.

1. Wählen Sie **Daten wiederherstellen** aus, um den Assistenten zu starten.

    ![Menü „Daten wiederherstellen“](./media/restore-all-files-volume-mars/recover.png)

1. Wählen Sie im Bereich **Erste Schritte** die Option **Dieser Server (Servername)**  > **Weiter** aus, um die Daten auf demselben Server oder Computer wiederherzustellen.

    ![Seite „Erste Schritte“](./media/restore-all-files-volume-mars/same-machine-instant-restore.png)

1. Wählen Sie auf der Seite **Wiederherstellungsmodus auswählen** die Option **Volume** > **Weiter** aus.

    ![Wiederherstellungsmodus auswählen](./media/restore-all-files-volume-mars/select-recovery-mode.png)

1. Wählen Sie auf der Seite **Volume und Datum auswählen** das Volume aus, das Sie wiederherstellen möchten.

    Wählen Sie im Kalender einen Wiederherstellungspunkt aus. Wenn Datumsangaben in **Fettdruck** angezeigt werden, ist dies ein Hinweis darauf, dass mindestens ein Wiederherstellungszeitpunkt verfügbar ist. Falls mehrere Wiederherstellungspunkte für ein einzelnes Datum verfügbar sind, wählen Sie im Dropdownmenü **Uhrzeit** den jeweiligen Wiederherstellungspunkt aus.

     ![Auswählen von Volume und Datum](./media/restore-all-files-volume-mars/select-volume-and-date.png)

1. Konfigurieren Sie auf der Seite **Wiederherstellungsoptionen angeben** das Wiederherstellungsverhalten.
    1. Wählen Sie das Wiederherstellungsziel aus:
        - **Ursprünglicher Speicherort**: Daten werden unter dem ursprünglichen Pfad wiederhergestellt.
        - **Anderer Speicherort**: Geben Sie einen alternativen Speicherort an, an dem die Daten wiederhergestellt werden sollen.
    1. Wählen Sie das Verhalten für **Falls Elemente in der Sicherung bereits am Wiederherstellungsziel vorhanden sind** aus:
        - **Kopien erstellen, sodass beide Versionen vorhanden sind**: Wenn bereits eine Datei mit demselben Namen vorhanden ist, werden die Daten im Wiederherstellungspunkt als Kopie wiederhergestellt. Die Kopie weist ein lokalisiertes Dateinamenspräfix unter Verwendung der lokalen Zeit des Wiederherstellungsauftrags in einem der folgenden Formate auf:
            - `YYYY-MM-DD HH-mm Copy of <original file name>`
            - `YYYY-MM-DD HH-mm Copy (n) of <original file name>`
        - **Vorhandene Versionen mit den wiederhergestellten Versionen überschreiben**: Wenn bereits eine Datei mit demselben Namen vorhanden ist, werden die Inhalte durch die Daten aus dem Wiederherstellungspunkt wiederhergestellt.
        - **Am Wiederherstellungsziel bereits vorhandene Elemente nicht wiederherstellen**: Wenn bereits eine Datei mit demselben Namen vorhanden ist, wird Sie übersprungen.
    1. Aktivieren Sie **ACL-Berechtigungen für wiederherzustellende(n) Datei/Ordner wiederherstellen**, wenn die Datei mit den ursprünglichen Berechtigungen im Wiederherstellungspunkt wiederhergestellt werden soll.
        ![Wiederherstellungsoptionen angeben](./media/restore-all-files-volume-mars/specify-recovery-options.png)

1. Überprüfen Sie die Details der Wiederherstellung im Bereich **Bestätigung**, und wählen Sie dann **Wiederherstellen** aus.

    ![Bestätigungsdetails](./media/restore-all-files-volume-mars/confirmation-details.png)

1. Überwachen Sie auf der Seite **Wiederherstellungsfortschritt** den Fortschritt des Wiederherstellungsauftrags. Der Assistent kann auch geschlossen werden. Der Wiederherstellungsvorgang wird im Hintergrund fortgesetzt. Um den Fortschritt wieder anzuzeigen, doppelklicken Sie auf den Wiederherstellungsauftrag im Dashboard.

## <a name="volume-level-restore-to-an-alternate-machine"></a>Volumeebenen-Wiederherstellung auf einem alternativen Computer

Mit den folgenden Schritten können Sie alle gesicherten Dateien eines Volumes auf einem alternativen Computer wiederherstellen. Sie können diese Schritte verwenden, um Daten aus Azure Backup wiederherzustellen, wenn Ihr gesamter Server verloren gegangen ist.

Bei diesen Schritte wird die folgende Terminologie verwendet:

- *Quellcomputer:* der ursprüngliche Computer, von dem die Sicherung erstellt wurde und der zurzeit nicht verfügbar ist.
- *Zielcomputer* – Der Computer, auf dem die Daten wiederhergestellt werden.
- *Beispieltresor:* der Recovery Services-Tresor, bei dem der Quellcomputer und der Zielcomputer registriert sind.

> [!NOTE]
> Sicherungen können nicht auf einem Zielcomputer wiederhergestellt werden, auf dem eine ältere Version des Betriebssystems ausgeführt wird. Beispielsweise kann eine Sicherung, die von einem Windows 7-Computer erstellt wurde, auf einem Windows 7-Computer (oder höher) wiederhergestellt werden. Es ist aber nicht möglich, eine Sicherung, die von einem Windows 10-Computer erstellt wurde, auf einem Windows 7-Computer wiederherzustellen.

1. Öffnen Sie das Snap-In **Microsoft Azure Backup** auf dem Zielcomputer.

1. Stellen Sie sicher, dass der Zielcomputer und der Quellcomputer beim selben Recovery Services-Tresor registriert sind.

1. Wählen Sie **Daten wiederherstellen** aus, um den **Assistenten zum Wiederherstellen von Daten** zu öffnen.

    ![Screenshot von Azure Backup mit hervorgehobener Option „Daten wiederherstellen“ (Wiederherstellen auf einem anderen Computer)](./media/backup-azure-restore-windows-server/recover.png)

1. Wählen Sie auf der Seite **Erste Schritte** die Option **Anderer Server** aus.

    ![Screenshot der Seite „Erste Schritte“ des Assistenten zum Wiederherstellen von Daten (Wiederherstellen auf einem anderen Computer)](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

1. Geben Sie die Datei mit den Tresoranmeldeinformationen an, die dem Beispieltresor entspricht, und wählen Sie **Weiter** aus.

    Wenn die Datei mit den Tresoranmeldeinformationen ungültig (oder abgelaufen) ist, [laden Sie eine neue Anmeldeinformationsdatei für den Tresor aus dem Beispieltresor im Azure-Portal herunter](backup-azure-file-folder-backup-faq.yml#where-can-i-download-the-vault-credentials-file-). Nachdem Sie gültige Tresoranmeldeinformationen angegeben haben, wird der Name des entsprechenden Sicherungstresors angezeigt.

1. Wählen Sie auf der Seite **Sicherungsserver auswählen** in der Liste mit den angezeigten Computern den Quellcomputer aus, und geben Sie die Passphrase an. Wählen Sie **Weiter** aus.

    ![Screenshot der Seite „Sicherungsserver auswählen“ des Assistenten zum Wiederherstellen von Daten (Wiederherstellen auf einem anderen Computer)](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

1. Wählen Sie auf der Seite **Wiederherstellungsmodus auswählen** die Option **Volume** > **Weiter** aus.

    ![Wiederherstellungsmodus auswählen](./media/restore-all-files-volume-mars/select-recovery-mode.png)

1. Wählen Sie auf der Seite **Volume und Datum auswählen** das Volume aus, das Sie wiederherstellen möchten.

    Wählen Sie im Kalender einen Wiederherstellungspunkt aus. Wenn Datumsangaben in **Fettdruck** angezeigt werden, ist dies ein Hinweis darauf, dass mindestens ein Wiederherstellungszeitpunkt verfügbar ist. Falls mehrere Wiederherstellungspunkte für ein einzelnes Datum verfügbar sind, wählen Sie im Dropdownmenü **Uhrzeit** den jeweiligen Wiederherstellungspunkt aus.

     ![Auswählen von Volume und Datum](./media/restore-all-files-volume-mars/select-volume-and-date.png)

1. Konfigurieren Sie auf der Seite **Wiederherstellungsoptionen angeben** das Wiederherstellungsverhalten.
    1. Wählen Sie das Wiederherstellungsziel aus:
        - **Ursprünglicher Speicherort**: Daten werden unter dem ursprünglichen Pfad wiederhergestellt.
        - **Anderer Speicherort**: Geben Sie einen alternativen Speicherort an, an dem die Daten wiederhergestellt werden sollen.
    1. Wählen Sie das Verhalten für **Falls Elemente in der Sicherung bereits am Wiederherstellungsziel vorhanden sind** aus:
        - **Kopien erstellen, sodass beide Versionen vorhanden sind**: Wenn bereits eine Datei mit demselben Namen vorhanden ist, werden die Daten im Wiederherstellungspunkt als Kopie wiederhergestellt. Die Kopie weist ein lokalisiertes Dateinamenspräfix unter Verwendung der lokalen Zeit des Wiederherstellungsauftrags in einem der folgenden Formate auf:
            - `YYYY-MM-DD HH-mm Copy of <original file name>`
            - `YYYY-MM-DD HH-mm Copy (n) of <original file name>`
        - **Vorhandene Versionen mit den wiederhergestellten Versionen überschreiben**: Wenn bereits eine Datei mit demselben Namen vorhanden ist, werden die Inhalte durch die Daten aus dem Wiederherstellungspunkt wiederhergestellt.
        - **Am Wiederherstellungsziel bereits vorhandene Elemente nicht wiederherstellen**: Wenn bereits eine Datei mit demselben Namen vorhanden ist, wird Sie übersprungen.
    1. Aktivieren Sie **ACL-Berechtigungen für wiederherzustellende(n) Datei/Ordner wiederherstellen**, wenn die Datei mit den ursprünglichen Berechtigungen im Wiederherstellungspunkt wiederhergestellt werden soll.
        ![Wiederherstellungsoptionen angeben](./media/restore-all-files-volume-mars/specify-recovery-options.png)

1. Überprüfen Sie die Details der Wiederherstellung im Bereich **Bestätigung**, und wählen Sie dann **Wiederherstellen** aus.

    ![Bestätigungsdetails](./media/restore-all-files-volume-mars/confirmation-details.png)

1. Überwachen Sie auf der Seite **Wiederherstellungsfortschritt** den Fortschritt des Wiederherstellungsauftrags. Der Assistent kann auch geschlossen werden. Der Wiederherstellungsvorgang wird im Hintergrund fortgesetzt. Um den Fortschritt wieder anzuzeigen, doppelklicken Sie auf den Wiederherstellungsauftrag im Dashboard.

## <a name="next-steps"></a>Nächste Schritte

- Nachdem Sie nun Ihre Dateien und Ordner wiederhergestellt haben, können Sie [Ihre Sicherungen verwalten](backup-azure-manage-windows-server.md).
- Lesen Sie die Informationen unter [Häufig gestellte Fragen zum Sichern von Dateien und Ordnern](backup-azure-file-folder-backup-faq.yml).
