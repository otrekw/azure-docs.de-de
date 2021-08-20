---
title: Erstellen eines grafischen Runbooks in Azure Automation
description: In diesem Artikel erfahren Sie, wie Sie ein grafisches Runbook in Azure Automation erstellen, testen und veröffentlichen.
services: automation
ms.subservice: process-automation
ms.date: 07/16/2021
ms.topic: tutorial
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: e782fd046af23beb5c84c8e419cd5925db7230ce
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114449047"
---
# <a name="tutorial-create-a-graphical-runbook"></a>Tutorial: Erstellen eines grafischen Runbooks

In diesem Tutorial werden die Schritte zur Erstellung eines [grafischen Runbooks](../automation-runbook-types.md#graphical-runbooks) in Azure Automation beschrieben. Sie können grafische PowerShell-Workflow-Runbooks im grafischen Editor im Azure-Portal erstellen und bearbeiten.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines einfachen grafischen Runbooks
> * Testen und Veröffentlichen des Runbooks
> * Ausführen des Runbookauftrags und Nachverfolgen seines Status
> * Aktualisieren des Runbooks zum Starten eines virtuellen Azure-Computers mit Runbookparametern und bedingten Links

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* [Automation-Konto](../index.yml) mit einem [ausführenden Azure-Konto](../create-run-as-account.md) zum Speichern des Runbooks und zum Authentifizieren gegenüber Azure-Ressourcen. Dieses Konto muss über die Berechtigung zum Starten und Beenden des virtuellen Computers verfügen.
* PowerShell-Module **Az.Accounts** und **Az.Compute** für das Automation-Konto. Weitere Informationen finden Sie unter [Verwalten von Modulen in Azure Automation](../shared-resources/modules.md).
* [Virtueller Azure-Computer](../../virtual-machines/windows/quick-create-portal.md) (Virtual Machine, VM). Da Sie diesen Computer starten und beenden, sollte es sich nicht um eine Produktions-VM handeln. Der virtuelle Computer muss sich zu Beginn im Zustand **Beendet** befinden.

## <a name="create-runbook"></a>Runbook erstellen

Erstellen Sie zunächst ein einfaches Runbook, das den Text `Hello World` ausgibt.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Navigieren Sie im Azure-Portal zu Ihrem Automation-Konto.

1. Wählen Sie unter **Prozessautomatisierung** die Option **Runbooks** aus, um die Seite **Runbooks** zu öffnen.

1. Wählen Sie **Runbook erstellen** aus, um die Seite **Runbook erstellen** zu öffnen.

1. Nennen Sie das Runbook `MyFirstRunbook-Graphical`.

1. Wählen Sie im Dropdownmenü **Runbooktyp** die Option **Grafisch** aus.

   :::image type="content" source="../media/automation-tutorial-runbook-graphical/create-graphical-runbook.png" alt-text="Erstellen einer Runbookeingabeseite":::

1. Wählen Sie **Erstellen** aus, um das Runbook zu erstellen und den grafischen Editor (Seite **Grafisches Runbook bearbeiten**) zu öffnen.

## <a name="add-activities"></a>Aktivitäten hinzufügen

Auf der linken Seite des Editors befindet sich das Steuerelement **Bibliothek**. In der Mitte befindet sich die **Canvas**. Auf der rechten Seite befindet sich die **Konfigurationssteuerung**. Über das Steuerelement **Bibliothek** können Sie die Aktivitäten auswählen, die Ihrem Runbook hinzugefügt werden sollen. Sie fügen ein `Write-Output`-Cmdlet hinzu, um Text aus dem Runbook auszugeben.

1. Geben Sie in das Suchfeld des Steuerelements **Bibliothek** den Suchbegriff `Write-Output` ein.

    ![Microsoft.PowerShell.Utility](../media/automation-tutorial-runbook-graphical/search-powershell-cmdlet-writeoutput.png)

1. Führen Sie einen Bildlauf zum Ende der Liste durch. Klicken Sie mit der rechten Maustaste auf **Write-Output**, und wählen Sie **Zur Canvas hinzufügen** aus. Sie können auch die Auslassungspunkte (...) neben dem Namen des Cmdlets und anschließend **Zur Canvas hinzufügen** auswählen.

1. Wählen Sie auf der **Canvas** die Aktivität **Write-Output** aus. Durch diese Aktion wird die Seite **Konfigurationssteuerung** aufgefüllt, auf der Sie die Aktivität konfigurieren können.

1. In der **Konfigurationssteuerung** wird das Feld **Bezeichnung** standardmäßig auf den Namen des Cmdlets festgelegt. Es kann aber in einen aussagekräftigeren Wert geändert werden. Ändern Sie diesen Wert in `Write Hello World to output`.

1. Wählen Sie **Parameter** aus, um Parameterwerte für das Cmdlet anzugeben.

   Einige Cmdlets verfügen über mehrere Parametersätze, und Sie müssen auswählen, welche verwendet werden. `Write-Output` besitzt in diesem Fall lediglich einen einzelnen Parametersatz.

1. Wählen Sie auf der Seite **Konfiguration der Aktivitätsparameter** den Parameter `INPUTOBJECT` aus, um die Seite **Parameterwert** zu öffnen. Dieser Parameter dient zum Angeben des Texts, der an den Ausgabedatenstrom gesendet werden soll.

1. Das Dropdownmenü **Datenquelle** enthält Quellen, die Sie zum Auffüllen eines Parameterwerts verwenden können. Wählen Sie in diesem Menü **PowerShell-Ausdruck** aus.

   Sie können Ausgaben von solchen Quellen (etwa eine andere Aktivität, ein Automation-Objekt oder ein PowerShell-Ausdruck) verwenden. In diesem Fall soll lediglich „`Hello World`“ ausgegeben werden. Hierzu können Sie einen PowerShell-Ausdruck verwenden und eine Zeichenfolge angeben.

1. Geben Sie im Textfeld **Ausdruck** die Zeichenfolge `"Hello World"` ein, und wählen Sie dann zweimal **OK** aus, um zum grafischen Editor zurückzukehren.

1. Wählen Sie **Speichern** aus, um das Runbook zu speichern.

## <a name="test-the-runbook"></a>Testen des Runbooks

Bevor Sie das Runbook für die Verwendung in der Produktionsumgebung veröffentlichen, sollten Sie es testen, um sicherzustellen, dass es ordnungsgemäß funktioniert. Beim Testen eines Runbooks führen Sie die Entwurfsversion aus und sehen sich interaktiv die Ausgabe an.

1. Wählen Sie im grafischen Editor die Option **Testbereich** aus, um den Bereich **Test** zu öffnen.

1. Wählen Sie die Option **Starten** aus, um den Test zu starten.

   Ein [Runbookauftrag](../automation-runbook-execution.md) wird erstellt, und der zugehörige Status wird im Bereich angezeigt. Der Auftrag weist zunächst den Status `Queued` auf, der angibt, dass der Auftrag darauf wartet, dass in der Cloud ein Runbook Worker verfügbar wird. Der Status wird in `Starting` geändert, wenn ein Worker den Auftrag beansprucht. Schließlich ändert sich der Status in `Running`, wenn die Ausführung des Runbooks beginnt.

   Nach Abschluss des Runbookauftrags wird die Ausgabe im Bereich „Test“ angezeigt. In diesem Fall wird `Hello World` angezeigt.

   :::image type="content" source="../media/automation-tutorial-runbook-graphical/runbook-test-results.png" alt-text="Ausgabe „Hello World“ des Runbooks":::

1. Wählen Sie in der rechten oberen Ecke **X** aus, um den Bereich **Test** zu schließen und zum grafischen Editor zurückzukehren.

## <a name="publish-and-start-the-runbook"></a>Veröffentlichen und Starten des Runbooks

Das von Ihnen erstellte Runbook befindet sich noch im Entwurfsmodus und muss veröffentlicht werden, damit es in der Produktion ausgeführt werden kann. Beim Veröffentlichen eines Runbooks wird die vorhandene veröffentlichte Version durch die Entwurfsversion überschrieben.

1. Wählen Sie im grafischen Editor die Option **Veröffentlichen** aus, um das Runbook zu veröffentlichen, und bestätigen Sie den Vorgang mit **Ja**, wenn Sie dazu aufgefordert werden. Daraufhin wird wieder die Übersichtsseite **Runbook** geöffnet.

1. Auf der Übersichtsseite **Runbook** wird unter **Status** der Wert **Veröffentlicht** angezeigt.

1. Wählen Sie **Starten** und anschließend **Ja** aus, wenn Sie dazu aufgefordert werden, um das Runbook zu starten und die Seite **Auftrag** zu öffnen.

   Mit den Optionen im oberen Bereich können Sie das Runbook jetzt starten, den Start für einen späteren Zeitpunkt planen oder einen [Webhook](../automation-webhooks.md) erstellen, um das Runbook über einen HTTP-Aufruf starten zu können.

   :::image type="content" source="../media/automation-tutorial-runbook-graphical/published-status.png" alt-text="Übersichtsseite und Status „Veröffentlicht“":::

1. Vergewissern Sie sich auf der Seite **Auftrag**, dass im Feld **Status** der Wert **Abgeschlossen** angezeigt wird.

1. Wählen Sie **Ausgabe** aus. Daraufhin wird `Hello World` angezeigt.

1. Wählen Sie **Alle Protokolle** aus, um die Datenströme für den Runbookauftrag anzuzeigen, und wählen Sie anschließend den einzigen Eintrag aus, um die Seite **Auftragsstreamdetails** zu öffnen. Es sollte nur `Hello World` angezeigt werden.

    Im Bereich **Alle Protokolle** können auch andere Datenströme für einen Runbookauftrag angezeigt werden (beispielsweise vom Typ „Ausführlich“ und „Fehler“), falls vom Runbook entsprechende Daten geschrieben werden.

1. Schließen Sie die Seiten **Auftragsstreamdetails** und **Auftrag**, um zur Übersichtsseite **Runbook** zurückzukehren.

1. Wählen Sie unter **Ressourcen** die Option **Aufträge** aus, um alle Aufträge für das Runbook anzuzeigen. Auf der Seite „Aufträge“ werden alle von Ihrem Runbook erstellten Aufträge aufgelistet. Es sollte nur ein Auftrag aufgeführt sein, da Sie den Auftrag bislang erst einmal ausgeführt haben.

1. Wählen Sie den Auftrag aus, um die Seite **Auftrag** zu öffnen, die auch beim Starten des Runbooks angezeigt wurde.

1. Schließen Sie die Seite **Auftrag**, und wählen Sie anschließend im linken Menü die Option **Übersicht** aus.

## <a name="create-variable-assets"></a>Erstellen variabler Ressourcen

Sie haben Ihr Runbook inzwischen zwar getestet und veröffentlicht, bislang ist es aber zum Verwalten von Azure-Ressourcen noch nicht sonderlich hilfreich. Bevor Sie das Runbook für die Authentifizierung konfigurieren, müssen Sie eine Variable erstellen, die die Abonnement-ID enthält, eine Aktivität zur Authentifizierung einrichten und dann auf die Variable verweisen. Das Einfügen eines Verweises in den Abonnementkontext ermöglicht Ihnen die einfache Nutzung mehrerer Abonnements.

1. Wählen Sie unter **Übersicht** neben **Abonnement-ID** das Symbol **In die Zwischenablage kopieren** aus.

1. Schließen Sie die Seite **Runbook**, um zur Seite **Automation-Konto** zurückzukehren.

1. Wählen Sie unter **Freigegebene Ressourcen** die Option **Variablen** aus.

1. Wählen Sie **Variable hinzufügen** aus, um die Seite **Neue Variable** zu öffnen.

1. Legen Sie auf der Seite **Neue Variable** die folgenden Werte fest:

    | Feld| Wert|
    |---|---|
    |Wert|Drücken Sie STRG+V, um Ihre Abonnement-ID einzufügen.<kbd></kbd>|
    |Name |Geben Sie `AzureSubscriptionId` ein.|
    |type|Behalten Sie den Standardwert (**Zeichenfolge**) bei.|
    |Verschlüsselt|Behalten Sie den Standardwert (**Nein**) bei.|

1. Wählen Sie **Erstellen** aus, um die Variable zu erstellen und zur Seite **Variablen** zurückzukehren.

1. Wählen Sie unter **Prozessautomatisierung** die Option **Runbooks** und anschließend Ihr Runbook (**MyFirstRunbook-Graphical**) aus.

## <a name="add-authentication"></a>Hinzufügen von Authentifizierung

Nachdem Sie nun über eine Variable für Ihre Abonnement-ID verfügen, können Sie das Runbook so konfigurieren, dass die Authentifizierung mit den Anmeldeinformationen für das ausführende Konto für Ihr Abonnement erfolgt. Konfigurieren Sie die Authentifizierung, indem Sie die Verbindung für das ausführende Azure-Konto als Objekt hinzufügen. Sie müssen auch die Cmdlets [Connect-AzAccount](/powershell/module/az.accounts/Connect-AzAccount) und [Set-AzContext](/powershell/module/az.accounts/Set-AzContext) zur Canvas hinzufügen.

> [!NOTE]
> Für PowerShell-Runbooks sind `Add-AzAccount` und `Add-AzureRMAccount` Aliase für `Connect-AzAccount`. Diese Aliase sind für Ihre grafischen Runbooks nicht verfügbar. Von einem grafischen Runbook kann nur `Connect-AzAccount` verwendet werden.

1. Wählen Sie auf der Seite **Runbook** die Option **Bearbeiten** aus, um zum grafischen Editor zurückzukehren.

1. Die Aktivität `Write Hello World to output` wird nicht mehr benötigt. Wählen Sie die Aktivität aus. Daraufhin werden in der rechten oberen Ecke der Aktivität Auslassungspunkte angezeigt. Die Auslassungspunkte sind möglicherweise nicht ganz einfach zu erkennen. Wählen Sie die Auslassungspunkte und anschließend **Löschen** aus.

1. Navigieren Sie im Steuerelement **Bibliothek** zu **RESSOURCEN** > **Verbindungen** > **AzureRunAsConnection**. Wählen Sie die Auslassungspunkte und anschließend **Zur Canvas hinzufügen** aus.

1. Ändern Sie in der **Konfigurationssteuerung** den Wert unter **Bezeichnung** von `Get-AutomationConnection` in `Get Run As Connection`.

1. Geben Sie im Suchfeld des Steuerelements **Bibliothek** den Suchbegriff `Connect-AzAccount` ein.

1. Fügen Sie `Connect-AzAccount` zur Canvas hinzu, und ziehen Sie die Aktivität unter `Get Run As Connection`.

1. Zeigen Sie auf `Get Run As Connection`, bis am unteren Rand der Form ein Kreis erscheint. Wählen Sie den Kreis aus, und halten Sie ihn gedrückt, bis ein Pfeil angezeigt wird. Ziehen Sie den Pfeil zu `Connect-AzAccount`, um eine Verknüpfung zu erstellen. Das Runbook wird mit `Get Run As Connection` gestartet und führt dann `Connect-AzAccount` aus.

    ![Verknüpfung zwischen Aktivitäten erstellen](../media/automation-tutorial-runbook-graphical/runbook-link-auth-activities.png)

1. Wählen Sie auf der **Canvas** die Option `Connect-AzAccount` aus.

1. Ändern Sie in der **Konfigurationssteuerung** die **Bezeichnung** von `Connect-AzAccount` in `Login to Azure`.

1. Wählen Sie **Parameter** aus, um die Seite **Konfiguration der Aktivitätsparameter** zu öffnen.

1. Das Cmdlet `Connect-AzAccount` verfügt über mehrere Parametersätze, und Sie müssen einen auswählen, bevor Sie Parameterwerte bereitstellen können. Wählen Sie **Parametersatz** und anschließend **ServicePrincipalCertificateWithSubscriptionId** aus. Achten Sie darauf, nicht versehentlich **ServicePrincipalCertificateFileWithSubscriptionId** auszuwählen. Die Namen sind recht ähnlich.

   Die Parameter für diesen Parametersatz werden auf der Seite **Konfiguration der Aktivitätsparameter** angezeigt.

    ![Azure-Kontoparameter hinzufügen](../media/automation-tutorial-runbook-graphical/Add-AzureRmAccount-params.png)

1. Wählen Sie **CERTIFICATETHUMBPRINT** aus, um die Seite **Parameterwert** zu öffnen.
    1. Wählen Sie im Dropdownmenü **Datenquelle** die Option **Aktivitätsausgabe** aus.
    1. Wählen Sie unter **Daten auswählen** die Option **Get Run As Connection** aus.
    1. Geben Sie im Textfeld **Feldpfad** die Zeichenfolge `CertificateThumbprint` ein.
    1. Wählen Sie **OK** aus, um zur Seite **Konfiguration der Aktivitätsparameter** zurückzukehren.

1. Wählen Sie **SERVICEPRINCIPAL** aus, um die Seite **Parameterwert** zu öffnen.
    1. Wählen Sie im Dropdownmenü **Datenquelle** die Option **Konstanter Wert** aus.
    1. Wählen Sie die Option **True** aus.
    1. Wählen Sie **OK** aus, um zur Seite **Konfiguration der Aktivitätsparameter** zurückzukehren.

1. Wählen Sie **TENANT** aus, um die Seite **Parameterwert** zu öffnen.
    1. Wählen Sie im Dropdownmenü **Datenquelle** die Option **Aktivitätsausgabe** aus.
    1. Wählen Sie unter **Daten auswählen** die Option **Get Run As Connection** aus.
    1. Geben Sie im Textfeld **Feldpfad** die Zeichenfolge `TenantId` ein.
    1. Wählen Sie **OK** aus, um zur Seite **Konfiguration der Aktivitätsparameter** zurückzukehren.

1. Wählen Sie **APPLICATIONID** aus, um die Seite **Parameterwert** zu öffnen.
    1. Wählen Sie im Dropdownmenü **Datenquelle** die Option **Aktivitätsausgabe** aus.
    1. Wählen Sie unter **Daten auswählen** die Option **Get Run As Connection** aus.
    1. Geben Sie im Textfeld **Feldpfad** die Zeichenfolge `ApplicationId` ein.
    1. Wählen Sie **OK** aus, um zur Seite **Konfiguration der Aktivitätsparameter** zurückzukehren.

1. Wählen Sie **OK** aus, um zum grafischen Editor zurückzukehren.

1. Geben Sie im Suchfeld des Steuerelements **Bibliothek** den Suchbegriff `Set-AzContext` ein.

1. Fügen Sie `Set-AzContext` zur Canvas hinzu, und ziehen Sie die Aktivität unter `Login to Azure`.

1. Ändern Sie in der **Konfigurationssteuerung** die **Bezeichnung** von `Set-AzContext` in `Specify Subscription Id`.

1. Wählen Sie **Parameter** aus, um die Seite **Konfiguration der Aktivitätsparameter** zu öffnen.

1. Das Cmdlet `Set-AzContext` verfügt über mehrere Parametersätze, und Sie müssen einen auswählen, bevor Sie Parameterwerte bereitstellen können. Wählen Sie **Parametersatz** und anschließend **Abonnement** aus. Die Parameter für diesen Parametersatz werden auf der Seite **Konfiguration der Aktivitätsparameter** angezeigt.

1. Wählen Sie **SUBSCRIPTION** aus, um die Seite **Parameterwert** zu öffnen.
    1. Wählen Sie im Dropdownmenü **Datenquelle** die Option **Variablenasset** aus.
    1. Wählen Sie in der Variablenliste die Option **AzureSubscriptionId** aus.
    1. Wählen Sie **OK** aus, um zur Seite **Konfiguration der Aktivitätsparameter** zurückzukehren.

1. Wählen Sie **OK** aus, um zum grafischen Editor zurückzukehren.

1. Erstellen Sie eine Verknüpfung zwischen `Login to Azure` und `Specify Subscription Id`. Ihr Runbook sollte nun wie folgt aussehen:

    :::image type="content" source="../media/automation-tutorial-runbook-graphical/runbook-auth-config.png" alt-text="Screenshot: Runbook nach dem Ziehen des Pfeils zu „Specify Subscription ID“":::

## <a name="add-activity-to-start-a-virtual-machine"></a>Hinzufügen einer Aktivität zum Starten eines virtuellen Computers

Jetzt müssen Sie eine Aktivität vom Typ `Start-AzVM` hinzufügen, um eine VM zu starten. Sie können einen beliebigen virtuellen Computer in Ihrem Azure-Abonnement auswählen und den Namen vorerst im Cmdlet [Start-AzVM](/powershell/module/az.compute/start-azvm) hartcodieren.

1. Geben Sie im Suchfeld des Steuerelements **Bibliothek** den Suchbegriff `Start-AzVM` ein.

1. Fügen Sie `Start-AzVM` zur Canvas hinzu, und ziehen Sie die Aktivität unter `Specify Subscription Id`.

1. Wählen Sie in der **Konfigurationssteuerung** die Option **Parameter** aus, um die Seite **Konfiguration der Aktivitätsparameter** zu öffnen.

1. Wählen Sie **Parametersatz** und anschließend **ResourceGroupNameParameterSetName** aus. Die Parameter für diesen Parametersatz werden auf der Seite **Konfiguration der Aktivitätsparameter** angezeigt. Neben den Parametern **RESOURCEGROUPNAME** und **NAME** wird ein Ausrufezeichen angezeigt, um anzugeben, dass es sich um erforderliche Parameter handelt. Für beide Felder werden Zeichenfolgenwerte erwartet.

1. Wählen Sie **RESOURCEGROUPNAME** aus, um die Seite **Parameterwert** zu öffnen.
    1. Wählen Sie im Dropdownmenü **Datenquelle** die Option **PowerShell-Ausdruck** aus.
    1. Geben Sie im Textfeld **Ausdruck** den Namen Ihrer Ressourcengruppe ein, und setzen Sie ihn in doppelte Anführungszeichen.
    1. Wählen Sie **OK** aus, um zur Seite **Konfiguration der Aktivitätsparameter** zurückzukehren.

1. Wählen Sie **NAME** aus, um die Seite **Parameterwert** zu öffnen.
    1. Wählen Sie im Dropdownmenü **Datenquelle** die Option **PowerShell-Ausdruck** aus.
    1. Geben Sie im Textfeld **Ausdruck** den Namen Ihres virtuellen Computers ein, und setzen Sie ihn in doppelte Anführungszeichen.
    1. Wählen Sie **OK** aus, um zur Seite **Konfiguration der Aktivitätsparameter** zurückzukehren.

1. Wählen Sie **OK** aus, um zum grafischen Editor zurückzukehren.

1. Erstellen Sie eine Verknüpfung zwischen `Specify Subscription Id` und `Start-AzVM`. Ihr Runbook sollte nun wie folgt aussehen:

    ![Runbook-Ausgabe für „Start-AzVM“](../media/automation-tutorial-runbook-graphical/runbook-startvm.png)

1. Wählen Sie **Testbereich** aus, um das Runbook zu testen.

1. Wählen Sie **Starten** aus, um den Test zu starten.

1. Vergewissern Sie sich nach Abschluss, dass die VM gestartet wurde. Beenden Sie dann den virtuellen Computer für spätere Schritte.

1. Kehren Sie zum grafischen Editor für Ihr Runbook zurück.

## <a name="add-input-parameters"></a>Hinzufügen von Eingabeparametern

Ihr Runbook startet die VM derzeit in der Ressourcengruppe, die Sie im Cmdlet `Start-AzVM` angegeben haben. Das Runbook wird hilfreicher sein, wenn Sie beim Starten des Runbooks sowohl den Namen als auch die Ressourcengruppe angeben. Zu diesem Zweck fügen wir dem Runbook Eingabeparameter hinzu.

1. Wählen Sie auf der oberen Menüleiste des grafischen Editors die Option **Eingabe und Ausgabe** aus.

1. Wählen Sie **Eingabe hinzufügen** aus, um die Seite **Runbook-Eingabeparameter** zu öffnen.

1. Legen Sie auf der Seite **Runbook-Eingabeparameter** die folgenden Werte fest:

    | Feld| Wert|
    |---|---|
    |Name| Geben Sie `VMName` ein.|
    |type|Behalten Sie den Standardwert (**Zeichenfolge**) bei.|
    |Obligatorisch.|Ändern Sie den Wert in **Ja**.|

1. Wählen Sie **OK** aus, um zur Seite **Eingabe und Ausgabe** zurückzukehren.

1. Wählen Sie **Eingabe hinzufügen** aus, um erneut die Seite **Runbook-Eingabeparameter** zu öffnen.

1. Legen Sie auf der Seite **Runbook-Eingabeparameter** die folgenden Werte fest:

    | Feld| Wert|
    |---|---|
    |Name| Geben Sie `ResourceGroupName` ein.|
    |type|Behalten Sie den Standardwert (**Zeichenfolge**) bei.|
    |Obligatorisch.|Ändern Sie den Wert in **Ja**.|

1. Wählen Sie **OK** aus, um zur Seite **Eingabe und Ausgabe** zurückzukehren. Die Seite sieht wahrscheinlich in etwa wie folgt aus:

    ![Runbook-Eingabeparameter](../media/automation-tutorial-runbook-graphical/start-azurermvm-params-outputs.png)

1. Wählen Sie **OK** aus, um zum grafischen Editor zurückzukehren.

1. Die neuen Eingaben sind möglicherweise nicht sofort verfügbar. Wählen Sie **Speichern** aus, schließen Sie den grafischen Editor, und öffnen Sie ihn wieder. Die neuen Eingaben sollten jetzt verfügbar sein.

1. Wählen Sie die Aktivität `Start-AzVM` und anschließend **Parameter** aus, um die Seite **Konfiguration der Aktivitätsparameter** zu öffnen.

1. Ändern Sie für den zuvor konfigurierten Parameter **RESOURCEGROUPNAME** den Wert unter **Datenquelle** in **Runbookeingabe**, und wählen Sie anschließend **ResourceGroupName** aus. Klicken Sie auf **OK**.

1. Ändern Sie für den zuvor konfigurierten Parameter **NAME** den Wert unter **Datenquelle** in **Runbookeingabe**, und wählen Sie anschließend **VMName** aus. Klicken Sie auf **OK**. Die Seite sieht wahrscheinlich in etwa wie folgt aus:

    ![Start-AzVM-Parameter](../media/automation-tutorial-runbook-graphical/start-azurermvm-params-runbookinput.png)

1. Wählen Sie **OK** aus, um zum grafischen Editor zurückzukehren.

1. Wählen Sie **Speichern** und anschließend **Testbereich** aus. Nun können Sie Werte für die beiden von Ihnen erstellten Eingabevariablen angeben.

1. Schließen Sie die Seite **Test**, um zum grafischen Editor zurückzukehren.

1. Wählen Sie **Veröffentlichen** und anschließend **Ja** aus, wenn Sie dazu aufgefordert werden, um die neue Version des Runbooks zu veröffentlichen. Daraufhin wird wieder die Übersichtsseite **Runbook** geöffnet.

1. Wählen Sie **Starten** aus, um die Seite **Runbook starten** zu öffnen.

1. Wählen Sie geeignete Werte für die Parameter `VMNAME` und `RESOURCEGROUPNAME` aus: Klicken Sie anschließend auf **OK**. Daraufhin wird die Seite **Auftrag** geöffnet.

1. Überwachen Sie den Auftrag, und vergewissern Sie sich, dass der virtuelle Computer gestartet wurde, wenn unter **Status** der Wert **Vollständig** angezeigt wird. Beenden Sie dann den virtuellen Computer für spätere Schritte.

1. Kehren Sie zum grafischen Editor für Ihr Runbook zurück.

## <a name="create-a-conditional-link"></a>Erstellen einer bedingten Verknüpfung

Nun können Sie das Runbook so anpassen, dass der virtuelle Computer nur gestartet wird, wenn er noch nicht gestartet wurde. Fügen Sie dazu das Cmdlet [Get-AzVM](/powershell/module/Az.Compute/Get-AzVM) hinzu, das den Status der VM auf Instanzebene abruft. Anschließend können Sie ein PowerShell-Workflow-Codemodul namens `Get Status` mit einem PowerShell-Codeausschnitt hinzufügen, mit dem ermittelt wird, ob die VM ausgeführt wird oder beendet ist. Eine bedingte Verknüpfung des Moduls `Get Status` führt `Start-AzVM` nur dann aus, wenn der aktuelle Ausführungszustand angibt, dass die VM beendet ist. Am Ende dieses Vorgangs gibt Ihr Runbook mit dem Cmdlet `Write-Output` eine Meldung aus, die Sie darüber informiert, ob die VM erfolgreich gestartet wurde.

1. Klicken Sie im grafischen Editor mit der rechten Maustaste auf den Link zwischen `Specify Subscription Id` und `Start-AzVM`, und wählen Sie **Löschen** aus.

1. Geben Sie im Suchfeld des Steuerelements **Bibliothek** den Suchbegriff `Get-AzVM` ein.

1. Fügen Sie `Get-AzVM` zur Canvas hinzu, und ziehen Sie die Aktivität unter `Specify Subscription Id`.

1. Wählen Sie in der **Konfigurationssteuerung** die Option **Parameter** aus, um die Seite **Konfiguration der Aktivitätsparameter** zu öffnen.

   Wählen Sie **Parametersatz** und anschließend **GetVirtualMachineInResourceGroupParamSet** aus. Die Parameter für diesen Parametersatz werden auf der Seite **Konfiguration der Aktivitätsparameter** angezeigt. Neben den Parametern **RESOURCEGROUPNAME** und **NAME** wird ein Ausrufezeichen angezeigt, um anzugeben, dass es sich um erforderliche Parameter handelt. Für beide Felder werden Zeichenfolgenwerte erwartet.

1. Wählen Sie **RESOURCEGROUPNAME** aus, um die Seite **Parameterwert** zu öffnen.
    1. Wählen Sie im Dropdownmenü **Datenquelle** die Option **Runbookeingabe** aus.
    1. Wählen Sie den Parameter **ResourceGroupName** aus.
    1. Wählen Sie **OK** aus, um zur Seite **Konfiguration der Aktivitätsparameter** zurückzukehren.

1. Wählen Sie **NAME** aus, um die Seite **Parameterwert** zu öffnen.
    1. Wählen Sie im Dropdownmenü **Datenquelle** die Option **Runbookeingabe** aus.
    1. Wählen Sie den Parameter **VMName** aus.
    1. Wählen Sie **OK** aus, um zur Seite **Konfiguration der Aktivitätsparameter** zurückzukehren.

1. Wählen Sie **STATUS** aus, um die Seite **Parameterwert** zu öffnen.
    1. Wählen Sie im Dropdownmenü **Datenquelle** die Option **Konstanter Wert** aus.
    1. Wählen Sie die Option **True** aus.
    1. Wählen Sie **OK** aus, um zur Seite **Konfiguration der Aktivitätsparameter** zurückzukehren.

1. Wählen Sie **OK** aus, um zum grafischen Editor zurückzukehren.

1. Erstellen Sie eine Verknüpfung zwischen `Specify Subscription Id` und `Get-AzVM`.

1. Löschen Sie das Suchfeld des Steuerelements **Bibliothek**, und navigieren Sie dann zu **RUNBOOKSTEUERUNG** > **Code**. Wählen Sie die Auslassungspunkte und anschließend **Zur Canvas hinzufügen** aus. Ziehen Sie die Aktivität unter `Get-AzVM`.

1. Ändern Sie in der **Konfigurationssteuerung** die **Bezeichnung** von `Code` in `Get Status`.

1. Wählen Sie in der **Konfigurationssteuerung** die Option **Code** aus, um die Seite **Code-Editor** zu öffnen.

1. Fügen Sie den folgenden Codeausschnitt in das Textfeld **PowerShell-Code** ein.

    ```powershell
    $Statuses = $ActivityOutput['Get-AzVM'].Statuses
    $StatusOut = ""
    foreach ($Status in $Statuses) {
      if($Status.Code -eq "Powerstate/running")
        {$StatusOut = "running"}
      elseif ($Status.Code -eq "Powerstate/deallocated")
        {$StatusOut = "stopped"}
    }
    $StatusOut
    ```

1. Wählen Sie **OK** aus, um zum grafischen Editor zurückzukehren.

1. Erstellen Sie eine Verknüpfung zwischen `Get-AzVM` und `Get Status`.

1. Erstellen Sie eine Verknüpfung zwischen `Get Status` und `Start-AzVM`. Ihr Runbook sollte nun wie folgt aussehen:

    ![Runbook mit Codemodul](../media/automation-tutorial-runbook-graphical/runbook-startvm-get-status.png)  

1. Wählen Sie die Verknüpfung zwischen `Get Status` und `Start-AzVM` aus.

1. Ändern Sie in der **Konfigurationssteuerung** die Option **Bedingung anwenden** in **Ja**. Daraufhin wird die Verknüpfung als gestrichelte Linie dargestellt. Das bedeutet, dass die Aktivität nur ausgeführt wird, wenn die Bedingung erfüllt ist.  

1. Geben Sie unter **Bedingungsausdruck** die Zeichenfolge `$ActivityOutput['Get Status'] -eq "Stopped"` ein. `Start-AzVM` wird jetzt nur noch ausgeführt, wenn die VM beendet wird.

1. Geben Sie im Suchfeld des Steuerelements **Bibliothek** den Suchbegriff `Write-Output` ein.

1. Fügen Sie `Write-Output` zur Canvas hinzu, und ziehen Sie die Aktivität unter `Start-AzVM`.

1. Wählen Sie die Auslassungspunkte der Aktivität und anschließend **Duplizieren** aus. Ziehen Sie die duplizierte Aktivität rechts neben die erste Aktivität.

1. Wählen Sie die erste Aktivität vom Typ `Write-Output` aus.
    1. Ändern Sie in der **Konfigurationssteuerung** die **Bezeichnung** von `Write-Output` in `Notify VM Started`.
    1. Wählen Sie **Parameter** aus, um die Seite **Konfiguration der Aktivitätsparameter** zu öffnen.
    1. Wählen Sie **INPUTOBJECT** aus, um die Seite **Parameterwert** zu öffnen.
    1. Wählen Sie im Dropdownmenü **Datenquelle** die Option **PowerShell-Ausdruck** aus.
    1. Geben Sie im Textfeld **Ausdruck** die Zeichenfolge `"$VMName successfully started."` ein.
    1. Wählen Sie **OK** aus, um zur Seite **Konfiguration der Aktivitätsparameter** zurückzukehren.
    1. Wählen Sie **OK** aus, um zum grafischen Editor zurückzukehren.

1. Wählen Sie die erste Aktivität vom Typ `Write-Output1` aus.
    1. Ändern Sie in der **Konfigurationssteuerung** die **Bezeichnung** von `Write-Output1` in `Notify VM Start Failed`.
    1. Wählen Sie **Parameter** aus, um die Seite **Konfiguration der Aktivitätsparameter** zu öffnen.
    1. Wählen Sie **INPUTOBJECT** aus, um die Seite **Parameterwert** zu öffnen.
    1. Wählen Sie im Dropdownmenü **Datenquelle** die Option **PowerShell-Ausdruck** aus.
    1. Geben Sie im Textfeld **Ausdruck** die Zeichenfolge `"$VMName could not start."` ein.
    1. Wählen Sie **OK** aus, um zur Seite **Konfiguration der Aktivitätsparameter** zurückzukehren.
    1. Wählen Sie **OK** aus, um zum grafischen Editor zurückzukehren.

1. Erstellen Sie eine Verknüpfung zwischen `Start-AzVM` und `Notify VM Started`.

1. Erstellen Sie eine Verknüpfung zwischen `Start-AzVM` und `Notify VM Start Failed`.

1. Klicken Sie auf die Verknüpfung mit `Notify VM Started`, und ändern Sie **Bedingung anwenden** in **Ja**.

1. Geben Sie für den **Bedingungsausdruck** den Wert `$ActivityOutput['Start-AzVM'].IsSuccessStatusCode -eq $true` ein. Dieses `Write-Output`-Steuerelement wird jetzt nur ausgeführt, wenn die VM erfolgreich gestartet wurde.

1. Wählen Sie die Verknüpfung mit `Notify VM Start Failed` aus.

1. Wählen Sie auf der Seite **Steuerung** für **Bedingung anwenden** die Option **Ja** aus.

1. Geben Sie im Textfeld **Bedingungsausdruck** die Zeichenfolge `$ActivityOutput['Start-AzVM'].IsSuccessStatusCode -ne $true` ein. Dieses Steuerelement vom Typ `Write-Output` wird jetzt nur ausgeführt, wenn der virtuelle Computer nicht erfolgreich gestartet wurde. Ihr Runbook sollte nun wie in der folgenden Abbildung aussehen:

    ![Runbook mit Write-Output](../media/automation-tutorial-runbook-graphical/runbook-startazurermvm-complete.png)

1. Speichern Sie das Runbook, und öffnen Sie den Testbereich.

1. Wenn Sie nun das Runbook starten, während der virtuellen Computer beendet ist, sollte der Computer gestartet werden.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur grafischen Erstellung finden Sie unter [Erstellen eines grafischen Runbooks in Azure Automation](../automation-graphical-authoring-intro.md).
* Informationen zu den ersten Schritten mit PowerShell-Runbooks finden Sie unter [Erstellen eines PowerShell-Runbooks](automation-tutorial-runbook-textual-powershell.md).
* Informationen zu den ersten Schritten mit PowerShell-Workflow-Runbooks finden Sie unter [Erstellen eines PowerShell-Workflow-Runbooks](automation-tutorial-runbook-textual.md).
* Eine Referenz zu den PowerShell-Cmdlets finden Sie unter [Az.Automation](/powershell/module/az.automation).
