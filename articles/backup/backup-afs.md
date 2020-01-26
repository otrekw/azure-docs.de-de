---
title: Sichern von Azure-Dateifreigaben im Azure-Portal
description: Hier erfahren Sie, wie Sie im Azure-Portal Azure-Dateifreigaben im Recovery Services-Tresor sichern.
ms.topic: conceptual
ms.date: 01/20/2020
ms.openlocfilehash: b2e2053857147513a95b3ae72b82d55450ebcffa
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294880"
---
# <a name="back-up-azure-file-shares-in-a-recovery-services-vault"></a>Sichern von Azure-Dateifreigaben in einem Recovery Services-Tresor

In diesem Artikel wird beschrieben, wie Sie mithilfe des Azure-Portals [Azure Dateifreigaben](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) sichern.

In diesem Leitfaden lernen Sie Folgendes:

* Erstellen eines Recovery Services-Tresors
* Ermitteln von Dateifreigaben und Konfigurieren von Sicherungen
* Ausführen eines bedarfsbasierten Sicherungsauftrags zum Erstellen eines Wiederherstellungspunkts

## <a name="prerequisites"></a>Voraussetzungen

* Bestimmen oder erstellen Sie einen [Recovery Services-Tresor](#create-a-recovery-services-vault) in derselben Region wie das Speicherkonto, das die Dateifreigabe hostet.

* Stellen Sie sicher, dass sich die Dateifreigabe in einem der [unterstützten Speicherkontotypen](#limitations-for-azure-file-share-backup-during-preview) befindet.

## <a name="limitations-for-azure-file-share-backup-during-preview"></a>Einschränkungen beim Sichern von Azure-Dateifreigaben während der Vorschauphase

Die Sicherung für Azure-Dateifreigaben befindet sich in der Vorschauphase. Azure-Dateifreigaben in Speicherkonten vom Typ „Universell V1“ und „Universell V2“ werden unterstützt. Folgende Einschränkungen gelten für das Sichern von Azure-Dateifreigaben:

* Unterstützung für die Sicherung von Azure-Dateifreigaben in Speicherkonten mit Replikation vom Typ [ZRS](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs) (Zone Redundant Storage, zonenredundanter Speicher) ist momentan auf [diese Regionen](https://docs.microsoft.com/azure/backup/backup-azure-files-faq#in-which-geos-can-i-back-up-azure-file-shares) beschränkt.
* Azure Backup unterstützt derzeit das Konfigurieren geplanter einmaliger täglicher Sicherungen von Azure-Dateifreigaben.
* Die Anzahl geplanter Sicherungen ist auf eine Sicherung pro Tag begrenzt.
* Die Anzahl bedarfsgesteuerter Sicherungen ist auf vier Sicherungen pro Tag begrenzt.
* Verwenden Sie [Ressourcensperren](https://docs.microsoft.com/cli/azure/resource/lock?view=azure-cli-latest) für das Speicherkonto, um das versehentliche Löschen von Sicherungen in Ihrem Recovery Services-Tresor zu verhindern.
* Löschen Sie keine Momentaufnahmen, die mit Azure Backup erstellt wurden. Das Löschen von Momentaufnahmen kann zum Verlust von Wiederherstellungspunkten bzw. zu Wiederherstellungsfehlern führen.
* Löschen Sie keine Dateifreigaben, die durch Azure Backup geschützt sind. In der aktuellen Lösung werden nach dem Löschen der Dateifreigabe alle von Azure Backup erstellten Momentaufnahmen gelöscht, sodass alle Wiederherstellungspunkte verloren gehen.

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="modify-storage-replication"></a>Ändern der Speicherreplikation

Tresore verwenden standardmäßig den [georedundanten Speicher (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs).

* Wenn der Tresor Ihr Hauptmechanismus für Sicherungen ist, empfehlen wir die Verwendung von GRS.

* Sie können [Lokal redundanter Speicher (LRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) als kostengünstigere Option verwenden.

Passen Sie die Speicherreplikationstyp wie folgt an:

1. Klicken Sie im neuen Tresor im Abschnitt **Einstellungen** auf **Eigenschaften**.

2. Klicken Sie in **Eigenschaften** unter **Sicherungskonfiguration** auf **Aktualisieren**.

3. Wählen Sie den Speicherreplikationstyps aus, und klicken Sie auf **Speichern**.

    ![Aktualisieren der Sicherungskonfiguration](./media/backup-afs/backup-configuration.png)

> [!NOTE]
> Sie können den Speichertyp für die Replikation nicht mehr ändern, nachdem der Tresor eingerichtet wurde und Sicherungselemente enthält. Dazu müssen Sie den Tresor neu erstellen.
>

## <a name="discover-file-shares-and-configure-backup"></a>Ermitteln der Dateifreigaben und Konfigurieren der Sicherung

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com/) den Recovery Services-Tresor, mit dem Sie die Dateifreigaben sichern möchten.

2. Wählen Sie **+Sichern** im Dashboard von **Recovery Services-Tresor** aus.

   ![Recovery Services-Tresor](./media/backup-afs/recovery-services-vault.png)

   a. Legen Sie unter **Sicherungsziel** die Einstellung **Wo wird die Workload ausgeführt?** auf **Azure** fest.

    ![Auswählen einer Azure-Dateifreigabe als Sicherungsziel](./media/backup-afs/backup-goal.png)

    b.    Wählen Sie unter **Was möchten Sie sichern?** aus dem Dropdownmenü die Option **Azure-Dateifreigabe** aus.

    c.    Klicken Sie auf **Sicherung**, um die Azure-Dateifreigabeerweiterung im Tresor zu registrieren.

      ![Klicken auf „Sicherung“, um die Azure-Dateifreigabe mit dem Tresor zu verknüpfen.](./media/backup-afs/register-extension.png)

3. Wenn Sie auf **Sicherung** geklickt haben, wird das Blatt „Sicherung“ geöffnet, und Sie werden aufgefordert, ein Speicherkonto aus einer Liste der ermittelten unterstützten Speicherkonten auszuwählen. Sie sind entweder diesem Tresor zugeordnet oder befinden sich in derselben Region wie der Tresor, sind aber noch keinem Recovery Services-Tresor zugeordnet.

   ![Auswählen des Speicherkontos](./media/backup-afs/select-storage-account.png)

4. Wählen Sie aus der Liste der ermittelten Speicherkonten ein Speicherkonto aus, und klicken Sie auf **OK**. Azure durchsucht das Speicherkonto nach Dateifreigaben, die gesichert werden können. Falls Sie die Dateifreigaben erst kürzlich hinzugefügt haben und diese nicht in der Liste angezeigt werden, warten Sie einen Moment, bis die Dateifreigaben angezeigt werden.

    ![Ermitteln von Dateifreigaben](./media/backup-afs/discovering-file-shares.png)

5. Wählen Sie in der Liste **Dateifreigaben** mindestens eine zu sichernde Dateifreigabe aus, und klicken Sie auf **OK**.

6. Nach dem Auswählen der Dateifreigaben wechselt das Menü **Sicherung** zu **Sicherheitsrichtlinie**. Wählen Sie in diesem Menü entweder eine vorhandene Sicherheitsrichtlinie aus, oder erstellen Sie eine neue, und klicken Sie dann auf **Sicherung aktivieren**.

    ![Auswählen der Sicherungsrichtlinie](./media/backup-afs/select-backup-policy.png)

Nach dem Einrichten einer Sicherungsrichtlinie wird eine Momentaufnahme der Dateifreigaben zum geplanten Zeitpunkt erstellt, und der Wiederherstellungspunkt wird für den ausgewählten Zeitraum aufbewahrt.

## <a name="create-an-on-demand-backup"></a>Erstellen einer bedarfsgesteuerten Sicherung

Gelegentlich empfiehlt es sich, eine Sicherungsmomentaufnahme oder einen Wiederherstellungspunkt außerhalb der geplanten Zeiten in der Sicherungsrichtlinie zu erstellen. Häufig ist es sinnvoll, eine bedarfsgesteuerte Sicherung direkt nach dem Konfigurieren der Sicherungsrichtlinie zu erstellen. Basierend auf den Zeitplan in der Sicherungsrichtlinie, kann es Stunden oder Tage dauern, bis eine Momentaufnahme erstellt wird. Initiieren Sie eine bedarfsgesteuerte Sicherung, um Ihre Daten zu schützen, bevor die Sicherungsrichtlinie in Kraft tritt. Die Erstellung einer bedarfsgesteuerten Sicherung ist häufig erforderlich, bevor Sie geplante Änderungen an den Dateifreigaben vornehmen.

### <a name="to-create-an-on-demand-backup"></a>So erstellen Sie eine bedarfsgesteuerte Sicherung

1. Öffnen Sie den Recovery Services-Tresor, den Sie zum Sichern der Dateifreigabe verwendet haben, und klicken Sie im Blatt **Übersicht** im Abschnitt **Geschützte Elemente** auf **Sicherungselemente**.

   ![Klicken Sie auf „Sicherungselemente“.](./media/backup-afs/backup-items.png)

2. Sobald Sie auf **Sicherungselemente** geklickt haben, wird neben dem Blatt **Übersicht** angezeigt ein neues Blatt mit allen **Sicherungsverwaltungstypen** wie folgt angezeigt:

   ![Liste mit Sicherungsverwaltungstypen](./media/backup-afs/backup-management-types.png)

3. Wählen Sie aus der Liste der **Sicherungsverwaltungstypen** die Option **Azure Storage (Azure Files)** aus. Angezeigt wird eine Liste aller Dateifreigaben und der entsprechenden Speicherkonten, die mit diesem Tresor gesichert werden.

   ![Azure Storage (Azure Files)-Sicherungselemente](./media/backup-afs/azure-files-backup-items.png)

4. Wählen Sie in der Liste der Azure-Dateifreigaben die gewünschte Dateifreigabe aus. Einzelheiten zum **Sicherungselement** werden angezeigt. Klicken Sie im Menü **Sicherungselement** auf **Jetzt sichern**. Da es sich um einen bedarfsgesteuerten Sicherungsauftrag handelt, ist dem Wiederherstellungspunkt keine Aufbewahrungsrichtlinie zugeordnet.

   ![Klicken Sie auf „Jetzt sichern“.](./media/backup-afs/backup-now.png)

5. Das Blatt **Jetzt sichern** wird geöffnet. Geben Sie den Tag an, bis zu dem der Wiederherstellungspunkt aufbewahrt werden soll. Bedarfsgesteuerte Sicherung können maximale 10 Jahre aufbewahrt werden.

   ![Wählen Sie das Aufbewahrungsdatum aus.](./media/backup-afs/retention-date.png)

6. Klicken Sie auf **OK**, um die Ausführung des bedarfsgesteuerten Sicherungsauftrags zu bestätigen.

7. Überwachen Sie die Portalbenachrichtigungen, um den Abschluss der Ausführung des Sicherungsauftrags zu verfolgen. Sie können den Auftragsstatus im Dashboard des Tresors unter **Sicherungsaufträge** > **In Bearbeitung** überwachen.

## <a name="next-steps"></a>Nächste Schritte

* [Wiederherstellen von Azure-Dateifreigaben](restore-afs.md)

* [Verwalten der Sicherungen von Azure-Dateifreigaben](manage-afs-backup.md)
