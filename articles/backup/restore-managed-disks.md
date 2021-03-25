---
title: Wiederherstellen von Azure Managed Disks
description: Erfahren Sie, wie Sie Azure Managed Disks über das Azure-Portal wiederherstellen können.
ms.topic: conceptual
ms.date: 01/07/2021
ms.openlocfilehash: 995217cd17d1e2a16cd7a5f963ee88aa7116d4a7
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101703748"
---
# <a name="restore-azure-managed-disks-in-preview"></a>Wiederherstellen von Azure Managed Disks (Vorschau)

>[!IMPORTANT]
>Die Vorschauversion von Azure Disk Backup wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Informationen zur regionalen Verfügbarkeit finden Sie in der [Supportmatrix](disk-backup-support-matrix.md).
>
>[Füllen Sie dieses Formular aus](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR1vE8L51DIpDmziRt_893LVUNFlEWFJBN09PTDhEMjVHS05UWFkxUlUzUS4u), wenn Sie sich für die Vorschau registrieren möchten.

In diesem Artikel wird erklärt, wie Sie [Azure Managed Disks](../virtual-machines/managed-disks-overview.md) aus einem von Azure Backup erstellten Wiederherstellungspunkt wiederherstellen.

Derzeit wird die Wiederherstellungsoption „Wiederherstellung am ursprünglichen Speicherort“, bei der die Wiederherstellung die vorhandene Instanz auf dem Quelldatenträger, von dem die Sicherungen erstellt wurden, ersetzt, nicht unterstützt. Sie können eine Wiederherstellung aus einem Wiederherstellungspunkt vornehmen, um einen neuen Datenträger entweder in derselben Ressourcengruppe wie der Quelldatenträger, von dem die Sicherungen erstellt wurden, oder in einer beliebigen anderen Ressourcengruppe zu erstellen. Dies wird als „Wiederherstellung an einem anderen Speicherort“ bezeichnet und hilft, sowohl den Quelldatenträger als auch den wiederhergestellten (neuen) Datenträger zu behalten.

In diesem Artikel lernen Sie Folgendes:

- Wiederherstellen zum Erstellen eines neuen Datenträgers

- Nachverfolgen des Status des Wiederherstellungsvorgangs

## <a name="restore-to-create-a-new-disk"></a>Wiederherstellen zum Erstellen eines neuen Datenträgers

Der Sicherungstresor verwendet die verwaltete Identität für den Zugriff auf andere Azure-Ressourcen. Um die Wiederherstellung aus einer Sicherung vornehmen, erfordert die verwaltete Identität des Sicherungstresors einen Satz von Berechtigungen für die Ressourcengruppe, in der der Datenträger wiederhergestellt werden soll.

Der Sicherungstresor verwendet eine systemseitig zugewiesene verwaltete Identität, die auf eine Ressource beschränkt und an den Lebenszyklus dieser Ressource gebunden ist. Sie können der verwalteten Identität mithilfe der rollenbasierten Zugriffssteuerung von Azure (Azure RBAC) Berechtigungen erteilen. Eine verwaltete Identität ist ein spezieller Dienstprinzipal, der nur zusammen mit Azure-Ressourcen verwendet werden kann. Informieren Sie sich ausführlicher über [verwaltete Identitäten](../active-directory/managed-identities-azure-resources/overview.md).

Der Wiederherstellungsvorgang setzt voraus, dass folgende Schritte ausgeführt werden:

1. Weisen Sie der verwalteten Identität des Sicherungstresors in der Ressourcengruppe, in der der Datenträger vom Azure Backup-Dienst wiederhergestellt werden soll, die Rolle **Operator für die Datenträgerwiederherstellung** zu.

    >[!NOTE]
    > Sie können dieselbe Ressourcengruppe wie die des Quelldatenträgers, von dem die Sicherungen erstellt werden, oder eine beliebige andere Ressourcengruppe innerhalb desselben oder eines anderen Abonnements auswählen.

    1. Wechseln Sie zu der Ressourcengruppe, in der der Datenträger wiederhergestellt werden soll. Die Ressourcengruppe ist beispielsweise *TargetRG*.

    1. Wechseln Sie zur **Zugriffssteuerung (IAM)** , und wählen Sie anschließend **Rollenzuweisung hinzufügen** aus.

    1. Wählen Sie im rechten Kontextbereich in der Dropdownliste **Rolle** die Option **Operator für die Datenträgerwiederherstellung** aus. Wählen Sie die verwaltete Identität des Sicherungstresors und dann **Speichern** aus.

        >[!TIP]
        >Geben Sie den Namen des Sicherungstresors ein, um die verwaltete Identität des Tresors auszuwählen.

        ![Rolle „Operator für die Datenträgerwiederherstellung“ auswählen](./media/restore-managed-disks/disk-restore-operator-role.png)

