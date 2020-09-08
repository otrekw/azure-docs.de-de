---
title: 'Tutorial: Sichern von mehreren virtuellen Azure-Computern'
description: In diesem Tutorial erfahren Sie, wie Sie einen Recovery Services-Tresor erstellen, eine Sicherungsrichtlinie definieren und mehrere virtuelle Computer gleichzeitig sichern.
ms.date: 07/26/2020
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: a9517ffc1e37d50f7c0e57b9ed53fb8bcf55fd70
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89180574"
---
# <a name="use-azure-portal-to-back-up-multiple-virtual-machines"></a>Verwenden des Azure-Portals zum Sichern mehrerer virtueller Computer

Wenn Sie Daten in Azure sichern, speichern Sie diese Daten in einer Azure-Ressource mit der Bezeichnung Recovery Services-Tresor. Sie finden den Recovery Services-Tresor in den meisten Azure-Diensten im Einstellungsmenü. Die Integration des Recovery Services-Tresors in das Einstellungsmenü der meisten Azure-Dienste vereinfacht das Sichern von Daten. Es ist jedoch mühsam, mit jeder Datenbank oder jedem virtuellen Computer in Ihrem Unternehmen einzeln zu arbeiten. Was geschieht, wenn Sie die Daten für alle virtuellen Computer in einer Abteilung oder an einem Standort sichern möchten? Es ist einfach, mehrere virtuelle Computer zu sichern, indem Sie eine Sicherungsrichtlinie erstellen und diese Richtlinie auf die gewünschten virtuellen Computer anwenden. In diesem Tutorial werden folgende Punkte erläutert:

> [!div class="checklist"]
>
> * Erstellen eines Recovery Services-Tresors
> * Definieren einer Sicherungsrichtlinie
> * Anwenden der Sicherungsrichtlinie zum Schützen mehrerer virtueller Computer
> * Auslösen eines bedarfsgesteuerten Sicherungsauftrags für die geschützten virtuellen Computer

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

Der Recovery Services-Tresor enthält die Sicherungsdaten, und Sicherungsrichtlinie, die auf die geschützten virtuellen Computer angewendet wird. Das Sichern virtueller Computer ist ein örtlich gebundener Vorgang. Es ist nicht möglich, einen virtuellen Computer eines Standorts in einem Recovery Services-Tresor an einem anderen Standort zu sichern. An jedem Azure-Standort, der über zu sichernde virtuelle Computer verfügt, muss also mindestens ein Recovery Services-Tresor vorhanden sein.

1. Wählen Sie im Menü links **Alle Dienste** aus.

    ![Auswählen von „Alle Dienste“](./media/tutorial-backup-vm-at-scale/click-all-services.png)

1. Geben Sie im Dialogfeld **Alle Dienste** *Recovery Services* ein. Die Liste der Ressourcen wird Ihrer Eingabe entsprechend gefiltert. Wählen Sie in der Liste der Ressourcen **Recovery Services-Tresore** aus.

    ![Eingeben und Auswählen von „Recovery Services-Tresore“](./media/tutorial-backup-vm-at-scale/all-services.png)

    Die Liste mit den Recovery Services-Tresoren im Abonnement wird angezeigt.

1. Wählen Sie auf dem Dashboard **Recovery Services-Tresore** die Option **Hinzufügen** aus.

    ![Hinzufügen eines Recovery Services-Tresors](./media/tutorial-backup-vm-at-scale/add-button-create-vault.png)

1. Führen Sie im Menü „Recovery Services-Tresor“ folgende Aktionen durch:

    * Geben Sie *myRecoveryServicesVault* in **Name** ein.
    * Die aktuelle Abonnement-ID wird unter **Abonnement** angezeigt. Wenn Sie über weitere Abonnements verfügen, können Sie ein anderes Abonnement für den neuen Tresor auswählen.
    * Wählen Sie für **Ressourcengruppe** die Option **Vorhandene verwenden** und dann *myResourceGroup* aus. Wenn *myResourceGroup* nicht vorhanden ist, wählen Sie **Neu erstellen** aus und geben *myResourceGroup* ein.
    * Wählen Sie im Dropdownmenü **Standort** die Region *Europa, Westen* aus.

    ![Werte für den Recovery Services-Tresor](./media/tutorial-backup-vm-at-scale/review-and-create.png)

    Recovery Services-Tresore müssen sich am gleichen Standort wie die zu schützenden virtuellen Computer befinden. Falls Sie über virtuelle Computer in mehreren Regionen verfügen, erstellen Sie in jeder Region einen Recovery Services-Tresor. In diesem Tutorial wird ein Recovery Services-Tresor in *Europa, Westen* erstellt, da dort auch *myVM* (der im Schnellstart erstellte virtuelle Computer) erstellt wurde.

