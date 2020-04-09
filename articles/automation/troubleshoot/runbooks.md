---
title: Beheben von Fehlern bei Azure Automation-Runbooks
description: Erfahren Sie, wie Sie Probleme beheben, die möglicherweise bei Azure Automation-Runbooks auftreten.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 01/24/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 26c5c5b31d5f3f9e1a642c0bafb947190e479055
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632625"
---
# <a name="troubleshoot-errors-with-runbooks"></a>Beheben von Fehlern bei Runbooks

Wenn beim Ausführen von Runbooks in Azure Automation Fehler auftreten, können Sie die folgenden Schritte ausführen, um die Probleme zu diagnostizieren.

1. **Stellen Sie sicher, dass Ihr Runbook-Skript auf Ihrem lokalen Computer erfolgreich ausgeführt wird.** 

    Sprachreferenzinformationen und Lernmodule finden Sie in der [PowerShell-Dokumentation](/powershell/scripting/overview) oder der [Python-Dokumentation](https://docs.python.org/3/). Mit der lokalen Ausführung Ihres Skripts können Sie häufige Fehler erkennen und beheben, z. B.:

      * Fehlende Module
      * Syntaxfehler
      * Logikfehler

2. **Untersuchen Sie Runbook-[Fehlerdatenströme](https://docs.microsoft.com/azure/automation/automation-runbook-output-and-messages#runbook-output).**

    Untersuchen Sie diese Datenströme auf bestimmte Meldungen, und vergleichen Sie sie mit den in diesem Artikel erläuterten Fehlern.

3. **Stellen Sie sicher, dass Ihre Knoten und der Automation-Arbeitsbereich über die erforderlichen Module verfügen.** 

    Wenn Ihr Runbook Module importiert, überprüfen Sie, ob diese in Ihrem Automation-Konto vorhanden sind, indem Sie die unter [Importieren von Modulen](../shared-resources/modules.md#importing-modules) aufgeführten Schritte ausführen. Aktualisieren Sie Ihre Module auf die neueste Version, indem Sie die Anleitungen unter [Aktualisieren von Azure-Modulen in Azure Automation](..//automation-update-azure-modules.md) befolgen. Weitere Problembehandlungsinformationen finden Sie unter [Module](shared-resources.md#modules).

4. **Wenn das Runbook angehalten wurde oder unerwartet fehlgeschlagen ist:**

    * Unter [Auftragsstatuswerte](https://docs.microsoft.com/azure/automation/automation-runbook-execution#job-statuses) werden Statuswerte für Runbooks sowie einige mögliche Ursachen beschrieben.
    * [Fügen Sie dem Runbook eine zusätzliche Ausgabe hinzu](https://docs.microsoft.com/azure/automation/automation-runbook-output-and-messages#message-streams), um zu ermitteln, was passiert, bevor das Runbook angehalten wird.
    * [Behandeln Sie alle Ausnahmen](https://docs.microsoft.com/azure/automation/automation-runbook-execution#handling-exceptions), die von Ihrem Auftrag ausgelöst werden.

## <a name="scenario-run-login-azurermaccount-to-log-in"></a><a name="login-azurerm"></a>Szenario: Ausführen von Login-AzureRMAccount zum Anmelden

### <a name="issue"></a>Problem

Beim Ausführen eines Runbooks wird die folgende Fehlermeldung angezeigt:

```error
Run Login-AzureRMAccount to login.
```

### <a name="cause"></a>Ursache

Dieser Fehler kann auftreten, wenn Sie kein ausführendes Konto verwenden oder wenn das ausführende Konto abgelaufen ist. Weitere Informationen finden Sie unter [Verwalten von ausführenden Azure Automation-Konten](https://docs.microsoft.com/azure/automation/manage-runas-account).

Dieser Fehler hat zwei Hauptursachen:

* Es sind verschiedene Versionen des AzureRM- oder Az-Moduls vorhanden.
* Sie versuchen, auf Ressourcen in einem separaten Abonnement zuzugreifen.

### <a name="resolution"></a>Lösung

Wenn diese Fehlermeldung nach dem Aktualisieren eines AzureRM- oder Az-Moduls angezeigt wird, sollten Sie alle Ihre Module auf die gleiche Version aktualisieren.

Wenn Sie versuchen, auf Ressourcen in einem anderen Abonnement zuzugreifen, können Sie die folgenden Schritte ausführen, um die Berechtigungen zu konfigurieren.

1. Wechseln Sie zum ausführenden Automation-Konto, und kopieren Sie die Anwendungs-ID und den Fingerabdruck.
  ![Kopieren der Anwendungs-ID und des Fingerabdrucks](../media/troubleshoot-runbooks/collect-app-id.png)
1. Wechseln Sie zur Zugriffssteuerung des Abonnements, in dem das Automation-Konto NICHT gehostet wird, und fügen Sie eine neue Rollenzuweisung hinzu.
  ![Zugriffssteuerung](../media/troubleshoot-runbooks/access-control.png)
1. Fügen Sie die zuvor kopierte Anwendungs-ID hinzu. Wählen Sie die Berechtigungen für „Mitwirkender“ aus.
   ![Hinzufügen der Rollenzuweisung](../media/troubleshoot-runbooks/add-role-assignment.png)
1. Kopieren Sie den Namen des Abonnements.
1. Sie können nun den folgenden Runbookcode verwenden, um die Berechtigungen Ihres Automation-Kontos für das andere Abonnement zu testen. Ersetzen Sie `"\<CertificateThumbprint\>"` durch den Wert, den Sie in Schritt 1 kopiert haben. Ersetzen Sie `"\<SubscriptionName\>"` durch den Wert, den Sie in Schritt 4 kopiert haben.

    ```powershell
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint "<CertificateThumbprint>"
    #Select the subscription you want to work with
    Select-AzureRmSubscription -SubscriptionName '<YourSubscriptionNameGoesHere>'

    #Test and get outputs of the subscriptions you granted access.
    $subscriptions = Get-AzureRmSubscription
    foreach($subscription in $subscriptions)
    {
        Set-AzureRmContext $subscription
        Write-Output $subscription.Name
    }
    ```

## <a name="scenario-unable-to-find-the-azure-subscription"></a><a name="unable-to-find-subscription"></a>Szenario: Azure-Abonnement nicht gefunden

### <a name="issue"></a>Problem

Beim Arbeiten mit dem Cmdlet `Select-AzureSubscription` oder `Select-AzureRmSubscription` wird folgende Fehlermeldung angezeigt:

```error
The subscription named <subscription name> cannot be found.
```

### <a name="error"></a>Fehler

Dieser Fehler kann in den folgenden Fällen auftreten:

* Der Abonnementname ist ungültig.
* Der Azure Active Directory-Benutzer, der die Abonnementdetails abrufen möchte, ist nicht als Administrator des Abonnements konfiguriert.

### <a name="resolution"></a>Lösung

Führen Sie die folgenden Schritte aus, um zu ermitteln, ob die Authentifizierung in Azure erfolgt ist und Sie auf das gewünschte Abonnement zugreifen können.

1. Testen Sie Ihr Skript außerhalb von Azure Automation, um sicherzustellen, dass es eigenständig verwendet werden kann.
2. Stellen Sie sicher, dass Ihr Skript das Cmdlet `Add-AzureAccount` vor dem Cmdlet `Select-AzureSubscription` ausführt.
3. Fügen Sie am Anfang Ihres Runbooks `Disable-AzureRmContextAutosave –Scope Process` hinzu. Durch das Aufrufen dieses Cmdlets wird sichergestellt, dass alle Anmeldeinformationen nur für die Ausführung des aktuellen Runbooks gelten.
4. Wenn diese Fehlermeldung weiterhin angezeigt wird, ändern Sie Ihren Code, indem Sie den Parameter `AzureRmContext` für das Cmdlet `Add-AzureAccount` hinzufügen, und führen Sie dann den Code aus.

   ```powershell
   Disable-AzureRmContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $context = Get-AzureRmContext

   Get-AzureRmVM -ResourceGroupName myResourceGroup -AzureRmContext $context
    ```

## <a name="scenario-authentication-to-azure-failed-because-multi-factor-authentication-is-enabled"></a><a name="auth-failed-mfa"></a>Szenario: Fehler beim Authentifizieren bei Azure, da die mehrstufige Authentifizierung aktiviert ist

### <a name="issue"></a>Problem

Sie erhalten bei der Authentifizierung bei Azure mit Ihrem Azure-Benutzernamen und -Kennwort die folgende Fehlermeldung:

```error
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

### <a name="cause"></a>Ursache

Falls für Ihr Azure-Konto die mehrstufige Authentifizierung eingerichtet ist, können Sie für die Authentifizierung bei Azure keinen Azure Active Directory-Benutzer verwenden. Stattdessen müssen Sie ein Zertifikat oder einen Dienstprinzipal für die Authentifizierung verwenden.

### <a name="resolution"></a>Lösung

Informationen zum Verwenden eines Zertifikats mit Cmdlets für das klassische Azure-Bereitstellungsmodell finden Sie unter [Erstellen und Hinzufügen eines Zertifikats zum Verwalten von Azure-Diensten](https://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx). Informationen zum Verwenden eines Dienstprinzipals mit Azure Resource Manager-Cmdlets finden Sie unter [Gewusst wie: Erstellen einer Azure AD-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff über das Portal](../../active-directory/develop/howto-create-service-principal-portal.md) und [Gewusst wie: Verwenden von Azure PowerShell zum Erstellen eines Dienstprinzipals mit einem Zertifikat](../../active-directory/develop/howto-authenticate-service-principal-powershell.md).

## <a name="scenario-you-see-an-error-in-your-job-streams-about-the-get_serializationsettings-method"></a><a name="get-serializationsettings"></a>Szenario: In Ihren Auftragsdatenströmen wird ein Fehler zur get_SerializationSettings-Methode angezeigt.

### <a name="issue"></a>Problem

In Ihren Auftragsdatenströmen für ein Runbook wird der folgende Fehler angezeigt:

```error
Connect-AzureRMAccount : Method 'get_SerializationSettings' in type
'Microsoft.Azure.Management.Internal.Resources.ResourceManagementClient' from assembly
'Microsoft.Azure.Commands.ResourceManager.Common, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'
does not have an implementation.
At line:16 char:1
+ Connect-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -Appl ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [Connect-AzureRmAccount], TypeLoadException
    + FullyQualifiedErrorId : System.TypeLoadException,Microsoft.Azure.Commands.Profile.ConnectAzureRmAccountCommand
```

### <a name="cause"></a>Ursache

Dieser Fehler wird durch die kombinierte Verwendung von AzureRM- und Az-Modul-Cmdlets in einem Runbook verursacht. Er tritt auf, wenn Sie das Az-Modul importieren, bevor Sie das AzureRM-Modul importiert haben.

### <a name="resolution"></a>Lösung

Az- und AzureRM-Cmdlets können nicht importiert und im selben Runbook verwendet werden. Weitere Informationen zu Az-Cmdlets in Azure Automation finden Sie unter [Az-Modulunterstützung in Azure Automation](../az-modules.md).

## <a name="scenario-the-runbook-fails-with-the-error-a-task-was-canceled"></a><a name="task-was-cancelled"></a>Szenario: Beim Runbook tritt folgender Fehler auf: Eine Aufgabe wurde abgebrochen.

### <a name="issue"></a>Problem

Für Ihr Runbook tritt ein Fehler auf, der dem im folgenden Beispiel ähnelt:

```error
Exception: A task was canceled.
```

### <a name="cause"></a>Ursache

Dieser Fehler kann durch veraltete Azure-Module verursacht werden.

### <a name="resolution"></a>Lösung

Sie können diesen Fehler beheben, indem Sie Ihre Azure-Module auf die neueste Version aktualisieren. 

1. Klicken Sie in Ihrem Automation-Konto auf **Module** und anschließend auf **Azure-Module aktualisieren**. 
2. Das Update dauert ungefähr 15 Minuten. Führen Sie das Runbook, bei dem der Fehler aufgetreten ist, erneut aus, sobald das Update abgeschlossen ist.

Weitere Informationen zum Aktualisieren Ihrer Module finden Sie unter [Aktualisieren von Azure-Modulen in Azure Automation](../automation-update-azure-modules.md).

## <a name="scenario-runbooks-fail-when-dealing-with-multiple-subscriptions"></a><a name="runbook-auth-failure"></a>Szenario: Fehler bei Runbooks, wenn mehrere Abonnements verwendet werden

### <a name="issue"></a>Problem

Beim Ausführen von Runbooks mit kann das Runbook Azure-Ressourcen nicht verwalten.

### <a name="cause"></a>Ursache

Das Runbook verwendet beim Ausführen nicht den richtigen Kontext.

### <a name="resolution"></a>Lösung

Der Abonnementkontext geht möglicherweise verloren, wenn ein Runbook mehrere Runbooks aufruft. Stellen Sie sicher, dass der Abonnementkontext an die Runbooks übergeben wird, indem Sie dafür sorgen, dass das Clientrunbook den Kontext im Parameter `AzureRmContext` an das Cmdlet `Start-AzureRmAutomationRunbook` übergibt. Verwenden Sie das Cmdlet `Disable-AzureRmContextAutosave` mit dem Wert `Process` für den Parameter `Scope`, um sicherzustellen, dass die angegebenen Anmeldeinformationen nur für das aktuelle Runbook verwendet werden. Weitere Informationen finden Sie unter [Arbeiten mit mehreren Abonnements](../automation-runbook-execution.md#working-with-multiple-subscriptions).

```azurepowershell-interactive
# Ensures that any credentials apply only to the execution of this runbook
Disable-AzureRmContextAutosave –Scope Process

# Connect to Azure with Run As account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true}

Start-AzureRmAutomationRunbook `
    –AutomationAccountName 'MyAutomationAccount' `
    –Name 'Test-ChildRunbook' `
    -ResourceGroupName 'LabRG' `
    -AzureRmContext $AzureContext `
    –Parameters $params –wait
```

## <a name="scenario-term-not-recognized-as-the-name-of-a-cmdlet-function-script"></a><a name="not-recognized-as-cmdlet"></a>Szenario: Die Benennung wird nicht als Name eines Cmdlets, einer Funktion oder eines Skripts erkannt.

### <a name="issue"></a>Problem

Für Ihr Runbook tritt ein Fehler auf, der dem im folgenden Beispiel ähnelt:

```error
The term 'Connect-AzureRmAccount' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if the path was included verify that the path is correct and try again.
```

### <a name="cause"></a>Ursache

Dieser Fehler kann aus einem der folgenden Gründe auftreten:

* Das Modul, das das Cmdlet enthält, wird nicht in das Automation-Konto importiert.
* Das Modul, das das Cmdlet enthält, wird zwar importiert, ist jedoch veraltet.

### <a name="resolution"></a>Lösung

Führen Sie eine der folgenden Aktionen aus, um diesen Fehler zu beheben. 

* Wenn es sich bei dem Modul um ein Azure-Modul handelt, finden Sie weitere Informationen zum Aktualisieren Ihrer Module in Ihrem Automation-Konto unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](../automation-update-azure-modules.md).

* Wenn es sich nicht um ein Azure-Modul handelt, stellen Sie sicher, dass das Modul in Ihr Automation-Konto importiert wurde.

## <a name="scenario-the-runbook-job-start-was-attempted-three-times-but-failed-to-start-each-time"></a><a name="job-attempted-3-times"></a>Szenario: Es wurde dreimal erfolglos versucht, den Runbookauftrag zu starten

### <a name="issue"></a>Problem

Bei Ihrem Runbook tritt ein Fehler mit der folgenden Fehlermeldung auf:

```error
The job was tried three times but it failed
```

### <a name="cause"></a>Ursache

Dieser Fehler tritt aufgrund eines der folgenden Probleme auf:

* Arbeitsspeicherlimit. Für einen Auftrag kann ein Fehler auftreten, wenn dafür mehr als 400 MB Arbeitsspeicher verwendet werden. Die dokumentierten Grenzwerte, die für das Zuordnen von Arbeitsspeicher zu einer Sandbox gelten, finden Sie unter [Grenzwerte für den Automation-Dienst](../../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits). 

* Netzwerksockets. Azure-Sandboxes sind auf 1000 gleichzeitige Netzwerksockets beschränkt. Weitere Informationen finden Sie unter [Grenzwerte für den Automation-Dienst](../../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits).

* Modul inkompatibel. Die Modulabhängigkeiten sind möglicherweise nicht korrekt. In diesem Fall gibt Ihr Runbook üblicherweise die Fehlermeldung `Command not found` oder `Cannot bind parameter` aus.

* Die Sandbox wurde nicht mit Active Directory authentifiziert. Ihr Runbook versucht, eine ausführbare Datei oder einen Teilprozess aufzurufen, die bzw. der in einer Azure-Sandbox ausgeführt wird. Das Konfigurieren von Runbooks für die Authentifizierung mit Azure AD mithilfe der Azure Active Directory-Authentifizierungsbibliothek (ADAL) wird nicht unterstützt.

* Zu viele Ausnahmedaten. Vom Runbook wurden zu viele Ausnahmedaten in den Ausgabestream geschrieben.

### <a name="resolution"></a>Lösung

* Arbeitsspeicherlimit, Netzwerksockets. Die empfohlenen Vorgehensweisen für die Einhaltung der Arbeitsspeichergrenzwerte bestehen darin, die Workload auf mehrere Runbooks zu verteilen, weniger Daten im Arbeitsspeicher zu verarbeiten, das Schreiben von unnötigen Ausgaben aus den Runbooks zu vermeiden und zu überprüfen, wie viele Prüfpunkte Sie in Ihre PowerShell-Workflowrunbooks schreiben. Verwenden Sie die clear-Methode (z.B. `$myVar.clear`), um Variablen zu löschen, und verwenden Sie dann `[GC]::Collect`, um sofort die Garbage Collection durchzuführen. Durch diese Aktionen wird der Speicherbedarf Ihres Runbooks während der Laufzeit reduziert.

* Modul inkompatibel. Aktualisieren Sie Ihre Azure-Module, indem Sie die Schritte unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](../automation-update-azure-modules.md) befolgen.

* Die Sandbox wurde nicht mit Active Directory authentifiziert. Stellen Sie beim Authentifizieren bei Azure AD mit einem Runbook sicher, dass das Azure AD-Modul in Ihrem Automation-Konto verfügbar ist. Vergewissern Sie sich, dass Sie dem ausführenden Konto die erforderlichen Berechtigungen zum Ausführen der vom Runbook automatisierten Aufgaben erteilen.

  Verwenden Sie das Runbook auf einem [Hybrid Runbook Worker](../automation-hrw-run-runbooks.md), wenn Ihr Runbook eine ausführbare Datei oder einen Teilprozess nicht aufrufen kann, die bzw. der in einer Azure Sandbox ausgeführt wird. Hybrid Worker unterliegen nicht den vom Arbeitsspeicher und Netzwerk vorgegebenen Grenzwerten, die für Azure-Sandboxes gelten.

* Zu viele Ausnahmedaten. Für den Auftragsausgabestream gilt ein Grenzwert von 1 MB. Stellen Sie sicher, dass Ihr Runbook Aufrufe einer ausführbaren Datei oder eines Teilprozesses in `try`- und `catch`-Blöcke einschließt. Wenn diese Vorgänge eine Ausnahme auslösen, lassen Sie den Code die Nachricht dieser Ausnahme in eine Automation-Variable schreiben. Mit dieser Technik wird verhindert, dass die Nachricht in den Auftragsausgabestream geschrieben wird.

## <a name="scenario-sign-in-to-azure-account-failed"></a><a name="sign-in-failed"></a>Szenario: Fehler beim Anmelden beim Azure-Konto

### <a name="issue"></a>Problem

Beim Arbeiten mit dem Cmdlet `Add-AzureAccount` oder `Connect-AzureRmAccount` wird eine der folgenden Fehlermeldungen angezeigt:

```error
Unknown_user_type: Unknown User Type
```

```error
No certificate was found in the certificate store with thumbprint
```

### <a name="cause"></a>Ursache

Diese Fehler treten auf, wenn der Name des Anmeldeinformationsobjekts ungültig ist. Sie treten möglicherweise auch auf, wenn Benutzername und Kennwort, die Sie zur Einrichtung des Automation-Anmeldeinformationsobjekts verwendet haben, nicht gültig sind.

### <a name="resolution"></a>Lösung

Führen Sie die folgenden Schritte aus, um zu ermitteln, wo der Fehler liegt:

1. Stellen Sie sicher, dass Sie keine Sonderzeichen eingegeben haben. Dazu zählt auch das `\@`-Zeichen im Namen des Automation-Anmeldeinformationsobjekts, mit dem Sie die Verbindung zu Azure herstellen.
2. Überprüfen Sie, ob Sie den Benutzernamen und das Kennwort aus den Azure Automation-Anmeldeinformationen in Ihrem lokalen PowerShell ISE-Editor verwenden können. Führen Sie die folgenden Cmdlets in der PowerShell ISE aus.

   ```powershell
   $Cred = Get-Credential
   #Using Azure Service Management
   Add-AzureAccount –Credential $Cred
   #Using Azure Resource Manager
   Connect-AzureRmAccount –Credential $Cred
   ```

3. Wenn die Authentifizierung lokal fehlschlägt, haben Sie Ihre Azure Active Directory-Anmeldeinformationen nicht richtig eingerichtet. Informationen zur richtigen Einrichtung des Azure Active Directory-Kontos finden Sie im Blogbeitrag [Azure Automation: Authentifizieren in Azure mit Azure Active Directory](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/).

4. Wenn es sich um einen vorübergehenden Fehler zu handeln scheint, versuchen Sie, eine Wiederholungslogik zu Ihrer Authentifizierungsroutine hinzuzufügen, um die Authentifizierung zuverlässiger zu gestalten.

   ```powershell
   # Get the connection "AzureRunAsConnection"
   $connectionName = "AzureRunAsConnection"
   $servicePrincipalConnection = Get-AutomationConnection -Name $connectionName

   $logonAttempt = 0
   $logonResult = $False

   while(!($connectionResult) -And ($logonAttempt -le 10))
   {
       $LogonAttempt++
       #Logging in to Azure...
       $connectionResult = Connect-AzureRmAccount `
                              -ServicePrincipal `
                              -TenantId $servicePrincipalConnection.TenantId `
                              -ApplicationId $servicePrincipalConnection.ApplicationId `
                              -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint

       Start-Sleep -Seconds 30
   }
   ```

## <a name="scenario-object-reference-not-set-to-an-instance-of-an-object"></a><a name="child-runbook-object"></a>Szenario: Objektverweis wurde nicht auf eine Objektinstanz festgelegt

### <a name="issue"></a>Problem

Die folgende Fehlermeldung wird angezeigt, wenn Sie ein untergeordnetes Runbook mit dem Parameter `Wait` aufrufen und der Ausgabedatenstrom ein Objekt enthält:

```error
Object reference not set to an instance of an object
```

### <a name="cause"></a>Ursache

`Start-AzureRmAutomationRunbook` verarbeitet den Ausgabedatenstrom nicht richtig, wenn der Datenstrom Objekte enthält.

### <a name="resolution"></a>Lösung

Es wird empfohlen, eine Abruflogik zu implementieren und zum Abrufen der Ausgabe das Cmdlet [Get-AzureRmAutomationJobOutput](/powershell/module/azurerm.automation/get-azurermautomationjoboutput) zu verwenden. Der folgende Code ist ein Beispiel für eine solche Logik.

```powershell
$automationAccountName = "ContosoAutomationAccount"
$runbookName = "ChildRunbookExample"
$resourceGroupName = "ContosoRG"

function IsJobTerminalState([string] $status) {
    return $status -eq "Completed" -or $status -eq "Failed" -or $status -eq "Stopped" -or $status -eq "Suspended"
}

$job = Start-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName -Name $runbookName -ResourceGroupName $resourceGroupName
$pollingSeconds = 5
$maxTimeout = 10800
$waitTime = 0
while((IsJobTerminalState $job.Status) -eq $false -and $waitTime -lt $maxTimeout) {
   Start-Sleep -Seconds $pollingSeconds
   $waitTime += $pollingSeconds
   $job = $job | Get-AzureRmAutomationJob
}

$jobResults | Get-AzureRmAutomationJobOutput | Get-AzureRmAutomationJobOutputRecord | Select-Object -ExpandProperty Value
```

## <a name="scenario-runbook-fails-because-of-deserialized-object"></a><a name="fails-deserialized-object"></a>Szenario: Runbookfehler aufgrund eines deserialisierten Objekts

### <a name="issue"></a>Problem

Ihr Runbook schlägt mit dem folgenden Fehler fehl:

```error
Cannot bind parameter <ParameterName>.

Cannot convert the <ParameterType> value of type Deserialized <ParameterType> to type <ParameterType>.
```

### <a name="cause"></a>Ursache

Wenn es sich bei Ihrem Runbook um einen PowerShell-Workflow handelt, werden komplexe Objekte in einem deserialisierten Format gespeichert, um den Runbookstatus beizubehalten, sobald der Workflow angehalten wird.

### <a name="resolution"></a>Lösung

Beheben Sie dieses Problem mit einer der folgenden Lösungen.

* Wenn Sie komplexe Objekte von einem Cmdlet an ein anderes übergeben, umschließen Sie diese Cmdlets mit einer `InlineScript`-Aktivität.
* Übergeben Sie den Namen oder Wert, den Sie aus dem komplexen Objekt benötigen, anstatt das gesamte Objekt zu übergeben.
* Verwenden Sie anstelle eines PowerShell-Workflow-Runbooks ein PowerShell-Runbook.

## <a name="scenario-runbook-job-fails-because-allocated-quota-exceeded"></a><a name="quota-exceeded"></a>Szenario: Fehler beim Runbookauftrag, da das zugeordnete Kontingent überschritten wurde

### <a name="issue"></a>Problem

Ihr Runbookauftrag schlägt mit dem folgenden Fehler fehl:

```error
The quota for the monthly total job run time has been reached for this subscription
```

### <a name="cause"></a>Ursache

Dieser Fehler tritt auf, wenn die Auftragsausführung das kostenlose Kontingent von 500 Minuten für Ihr Konto überschreitet. Dieses Kontingent gilt für alle Arten von Auftragsausführungsaufgaben. Zu diesen Aufgaben zählen unter anderem: Testen eines Auftrags, Starten eines Auftrags im Portal, Ausführen eines Auftrags per Webhook und Planen der Ausführung eines Auftrags über das Azure-Portal oder in Ihrem Rechenzentrum. Weitere Informationen zu den Preisen für Automation finden Sie unter [Automation – Preise](https://azure.microsoft.com/pricing/details/automation/).

### <a name="resolution"></a>Lösung

Wenn Sie mehr als 500 Minuten Verarbeitungszeit pro Monat nutzen möchten, müssen Sie Ihr Abonnement vom Free-Tarif auf den Basic-Tarif umstellen.

1. Melden Sie sich bei Ihrem Azure-Abonnement an.
2. Wählen Sie das Automation-Konto aus, das Sie aktualisieren möchten.
3. Klicken Sie zunächst auf **Einstellungen** und dann auf **Preise**.
4. Klicken Sie unten auf der Seite auf **Aktivieren**, um für Ihr Konto ein Upgrade auf den Basic-Tarif durchzuführen.

## <a name="scenario-cmdlet-not-recognized-when-executing-a-runbook"></a><a name="cmdlet-not-recognized"></a>Szenario: Cmdlet wird beim Ausführen eines Runbooks nicht erkannt

### <a name="issue"></a>Problem

Ihr Runbookauftrag schlägt mit dem folgenden Fehler fehl:

```error
<cmdlet name>: The term <cmdlet name> is not recognized as the name of a cmdlet, function, script file, or operable program.
```

### <a name="cause"></a>Ursache

Dieser Fehler wird verursacht, wenn die PowerShell-Engine das Cmdlet nicht findet, das Sie in Ihrem Runbook verwenden. Dies kann der Fall sein, weil das Modul mit dem Cmdlet in dem entsprechenden Konto nicht vorhanden ist, ein Namenskonflikt mit einem Runbooknamen besteht oder das Cmdlet auch in einem anderen Modul vorhanden ist und Automation den Namen nicht auflösen kann.

### <a name="resolution"></a>Lösung

Beheben Sie dieses Problem mit einer der folgenden Lösungen.

* Überprüfen Sie, ob Sie den Cmdlet-Namen richtig eingegeben haben.
* Stellen Sie sicher, dass das Cmdlet in Ihrem Automation-Konto vorhanden ist und dass keine Konflikte bestehen. Überprüfen Sie wie folgt, ob das Cmdlet vorhanden ist: Öffnen Sie ein Runbook im Bearbeitungsmodus, und suchen Sie in der Bibliothek nach dem gewünschten Cmdlet, oder führen Sie `Get-Command <CommandName>` aus. Wenn Sie festgestellt haben, dass das Cmdlet für das Konto verfügbar ist und keine Namenskonflikte mit anderen Cmdlets oder Runbooks bestehen, fügen Sie es der Canvas hinzu und stellen Sie sicher, dass Sie in Ihrem Runbook einen gültigen Parametersatz verwenden.
* Falls ein Namenskonflikt vorliegt und das Cmdlet in zwei unterschiedlichen Modulen verfügbar ist, beheben Sie das Problem, indem Sie den vollqualifizierten Namen für das Cmdlet verwenden. Sie können z. B. `ModuleName\CmdletName` verwenden.
* Wenn Sie das Runbook lokal in einer Hybrid Worker-Gruppe ausführen, stellen Sie sicher, dass das Modul und das Cmdlet auf dem Computer installiert sind, auf dem der Hybrid Worker gehostet wird.

## <a name="scenario-a-long-running-runbook-fails-to-complete"></a><a name="long-running-runbook"></a>Szenario: Ein Runbook mit langer Ausführungszeit kann nicht abgeschlossen werden

### <a name="issue"></a>Problem

Ihr Runbook befindet sich nach 3 Stunden Laufzeit im Zustand „Angehalten“. Möglicherweise wird auch diese Fehlermeldung angezeigt:

```error
The job was evicted and subsequently reached a Stopped state. The job cannot continue running.
```

Dies ist das in Azure-Sandboxes vorgesehene Verhalten aufgrund der Überwachung der Prozesse in Azure Automation auf [gleichmäßige Verteilung](../automation-runbook-execution.md#fair-share). Wenn ein Prozess länger als drei Stunden ausgeführt wird, wird das Runbook von der gleichmäßigen Verteilung automatisch beendet. Der Status eines Runbooks, der das Zeitlimit für die gleichmäßigen Verteilung überschreitet, ist je nach Runbooktyp unterschiedlich. PowerShell- und Python-Runbooks werden auf den Zustand „Angehalten“ festgelegt. PowerShell-Workflow-Runbooks werden auf „Fehlerhaft“ festgelegt.

### <a name="cause"></a>Ursache

Das Runbook hat den von der gleichmäßigen Verteilung in einer Azure-Sandbox vorgegebenen Grenzwert von drei Stunden überschritten.

### <a name="resolution"></a>Lösung

Die empfohlene Lösung ist das Ausführen des Runbooks in einem [Hybrid Runbook Worker](../automation-hrw-run-runbooks.md). Hybrid Worker unterliegen nicht dem von der gleichmäßigen Verteilung in einer Azure-Sandbox vorgegebenen Grenzwert von 3 Stunden. Runbooks, die auf Hybrid Runbook Workers ausgeführt werden, sollten weiterentwickelt werden, um das Neustartverhalten bei unerwarteten lokalen Infrastrukturproblemen zu unterstützen.

Eine weitere Lösung ist das Optimieren des Runbooks durch die Erstellung von [untergeordneten Runbooks](../automation-child-runbooks.md). Wenn Ihr Runbook für mehrere Ressourcen eine Schleife durch die gleiche Funktion durchführt, z. B. für einen Datenbankvorgang in mehreren Datenbanken, können Sie diese Funktion in ein untergeordnetes Runbook verschieben. Jedes dieser untergeordneten Runbooks wird parallel in separaten Prozessen ausgeführt. Dieses Verhalten führt dazu, dass die Gesamtdauer der Verarbeitung für das übergeordnete Runbook verringert wird.

Die PowerShell-Cmdlets für dieses Szenario, die das untergeordnete Runbook aktivieren, sind:

* [Start-AzureRMAutomationRunbook:](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook) Mit diesem Cmdlet können Sie ein Runbook starten und Parameter an das Runbook übergeben.

* [Get-AzureRmAutomationJob:](/powershell/module/azurerm.automation/get-azurermautomationjob) Mit diesem Cmdlet können Sie den Auftragsstatus für jedes untergeordnete Element überprüfen, wenn Vorgänge vorhanden sind, die nach Abschluss des untergeordneten Runbooks durchgeführt werden müssen.

## <a name="scenario-status-400-bad-request-when-calling-a-webhook"></a><a name="expired webhook"></a>Szenario: Status: 400 (ungültige Anforderung) beim Aufrufen eines Webhooks

### <a name="issue"></a>Problem

Beim Versuch, einen Webhook für ein Azure Automation-Runbook aufzurufen, tritt der folgende Fehler auf:

```error
400 Bad Request : This webhook has expired or is disabled
```

### <a name="cause"></a>Ursache

Der Webhook, den Sie aufrufen möchten, ist deaktiviert oder abgelaufen.

### <a name="resolution"></a>Lösung

Falls der Webhook deaktiviert ist, können Sie ihn über das Azure-Portal wieder aktivieren. Wenn der Webhook abgelaufen ist, müssen Sie ihn löschen und dann erneut erstellen. Das [Verlängern eines Webhooks](../automation-webhooks.md#renew-webhook) ist nur möglich, solange er noch nicht abgelaufen ist.

## <a name="scenario-429-the-request-rate-is-currently-too-large"></a><a name="429"></a>Szenario: 429: The request rate is currently too large. (Die Anforderungsrate ist zurzeit zu hoch.)

### <a name="issue"></a>Problem

Beim Ausführen des Cmdlets `Get-AzureRmAutomationJobOutput` wird folgende Fehlermeldung angezeigt:

```error
429: The request rate is currently too large. Please try again
```

### <a name="cause"></a>Ursache

Dieser Fehler kann beim Abrufen der Auftragsausgabe eines Runbooks auftreten, das über zahlreiche [ausführliche Datenströme](../automation-runbook-output-and-messages.md#verbose-stream) verfügt.

### <a name="resolution"></a>Lösung

Führen Sie eine der folgenden Aktionen aus, um diesen Fehler zu beheben.

* Bearbeiten Sie das Runbook, und verringern Sie die Anzahl ausgegebener Auftragsdatenströme.

* Verringern Sie die Anzahl von Datenströmen, die beim Ausführen des Cmdlets abgerufen werden. Hierzu können Sie den Wert des Parameters `Stream` für das Cmdlet `Get-AzureRmAutomationJobOutput` so festlegen, dass ausschließlich Ausgabedatenströme abgerufen werden. 

## <a name="scenario-powershell-job-fails-with-error-cannot-invoke-method"></a><a name="cannot-invoke-method"></a>Szenario: Bei einem PowerShell-Auftrag wird folgender Fehler ausgegeben: Cannot invoke method (Methode kann nicht aufgerufen werden)

### <a name="issue"></a>Problem

Beim Starten eines PowerShell-Auftrags in einem in Azure ausgeführten Runbook wird die folgende Fehlermeldung angezeigt:

```error
Exception was thrown - Cannot invoke method. Method invocation is supported only on core types in this language mode.
```

### <a name="cause"></a>Ursache

Dieser Fehler weist möglicherweise darauf hin, dass Runbooks, die in einer Azure-Sandbox ausgeführt werden, nicht im [Sprachmodus FullLanguage](/powershell/module/microsoft.powershell.core/about/about_language_modes) ausgeführt werden können.

### <a name="resolution"></a>Lösung

Es gibt zwei Möglichkeiten, diesen Fehler zu beheben.

* Verwenden Sie `Start-AzureRmAutomationRunbook` anstelle von `Start-Job` zum Starten des Runbooks.
* Führen Sie das Runbook auf einem Hybrid Runbook Worker aus.

Weitere Informationen zu diesem Verhalten und anderen von Azure Automation-Runbooks finden Sie unter [Runbook-Verhalten](../automation-runbook-execution.md#runbook-behavior).

## <a name="scenario-linux-hybrid-runbook-worker-receives-a-prompt-for-a-password-when-signing-a-runbook"></a>Szenario: Beim Signieren eines Runbooks erhält der Linux-Hybrid Runbook Worker eine Aufforderung zum Eingeben eines Kennworts

### <a name="issue"></a>Problem

Wenn Sie den Befehl `sudo` für einen Linux-Hybrid Runbook Worker ausführen, wird eine unerwartete Aufforderung für die Eingabe eines Kennworts abgerufen.

### <a name="cause"></a>Ursache

Das **nxautomationuser**-Konto für den Log Analytics-Agent für Linux ist in der **sudoers**-Datei nicht ordnungsgemäß konfiguriert. Der Hybrid Runbook Worker benötigt die entsprechende Konfiguration der Kontobenachrichtigungen und anderer Daten, damit Runbooks auf dem Linux-Runbook Worker signiert werden können.

### <a name="resolution"></a>Lösung

* Stellen Sie sicher, dass der Hybrid Runbook Worker auf dem Computer über die ausführbare Datei „GnuPG (GPG)“ verfügt.

* Überprüfen Sie die Konfiguration des **nxautomationuser**-Kontos in der **sudoers**-Datei. Lesen Sie sich den Artikel [Ausführen von Runbooks auf einem Hybrid Runbook Worker](../automation-hrw-run-runbooks.md) durch.

## <a name="scenario-cmdlet-failing-in-pnp-powershell-runbook-on-azure-automation"></a>Szenario: Cmdlet-Fehler in PnP-PowerShell-Runbook auf Azure Automation

### <a name="issue"></a>Problem

Wenn ein Runbook ein von PnP-PowerShell generiertes Objekt direkt in die Azure Automation-Ausgabe schreibt, kann die Cmdlet-Ausgabe nicht in Automation zurück gestreamt werden.

### <a name="cause"></a>Ursache

Dieses Problem tritt am häufigsten auf, wenn Azure Automation Runbooks verarbeitet, die PnP-PowerShell-Cmdlets aufrufen, z. B. `add-pnplistitem`, ohne dass die Rückgabeobjekte abgefangen werden.

### <a name="resolution"></a>Lösung

Bearbeiten Sie die Skripts, um den Variablen beliebige Rückgabewerte zuzuweisen, sodass die Cmdlets nicht versuchen, ganze Objekte in die Standardausgabe zu schreiben. Ein Skript kann den Ausgabedatenstrom wie unten dargestellt an ein Cmdlet umleiten.

```azurecli
  $null = add-pnplistitem
```

Wenn das Skript die Cmdletausgabe analysiert, muss das Skript die Ausgabe in einer Variablen speichern und die Variable bearbeiten, anstatt einfach die Ausgabe zu streamen.

```azurecli
$SomeVariable = add-pnplistitem ....
if ($SomeVariable.someproperty -eq ....
```

## <a name="my-problem-isnt-listed-above"></a><a name="other"></a>Mein Problem ist oben nicht aufgeführt.

In den folgenden Abschnitten sind weitere häufige Fehler aufgeführt. Dabei wird auf unterstützende Dokumentation verwiesen, um Ihnen bei der Behebung Ihres Problems zu helfen.

### <a name="hybrid-runbook-worker-doesnt-run-jobs-or-isnt-responding"></a>Hybrid Runbook Worker führt keine Aufträge aus oder reagiert nicht

Wenn Sie Aufträge nicht in Azure Automation, sondern auf einem Hybrid Worker ausführen, ist möglicherweise eine [Problembehandlung für den Hybrid Worker selbst](https://docs.microsoft.com/azure/automation/troubleshoot/hybrid-runbook-worker) erforderlich.

### <a name="runbook-fails-with-no-permission-or-some-variation"></a>Beim Runbook tritt der Fehler „Keine Berechtigung“ oder ein ähnlicher Fehler auf

Ausführende Konten verfügen unter Umständen nicht über die gleichen Berechtigungen für Azure-Ressourcen wie Ihr aktuelles Konto. Vergewissern Sie sich, dass Ihr ausführendes Konto über [Berechtigungen für den Zugriff auf alle Ressourcen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) verfügt, die in Ihrem Skript verwendet werden.

### <a name="issues-passing-parameters-into-webhooks"></a>Probleme beim Übergeben von Parametern an Webhooks

Lesen Sie den Artikel [Starten eines Runbooks mit einem Webhook](../automation-webhooks.md#parameters-used-when-the-webhook-starts-a-runbook), wenn Sie Unterstützung beim Übergeben von Parametern an Webhooks benötigen.

### <a name="issues-using-az-modules"></a>Probleme beim Verwenden von Az-Modulen

Die Verwendung von Az-Modulen und AzureRM-Modulen im gleichen Automation-Konto wird nicht unterstützt. Weitere Informationen finden Sie unter [Unterstützung für Az-Module in Azure Automation](https://docs.microsoft.com/azure/automation/az-modules).

### <a name="inconsistent-behavior-in-runbooks"></a>Inkonsistentes Verhalten in Runbooks

Führen Sie die im Artikel [Ausführen von Runbooks in Azure Automation](https://docs.microsoft.com/azure/automation/automation-runbook-execution#runbook-behavior) enthaltenen Schritte aus, um Probleme mit gleichzeitigen Aufträgen, mehrmals erstellten Ressourcen oder sonstiger zeitabhängiger Logik in Runbooks zu vermeiden.

### <a name="runbook-fails-with-the-error-no-permission-forbidden-403-or-some-variation"></a>Runbookfehler "No permission, Forbidden (403)" (Keine Berechtigung, Verboten) oder ein ähnlicher Fehler tritt auf

Ausführende Konten verfügen unter Umständen nicht über die gleichen Berechtigungen für Azure-Ressourcen wie Ihr aktuelles Konto. Vergewissern Sie sich, dass Ihr ausführendes Konto über [Berechtigungen für den Zugriff auf alle Ressourcen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) verfügt, die in Ihrem Skript verwendet werden.

### <a name="runbooks-were-working-but-suddenly-stopped"></a>Runbooks haben funktioniert, wurden aber plötzlich angehalten

* Stellen Sie sicher, dass das ausführende Konto nicht abgelaufen ist. Weitere Informationen finden Sie unter [Zertifizierungsverlängerung](https://docs.microsoft.com/azure/automation/manage-runas-account#cert-renewal).
* Falls Sie zum Starten von Runbooks [Webhooks](https://docs.microsoft.com/azure/automation/automation-webhooks#renew-webhook) verwenden, vergewissern Sie sich, dass der Webhook nicht abgelaufen ist.

### <a name="passing-parameters-into-webhooks"></a>Übergeben von Parametern an Webhooks

Lesen Sie den Artikel [Starten eines Runbooks mit einem Webhook](https://docs.microsoft.com/azure/automation/automation-webhooks#parameters-used-when-the-webhook-starts-a-runbook), wenn Sie Unterstützung beim Übergeben von Parametern an Webhooks benötigen.

### <a name="using-az-modules"></a>Verwenden von Az-Modulen

Die Verwendung von Az-Modulen und AzureRM-Modulen im gleichen Automation-Konto wird nicht unterstützt. Weitere Informationen finden Sie unter [Az-Module in Runbooks](https://docs.microsoft.com/azure/automation/az-modules).

### <a name="using-self-signed-certificates"></a>Verwenden selbstsignierter Zertifikate

Informationen zur Verwendung von selbstsignierten Zertifikaten finden Sie unter [Erstellen eines neuen Zertifikats](https://docs.microsoft.com/azure/automation/shared-resources/certificates#creating-a-new-certificate).

### <a name="access-denied-when-using-azure-sandbox-for-a-runbook"></a>Zugriff wird verweigert beim Verwenden einer Azure-Sandbox für ein Runbook

Die Azure-Sandbox verhindert den Zugriff auf sämtliche Out-of-Process-COM-Server. Sandkastenanwendungen oder -runbooks können beispielsweise die Windows-Verwaltungsinstrumentation (Windows Management Instrumentation, WMI) oder den Windows Installer-Dienst (msiserver.exe) nicht aufrufen. Weitere Informationen zur Verwendung der Sandbox finden Sie unter [Ausführen von Runbooks in Azure Automation](https://docs.microsoft.com/azure/automation/automation-runbook-execution).

## <a name="recommended-documents"></a>Empfohlene Dokumente

* [Starten eines Runbooks in Azure Automation](https://docs.microsoft.com/azure/automation/automation-starting-a-runbook)
* [Ausführen von Runbooks in Azure Automation](https://docs.microsoft.com/azure/automation/automation-runbook-execution)

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
* Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.
