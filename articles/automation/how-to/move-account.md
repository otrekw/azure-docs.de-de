---
title: Verschieben Ihres Azure Automation-Kontos in ein anderes Abonnement
description: In diesem Artikel wird beschrieben, wie Sie Ihr Automation-Konto in ein anderes Abonnement verschieben.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/11/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 2dbe7dc171b6e0ec81c99a460a4f997eeb9e27a5
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81681893"
---
# <a name="move-your-azure-automation-account-to-another-subscription"></a>Verschieben Ihres Azure Automation-Kontos in ein anderes Abonnement

In Azure Automation können Sie einige Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement verschieben. Sie können Ressourcen mit dem Azure-Portal, PowerShell, der Azure CLI oder der REST-API verschieben. Weitere Informationen zum Prozess finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../../azure-resource-manager/management/move-resource-group-and-subscription.md).

Das Azure Automation-Konto ist eine der Ressourcen, die Sie verschieben können. In diesem Artikel erfahren Sie, wie Sie Automation-Konten in eine andere Ressource oder in ein anderes Abonnement verschieben. Die groben Schritte zum Verschieben Ihres Automation-Kontos sind:

1. Entfernen Sie Ihre Lösungen.
2. Heben Sie die Verknüpfung mit Ihrem Arbeitsbereich auf.
3. Verschieben Sie das Automation-Konto.
4. Löschen Sie die ausführenden Konten, und erstellen Sie sie neu.
5. Reaktivieren Sie Ihre Lösungen.

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](../automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="remove-solutions"></a>Entfernen von Lösungen

Um die Verknüpfung Ihres Arbeitsbereichs mit Ihrem Automation-Konto aufzuheben, müssen Sie diese Lösungen aus Ihrem Arbeitsbereich entfernen:

- Änderungsnachverfolgung und Bestand
- Updateverwaltung
- Starten/Beenden von virtuellen Computern außerhalb der Geschäftszeiten

1. Navigieren Sie im Azure-Portal zu Ihrer Ressourcengruppe.
2. Suchen Sie jede Lösung, und klicken Sie auf der Seite „Ressourcen löschen“ auf **Löschen**.

    ![Löschen von Lösungen über das Azure-Portal](../media/move-account/delete-solutions.png)

    Wenn Sie dies bevorzugen, können Sie die Lösungen mithilfe des Cmdlets [Remove-AzResource](https://docs.microsoft.com/powershell/module/Az.Resources/Remove-AzResource?view=azps-3.7.0) löschen:

    ```azurepowershell-interactive
    $workspaceName = <myWorkspaceName>
    $resourceGroupName = <myResourceGroup>
    Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "ChangeTracking($workspaceName)" -ResourceGroupName $resourceGroupName
    Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Updates($workspaceName)" -ResourceGroupName $resourceGroupName
    Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Start-Stop-VM($workspaceName)" -ResourceGroupName $resourceGroupName
    ```

### <a name="remove-alert-rules-for-the-startstop-vms-during-off-hours-solution"></a>Entfernen von Warnungsregeln für die Lösung „VMs außerhalb der Geschäftszeiten starten/beenden“

Für die Lösung „VMs außerhalb der Geschäftszeiten starten/beenden“ müssen Sie auch die Warnungsregeln entfernen, die von der Lösung erstellt werden.

1. Navigieren Sie im Azure-Portal zu Ihrer Ressourcengruppe, und wählen Sie **Überwachung** > **Warnungen** > **Warnungsregeln verwalten**.

![Seite „Warnungen“ mit Auswahl „Warnungsregeln verwalten“](../media/move-account/alert-rules.png)

2. Auf der Seite „Regeln“ sollte eine Liste mit den Warnungen angezeigt werden, die in dieser Ressourcengruppe konfiguriert sind. Die Lösung erstellt folgende Regeln:

    * AutoStop_VM_Child
    * ScheduledStartStop_Parent
    * SequencedStartStop_Parent

3. Wählen Sie die Regeln einzeln aus, und klicken Sie auf **Löschen**, um sie zu entfernen.

    ![Seite „Regeln“ mit Anforderung der Bestätigung des Löschvorgangs für ausgewählte Regeln](../media/move-account/delete-rules.png)

    > [!NOTE]
    > Wenn auf der Seite „Regeln“ keine Warnungsregeln angezeigt werden, ändern Sie das Feld **Status** in „Deaktiviert“, um deaktivierte Warnungen anzuzeigen, da Sie sie möglicherweise deaktiviert haben.

4. Nachdem die Warnungsregeln entfernt wurden, müssen Sie die Aktionsgruppe entfernen, die für Benachrichtigungen der Lösung „VMs außerhalb der Geschäftszeiten starten/beenden“ erstellt wurde. Wählen Sie im Azure-Portal die Optionen **Überwachung** > **Warnungen** > **Aktionsgruppen verwalten**.

5. Wählen Sie **StartStop_VM_Notification** aus. 

6. Wählen Sie auf der Seite „Aktionsgruppe“ die Option **Löschen** aus.

    ![Seite „Aktionsgruppe“](../media/move-account/delete-action-group.png)

    Wenn Sie dies bevorzugen, können Sie Ihre Aktionsgruppe mithilfe des Cmdlets [Remove-AzActionGroup](https://docs.microsoft.com/powershell/module/az.monitor/remove-azactiongroup?view=azps-3.7.0) löschen:

    ```azurepowershell-interactive
    Remove-AzActionGroup -ResourceGroupName <myResourceGroup> -Name StartStop_VM_Notification
    ```

## <a name="unlink-your-workspace"></a>Aufheben der Verknüpfung mit Ihrem Arbeitsbereich

Jetzt können Sie die Verknüpfung mit Ihrem Arbeitsbereich aufheben:

1. Wählen Sie im Azure-Portal **Automation-Konto** > **Zugehörige Ressourcen** > **Verknüpfter Arbeitsbereich**. 

2. Wählen Sie **Verknüpfung des Arbeitsbereichs aufheben**, um für den Arbeitsbereich die Verknüpfung mit Ihrem Automation-Konto aufzuheben.

    ![Aufheben der Verknüpfung eines Arbeitsbereichs mit einem Automation-Konto](../media/move-account/unlink-workspace.png)

## <a name="move-your-automation-account"></a>Verschieben Ihres Automation-Kontos

Jetzt können Sie Ihr Automation-Konto und dessen Runbooks verschieben. 

1. Navigieren Sie im Azure-Portal zur Ressourcengruppe Ihres Automation-Kontos. Wählen Sie **Verschieben** > **In ein anderes Abonnement verschieben**.

    ![Seite „Ressourcengruppe“ > „In ein anderes Abonnement verschieben“](../media/move-account/move-resources.png)

2. Wählen Sie die Ressourcen in der Ressourcengruppe aus, die Sie verschieben möchten. Stellen Sie sicher, dass Sie Ihr Automation-Konto, Runbooks und die Log Analytics-Arbeitsbereichsressourcen einschließen.

## <a name="recreate-run-as-accounts"></a>Erneutes Erstellen von ausführenden Konten

Mit [ausführenden Konten](../manage-runas-account.md) wird in Azure Active Directory ein Dienstprinzipal für die Authentifizierung mit Azure-Ressourcen erstellt. Wenn Sie Abonnements ändern, werden die vorhandenen ausführenden Konten nicht mehr für das Automation-Konto verwendet. So erstellen Sie die ausführenden Konten erneut

1. Navigieren Sie unter dem neuen Abonnement zu Ihrem Automation-Konto, und wählen Sie unter **Kontoeinstellungen** die Option **Ausführende Konten**. Sie sehen, dass die ausführenden Konten jetzt als unvollständig angezeigt werden.

    ![Ausführende Konten sind unvollständig](../media/move-account/run-as-accounts.png)

2. Löschen Sie die ausführenden Konten einzeln mithilfe der Schaltfläche **Löschen** auf der Seite „Eigenschaften“. 

    > [!NOTE]
    > Wenn Sie keine Berechtigungen zum Erstellen oder Anzeigen der ausführenden Konten haben, wird die folgende Meldung angezeigt: `You do not have permissions to create an Azure Run As account (service principal) and grant the Contributor role to the service principal.` Informationen zu den Berechtigungen, die zum Konfigurieren eines ausführenden Kontos benötigt werden, finden Sie unter [Berechtigungen zum Konfigurieren von ausführenden Konten](../manage-runas-account.md#permissions).

3. Wählen Sie nach dem Löschen der ausführenden Konten unter **Ausführendes Azure-Konto** die Option **Erstellen** aus. 

4. Wählen Sie auf der Seite „Ausführendes Azure-Konto hinzufügen“ die Option **Erstellen** aus, um das ausführende Konto und den Dienstprinzipal zu erstellen. 

5. Wiederholen Sie die vorherigen Schritte mit dem klassischen ausführenden Azure-Konto.

## <a name="enable-solutions"></a>Aktivieren von Lösungen

Nachdem Sie die ausführenden Konten neu erstellt haben, müssen Sie die Lösungen wieder aktivieren, die Sie vor dem Verschieben entfernt haben: 

1. Um die Lösungen „Änderungsnachverfolgung“ und „Bestand“ zu aktivieren, wählen Sie „Änderungsnachverfolgung“ und „Bestand“ in Ihrem Automation-Konto aus. Wählen Sie den Log Analytics-Arbeitsbereich aus, den Sie verschoben haben, und dann **Aktivieren**.

2. Wiederholen Sie Schritt 1 für die Updateverwaltungslösung.

    ![Reaktivieren von Lösungen in Ihrem verschobenen Automation-Konto](../media/move-account/reenable-solutions.png)

3. Computer, für die mit Ihren Lösungen das Onboarding durchgeführt wird, sind sichtbar, wenn Sie eine Verbindung mit dem vorhandenen Log Analytics-Arbeitsbereich hergestellt haben. Um die Lösung „VMs außerhalb der Geschäftszeiten starten/beenden“ zu aktivieren, müssen Sie die Lösung erneut bereitstellen. Wählen Sie unter **Zugehörige Ressourcen** die Optionen **VMs starten/beenden** > **Weitere Informationen anzeigen und Lösung aktivieren** > **Erstellen**, um die Bereitstellung zu starten.

4. Wählen Sie auf der Seite „Lösung hinzufügen“ Ihren Log Analytics-Arbeitsbereich und das Automation-Konto aus.

    ![Menü „Lösung hinzufügen“](../media/move-account/add-solution-vm.png)

5. Konfigurieren Sie die Lösung wie unter [Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten in Azure Automation](../automation-solution-vm-management.md) beschrieben.

## <a name="verify-the-move"></a>Überprüfen der Verschiebung

Vergewissern Sie sich nach Abschluss der Verschiebung, dass die unten aufgeführten Funktionen aktiviert sind. 

|Funktion|Tests|Problembehandlung|
|---|---|---|
|Runbooks|Ein Runbook kann erfolgreich ausgeführt werden und eine Verbindung mit Azure-Ressourcen herstellen.|[Beheben von Fehlern bei Runbooks](../troubleshoot/runbooks.md)
|Quellcodeverwaltung|Sie können eine manuelle Synchronisierung für Ihr Repository für die Quellcodeverwaltung durchführen.|[Integration für Quellcodeverwaltung](../source-control-integration.md)|
|Änderungsnachverfolgung und Bestand|Stellen Sie sicher, dass die aktuellen Bestandsdaten Ihrer Computer angezeigt werden.|[Problembehandlung bei Änderungsnachverfolgung und Inventur](../troubleshoot/change-tracking.md)|
|Updateverwaltung|Stellen Sie sicher, dass Ihre Computer angezeigt werden und fehlerfrei sind.</br>Führen Sie die Bereitstellung eines Updates der Testsoftware durch.|[Behandeln von Problemen mit Updateverwaltung](../troubleshoot/update-management.md)|
|Gemeinsame Ressourcen|Vergewissern Sie sich, dass alle freigegebenen Ressourcen angezeigt werden, z. B. [Anmeldeinformationen](../shared-resources/credentials.md), [Variablen](../shared-resources/variables.md) usw.|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verschieben von Ressourcen in Azure finden Sie unter [Unterstützung des Verschiebevorgangs für Ressourcen](../../azure-resource-manager/management/move-support-resources.md).