1. Wenn Sie den Recovery Services-Tresor erstellen möchten, wählen Sie **Erstellen** aus.

    ![Erstellen des Recovery Services-Tresors](./media/tutorial-backup-vm-at-scale/click-create-button.png)

1. Es kann einige Zeit dauern, denn Recovery Services-Tresor zu erstellen. Verfolgen Sie die Statusbenachrichtigungen im **Benachrichtigungsbereich** in der oberen rechten Ecke im Portal. Nach Abschluss des Erstellungsvorgangs wird der Tresor in der Liste mit den Recovery Services-Tresoren angezeigt. Sollte der Tresor nicht angezeigt werden, wählen Sie **Aktualisieren**.

     ![Aktualisieren der Liste der Sicherungstresore](./media/tutorial-backup-vm-at-scale/refresh-button.png)

Wenn Sie einen Recovery Services-Tresor erstellen, verfügt dieser standardmäßig über georedundanten Speicher. Für mehr Datenresilienz repliziert der georedundante Speicher die Daten mehrmals zwischen zwei Azure-Regionen.

## <a name="set-backup-policy-to-protect-vms"></a>Festlegen der Sicherungsrichtlinie zum Schutz von virtuellen Computern

Nach dem Erstellen des Recovery Services-Tresors konfigurieren Sie im nächsten Schritt den Tresor für den Datentyp und legen die Sicherungsrichtlinie fest. Die Sicherungsrichtlinie gibt die Häufigkeit und den Zeitpunkt für die Erstellung von Wiederherstellungspunkten an. Darüber hinaus enthält die Richtlinie die Beibehaltungsdauer für die Wiederherstellungspunkte. Für dieses Tutorial wird davon ausgegangen, dass Ihr Unternehmen ein Sportkomplex mit Hotel, Stadion, Restaurants und Konzessionen ist und Sie die Daten auf den virtuellen Computern schützen möchten. Anhand der folgenden Schritte erstellen Sie eine Sicherungsrichtlinie für Finanzdaten.

1. Wählen Sie in der Liste mit den Recovery Services-Tresoren **myRecoveryServicesVault** aus, um das zugehörige Dashboard zu öffnen.

   ![Menü „Szenario“ öffnen](./media/tutorial-backup-vm-at-scale/open-vault-from-list.png)

1. Wählen Sie im Menü des Tresordashboards **Sicherung** aus, um das Menü „Sicherung“ zu öffnen.

1. Wählen Sie im Menü „Sicherungsziel“ im Dropdownmenü **Wo wird Ihre Workload ausgeführt?** die Option *Azure* aus. Wählen Sie im Dropdownmenü **Was möchten Sie sichern?** die Option *Virtueller Computer* aus, und wählen Sie dann **Sichern** aus.

    Diese Aktionen bereiten den Recovery Services-Tresor auf die Interaktion mit einem virtuellen Computer vor. Recovery Services-Tresore haben eine Standardrichtlinie, die jeden Tag einen Wiederherstellungspunkt erstellt und die Wiederherstellungspunkte für 30 Tage aufbewahrt.

    ![Sicherungsziel](./media/tutorial-backup-vm-at-scale/backup-goal.png)

1. Zum Erstellen einer neuen Richtlinie wählen Sie im Menü „Sicherungsrichtlinie“ im Dropdownmenü **Sicherungsrichtlinie auswählen** die Option *Neue Richtlinie erstellen* aus.

    ![Erstellen Sie eine neue Richtlinie.](./media/tutorial-backup-vm-at-scale/create-new-policy.png)

