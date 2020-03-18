---
title: Mein erstes grafisches Runbook in Azure Automation
description: Tutorial, in dem Sie sich mit dem Erstellen, Testen und Veröffentlichen eines einfachen grafischen Runbooks vertraut machen können.
keywords: Runbooks, Runbookvorlage, Runbookautomatisierung, Azure-Runbook
services: automation
ms.subservice: process-automation
ms.date: 04/13/2018
ms.topic: conceptual
ms.openlocfilehash: 6a967f328a4fbe17f2c451d35f413bd7fdcbc24a
ms.sourcegitcommit: 021ccbbd42dea64d45d4129d70fff5148a1759fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2020
ms.locfileid: "78331467"
---
# <a name="my-first-graphical-runbook"></a>Mein erstes grafisches Runbook

> [!div class="op_single_selector"]
> * [Grafisch](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell-Workflow](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)
> 

In diesem Tutorial werden die Schritte zur Erstellung eines [grafischen Runbooks](automation-runbook-types.md#graphical-runbooks) in Azure Automation beschrieben. Beginnen Sie mit einem einfachen Runbook, das Sie testen und veröffentlichen können, während Sie erfahren, wie Sie den Status des Runbookauftrags nachverfolgen. Anschließend ändern Sie das Runbook, um damit tatsächlich Azure-Ressourcen zu verwalten. Im vorliegenden Fall soll ein virtueller Azure-Computer gestartet werden. Vervollständigen Sie das Tutorial, um die Stabilität des Runbooks durch Hinzufügen von Runbookparametern und bedingten Verknüpfungen zu erhöhen.

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Automation-Konto](automation-offering-get-started.md) dient zur Aufbewahrung des Runbooks und zur Authentifizierung gegenüber Azure-Ressourcen. Dieses Konto muss über die Berechtigung zum Starten und Beenden des virtuellen Computers verfügen.
* Einen virtuellen Azure-Computer. Da Sie diesen Computer starten und beenden, sollte es sich nicht um eine Produktions-VM handeln.

## <a name="step-1---create-runbook"></a>Schritt 1: Erstellen eines Runbooks

Erstellen Sie zunächst ein einfaches Runbook, das den Text **Hello World** ausgibt.

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto. 

    Die Seite des Automation-Kontos bietet einen kurzen Überblick über die Ressourcen des Kontos. Sie sollten bereits über einige Objekte verfügen. Bei den meisten Objekten handelt es sich um die in einem neuen Automation-Konto automatisch enthaltenen Module. Sie sollten auch über das Anmeldeobjekt verfügen, das Ihrem Abonnement zugeordnet ist.
2. Wählen Sie unter **Prozessautomatisierung** die Option **Runbooks** aus, um die Liste der Runbooks zu öffnen.
3. Erstellen Sie ein neues Runbook, indem Sie **Runbook erstellen** auswählen.
4. Nennen Sie das Runbook **MyFirstRunbook-Graphical**.
5. In diesem Fall erstellen Sie ein [grafisches Runbook](automation-graphical-authoring-intro.md). Wählen Sie **Grafisch** für den **Runbooktyp** aus.<br> ![Neues Runbook](media/automation-first-runbook-graphical/create-new-runbook.png)<br>
6. Klicken Sie auf **Erstellen** , um das Runbook zu erstellen und den grafischen Editor zu öffnen.

## <a name="step-2---add-activities"></a>Schritt 2: Hinzufügen von Aktivitäten

Mithilfe des Bibliotheksteuerelements auf der linken Seite des Editors können Sie Aktivitäten auswählen und Ihrem Runbook hinzufügen. Sie fügen ein **Write-Output**-Cmdlet hinzu, um Text aus dem Runbook auszugeben.

