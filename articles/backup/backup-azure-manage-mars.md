---
title: Verwalten und Überwachen von MARS-Agent-Sicherungen
description: Erfahren Sie etwas über das Verwalten und Überwachen von MARS-Agent-Sicherungen (Microsoft Azure Recovery Services) mit dem Azure Backup-Dienst.
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 10/07/2019
ms.openlocfilehash: f299bdeebab4f42721255d462101f0065a640fab
ms.sourcegitcommit: 57eb9acf6507d746289efa317a1a5210bd32ca2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2019
ms.locfileid: "74665592"
---
# <a name="manage-microsoft-azure-recovery-services-mars-agent-backups-by-using-the-azure-backup-service"></a>Verwalten von MARS-Agent-Sicherungen (Microsoft Azure Recovery Services) mit dem Azure Backup-Dienst

In diesem Artikel wird beschrieben, wie Sie Dateien und Ordner verwalten, die mit Microsoft Azure Recovery Services Agent gesichert wurden.

## <a name="create-a-backup-policy"></a>Erstellen einer Sicherungsrichtlinie

Die Sicherungsrichtlinie legt fest, wann Momentaufnahmen der Daten zur Erstellung von Wiederherstellungspunkten erstellt werden sollen und wie lange die Wiederherstellungspunkte aufbewahrt werden. Sie konfigurieren die Sicherungsrichtlinie mit dem MARS-Agent.

Erstellen Sie eine Richtlinie wie folgt:

1. Nachdem Sie den MARS-Agent heruntergeladen und registriert haben, starten Sie die Agent-Konsole. Den Agent finden Sie, indem Sie auf Ihrem Computer nach **Microsoft Azure Backup**suchen.  
2. Klicken Sie unter **Aktionen** auf **Sicherung planen**.

    ![Planen einer Windows Server-Sicherung](./media/backup-configure-vault/schedule-first-backup.png)
3. Klicken Sie im Assistenten zum Planen der Sicherung auf **Erste Schritte** und dann auf **Weiter**.
4. Klicken Sie unter **Elemente für Sicherung auswählen** auf **Elemente hinzufügen**.

    ![Elemente für die Sicherung auswählen](./media/backup-azure-manage-mars/select-item-to-backup.png)

5. Wählen Sie unter **Elemente auswählen**, was Sie sichern möchten, und klicken Sie auf **OK**.

    ![Ausgewählte zu sichernde Elemente](./media/backup-azure-manage-mars/selected-items-to-backup.png)

6. Klicken Sie auf der Seite **Elemente für Sicherung auswählen** auf **Weiter**.
7. Geben Sie auf der Seite **Sicherungszeitplan angeben** an, wann Sie tägliche oder wöchentliche Sicherungen erstellen möchten. Klicken Sie auf **Weiter**.

    - Ein Wiederherstellungspunkt wird gleichzeitig mit einer Sicherung erstellt.
    - Die Anzahl der in Ihrer Umgebung erstellten Wiederherstellungspunkte hängt von Ihrem Sicherungszeitplan ab.

8. Es können bis zu drei tägliche Sicherungen geplant werden. Der Screenshot zeigt beispielsweise zwei tägliche Sicherungen, eine um Mitternacht und eine um 18 Uhr.

    ![Täglicher Zeitplan](./media/backup-configure-vault/day-schedule.png)

9. Sie können auch wöchentliche Sicherungen ausführen. Der Screenshot zeigt beispielsweise Sicherungen, die jeden zweiten Sonntag und Mittwoch um 9:30 Uhr und 1:00 Uhr erstellt werden.

    ![Wöchentlicher Zeitplan](./media/backup-configure-vault/week-schedule.png)