1. Der Bereich **Sicherungsrichtlinie** wird geöffnet. Füllen Sie die folgenden Angaben aus:
   * Geben Sie für **Richtlinienname** die Angabe *Finance* ein. Geben Sie die folgenden Änderungen für die Sicherungsrichtlinie ein:
   * Legen Sie für **Sicherungshäufigkeit** die Zeitzone auf *Central Time* fest. Der Sportkomplex befindet sich in Texas, und der Besitzer möchte die lokale Zeit verwenden. Lassen Sie die Sicherungshäufigkeit auf täglich um 3:30 Uhr festgelegt.
   * Legen Sie für **Aufbewahrung für täglichen Sicherungspunkt** den Zeitraum auf 90 Tage fest.
   * Verwenden Sie für **Aufbewahrung für wöchentlichen Sicherungspunkt** den Wiederherstellungspunkt *Monday*, und legen Sie als Aufbewahrungszeit 52 Wochen fest.
   * Verwenden Sie für **Aufbewahrung für monatlichen Sicherungspunkt** den Wiederherstellungspunkt vom ersten Sonntag des Monats, und legen Sie als Aufbewahrungszeit 36 Monate fest.
   * Deaktivieren Sie die Option **Aufbewahrung für jährlichen Sicherungspunkt**. Der Leiter der Finanzabteilung möchte die Daten nicht länger als 36 Monate aufbewahren.
   * Wählen Sie **OK** aus, um die Sicherungsrichtlinie zu erstellen.

     ![Sicherungsrichtlinieneinstellungen](./media/tutorial-backup-vm-at-scale/set-new-policy.png)

     Nach ihrer Erstellung weisen Sie die Sicherungsrichtlinie den virtuellen Computern zu.

1. Wählen Sie in unter **Virtuelle Computer** die Option **Hinzufügen** aus.

     ![Virtuelle Computer hinzufügen](./media/tutorial-backup-vm-at-scale/add-virtual-machines.png)

1. Der Bereich **Virtuelle Computer auswählen** wird geöffnet. Wählen Sie *myVM* und dann **OK** aus, um die Sicherungsrichtlinie auf den virtuellen Computern bereitzustellen.

    Es werden alle virtuellen Computer am gleichen Standort angezeigt, die noch keiner Sicherungsrichtlinie zugeordnet sind. *myVMH1* und *myVMR1* wurden für die Richtlinie *Finance* ausgewählt.

    ![Auswählen der zu schützenden VMs](./media/tutorial-backup-vm-at-scale/choose-vm-to-protect.png)

1. Nachdem Sie die virtuellen Computer ausgewählt haben, wählen Sie **Sicherung aktivieren** aus.

    Wenn die Bereitstellung abgeschlossen ist, erhalten Sie eine Benachrichtigung darüber, dass die Bereitstellung erfolgreich abgeschlossen wurde.

## <a name="initial-backup"></a>Erste Sicherung

Sie haben zwar die Sicherung für die Recovery Services-Tresore aktiviert, aber noch keine erste Sicherung erstellt. Es wird für die Notfallwiederherstellung empfohlen, eine erste Sicherung auszulösen, damit Ihre Daten geschützt sind.

So führen Sie einen bedarfsgesteuerten Sicherungsauftrag aus

1. Wählen Sie im Tresordashboard die Angabe **3** unter **Sicherungselemente** aus, um das Menü „Sicherungselemente“ zu öffnen.

    ![Sicherungselemente](./media/tutorial-backup-vm-at-scale/tutorial-vm-back-up-now.png)

    Das Menü **Sicherungselemente** wird geöffnet.

1. Wählen Sie im Menü **Sicherungselemente** die Option **Virtueller Azure-Computer** aus, um die Liste der virtuellen Computer, die dem Tresor zugeordnet sind, zu öffnen.

    ![Liste der virtuellen Computer](./media/tutorial-backup-vm-at-scale/three-virtual-machines.png)

1. Die Liste **Sicherungselemente** wird geöffnet.

    ![Sicherungsauftrag ausgelöst](./media/tutorial-backup-vm-at-scale/initial-backup-context-menu.png)

1. Klicken Sie in der Liste **Sicherungselemente** auf die Auslassungszeichen ( **...** ), um das Kontextmenü zu öffnen.

1. Wählen Sie im Kontextmenü **Jetzt sichern** aus.

    ![Kontextmenü: Auswählen von „Jetzt sichern“](./media/tutorial-backup-vm-at-scale/context-menu.png)

    Das Menü „Jetzt sichern“ wird geöffnet.