1. Klicken Sie im Bibliothekssteuerelement in das Suchfeld, und geben Sie **write-output** ein. Die folgende Abbildung zeigt die Suchergebnisse. <br> ![Microsoft.PowerShell.Utility](media/automation-first-runbook-graphical/search-powershell-cmdlet-writeoutput.png)
1. Führen Sie einen Bildlauf zum Ende der Liste durch. Klicken Sie mit der rechten Maustaste auf **Write-Output**, und wählen Sie **Zur Canvas hinzufügen** aus. Alternativ können Sie auf die Auslassungspunkte (...) neben dem Namen des Cmdlets klicken und dann **Zur Canvas hinzufügen** auswählen.
1. Klicken Sie in der Canvas auf die Aktivität **Write-Output**. Über diese Aktion wird die Seite für die Konfigurationssteuerung geöffnet, auf der Sie die Aktivität konfigurieren können.
1. Das Feld **Bezeichnung** ist standardmäßig auf den Namen des Cmdlets festgelegt, kann aber in einen aussagekräftigeren Wert geändert werden. Legen Sie ihn auf **Write Hello World to output**fest.
1. Klicken Sie auf **Parameter** , um Parameterwerte für das Cmdlet anzugeben.

   Einige Cmdlets verfügen über mehrere Parametersätze, und Sie müssen auswählen, welche verwendet werden. **Write-Output** besitzt in diesem Fall lediglich einen einzelnen Parametersatz.

1. Wählen Sie den *InputObject* -Parameter aus. Dies ist der Parameter, mit dem Sie den Text angeben, der an den Ausgabedatenstrom gesendet werden soll.
1. Das Dropdownmenü **Datenquelle** enthält Quellen, die Sie zum Auffüllen eines Parameterwerts verwenden können. Wählen Sie in diesem Menü **PowerShell-Ausdruck** aus. 

   Sie können Ausgaben von solchen Quellen (etwa eine andere Aktivität, ein Automation-Objekt oder ein PowerShell-Ausdruck) verwenden. In diesem Fall soll lediglich **Hello World** ausgegeben werden. Hierzu können Sie einen PowerShell-Ausdruck verwenden und eine Zeichenfolge angeben.<br>

1. Geben Sie in das Feld **Ausdruck** **Hallo Welt** ein, und klicken Sie dann zweimal auf **OK**, um zur Canvas zurückzukehren.
1. Klicken Sie auf **Speichern**, um das Runbook zu speichern.

## <a name="step-3---test-the-runbook"></a>Schritt 3: Testen des Runbooks

Bevor Sie das Runbook für die Verwendung in der Produktionsumgebung veröffentlichen, sollten Sie es testen, um sicherzustellen, dass es ordnungsgemäß funktioniert. Beim Testen eines Runbooks führen Sie die Entwurfsversion aus und sehen sich interaktiv die Ausgabe an.

1. Klicken Sie auf **Testbereich**, um den Bereich „Test“ zu öffnen.
1. Klicken Sie auf **Starten** , um den Test zu starten. Andere Optionen dürften nicht zur Verfügung stehen.
1. Beachten Sie, dass ein [Runbookauftrag](automation-runbook-execution.md) erstellt und der zugehörige Status im Bereich angezeigt wird.

   Der Auftrag weist zunächst den Status **In Warteschlange** auf, der angibt, dass der Auftrag darauf wartet, dass in der Cloud ein Runbook Worker verfügbar wird. Der Status ändert sich in **Wird gestartet**, wenn ein Worker den Auftrag beansprucht. Schließlich ändert sich der Status in **Wird ausgeführt**, wenn die Ausführung des Runbooks beginnt.

1. Nach Abschluss des Runbookauftrags wird die Ausgabe im Bereich „Test“ angezeigt. In diesem Fall wird **Hello World** angezeigt.<br> ![Hello World](media/automation-first-runbook-graphical/runbook-test-results.png)
1. Schließen Sie den Testbereich, um zum Zeichenbereich zurückzukehren.

## <a name="step-4---publish-and-start-the-runbook"></a>Schritt 4: Veröffentlichen und Starten des Runbooks