10. Geben Sie auf der Seite **Aufbewahrungsrichtlinie auswählen** an, wie Sie historische Kopien Ihrer Daten speichern. Klicken Sie auf **Weiter**.

    - Die Aufbewahrungseinstellungen legen fest, welche Wiederherstellungspunkte gespeichert werden sollen und wie lange sie gespeichert werden.
    - Wenn Sie beispielsweise Einstellungen für die tägliche Aufbewahrung festlegen, geben Sie an, dass zu dem für die tägliche Aufbewahrung angegebenen Zeitpunkt der neueste Wiederherstellungspunkt für die angegebene Anzahl von Tagen beibehalten wird. Oder Sie können als weiteres Beispiel eine monatliche Aufbewahrungsrichtlinie angeben, die angibt, dass der am 30. eines jeden Monats erstellte Wiederherstellungspunkt für 12 Monate gespeichert werden soll.
    - Die tägliche und wöchentliche Aufbewahrung der Wiederherstellungspunkte stimmt in der Regel mit dem Sicherungszeitplan überein. Das bedeutet, dass bei planmäßiger Auslösung der Sicherung der von der Sicherung erstellte Wiederherstellungspunkt für die in der täglichen oder wöchentlichen Aufbewahrungsrichtlinie angegebene Dauer gespeichert wird.
    - Dies ist beispielsweise im folgenden Screenshot dargestellt: – Tägliche Sicherungen um Mitternacht und um 18:00 Uhr werden sieben Tage lang aufbewahrt.
            – Sicherungen, die an einem Samstag um Mitternacht und 18 Uhr erstellt werden, werden vier Wochen lang aufbewahrt.
            – Sicherungen, die am Samstag in der letzten Woche des Monats um Mitternacht und 18 Uhr erstellt werden, werden 12 Monate lang aufbewahrt.
            - Sicherungen, die an einem Samstag in der letzten Märzwoche erstellt wurden, werden zehn Jahre lang aufbewahrt.

    ![Beispiel für die Aufbewahrung](./media/backup-configure-vault/retention-example.png)

11. Wählen Sie in **Erstsicherungstyp auswählen**, ob Sie die Erstsicherung über das Netzwerk ausführen oder eine Offlinesicherung verwenden möchten (weitere Informationen zur Offlinesicherung finden Sie in diesem [Artikel](backup-azure-backup-import-export.md)). Um die Erstsicherung über das Netzwerk auszuführen, wählen Sie **Automatisch über das Netzwerk** aus, und klicken Sie auf **Weiter**.

    ![Erstsicherungstyp](./media/backup-azure-manage-mars/choose-initial-backup-type.png)

12. Lesen Sie sich die Informationen unter **Bestätigung** durch, und klicken Sie dann auf **Fertig stellen**.
    ![Bestätigen des Sicherungstyps](./media/backup-azure-manage-mars/confirm-backup-type.png)

13. Klicken Sie auf **Schließen**, nachdem der Assistent die Erstellung des Sicherungszeitplans abgeschlossen hat.
  ![Bestätigen des Änderns des Sicherungsvorgangs](./media/backup-azure-manage-mars/confirm-modify-backup-process.png)

Sie müssen auf jedem Computer, auf dem der Agent installiert ist, eine Richtlinie erstellen.

## <a name="modify-a-backup-policy"></a>Ändern einer Sicherungsrichtlinie

Wenn Sie die Sicherungsrichtlinie ändern, können Sie neue Elemente hinzufügen, vorhandene Elemente aus der Sicherung entfernen oder mithilfe von Ausschlusseinstellungen Dateien aus der Sicherung ausschließen.

- **Elemente hinzufügen**: Verwenden Sie diese Option nur, um neue zu sichernde Elemente hinzuzufügen. Um vorhandene Elemente zu entfernen, verwenden Sie die Option **Elemente entfernen** oder **Ausschlusseinstellungen**.  
- **Elemente entfernen**: Verwenden Sie diese Option, um Elemente aus der Sicherung auszuschließen.
  - Verwenden Sie **Ausschlusseinstellungen** statt **Elemente entfernen**, um alle Elemente in einem Volume zu entfernen.
  - Das Deaktivieren der gesamten Auswahloptionen in einem Volume bewirkt, dass alte Sicherungen der Elemente gemäß den Aufbewahrungseinstellungen zum Zeitpunkt der letzten Sicherung aufbewahrt werden, ohne Änderungen vornehmen zu können.
  - Wenn Sie diese Optionen erneut aktivieren, führt dies zu einer ersten vollständigen Sicherung, und neue Richtlinienänderungen werden nicht auf alte Sicherungen angewendet.
  - Durch das Aufheben der Auswahl des gesamten Volumes wird die letzte Sicherung aufbewahrt, ohne dass an der Aufbewahrungsrichtlinie Änderungen vorgenommen werden können.