1. Geben Sie im Menü „Jetzt sichern“ den letzten Tag für die Aufbewahrung des Wiederherstellungspunkts ein, und wählen Sie **Sichern** aus.

    ![Festlegen des letzten Aufbewahrungstags für den Wiederherstellungspunkt für „Jetzt sichern“](./media/tutorial-backup-vm-at-scale/backup-now-short.png)

    Anhand von Bereitstellungsbenachrichtigungen werden Sie darüber informiert, dass der Sicherungsauftrag ausgelöst wurde und dass Sie den Status des Auftrags auf der Seite „Sicherungsaufträge“ überwachen können. Je nach Größe Ihres virtuellen Computers kann das Erstellen der ersten Sicherung einige Zeit dauern.

    Nach Abschluss des Auftrags für die erste Sicherung wird sein Status im Menü „Sicherungsauftrag“ angezeigt. Der manuell ausgelöste Sicherungsauftrag hat den ersten Wiederherstellungspunkt für *myVM* erstellt. Wenn Sie weitere virtuelle Computer sichern möchten, wiederholen Sie diese Schritte für jeden virtuellen Computer.

    ![Kachel „Sicherungsaufträge“](./media/tutorial-backup-vm-at-scale/initial-backup-complete.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie planen, mit den nachfolgenden Tutorials fortzufahren, sollten Sie die in diesem Tutorial erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie die folgenden Schritte durchführen, um alle in diesem Tutorial erstellten Ressourcen im Azure-Portal zu löschen.

1. Wählen Sie im Dashboard von **myRecoveryServicesVault** die Angabe **3** unter **Sicherungselemente**aus, um das Menü „Sicherungselemente“ zu öffnen.

    ![Öffnen des Menüs „Sicherungselemente“](./media/tutorial-backup-vm-at-scale/tutorial-vm-back-up-now.png)

1. Wählen Sie im Menü **Sicherungselemente** die Option **Virtueller Azure-Computer** aus, um die Liste der virtuellen Computer, die dem Tresor zugeordnet sind, zu öffnen.

    ![Liste der virtuellen Computer](./media/tutorial-backup-vm-at-scale/three-virtual-machines.png)

    Die Liste **Sicherungselemente** wird geöffnet.

1. Wählen Sie im Menü **Sicherungselemente** die Auslassungszeichen (...) aus, um das Kontextmenü zu öffnen.

    ![Öffnen des Kontextmenüs im Menü „Sicherungselemente“](./media/tutorial-backup-vm-at-scale/context-menu-to-delete-vm.png)

1. Wählen Sie im Kontextmenü **Sicherung beenden** aus, um das Menü „Sicherung beenden“ zu öffnen.

    ![Menü „Sicherung beenden“](./media/tutorial-backup-vm-at-scale/context-menu-for-delete.png)

1. Wählen Sie im Menü **Sicherung beenden** das obere Dropdownmenü und dann **Sicherungsdaten löschen** aus.

1. Geben Sie im Dialogfeld **Geben Sie den Namen des Sicherungselements ein** den Namen *myVM* ein.

1. Nachdem das Sicherungselement überprüft wurde (ein Häkchen wird angezeigt), wird die Schaltfläche **Sicherung beenden** aktiviert. Wählen Sie **Sicherung beenden** aus, um die Richtlinie zu beenden und die Wiederherstellungspunkte zu löschen.

    ![Wählen Sie „Sicherung beenden“ aus, um den Tresor zu löschen.](./media/tutorial-backup-vm-at-scale/provide-reason-for-delete.png)

    >[!NOTE]
    >Gelöschte Elemente werden 14 Tage lang im Zustand „vorläufig gelöscht“ beibehalten. Erst nach diesem Zeitraum kann der Tresor gelöscht werden. Weitere Informationen finden Sie unter [Löschen eines Azure Backup Recovery Services-Tresors](backup-azure-delete-vault.md).

1. Wählen Sie **Löschen** aus, wenn im Tresor keine Elemente mehr vorhanden sind.

    ![Auswählen von „Löschen“](./media/tutorial-backup-vm-at-scale/deleting-the-vault.png)

    Nachdem der Tresor gelöscht wurde, wird erneut die Liste der Recovery Services-Tresore angezeigt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie das Azure-Portal zu folgenden Zwecken verwendet:

> [!div class="checklist"]
>
> * Erstellen eines Recovery Services-Tresors
> * Festlegen des Tresors zum Schutz von virtuellen Computern
> * Erstellen einer benutzerdefinierte Sicherungs- und Aufbewahrungsrichtlinie
> * Zuweisen der Sicherungsrichtlinie zum Schützen mehrerer virtueller Computer
> * Auslösen einer bedarfsabhängigen Sicherung für virtuelle Computer

Im nächsten Tutorial stellen Sie einen virtuellen Azure-Computer von einem Datenträger wieder her.

> [!div class="nextstepaction"]
> [Verwalten von virtuellen Computern mithilfe der Befehlszeilenschnittstelle](./tutorial-restore-disk.md)
