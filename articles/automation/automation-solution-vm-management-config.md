---
title: 'Azure Automation: Konfigurieren des Startens/Beendens von VMs außerhalb der Geschäftszeiten'
description: In diesem Artikel wird beschrieben, wie Sie die Lösung „VMs außerhalb der Geschäftszeiten starten/beenden“ so konfigurieren, das sie unterschiedliche Anwendungsfälle oder Szenarien unterstützt.
services: automation
ms.subservice: process-automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: 241866129aa36d67af18fab86a26d9cbf7ce42d6
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82582456"
---
# <a name="how-to-configure-startstop-vms-during-off-hours-solution"></a>Konfigurieren der Lösung „VMs außerhalb der Geschäftszeiten starten/beenden“

Mit der Lösung **VMs außerhalb der Geschäftszeiten starten/beenden** können Sie Folgendes tun:

- [Starten und Beenden von VMs nach Zeitplan](#schedule).
- Starten und Beenden von VMs nach Zeitplan in aufsteigender Reihenfolge [mithilfe von Azure-Tags](#tags) (wird für klassische VMs nicht unterstützt).
- Automatisches Beenden von VMs bei [geringer CPU-Auslastung](#cpuutil).

In diesem Artikel wird beschrieben, wie Sie die Lösung erfolgreich für die Unterstützung dieser Szenarien konfigurieren. Sie erfahren auch, wie Sie andere gängige Konfigurationseinstellungen für die Lösung vornehmen, z. B.:

* [Konfigurieren von E-Mail-Benachrichtigungen](#configure-email-notifications)

* [Hinzufügen eines virtuellen Computers](#add-a-vm)

* [Ausschließen eines virtuellen Computers](#exclude-a-vm)

* [Ändern der Zeitpläne für das Starten und Herunterfahren](#modify-the-startup-and-shutdown-schedules)

## <a name="scenario-1-startstop-vms-on-a-schedule"></a><a name="schedule"></a>Szenario 1: Starten/Beenden von VMs nach Zeitplan

Dieses Szenario ist die Standardkonfiguration bei der ersten Bereitstellung der Lösung. Beispielsweise können Sie sie so konfigurieren, dass alle VMs in einem Abonnement zum Feierabend beendet und am Morgen wieder gestartet werden, wenn Sie wieder ins Büro kommen. Wenn Sie die Zeitpläne **Scheduled-StartVM** und **Scheduled-StopVM** während der Bereitstellung konfigurieren, werden Ziel-VMs nach diesen Zeitplänen gestartet und beendet. Die Lösung kann auch so konfiguriert werden, dass VMs nur beendet werden. Informationen zum Konfigurieren eines benutzerdefinierten Zeitplans finden Sie unter [Ändern der Zeitpläne für das Starten und Herunterfahren](#modify-the-startup-and-shutdown-schedules).

> [!NOTE]
> Die Zeitzone entspricht Ihrer aktuellen Zeitzone zum Zeitpunkt der Konfiguration des Parameters für den geplanten Zeitpunkt. Sie wird in Azure Automation aber im UTC-Format gespeichert. Sie müssen keine Zeitzonenkonvertierung durchführen, da dies während der Bereitstellung verarbeitet wird.

Sie bestimmen, welche VMs im Geltungsbereich liegen, indem Sie die folgenden Variablen konfigurieren: **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames** und **External_ExcludeVMNames**.

Sie können die Aktion entweder für ein Abonnement und eine Ressourcengruppe oder für eine spezifische Liste von VMs aktivieren, aber nicht für beides.

### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Festlegen der Aktionen zum Starten und Beenden für ein Abonnement und eine Ressourcengruppe

1. Konfigurieren Sie die Variablen `External_Stop_ResourceGroupNames` und `External_ExcludeVMNames`, um die Ziel-VMs anzugeben.

2. Aktivieren und aktualisieren Sie die Zeitpläne **Scheduled-StartVM** und **Scheduled-StopVM**.

3. Führen Sie das Runbook **ScheduledStartStop_Parent** aus. Legen Sie dabei das Parameterfeld **ACTION** auf **start** und das Parameterfeld **WHATIF** auf TRUE fest, um für die Änderungen eine Vorschau anzuzeigen.

### <a name="target-the-start-and-stop-action-by-vm-list"></a>Festlegen der Aktionen zum Starten und Beenden über eine VM-Liste

1. Führen Sie das Runbook **ScheduledStartStop_Parent** aus. Legen Sie dabei **ACTION** auf **start** fest. Fügen Sie eine durch Trennzeichen getrennte Liste von VMs im Parameterfeld **VMList** hinzu, und legen Sie das Parameterfeld **WHATIF** auf TRUE fest. Zeigen Sie eine Vorschau für die Änderungen an.

2. Konfigurieren Sie die Variable `External_ExcludeVMNames` mit einer durch Kommas getrennten Liste von VMs (VM1, VM2, VM3).

3. In diesem Szenario werden die Variablen `External_Start_ResourceGroupNames` und `External_Stop_ResourceGroupnames` nicht berücksichtigt. Für dieses Szenario müssen Sie Ihren eigenen Automation-Zeitplan erstellen. Ausführliche Informationen finden Sie unter [Planen eines Runbooks in Azure Automation](../automation/automation-schedules.md).

    > [!NOTE]
    > Der Wert für **ResourceGroup-Zielnamen** wird als der Wert für sowohl `External_Start_ResourceGroupNames` aus auch `External_Stop_ResourceGroupNames` gespeichert. Für weitere Granularität können Sie jede dieser Variablen so ändern, dass sie für unterschiedliche Ressourcengruppen gelten. Verwenden Sie für Startaktion `External_Start_ResourceGroupNames` und für die Beendigungsaktion `External_Stop_ResourceGroupNames`. VMs werden automatisch den Zeitplänen zum Starten und Beenden hinzugefügt.

## <a name="scenario-2-startstop-vms-in-sequence-by-using-tags"></a><a name="tags"></a>Szenario 2: Starten/Beenden von VMs der Reihe nach mithilfe von Tags

In einer Umgebung mit mehreren Komponenten auf mehreren VMs, die eine verteilte Workload unterstützen, spielt die Reihenfolge, in der Komponenten nacheinander gestartet und beendet werden, eine wichtige Rolle. Führen Sie zum Erreichen dieses Szenarios die folgenden Schritte aus:

### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Festlegen der Aktionen zum Starten und Beenden für ein Abonnement und eine Ressourcengruppe

1. Fügen Sie die Tags `sequencestart` und `sequencestop` mit einem positiven ganzzahligen Wert zu VMs hinzu, die das Ziel der Variablen `External_Start_ResourceGroupNames` und `External_Stop_ResourceGroupNames` sind. Die Aktionen zum Starten und Beenden werden in aufsteigender Reihenfolge durchgeführt. Informationen zum Markieren von VMs finden Sie unter [Markieren eines virtuellen Windows-Computers in Azure](../virtual-machines/windows/tag.md) und [Markieren eines virtuellen Linux-Computers in Azure](../virtual-machines/linux/tag.md).

2. Ändern Sie die Zeitpläne **Sequenced-StartVM** und **Sequenced-StopVM** in das Datum und den Zeitpunkt, die Ihren Anforderungen entsprechen, und aktivieren Sie die Zeitpläne.

3. Führen Sie das Runbook **SequencedStartStop_Parent** aus. Legen Sie dabei **ACTION** auf **start** und **WHATIF** auf TRUE fest, um für die Änderungen eine Vorschau anzuzeigen.

4. Zeigen Sie für die Aktion eine Vorschau an, und nehmen Sie alle erforderlichen Änderungen vor, bevor Sie die Implementierung für Produktions-VMs durchführen. Führen Sie, wenn Sie soweit sind, das Runbook manuell aus, wobei Sie den Parameter auf **False** festlegen. Sie können alternativ die Automation-Zeitpläne `Sequenced-StartVM` und `Sequenced-StopVM` entsprechend Ihrem vorgegebenen Zeitplan automatisch ausführen lassen.

### <a name="target-the-start-and-stop-action-by-vm-list"></a>Festlegen der Aktionen zum Starten und Beenden über eine VM-Liste

1. Fügen Sie den VMs, die Sie dem Parameter `VMList` hinzufügen möchten, die Tags `sequencestart` und `sequencestop` mit einem positiven ganzzahligen Wert hinzu.

2. Führen Sie das Runbook **SequencedStartStop_Parent** aus. Legen Sie dabei **ACTION** auf **start** fest. Fügen Sie eine durch Trennzeichen getrennte Liste von VMs im Parameterfeld **VMList** hinzu, und legen Sie **WHATIF** auf TRUE fest. Zeigen Sie eine Vorschau für die Änderungen an.

3. Konfigurieren Sie die Variable `External_ExcludeVMNames` mit einer durch Kommas getrennten Liste von VMs (VM1, VM2, VM3).

4. In diesem Szenario werden die Variablen `External_Start_ResourceGroupNames` und `External_Stop_ResourceGroupnames` nicht berücksichtigt. Für dieses Szenario müssen Sie Ihren eigenen Automation-Zeitplan erstellen. Ausführliche Informationen finden Sie unter [Planen eines Runbooks in Azure Automation](../automation/automation-schedules.md).

5. Zeigen Sie für die Aktion eine Vorschau an, und nehmen Sie alle erforderlichen Änderungen vor, bevor Sie die Implementierung für Produktions-VMs durchführen. Wenn Sie soweit sind, führen Sie das **monitoring-and-diagnostics/monitoring-action-groupsrunbook** mit dem auf **False** festgelegten Parameter manuell aus. Alternativ können Sie den Automation-Zeitplan `Sequenced-StartVM` und `Sequenced-StopVM` gemäß dem vorgegebenen Zeitplan automatisch ausführen lassen.

## <a name="scenario-3-startstop-automatically-based-on-cpu-utilization"></a><a name="cpuutil"></a>Szenario 3: Automatisches Starten/Beenden basierend auf der CPU-Auslastung

Diese Lösung kann einen Beitrag zur Verwaltung der Kosten für die Ausführung von Azure Resource Manager- und klassischen virtuellen Computern in Ihrem Abonnement leisten, indem VMs, die außerhalb der Spitzenzeiten (z. B. nach Geschäftsschluss) nicht verwendet werden, ausgewertet und automatisch beendet werden, wenn die Prozessorauslastung unter einem angegebenen Prozentsatz liegt.

Standardmäßig ist die Lösung so vorkonfiguriert, dass die Metrik zur CPU-Auslastung in Prozent ausgewertet wird, um zu ermitteln, ob die durchschnittliche Auslastung kleiner oder gleich 5% ist. Dieses Szenario wird mit den folgenden Variablen gesteuert und kann geändert werden, wenn die Standardwerte nicht Ihren Anforderungen entsprechen:

* `External_AutoStop_MetricName`
* `External_AutoStop_Threshold`
* `External_AutoStop_TimeAggregationOperator`
* `External_AutoStop_TimeWindow`
* `External_AutoStop_Frequency`
* `External_AutoStop_Severity`

Sie können die Aktion für ein Abonnement und eine Ressourcengruppe oder für eine spezifische Liste von VMs aktivieren und darauf zielgerichtet anwenden.

Wenn Sie das **AutoStop_CreateAlert_Parent**-Runbook ausführen, überprüft es, ob das Zielabonnement, die Zielressourcengruppe(n) und die Ziel-VMs vorhanden sind. Wenn die VMs vorhanden sind, ruft das Runbook dann das Runbook **AutoStop_CreateAlert_Child** für jede vom übergeordneten Runbook überprüfte VM auf. Dieses untergeordnete Runbook führt Folgendes aus:

* Erstellt eine Metrikwarnungsregel für jede überprüfte VM.

* Löst das Runbook **AutoStop_VM_Child** für eine bestimmte VM aus, wenn die CPU-Auslastung unter den konfigurierten Schwellenwert für das angegebene Zeitintervall fällt. Dieses Runbook versucht dann, den virtuellen Computer zu beenden.

### <a name="to-target-the-auto-stop-action-against-all-vms-in-a-subscription"></a>So legen Sie die automatische Beendigungsaktion für alle VMs in einem Abonnement fest

1. Stellen Sie sicher, dass die Variable `External_Stop_ResourceGroupNames` leer oder auf „*“ (Platzhalterzeichen) festgelegt ist.

2. [Optionaler Schritt] Wenn Sie einige virtuelle Computer vom automatischen Herunterfahren ausnehmen möchten, können Sie der Variablen `External_ExcludeVMNames` eine durch Trennzeichen getrennte Liste von VM-Namen hinzufügen.

3. Aktivieren Sie die Ausführung des Zeitplans `Schedule_AutoStop_CreateAlert_Parent`, um die erforderlichen Metrikwarnungsregeln für das Beenden von VMs für alle VMs in Ihrem Abonnement zu erstellen. Durch Ausführen dieses Zeitplantyps können Sie neue Metrikwarnungsregeln erstellen, sobald dem Abonnement neue VMs hinzugefügt werden.

### <a name="to-target-the-auto-stop-action-against-all-vms-in-a-resource-group-or-multiple-resource-groups"></a>So legen Sie die automatische Beendigungsaktion für alle VMs in einer oder mehreren Ressourcengruppen fest

1. Fügen Sie der Variablen `External_Stop_ResourceGroupNames` eine durch Trennzeichen getrennte Liste von Ressourcengruppennamen hinzu.

2. Wenn Sie einige virtuelle Computer vom automatischen Herunterfahren ausnehmen möchten, können Sie der Variablen `External_ExcludeVMNames` eine durch Trennzeichen getrennte Liste von VM-Namen hinzufügen.

3. Aktivieren Sie die Ausführung des Zeitplans **Schedule_AutoStop_CreateAlert_Parent**, um die erforderlichen Metrikwarnungsregeln für das Beenden von VMs für alle VMs in Ihren Ressourcengruppen zu erstellen. Wenn Sie diesen Vorgang nach einem Zeitplan ausführen, können Sie neue Metrikwarnungsregeln erstellen, wenn Ressourcengruppen neue VMs hinzugefügt werden.

### <a name="to-target-the-autostop-action-to-a-list-of-vms"></a>So legen Sie die automatische Beendigungsaktion für eine Liste von VMs fest

1. Erstellen Sie einen neuen [Zeitplan](shared-resources/schedules.md#create-a-schedule), und verknüpfen Sie ihn mit dem **AutoStop_CreateAlert_Parent**-Runbook, wobei Sie dem Parameter `VMList` eine durch Trennzeichen getrennte Liste von VM-Namen hinzufügen.

2. Wenn Sie optional einige VMs vom automatischen Herunterfahren ausnehmen möchten, können Sie der Variablen `External_ExcludeVMNames` eine durch Trennzeichen getrennte Liste von VM-Namen hinzufügen.

## <a name="configure-email-notifications"></a>Konfigurieren von E-Mail-Benachrichtigungen

Um E-Mail-Benachrichtigungen nach dem Bereitstellen der Lösung zu ändern, bearbeiten Sie die Aktionsgruppe, die während der Bereitstellung erstellt wurde.  

> [!NOTE]
> Abonnements in der Azure Government-Cloud unterstützen die E-Mail-Funktionalität dieser Lösung nicht.

1. Navigieren Sie im Azure-Portal zu **Überwachung** > **Aktionsgruppen**. Wählen Sie die Aktionsgruppe **StartStop_VM_Notication** aus.

    ![Seite der Automation-Lösung „Updateverwaltung“](media/automation-solution-vm-management/azure-monitor.png)

2. Klicken Sie auf der Seite **StartStop_VM_Notification** unter **Details** auf **Details bearbeiten**. Dadurch wird die Seite **Email/SMS/Push/Voice** (E-Mail/SMS/Push/Sprache) geöffnet. Aktualisieren Sie die E-Mail-Adresse, und klicken Sie auf **OK**, um Ihre Änderungen zu speichern.

    ![Seite der Automation-Lösung „Updateverwaltung“](media/automation-solution-vm-management/change-email.png)

    Alternativ können Sie der Aktionsgruppe weitere Aktionen hinzufügen. Weitere Informationen über Aktionsgruppen finden Sie unter [Aktionsgruppen](../azure-monitor/platform/action-groups.md)

Die folgende Beispiel-E-Mail wird gesendet, wenn die Lösung virtuelle Computer herunterfährt.

![Seite der Automation-Lösung „Updateverwaltung“](media/automation-solution-vm-management/email.png)

## <a name="addexclude-vms"></a><a name="add-exclude-vms"></a>Hinzufügen/Ausschließen von VMs

Die Lösung bietet die Möglichkeit, VMs hinzuzufügen, auf die die Lösung ausgerichtet ist, oder Computer gezielt von der Lösung auszuschließen.

### <a name="add-a-vm"></a>Hinzufügen eines virtuellen Computers

Es stehen zwei Optionen zur Verfügung, mit denen Sie sicherstellen können, dass eine VM bei ihrer Ausführung in die Lösung zum Starten/Beenden aufgenommen wird.

* Jedes der übergeordneten [Runbooks](automation-solution-vm-management.md#runbooks) der Lösung weist einen `VMList`-Parameter auf. Sie können diesem Parameter eine durch Komma getrennte Liste von VM-Namen übergeben, wenn Sie das für Ihre Situation geeignete übergeordnete Runbook planen, und diese VMs beim Ausführen der Lösung berücksichtigt werden.

* Um mehrere VMs auszuwählen, legen Sie für `External_Start_ResourceGroupNames` und `External_Stop_ResourceGroupNames` die Namen der Ressourcengruppen fest, die die VMs enthalten, die Sie starten oder beenden möchten. Sie können die Variablen auch auf den Wert `*` festlegen, damit die Lösung für alle Ressourcengruppen im Abonnement ausgeführt wird.

### <a name="exclude-a-vm"></a>Ausschließen eines virtuellen Computers

Wenn Sie eine VM von der Lösung ausschließen möchten, können Sie sie der Variablen `External_ExcludeVMNames` hinzufügen. Diese Variable ist eine durch Trennzeichen getrennte Liste von bestimmten VMs, die von der Lösung zum Starten/Beenden ausgeschlossen werden sollen. Diese Liste ist auf 140 virtuelle Computer beschränkt. Wenn Sie dieser durch Trennzeichen getrennten Liste mehr als 140 VMs hinzufügen, kann es bei VMs, für die Ausschluss festgelegt ist, zu unbeabsichtigtem Starten oder Beenden kommen.

## <a name="modify-the-startup-and-shutdown-schedules"></a>Ändern der Zeitpläne für das Starten und Herunterfahren

Bei der Verwaltung der Zeitpläne für das Starten und Herunterfahren werden bei dieser Lösung die gleichen Schritte ausgeführt, die unter [Planen eines Runbooks in Azure Automation](automation-schedules.md) beschrieben werden. Es muss einen separaten Zeitplan für das Starten und Beenden von VMs geben.

Die Lösung kann auch so konfiguriert werden, dass VMs zu einem bestimmten Zeitpunkt beendet werden. In diesem Szenario erstellen Sie nur einen Zeitplan für **Beenden** und keinen entsprechenden Zeitplan für **Starten**. Gehen Sie hierzu wie folgt vor:

1. Vergewissern Sie sich, dass Sie die Ressourcengruppen für die zu beendenden VMs in der Variablen `External_Stop_ResourceGroupNames` hinzugefügt haben.

2. Erstellen Sie Ihren eigenen Zeitplan für den Zeitpunkt, an dem die VMs heruntergefahren werden sollen.

3. Navigieren Sie zum Runbook **ScheduledStartStop_Parent**, und klicken Sie auf **Zeitplan**. Dadurch können Sie den Zeitplan auswählen, den Sie im vorherigen Schritt erstellt haben.

4. Wählen Sie **Parameter und Ausführungseinstellungen** aus, und legen Sie das Feld **ACTION** auf **Stop** fest.

5. Klicken Sie zum Speichern der Änderungen auf **OK** .

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Behandlung von Problemen mit „VMs außerhalb der Geschäftszeiten starten/beenden“ finden Sie unter [Problembehandlung beim Starten/Beenden von VMs](troubleshoot/start-stop-vm.md).

* [Überprüfen Sie](automation-solution-vm-management-logs.md) die in Azure Monitor-Protokolle geschriebenen Automation-Datensätze und die Beispielprotokollsuchabfragen, um den Status von Automation-Runbookaufträgen von virtuellen Computern mit Starten/Beenden analysieren.
