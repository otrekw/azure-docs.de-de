---
title: Erstellen von Recovery Services-Tresoren
description: In diesem Artikel erfahren Sie, wie Sie Recovery Services-Tresore zum Speichern der Sicherungen und Wiederherstellungspunkte erstellen.
ms.reviewer: sogup
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: 6a880f84d5e8626d36ac3f4b440436b479ec5f6d
ms.sourcegitcommit: f2149861c41eba7558649807bd662669574e9ce3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/07/2020
ms.locfileid: "75708515"
---
# <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

Bei einem Recovery Services-Tresor handelt es sich um eine Entität, in der alle im Laufe der Zeit erstellten Sicherungen und Wiederherstellungspunkte gespeichert werden. Der Recovery Services-Tresor enthält auch die Sicherungsrichtlinien, die den geschützten virtuellen Computern zugeordnet sind.

So erstellen Sie einen Recovery Services-Tresor

1. Melden Sie sich bei Ihrem Abonnement im [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie im Menü links **Alle Dienste** aus.

    ![Auswählen von „Alle Dienste“](./media/backup-create-rs-vault/click-all-services.png)

3. Geben Sie im Dialogfeld **Alle Dienste** **Recovery Services** ein. Die Liste der Ressourcen wird Ihrer Eingabe entsprechend gefiltert. Wählen Sie in der Liste der Ressourcen **Recovery Services-Tresore** aus.

    ![Eingeben und Auswählen von „Recovery Services-Tresore“](./media/backup-create-rs-vault/all-services.png)

    Die Liste mit den Recovery Services-Tresoren im Abonnement wird angezeigt.

4. Wählen Sie auf dem Dashboard **Recovery Services-Tresore** die Option **Hinzufügen** aus.

    ![Hinzufügen eines Recovery Services-Tresors](./media/backup-create-rs-vault/add-button-create-vault.png)

    Das Dialogfeld **Recovery Services-Tresor** wird geöffnet. Legen Sie Werte für **Name**, **Abonnement**, **Ressourcengruppe** und **Speicherort** fest.

    ![Konfigurieren des Recovery Services-Tresors](./media/backup-create-rs-vault/create-new-vault-dialog.png)

   - **Name**: Geben Sie einen Anzeigenamen ein, über den der Tresor identifiziert wird. Der Name muss für das Azure-Abonnement eindeutig sein. Geben Sie einen Namen ein, der mindestens zwei, aber nicht mehr als 50 Zeichen enthält. Der Name muss mit einem Buchstaben beginnen und darf nur Buchstaben, Zahlen und Bindestriche enthalten.
   - **Abonnement**: Wählen Sie das zu verwendende Abonnement aus. Wenn Sie nur in einem Abonnement Mitglied sind, wird dessen Name angezeigt. Falls Sie nicht sicher sind, welches Abonnement geeignet ist, können Sie das Standardabonnement bzw. das vorgeschlagene Abonnement verwenden. Es sind nur dann mehrere Auswahlmöglichkeiten verfügbar, wenn Ihr Geschäfts-, Schul- oder Unikonto mehreren Azure-Abonnements zugeordnet ist.
   - **Ressourcengruppe**: Verwenden Sie eine vorhandene Ressourcengruppe, oder erstellen Sie eine neue Ressourcengruppe. Um eine Liste der verfügbaren Ressourcengruppen in Ihrem Abonnement anzuzeigen, wählen Sie **Vorhandene verwenden** und dann eine Ressource im Dropdownlistenfeld aus. Wählen Sie zum Erstellen einer neuen Ressourcengruppe **Neu erstellen** aus, und geben Sie den Namen ein. Umfassende Informationen zu Ressourcengruppen finden Sie unter [Übersicht über den Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
   - **Standort**: Wählen Sie die geografische Region für den Tresor aus. Um einen Tresor zum Schutz virtueller Computer zu erstellen, **muss** sich dieser in derselben Region wie die virtuellen Computer befinden.

      > [!IMPORTANT]
      > Wenn Sie den Speicherort des virtuellen Computers nicht kennen, schließen Sie das Dialogfeld. Wechseln Sie zur Liste der virtuellen Computer im Portal. Falls Sie über virtuelle Computer in mehreren Regionen verfügen, erstellen Sie in jeder Region einen Recovery Services-Tresor. Erstellen Sie den Tresor am ersten Speicherort, bevor Sie den Tresor für einen anderen Speicherort erstellen. Das Angeben von Speicherkonten zum Speichern der Sicherungsdaten ist nicht erforderlich. Der Recovery Services-Tresor und der Azure Backup-Dienst nehmen dies automatisch vor.
      >
      >

5. Wenn Sie den Recovery Services-Tresor erstellen möchten, wählen Sie **Erstellen** aus.

    ![Erstellen des Recovery Services-Tresors](./media/backup-create-rs-vault/click-create-button.png)

    Es kann einige Zeit dauern, denn Recovery Services-Tresor zu erstellen. Verfolgen Sie die Statusbenachrichtigungen im **Benachrichtigungsbereich** in der oberen rechten Ecke im Portal. Nach Abschluss des Erstellungsvorgangs wird der Tresor in der Liste mit den Recovery Services-Tresoren angezeigt. Sollte der Tresor nicht angezeigt werden, wählen Sie **Aktualisieren**.

     ![Aktualisieren der Liste der Sicherungstresore](./media/backup-create-rs-vault/refresh-button.png)

## <a name="set-storage-redundancy"></a>Festlegen der Speicherredundanz

Azure Backup übernimmt automatisch die Speicherung für den Tresor. Sie müssen angeben, wie dieser Speicher repliziert wird.

1. Klicken Sie auf dem Blatt **Recovery Services-Tresore** auf den neuen Tresor. Klicken Sie im Abschnitt **Einstellungen** auf **Eigenschaften**.
2. Klicken Sie in **Eigenschaften** unter **Sicherungskonfiguration** auf **Aktualisieren**.

3. Wählen Sie den Speicherreplikationstyps aus, und klicken Sie auf **Speichern**.

     ![Speicherkonfiguration für neuen Tresor festlegen](./media/backup-try-azure-backup-in-10-mins/recovery-services-vault-backup-configuration.png)

   - Wir empfehlen, dass Sie, wenn Sie Azure als primären Endpunkt für den Sicherungsspeicher verwenden, weiterhin die Standardeinstellung **Georedundant** verwenden.
   - Wenn Sie Azure nicht als primären Speicherendpunkt für die Sicherung verwenden, wählen Sie **Lokal redundant** aus. Dadurch verringern sich die Kosten für Azure-Speicher.
   - Erfahren Sie mehr über [Georedundanz](../storage/common/storage-redundancy-grs.md) and [lokale Redundanz](../storage/common/storage-redundancy-lrs.md).

> [!NOTE]
> Die Änderung des **Speicherreplikationstyps** (lokal redundant/georedundant) für einen Recovery Services-Tresor muss vorgenommen werden, bevor Sicherungen im Tresor konfiguriert werden. Nachdem Sie die Sicherung konfiguriert haben, ist die Option für die Änderung deaktiviert, und Sie können den **Speicherreplikationstyp** nicht ändern.

## <a name="set-cross-region-restore"></a>Festlegen der bereichsübergreifenden Wiederherstellung

Als eine der Wiederherstellungsoptionen ermöglicht die bereichsübergreifende Wiederherstellung (Cross Region Restore, CRR) die Wiederherstellung von virtuellen Azure-Computern in einer sekundären Region, bei der es sich um eine [gepaarte Azure-Region](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) handelt. Diese Option ermöglicht Folgendes:

- Durchführen von Übungen für Audit- oder Complianceanforderungen
- Wiederherstellen des virtuellen Computers oder dessen Datenträgers im Falle eines Notfalls in der primären Region

Legen Sie zum Auswählen dieses Features auf dem Blatt **Sicherungskonfiguration** die Option **Bereichsübergreifende Wiederherstellung** auf „Aktiviert“ fest.

Dieser Prozess hat Auswirkungen auf den Preis, da er auf der Speicherebene erfolgt.

>[!NOTE]
>Vorbereitungen
>
>- Machen Sie sich anhand der [Unterstützungsmatrix](backup-support-matrix.md#cross-region-restore) mit der Liste unterstützter verwalteter Typen und Regionen vertraut.
>- Die bereichsübergreifende Wiederherstellung (Cross Region Restore, CRR) steht aktuell nur in der Region „USA, Westen-Mitte“ (WCUS) zur Verfügung.
>- CRR ist ein optionales Feature auf Tresorebene für beliebige GRS-Tresore und standardmäßig deaktiviert.
>- Verwenden Sie *"featureName": "CrossRegionRestore"* , um das Onboarding Ihres Abonnements für dieses Feature durchzuführen.
>- Wenn das Onboarding für dieses Feature im Rahmen der eingeschränkten Public Preview-Phase erfolgt, enthält die Bestätigungs-E-Mail Details zur Preisrichtlinie.
>- Nach der Aktivierung kann es bis zu 48 Stunden dauern, bis die Sicherungselemente in sekundären Regionen verfügbar sind.
>- Momentan wird CRR nur für virtuelle Azure-Computer mit dem Sicherungsverwaltungstyp „ARM“ unterstützt. (Klassische virtuelle Azure-Computer werden nicht unterstützt.)  Wenn CRR von weiteren Verwaltungstypen unterstützt wird, werden diese **automatisch** registriert.

### <a name="configure-cross-region-restore"></a>Konfigurieren der bereichsübergreifenden Wiederherstellung

Bei einem mit GRS-Redundanz erstellten Tresor kann die bereichsübergreifende Wiederherstellung konfiguriert werden. Jeder GRS-Tresor verfügt über ein Banner, das mit der Dokumentation verknüpft ist. Wenn Sie CRR für den Tresor konfigurieren möchten, navigieren Sie zum Blatt mit der Sicherungskonfiguration. Dort befindet sich die Option zum Aktivieren dieses Features.

 ![Banner für die Sicherungskonfiguration](./media/backup-azure-arm-restore-vms/banner.png)

1. Navigieren Sie im Portal zu „Recovery Services-Tresor“ > „Einstellungen“ > „Eigenschaften“.
2. Klicken Sie unter **Bereichsübergreifende Wiederherstellung** auf „Aktivieren“, um die Funktion zu aktivieren.

   ![Vor Aktivierung der bereichsübergreifenden Wiederherstellung in diesem Tresor](./media/backup-azure-arm-restore-vms/backup-configuration1.png)

   ![Nach Aktivierung der bereichsübergreifenden Wiederherstellung in diesem Tresor](./media/backup-azure-arm-restore-vms/backup-configuration2.png)

Informationen zum Anzeigen von Sicherungselementen in der sekundären Region finden Sie [hier](backup-azure-arm-restore-vms.md#view-backup-items-in-secondary-region).

Informationen zur Wiederherstellung in der sekundären Region finden Sie [hier](backup-azure-arm-restore-vms.md#restore-in-secondary-region).

Informationen zum Überwachen von Wiederherstellungsaufträgen für die sekundäre Regionen finden Sie [hier](backup-azure-arm-restore-vms.md#monitoring-secondary-region-restore-jobs).

## <a name="modifying-default-settings"></a>Ändern der Standardeinstellungen

Es wird dringend empfohlen, vor dem Konfigurieren von Sicherungen im Tresor die Standardeinstellungen für **Speicherreplikationstyp** und **Sicherheitseinstellungen** zu überprüfen.

- **Speicherreplikationstyp** ist standardmäßig auf **Georedundant** festgelegt. Nachdem Sie die Sicherung konfiguriert haben, ist die Option zum Ändern deaktiviert. Führen Sie diese [Schritte](https://docs.microsoft.com/azure/backup/backup-create-rs-vault#set-storage-redundancy) aus, um die Einstellungen zu überprüfen und zu ändern.

- **Vorläufiges Löschen** ist für neu erstellte Tresore standardmäßig auf **Aktiviert** festgelegt, um das versehentliche oder bösartige Löschen von Sicherungsdaten zu verhindern. Führen Sie diese [Schritte](https://docs.microsoft.com/azure/backup/backup-azure-security-feature-cloud#disabling-soft-delete) aus, um die Einstellungen zu überprüfen und zu ändern.

## <a name="next-steps"></a>Nächste Schritte

[Übersicht über Recovery Services-Tresore](backup-azure-recovery-services-vault-overview.md)
Informieren Sie sich über das [Löschen von Recovery Services-Tresoren](backup-azure-delete-vault.md).