Das erstellte Runbook befindet sich immer noch im Entwurfsmodus. Es muss veröffentlicht werden, damit es in der Produktionsumgebung ausgeführt werden kann. Beim Veröffentlichen eines Runbooks wird die vorhandene veröffentlichte Version durch die Entwurfsversion überschrieben. In diesem Fall ist noch keine veröffentlichte Version vorhanden, da Sie das Runbook gerade erst erstellt haben.

1. Klicken Sie auf **Veröffentlichen**, um das Runbook zu veröffentlichen, und bestätigen Sie den Vorgang mit **Ja**.
1. Scrollen Sie nach links, um das Runbook auf der Seite „Runbooks“ anzuzeigen, und beachten Sie, dass der Wert **Erstellungsstatus** als **Veröffentlicht** angezeigt wird.
1. Scrollen Sie zurück nach rechts, um die Seite für **MyFirstRunbook-Graphical** anzuzeigen.

   Mit den Optionen im oberen Bereich können Sie das Runbook jetzt starten, den Start für einen späteren Zeitpunkt planen oder einen [Webhook](automation-webhooks.md) erstellen, um das Runbook über einen HTTP-Aufruf starten zu können.

1. Klicken Sie zum Starten des Runbooks auf **Starten** und anschließend auf **Ja**.
1. Ein Auftragsbereich für den soeben erstellten Runbookauftrag wird angezeigt. Vergewissern Sie sich, dass im Feld **Auftragsstatus** der Wert **Abgeschlossen** angezeigt wird.
1. Klicken Sie auf **Ausgabe**, um die Ausgabeseite zu öffnen, auf der **Hallo Welt** angezeigt wird.
1. Schließen Sie die Seite „Ausgabe“.
1. Klicken Sie auf **Alle Protokolle**, um den Bereich „Datenströme“ für den Runbookauftrag zu öffnen. Im Ausgabedatenstrom sollte nur **Hallo Welt** angezeigt werden. 

    Beachten Sie, dass im Bereich „Streams“ auch andere Streams für einen Runbookauftrag, etwa ausführliche Streams und Fehlerstreams, angezeigt werden können, sofern das Runbook in diese schreibt.
1. Schließen Sie den Datenstrom- und den Auftragsbereich, um zum Bereich **MyFirstRunbook-Graphical** zurückzukehren.
1. Wählen Sie **Aufträge** unter **Ressourcen** aus, um alle Aufträge für das Runbook anzuzeigen. Auf der Seite „Aufträge“ werden alle von Ihrem Runbook erstellten Aufträge aufgelistet. Es sollte nur ein Auftrag aufgeführt sein, da Sie den Auftrag bislang erst einmal ausgeführt haben.
1. Klicken Sie auf den Namen des Auftrags, um den Bereich „Auftrag“ zu öffnen, der beim Starten des Runbooks angezeigt wurde. In diesem Bereich können Sie die Details zu jedem Auftrag anzeigen, der für das Runbook erstellt wurde.

## <a name="step-5---create-variable-assets"></a>Schritt 5: Erstellen variabler Ressourcen

Sie haben Ihr Runbook inzwischen zwar getestet und veröffentlicht, bislang ist es aber zum Verwalten von Azure-Ressourcen noch nicht sonderlich hilfreich. Bevor Sie das Runbook für die Authentifizierung konfigurieren, müssen Sie eine Variable erstellen, die die Abonnement-ID enthält, eine Aktivität zur Authentifizierung einrichten und dann auf die Variable verweisen. Das Einfügen eines Verweises in den Abonnementkontext ermöglicht Ihnen die einfache Nutzung mehrerer Abonnements.

1. Kopieren Sie Ihre Abonnement-ID aus der Option **Abonnements** im Navigationsbereich.
1. Klicken Sie auf der Seite „Automation-Konten“ unter **Freigegebene Ressourcen** auf **Variablen**.
1. Klicken Sie auf **Variable hinzufügen**.
1. Nehmen Sie auf der Seite „Neue Variable“ die folgenden Einstellungen in den dafür vorgesehenen Feldern vor.

    * **Name**: Geben Sie **AzureSubscriptionId** ein.
    * **Wert**: Geben Sie Ihre Abonnement-ID ein. 
    * **Typ**: Lassen Sie die Zeichenfolge ausgewählt.
    * **Verschlüsselung**: Andernfalls verwenden Sie den Standardwert.