1. Vergewissern Sie sich, dass die verwaltete Identität des Sicherungstresors für die Ressourcengruppe, in der der Datenträger wiederhergestellt wird, über den richtigen Satz an Rollenzuweisungen verfügt.

    1. Wechseln Sie zu **Sicherungstresor > Identität**, und wählen Sie **Azure-Rollenzuweisungen**

        ![Azure-Rollenzuweisungen auswählen](./media/restore-managed-disks/azure-role-assignments.png)

    1. Überprüfen Sie, ob Rolle, Ressourcenname und Ressourcentyp ordnungsgemäß angezeigt werden.

        ![Rolle, Ressourcennamen und Ressourcentyp überprüfen](./media/restore-managed-disks/verify-role.png)

    >[!NOTE]
    >Während die Rollenzuweisungen im Portal richtig widergegeben werden, kann es ungefähr 15 Minuten dauern, bis die Berechtigung auf die verwaltete Identität des Sicherungstresors angewendet wird.
    >
    >Bei geplanten oder bedarfsgesteuerten Sicherungen speichert Azure Backup die inkrementellen Momentaufnahmen des Datenträgers in der Momentaufnahme-Ressourcengruppe, die beim Konfigurieren der Sicherung des Datenträgers bereitgestellt wird. Azure Backup verwendet diese inkrementellen Momentaufnahmen während des Wiederherstellungsvorgangs. Wenn die Momentaufnahmen gelöscht oder aus der Momentaufnahme-Ressourcengruppe verschoben werden oder wenn die Rollenzuweisungen des Sicherungstresors für die Ressourcengruppe für die Momentaufnahme aufgehoben werden, tritt beim Wiederherstellungsvorgang ein Fehler auf.

1. Wenn der wiederherzustellende Datenträger mit [kundenseitig verwalteten Schlüsseln (Customer-Managed Keys, CMK)](../virtual-machines/disks-enable-customer-managed-keys-portal.md) oder mit [Mehrfachverschlüsselung mithilfe von über die Plattform verwalteten Schlüsseln und kundenseitig verwalteten Schlüsseln](../virtual-machines/disks-enable-double-encryption-at-rest-portal.md) verschlüsselt wurde, weisen Sie der verwalteten Identität des Sicherungstresors auf der Ressource **Datenträgerverschlüsselungssatz** die Rolle **Leser** zu.

Sobald die Voraussetzungen erfüllt sind, führen Sie die folgenden Schritte aus, um den Wiederherstellungsvorgang zu starten.

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com/) zu **Backup Center**. Wählen Sie im Abschnitt **Verwalten** die Option **Sicherungsinstanzen** aus. Wählen Sie aus der Liste der Sicherungsinstanzen die gewünschte Sicherungsinstanz aus, für die Sie den Wiederherstellungsvorgang durchführen möchten.

    ![Liste der Sicherungsinstanzen](./media/restore-managed-disks/backup-instances.png)

    Alternativ können Sie diesen Vorgang über den Sicherungstresor ausführen, den Sie zum Konfigurieren der Sicherung für den Datenträger verwendet haben.

1. Wählen Sie auf dem Bildschirm **Sicherungsinstanz** den Wiederherstellungspunkt aus, den Sie für die Wiederherstellung verwenden möchten, und wählen Sie **Wiederherstellen** aus.

    ![Auswählen eines Wiederherstellungspunkts](./media/restore-managed-disks/select-restore-point.png)

1. Prüfen Sie im Workflow **Wiederherstellen** die Informationen auf den Registerkarten **Grundlagen** und **Wiederherstellungspunkt auswählen** und wählen Sie **Weiter: Wiederherstellungsparameter** aus.

    ![Informationen von „Grundlagen“ und „Wiederherstellungspunkt auswählen“ überprüfen](./media/restore-managed-disks/review-information.png)

1. Wählen Sie auf der Registerkarte **Wiederherstellungsparameter** das **Zielabonnement** und die **Zielressourcengruppe** zur Wiederherstellung der Sicherung aus. Geben Sie den Namen des wiederherzustellenden Datenträgers an. Klicken Sie auf **Weiter: Überprüfen + Wiederherstellen**.

    ![Wiederherstellungsparameter](./media/restore-managed-disks/restore-parameters.png)

    >[!TIP]
    >Datenträger, die durch Azure Backup mithilfe der Lösung für die Datenträgersicherung gesichert werden, können auch durch Azure Backup mithilfe der Azure VM-Sicherungslösung mit dem Recovery Services-Tresor gesichert werden. Wenn Sie den Schutz des virtuellen Azure-Computers, an den dieser Datenträger angefügt ist, konfiguriert haben, können Sie auch den Wiederherstellungsvorgang des virtuellen Azure-Computers verwenden. Sie können wählen, ob Sie den virtuellen Computer oder Datenträger und Dateien oder Ordner aus dem Wiederherstellungspunkt der entsprechenden Sicherungsinstanz des Azure-Computers wiederherstellen möchten. Weitere Informationen finden Sie auf der [Sicherung von virtuellen Azure-Computern](./about-azure-vm-restore.md).

