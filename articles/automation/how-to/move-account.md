---
title: Verschieben Ihres Azure Automation-Kontos in ein anderes Abonnement
description: In diesem Artikel erfahren Sie, wie Sie Ihr Automation-Konto in ein anderes Abonnement verschieben.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/11/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 562ea5e0e9e4851ed59bd3ef917be2f9c48cd2a7
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86185550"
---
# <a name="move-your-azure-automation-account-to-another-subscription"></a>Verschieben Ihres Azure Automation-Kontos in ein anderes Abonnement

In Azure Automation können Sie einige Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement verschieben. Sie können Ressourcen mit dem Azure-Portal, PowerShell, der Azure CLI oder der REST-API verschieben. Weitere Informationen zum Prozess finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../../azure-resource-manager/management/move-resource-group-and-subscription.md).

Das Automation-Konto ist eine der Ressourcen, die Sie verschieben können. In diesem Artikel erfahren Sie, wie Sie Automation-Konten in eine andere Ressource oder in ein anderes Abonnement verschieben. Die groben Schritte zum Verschieben Ihres Automation-Kontos sind:

1. Deaktivieren Sie Ihre Features.
2. Heben Sie die Verknüpfung mit Ihrem Arbeitsbereich auf.
3. Verschieben Sie das Automation-Konto.
4. Löschen Sie die ausführenden Konten, und erstellen Sie sie neu.
5. Reaktivieren Sie Ihre Features.

## <a name="remove-features"></a>Entfernen von Features

Um die Verknüpfung Ihres Arbeitsbereichs mit Ihrem Automation-Konto aufzuheben, müssen Sie die Featureressourcen in Ihrem Arbeitsbereich entfernen:

- Änderungsnachverfolgung und Bestand
- Updateverwaltung
- Starten/Beenden von VMs außerhalb der Kernzeit

1. Navigieren Sie im Azure-Portal zu Ihrer Ressourcengruppe.
2. Navigieren Sie zum jeweiligen Feature, und wählen Sie auf der Seite **Ressourcen löschen** die Option **Löschen** aus.

    ![Screenshot: Löschen von Featureressourcen über das Azure-Portal](../media/move-account/delete-solutions.png)

Alternativ können die Ressourcen auch mithilfe des Cmdlets [Remove-AzResource](/powershell/module/Az.Resources/Remove-AzResource?view=azps-3.7.0) gelöscht werden:

```azurepowershell-interactive
$workspaceName = <myWorkspaceName>
$resourceGroupName = <myResourceGroup>
Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "ChangeTracking($workspaceName)" -ResourceGroupName $resourceGroupName
Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Updates($workspaceName)" -ResourceGroupName $resourceGroupName
Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Start-Stop-VM($workspaceName)" -ResourceGroupName $resourceGroupName
```

### <a name="remove-alert-rules-for-startstop-vms-during-off-hours"></a>Entfernen von Warnungsregeln für „VMs außerhalb der Geschäftszeiten starten/beenden“

Für „VMs außerhalb der Geschäftszeiten starten/beenden“ müssen auch die durch das Feature erstellten Warnungsregeln entfernt werden.

1. Navigieren Sie im Azure-Portal zu Ihrer Ressourcengruppe, und wählen Sie **Überwachung** > **Warnungen** > **Warnungsregeln verwalten**.

   ![Screenshot: Seite „Warnungen“ mit hervorgehobener Option „Warnungsregeln verwalten“](../media/move-account/alert-rules.png)

2. Auf der Seite „Regeln“ sollte eine Liste mit den Warnungen angezeigt werden, die in dieser Ressourcengruppe konfiguriert sind. Durch das Feature werden folgende Regeln erstellt:

    * AutoStop_VM_Child
    * ScheduledStartStop_Parent
    * SequencedStartStop_Parent

3. Wählen Sie die Regeln einzeln aus, und wählen Sie **Löschen** aus, um sie zu entfernen.

    ![Screenshot: Seite „Regeln“ mit Anforderung zum Bestätigen des Löschvorgangs für ausgewählte Regeln](../media/move-account/delete-rules.png)

    > [!NOTE]
    > Sollten auf der Seite „Regeln“ keine Warnungsregeln angezeigt werden, ändern Sie das Feld **Status** in **Deaktiviert**, um deaktivierte Warnungen anzuzeigen. 

