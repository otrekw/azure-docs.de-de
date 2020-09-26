---
title: Sichern von Windows-Computern mit dem MARS-Agent
description: Verwenden Sie den Microsoft Azure Recovery Services-Agent (MARS) zum Sichern von Windows-Computern.
ms.topic: conceptual
ms.date: 03/03/2020
ms.openlocfilehash: d2cdd5d1fa98462a70d72fd9f8723685952b665a
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2020
ms.locfileid: "90052221"
---
# <a name="back-up-windows-server-files-and-folders-to-azure"></a>Sichern von Windows Server-Dateien und -Ordnern in Azure

Dieser Artikel erklärt, wie Sie Windows-Computer mit dem [Azure Backup](backup-overview.md)-Dienst und dem Microsoft Azure Recovery Services-Agent (MARS) sichern. MARS wird auch als Azure Backup-Agent bezeichnet.

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Überprüfen der Voraussetzungen
> * Erstellen einer Sicherungsrichtlinie und eines Zeitplans
> * Durchführen eines bedarfsgesteuerten Sicherungsauftrags

## <a name="before-you-start"></a>Vorbereitung

* Informieren Sie sich darüber, wie [Azure Backup den MARS-Agent zum Sichern von Windows-Computern verwendet](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders).
* Informieren Sie sich über die [Sicherungsarchitektur](backup-architecture.md#architecture-back-up-to-dpmmabs), die den MARS-Agent auf einem sekundären MABS- oder Data Protection Manager-Server ausführt.
* Überprüfen Sie, [was unterstützt wird und was mit dem MARS-Agent gesichert werden kann](backup-support-matrix-mars-agent.md).
* [Überprüfen Sie den Internetzugriff](install-mars-agent.md#verify-internet-access) auf den Computern, die Sie sichern möchten.
* Wenn der MARS-Agent nicht installiert ist, erfahren Sie [hier](install-mars-agent.md), wie er installiert wird.

## <a name="create-a-backup-policy"></a>Erstellen einer Sicherungsrichtlinie

Die Sicherungsrichtlinie legt fest, wann Momentaufnahmen der Daten zur Erstellung von Wiederherstellungspunkten erstellt werden sollen und wie lange die Wiederherstellungspunkte aufbewahrt werden. Sie konfigurieren mit dem MARS-Agent eine Sicherungsrichtlinie.

Bei Azure Backup wird die Sommerzeit nicht automatisch berücksichtigt. Dieser Standard kann zu Abweichungen zwischen der tatsächlichen und der geplanten Sicherungszeit führen.

So erstellen Sie eine Sicherungsrichtlinie

1. Nachdem Sie den MARS-Agent heruntergeladen und registriert haben, öffnen Sie die Agent-Konsole. Den Agent finden Sie, indem Sie auf Ihrem Computer nach **Microsoft Azure Backup**suchen.  

1. Wählen Sie unter **Aktionen** die Option **Sicherung planen** aus.

    ![Planen einer Windows Server-Sicherung](./media/backup-configure-vault/schedule-first-backup.png)
1. Wählen Sie im Assistenten zum Planen der Sicherung die Option **Erste Schritte** > **Weiter**.
1. Wählen Sie unter **Elemente für Sicherung auswählen** die Option **Elemente hinzufügen**.

    ![Hinzufügen von Elementen zur Sicherung](./media/backup-azure-manage-mars/select-item-to-backup.png)

1. Wählen Sie im Feld **Elemente auswählen** zu sichernde Elemente aus, und wählen Sie dann **OK** aus.

    ![Auswählen von Elementen für die Sicherung](./media/backup-azure-manage-mars/selected-items-to-backup.png)

1. Wählen Sie auf der Seite **Elemente für Sicherung auswählen** die Option **Weiter** aus.
1. Geben Sie auf der Seite **Sicherungszeitplan angeben** an, wann tägliche oder wöchentliche Sicherungen erstellt werden sollen. Wählen Sie **Weiter**aus.

    * Ein Wiederherstellungspunkt wird gleichzeitig mit einer Sicherung erstellt.
    * Die Anzahl der in Ihrer Umgebung erstellten Wiederherstellungspunkte hängt von Ihrem Sicherungszeitplan ab.
    * Sie können bis zu drei tägliche Sicherungen pro Tag planen. Im folgenden Beispiel werden zwei tägliche Sicherungen durchgeführt, eine um Mitternacht und eine um 18 Uhr.

        ![Einrichten eines täglichen Sicherungszeitplans](./media/backup-configure-vault/day-schedule.png)

    * Sie können auch wöchentliche Sicherungen ausführen. Im folgenden Beispiel werden Sicherungen jeden zweiten Sonntag und Mittwoch um 9:30 und 1:00 Uhr erstellt.

        ![Einrichten eines wöchentlichen Sicherungszeitplans](./media/backup-configure-vault/week-schedule.png)

1. Legen Sie auf der Seite **Aufbewahrungsrichtlinie auswählen** fest, wie historische Kopien Ihrer Daten gespeichert werden. Wählen Sie **Weiter**aus.

    * Aufbewahrungseinstellungen geben an, welche Wiederherstellungspunkte gespeichert und wie lange sie gespeichert werden sollen.
    * Für die Einstellung einer täglichen Aufbewahrung geben Sie an, dass zu dem für die tägliche Aufbewahrung angegebenen Zeitpunkt der neueste Wiederherstellungspunkt für die angegebene Anzahl von Tagen beibehalten wird. Oder Sie können eine monatliche Aufbewahrungsrichtlinie angeben, die angibt, dass der am 30. eines jeden Monats erstellte Wiederherstellungspunkt für 12 Monate gespeichert werden soll.
    * Die tägliche und wöchentliche Aufbewahrung der Wiederherstellungspunkte stimmt in der Regel mit dem Sicherungszeitplan überein. Wenn der Zeitplan also eine Sicherung auslöst, wird der Wiederherstellungspunkt, der von der Sicherung erstellt wird, für die Dauer gespeichert, die die Richtlinie für tägliche oder wöchentliche Aufbewahrung angibt.
    * Siehe folgendes Beispiel:

        * Tägliche Sicherungen um Mitternacht und 18:00 Uhr werden sieben Tage lang aufbewahrt.
        * Sicherungen, die an einem Samstag um Mitternacht und 18 Uhr erstellt werden, werden vier Wochen lang aufbewahrt.
        * Sicherungen, die am letzten Samstag im Monat um Mitternacht und 18 Uhr erstellt werden, werden 12 Monate lang aufbewahrt.
        * Sicherungen, die am letzten Samstag im März erstellt wurden, werden zehn Jahre lang aufbewahrt.

        ![Beispiel für eine Aufbewahrungsrichtlinie](./media/backup-configure-vault/retention-example.png)

1. Wählen Sie auf der Seite **Erstsicherungstyp auswählen** aus, ob Sie die Erstsicherung über das Netzwerk ausführen oder eine Offlinesicherung verwenden möchten. Um die Erstsicherung über das Netzwerk auszuführen, wählen Sie **Automatisch über das Netzwerk** > **Weiter** aus.

    Weitere Informationen zur Offlinesicherung finden Sie unter [Offlinesicherung in Azure Backup unter Verwendung von Azure Data Box](offline-backup-azure-data-box.md).

    ![Erstsicherungstyp wählen](./media/backup-azure-manage-mars/choose-initial-backup-type.png)

1. Lesen Sie sich die Informationen auf der Seite **Bestätigung** durch, und wählen Sie dann **Fertig stellen** aus.

    ![Bestätigen des Sicherungstyps](./media/backup-azure-manage-mars/confirm-backup-type.png)

1. Wählen Sie **Schließen** aus, nachdem der Assistent die Erstellung des Sicherungszeitplans abgeschlossen hat.

    ![Anzeigen des Fortschritts des Sicherungszeitplans](./media/backup-azure-manage-mars/confirm-modify-backup-process.png)

Erstellen Sie auf jedem Computer, auf dem der Agent installiert ist, eine Richtlinie.

### <a name="do-the-initial-backup-offline"></a>Durchführen der ersten Sicherung offline

Sie können eine erste Sicherung automatisch über das Netzwerk durchführen, oder Sie können offline sichern. Wenn Sie bei der ersten Sicherung große Datenmengen haben, die viel Netzwerkbandbreite für die Übertragung benötigen, ist das Offlineseeding sehr praktisch.

Ausführen einer Offlineübertragung:

1. Schreiben Sie die Sicherungsdaten an einen Stagingspeicherort.
1. Verwenden Sie das Tool „AzureOfflineBackupDiskPrep“, um die Daten vom Stagingspeicherort auf einen oder mehrere SATA-Datenträger zu kopieren.

    Das Tool erstellt einen Azure-Importauftrag. Weitere Informationen finden Sie unter [Was ist der Azure Import/Export-Dienst?](../storage/common/storage-import-export-service.md).
1. Senden Sie die SATA-Datenträger an ein Azure-Rechenzentrum.

    Im Rechenzentrum werden die Daten auf den Datenträgern in ein Azure-Speicherkonto kopiert. Azure Backup kopiert die Daten aus dem Speicherkonto in den Tresor, und inkrementelle Sicherungen werden geplant.

Weitere Informationen zum Offlineseeding finden Sie unter [Offlinesicherung in Azure Backup unter Verwendung von Azure Data Box](offline-backup-azure-data-box.md).

### <a name="enable-network-throttling"></a>Aktivieren der Netzwerkdrosselung

Sie können steuern, wie der MARS-Agent die Netzwerkbandbreite nutzt, indem Sie die Netzwerkdrosselung aktivieren. Eine Drosselung ist hilfreich, wenn Sie Daten während der Arbeitszeit sichern müssen, aber steuern möchten, wie viel Bandbreite für Sicherungs- und Wiederherstellungsaktivitäten verwendet wird.

Für die Netzwerkdrosselung in Azure Backup wird [Quality of Service (QoS)](/windows-server/networking/technologies/qos/qos-policy-top) auf dem lokalen Betriebssystem verwendet.

Die Netzwerkdrosselung für Sicherungen ist ab Windows Server 2012 und Windows 8 verfügbar. Die Betriebssysteme müssen mit den neuesten Service Packs ausgeführt werden.

Aktivieren der Netzwerkdrosselung:

1. Wählen Sie im MARS-Agent die Option **Eigenschaften ändern** aus.
1. Aktivieren Sie auf der Registerkarte **Drosselung** die Option **Internet-Bandbreiteneinschränkung für Sicherungsvorgänge aktivieren**.

    ![Einrichten der Netzwerkdrosselung für Sicherungsvorgänge](./media/backup-configure-vault/throttling-dialog.png)
1. Geben Sie die zulässige Bandbreite während der Arbeit und außerhalb der Arbeitszeit an. Die Bandbreitenwerte beginnen bei 512 KBit/s und reichen bis zu 1.023 MBit/s. Klicken Sie anschließend auf **OK**.

## <a name="run-an-on-demand-backup"></a>Ausführen einer On-Demand-Sicherung

1. Wählen Sie im MARS-Agent die Option **Jetzt sichern** aus.

    ![Jetzt unter Windows Server sichern](./media/backup-configure-vault/backup-now.png)

1. Wenn die Version des MARS-Agent 2.0.9169.0 oder neuer ist, können Sie ein benutzerdefiniertes Datum für die Aufbewahrung festlegen. Wählen Sie im Abschnitt **Sicherung aufbewahren bis** im Kalender ein Datum aus.

   ![Verwenden des Kalenders zum Anpassen eines Aufbewahrungsdatums](./media/backup-configure-vault/mars-ondemand.png)

1. Überprüfen Sie auf der Seite **Bestätigung** die Einstellungen, und wählen Sie dann **Sichern** aus.
1. Wählen Sie **Schließen** aus, um den Assistenten zu schließen. Wenn Sie den Assistenten schließen, bevor der Sicherungsvorgang abgeschlossen ist, wird der Assistent im Hintergrund weiter ausgeführt.

Nach Abschluss der ersten Sicherung wird der Status des Auftrags in der Backup-Konsole als **Auftrag abgeschlossen** angezeigt.

## <a name="set-up-on-demand-backup-policy-retention-behavior"></a>Einrichten des Verhaltens der Richtlinie für bedarfsgesteuerte Sicherungen

> [!NOTE]
> Diese Informationen gelten nur für ältere MARS-Agentversionen als 2.0.9169.0.
>

| Optionaler Sicherungszeitplan | Dauer der Datenaufbewahrung
| -- | --
| Day (Tag) | **Standardaufbewahrungsdauer**: Entspricht „Aufbewahrung in Tagen für tägliche Sicherungen“. <br/><br/> **Ausnahme**: Wenn eine täglich geplante Sicherung für die Langzeitaufbewahrung (Wochen, Monate, Jahre) fehlschlägt, wird eine unmittelbar nach der fehlgeschlagenen geplanten Sicherung eine ausgelöste bedarfsgesteuerte Sicherung für die Langzeitaufbewahrung berücksichtigt. Andernfalls wird die nächste geplante Sicherung für die Langzeitaufbewahrung berücksichtigt.<br/><br/> **Beispielszenario**: Die geplante Sicherung am Donnerstag um 8:00 Uhr ist fehlgeschlagen. Diese Sicherung sollte für die wöchentliche, monatliche oder jährliche Aufbewahrung berücksichtigt werden. Also wird die erste bedarfsgesteuerte Sicherung, die vor der nächsten geplanten Sicherung am Freitag um 8:00 Uhr ausgelöst wird, automatisch für die wöchentliche, monatliche oder jährliche Aufbewahrung gekennzeichnet. Diese Sicherung ersetzt die Sicherung am Donnerstag um 8:00 Uhr.
| Week | **Standardaufbewahrungsdauer**: Ein Tag. Bedarfsgesteuerte Sicherungen, die für eine Datenquelle mit einer Richtlinie für wöchentliche Sicherung erstellt werden, werden am nächsten Tag gelöscht. Sie werden auch dann gelöscht, wenn sie die neuesten Sicherungen für die Datenquelle sind. <br/><br/> **Ausnahme**: Wenn eine wöchentlich geplante Sicherung für die Langzeitaufbewahrung (Wochen, Monate, Jahre) fehlschlägt, wird eine unmittelbar nach der fehlgeschlagenen geplanten Sicherung eine ausgelöste bedarfsgesteuerte Sicherung für die Langzeitaufbewahrung berücksichtigt. Andernfalls wird die nächste geplante Sicherung für die Langzeitaufbewahrung berücksichtigt. <br/><br/> **Beispielszenario**: Die geplante Sicherung am Donnerstag um 8:00 Uhr ist fehlgeschlagen. Diese Sicherung sollte für die monatliche oder jährliche Aufbewahrung berücksichtigt werden. Also wird die erste bedarfsgesteuerte Sicherung, die vor der nächsten geplanten Sicherung am Donnerstag um 8:00 Uhr ausgelöst wird, automatisch für die monatliche oder jährliche Aufbewahrung gekennzeichnet. Diese Sicherung ersetzt die Sicherung am Donnerstag um 8:00 Uhr.

Weitere Informationen finden Sie unter [Erstellen einer Sicherungsrichtlinie](#create-a-backup-policy).

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Wiederherstellen von Dateien in Azure](backup-azure-restore-windows-server.md).
* Nutzen Sie [Häufig gestellte Fragen zum Sichern von Dateien und Ordnern](backup-azure-file-folder-backup-faq.md).
