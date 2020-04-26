---
title: Verwalten von Runbooks in Azure Automation
description: In diesem Artikel wird beschrieben, wie Sie Runbooks in Azure Automation verwalten.
services: automation
ms.subservice: process-automation
ms.date: 02/14/2019
ms.topic: conceptual
ms.openlocfilehash: a1229ee389b41625554fb2869089b08a3cb9cb6d
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81676512"
---
# <a name="manage-runbooks-in-azure-automation"></a>Verwalten von Runbooks in Azure Automation

Sie können Azure Automation ein Runbook hinzufügen, indem Sie entweder [ein neues Runbook erstellen](#creating-a-runbook) oder [ein vorhandenes Runbook](#importing-a-runbook) aus einer Datei oder aus dem [Runbookkatalog](automation-runbook-gallery.md) importieren. Dieser Artikel enthält Informationen zum Erstellen und Importieren von Runbooks aus einer Datei. Einzelheiten zum Zugreifen auf Communityrunbooks und -module erfahren Sie in [Runbook- und Modulkataloge für Azure Automation](automation-runbook-gallery.md).

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="creating-a-runbook"></a>Erstellen eines Runbooks

Sie können ein neues Runbook in Azure Automation mithilfe eines der Azure-Portale oder Windows PowerShell erstellen. Sobald das Runbook erstellt wurde, können Sie es mithilfe der Informationen in [Grundlagen des PowerShell-Workflows](automation-powershell-workflow.md) und [Grafische Erstellung in Azure Automation](automation-graphical-authoring-intro.md) bearbeiten.

### <a name="create-a-runbook-in-the-azure-portal"></a>Erstellen eines Runbooks im Azure-Portal

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto.
2. Wählen Sie im Hub unter **Prozessautomatisierung** die Option **Runbooks** aus, um die Liste der Runbooks zu öffnen.
3. Klicken Sie auf **Runbook erstellen**.
4. Geben Sie einen Namen für das Runbook ein, und wählen Sie dessen [Typ](automation-runbook-types.md) aus. Der Name des Runbooks muss mit einem Buchstaben beginnen und darf Buchstaben, Zahlen, Unterstriche und Bindestriche enthalten.
5. Klicken Sie auf **Erstellen** , um das Runbook zu erstellen und den Editor zu öffnen.

### <a name="create-a-runbook-with-powershell"></a>Erstellen eines Runbooks mit PowerShell

Sie können das Cmdlet [New-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationrunbook?view=azps-3.5.0) verwenden, um ein leeres [PowerShell-Workflow-Runbook](automation-runbook-types.md#powershell-workflow-runbooks) zu erstellen. Verwenden Sie den Parameter `Type`, um einen der für `New-AzAutomationRunbook` definierten Runbooktypen anzugeben.

Das folgenden Beispiel zeigt, wie Sie ein neues leeres Runbook erstellen.

```azurepowershell-interactive
New-AzAutomationRunbook -AutomationAccountName MyAccount `
-Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell
```

## <a name="importing-a-runbook"></a>Importieren eines Runbooks

Sie können ein neues Runbook in Azure Automation erstellen, indem Sie ein PowerShell-Skript, einen PowerShell-Workflow ( **.ps1**), ein exportiertes grafisches Runbook ( **.graphrunbook**) oder ein Python2-Skript ( **.py**) importieren.  Geben Sie den [Runbook-Typ](automation-runbook-types.md) an, der beim Import erstellt wird. Berücksichtigen Sie dabei die folgenden Aspekte.

* Eine **.ps1**-Datei ohne Workflow kann in ein [PowerShell-Runbook](automation-runbook-types.md#powershell-runbooks) oder [PowerShell-Workflow-Runbook](automation-runbook-types.md#powershell-workflow-runbooks) importiert werden. Wenn Sie sie in ein PowerShell-Workflow-Runbook importieren, wird sie in einen Workflow konvertiert. In diesem Fall sind im Runbook Kommentare enthalten, um die vorgenommenen Änderungen zu beschreiben.

* Eine **.ps1**-Datei mit einem PowerShell-Workflow kann nur in ein [PowerShell-Workflow-Runbook](automation-runbook-types.md#powershell-workflow-runbooks) importiert werden. Wenn die Datei mehrere PowerShell-Workflows enthält, schlägt der Import fehl. Sie müssen jeden Workflow in einer eigenen Datei speichern und einzeln importieren.

* Eine **.ps1**-Datei mit einem PowerShell-Workflow darf nicht in ein [PowerShell-Runbook](automation-runbook-types.md#powershell-runbooks) importiert werden, da sie von der PowerShell-Skript-Engine nicht erkannt werden kann.

* Eine **.graphrunbook**-Datei kann nur in ein neues [grafisches Runbook](automation-runbook-types.md#graphical-runbooks) importiert werden. Beachten Sie, dass Sie anhand einer **.graphrunbook**-Datei nur ein grafisches Runbook erstellen können.

### <a name="import-a-runbook-from-a-file-with-the-azure-portal"></a>Importieren eines Runbooks aus einer Datei im Azure-Portal

Mit dem folgenden Verfahren können eine Skriptdatei in Azure Automation importieren.

> [!NOTE]
> Sie können über das Portal nur eine **.ps1**-Datei in ein PowerShell-Workflow-Runbook importieren.

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto.
2. Wählen Sie im Hub unter **Prozessautomatisierung** die Option **Runbooks** aus, um die Liste der Runbooks zu öffnen.
3. Klicken Sie auf **Runbook importieren**.
4. Klicken Sie auf **Runbookdatei**, und wählen Sie die zu importierende Datei aus.
5. Wenn das Feld **Name** aktiviert ist, haben Sie die Möglichkeit, den Namen des Runbooks zu ändern. Der Name muss mit einem Buchstaben beginnen und darf Buchstaben, Zahlen, Unterstriche und Bindestriche enthalten.
6. Der [Runbooktyp](automation-runbook-types.md) wird automatisch ausgewählt, Sie können den Typ jedoch unter Berücksichtigung der geltenden Einschränkungen ändern.
7. Klicken Sie auf **Erstellen**. Das neue Runbook wird in der Liste der Runbooks für das Automation-Konto angezeigt.
8. Sie müssen [das Runbook veröffentlichen](#publishing-a-runbook) , bevor Sie es ausführen können.

> [!NOTE]
> Nachdem Sie ein grafisches Runbook oder ein grafisches PowerShell-Workflow-Runbook importiert haben, können Sie es in einen anderen Typ konvertieren. Sie können allerdings nicht eines dieser grafischen Runbooks in ein Textrunbook konvertieren.

### <a name="import-a-runbook-from-a-script-file-with-windows-powershell"></a>Importieren eines Runbooks aus einer Skriptdatei mit Windows PowerShell

Verwenden Sie das Cmdlet [Import-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/import-azautomationrunbook?view=azps-3.5.0), um eine Skriptdatei als Entwurf eines PowerShell-Workflow-Runbooks zu importieren. Wenn das Runbook bereits vorhanden ist, tritt beim Import ein Fehler auf, sofern Sie nicht den Parameter `Force` mit dem Cmdlet verwenden.

Das folgende Beispiel zeigt, wie Sie eine Skriptdatei in ein Runbook importieren.

```azurepowershell-interactive
$automationAccountName =  "AutomationAccount"
$runbookName = "Sample_TestRunbook"
$scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
$RGName = "ResourceGroup"

Import-AzAutomationRunbook -Name $runbookName -Path $scriptPath `
-ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
-Type PowerShellWorkflow
```

## <a name="testing-a-runbook"></a>Testen eines Runbooks

Wenn Sie ein Runbook testen, wird die [Entwurfsversion](#publishing-a-runbook) ausgeführt, und alle darin ausgeführten Aktionen werden abgeschlossen. Es wird kein Auftragsverlauf erstellt. Doch die Datenströme [Ausgabe](automation-runbook-output-and-messages.md#output-stream) sowie [Warnung und Fehler](automation-runbook-output-and-messages.md#message-streams) werden im Ausgabebereich „Test“ angezeigt. Nachrichten an den [ausführlichen Datenstrom](automation-runbook-output-and-messages.md#message-streams) werden im Ausgabebereich nur angezeigt, wenn die Variable [VerbosePreference](automation-runbook-output-and-messages.md#preference-variables) auf `Continue` festgelegt ist.

Auch wenn die Entwurfsversion ausgeführt wird, wird das Runbook trotzdem normal ausgeführt, und alle Aktionen werden auf die Ressourcen in der Umgebung angewendet. Aus diesem Grund sollten Sie Runbooks nur auf Ressourcen testen, die sich nicht in der Produktionsumgebung befinden.

Die Vorgehensweise zum Testen der einzelnen [Typen von Runbooks](automation-runbook-types.md) ist identisch. Es besteht kein Unterschied zwischen Tests mit dem Text-Editor und dem grafischen Editor im Azure-Portal.

1. Öffnen Sie die Entwurfsversion des Runbooks entweder im [Text-Editor](automation-edit-textual-runbook.md) oder [grafischen Editor](automation-graphical-authoring-intro.md).
1. Klicken Sie auf die Schaltfläche **Test**, um die Seite „Test“ zu öffnen.
1. Wenn das Runbook über Parameter verfügt, sind diese im linken Bereich aufgeführt. Hier können Sie die für den Test zu verwendenden Werte angeben.
1. Wenn Sie den Test auf einem [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md) ausführen möchten, ändern Sie die **Testlaufeinstellungen** in **Hybrid Worker**, und wählen Sie den Namen der Zielgruppe aus.  Andernfalls behalten Sie den Standardwert **Azure** bei, um den Test in der Cloud auszuführen.
1. Klicken Sie auf die Schaltfläche **Start**, um den Test zu starten.
1. Sie können die Schaltflächen unter dem Bereich „Ausgabe“ verwenden, um während des Tests ein [PowerShell-Workflow](automation-runbook-types.md#powershell-workflow-runbooks)- oder [graphisches](automation-runbook-types.md#graphical-runbooks) Runbook zu beenden oder anzuhalten. Wenn Sie das Runbook anhalten, wird die aktuelle Aktivität vor der Unterbrechung abgeschlossen. Nachdem das Runbook angehalten wurde, können Sie es beenden oder erneut starten.
1. Untersuchen Sie die Ausgabe des Runbooks im Bereich „Ausgabe“.

## <a name="publishing-a-runbook"></a>Veröffentlichen eines Runbooks

Wenn Sie ein neues Runbooks erstellen oder importieren, müssen Sie es veröffentlichen, bevor Sie es ausführen können. Jedes Runbook in Azure Automation umfasst eine Entwurfsversion und eine veröffentlichte Version. Nur die veröffentlichte Version kann ausgeführt werden, und nur die Entwurfsversion kann bearbeitet werden. Die veröffentlichte Version bleibt von Änderungen an der Entwurfsversion unberührt. Wenn die Entwurfsversion zur Verfügung gestellt werden soll, können Sie sie veröffentlichen, wodurch die Entwurfsversion die aktuell veröffentlichte Version überschreibt.

### <a name="publish-a-runbook-in-the-azure-portal"></a>Veröffentlichen eines Runbooks im Azure-Portal

1. Öffnen Sie das Runbook im Azure-Portal.
2. Klicken Sie auf **Bearbeiten**.
3. Klicken Sie auf **Veröffentlichen** und dann als Antwort in der Bestätigungsmeldung auf **Ja**.

### <a name="publish-a-runbook-using-powershell"></a>Veröffentlichen eines Runbooks mithilfe von PowerShell

Mit dem Cmdlet [Publish-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/Az.Automation/Publish-AzAutomationRunbook?view=azps-3.5.0) können Sie ein Runbook mit Windows PowerShell veröffentlichen. Das folgende Beispiel zeigt, wie Sie ein Beispielrunbook veröffentlichen:

```azurepowershell-interactive
$automationAccountName =  "AutomationAccount"
$runbookName = "Sample_TestRunbook"
$RGName = "ResourceGroup"

Publish-AzAutomationRunbook -AutomationAccountName $automationAccountName `
-Name $runbookName -ResourceGroupName $RGName
```

## <a name="scheduling-a-runbook-in-the-azure-portal"></a>Planen eines Runbooks im Azure-Portal

Wenn Ihr Runbook veröffentlicht wurde, können Sie seinen Betrieb planen.

1. Öffnen Sie das Runbook im Azure-Portal.
2. Wählen Sie unter **Ressourcen** die Option **Zeitpläne** aus.
3. Wählen Sie **Zeitplan hinzufügen** aus.
4. Wählen Sie im Bereich „Runbook planen“ **Zeitplan mit Runbook verknüpfen** aus.
5. Wählen Sie im Bereich „Zeitplan“ **Neuen Zeitplan erstellen** aus.
6. Geben Sie im Bereich „Neuer Zeitplan“ einen Namen, eine Beschreibung und andere Parameter ein. 
7. Nachdem der Zeitplan erstellt wurde, markieren Sie ihn, und klicken Sie auf **OK**. Er sollte jetzt mit Ihrem Runbook verknüpft sein.
8. Suchen Sie in Ihrem Postfach nach einer E-Mail, die Sie über den Status des Runbooks informiert.

## <a name="next-steps"></a>Nächste Schritte

* Informationen dazu, wie Sie vom Katalog mit Runbooks und PowerShell-Modulen profitieren, finden Sie unter [Runbook- und Modulkataloge für Azure Automation](automation-runbook-gallery.md).
* Weitere Informationen zum Bearbeiten von PowerShell- und PowerShell-Workflow-Runbooks mit einem Text-Editor finden Sie unter [Bearbeiten von Textrunbooks in Azure Automation](automation-edit-textual-runbook.md).
* Weitere Informationen zum Erstellen grafischer Runbooks finden Sie unter [Grafische Erstellung in Azure Automation](automation-graphical-authoring-intro.md).