4. Wenn Sie die Warnungsregeln entfernen, müssen Sie auch die Aktionsgruppe entfernen, die für Benachrichtigungen von „VMs außerhalb der Geschäftszeiten starten/beenden“ erstellt wurde. Wählen Sie im Azure-Portal die Optionen **Überwachung** > **Warnungen** > **Aktionsgruppen verwalten**.

5. Wählen Sie **StartStop_VM_Notification** aus. 

6. Wählen Sie auf der Seite „Aktionsgruppe“ die Option **Löschen** aus.

    ![Screenshot: Seite „Aktionsgruppe“](../media/move-account/delete-action-group.png)

Alternativ kann die Aktionsgruppe auch mithilfe des Cmdlets [Remove-AzActionGroup](/powershell/module/az.monitor/remove-azactiongroup?view=azps-3.7.0) gelöscht werden:

```azurepowershell-interactive
Remove-AzActionGroup -ResourceGroupName <myResourceGroup> -Name StartStop_VM_Notification
```

## <a name="unlink-your-workspace"></a>Aufheben der Verknüpfung mit Ihrem Arbeitsbereich

Jetzt können Sie die Verknüpfung mit Ihrem Arbeitsbereich aufheben:

1. Wählen Sie im Azure-Portal **Automation-Konto** > **Zugehörige Ressourcen** > **Verknüpfter Arbeitsbereich**. 

2. Wählen Sie **Verknüpfung des Arbeitsbereichs aufheben**, um für den Arbeitsbereich die Verknüpfung mit Ihrem Automation-Konto aufzuheben.

    ![Screenshot: Aufheben der Verknüpfung eines Arbeitsbereichs mit einem Automation-Konto](../media/move-account/unlink-workspace.png)

## <a name="move-your-automation-account"></a>Verschieben Ihres Automation-Kontos

Jetzt können Sie Ihr Automation-Konto und dessen Runbooks verschieben. 

1. Navigieren Sie im Azure-Portal zur Ressourcengruppe Ihres Automation-Kontos. Wählen Sie **Verschieben** > **In ein anderes Abonnement verschieben**.

    ![Screenshot: Seite „Ressourcengruppe“ > „In ein anderes Abonnement verschieben“](../media/move-account/move-resources.png)

2. Wählen Sie die Ressourcen in der Ressourcengruppe aus, die Sie verschieben möchten. Stellen Sie sicher, dass Sie Ihr Automation-Konto, Runbooks und die Log Analytics-Arbeitsbereichsressourcen einschließen.

## <a name="re-create-run-as-accounts"></a>Erneutes Erstellen von ausführenden Konten

Mit [ausführenden Konten](../manage-runas-account.md) wird in Azure Active Directory ein Dienstprinzipal für die Authentifizierung mit Azure-Ressourcen erstellt. Wenn Sie Abonnements ändern, werden die vorhandenen ausführenden Konten nicht mehr für das Automation-Konto verwendet. So erstellen Sie die ausführenden Konten neu:

1. Navigieren Sie unter dem neuen Abonnement zu Ihrem Automation-Konto, und wählen Sie unter **Kontoeinstellungen** die Option **Ausführende Konten** aus. Sie sehen, dass die ausführenden Konten jetzt als unvollständig angezeigt werden.

    ![Screenshot: Als unvollständig angezeigte ausführende Konten](../media/move-account/run-as-accounts.png)

