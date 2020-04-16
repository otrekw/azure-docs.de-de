---
title: Bearbeiten von Textrunbooks in Azure Automation
description: Dieser Artikel enthält verschiedene Verfahren zum Arbeiten mit PowerShell und PowerShell-Workflow-Runbooks in Azure Automation unter Verwendung des Text-Editors.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 08/01/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 2ef7db244057bc8b3b2e4d938b9f3bdd11c7940a
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81406027"
---
# <a name="editing-textual-runbooks-in-azure-automation"></a>Bearbeiten von Textrunbooks in Azure Automation

Mit dem Text-Editor in Azure Automation können Sie [PowerShell-Runbooks](automation-runbook-types.md#powershell-runbooks) und [PowerShell-Workflow-Runbooks](automation-runbook-types.md#powershell-workflow-runbooks) bearbeiten. Dieser Editor hat die typischen Merkmale anderer Code-Editoren, z. B. IntelliSense. Es verfügt außerdem über eine Farbcodierung mit zusätzlichen speziellen Features, die Ihnen den Zugriff auf Ressourcen erleichtern, die für Runbooks üblich sind. 

Der Text-Editor enthält ein Feature zum Einfügen von Code für Cmdlets, Objekte und untergeordnete Runbooks in ein Runbook. Anstatt den Code selbst einzugeben, können Sie aus einer Liste verfügbarer Ressourcen auswählen und der Editor fügt den entsprechenden Code in das Runbook ein.

Jedes Runbook in Azure Automation umfasst zwei Versionen: eine Entwurfsversion und eine veröffentlichte Version. Sie bearbeiten die Entwurfsversion des Runbooks und veröffentlichen sie anschließend, damit sie ausgeführt werden kann. Die veröffentlichte Version kann nicht bearbeitet werden. Weitere Informationen finden Sie unter [Veröffentlichen eines Runbooks](manage-runbooks.md#publishing-a-runbook).

Dieser Artikel enthält detaillierte Schritte zum Ausführen verschiedener Funktionen mit diesem Editor. Diese gelten nicht für [Grafische Runbooks](automation-runbook-types.md#graphical-runbooks). Informationen zur Arbeit mit diesen Runbooks finden Sie unter [Grafische Erstellung in Azure Automation](automation-graphical-authoring-intro.md).

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="editing-a-runbook-with-the-azure-portal"></a>Bearbeiten eines Runbooks über das Azure-Portal

Gehen Sie wie folgt vor, um ein Runbook zur Bearbeitung im Text-Editor zu öffnen.

1. Wählen Sie im Azure-Portal Ihr Automation-Konto aus.
2. Klicken Sie unter **PROZESSAUTOMATISIERUNG** auf **Runbooks**, um die Liste mit den Runbooks zu öffnen.
3. Wählen Sie das zu bearbeitende Runbook aus, und klicken Sie dann auf **Bearbeiten**.
4. Bearbeiten Sie das Runbook.
5. Klicken Sie auf **Speichern** , wenn die Bearbeitung abgeschlossen ist.
6. Klicken Sie auf **Veröffentlichen**, wenn Sie die neueste Entwurfsversion des Runbooks veröffentlichen möchten.

### <a name="insert-a-cmdlet-into-a-runbook"></a>Einfügen eines Cmdlets in ein Runbook

1. Platzieren Sie den Cursor im Canvas des Text-Editors an der Position, an der Sie das Cmdlet einfügen möchten.
2. Erweitern Sie im Bibliotheksteuerelement den Knoten **Cmdlets** .
3. Erweitern Sie das Modul mit dem zu verwendenden Cmdlet.
4. Klicken Sie mit der rechten Maustaste auf den Namen des einzufügenden Cmdlets, und wählen Sie **Zur Canvas hinzufügen** aus. Wenn das Cmdlet über mehrere Parametersätze verfügt, fügt der Editor den Standardsatz hinzu. Sie können das Cmdlet auch erweitern, um einen anderen Parametersatz auszuwählen.
5. Beachten Sie, dass der Code für das Cmdlet mit der gesamten Liste der Parameter eingefügt wird.
6. Geben Sie für jeden erforderlichen Parameter einen geeigneten Wert anstelle des von spitzen Klammern (<>) umgebenen Werts an. Entfernen Sie alle nicht erforderlichen Parameter.

### <a name="insert-code-for-a-child-runbook-into-a-runbook"></a>Einfügen von Code für ein untergeordnetes Runbook in ein Runbook

1. Platzieren Sie den Cursor in der Canvas des Text-Editors an der Position, an der Sie den Code für das [untergeordnete Runbook](automation-child-runbooks.md) einfügen möchten.
2. Erweitern Sie im Bibliotheksteuerelement den Knoten **Runbooks** .
3. Klicken Sie mit der rechten Maustaste auf das einzufügende Runbook, und wählen Sie **Add to canvas** (Zur Canvas hinzufügen).
4. Der Code für das untergeordnete Runbook wird mit allen Platzhaltern für alle Runbook-Parameter eingefügt.
5. Ersetzen Sie die Platzhalter durch entsprechende Werte für jeden Parameter.

### <a name="insert-an-asset-into-a-runbook"></a>Einfügen eines Objekts in ein Runbook

1. Platzieren Sie den Cursor im Canvassteuerelement des Text-Editors an der Position, an der Sie den Code für das untergeordnete Runbook einfügen möchten.
2. Erweitern Sie im Bibliotheksteuerelement den Knoten **Objekte** .
3. Erweitern Sie den Knoten für den gewünschten Objekttyp.
4. Klicken Sie mit der rechten Maustaste auf den Namen des einzufügenden Objekts, und wählen Sie **Zur Canvas hinzufügen** aus. Wählen Sie für [Variablenobjekte](automation-variables.md) entweder **„Variable abrufen“ zum Zeichenbereich hinzufügen** oder **„Variable festlegen“ zum Zeichenbereich hinzufügen** aus. Dies ist abhängig davon, ob Sie die Variable abrufen oder festlegen möchten.
5. Beachten Sie, dass der Code für das Objekt in das Runbook eingefügt wird.

## <a name="editing-an-azure-automation-runbook-using-windows-powershell"></a>Bearbeiten eines Azure Automation-Runbooks mit Windows PowerShell

Zum Bearbeiten eines Runbooks mit Windows PowerShell verwenden Sie einen Editor Ihrer Wahl und speichern dann das Runbook in einer **PS1**-Datei. Sie können das Cmdlet [Export-AzAutomationRunbook](/powershell/module/Az.Automation/Export-AzAutomationRunbook) verwenden, um den Inhalt des Runbooks abzurufen. Sie können das Cmdlet [Import-AzAutomationRunbook](/powershell/module/Az.Automation/import-azautomationrunbook) verwenden, um den vorhandenen Entwurf des Runbooks durch das geänderte Runbook zu ersetzen.

### <a name="retrieve-the-contents-of-a-runbook-using-windows-powershell"></a>Abrufen des Inhalts eines Runbooks mit Windows PowerShell

Die folgenden Beispielbefehle zeigen, wie das Skript für ein Runbook abgerufen und in einer Skriptdatei gespeichert wird. In diesem Beispiel wird die Entwurfsversion abgerufen. Sie können auch die veröffentlichte Version des Runbooks abrufen, auch wenn diese Version nicht geändert werden kann.

```powershell-interactive
$resourceGroupName = "MyResourceGroup"
$automationAccountName = "MyAutomatonAccount"
$runbookName = "Hello-World"
$scriptFolder = "c:\runbooks"

Export-AzAutomationRunbook -Name $runbookName -AutomationAccountName $automationAccountName -ResourceGroupName $resourceGroupName -OutputFolder $scriptFolder -Slot Draft
```

### <a name="change-the-contents-of-a-runbook-using-windows-powershell"></a>Ändern des Inhalts eines Runbooks mit Windows PowerShell

Die folgenden Beispielbefehle zeigen, wie der vorhandene Inhalt eines Runbooks durch den Inhalt einer Skriptdatei ersetzt wird. Hierbei handelt es sich um das gleiche Beispielverfahren wie unter [So importieren Sie ein Runbook aus einer Skriptdatei mit Windows PowerShell](manage-runbooks.md#importing-a-runbook).

```powershell-interactive
$resourceGroupName = "MyResourceGroup"
$automationAccountName = "MyAutomatonAccount"
$runbookName = "Hello-World"
$scriptFolder = "c:\runbooks"

Import-AzAutomationRunbook -Path "$scriptfolder\Hello-World.ps1" -Name $runbookName -Type PowerShell -AutomationAccountName $automationAccountName -ResourceGroupName $resourceGroupName -Force
Publish-AzAutomationRunbook -Name $runbookName -AutomationAccountName $automationAccountName -ResourceGroupName $resourceGroupName
```

## <a name="related-articles"></a>Verwandte Artikel

* [Verwalten von Runbooks in Azure Automation](manage-runbooks.md)
* [Grundlagen des PowerShell-Workflows](automation-powershell-workflow.md)
* [Grafische Erstellung in Azure Automation](automation-graphical-authoring-intro.md)
* [Zertifikate](automation-certificates.md)
* [Verbindungen](automation-connections.md)
* [Anmeldeinformationen](automation-credentials.md)
* [Zeitpläne](automation-schedules.md)
* [Variablen](automation-variables.md)
* [PowerShell-Cmdlet-Referenz](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation)
