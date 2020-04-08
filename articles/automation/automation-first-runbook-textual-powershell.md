---
title: Mein erstes PowerShell-Runbook in Azure Automation
description: Tutorial, in dem Sie sich mit dem Erstellen, Testen und Veröffentlichen eines einfachen PowerShell-Runbooks vertraut machen können.
keywords: Azure PowerShell, Tutorial zu PowerShell-Skripts, PowerShell-Automation
services: automation
ms.subservice: process-automation
ms.date: 11/27/2018
ms.topic: conceptual
ms.openlocfilehash: e0c48137f5eecc96b6e7b1cbce5f0c683b2a976a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79367311"
---
# <a name="my-first-powershell-runbook"></a>Mein erstes PowerShell-Runbook

> [!div class="op_single_selector"]
> * [Grafisch](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell-Workflow](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)

Dieses Tutorial führt Sie durch die Erstellung eines [PowerShell-Runbooks](automation-runbook-types.md#powershell-runbooks) in Azure Automation. Beginnen Sie mit einem einfachen Runbook, das Sie testen und veröffentlichen können, während Sie erfahren, wie Sie den Status des Runbookauftrags nachverfolgen. Anschließend ändern Sie das Runbook, um damit tatsächlich Azure-Ressourcen zu verwalten. Im vorliegenden Fall soll ein virtueller Azure-Computer gestartet werden. Absolvieren Sie das Tutorial, um durch Hinzufügen von Runbookparametern die Stabilität des Runbooks zu erhöhen.

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Automation-Konto](automation-quickstart-create-account.md) dient zur Aufbewahrung des Runbooks und zur Authentifizierung gegenüber Azure-Ressourcen. Dieses Konto muss über die Berechtigung zum Starten und Beenden des virtuellen Computers verfügen.
* Einen virtuellen Azure-Computer. Da Sie diesen Computer starten und beenden, sollte es sich nicht um eine Produktions-VM handeln.
* Abhängig von den verwendeten Cmdlets können Sie bei Bedarf [Azure-Module importieren](shared-resources/modules.md) oder [Module aktualisieren](automation-update-azure-modules.md).

## <a name="differences-from-powershell-workflow-runbooks"></a>Unterschiede zu PowerShell-Workflow-Runbooks

PowerShell-Runbooks verfügen über den gleichen Lebenszyklus, die gleichen Funktionen und die gleiche Verwaltung wie PowerShell-Workflow-Runbooks. Es gibt jedoch auch einige Unterschiede und Einschränkungen.

| Merkmal  | PowerShell-Runbooks | PowerShell-Workflow-Runbooks |
| ------ | ----- | ----- |
| Geschwindigkeit | Werden schnell ausgeführt, da sie keinen Kompilierungsschritt enthalten. | Werden langsamer ausgeführt. |
| Prüfpunkte | Unterstützen keine Prüfpunkte. PowerShell-Runbooks beginnen beim Fortsetzen von vorn. | Verwenden Prüfpunkte, sodass eine Arbeitsmappe von einem beliebigen Punkt aus fortgesetzt werden kann. |
| Befehlsausführung | Unterstützen nur die serielle Ausführung. | Unterstützen sowohl die serielle als auch die parallele Ausführung.|
| Runspace | Sämtliche Skriptschritte werden von einem einzelnen Runspace ausgeführt. | Ein separater Runspace kann für eine Aktivität, einen Befehl oder einen Skriptblock verwendet werden. |