2. Löschen Sie die ausführenden Konten einzeln, indem Sie auf der Seite **Eigenschaften** die Schaltfläche **Löschen** auswählen. 

    > [!NOTE]
    > Wenn Sie keine Berechtigungen zum Erstellen oder Anzeigen der ausführenden Konten haben, wird die folgende Meldung angezeigt: `You do not have permissions to create an Azure Run As account (service principal) and grant the Contributor role to the service principal.` Weitere Informationen finden Sie unter [Berechtigungen zum Konfigurieren von ausführenden Konten](../manage-runas-account.md#permissions).

3. Wählen Sie nach dem Löschen der ausführenden Konten unter **Ausführendes Azure-Konto** die Option **Erstellen** aus. 

4. Wählen Sie auf der Seite „Ausführendes Azure-Konto hinzufügen“ die Option **Erstellen** aus, um das ausführende Konto und den Dienstprinzipal zu erstellen. 

5. Wiederholen Sie die vorherigen Schritte mit dem klassischen ausführenden Azure-Konto.

## <a name="enable-features"></a>Aktivieren von Features

Nachdem Sie die ausführenden Konten neu erstellt haben, müssen die vor dem Verschieben entfernten Features reaktiviert werden:

1. Wählen Sie zum Aktivieren von „Änderungsnachverfolgung und Bestand“ die Option **Änderungsnachverfolgung und „Bestand** in Ihrem Automation-Konto aus. Wählen Sie den Log Analytics-Arbeitsbereich, den Sie verschoben haben, und anschließend **Aktivieren** aus.

2. Wiederholen Sie Schritt 1 für die Updateverwaltung.

    ![Screenshot: Reaktivieren von Features in Ihrem verschobenen Automation-Konto](../media/move-account/reenable-solutions.png)

3. Computer, für die Ihre Features aktiviert sind, werden angezeigt, wenn Sie eine Verbindung mit dem vorhandenen Log Analytics-Arbeitsbereich hergestellt haben. Das Feature „VMs außerhalb der Geschäftszeiten starten/beenden“ muss reaktiviert werden. Wählen Sie unter **Zugehörige Ressourcen** die Optionen **VMs starten/beenden** > **Weitere Informationen anzeigen und Lösung aktivieren** > **Erstellen**, um die Bereitstellung zu starten.

4. Wählen Sie auf der Seite „Lösung hinzufügen“ Ihren Log Analytics-Arbeitsbereich und das Automation-Konto aus.

    ![Screenshot: Menü „Lösung hinzufügen“](../media/move-account/add-solution-vm.png)

5. Konfigurieren Sie das Feature gemäß der Anleitung unter [VMs außerhalb der Geschäftszeiten starten/beenden – Übersicht](../automation-solution-vm-management.md).

## <a name="verify-the-move"></a>Überprüfen der Verschiebung

Vergewissern Sie sich nach Abschluss der Verschiebung, dass die unten aufgeführten Funktionen aktiviert sind. 

|Funktion|Tests|Problembehandlung|
|---|---|---|
|Runbooks|Ein Runbook kann erfolgreich ausgeführt werden und eine Verbindung mit Azure-Ressourcen herstellen.|[Beheben von Fehlern bei Runbooks](../troubleshoot/runbooks.md)
|Quellcodeverwaltung|Sie können eine manuelle Synchronisierung für Ihr Repository für die Quellcodeverwaltung durchführen.|[Integration für Quellcodeverwaltung](../source-control-integration.md)|
|Änderungsnachverfolgung und Bestand|Stellen Sie sicher, dass die aktuellen Bestandsdaten Ihrer Computer angezeigt werden.|[Problembehandlung bei Änderungsnachverfolgung und Inventur](../troubleshoot/change-tracking.md)|
|Updateverwaltung|Stellen Sie sicher, dass Ihre Computer angezeigt werden und fehlerfrei sind.</br>Führen Sie die Bereitstellung eines Updates der Testsoftware durch.|[Behandeln von Problemen mit Updateverwaltung](../troubleshoot/update-management.md)|
|Gemeinsame Ressourcen|Vergewissern Sie sich, dass alle Ihre freigegebenen Ressourcen ([Anmeldeinformationen](../shared-resources/credentials.md), [Variablen](../shared-resources/variables.md) und Ähnliches) angezeigt werden.|

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Verschieben von Ressourcen in Azure finden Sie unter [Unterstützung des Verschiebevorgangs für Ressourcen](../../azure-resource-manager/management/move-support-resources.md).
