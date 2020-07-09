---
title: Aktivieren von „VMs außerhalb der Geschäftszeiten starten/beenden“ in Azure Automation
description: In diesem Artikel erfahren Sie, wie Sie die Funktion „VMs außerhalb der Geschäftszeiten starten/beenden“ für Ihre Azure-VMs aktivieren.
services: automation
ms.subservice: process-automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: dde2c3e4cf496bb15ca91c72d9a41936af7051c5
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83743757"
---
# <a name="enable-startstop-vms-during-off-hours"></a>Aktivieren von „VMs außerhalb der Geschäftszeiten starten/beenden“

Führen Sie die Schritte in der vorliegenden Reihenfolge in diesem Thema aus, um die Funktion „VMs außerhalb der Geschäftszeiten starten/beenden“ für VMs mithilfe eines neuen oder vorhandenen Automation-Kontos sowie eines verknüpften Log Analytics-Arbeitsbereichs zu aktivieren. Nachdem Sie den Einrichtungsprozess abgeschlossen haben, konfigurieren Sie die Variablen zum Anpassen der Funktion.

>[!NOTE]
>Wenn Sie diese Funktion mit klassischen VMs verwenden möchten, benötigen Sie ein klassisches ausführendes Konto. Dieses wird nicht standardmäßig erstellt. Siehe [Erstellen eines klassischen ausführenden Kontos](automation-create-standalone-account.md#create-a-classic-run-as-account).
>

## <a name="create-resources-for-the-feature"></a>Erstellen von Ressourcen für die Funktion

1. Melden Sie sich beim Azure-[Portal](https://portal.azure.com) an.
2. Suchen Sie nach **Automation-Konten**, und wählen Sie diese Option aus.
3. Wählen Sie auf der Seite „Automation-Konten“ in der entsprechenden Liste Ihr Automation-Konto aus.
4. Wählen Sie in dem Automation-Konto unter **Verwandtes Ressourcen** die Option **VM starten/beenden** aus. Dort können Sie **Weitere Informationen anzeigen und Lösung aktivieren** anklicken. Wenn Sie die Funktion bereits bereitgestellt haben, können Sie auf **Lösung verwalten** klicken, und sie wird in der Liste angezeigt.

   ![Aktivieren über ein Automation-Konto](./media/automation-solution-vm-management/enable-from-automation-account.png)

   > [!NOTE]
   > Sie können die Ressource auch von überall im Azure-Portal erstellen, indem Sie auf **Ressource erstellen** klicken. Geben Sie auf der Seite „Marketplace“ ein Schlüsselwort ein, z.B. **Starten** oder **Starten/Beenden**. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Alternativ hierzu können Sie ein oder mehrere Schlüsselwörter des vollständigen Namens der Funktion eingeben und dann die **EINGABETASTE** drücken. Wählen Sie in den Suchergebnissen **VMs außerhalb der Geschäftszeiten starten/beenden** aus.

5. Überprüfen Sie auf der Seite „VMs außerhalb der Geschäftszeiten starten/beenden“ die Zusammenfassung der ausgewählten Bereitstellung, und klicken Sie dann auf **Erstellen**.

   ![Azure-Portal](media/automation-solution-vm-management/azure-portal-01.png)

## <a name="configure-the-feature"></a>Konfigurieren der Funktion

Nachdem die Ressource erstellt wurde, wird die Seite „Lösung hinzufügen“ angezeigt. Sie werden aufgefordert, die Funktion vor dem Importieren in Ihr Automation-Abonnement zu konfigurieren. Siehe [Konfigurieren von „VMs außerhalb der Geschäftszeiten starten/beenden“](automation-solution-vm-management-config.md).

   ![Seite „Lösung hinzufügen“ der VM-Verwaltung](media/automation-solution-vm-management/azure-portal-add-solution-01.png)

## <a name="select-a-log-analytics-workspace"></a>Auswählen eines Log Analytics-Arbeitsbereichs

1. Wählen Sie auf der Seite „Lösung hinzufügen“ die Option **Arbeitsbereich** aus. Wählen Sie einen Log Analytics-Arbeitsbereich aus, der mit dem Azure-Abonnement verknüpft ist, das vom Automation-Konto verwendet wird. 

2. Wählen Sie die Option **Neuen Arbeitsbereich erstellen**, wenn kein Arbeitsbereich vorhanden ist. Führen Sie auf der Seite „Log Analytics-Arbeitsbereich“ die folgenden Schritte aus:

   - Geben Sie einen Namen für den neuen Log Analytics-Arbeitsbereich an, z. B. **ContosoLAWorkspace**.
   - Wählen Sie ein **Abonnement** aus, mit dem eine Verknüpfung erstellt werden soll, indem Sie in der Dropdownliste einen anderen Eintrag auswählen, falls der Standardeintrag nicht geeignet ist.
   - Unter **Ressourcengruppe** können Sie eine neue Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe auswählen.
   - Wählen Sie einen **Speicherort**aus.
   - Wählen Sie einen **Tarif**aus. Wählen Sie die Option **Pro GB (eigenständig)** aus. Für Azure Monitor-Protokolle wurden die [Preise](https://azure.microsoft.com/pricing/details/log-analytics/) aktualisiert, und der Tarif „Pro GB“ ist die einzige Option.

   > [!NOTE]
   > Wenn Sie Funktionen aktivieren, werden nur bestimmte Regionen zum Verknüpfen mit einem Log Analytics-Arbeitsbereich und einem Automation-Konto unterstützt. Eine Liste der unterstützten Zuordnungspaare finden Sie unter [Regionszuordnung für Automation-Konto und Log Analytics-Arbeitsbereich](how-to/region-mappings.md).

3. Klicken Sie nach dem Bereitstellen der erforderlichen Informationen auf der Seite „Log Analytics-Arbeitsbereich“ auf **Erstellen**. Sie können den Status unter **Benachrichtigungen** über das Menü nachverfolgen, und nach Abschluss des Vorgangs gelangen Sie zurück zur Seite „Lösung hinzufügen“.

## <a name="add-automation-account"></a>Automation-Konto hinzufügen

Greifen Sie erneut auf die Seite „Lösung hinzufügen“ zu, und wählen Sie **Automation-Konto** aus. Sie können ein vorhandenes Automation-Konto auswählen, das noch mit keinem Log Analytics-Arbeitsbereich verknüpft ist. Wenn Sie einen neuen Log Analytics-Arbeitsbereich erstellen, können Sie ein neues Automation-Konto erstellen, um es dem Arbeitsbereich zuzuordnen. Wählen Sie ein vorhandenes Automation-Konto aus, oder klicken Sie auf **Automation-Konto erstellen**, und geben Sie auf der Seite „Automation-Konto hinzufügen“ den Namen des Automation-Kontos im Feld **Name** an.

Alle anderen Optionen werden basierend auf dem Log Analytics-Arbeitsbereich automatisch aufgefüllt. Sie können diese Optionen nicht ändern. Ein ausführendes Azure-Konto ist die Standardmethode für die Authentifizierung von Runbooks mit dieser Funktion. 

Nach dem Klicken auf **OK** werden die Konfigurationsoptionen überprüft, und das Automation-Konto wird erstellt. Sie können den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen.

## <a name="define-feature-parameters"></a>Definieren von Funktionsparametern

1. Wählen Sie auf der Seite „Lösung hinzufügen“ die Option **Konfiguration** aus. Die Seite „Parameter“ wird angezeigt.

    ![Seite „Parameter“ für die Lösung](media/automation-solution-vm-management/azure-portal-add-solution-02.png)

2. Geben Sie einen Wert für das Feld **ResourceGroup-Zielnamen** an. Das-Feld definiert Gruppennamen, die VMs enthalten, die von der Funktion verwaltet werden sollen. Sie können mehrere Namen eingeben und die Namen jeweils durch ein Komma trennen (Groß-/Kleinschreibung wird nicht berücksichtigt). Die Verwendung eines Platzhalterzeichens wird unterstützt, wenn Sie einen Vorgang für VMs in allen Ressourcengruppen des Abonnements durchführen möchten. Die Werte werden in den Variablen `External_Start_ResourceGroupNames` und `External_Stop_ResourceGroupNames` gespeichert.

    > [!IMPORTANT]
    > Der Standardwert für **ResourceGroup-Zielnamen** ist ein **&ast;** . Diese Einstellung betrifft alle VMs in einem Abonnement. Wenn Sie nicht möchten, dass die Funktion für alle VMs in Ihrem Abonnement gilt, müssen Sie vor der Auswahl eines Zeitplans eine Liste mit Ressourcengruppennamen angeben.
  
3. Geben Sie einen Wert für das Feld **VM-Ausschlussliste (Zeichenfolge)** an. Dies ist der Wert von einem oder mehreren virtuellen Computern der Zielressourcengruppe. Sie können mehrere Namen eingeben und die Namen jeweils durch ein Komma trennen (Groß-/Kleinschreibung wird nicht berücksichtigt). Platzhalter können verwendet werden. Dieser Wert wird in der Variablen `External_ExcludeVMNames` gespeichert.
  
4. Verwenden Sie das Feld **Zeitplan**, um einen Zeitplan für die VM-Verwaltung durch die Funktion auszuwählen. Wählen Sie ein Startdatum und eine Uhrzeit für Ihren Zeitplan aus, um einen sich täglich wiederholenden Zeitplan zu erstellen, der zur ausgewählten Uhrzeit startet. Es kann keine andere Region ausgewählt werden. Falls Sie den Zeitplan nach dem Konfigurieren der Funktion für Ihre Zeitzone konfigurieren möchten, helfen Ihnen die Informationen unter [Ändern des Zeitplans für das Starten und Herunterfahren](automation-solution-vm-management-config.md#modify-the-startup-and-shutdown-schedules) weiter.

5. Um E-Mail-Benachrichtigungen von einer [Aktionsgruppe](../azure-monitor/platform/action-groups.md) zu empfangen, übernehmen Sie den Standardwert **Ja** im Feld **E-Mail-Adresse**, und geben Sie eine gültige E-Mail-Adresse an. Wenn Sie **Nein** auswählen, sich aber zu einem späteren Zeitpunkt entscheiden, dass Sie E-Mail-Benachrichtigungen erhalten möchten, können Sie die erstellte Aktionsgruppe mit gültigen E-Mail-Adressen, die durch Kommas getrennt sind, aktualisieren. 

6. Aktivieren Sie die folgenden Warnungsregeln:

   - `AutoStop_VM_Child`
   - `Scheduled_StartStop_Parent`
   - `Sequenced_StartStop_Parent`

## <a name="create-alerts"></a>Erstellen von Warnungen

„VMs außerhalb der Geschäftszeiten starten/beenden“ umfasst keinen vordefinierten Satz von Warnungen. Informationen zum Erstellen von Warnungen bei fehlgeschlagenen Aufträgen zur Unterstützung Ihrer DevOps oder Betriebsprozesse und -verfahren finden Sie unter [Erstellen von Protokollwarnungen mit Azure Monitor](../azure-monitor/platform/alerts-log.md).

## <a name="deploy-the-feature"></a>Bereitstellen der Funktion

1. Nachdem Sie die erforderlichen Anfangseinstellungen für die Funktion konfiguriert haben, klicken Sie auf **OK**, um die Seite „Parameter“ zu schließen.

2. Klicken Sie auf **Erstellen**. Nachdem alle Einstellungen überprüft wurden, wird die Funktion in Ihrem Abonnement bereitgestellt. Dieser Vorgang kann einige Sekunden dauern, und Sie können den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen.

    > [!NOTE]
    > Wenn Sie ein Abonnement für Azure Cloud Solution Provider (Azure CSP) besitzen, wechseln Sie nach Abschluss der Bereitstellung in Ihrem Automation-Konto unter **Freigegebene Ressourcen** zu **Variablen**, und legen Sie die Variable [External_EnableClassicVMs](automation-solution-vm-management.md#variables) auf **False** fest. Dadurch hört die Lösung auf, nach klassischen VM-Ressourcen zu suchen.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Einrichten des Features finden Sie unter [Konfigurieren von „VMs außerhalb der Geschäftszeiten starten/beenden“](automation-solution-vm-management-config.md).
* Informationen zum Beheben von Problemen mit dem Feature finden Sie unter [Problembehandlung von „VMs außerhalb der Geschäftszeiten starten/beenden“](troubleshoot/start-stop-vm.md).