---
title: Sichern von Azure-Dateifreigaben im Azure-Portal
description: Erfahren Sie, wie Sie das Azure-Portal zum Sichern von Azure-Dateifreigaben im Recovery Services-Tresor verwenden.
ms.topic: conceptual
ms.date: 01/20/2020
ms.openlocfilehash: da2c7fa4cc5c3b7b948604a6f6d3999671cb3697
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82101289"
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

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com/) den Recovery Services-Tresor, mit dem Sie die Dateifreigaben sichern möchten.

1. Wählen Sie **+Sichern** im Dashboard des **Recovery Services-Tresors** aus.

   ![Recovery Services-Tresor](./media/backup-afs/recovery-services-vault.png)

    a. Legen Sie unter **Sicherungsziel** die Einstellung **Wo wird die Workload ausgeführt?** auf **Azure** fest.

    ![Auswählen einer Azure-Dateifreigabe als Sicherungsziel](./media/backup-afs/backup-goal.png)

    b.  Wählen Sie unter **Was möchten Sie sichern?** aus der Dropdownliste die Option **Azure-Dateifreigabe** aus.

    c.  Wählen Sie **Sicherung** aus, um die Azure-Dateifreigabeerweiterung im Tresor zu registrieren.

    ![Wählen Sie „Sicherung“ aus, um die Azure-Dateifreigabe mit dem Tresor zu verknüpfen.](./media/backup-afs/register-extension.png)

1. Wenn Sie **Sicherung** ausgewählt haben, wird der Bereich **Sicherung** geöffnet, und Sie werden aufgefordert, ein Speicherkonto aus einer Liste der ermittelten unterstützten Speicherkonten auszuwählen. Sie sind entweder diesem Tresor zugeordnet oder befinden sich in derselben Region wie der Tresor, sind aber noch keinem Recovery Services-Tresor zugeordnet.

1. Wählen Sie aus der Liste der ermittelten Speicherkonten ein Speicherkonto und dann **OK** aus. Azure durchsucht das Speicherkonto nach Dateifreigaben, die gesichert werden können. Falls Sie die Dateifreigaben erst kürzlich hinzugefügt haben und diese nicht in der Liste angezeigt werden, warten Sie einen Moment, bis die Dateifreigaben angezeigt werden.

    ![Ermitteln von Dateifreigaben](./media/backup-afs/discovering-file-shares.png)

1. Wählen Sie in der Liste **Dateifreigaben** mindestens eine zu sichernde Dateifreigabe aus. Klicken Sie auf **OK**.

   ![Auswählen der Dateifreigaben](./media/backup-afs/select-file-shares.png)

1. Nachdem Sie Ihre Dateifreigaben ausgewählt haben, wechselt das Menü **Sicherung** zu **Sicherheitsrichtlinie**. Wählen Sie in diesem Menü entweder eine vorhandene Sicherheitsrichtlinie aus, oder erstellen Sie eine neue. Wählen Sie dann **Sicherung aktivieren** aus.

    ![Auswählen der Sicherungsrichtlinie](./media/backup-afs/select-backup-policy.png)

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
