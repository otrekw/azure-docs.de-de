---
title: Mein erstes PowerShell-Workflowrunbook in Azure Automation
description: Tutorial, in dem Sie sich mit dem Erstellen, Testen und Veröffentlichen eines einfachen Textrunbooks unter Verwendung eines PowerShell-Workflows vertraut machen können.
keywords: PowerShell-Workflow, Beispiele für Powershell-Workflows, Workflow PowerShell
services: automation
ms.subservice: process-automation
ms.date: 09/24/2018
ms.topic: conceptual
ms.openlocfilehash: b96860afd649f33936ee8dd2954e6873f908a369
ms.sourcegitcommit: 0cc25b792ad6ec7a056ac3470f377edad804997a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77605058"
---
# <a name="my-first-powershell-workflow-runbook"></a>Mein erstes PowerShell-Workflow-Runbook

> [!div class="op_single_selector"]
> * [Grafisch](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell-Workflow](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)

Dieses Tutorial führt Sie durch die Erstellung eines [PowerShell-Workflow-Runbooks](automation-runbook-types.md#powershell-workflow-runbooks) in Azure Automation. Beginnen Sie mit einem einfachen Runbook, das Sie testen und veröffentlichen, während Sie erfahren, wie Sie den Status des Runbookauftrags nachverfolgen. Ändern Sie anschließend das Runbook, um damit tatsächlich Azure-Ressourcen zu verwalten. Dies wird mit dem Starten eines virtuellen Azure-Computers veranschaulicht. Abschließend fügen Sie Runbookparameter hinzu, um die Stabilität des Runbooks zu erhöhen.

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Automation-Konto](automation-offering-get-started.md) dient zur Aufbewahrung des Runbooks und zur Authentifizierung gegenüber Azure-Ressourcen. Dieses Konto muss über die Berechtigung zum Starten und Beenden des virtuellen Computers verfügen.
* Einen virtuellen Azure-Computer. Da Sie diesen Computer starten und beenden, sollte es sich nicht um eine Produktions-VM handeln.

## <a name="step-1---create-new-runbook"></a>Schritt 1: Erstellen eines neuen Runbooks

Erstellen Sie zunächst ein einfaches Runbook, das den Text „Hello World“ ausgibt.

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto.

   Die Seite des Automation-Kontos bietet einen kurzen Überblick über die Ressourcen des Kontos. Sie sollten bereits über einige Objekte verfügen. Bei den meisten Objekten handelt es sich um die in einem neuen Automation-Konto automatisch enthaltenen Module. Sie sollten auch über das Anmeldeobjekt verfügen, das Ihrem Abonnement zugeordnet ist.
 