- **Ausschlusseinstellungen**: Verwenden Sie diese Option, um bestimmte Elemente aus der Sicherung auszuschließen.
  
### <a name="add-new-items-to-existing-policy"></a>Hinzufügen neuer Elemente zu einer vorhandenen Richtlinie

1. Klicken Sie unter **Aktionen** auf **Sicherung planen**.

    ![Planen einer Windows Server-Sicherung](./media/backup-configure-vault/schedule-first-backup.png)

2. Wählen Sie auf der Registerkarte **Wählen Sie das Richtlinienelement aus** die Option **Ändern Sie einen Sicherungszeitplan für Ihre Dateien und Ordner** aus, und klicken Sie dann auf **Weiter**.

    ![Hinzufügen von Richtlinienelementen](./media/backup-azure-manage-mars/select-policy-items.png)

3. Wählen Sie auf der Registerkarte **Geplante Sicherung ändern oder beenden** die Option **Sicherungselemente oder -zeiten ändern** aus, und klicken Sie auf **Weiter**.

    ![Ändern oder Planen der Sicherung](./media/backup-azure-manage-mars/modify-schedule-backup.png)

4. Klicken Sie auf der Registerkarte **Elemente für die Sicherung auswählen** auf **Elemente hinzufügen**, um die Elemente hinzuzufügen, die Sie sichern möchten.

    ![Ändern oder Planen der Sicherung – Elemente hinzufügen](./media/backup-azure-manage-mars/modify-schedule-backup-add-items.png)

5. Wählen Sie im Fenster **Elemente auswählen** Dateien oder Ordner aus, die Sie hinzufügen möchten, und klicken Sie auf **OK**.

    ![Auswählen der Elemente](./media/backup-azure-manage-mars/select-item.png)

6. Führen Sie die nachfolgenden Schritte aus, und klicken Sie zum Abschließen des Vorgangs auf **Fertig stellen**.

### <a name="add-exclusion-rules-to-existing-policy"></a>Hinzufügen von Ausschlussregeln zu einer vorhandenen Richtlinie

Sie können Ausschlussregeln hinzufügen, um Dateien und Ordner zu überspringen, die nicht gesichert werden sollen. Sie können dies beim Definieren einer neuen Richtlinie oder Ändern einer vorhandenen Richtlinie durchführen.

1. Klicken Sie im Bereich „Aktionen“ auf **Sicherung planen**. Wechseln Sie zu **Elemente für die Sicherung auswählen**, und klicken Sie auf **Ausschlusseinstellungen**.

    ![Auswählen der Elemente](./media/backup-azure-manage-mars/select-exclusion-settings.png)

2. Klicken Sie in **Ausschlusseinstellungen**auf **Ausschluss hinzufügen**.

    ![Auswählen der Elemente](./media/backup-azure-manage-mars/add-exclusion.png)

3. Durchsuchen Sie in **Auszuschließende Elemente auswählen** die Dateien und Ordnern, die Sie ausschließen möchten, wählen Sie Elemente aus, die Sie ausschließen möchten, und klicken Sie auf **OK**.

    ![Auswählen der Elemente](./media/backup-azure-manage-mars/select-items-exclude.png)

4. Standardmäßig werden alle **Unterordner** in den ausgewählten Ordnern ausgeschlossen. Sie können dies ändern, indem Sie **Ja** oder **Nein** auswählen. Sie können die auszuschließenden Dateitypen wie unten dargestellt bearbeiten und angeben:

    ![Auswählen der Elemente](./media/backup-azure-manage-mars/subfolders-type.png)

5. Führen Sie die nachfolgenden Schritte aus, und klicken Sie zum Abschließen des Vorgangs auf **Fertig stellen**.

### <a name="remove-items-from-existing-policy"></a>Entfernen von Elementen aus vorhandener Richtlinie

1. Klicken Sie im Bereich „Aktionen“ auf **Sicherung planen**. Wechseln Sie zu **Elemente für die Sicherung auswählen**. Wählen Sie in der Liste die Dateien und Ordner aus, die Sie aus dem Sicherungszeitplan entfernen möchten, und klicken Sie auf **Elemente entfernen**.

    ![Auswählen der Elemente](./media/backup-azure-manage-mars/select-items-remove.png)