1. Nachdem die Überprüfung erfolgreich war, wählen Sie **Wiederherstellen** aus, um den Wiederherstellungsvorgang zu starten.

    ![Wiederherstellungsvorgang starten](./media/restore-managed-disks/initiate-restore.png)

    >[!NOTE]
    > Die Überprüfung kann einige Minuten dauern, bevor Sie den Wiederherstellungsvorgang starten können. Die Überprüfung kann fehlschlagen, wenn:
    >
    > - Ein Datenträger mit dem gleichen Namen in **Name des wiederhergestellten Datenträgers** bereits in der **Zielressourcengruppe** vorhanden ist.
    > - Die verwaltete Identität des Sicherungstresors keine gültigen Rollenzuweisungen für die **Zielressourcengruppe** aufweist.
    > - Die Rollenzuweisungen der verwalteten Identität des Sicherungstresors für die **Momentaufnahme-Ressourcengruppe**, in der inkrementelle Momentaufnahmen gespeichert werden, widerrufen wurden.
    > - Wenn inkrementelle Momentaufnahmen gelöscht oder aus der Momentaufnahme-Ressourcengruppe verschoben werden

Durch die Wiederherstellung wird in der während des Wiederherstellungsvorgangs angegebenen Zielressourcengruppe ein neuer Datenträger aus dem ausgewählten Wiederherstellungspunkt erstellt. Um den wiederhergestellten Datenträger auf einem vorhandenen virtuellen Computer zu verwenden, müssen Sie weitere Schritte durchführen:

- Wenn es sich bei dem wiederhergestellten Datenträger um einen regulären Datenträger handelt, können Sie einen vorhandenen Datenträger an einen virtuellen Computer anfügen. Wenn es sich bei dem wiederhergestellten Datenträger um einen Betriebssystem-Datenträger handelt, können Sie den Betriebssystem-Datenträger eines virtuellen Computers im Azure-Portal über den Bereich **Virtueller Computer** im Menü **Datenträger** unter **Einstellungen** tauschen.

    ![Betriebssystem-Datenträger tauschen](./media/restore-managed-disks/swap-os-disks.png)

- Wenn der wiederhergestellte Datenträger bei virtuellen Windows-Computern ein regulärer Datenträger ist, befolgen Sie die Anweisungen, um [den ursprünglichen regulären Datenträger](../virtual-machines/windows/detach-disk.md#detach-a-data-disk-using-the-portal) vom virtuellen Computer zu trennen. Anschließend können Sie [den wiederhergestellten Datenträger an den virtuellen Computer anfügen](../virtual-machines/windows/attach-managed-disk-portal.md). Befolgen Sie die Anweisungen zum [Tauschen des Betriebssystem-Datenträgers](../virtual-machines/windows/os-disk-swap.md) des virtuellen Computers gegen den wiederhergestellten Datenträger.

- Wenn der wiederhergestellte Datenträger bei virtuellen Linux-Computern ein regulärer Datenträger ist, befolgen Sie die Anweisungen, um [den ursprünglichen regulären Datenträger](../virtual-machines/linux/detach-disk.md#detach-a-data-disk-using-the-portal) vom virtuellen Computer zu trennen. Anschließend können Sie [den wiederhergestellten Datenträger an den virtuellen Computer anfügen](../virtual-machines/linux/attach-disk-portal.md#attach-an-existing-disk). Befolgen Sie die Anweisungen zum [Tauschen des Betriebssystem-Datenträgers](../virtual-machines/linux/os-disk-swap.md) des virtuellen Computers gegen den wiederhergestellten Datenträger.

Es wird empfohlen, die Rollenzuweisung **Operator für die Datenträgerwiederherstellung** aus der verwalteten Identität des Sicherungstresors in der **Zielressourcengruppe** zu widerrufen.

## <a name="track-a-restore-operation"></a>Wiederherstellungsvorgang nachverfolgen

Nachdem Sie den Wiederherstellungsvorgang ausgelöst haben, erstellt der Sicherungsdienst einen Auftrag zum Nachverfolgen. Azure Backup zeigt Benachrichtigungen zum Auftrag im Portal an. So zeigen Sie den Fortschritt des Wiederherstellungsauftrags an

1. Navigieren Sie zum Bildschirm **Sicherungsinstanz**. Hier wird das Dashboard „Aufträge“ mit Betrieb und Status der letzten sieben Tage angezeigt.

    ![Dashboard „Aufträge“](./media/restore-managed-disks/jobs-dashboard.png)

1. Wenn Sie den Status des Wiederherstellungsvorgangs anzeigen möchten, wählen Sie **Alle anzeigen** aus, um laufende und vergangene Aufträge dieser Sicherungsinstanz anzuzeigen.

    ![„Alle anzeigen“ auswählen](./media/restore-managed-disks/view-all.png)

1. Überprüfen Sie die Liste der Sicherungs- und Wiederherstellungsaufträge und deren Status. Wählen Sie in der Liste mit den Aufträgen einen Auftrag aus, um die Auftragsdetails anzuzeigen.

    ![Liste der Aufträge](./media/restore-managed-disks/list-of-jobs.png)

## <a name="next-steps"></a>Nächste Schritte

- [Häufig gestellte Fragen zu Azure Disk Backup](disk-backup-faq.md)