1. Wählen Sie unter **Prozessautomatisierung** die Option **Runbooks** aus, um die Liste der Runbooks zu öffnen.
1. Erstellen Sie ein neues Runbook, indem Sie **+ Runbook erstellen** auswählen.
1. Geben Sie dem Runbook den Namen „MyFirstRunbook-Workflow“.
1. In diesem Fall erstellen Sie ein [PowerShell-Workflow-Runbook](automation-runbook-types.md#powershell-workflow-runbooks). Wählen Sie daher als **Runbooktyp** die Option **PowerShell-Workflow** aus.
1. Klicken Sie auf **Erstellen** , um das Runbook zu erstellen und den Text-Editor zu öffnen.

## <a name="step-2---add-code-to-the-runbook"></a>Schritt 2 – Hinzufügen von Code zum Runbook

Sie können direkt Code in das Runbook eingeben, oder Sie wählen Cmdlets, Runbooks und Objekte im Bibliotheksteuerelement aus und fügen diese dem Runbook mit den zugehörigen Parametern hinzu. In dieser exemplarischen Vorgehensweise geben Sie den Code direkt in das Runbook ein.

1. Ihr Runbook ist zurzeit leer und enthält lediglich das erforderliche Schlüsselwort **workflow**, den Namen des Runbooks und die geschweiften Klammern, die den gesamten Workflow umschließen.

   ```powershell-interactive
   Workflow MyFirstRunbook-Workflow
   {
   }
   ```

1. Geben Sie zwischen den geschweiften Klammern `Write-Output "Hello World"` ein.

   ```powershell-interactive
   Workflow MyFirstRunbook-Workflow
   {
   Write-Output "Hello World"
   }
   ```

1. Klicken Sie auf **Speichern**, um das Runbook zu speichern.

## <a name="step-3---test-the-runbook"></a>Schritt 3: Testen des Runbooks

Bevor Sie das Runbook für die Verwendung in der Produktionsumgebung veröffentlichen, sollten Sie es testen, um sicherzustellen, dass es ordnungsgemäß funktioniert. Beim Testen eines Runbooks führen Sie die Entwurfsversion aus und sehen sich interaktiv die Ausgabe an.

1. Wählen Sie **Testbereich** aus, um den Bereich „Test“ zu öffnen.
1. Klicken Sie auf **Starten**, um den Test zu starten, wobei der Test die einzige aktivierte Option ist.
1. Beachten Sie, dass ein [Runbookauftrag](automation-runbook-execution.md) erstellt und der zugehörige Status im Bereich angezeigt wird.

   Der Auftrag weist zunächst den Status „In Warteschlange“ auf, der angibt, dass der Auftrag darauf wartet, dass in der Cloud ein Runbook Worker verfügbar wird. Der Status ändert sich in „Wird gestartet“, wenn ein Worker den Auftrag beansprucht. Schließlich ändert sich der Status in „Wird ausgeführt“, wenn die Ausführung des Runbooks beginnt.

1. Nach Abschluss des Runbookauftrags wird die Ausgabe im Bereich „Test“ angezeigt. In diesem Fall wird „Hello World“ angezeigt.

   ![Hello World](media/automation-first-runbook-textual/test-output-hello-world.png)

1. Schließen Sie den Testbereich, um zum Zeichenbereich zurückzukehren.

## <a name="step-4---publish-and-start-the-runbook"></a>Schritt 4: Veröffentlichen und Starten des Runbooks

Das erstellte Runbook befindet sich immer noch im Entwurfsmodus. Sie müssen das Runbook zuerst veröffentlichen, um es in der Produktionsumgebung ausführen zu können. Beim Veröffentlichen eines Runbooks wird die vorhandene veröffentlichte Version durch die Entwurfsversion überschrieben. In diesem Fall ist noch keine veröffentlichte Version vorhanden, da Sie das Runbook gerade erst erstellt haben.

1. Klicken Sie auf **Veröffentlichen**, um das Runbook zu veröffentlichen, und bestätigen Sie den Vorgang mit **Ja**.
1. Scrollen Sie nach links, um das Runbook im Bereich **Runbooks** anzuzeigen, und beachten Sie, dass für **Erstellungsstatus** der Wert „Veröffentlicht“ angezeigt wird.
1. Scrollen Sie wieder nach rechts, um den Bereich für „MyFirstRunbook-Workflow“ anzuzeigen.

   Mit den Optionen im oberen Bereich können Sie das Runbook jetzt starten, den Start für einen späteren Zeitpunkt planen oder einen [Webhook](automation-webhooks.md) erstellen, um das Runbook über einen HTTP-Aufruf starten zu können.

1. Klicken Sie zum Starten des Runbooks auf **Starten** und anschließend auf **Ja**.

   ![Runbook starten](media/automation-first-runbook-textual/automation-runbook-controls-start.png)

1. Ein Auftragsbereich für den soeben erstellten Runbookauftrag wird angezeigt. Lassen Sie in diesem Fall den Bereich geöffnet, damit Sie den Fortschritt des Auftrags überwachen können.

1. Beachten Sie, dass der Auftragsstatus unter **Auftragszusammenfassung** angezeigt wird. Dieser Status stimmt mit einem der Status überein, die Sie beim Testen des Runbooks gesehen haben.

   ![API-Zusammenfassung](media/automation-first-runbook-textual/job-pane-status-blade-jobsummary.png)

1. Wenn der Runbookstatus „Abgeschlossen“ lautet, klicken Sie auf **Ausgabe**. Die Seite „Ausgabe“ wird geöffnet, und Ihre Meldung „Hello World“ wird angezeigt.

   ![API-Zusammenfassung](media/automation-first-runbook-textual/job-pane-status-blade-outputtile.png)

1. Schließen Sie die Seite „Ausgabe“.

1. Klicken Sie auf **Alle Protokolle**, um den Bereich „Datenströme“ für den Runbookauftrag zu öffnen. Im Ausgabestream sollte nur „Hello World“ angezeigt werden. Beachten Sie, dass im Bereich „Streams“ auch andere Streams für einen Runbookauftrag, etwa ausführliche Streams und Fehlerstreams, angezeigt werden können, sofern das Runbook in diese schreibt.

   ![API-Zusammenfassung](media/automation-first-runbook-textual/job-pane-status-blade-alllogstile.png)

1. Schließen Sie den Bereich „Streams“ und den Bereich „Auftrag“, um zur Seite „MyFirstRunbook“ zurückzukehren.
1. Klicken Sie unter **Ressourcen** auf **Aufträge**, um den Bereich „Aufträge“ für dieses Runbook zu öffnen. Auf dieser Seite werden alle von diesem Runbook erstellten Aufträge aufgeführt. Es sollte nur ein Auftrag aufgeführt sein, da Sie den Auftrag bislang erst einmal ausgeführt haben.

   ![Aufträge](media/automation-first-runbook-textual/runbook-control-job-tile.png)

1. Klicken Sie auf den Namen des Auftrags, um den Bereich „Auftrag“ zu öffnen, der beim Starten des Runbooks angezeigt wurde. In diesem Bereich können Sie Details zu jedem Auftrag anzeigen, der für das Runbook erstellt wurde.

## <a name="step-5---add-authentication-to-manage-azure-resources"></a>Schritt 5: Hinzufügen von Authentifizierungsfunktionen für die Verwaltung von Azure-Ressourcen

Sie haben Ihr Runbook inzwischen zwar getestet und veröffentlicht, bislang ist es aber noch nicht sonderlich hilfreich. Sie möchten damit ja eigentlich Azure-Ressourcen verwalten. Dies ist nur möglich, wenn die Authentifizierung mit den Anmeldeinformationen für das Abonnement erfolgt. Für die Authentifizierung wird das Cmdlet **Connect-AzAccount** verwendet.

>[!NOTE]
>Für PowerShell-Runbooks sind **Add-AzAccount** und **Add-AzureRMAccount** Aliase für **Connect-AzAccount**. Sie können diese Cmdlets verwenden, oder Sie können Ihre Module in Ihrem Automation-Konto auf die aktuellen Versionen [aktualisieren](automation-update-azure-modules.md). Möglicherweise müssen Sie Ihre Module auch dann aktualisieren, wenn Sie gerade ein neues Automation-Konto erstellt haben.

1. Navigieren Sie zum Bereich „MyFirstRunbook-Workflow“, und öffnen Sie den Text-Editor, indem Sie auf **Bearbeiten** klicken.
2. Löschen Sie die Zeile **Write-Output**.
3. Positionieren Sie den Cursor in einer leeren Zeile zwischen den geschweiften Klammern.
4. Geben Sie den folgenden Code ein, der für die Authentifizierung bei Ihrem ausführenden Automation-Konto zuständig ist, oder fügen Sie ihn ein.

   ```powershell-interactive
   # Ensures you do not inherit an AzContext in your runbook
   Disable-AzContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $AzureContext = Select-AzSubscription -SubscriptionId $Conn.SubscriptionID
   ```

1. Klicken Sie auf den **Testbereich**, um das Runbook zu testen.
1. Klicken Sie auf **Starten** , um den Test zu starten. Nach Abschluss des Tests sollte eine Ausgabe ähnlich der nachstehenden mit allgemeinen Informationen aus Ihrem Konto angezeigt werden. Diese Aktion bestätigt, dass die Anmeldeinformationen gültig sind.

   ![Authenticate](media/automation-first-runbook-textual/runbook-auth-output.png)

## <a name="step-6---add-code-to-start-a-virtual-machine"></a>Schritt 6: Hinzufügen von Code zum Starten eines virtuellen Computers

Nachdem das Runbook jetzt in Ihrem Azure-Abonnement authentifiziert ist, können Sie Ressourcen verwalten. Fügen Sie nun einen Befehl zum Starten eines virtuellen Computers hinzu. Sie können eine beliebige VM in Ihrem Azure-Abonnement auswählen. Vorerst geben Sie diesen Namen im Runbook fest ein. Wenn Sie Ressourcen über mehrere Abonnements verwalten, müssen Sie das Cmdlet [Get-AzContext](/powershell/module/az.accounts/get-azcontext) mit dem Parameter *AzContext* verwenden.

1. Geben Sie den Namen und den Ressourcengruppennamen der VM an, die gestartet werden soll, indem Sie wie unten gezeigt einen Aufruf des Cmdlets [Start-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Start-AzVM?view=azps-3.5.0
) eingeben. 

   ```powershell-interactive
   workflow MyFirstRunbook-Workflow
   {
   # Ensures that you do not inherit an AzContext in your runbook
   Disable-AzContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $AzureContext = Get-AzSubscription -SubscriptionId $Conn.SubscriptionID

   Start-AzVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName' -AzContext $AzureContext
   }
   ```

1. Speichern Sie das Runbook, und klicken Sie dann auf den **Testbereich**, um es zu testen.
1. Klicken Sie auf **Starten** , um den Test zu starten. Vergewissern Sie sich nach Abschluss, dass die VM gestartet wurde.

## <a name="step-7---add-an-input-parameter-to-the-runbook"></a>Schritt 7: Hinzufügen eines Eingabeparameters zum Runbook

Ihr Runbook startet derzeit die im Runbook hartcodierte VM. Es ist nützlicher, wenn Sie die VM beim Start des Runbooks angeben können. Fügen Sie zu diesem Zweck dem Runbook Eingabeparameter hinzu.

1. Fügen Sie dem Runbook Werte für *VMName* und *ResourceGroupName* hinzu, und verwenden Sie die zugehörigen Variablen mit dem Cmdlet **Start-AzVM**, wie nachstehend gezeigt.

   ```powershell-interactive
   workflow MyFirstRunbook-Workflow
   {
    Param(
     [string]$VMName,
     [string]$ResourceGroupName
    )
   # Ensures you do not inherit an AzContext in your runbook
   Disable-AzContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzVM -Name $VMName -ResourceGroupName $ResourceGroupName
   }
   ```

2. Speichern Sie das Runbook, und öffnen Sie den Testbereich. Sie können nun Werte für die beiden Eingabevariablen angeben, die sich im Testbereich befinden.
3. Schließen Sie den Testbereich.
4. Klicken Sie auf **Veröffentlichen** , um die neue Version des Runbooks zu veröffentlichen.
5. Beenden Sie den virtuellen Computer, den Sie gestartet haben.
6. Klicken Sie auf **Starten** , um das Runbook zu starten. 
7. Geben Sie die Werte **VMName** und **ResourceGroupName** für die VM ein, die Sie starten möchten.

   ![Start des Runbooks](media/automation-first-runbook-textual/automation-pass-params.png)

8. Wenn das Runbook abgeschlossen ist, vergewissern Sie sich, dass die VM gestartet wurde.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu PowerShell, einschließlich Sprachreferenz und Lernmodulen, finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/scripting/overview).
* Informationen zu den ersten Schritten mit grafischen Runbooks finden Sie unter [Mein erstes grafisches Runbook](automation-first-runbook-graphical.md).
* Erste Schritte mit PowerShell-Runbooks werden unter [Mein erstes PowerShell-Runbook](automation-first-runbook-textual-powershell.md) beschrieben.
* Informationen zu den verschiedenen Runbooktypen mit ihren Vorteilen und Einschränkungen finden Sie unter [Azure Automation-Runbooktypen](automation-runbook-types.md).
* Weitere Informationen zur PowerShell-Skriptunterstützung finden Sie unter [Native PowerShell Script Support in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/) (Native PowerShell-Skriptunterstützung in Azure Automation).