1. Klicken Sie auf **Erstellen** , um die Variable zu erstellen.

## <a name="step-6---add-authentication"></a>Schritt 6: Hinzufügen der Authentifizierung

Nachdem Sie nun über eine Variable für Ihre Abonnement-ID verfügen, können Sie das Runbook so konfigurieren, dass die Authentifizierung mit den Anmeldeinformationen für das ausführende Konto für Ihr Abonnement erfolgt. Fügen Sie dazu die Verbindung für das ausführende Azure-Konto als Objekt hinzu. Sie müssen auch die Cmdlets [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/Connect-AzAccount?view=azps-3.5.0) und [Set-AzContext](https://docs.microsoft.com/powershell/module/az.accounts/Set-AzContext?view=azps-3.5.0) zur Canvas hinzufügen.

>[!NOTE]
>Für PowerShell-Runbooks sind **Add-AzAccount** und **Add-AzureRMAccount** Aliase für **Connect-AzAccount**. Beachten Sie, dass diese Aliase für Ihre grafischen Runbooks nicht verfügbar sind. Ein grafisches Runbook kann nur **Connect-AzAccount** selbst verwenden.

1. Navigieren Sie zu Ihrem Runbook, und klicken Sie auf der Seite **MyFirstRunbook-Graphical** auf **Bearbeiten**.
1. Den Eintrag **Write Hallo Welt to output** benötigen Sie nicht mehr. Klicken Sie einfach auf die Auslassungspunkte, und wählen Sie **Löschen** aus.
1. Erweitern Sie im Bibliothekssteuerelement **OBJEKTE** und dann **Verbindungen**. Fügen Sie **AzureRunAsConnection** zur Canvas hinzu, indem Sie **Zur Canvas hinzufügen** auswählen.
1. Geben Sie im Bibliotheksteuerelement in das Suchfeld die Zeichenfolge **Connect-AzAccount** ein.
1. Fügen Sie **Connect-AzAccount** der Canvas hinzu.
1. Zeigen Sie auf **Get Run As Connection** , bis am unteren Rand der Form ein Kreis erscheint. Klicken Sie auf den Kreis, und ziehen Sie den Pfeil zu **Connect-AzAccount**, um eine Verknüpfung herzustellen. Das Runbook wird mit **Get Run As Connection** gestartet und führt dann **Connect-AzAccount** aus.<br> ![Verknüpfung zwischen Aktivitäten erstellen](media/automation-first-runbook-graphical/runbook-link-auth-activities.png)
1. Wählen Sie auf der Canvas die Option **Connect-AzAccount** aus. Geben Sie im Konfigurationssteuerungsbereich **Anmelden bei Azure** in das Feld **Bezeichnung** ein.
1. Klicken Sie auf **Parameter**, um die Seite „Konfiguration der Aktivitätsparameter“ anzuzeigen.
1. Das Cmdlet **Connect-AzAccount** verfügt über mehrere Parametersätze, und Sie müssen einen auswählen, bevor Sie Parameterwerte bereitstellen können. Klicken Sie auf **Parametersatz**, und wählen Sie dann den Parametersatz **ServicePrincipalCertificate** aus.
1. Die Parameter für diesen Parametersatz werden auf der Seite „Konfiguration der Aktivitätsparameter“ angezeigt. Klicken Sie auf **APPLICATIONID**.<br> ![Azure-Kontoparameter hinzufügen](media/automation-first-runbook-graphical/Add-AzureRmAccount-params.png)
1. Nehmen Sie auf der Seite „Parameterwert“ die folgenden Einstellungen vor, und klicken Sie dann auf **OK**.

   * **Datenquelle**: Wählen Sie **Aktivitätsausgabe** aus.
   * Datenquellenliste: Wählen Sie **Get Run As Connection** aus.
   * **Feldpfad**: Geben Sie **ApplicationId** ein. Geben Sie den Namen der Eigenschaft für den Feldpfad an, da die Aktivität ein Objekt mit mehreren Eigenschaften ausgibt.
1. Klicken Sie auf **CERTIFICATETHUMBPRINT**, nehmen Sie auf der Seite „Parameterwert“ die folgenden Einstellungen vor, und klicken Sie dann auf **OK**.

    * **Datenquelle**: Wählen Sie **Aktivitätsausgabe** aus.
    * Datenquellenliste: Wählen Sie **Get Run As Connection** aus.
    * **Feldpfad**: Geben Sie **CertificateThumbprint** ein.
1. Klicken Sie auf **SERVICEPRINCIPAL**, und wählen Sie auf der Seite „Parameterwert“ für das Feld **Datenquelle** die Option **ConstantValue** aus. Klicken Sie auf die Option **True** und dann auf **OK**.
1. Klicken Sie auf **TENANTID**, und nehmen Sie auf der Seite „Parameterwert“ die folgenden Einstellungen vor. Wenn Sie fertig sind, klicken Sie zweimal auf **OK**.

    * **Datenquelle**: Wählen Sie **Aktivitätsausgabe** aus. 
    * Datenquellenliste: Wählen Sie **Get Run As Connection** aus.
    * **Feldpfad**: Geben Sie **TenantId** ein. 
1. Geben Sie im Bibliotheksteuerelement in das Suchfeld die Zeichenfolge **Set-AzContext** ein.
1. Fügen Sie **Set-AzContext** zur Canvas hinzu.
1. Wählen Sie **Set-AzContext** auf der Canvas aus. Geben Sie im Konfigurationssteuerungsbereich **Abonnement-ID angeben** in das Feld **Bezeichnung** ein.
1. Klicken Sie auf **Parameter**, um die Seite „Konfiguration der Aktivitätsparameter“ anzuzeigen.
1. Das Cmdlet **Set-AzContext** verfügt über mehrere Parametersätze, und Sie müssen einen auswählen, bevor Sie Parameterwerte bereitstellen können. Klicken Sie auf **Parametersatz**, und wählen Sie anschließend den Parametersatz **SubscriptionId** aus.
1. Die Parameter für diesen Parametersatz werden auf der Seite „Konfiguration der Aktivitätsparameter“ angezeigt. Klicken Sie auf **SubscriptionID**.
1. Wählen Sie auf der Seite „Parameterwert“ für das Feld **Datenquelle** die Option **Variablenasset** und in der Liste die Option **AzureSubscriptionId** aus. Wenn Sie fertig sind, klicken Sie zweimal auf **OK**.
1. Zeigen Sie auf **Login to Azure** , bis am unteren Rand der Form ein Kreis erscheint. Klicken Sie auf den Kreis, und ziehen Sie den Pfeil auf **Specify Subscription Id**. 

Ihr Runbook sollte nun wie folgt aussehen: <br>![Konfiguration der Runbook-Authentifizierung](media/automation-first-runbook-graphical/runbook-auth-config.png)

## <a name="step-7---add-activity-to-start-a-virtual-machine"></a>Schritt 7: Hinzufügen einer Aktivität zum Starten eines virtuellen Computers

Jetzt müssen Sie eine Aktivität vom Typ **Start-AzVM** hinzufügen, um einen virtuellen Computer zu starten. Sie können einen beliebigen virtuellen Computer in Ihrem Azure-Abonnement auswählen. Sie werden seinen Namen vorerst im Cmdlet [Start-AzVM](https://docs.microsoft.com/powershell/module/az.compute/start-azvm?view=azps-3.5.0) hartcodieren.

1. Geben Sie im Bibliotheksteuerelement in das Suchfeld die Zeichenfolge **Start-Az** ein.
2. Fügen Sie **Start-AzVM** der Canvas hinzu, und ziehen Sie das Element anschließend unter **Specify Subscription Id**.
1. Zeigen Sie auf **Specify Subscription Id** , bis am unteren Rand der Form ein Kreis erscheint. Klicken Sie auf den Kreis, und ziehen Sie den Pfeil zu **Start-AzVM**.
1. Wählen Sie **Start-AzVM** aus. Klicken Sie auf **Parameter** und anschließend auf **Parametersatz**, um die Sätze für die Aktivität anzuzeigen.
1. Wählen Sie den Parametersatz **ResourceGroupNameParameterSetName** aus. Die Felder **ResourceGroupName** und **Name** verfügen über nebenstehende Ausrufezeichen, um anzuzeigen, dass es sich um erforderliche Parameter handelt. Beachten Sie, dass beide Felder Zeichenfolgenwerte erwarten.
1. Wählen Sie **Name**. Wählen Sie **PowerShell-Ausdruck** für das Feld **Datenquelle** aus. Geben Sie für den virtuellen Computer, mit dem Sie dieses Runbook starten, den Computernamen in doppelten Anführungszeichen ein. Klicken Sie auf **OK**.
1. Wählen Sie **ResourceGroupName**aus. Verwenden Sie den Wert **PowerShell-Ausdruck** für das Feld **Datenquelle**, und geben Sie den Namen der Ressourcengruppe in doppelten Anführungszeichen ein. Klicken Sie auf **OK**.
1. Klicken Sie auf den **Testbereich**, um das Runbook zu testen.
1. Klicken Sie auf **Starten**, um den Test zu starten. Vergewissern Sie sich nach Abschluss, dass die VM gestartet wurde. 

Ihr Runbook sollte nun wie folgt aussehen: <br>![Konfiguration der Runbook-Authentifizierung](media/automation-first-runbook-graphical/runbook-startvm.png)

## <a name="step-8---add-additional-input-parameters"></a>Schritt 8: Hinzufügen zusätzlicher Eingabeparameter

Ihr Runbook startet den virtuellen Computer derzeit in der Ressourcengruppe, die Sie im Cmdlet **Start-AzVM** angegeben haben. Das Runbook wird hilfreicher sein, wenn Sie beim Starten des Runbooks sowohl den Namen als auch die Ressourcengruppe angeben. Zu diesem Zweck fügen wir dem Runbook Eingabeparameter hinzu.

1. Öffnen Sie den grafischen Editor, indem Sie im Bereich **MyFirstRunbook-Graphical** auf **Bearbeiten** klicken.
1. Klicken Sie auf **Eingabe und Ausgabe** und anschließend auf **Eingabe hinzufügen**, um den Eingabeparameterbereich für das Runbook zu öffnen.
1. Nehmen Sie die folgenden Einstellungen in den vorgesehenen Feldern vor, und klicken Sie dann auf **OK**.
   * **Name**: Geben Sie **VMName** an.
   * **Typ**: Behalten Sie die Einstellung „String“ (Zeichenfolge) bei.
   * **Obligatorisch**: Ändern Sie den Wert in **Yes** (Ja).
1. Erstellen Sie einen zweiten obligatorischen Eingabeparameter namens *ResourceGroupName*, und klicken Sie anschließend auf **OK**, um den Bereich „Eingabe und Ausgabe“ zu schließen.<br> ![Runbook-Eingabeparameter](media/automation-first-runbook-graphical/start-azurermvm-params-outputs.png)
1. Wählen Sie die Aktivität **Start-AzVM** aus, und klicken Sie anschließend auf **Parameter**.
1. Ändern Sie das Feld **Datenquelle** für **Name** in **Runbookeingabe**. Wählen Sie dann **VMName** aus.
1. Ändern Sie das Feld **Datenquelle** für **ResourceGroupName** in **Runbookeingabe**, und wählen Sie anschließend **ResourceGroupName** aus.<br> ![Start-AzVM-Parameter](media/automation-first-runbook-graphical/start-azurermvm-params-runbookinput.png)
1. Speichern Sie das Runbook, und öffnen Sie den Testbereich. Nun können Sie Werte für die beiden Eingabevariablen angeben, die im Test verwendet werden.
1. Schließen Sie den Testbereich.
1. Klicken Sie auf **Veröffentlichen** , um die neue Version des Runbooks zu veröffentlichen.
1. Beenden Sie den virtuellen Computer, den Sie zuvor gestartet haben.
1. Klicken Sie auf **Starten** , um das Runbook zu starten. Geben Sie die Werte **VMName** und **ResourceGroupName** für die VM ein, die Sie starten möchten.
1. Wenn das Runbook abgeschlossen ist, vergewissern Sie sich, dass die VM gestartet wurde.

## <a name="step-9---create-a-conditional-link"></a>Schritt 9: Erstellen einer bedingten Verknüpfung

In diesem Schritt können Sie nun das Runbook so anpassen, dass nur dann versucht wird, den virtuellen Computer zu starten, wenn dieser noch nicht gestartet ist. Fügen Sie dazu das Cmdlet [Get-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Get-AzVM?view=azps-3.5.0) hinzu, das den Status der VM auf Instanzebene abruft. Anschließend können Sie ein PowerShell-Workflow-Codemodul namens **Get Status** mit einem PowerShell-Codeausschnitt hinzufügen, mit dem ermittelt wird, ob der virtuelle Computer ausgeführt wird oder beendet ist. Eine bedingte Verknüpfung des Moduls **Get Status** führt **Start-AzVM** nur dann aus, wenn der aktuelle Ausführungszustand angibt, dass der virtuelle Computer beendet ist. Am Ende dieses Vorgangs gibt Ihr Runbook mit dem Cmdlet **Write-Output** eine Meldung aus, die Sie darüber informiert, ob die VM erfolgreich gestartet wurde.

1. Öffnen Sie **MyFirstRunbook-Graphical** im grafischen Editor.
1. Entfernen Sie die Verknüpfung zwischen **Specify Subscription Id** und **Start-AzVM**, indem Sie auf die Verknüpfung klicken und die **ENTF** drücken.
1. Geben Sie im Bibliotheksteuerelement in das Suchfeld die Zeichenfolge **Get-Az** ein.
1. Fügen Sie der Canvas **Get-AzVM** hinzu.
1. Wählen Sie **Get-AzVM** und anschließend **Parametersatz** aus, um die Sätze für das Cmdlet anzuzeigen. 
1. Wählen Sie den Parametersatz **GetVirtualMachineInResourceGroupNameParamSet** aus. Die Felder **ResourceGroupName** und **Name** verfügen über nebenstehende Ausrufezeichen, um anzuzeigen, dass sie erforderliche Parameter angeben. Beachten Sie, dass beide Felder Zeichenfolgenwerte erwarten.
1. Wählen Sie unter **Datenquelle** für **Name** die Option **Runbookeingabe** und anschließend **VMName** aus. Klicken Sie auf **OK**.
1. Wählen Sie unter **Datenquelle** für **ResourceGroupName** die Option **Runbookeingabe** und anschließend **ResourceGroupName** aus. Klicken Sie auf **OK**.
1. Wählen Sie unter **Datenquelle** als **Status** die Option **Konstanter Wert** und anschließend **True** aus. Klicken Sie auf **OK**.
1. Erstellen Sie eine Verknüpfung zwischen **Specify Subscription Id** und **Get-AzVM**.
1. Erweitern Sie im Bibliotheksteuerelement den Bereich **Runbooksteuerung**, und fügen Sie der Canvas **Code** hinzu.  
1. Erstellen Sie eine Verknüpfung zwischen **Get-AzVM** und **Code**.  
1. Klicken Sie auf **Code**, und ändern Sie im Konfigurationsbereich die Bezeichnung in **Get Status**.
1. Wählen Sie **Code** aus, um die Seite „Code-Editor“ anzuzeigen.  
1. Fügen Sie den folgenden Codeausschnitt auf der Editorseite ein.

    ```powershell-interactive
     $StatusesJson = $ActivityOutput['Get-AzVM'].StatusesText
     $Statuses = ConvertFrom-Json $StatusesJson
     $StatusOut =""
     foreach ($Status in $Statuses){
     if($Status.Code -eq "Powerstate/running"){$StatusOut = "running"}
     elseif ($Status.Code -eq "Powerstate/deallocated") {$StatusOut = "stopped"}
     }
     $StatusOut
     ```

1. Erstellen Sie eine Verknüpfung zwischen **Get Status** und **Start-AzVM**.<br> ![Runbook mit Codemodul](media/automation-first-runbook-graphical/runbook-startvm-get-status.png)  
1. Wählen Sie die Verknüpfung aus, und legen Sie **Bedingung anwenden** im Konfigurationsbereich auf **Yes** (Ja) fest. Beachten Sie, dass die Verknüpfung nun als gestrichelte Linie dargestellt wird. Das bedeutet, dass die Aktivität nur ausgeführt wird, wenn die Bedingung erfüllt ist.  
1. Geben Sie für **Bedingungsausdruck** den Wert `$ActivityOutput['Get Status'] -eq "Stopped"` ein. **Start-AzVM** wird jetzt nur noch ausgeführt, wenn die VM beendet wird.
1. Erweitern Sie im Bibliotheksteuerelement den Knoten **Cmdlets** und anschließend **Microsoft.PowerShell.Utility**.
1. Fügen Sie dem Zeichenbereich zweimal **Write-Output** hinzu.
1. Klicken Sie für das erste Steuerelement vom Typ **Write-Output** auf **Parameter**, und legen Sie die **Bezeichnung** auf **Notify VM Started** fest.
1. Legen Sie die **Datenquelle** für **InputObject** auf **PowerShell-Ausdruck** fest, und geben Sie den Ausdruck **$VMName successfully started.** ein.
1. Klicken Sie im zweiten Steuerelement vom Typ **Write-Output** auf **Parameter**, und legen Sie die **Bezeichnung** auf **Notify VM Start Failed** fest.
1. Legen Sie die **Datenquelle** für **InputObject** auf **PowerShell-Ausdruck** fest, und geben Sie den Ausdruck **$VMName could not start.** ein.
1. Verknüpfen Sie **Start AzVM** mit **Notify VM Started** und **Notify VM Start Failed**.
1. Wählen Sie die Verknüpfung mit **Notify VM Started** aus, und legen Sie **Bedingung anwenden** auf „true“ fest.
1. Geben Sie für den **Bedingungsausdruck** den Wert `$ActivityOutput['Start-AzVM'].IsSuccessStatusCode -eq $true` ein. Dieses **Write-Output**-Steuerelement wird nur ausgeführt, wenn der virtuelle Computer erfolgreich gestartet wurde.
1. Wählen Sie die Verknüpfung mit **Notify VM Start Failed** aus, und legen Sie **Bedingung anwenden** auf „true“ fest.
1. Geben Sie für das Feld **Bedingungsausdruck** den Wert `$ActivityOutput['Start-AzVM'].IsSuccessStatusCode -ne $true` ein. Dieses **Write-Output**-Steuerelement wird nur ausgeführt, wenn der virtuelle Computer nicht erfolgreich gestartet wurde. Ihr Runbook sollte nun wie in der folgenden Abbildung aussehen: <br> ![Runbook mit Write-Output](media/automation-first-runbook-graphical/runbook-startazurermvm-complete.png)
1. Speichern Sie das Runbook, und öffnen Sie den Testbereich.
1. Wenn Sie nun das Runbook starten, während der virtuellen Computer beendet ist, sollte der Computer gestartet werden.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur grafischen Inhaltserstellung finden Sie unter [Grafische Erstellung in Azure Automation](automation-graphical-authoring-intro.md).
* Erste Schritte mit PowerShell-Runbooks werden unter [Mein erstes PowerShell-Runbook](automation-first-runbook-textual-powershell.md) beschrieben.
* Die ersten Schritte mit PowerShell-Workflow-Runbooks sind unter [Mein erstes PowerShell-Workflow-Runbook](automation-first-runbook-textual.md)beschrieben.