Neben diesen Unterschieden gibt es einige [syntaktische Unterschiede](https://technet.microsoft.com/magazine/dn151046.aspx) zwischen PowerShell-Runbooks und PowerShell-Workflow-Runbooks.

## <a name="step-1---create-runbook"></a>Schritt 1: Erstellen eines Runbooks

Erstellen Sie zunächst ein einfaches Runbook, das den Text `Hello World` ausgibt.

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto.
2. Wählen Sie unter **Prozessautomatisierung** die Option **Runbooks** aus, um die Liste der Runbooks zu öffnen.
3. Erstellen Sie ein neues Runbook, indem Sie **Runbook erstellen** auswählen.
4. Nennen Sie das Runbook **MyFirstRunbook-PowerShell**.
5. In diesem Fall wird ein [PowerShell-Runbook](automation-runbook-types.md#powershell-runbooks) erstellt. Wählen Sie unter **Runbooktyp** die Option **PowerShell** aus.
6. Klicken Sie auf **Erstellen** , um das Runbook zu erstellen und den Text-Editor zu öffnen.

## <a name="step-2---add-code-to-the-runbook"></a>Schritt 2 – Hinzufügen von Code zum Runbook

Sie können entweder direkt Code in das Runbook eingeben, oder Sie wählen Cmdlets, Runbooks und Objekte aus dem Bibliotheksteuerelement aus und fügen diese mit entsprechenden Parametern zum Runbook hinzu. In diesem Tutorial wird der Code direkt in das Runbook eingegeben.

1. Ihr Runbook ist momentan leer. Geben Sie `Write-Output "Hello World"` in den Textbereich des Skripts ein.

   ![Hello World](media/automation-first-runbook-textual-powershell/automation-helloworld.png)

2. Klicken Sie auf **Speichern**, um das Runbook zu speichern.

## <a name="step-3---test-the-runbook"></a><a name="step-3---test-the-runbook"> </a> Schritt 3: Testen des Runbooks

Bevor Sie das Runbook für die Verwendung in der Produktionsumgebung veröffentlichen, sollten Sie es testen, um sicherzustellen, dass es ordnungsgemäß funktioniert. Beim Testen eines Runbooks führen Sie die Entwurfsversion aus und sehen sich interaktiv die Ausgabe an.

1. Klicken Sie auf **Testbereich** , um den Testbereich zu öffnen.
2. Klicken Sie auf **Starten** , um den Test zu starten. Andere Optionen dürften nicht zur Verfügung stehen.
3. Beachten Sie, dass ein [Runbookauftrag](automation-runbook-execution.md) erstellt und der zugehörige Status im Bereich angezeigt wird.

   Der Auftrag weist zunächst den Status `Queued` auf, um anzugeben, dass der Auftrag darauf wartet, dass in der Cloud ein Runbook Worker verfügbar wird. Der Status wechselt zu `Starting`, wenn ein Worker den Auftrag beansprucht. Wenn die Ausführung des Runbooks beginnt, wechselt der Status zu `Running`.

4. Nach Abschluss des Runbookauftrags wird die Ausgabe im Bereich „Test“ angezeigt. In diesem Fall wird `Hello World` angezeigt.

   ![Ausgabe des Testbereichs](media/automation-first-runbook-textual-powershell/automation-testpane-output.png)

5. Schließen Sie den Testbereich, um zum Zeichenbereich zurückzukehren.

## <a name="step-4---publish-and-start-the-runbook"></a>Schritt 4: Veröffentlichen und Starten des Runbooks

Das erstellte Runbook befindet sich immer noch im Entwurfsmodus. Es muss veröffentlicht werden, damit es in der Produktionsumgebung ausgeführt werden kann. Beim Veröffentlichen eines Runbooks wird die vorhandene veröffentlichte Version durch die Entwurfsversion überschrieben. In diesem Fall ist noch keine veröffentlichte Version vorhanden, da Sie das Runbook gerade erst erstellt haben.

1. Klicken Sie auf **Veröffentlichen**, um das Runbook zu veröffentlichen, und bestätigen Sie den Vorgang mit **Ja**.
1. Scrollen Sie nach links, um das Runbook auf der Seite „Runbooks“ anzuzeigen, und beachten Sie, dass der Wert **Erstellungsstatus** als **Veröffentlicht** angezeigt wird.
1. Führen Sie wieder einen Bildlauf nach rechts durch, um den Bereich für **MyFirstRunbook-PowerShell**anzuzeigen.
   
   Mit den Optionen im oberen Bereich können Sie das Runbook jetzt starten, den Start für einen späteren Zeitpunkt planen oder einen [Webhook](automation-webhooks.md) erstellen, um das Runbook über einen HTTP-Aufruf starten zu können.
1. Klicken Sie zum Starten des Runbooks auf **Starten** und anschließend auf **Ja**. 
1. Ein Auftragsbereich für den soeben erstellten Runbookauftrag wird angezeigt. Dieser Bereich kann zwar geschlossen werden, lassen Sie ihn aber vorerst geöffnet, um den Status des Auftrags zu verfolgen. Der Auftragsstatus wird in der **Auftragszusammenfassung** angezeigt. Bei den möglichen Statuswerten handelt es sich um die beschriebenen Statusoptionen für Runbooktests.

   ![API-Zusammenfassung](media/automation-first-runbook-textual-powershell/job-pane-status-blade-jobsummary.png)

1. Wenn der Runbookstatus `Completed` lautet, klicken Sie auf **Ausgabe**, um die Ausgabeseite zu öffnen. Dort wird `Hello World` angezeigt.

   ![Auftragsausgabe](media/automation-first-runbook-textual-powershell/job-pane-status-blade-outputtile.png)

1. Schließen Sie die Seite „Ausgabe“.
1. Klicken Sie auf **Alle Protokolle**, um den Bereich „Datenströme“ für den Runbookauftrag zu öffnen. Im Ausgabestream sollte nur `Hello World` angezeigt werden.

    Beachten Sie, dass im Bereich „Streams“ auch andere Streams für einen Runbookauftrag, etwa ausführliche Streams und Fehlerstreams, angezeigt werden können, sofern das Runbook in diese schreibt.

   ![Alle Protokolle](media/automation-first-runbook-textual-powershell/job-pane-status-blade-alllogstile.png)

1. Schließen Sie den Datenstrom- und den Auftragsbereich, um zum Bereich **MyFirstRunbook-PowerShell** zurückzukehren.
1. Klicken Sie unter **Details** auf **Aufträge**, um die Auftragsseite für dieses Runbook zu öffnen. Auf dieser Seite werden alle von diesem Runbook erstellten Aufträge aufgeführt. Es sollte nur ein Auftrag aufgeführt sein, da Sie den Auftrag bislang erst einmal ausgeführt haben.

   ![Auftragsliste](media/automation-first-runbook-textual-powershell/runbook-control-job-tile.png)

1. Klicken Sie auf den Namen des Auftrags, um den Bereich „Auftrag“ zu öffnen, der beim Starten des Runbooks angezeigt wurde. In diesem Bereich können Sie Details zu jedem Auftrag anzeigen, der für das Runbook erstellt wurde.

## <a name="step-5---add-authentication-to-manage-azure-resources"></a>Schritt 5: Hinzufügen von Authentifizierungsfunktionen für die Verwaltung von Azure-Ressourcen

Sie haben Ihr Runbook inzwischen zwar getestet und veröffentlicht, bislang ist es aber noch nicht sonderlich hilfreich. Sie möchten damit ja eigentlich Azure-Ressourcen verwalten. Zu diesem Zweck muss sich das Runbook mit dem ausführenden Konto authentifizieren können, das automatisch bei der Erstellung des Automation-Kontos erstellt wurde.

Die Verbindung für das ausführende Konto wird wie im folgenden Beispiel zu sehen mithilfe des Cmdlets [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.5.0) hergestellt. Sollten Sie Ressourcen für mehrere Abonnements verwalten, verwenden Sie [Get-AzContext](https://docs.microsoft.com/powershell/module/Az.Accounts/Get-AzContext?view=azps-3.5.0) mit dem Parameter `AzContext`.

> [!NOTE]
> Für PowerShell-Runbooks sind `Add-AzAccount` und `Add-AzureRMAccount` Aliase für `Connect-AzAccount`. Sie können diese Cmdlets verwenden, oder Sie können Ihre Module in Ihrem Automation-Konto auf die aktuellen Versionen [aktualisieren](automation-update-azure-modules.md). Möglicherweise müssen Sie Ihre Module auch dann aktualisieren, wenn Sie gerade ein neues Automation-Konto erstellt haben.

   ```powershell
   # Ensures you do not inherit an AzContext in your runbook
   Disable-AzContextAutosave –Scope Process

   $connection = Get-AutomationConnection -Name AzureRunAsConnection

   # Wrap authentication in retry logic for transient network failures
   $logonAttempt = 0
   while(!($connectionResult) -And ($logonAttempt -le 10))
   {
       $LogonAttempt++
       # Logging in to Azure...
       $connectionResult =    Connect-AzAccount `
                                  -ServicePrincipal `
                                  -Tenant $connection.TenantID `
                                  -ApplicationId $connection.ApplicationID `
                                  -CertificateThumbprint $connection.CertificateThumbprint

       Start-Sleep -Seconds 30
   }

   $AzureContext = Get-AzSubscription -SubscriptionId $connection.SubscriptionID

   Get-AzVM -ResourceGroupName myResourceGroup -AzContext $AzureContext
   ```
1. Öffnen Sie den Text-Editor, indem Sie auf der Seite **MyFirstRunbook-PowerShell** auf **Bearbeiten** klicken.
1. Die Zeile `Write-Output` wird nicht mehr benötigt. Löschen Sie sie daher einfach.
1. Geben Sie den folgenden Code ein, der für die Authentifizierung bei Ihrem ausführenden Automation-Konto zuständig ist, oder fügen Sie ihn ein.

   ```powershell
   # Ensures you do not inherit an AzContext in your runbook
   Disable-AzContextAutosave –Scope Process

   $connection = Get-AutomationConnection -Name AzureRunAsConnection

   while(!($connectionResult) -And ($logonAttempt -le 10))
   {
       $LogonAttempt++
       # Logging in to Azure...
       $connectionResult =    Connect-AzAccount `
                                  -ServicePrincipal `
                                  -Tenant $connection.TenantID `
                                  -ApplicationId $connection.ApplicationID `
                                  -CertificateThumbprint $connection.CertificateThumbprint

       Start-Sleep -Seconds 30
   }
   ```

1. Klicken Sie auf den **Testbereich**, um das Runbook zu testen.
1. Klicken Sie auf **Starten** , um den Test zu starten. Nach Abschluss des Tests sollte eine Ausgabe ähnlich der nachstehenden mit allgemeinen Informationen aus Ihrem Konto angezeigt werden. Diese Ausgabe bestätigt, dass das ausführende Konto gültig ist.

   ![Authenticate](media/automation-first-runbook-textual-powershell/runbook-auth-output.png)

## <a name="step-6---add-code-to-start-a-virtual-machine"></a>Schritt 6: Hinzufügen von Code zum Starten eines virtuellen Computers

Nachdem das Runbook jetzt in Ihrem Azure-Abonnement authentifiziert ist, können Sie Ressourcen verwalten. Fügen Sie nun einen Befehl zum Starten eines virtuellen Computers hinzu. Sie können einen beliebigen virtuellen Computer in Ihrem Azure-Abonnement auswählen und den Namen vorerst einfach im Runbook hartcodieren.

1. Fügen Sie Ihrem Runbookskript das Cmdlet [Start-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Start-AzVM?view=azps-3.5.0) hinzu, um den virtuellen Computer zu starten. Das Cmdlet startet einen virtuellen Computer mit dem Namen `VMName` und einer Ressourcengruppe namens `ResourceGroupName`, wie hier zu sehen:

   ```powershell
   # Ensures you do not inherit an AzContext in your runbook
   Disable-AzContextAutosave –Scope Process

   $connection = Get-AutomationConnection -Name AzureRunAsConnection
   while(!($connectionResult) -And ($logonAttempt -le 10))
   {
       $LogonAttempt++
       # Logging in to Azure...
       $connectionResult =    Connect-AzAccount `
                                  -ServicePrincipal `
                                  -Tenant $connection.TenantID `
                                  -ApplicationId $connection.ApplicationID `
                                  -CertificateThumbprint $connection.CertificateThumbprint

       Start-Sleep -Seconds 30
   }

   Start-AzVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName'
   ```

1. Speichern Sie das Runbook, und klicken Sie dann auf den **Testbereich**, um es zu testen.
1. Klicken Sie auf **Starten**, um den Test zu starten. Vergewissern Sie sich nach Abschluss des Tests, dass der virtuelle Computer gestartet wurde.

## <a name="step-7---add-an-input-parameter"></a>Schritt 7: Hinzufügen eines Eingabeparameters

Ihr Runbook startet momentan den virtuellen Computer, den Sie im Runbook hartcodiert haben. Das Runbook ist jedoch hilfreicher, wenn Sie beim Starten des Runbooks den virtuellen Computer angeben. Zu diesem Zweck fügen wir dem Runbook Eingabeparameter hinzu.

1. Ändern Sie im Text-Editor das Cmdlet `Start-AzVM`, um Variablen für die Parameter `VMName` und `ResourceGroupName` zu verwenden. 

   ```powershell
   Param(
    [string]$VMName,
    [string]$ResourceGroupName
   )
   # Ensures you do not inherit an AzContext in your runbook
   Disable-AzContextAutosave –Scope Process

   $connection = Get-AutomationConnection -Name AzureRunAsConnection
   while(!($connectionResult) -And ($logonAttempt -le 10))
   {
       $LogonAttempt++
       # Logging in to Azure...
       $connectionResult =    Connect-AzAccount `
                                  -ServicePrincipal `
                                  -Tenant $connection.TenantID `
                                  -ApplicationId $connection.ApplicationID `
                                  -CertificateThumbprint $connection.CertificateThumbprint

       Start-Sleep -Seconds 30
   }

   Start-AzVM -Name $VMName -ResourceGroupName $ResourceGroupName
   ```

1. Speichern Sie das Runbook, und öffnen Sie den Testbereich. Nun können Sie Werte für die beiden Eingabevariablen angeben, die im Test verwendet werden.
1. Schließen Sie den Testbereich.
1. Klicken Sie auf **Veröffentlichen** , um die neue Version des Runbooks zu veröffentlichen.
1. Beenden Sie den virtuellen Computer, den Sie zuvor gestartet haben.
1. Klicken Sie auf **Starten** , um das Runbook zu starten. 
1. Geben Sie Werte für **VMNAME** und **RESOURCEGROUPNAME** für den virtuellen Computer ein, den Sie starten möchten, und klicken Sie anschließend auf **OK**.<br><br> ![Parameter übergeben](media/automation-first-runbook-textual-powershell/automation-pass-params.png)<br>
1. Vergewissern Sie sich nach Abschluss des Runbooks, dass der virtuelle Computer gestartet wurde.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur PowerShell, einschließlich Sprachreferenz und Lernmodulen, finden Sie in der [PowerShell-Dokumentation](/powershell/scripting/overview).
* Informationen zu den ersten Schritten mit grafischen Runbooks finden Sie unter [Mein erstes grafisches Runbook](automation-first-runbook-graphical.md).
* Informationen zu den ersten Schritten mit PowerShell-Workflow-Runbooks finden Sie unter [Mein erstes PowerShell-Workflow-Runbook](automation-first-runbook-textual.md).
* Weitere Informationen zu den verschiedenen Runbooktypen sowie zu ihren Vorteilen und Einschränkungen finden Sie unter [Azure Automation-Runbooktypen](automation-runbook-types.md).
* Weitere Informationen zur PowerShell-Skriptunterstützung finden Sie unter [Ankündigung der nativen PowerShell-Skriptunterstützung in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).
