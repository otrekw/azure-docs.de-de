---
title: Verwalten der Sicherungen von Azure-Dateifreigaben
description: In diesem Artikel werden allgemeine Aufgaben zur Verwaltung und Überwachung der Azure-Dateifreigaben beschrieben, die durch den Azure Backup-Dienst gesichert werden.
ms.topic: conceptual
ms.date: 01/07/2020
ms.openlocfilehash: a5477d021b6e3600693e183d8707e11592b7cc38
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294892"
---
# <a name="manage-azure-file-share-backups"></a>Verwalten der Sicherungen von Azure-Dateifreigaben

In diesem Artikel werden allgemeine Aufgaben zur Verwaltung und Überwachung der Azure-Dateifreigaben beschrieben, die durch den [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview)-Dienst gesichert werden. Sie lernen, wie Sie die folgenden Verwaltungsaufgaben im Recovery Services-Tresor ausführen können:

* [Überwachen von Aufträgen](#monitor-jobs)
* [Erstellen einer neuen Richtlinie](#create-a-new-policy)
* [Ändern der Richtlinie](#modify-policy)
* [Beenden des Schutzes für eine Dateifreigabe](#stop-protection-on-a-file-share)
* [Fortsetzen des Schutzes für eine Dateifreigabe](#resume-protection-on-a-file-share)
* [Löschen von Sicherungsdaten](#delete-backup-data)
* [Aufheben der Registrierung eines Speicherkontos](#unregister-storage-account)

## <a name="monitor-jobs"></a>Überwachen von Aufträgen

Wenn Sie einen Sicherungs- oder Wiederherstellungsvorgang auslösen, erstellt der Sicherungsdienst einen Auftrag zum Nachverfolgen. Sie können den Status aller Aufträge auf der Seite **Sicherungsaufträgen** überwachen.

So öffnen Sie die Seite **Sicherungsaufträge**

1. Öffnen Sie den Recovery Services-Tresor, den Sie zur Konfiguration der Sicherung Ihrer Dateifreigaben verwendet haben. Klicken Sie im Blatt **Übersicht** im Abschnitt **Überwachung** auf **Sicherungsaufträge**.

   ![Sicherungsaufträge im Abschnitt „Überwachung“](./media/manage-afs-backup/backup-jobs.png)

2. Sobald Sie auf „OK“ geklickt haben, wird das Blatt **Sicherungsaufträge** mit dem Status aller Aufträge angezeigt. Sie können auf den Workloadnamen klicken, der der Dateifreigabe entspricht, die Sie überwachen möchten.

   ![Workloadname](./media/manage-afs-backup/workload-name.png)

## <a name="create-a-new-policy"></a>Erstellen einer neuen Richtlinie

Sie können im Recovery Services-Tresor im Abschnitt **Sicherungsrichtlinien** eine neue Richtlinie zum Sichern von Azure-Dateifreigaben erstellen. Alle beim Konfigurieren der Sicherung für Dateifreigaben erstellten Richtlinien werden mit dem Richtlinientyp „Azure-Dateifreigabe“ angezeigt.

So zeigen Sie die vorhandenen Sicherungsrichtlinien an

1. Öffnen Sie den Recovery Services-Tresor, den Sie zur Konfiguration der Sicherung für die Dateifreigabe verwendet haben, und klicken Sie im Menü des Recovery Services Tresors im Abschnitt „Verwalten“ auf **Sicherungsrichtlinien**. Alle im Tresor konfigurierten Sicherungsrichtlinien werden aufgelistet.

   ![Alle Sicherungsrichtlinien](./media/manage-afs-backup/all-backup-policies.png)

2. Um spezifische Richtlinien für Azure-Dateifreigaben anzuzeigen, wählen Sie **Azure-Dateifreigabe** aus der Dropdownliste oben rechts aus.

   ![Azure-Dateifreigabe auswählen](./media/manage-afs-backup/azure-file-share.png)

So erstellen Sie eine neue Sicherungsrichtlinie

1. Klicken Sie auf dem Blatt „Sicherungsrichtlinien“ auf **+ Hinzufügen**.

   ![Neue Sicherungsrichtlinie](./media/manage-afs-backup/new-backup-policy.png)

2. Wählen Sie auf dem Blatt **Hinzufügen** die Option **Azure-Dateifreigabe** als **Richtlinientyp** aus. Das Blatt „Sicherungsrichtlinie“ für Azure-Dateifreigaben wird geöffnet. Geben Sie den Richtliniennamen, die Sicherungshäufigkeit und die Aufbewahrungsdauer für die Wiederherstellungspunkte an. Klicken Sie auf **OK**, wenn Sie die Richtlinie definiert haben.

   ![Sicherungsrichtlinie definieren](./media/manage-afs-backup/define-backup-policy.png)

## <a name="modify-policy"></a>Richtlinie ändern

Sie können eine Sicherungsrichtlinie bearbeiten, um die Sicherungshäufigkeit oder die Aufbewahrungsdauer zu ändern.

So ändern Sie eine Richtlinie

1. Öffnen Sie den Recovery Services-Tresor, den Sie zur Konfiguration der Sicherung für die Dateifreigabe verwendet haben, und klicken Sie im Menü des Recovery Services Tresors im Abschnitt „Verwalten“ auf **Sicherungsrichtlinien**. Alle im Tresor konfigurierten Sicherungsrichtlinien werden aufgelistet.

   ![Alle Sicherungsrichtlinien im Tresor](./media/manage-afs-backup/all-backup-policies-modify.png)

2. Um spezifische Richtlinien für eine Azure-Dateifreigabe anzuzeigen, wählen Sie **Azure-Dateifreigabe** aus der Dropdownliste oben rechts aus. Klicken Sie auf die Sicherungsrichtlinie, die Sie ändern möchten.

   ![Azure-Dateifreigabe, die Sie ändern möchten](./media/manage-afs-backup/azure-file-share-modify.png)

3. Das Blatt **Zeitplan** wird geöffnet. Bearbeiten Sie den Sicherungszeitplan/Aufbewahrungszeitraum wie gewünscht, und klicken Sie auf **Speichern**. Auf dem Blatt wird eine Meldung „Update wird ausgeführt“ angezeigt, und wenn die Richtlinienänderungen erfolgreich aktualisiert wurden, wird die Meldung „Die Sicherungsrichtlinie wurde erfolgreich aktualisiert“ angezeigt.

   ![Geänderte Richtlinie speichern](./media/manage-afs-backup/save-policy.png)

## <a name="stop-protection-on-a-file-share"></a>Beenden des Schutzes für eine Dateifreigabe

Der Schutz für Azure-Dateifreigaben kann auf zwei Arten beendet werden:

* Beenden aller zukünftigen Sicherungsaufträge und *Löschen aller Wiederherstellungspunkte*
* Beenden aller zukünftigen Sicherungsaufträge und *Beibehalten der Wiederherstellungspunkte*

Unter Umständen fallen für die Aufbewahrung der Wiederherstellungspunkte im Speicher Gebühren an, da die von Azure Backup erstellten zugrundeliegenden Momentaufnahmen beibehalten werden. Der Vorteil der Beibehaltung von Wiederherstellungspunkten ist, dass Sie die Dateifreigabe bei Bedarf später wiederherstellen können. Informationen zu den Kosten, die durch die Beibehaltung der Wiederherstellungspunkte entstehen, finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/backup/). Wenn Sie alle Wiederherstellungspunkte löschen, kann die Dateifreigabe nicht mehr wiederhergestellt werden.

So beenden Sie den Schutz für eine Azure-Dateifreigabe

1. Öffnen Sie den Recovery Services-Tresor, der die Wiederherstellungspunkte der Dateifreigaben enthält, und klicken Sie im Anschnitt „Geschützte Elemente“ auf **Sicherungselemente**. Die Liste der Sicherungselementtypen wird angezeigt.

   ![Sicherungselemente](./media/manage-afs-backup/backup-items.png)

2. Wählen Sie in der Liste **Backup Management Type** (Sicherungsverwaltungstyp) den Eintrag **Azure Storage (Azure Files)** aus. Die Liste der **Sicherungselemente für Azure Storage (Azure Files)** wird angezeigt.

   ![Azure Storage (Azure Files) auswählen](./media/manage-afs-backup/azure-storage-azure-files.png)

3. Wählen Sie in der Liste der **Sicherungselemente (Azure Storage (Azure Files))** das Sicherungselement aus, für das der Schutz beendet werden soll.

4. Wählen Sie aus dem Menü des Blatts **Sicherungselement** die Option **Sicherung beenden** aus.

   ![Auswählen von „Sicherung beenden“](./media/manage-afs-backup/stop-backup.png)

5. Wählen Sie auf dem Blatt **Sicherung beenden** die Option **Sicherungsdaten beibehalten** oder **Sicherungsdaten löschen**, und klicken Sie auf **Sicherung beenden**.

    ![„Sicherungsdaten beibehalten“ oder „Sicherungsdaten löschen“ auswählen](./media/manage-afs-backup/retain-or-delete-backup-data.png)

## <a name="resume-protection-on-a-file-share"></a>Fortsetzen des Schutzes für eine Dateifreigabe

Wenn beim Beenden des Schutzes für die Dateifreigabe die Option **Sicherungsdaten beibehalten** ausgewählt wurde, kann der Schutz fortgesetzt werden. Wenn die Option **Sicherungsdaten löschen** ausgewählt wurde, kann der Schutz für die Dateifreigabe nicht fortgesetzt werden.

So setzen Sie den Schutz für eine Azure-Dateifreigabe fort

1. Öffnen Sie den Recovery Services-Tresor, der die Wiederherstellungspunkte der Dateifreigaben enthält, und klicken Sie im Anschnitt „Geschützte Elemente“ auf **Sicherungselemente**. Die Liste der Sicherungselementtypen wird angezeigt.

   ![Sicherungselemente für die Fortsetzung](./media/manage-afs-backup/backup-items-resume.png)

2. Wählen Sie in der Liste **Backup Management Type** (Sicherungsverwaltungstyp) den Eintrag **Azure Storage (Azure Files)** aus. Die Liste der **Sicherungselemente für Azure Storage (Azure Files)** wird angezeigt.

   ![Liste von Azure Storage (Azure Files)](./media/manage-afs-backup/azure-storage-azure-files.png)

3. Wählen Sie in der Liste der **Sicherungselemente (Azure Storage (Azure Files))** das Sicherungselement aus, für das der Schutz fortgesetzt werden soll.

4. Wählen Sie aus dem Menü des Blatts **Sicherungselement** die Option **Sicherung fortsetzen** aus.

   ![„Sicherung fortsetzen“ auswählen](./media/manage-afs-backup/resume-backup.png)

5. Das Blatt **Sicherungsrichtlinie** wird geöffnet, und Sie können eine Richtlinie Ihrer Wahl zum Fortsetzen der Sicherung auswählen.

6. Klicken Sie auf **Speichern**, nachdem Sie die gewünschte **Sicherungsrichtlinie** ausgewählt haben. Im Portal wird eine Meldung „Update wird ausgeführt“ angezeigt. Sobald das Backup erfolgreich fortgesetzt wird, wird die Meldung „Die Sicherungsrichtlinie wurde für die geschützte Azure-Dateifreigabe erfolgreich aktualisiert“.

   ![Die Sicherungsrichtlinie wurde erfolgreich aktualisiert](./media/manage-afs-backup/successfully-updated.png)

## <a name="delete-backup-data"></a>Löschen von Sicherungsdaten

Sie können die Sicherung einer Dateifreigabe während des Auftrags zum **Beenden der Sicherung** oder jederzeit nach Beenden des Schutzes löschen. Es kann gegebenenfalls sogar von Vorteil sein, mit dem Löschen der Wiederherstellungspunkte mehrere Tage oder Wochen zu warten. Sie können beim Löschen von Sicherungsdaten keine bestimmten Wiederherstellungspunkte auswählen, die gelöscht werden sollen. Wenn Sie sich für die Löschung Ihrer Sicherungsdaten entscheiden, werden alle der Dateifreigabe zugeordneten Wiederherstellungspunkte gelöscht.

Bei der folgenden Vorgehensweise wird davon ausgegangen, dass der Schutz für die Dateifreigabe beendet wurde.

So löschen Sie Sicherungsdaten für die Azure-Dateifreigabe

1. Nach dem Beenden des Sicherungsauftrags sind die Optionen **Sicherung fortsetzen** und **Sicherungsdaten löschen** auf dem Dashboard **Sicherungselement** verfügbar. Klicken Sie auf die Option **Sicherungsdaten löschen** im Menü des Blatts **Sicherungselement**.

   ![Löschen von Sicherungsdaten](./media/manage-afs-backup/delete-backup-data.png)

2. Das Blatt **Sicherungsdaten löschen** wird geöffnet. Geben Sie den Namen der Dateifreigabe ein, um den Löschvorgang zu bestätigen. Geben Sie optional einen **Grund** oder einen **Kommentar** für den Löschvorgang an. Klicken Sie auf **Löschen**, wenn Sie sicher sind, dass die Sicherungsdaten gelöscht werden sollen.

   ![Datenlöschung bestätigen](./media/manage-afs-backup/confirm-delete-data.png)

## <a name="unregister-storage-account"></a>Aufheben der Registrierung eines Speicherkontos

Wenn Sie Ihre Dateifreigaben in einem bestimmten Speicherkonto unter Verwendung eines anderen Recovery Services-Tresors schützen möchten, [beenden Sie zunächst den Schutz für alle Dateifreigaben](#stop-protection-on-a-file-share) in diesem Speicherkonto. Heben Sie dann die Registrierung des Kontos im aktuellen zum Schutz verwendeten Recovery Services-Tresor auf.

Bei der folgenden Vorgehensweise wird davon ausgegangen, dass der Schutz für alle Dateifreigaben in dem Speicherkonto, dessen Registrierung Sie aufheben möchten, beendet wurde.

So heben Sie die Registrierung eines Speicherkontos auf

1. Öffnen Sie den Recovery Services-Tresor, bei dem Ihr Speicherkonto registriert ist.
2. Klicken Sie im Abschnitt **Verwalten** des Blatts **Übersicht** auf die Option **Sicherungsinfrastruktur**.

   ![Auf „Sicherungsinfrastruktur“ klicken](./media/manage-afs-backup/backup-infrastructure.png)

3. Das Blatt **Sicherungsinfrastruktur** wird geöffnet. Klicken Sie auf diesem Blatt im Abschnitt **Azure Storage-Konten** auf **Speicherkonten**.

   ![Auf „Speicherkonten“ klicken](./media/manage-afs-backup/storage-accounts.png)

4. Nachdem Sie auf **Speicherkonten** geklickt haben, wird eine Liste der beim Tresor registrierten Speicherkonten angezeigt.
5. Klicken Sie mit der rechten Maustaste auf das Speicherkonto, dessen Registrierung Sie aufheben möchten, und wählen Sie **Registrierung aufheben** aus.

   ![Auswählen der Aufhebung der Registrierung](./media/manage-afs-backup/select-unregister.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Behandeln von Problemen beim Sichern von Azure-Dateifreigaben](https://docs.microsoft.com/azure/backup/troubleshoot-azure-files).
