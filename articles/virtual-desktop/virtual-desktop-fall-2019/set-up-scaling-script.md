---
title: Skalieren von Sitzungshosts mit Azure Automation – Azure
description: Erfahren Sie, wie Sie mit Azure Automation eine automatische Skalierung von Windows Virtual Desktop-Sitzungshosts durchführen.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: f94852a99f0bc430ac193b9951de607cdd7fa933
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85362542"
---
# <a name="scale-session-hosts-using-azure-automation"></a>Skalieren von Sitzungshosts mit Azure Automation

>[!IMPORTANT]
>Dieser Inhalt gilt für das Release vom Herbst 2019, das keine Windows Virtual Desktop-Objekte in Azure Resource Manager unterstützt.

Sie können die Gesamtkosten für die Bereitstellung von Windows Virtual Desktop verringern, indem Sie Ihre virtuellen Computer (VMs) skalieren. Das bedeutet, dass Sie die Sitzungshost-VMs außerhalb der Spitzenzeiten herunterfahren und ihre Zuordnung aufheben und sie dann während der Spitzenzeiten wieder einschalten und erneut zuordnen.

In diesem Artikel erfahren Sie mehr über das mit Azure Automation und Azure Logic Apps erstellte Skalierungstool, mit dem die virtuellen Computer der Sitzungshosts automatisch in Ihrer Windows Virtual Desktop-Umgebung skaliert werden. Um zu erfahren, wie Sie das Skalierungstool verwenden, fahren Sie mit dem Abschnitt [Voraussetzungen](#prerequisites) fort.

## <a name="report-issues"></a>Melden von Problemen

Problemberichte für das Skalierungstool werden derzeit auf GitHub anstatt auf der Microsoft-Support-Seite verarbeitet. Wenn bei Ihnen Probleme mit dem Skalierungstool auftreten, können Sie sie melden, indem Sie auf der [RDS-GitHub-Seite](https://github.com/Azure/RDS-Templates/issues?q=is%3Aissue+is%3Aopen+label%3A4a-WVD-scaling-logicapps) ein GitHub-Issue mit der Bezeichnung „4a-WVD-scaling-logicapps“ eröffnen.

## <a name="how-the-scaling-tool-works"></a>Funktionsweise des Skalierungstools

Das Skalierungstool bietet eine kostengünstige Automatisierungsoption für Kunden, die ihre Kosten für die Sitzungshost-VMs optimieren möchten.

Sie können das Skalierungstool für Folgendes verwenden:

- Planen des Startens und Beendens von VMs basierend auf Spitzenzeiten und ruhigeren Geschäftszeiten
- Aufskalieren von VMs basierend auf der Anzahl von Sitzungen pro CPU-Kern
- Abskalieren von VMs außerhalb von Spitzenzeiten unter Beibehaltung einer Mindestanzahl von Sitzungshost-VMs

Das Skalierungstool nutzt eine Kombination aus Azure Automation-PowerShell-Runbooks, Webhooks und Azure Logic Apps. Wenn das Tool ausgeführt wird, ruft Azure Logic Apps einen Webhook auf, um das Azure Automation-Runbook zu starten. Das Runbook erstellt dann einen Auftrag.

Während der Spitzenauslastung überprüft der Auftrag die aktuelle Anzahl von Sitzungen und die VM-Kapazität der derzeit ausgeführten Sitzungshosts für jeden Hostpool. Es berechnet anhand dieser Informationen, ob die ausgeführten Sitzungshost-VMs die vorhandenen Sitzungen ausreichend unterstützen können. Die Berechnung basiert auf dem Parameter *SessionThresholdPerCPU*, der in der Datei **createazurelogicapp.ps1** definiert ist. Wenn die Sitzungshost-VMs die aktiven Sitzungen nicht unterstützen können, startet der Auftrag zusätzliche Sitzungshost-VMs im Hostpool.

>[!NOTE]
>*SessionThresholdPerCPU* schränkt die Anzahl der Sitzungen auf dem virtuellen Computer nicht ein. Dieser Parameter legt nur fest, wann neue VMs gestartet werden müssen, um einen Lastenausgleich für die Verbindungen auszuführen. Wenn Sie die Anzahl der Sitzungen einschränken möchten, müssen Sie die Anweisungen zu [Set-RdsHostPool](/powershell/module/windowsvirtualdesktop/set-rdshostpool/) befolgen, um den Parameter *MaxSessionLimit* entsprechend zu konfigurieren.

Außerhalb der Spitzenauslastungszeiten ermittelt der Auftrag, welche Sitzungshost-VMs heruntergefahren werden sollten. Dies wird basierend auf dem Parameter *MinimumNumberOfRDSH* durchgeführt. Mit dem Auftrag werden die Sitzungshost-VMs auf den Ausgleichsmodus festgelegt, um zu verhindern, dass neue Sitzungen eine Verbindung mit den Hosts herstellen. Wenn Sie den Parameter *LimitSecondsToForceLogOffUser* auf einen positiven Wert (nicht null) festlegen, fordert der Auftrag alle derzeit angemeldeten Benutzer über eine Benachrichtigung auf, ihre Änderungen zu speichern. Anschließend wird so lange gewartet, wie dies in der Konfiguration angegeben ist, und anschließend wird für die Benutzer das Abmelden erzwungen. Nachdem alle Benutzersitzungen einer Sitzungshost-VM abgemeldet wurden, wird die VM über den Auftrag heruntergefahren.

Wenn Sie den Parameter *LimitSecondsToForceLogOffUser* auf null festlegen, wird die Abmeldung von Benutzersitzungen durch den Auftrag gemäß der Einstellung für die Sitzungskonfiguration in den angegebenen Gruppenrichtlinien verwaltet. Um diese Gruppenrichtlinien anzuzeigen, wechseln Sie zu **Computerkonfiguration** > **Richtlinien** > **Administrative Vorlagen** > **Windows-Komponenten** > **Terminaldienste** > **Terminalserver** > **Sitzungszeitlimits**. Wenn aktive Sitzungen auf einer Sitzungshost-VM ausgeführt werden, wird die Sitzungshost-VM vom Auftrag weiterhin ausgeführt. Falls keine aktiven Sitzungen vorhanden sind, wird die Sitzungshost-VM über den Auftrag heruntergefahren.

Der Auftrag wird in regelmäßigen Abständen basierend auf einem festgelegten Wiederholungsintervall ausgeführt. Sie können dieses Intervall basierend auf der Größe Ihrer Windows Virtual Desktop-Umgebung ändern. Beachten Sie jedoch, dass das Starten und Herunterfahren virtueller Computer einige Zeit in Anspruch nehmen kann, und planen Sie eine entsprechende Verzögerung ein. Es wird empfohlen, das Wiederholungsintervall auf 15 Minuten festzulegen.

Für das Tool gelten allerdings folgende Einschränkungen:

- Diese Lösung gilt nur für in einem Pool zusammengefasste Sitzungshost-VMs.
- Mit dieser Lösung können VMs in allen Regionen verwaltet werden. Allerdings lässt sie sich nur unter demselben Abonnement wie Ihr Azure Automation-Konto und Azure Logic Apps verwenden.

>[!NOTE]
>Das Skalierungstool steuert den Lastenausgleichsmodus des Hostpools, den es skaliert. Dabei wird die Lastenausgleichsmethode „Breiter Ansatz“ sowohl für Spitzenzeiten als auch außerhalb der Spitzenzeiten festgelegt.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit der Einrichtung des Skalierungstools beginnen, sollten Sie Folgendes vorbereiten:

- Einen [Windows Virtual Desktop-Mandanten und -Hostpool](create-host-pools-arm-template.md)
- Sitzungshostpool-VMs, die konfiguriert und für den Windows Virtual Desktop-Dienst registriert sind
- Einen Benutzer mit dem Zugriff [Mitwirkender](../../role-based-access-control/role-assignments-portal.md) für das Azure-Abonnement

Der Computer, den Sie zum Bereitstellen des Tools verwenden, muss Folgendes aufweisen:

- Windows PowerShell 5.1 oder höher
- Das Microsoft PowerShell-Modul „Az“

Wenn alles bereit ist, können Sie anfangen.

## <a name="create-an-azure-automation-account"></a>Erstellen eines Azure Automation-Kontos

Zunächst benötigen Sie ein Azure Automation-Konto zum Ausführen des PowerShell-Runbooks. So richten Sie Ihr Konto ein

1. Öffnen Sie Windows PowerShell als Administrator.
2. Führen Sie das folgende Cmdlet aus, um sich bei Ihrem Azure-Konto anzumelden.

     ```powershell
     Login-AzAccount
     ```

     >[!NOTE]
     >Ihr Konto muss über die Berechtigung „Mitwirkender“ für das Azure-Abonnement verfügen, unter dem Sie das Skalierungstool bereitstellen möchten.

3. Führen Sie das folgende Cmdlet aus, um das Skript zum Erstellen des Azure Automation-Kontos herunterzuladen:

     ```powershell
     Set-Location -Path "c:\temp"
     $uri = "https://raw.githubusercontent.com/Azure/RDS-Templates/master/wvd-templates/wvd-scaling-script/createazureautomationaccount.ps1"
     Invoke-WebRequest -Uri $uri -OutFile ".\createazureautomationaccount.ps1"
     ```

4. Führen Sie das folgende Cmdlet aus, um das Skript auszuführen und das Azure Automation-Konto zu erstellen:

     ```powershell
     .\createazureautomationaccount.ps1 -SubscriptionID <azuresubscriptionid> -ResourceGroupName <resourcegroupname> -AutomationAccountName <name of automation account> -Location "Azure region for deployment"
     ```

5. Die Ausgabe des Cmdlets enthält einen Webhook-URI. Notieren Sie sich den URI unbedingt, da Sie ihn als Parameter verwenden, wenn Sie den Ausführungszeitplan für Azure Logic Apps einrichten.

6. Nachdem Sie Ihr Azure Automation-Konto eingerichtet haben, melden Sie sich bei Ihrem Azure-Abonnement an, und überprüfen Sie, ob Ihr Azure Automation-Konto und das zugehörige Runbook in der angegebenen Ressourcengruppe angezeigt werden, wie in der folgenden Abbildung dargestellt:

> [!div class="mx-imgBorder"]
> ![Darstellung der Azure-Übersicht mit dem neu erstellten Automation-Konto und den Runbooks.](../media/automation-account.png)

  Wählen Sie den Namen Ihres Runbooks aus, um zu überprüfen, ob sich Ihr Webhook dort befindet, wo er sein sollte. Navigieren Sie als nächstes zum Abschnitt „Ressourcen“ des Runbooks, und wählen Sie **Webhooks**aus.

## <a name="create-an-azure-automation-run-as-account"></a>Erstellen eines ausführenden Azure Automation-Kontos

Nachdem Sie nun über ein Azure Automation-Konto verfügen, müssen Sie auch ein ausführendes Azure Automation-Konto erstellen, um auf Ihre Azure-Ressourcen zuzugreifen.

Ein [ausführendes Azure Automation-Konto](../../automation/manage-runas-account.md) ermöglicht die Authentifizierung für die Verwaltung von Ressourcen in Azure mit Azure-Cmdlets. Wenn Sie ein ausführendes Konto erstellen, wird in Azure Active Directory ein neuer Dienstprinzipalbenutzer erstellt, dem dann die Rolle „Mitwirkender“ auf Abonnementebene zugewiesen wird. Das ausführende Azure-Konto bietet eine gute Möglichkeit für die sichere Authentifizierung mit Zertifikaten und einem Dienstprinzipalnamen, ohne einen Benutzernamen und ein Kennwort in einem Objekt mit Anmeldeinformationen speichern zu müssen. Weitere Informationen zur Authentifizierung mit ausführenden Konten finden Sie unter [Einschränken der Berechtigungen für ausführendes Konto](../../automation/manage-runas-account.md#limit-run-as-account-permissions).

Alle Benutzer, die Mitglied der Rolle „Abonnement-Administratoren“ und Co-Administrator des Abonnements sind, können anhand der Anweisungen im nächsten Abschnitt ein ausführendes Konto erstellen.

So erstellen Sie ein ausführendes Konto in Ihrem Azure-Konto

1. Wählen Sie im Azure-Portal **Alle Dienste** aus. Geben Sie in der Liste der Ressourcen **Automation-Konten** ein, und wählen Sie diese Option aus.

2. Wählen Sie auf der Seite **Automation-Konten** den Namen Ihres Automation-Kontos aus.

3. Wählen Sie im Bereich auf der linken Seite des Fensters im Abschnitt „Kontoeinstellungen“ die Option **Ausführendes Konto** aus.

4. Wählen Sie **Ausführendes Azure-Konto** aus. Wenn der Bereich **Ausführendes Azure-Konto hinzufügen** angezeigt wird, überprüfen Sie die Übersichtsinformationen, und wählen Sie dann **Erstellen** aus, um den Kontoerstellungsprozess zu starten.

5. Warten Sie einige Minuten, bis Azure das ausführende Konto erstellt hat. Sie können den Erstellungsprozess im Menü unter den Benachrichtigungen nachverfolgen.

6. Zum Abschluss erstellt der Prozess die Ressource „AzureRunAsConnection“ im angegebenen Automation-Konto. Die Verbindungsressource enthält die Anwendungs-ID, die Mandanten-ID, die Abonnement-ID und den Zertifikatfingerabdruck. Kopieren Sie die Anwendungs-ID, da Sie diese später verwenden.

### <a name="create-a-role-assignment-in-windows-virtual-desktop"></a>Erstellen einer Rollenzuweisung in Windows Virtual Desktop

Als Nächstes müssen Sie eine Rollenzuweisung erstellen, damit AzureRunAsConnection mit Windows Virtual Desktop interagieren kann. Achten Sie darauf, dass Sie PowerShell verwenden und sich mit einem Konto anmelden, das über Berechtigungen zum Erstellen von Rollenzuweisungen verfügt.

Zunächst müssen Sie das [Windows Virtual Desktop-PowerShell-Modul](/powershell/windows-virtual-desktop/overview/) herunterladen und importieren, um es in Ihrer PowerShell-Sitzung verwenden zu können. Führen Sie die folgenden PowerShell-Cmdlets aus, um eine Verbindung mit Windows Virtual Desktop herzustellen und Ihre Mandanten anzuzeigen:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"

Get-RdsTenant
```

Wenn Sie den Mandanten mit den Hostpools, die Sie skalieren möchten, gefunden haben, befolgen Sie die Anweisungen unter [Erstellen eines Azure Automation-Kontos](#create-an-azure-automation-account), und verwenden Sie den Mandantennamen aus dem vorherigen Cmdlet, um mit dem folgenden Cmdlet die Rollenzuweisung zu erstellen:

```powershell
New-RdsRoleAssignment -RoleDefinitionName "RDS Contributor" -ApplicationId <applicationid> -TenantName <tenantname>
```

## <a name="create-the-azure-logic-app-and-execution-schedule"></a>Erstellen der Azure-Logik-App und des Ausführungszeitplans

Abschließend müssen Sie die Azure-Logik-App erstellen und einen Ausführungszeitplan für das neue Skalierungstool einrichten.

1.  Öffnen Sie Windows PowerShell als Administrator.

2.  Führen Sie das folgende Cmdlet aus, um sich bei Ihrem Azure-Konto anzumelden.

     ```powershell
     Login-AzAccount
     ```

3. Führen Sie das folgende Cmdlet aus, um die Skriptdatei „createazurelogicapp.ps1“ auf den lokalen Computer herunterzuladen.

     ```powershell
     Set-Location -Path "c:\temp"
     $uri = "https://raw.githubusercontent.com/Azure/RDS-Templates/master/wvd-templates/wvd-scaling-script/createazurelogicapp.ps1"
     Invoke-WebRequest -Uri $uri -OutFile ".\createazurelogicapp.ps1"
     ```

4. Führen Sie das folgende Cmdlet aus, um sich bei Windows Virtual Desktop mit einem Konto anzumelden, das über die Berechtigungen „RDS-Besitzer“ oder „RDS-Mitwirkender“ verfügt.

     ```powershell
     Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
     ```

5. Führen Sie das folgende PowerShell-Skript aus, um die Azure-Logik-App und den Ausführungszeitplan zu erstellen.

     ```powershell
     $aadTenantId = (Get-AzContext).Tenant.Id

     $azureSubscription = Get-AzSubscription | Out-GridView -PassThru -Title "Select your Azure Subscription"
     Select-AzSubscription -Subscription $azureSubscription.Id
     $subscriptionId = $azureSubscription.Id

     $resourceGroup = Get-AzResourceGroup | Out-GridView -PassThru -Title "Select the resource group for the new Azure Logic App"
     $resourceGroupName = $resourceGroup.ResourceGroupName
     $location = $resourceGroup.Location

     $wvdTenant = Get-RdsTenant | Out-GridView -PassThru -Title "Select your WVD tenant"
     $tenantName = $wvdTenant.TenantName

     $wvdHostpool = Get-RdsHostPool -TenantName $wvdTenant.TenantName | Out-GridView -PassThru -Title "Select the host pool you'd like to scale"
     $hostPoolName = $wvdHostpool.HostPoolName

     $recurrenceInterval = Read-Host -Prompt "Enter how often you'd like the job to run in minutes, e.g. '15'"
     $beginPeakTime = Read-Host -Prompt "Enter the start time for peak hours in local time, e.g. 9:00"
     $endPeakTime = Read-Host -Prompt "Enter the end time for peak hours in local time, e.g. 18:00"
     $timeDifference = Read-Host -Prompt "Enter the time difference between local time and UTC in hours, e.g. +5:30"
     $sessionThresholdPerCPU = Read-Host -Prompt "Enter the maximum number of sessions per CPU that will be used as a threshold to determine when new session host VMs need to be started during peak hours"
     $minimumNumberOfRdsh = Read-Host -Prompt "Enter the minimum number of session host VMs to keep running during off-peak hours"
     $limitSecondsToForceLogOffUser = Read-Host -Prompt "Enter the number of seconds to wait before automatically signing out users. If set to 0, users will be signed out immediately"
     $logOffMessageTitle = Read-Host -Prompt "Enter the title of the message sent to the user before they are forced to sign out"
     $logOffMessageBody = Read-Host -Prompt "Enter the body of the message sent to the user before they are forced to sign out"

     $automationAccount = Get-AzAutomationAccount -ResourceGroupName $resourceGroup.ResourceGroupName | Out-GridView -PassThru
     $automationAccountName = $automationAccount.AutomationAccountName
     $automationAccountConnection = Get-AzAutomationConnection -ResourceGroupName $resourceGroup.ResourceGroupName -AutomationAccountName $automationAccount.AutomationAccountName | Out-GridView -PassThru -Title "Select the Azure RunAs connection asset"
     $connectionAssetName = $automationAccountConnection.Name

     $webHookURI = Read-Host -Prompt "Enter the URI of the WebHook returned by when you created the Azure Automation Account"
     $maintenanceTagName = Read-Host -Prompt "Enter the name of the Tag associated with VMs you don't want to be managed by this scaling tool"

     .\createazurelogicapp.ps1 -ResourceGroupName $resourceGroupName `
       -AADTenantID $aadTenantId `
       -SubscriptionID $subscriptionId `
       -TenantName $tenantName `
       -HostPoolName $hostPoolName `
       -RecurrenceInterval $recurrenceInterval `
       -BeginPeakTime $beginPeakTime `
       -EndPeakTime $endPeakTime `
       -TimeDifference $timeDifference `
       -SessionThresholdPerCPU $sessionThresholdPerCPU `
       -MinimumNumberOfRDSH $minimumNumberOfRdsh `
       -LimitSecondsToForceLogOffUser $limitSecondsToForceLogOffUser `
       -LogOffMessageTitle $logOffMessageTitle `
       -LogOffMessageBody $logOffMessageBody `
       -Location $location `
       -ConnectionAssetName $connectionAssetName `
       -WebHookURI $webHookURI `
       -AutomationAccountName $automationAccountName `
       -MaintenanceTagName $maintenanceTagName
     ```

     Nachdem Sie das Skript ausgeführt haben, sollte die Logik-App in einer Ressourcengruppe angezeigt werden, wie in der folgenden Abbildung dargestellt.

     > [!div class="mx-imgBorder"]
     > ![Darstellung der Übersicht für eine Beispiel-Logik-App in Azure.](../media/logic-app.png)

Wenn Sie Änderungen am Ausführungszeitplan vornehmen möchten, um z. B. das Wiederholungsintervall oder die Zeitzone zu ändern, navigieren Sie zum Zeitplan für die Autoskalierung, und wählen Sie **Bearbeiten** aus, um zum Logik-App-Designer zu wechseln.

> [!div class="mx-imgBorder"]
> ![Darstellung des Logik-App-Designers. Die Menüs „Wiederholung“ und „Webhook“, mit denen Benutzer die Wiederholungszeiten und die Webhook-Datei bearbeiten können, sind geöffnet.](../media/logic-apps-designer.png)

## <a name="manage-your-scaling-tool"></a>Verwalten Ihres Skalierungstools

Nachdem Sie das Skalierungstool erstellt haben, können Sie auf seine Ausgabe zugreifen. In diesem Abschnitt werden einige Features beschrieben, die möglicherweise hilfreich sind.

### <a name="view-job-status"></a>Anzeigen des Auftragsstatus

Sie können im Azure-Portal einen zusammengefassten Status aller Runbookaufträge anzeigen oder ausführlichere Details zum Status eines bestimmten Runbookauftrags einsehen.

Rechts in Ihrem ausgewählten Automation-Konto sehen Sie unter „Auftragsstatistik“ die Zusammenfassung aller Runbookaufträge. Wenn Sie auf der linken Seite des Fensters die Seite **Aufträge** öffnen, werden die aktuellen Auftragsstatus, Startzeiten und Beendigungszeiten angezeigt.

> [!div class="mx-imgBorder"]
> ![Screenshot der Auftragsstatusseite.](../media/jobs-status.png)

### <a name="view-logs-and-scaling-tool-output"></a>Anzeigen von Protokollen und der Ausgabe des Skalierungstools

Sie können die Protokolle der Vorgänge für das horizontale Hoch- und Herunterskalieren anzeigen, indem Sie das Runbook öffnen und den Namen Ihres Auftrags auswählen.

Navigieren Sie in der Ressourcengruppe, in der das Azure Automation-Konto gehostet wird, zum Runbook (der Standardname ist „WVDAutoScaleRunbook“), und wählen Sie **Übersicht** aus. Wählen Sie auf der Seite „Übersicht“ unter „Kürzlich ausgeführte Aufträge“ einen Auftrag aus, um die zugehörige Ausgabe des Skalierungstools anzuzeigen, wie in der folgenden Abbildung dargestellt.

> [!div class="mx-imgBorder"]
> ![Darstellung des Ausgabefensters für das Skalierungstool.](../media/tool-output.png)