> [!NOTE]
> Gehen Sie mit Bedacht vor, wenn Sie ein Volume vollständig aus der Richtlinie entfernen.  Wenn Sie es erneut hinzufügen müssen, wird es als neues Volume behandelt. Die nächste geplante Sicherung wird als Erstsicherung (vollständige Sicherung) und nicht als inkrementellen Sicherung ausgeführt. Wenn Sie zu einem späteren Zeitpunkt Elemente temporär entfernen und hinzufügen müssen, empfiehlt es sich, **Ausschlusseinstellungen** anstelle von **Elemente entfernen** zu verwenden, damit statt einer vollständigen Sicherung eine inkrementelle Sicherung erfolgt.

2. Führen Sie die nachfolgenden Schritte aus, und klicken Sie zum Abschließen des Vorgangs auf **Fertig stellen**.

## <a name="stop-protecting-files-and-folder-backup"></a>Beenden des Schutzes von Dateien und Ordnern durch Sicherungen

Sie haben zwei Möglichkeiten, den Schutz von Dateien und Ordnern durch Sicherungen zu beenden:

- **Schutz beenden und Sicherungsdaten beibehalten**.
  - Mit dieser Option werden alle zukünftigen Sicherungsaufträge beendet.
  - Der Azure Backup-Dienst behält die Wiederherstellungspunkte bei, die gemäß der Aufbewahrungsrichtlinie gesichert wurden.
  - Sie können die gesicherten Daten für nicht abgelaufene Wiederherstellungspunkte wiederherstellen.
  - Mit der Option *Aktivieren Sie den Sicherungszeitplan erneut* können Sie bei Bedarf den Schutz fortsetzen. Anschließend werden die Daten basierend auf der neuen Aufbewahrungsrichtlinie aufbewahrt.
- **Schutz beenden und Sicherungsdaten löschen**.
  - Mit dieser Option werden alle zukünftigen Sicherungsaufträge zum Schutz Ihrer Daten beendet und alle Wiederherstellungspunkte gelöscht.
  - Sie erhalten eine Benachrichtigungs-E-Mail mit der Nachricht *Your data for this Backup item has been deleted. This data will be temporarily available for 14 days, after which it will be permanently deleted* (Die Daten für dieses Sicherungselement wurden gelöscht. Diese Daten sind 14 Tage lang verfügbar und werden anschließend endgültig gelöscht.) und der empfohlenen Aktion *Reprotect the Backup item within 14 days to recover your data.* (Schützen Sie das Sicherungselement innerhalb von 14 Tagen erneut, um die Daten wiederherzustellen.).
  - Um den Schutz fortzusetzen, stellen Sie ihn innerhalb von 14 Tagen nach dem Löschvorgang wieder her.

### <a name="stop-protection-and-retain-backup-data"></a>Schutz beenden und Sicherungsdaten beibehalten

1. Öffnen Sie die MARS-Verwaltungskonsole, navigieren Sie zum Bereich **Aktionen**, und wählen Sie **Sicherung planen** aus.
    ![Ändern oder Beenden einer geplanten Sicherung](./media/backup-azure-manage-mars/mars-actions.png)
1. Wählen Sie auf der Seite **Wählen Sie das Richtlinienelement aus** die Option **Ändern Sie einen Sicherungszeitplan für Ihre Dateien und Ordner** aus, und klicken Sie auf **Weiter**.
    ![Ändern oder Beenden einer geplanten Sicherung](./media/backup-azure-manage-mars/select-policy-item-retain-data.png)
1. Wählen Sie auf der Seite **Geplante Sicherung ändern oder beenden** die Option **Stop using this backup schedule, but keep the stored backups until a schedule is activated again** (Diesen Sicherungszeitplan nicht länger verwenden, die gespeicherten Sicherungen jedoch beibehalten, bis erneut ein Sicherungszeitplan aktiviert wird) aus. Klicken Sie anschließend auf **Weiter**.  
    ![Ändern oder Beenden einer geplanten Sicherung](./media/backup-azure-manage-mars/stop-schedule-backup.png)
