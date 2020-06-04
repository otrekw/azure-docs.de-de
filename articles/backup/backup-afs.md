---
title: Sichern von Azure-Dateifreigaben im Azure-Portal
description: Erfahren Sie, wie Sie das Azure-Portal zum Sichern von Azure-Dateifreigaben im Recovery Services-Tresor verwenden.
ms.topic: conceptual
ms.date: 01/20/2020
ms.openlocfilehash: a77f7fd0ec21eae60a7313a9ffa889fbef4372c6
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82978011"
---
# <a name="back-up-azure-file-shares-in-a-recovery-services-vault"></a>Sichern von Azure-Dateifreigaben in einem Recovery Services-Tresor

In diesem Artikel wird beschrieben, wie Sie mithilfe des Azure-Portals [Azure Dateifreigaben](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) sichern.

In diesem Artikel lernen Sie Folgendes:

* Erstellen Sie einen Recovery Services-Tresor.
* Ermitteln der Dateifreigaben und Konfigurieren der Sicherungen.
* Ausführen eines bedarfsgesteuerten Sicherungsauftrags zum Erstellen eines Wiederherstellungspunkts.

## <a name="prerequisites"></a>Voraussetzungen

* Bestimmen oder erstellen Sie einen [Recovery Services-Tresor](#create-a-recovery-services-vault) in derselben Region wie das Speicherkonto, das die Dateifreigabe hostet.
* Stellen Sie sicher, dass sich die Dateifreigabe in einem der [unterstützten Speicherkontotypen](azure-file-share-support-matrix.md) befindet.

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="modify-storage-replication"></a>Ändern der Speicherreplikation

Tresore verwenden standardmäßig den [georedundanten Speicher (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs).

* Wenn der Tresor Ihr Hauptmechanismus für Sicherungen ist, empfehlen wir die Verwendung von GRS.
* Sie können [Lokal redundanter Speicher (LRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) als kostengünstigere Option verwenden.

So ändern Sie den Speicherreplikationstyp

1. Wählen Sie im neuen Tresor im Abschnitt **Einstellungen** auf **Eigenschaften**.

1. Wählen Sie auf dem Blatt **Eigenschaften** unter **Sicherungskonfiguration** die Option **Aktualisieren** aus.

1. Wählen Sie den Speicherreplikationstyp und dann **Speichern** aus.

    ![Aktualisieren der Sicherungskonfiguration](./media/backup-afs/backup-configuration.png)

> [!NOTE]
> Sie können den Speichertyp für die Replikation nicht mehr ändern, nachdem der Tresor eingerichtet wurde und Sicherungselemente enthält. Dazu müssen Sie den Tresor neu erstellen.
>

## <a name="discover-file-shares-and-configure-backup"></a>Ermitteln der Dateifreigaben und Konfigurieren der Sicherung

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com/) den Recovery Services-Tresor, den Sie zur Konfiguration der Sicherung für die Dateifreigabe verwenden möchten.

1. Wählen Sie im Bereich **Recovery Services-Tresor** im Menü oben die Option **+Sicherung** aus.

   ![Recovery Services-Tresor](./media/backup-afs/recovery-services-vault.png)

    1. Legen Sie im Bereich **Sicherungsziel** die Option **Wo wird Ihre Workload ausgeführt?** auf **Azure** fest, indem Sie in der Dropdownliste **Azure** auswählen.

          ![Azure als Workload wählen](./media/backup-afs/backup-goal.png)

    2. Wählen Sie unter **Was möchten Sie sichern?** aus der Dropdownliste die Option **Azure-Dateifreigabe** aus.

          ![Azure-Dateifreigabe auswählen](./media/backup-afs/select-azure-file-share.png)

    3. Wählen Sie **Sicherung** aus, um die Azure-Dateifreigabeerweiterung im Tresor zu registrieren.

          ![Wählen Sie „Sicherung“ aus, um die Azure-Dateifreigabe mit dem Tresor zu verknüpfen.](./media/backup-afs/register-extension.png)

1. Nachdem Sie **Sicherung** ausgewählt haben, wird der Bereich **Sicherung** geöffnet. Um das Speicherkonto auszuwählen, das die zu schützende Dateifreigabe hostet, klicken Sie unter dem Textfeld **Speicherkonto** auf den Linktext **Auswählen**.

   ![Klicken auf den Link „Auswählen“](./media/backup-afs/choose-select-link.png)

1. Der Bereich **Speicherkonto auswählen**  wird auf der rechten Seite geöffnet, in dem eine Reihe erkannter unterstützter Speicherkonten aufgeführt ist. Sie sind entweder diesem Tresor zugeordnet oder befinden sich in derselben Region wie der Tresor, sind aber noch keinem Recovery Services-Tresor zugeordnet.

1. Wählen Sie aus der Liste der ermittelten Speicherkonten ein Speicherkonto und dann **OK** aus.

   ![In den erkannten Speicherkonten eine Wahl treffen](./media/backup-afs/select-discovered-storage-account.png)

1. Der nächste Schritt ist das Auswählen der zu sichernden Dateifreigaben. Klicken Sie im Abschnitt **Dateifreigaben für die Sicherung** auf die Schaltfläche **Hinzufügen**.

   ![Auswählen der zu sichernden Dateifreigaben](./media/backup-afs/select-file-shares-to-back-up.png)

1. Der Kontextbereich **Dateifreigaben auswählen** wird auf der rechten Seite geöffnet. Azure durchsucht das Speicherkonto nach Dateifreigaben, die gesichert werden können. Falls Sie die Dateifreigaben erst kürzlich hinzugefügt haben und diese nicht in der Liste angezeigt werden, warten Sie einen Moment, bis die Dateifreigaben angezeigt werden.

1. Wählen Sie in der Liste **Dateifreigaben auswählen** mindestens eine zu sichernde Dateifreigabe aus. Klicken Sie auf **OK**.

   ![Auswählen der Dateifreigaben](./media/backup-afs/select-file-shares.png)

1. Für die Auswahl einer Sicherungsrichtlinie für Ihre Dateifreigabe gibt es drei Optionen:

   * Die Standardrichtlinie wählen.<br>
   Diese Option ermöglicht Ihnen, eine tägliche Sicherung mit einer Aufbewahrung von 30 Tagen zu aktivieren. Wenn keine Sicherungsrichtlinie im Tresor vorhanden ist, wird der Sicherungsbereich mit den Standardrichtlinieneinstellungen geöffnet. Wenn Sie die Standardeinstellungen wählen möchten, können Sie direkt auf  **Sicherung aktivieren** klicken.

   * Erstellen einer neuen Richtlinie <br>

      1. Um eine neue Sicherungsrichtlinie für Ihre Dateifreigabe zu erstellen, klicken Sie im Abschnitt **Sicherungsrichtlinie** auf den Linktext unter der Dropdownliste.<br>

         ![Eine neue Richtlinie erstellen.](./media/backup-afs/create-new-policy.png)

      1. Der Kontextbereich **Sicherungsrichtlinie** wird auf der rechten Seite geöffnet. Geben Sie einen Richtliniennamen in das Textfeld ein, und wählen Sie den Aufbewahrungszeitraum entsprechend Ihren Anforderungen. Nur die Aufbewahrungsoption „Täglich“ ist standardmäßig aktiviert. Wenn Sie eine wöchentliche, monatliche oder jährliche Aufbewahrung wünschen, aktivieren Sie das entsprechende Kontrollkästchen, und geben Sie die gewünschte Aufbewahrungsdauer an.

      1. Klicken Sie auf „OK“, nachdem Sie die Aufbewahrungsdauer und einen gültigen Richtliniennamen angegeben haben.<br>

         ![Richtliniennamen und Aufbewahrungsdauer angeben](./media/backup-afs/policy-name.png)

   * Eine der vorhandenen Sicherungsrichtlinien wählen <br>

   Um eine der vorhandenen Sicherungsrichtlinien für die Konfiguration des Schutzes zu wählen, wählen Sie die gewünschte Richtlinie in der Dropdownliste **Sicherungsrichtlinie** aus.<br>

   ![Vorhandene Richtlinie wählen](./media/backup-afs/choose-existing-policy.png)

1. Klicken Sie auf **Sicherung aktivieren**, um den Schutz der Dateifreigabe zu aktivieren.

   ![„Sicherung aktivieren“ wählen](./media/backup-afs/enable-backup.png)

Nachdem Sie eine Sicherungsrichtlinie festgelegt haben, wird zum geplanten Zeitpunkt eine Momentaufnahme der Dateifreigaben erstellt. Der Wiederherstellungspunkt wird ebenfalls für den gewählten Zeitraum beibehalten.

>[!NOTE]
>Azure Backup unterstützt jetzt Richtlinien mit täglicher/wöchentlicher/monatlicher oder jährlicher Aufbewahrung für die Sicherung von Azure-Dateifreigaben.

## <a name="create-an-on-demand-backup"></a>Erstellen einer bedarfsgesteuerten Sicherung

Gelegentlich empfiehlt es sich, eine Sicherungsmomentaufnahme oder einen Wiederherstellungspunkt außerhalb der geplanten Zeiten in der Sicherungsrichtlinie zu erstellen. Häufig ist es sinnvoll, eine bedarfsgesteuerte Sicherung direkt nach dem Konfigurieren der Sicherungsrichtlinie zu erstellen. Basierend auf den Zeitplan in der Sicherungsrichtlinie, kann es Stunden oder Tage dauern, bis eine Momentaufnahme erstellt wird. Initiieren Sie eine bedarfsgesteuerte Sicherung, um Ihre Daten zu schützen, bevor die Sicherungsrichtlinie in Kraft tritt. Die Erstellung einer bedarfsgesteuerten Sicherung ist häufig erforderlich, bevor Sie geplante Änderungen an den Dateifreigaben vornehmen.

### <a name="create-a-backup-job-on-demand"></a>Erstellen eines bedarfsgesteuerten Sicherungsauftrags

1. Öffnen Sie den Recovery Services-Tresor, den Sie zum Sichern Ihrer Dateifreigaben verwendet haben. Wahlen Sie auf dem Blatt **Übersicht** im Abschnitt **Geschützte Elemente** die Option **Sicherungselemente** aus.

   ![Sicherungselemente auswählen](./media/backup-afs/backup-items.png)

1. Nachdem Sie **Sicherungselemente** ausgewählt haben, wird neben dem Bereich **Übersicht** ein neuer Bereich angezeigt, in dem alle **Sicherungsverwaltungstypen** aufgelistet sind.

   ![Liste mit Sicherungsverwaltungstypen](./media/backup-afs/backup-management-types.png)

1. Wählen Sie aus der Liste der **Sicherungsverwaltungstypen** den Eintrag **Azure Storage (Azure Files)** aus. Angezeigt wird eine Liste aller Dateifreigaben und der entsprechenden Speicherkonten, die mit diesem Tresor gesichert werden.

   ![Azure Storage (Azure Files)-Sicherungselemente](./media/backup-afs/azure-files-backup-items.png)

1. Wählen Sie in der Liste der Azure-Dateifreigaben die gewünschte Dateifreigabe aus. Einzelheiten zum **Sicherungselement** werden angezeigt. Wählen Sie im Menü **Sicherungselement** die Option **Jetzt sichern** aus. Da es sich um einen bedarfsgesteuerten Sicherungsauftrag handelt, ist dem Wiederherstellungspunkt keine Aufbewahrungsrichtlinie zugeordnet.

   ![„Jetzt sichern“ auswählen](./media/backup-afs/backup-now.png)

1. Der Bereich **Jetzt sichern** wird geöffnet. Geben Sie den Tag an, bis zu dem der Wiederherstellungspunkt aufbewahrt werden soll. Bedarfsgesteuerte Sicherung können maximale 10 Jahre aufbewahrt werden.

   ![Wählen Sie das Aufbewahrungsdatum aus.](./media/backup-afs/retention-date.png)

1. Wählen Sie **OK** aus, um die Ausführung des bedarfsgesteuerten Sicherungsauftrags zu bestätigen.

1. Überwachen Sie die Portalbenachrichtigungen, um den Abschluss der Ausführung des Sicherungsauftrags zu verfolgen. Sie können den Auftragsstatus im Dashboard des Tresors überwachen. Wählen Sie dazu **Sicherungsaufträge** > **In Bearbeitung** aus.

>[!NOTE]
>Azure Backup sperrt das Speicherkonto, wenn Sie Schutz für eine beliebige Dateifreigabe im entsprechenden Konto konfigurieren. Dies bietet Schutz vor dem versehentlichen Löschen eines Speicherkontos mit gesicherten Dateifreigaben.

## <a name="best-practices"></a>Bewährte Methoden

* Löschen Sie keine Momentaufnahmen, die mit Azure Backup erstellt wurden. Das Löschen von Momentaufnahmen kann zum Verlust von Wiederherstellungspunkten bzw. zu Wiederherstellungsfehlern führen.

* Entfernen Sie die von Azure Backup gesetzte Sperre für das Speicherkonto nicht. Wenn Sie die Sperre löschen, ist Ihr Speicherkonto für versehentliches Löschen anfällig, und wenn es gelöscht wird, verlieren Sie Ihre Momentaufnahmen oder Sicherungen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel werden folgende Themen erläutert:

* [Wiederherstellen von Azure-Dateifreigaben](restore-afs.md)
* [Verwalten der Sicherungen von Azure-Dateifreigaben](manage-afs-backup.md)