1. Überprüfen Sie die Informationen in **Geplante Sicherung anhalten**, und klicken Sie auf **Fertig stellen**. ![Ändern oder Beenden einer geplanten Sicherung](./media/backup-azure-manage-mars/pause-schedule-backup.png)
1. Überprüfen Sie in **Sicherungsstatus ändern**, ob der Status der geplanten Sicherung „Erfolgreich“ ist, und klicken Sie auf **Schließen**, um den Vorgang abzuschließen.

### <a name="stop-protection-and-delete-backup-data"></a>Schutz beenden und Sicherungsdaten löschen

1. Öffnen Sie die MARS-Verwaltungskonsole, navigieren Sie zum Bereich **Aktionen**, und wählen Sie **Sicherung planen** aus.
2. Wählen Sie auf der Seite **Geplante Sicherung ändern oder beenden** die Option **Sicherungszeitplan nicht länger verwenden und alle gespeicherten Sicherungen löschen** aus. Klicken Sie anschließend auf **Weiter**.

    ![Ändern oder Beenden einer geplanten Sicherung](./media/backup-azure-delete-vault/modify-schedule-backup.png)

3. Wählen Sie auf der Seite **Geplante Sicherung beenden** die Option **Fertig stellen** aus.

    ![Beenden einer geplanten Sicherung](./media/backup-azure-delete-vault/stop-schedule-backup.png)
4. Sie werden aufgefordert, eine Sicherheits-PIN (persönliche Identifikationsnummer) einzugeben, die Sie manuell generieren müssen. Dazu melden Sie sich zunächst beim Azure-Portal an.
5. Navigieren Sie zu **Recovery Services-Tresor** > **Einstellungen** > **Eigenschaften**.
6. Wählen Sie unter **Sicherheits-PIN** die Option **Generieren** aus. Kopieren Sie diese PIN. Die PIN ist nur fünf Minuten lang gültig.
7. Fügen Sie die PIN in der Verwaltungskonsole ein, und klicken Sie anschließend auf **OK**.

    ![Generieren einer Sicherheits-PIN](./media/backup-azure-delete-vault/security-pin.png)

8. Auf der Seite **Sicherungsstatus ändern** wird die folgende Meldung angezeigt: *Gelöschte Sicherungsdaten werden 14 Tage lang aufbewahrt. Danach werden Sicherungsdaten dauerhaft gelöscht.*  

    ![Löschen der Sicherungsinfrastruktur](./media/backup-azure-delete-vault/deleted-backup-data.png)

Nachdem Sie die lokalen Sicherungselemente gelöscht haben, führen Sie die nächsten Schritte im Portal aus.

## <a name="re-enable-protection"></a>Erneutes Aktivieren des Schutzes

Wenn Sie den Schutz beendet haben, die Daten jedoch aufbewahrt wurden, und Sie den Schutz fortsetzen möchten, können Sie durch Ändern der Sicherungsrichtlinie den Sicherungszeitplan erneut aktivieren.

1. Wählen Sie in **Aktionen** die Option **Sicherung planen** aus.
1. Wählen Sie **Aktivieren Sie den Sicherungszeitplan erneut. Sie können Sicherungselemente oder -zeiten auch ändern.** aus, und klicken Sie auf **Weiter**.
    ![Löschen der Sicherungsinfrastruktur](./media/backup-azure-manage-mars/re-enable-policy-next.png)
1. Klicken Sie in **Elemente für Sicherung auswählen** auf **Weiter**.
    ![Löschen der Sicherungsinfrastruktur](./media/backup-azure-manage-mars/re-enable-next.png)
1. Geben Sie in **Sicherungszeitplan angeben** den Sicherungszeitplan an, und klicken Sie auf **Weiter**.
1. Geben Sie in **Aufbewahrungsrichtlinie auswählen** die Aufbewahrungsdauer an, und klicken Sie auf **Weiter**.
1. Überprüfen Sie abschließend im Bildschirm **Bestätigung** die Richtliniendetails, und klicken Sie auf **Fertig stellen**.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu unterstützten Szenarien und Einschränkungen finden Sie in der [Supportmatrix für MARS](https://docs.microsoft.com/azure/backup/backup-support-matrix-mars-agent).
- Erfahren Sie mehr über die [Aufbewahrung bei Richtlinien für bedarfsgesteuerte Sicherungen](backup-configure-vault.md#on-demand-backup-policy-retention-behavior).
