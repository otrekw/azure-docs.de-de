---
title: Problembehandlung der Desired State Configuration (DSC) in Azure Automation
description: Dieser Artikel enthält Informationen zur Behandlung von Problemen mit der Konfiguration des gewünschten Zustands (Desired State Configuration, DSC).
services: automation
ms.service: automation
ms.subservice: ''
author: mgoedtel
ms.author: magoedte
ms.date: 04/16/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9f33dc9528d5f7043dda2c6fad207a9a51347a2b
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631494"
---
# <a name="troubleshoot-issues-with-azure-automation-desired-state-configuration-dsc"></a>Behandeln von Problemen mit der Azure Automation-Konfiguration des gewünschten Zustands (Desired State Configuration, DSC)

Dieser Artikel enthält Informationen zur Behandlung von Problemen mit der Konfiguration des gewünschten Zustands (Desired State Configuration, DSC).

## <a name="diagnosing-an-issue"></a>Diagnostizieren eines Problems

Wenn es beim Kompilieren oder Bereitstellen von Konfigurationen in Azure State Configuration zu Fehlern kommt, finden Sie hier einige Schritte, um das Problem zu diagnostizieren.

### <a name="1-ensure-that-your-configuration-compiles-successfully-on-the-local-machine"></a>1. Sicherstellen, dass Ihre Konfiguration auf dem lokalen Computer erfolgreich kompiliert wird

Azure State Configuration ist in PowerShell DSC integriert. Die Dokumentation zur DSC-Sprache und -Syntax finden Sie in der [PowerShell DSC-Dokumentation](https://docs.microsoft.com/powershell/scripting/overview).

Durch das Kompilieren der DSC-Konfiguration auf Ihrem lokalen Computer können Sie unter anderem die folgenden allgemeinen Fehler ermitteln und beheben:

   - Fehlende Module
   - Syntaxfehler
   - Logikfehler

### <a name="2-view-dsc-logs-on-your-node"></a>2. Anzeigen der DSC-Protokolle auf Ihrem Knoten

Wenn Ihre Konfiguration erfolgreich kompiliert wird, aber nicht auf einen Knoten angewendet werden kann, finden Sie in den DSC-Protokollen ausführliche Informationen. Informationen zum Speicherort dieser Protokolle finden Sie unter [Wo befinden sich die DSC-Ereignisprotokolle?](/powershell/scripting/dsc/troubleshooting/troubleshooting#where-are-dsc-event-logs).

Das [xDscDiagnostics](https://github.com/PowerShell/xDscDiagnostics)-Modul kann Ihnen helfen, die Informationen in den DSC-Protokollen analysieren. Wenn Sie sich an den Support wenden, fordert der diese Protokolle zur Diagnose des Problems an.

Sie können das „xDscDiagnostics“-Modul auf Ihrem lokalen Computer installieren. Folgen Sie hierzu den Anweisungen unter [Installation der stabilen Modulversion](https://github.com/PowerShell/xDscDiagnostics#install-the-stable-version-module).

Um das „xDscDiagnostics“-Modul auf Ihrem Azure-Computer zu installieren, können Sie [Invoke-AzVMRunCommand](/powershell/module/azurerm.compute/invoke-azurermvmruncommand) verwenden. Es ist auch möglich, die Option **Befehl ausführen** aus dem Portal zu verwenden. Folgen Sie hierzu den Schritten unter [Ausführen von PowerShell-Skripts in Ihrer Windows-VM mit „Befehl ausführen“](../../virtual-machines/windows/run-command.md).

Informationen zur Verwendung von „xDscDiagnostics“ finden Sie unter [Verwenden von „xDscDiagnostics“ zum Analysieren von DSC-Protokollen](/powershell/scripting/dsc/troubleshooting/troubleshooting#using-xdscdiagnostics-to-analyze-dsc-logs). Siehe auch [xDscDiagnostics-Cmdlets](https://github.com/PowerShell/xDscDiagnostics#cmdlets).

### <a name="3-ensure-that-nodes-and-the-automation-workspace-have-required-modules"></a>3. Sicherstellen, dass Knoten und der Automation-Arbeitsbereich über die erforderlichen Module verfügen

DSC ist von den Modulen abhängig, die auf dem Knoten installiert sind. Wenn Sie Azure Automation State Configuration verwenden, importieren Sie alle benötigten Module in Ihr Automation-Konto. Die hierzu erforderlichen Schritte finden Sie unter [Importieren von Modulen](../shared-resources/modules.md#importing-modules). Konfigurationen können auch von bestimmten Modulversionen abhängen. Weitere Informationen finden Sie unter [Problembehandlung von Modulen](shared-resources.md#modules).

## <a name="common-errors-when-working-with-dsc"></a>Häufige Fehler beim Arbeiten mit DSC

### <a name="scenario-a-configuration-with-special-characters-cannot-be-deleted-from-the-portal"></a><a name="unsupported-characters"></a>Szenario: Eine Konfiguration mit speziellen Zeichen kann nicht aus dem Portal gelöscht werden

#### <a name="issue"></a>Problem

Beim Versuch, eine DSC-Konfiguration aus dem Portal zu löschen, wird der folgende Fehler angezeigt:

```error
An error occurred while deleting the DSC configuration '<name>'.  Error-details: The argument configurationName with the value <name> is not valid.  Valid configuration names can contain only letters,  numbers, and underscores.  The name must start with a letter.  The length of the name must be between 1 and 64 characters.
```

#### <a name="cause"></a>Ursache

Der Fehler ist ein vorübergehendes Problem, das gelöst werden soll.

#### <a name="resolution"></a>Lösung

* Verwenden Sie das Azure-Cmdlet „Remove-AzAutomationDscConfiguration“, um die Konfiguration zu löschen.
* Die Dokumentation für dieses Cmdlet wurde noch nicht aktualisiert.  Nutzen Sie bis dahin bitte die Dokumentation für das AzureRM-Modul.
  * [Remove-AzureRmAutomationDSCConfiguration](/powershell/module/azurerm.automation/Remove-AzureRmAutomationDscConfiguration)

### <a name="scenario-failed-to-register-dsc-agent"></a><a name="failed-to-register-agent"></a>Szenario: Fehler beim Registrieren des DSC-Agents

#### <a name="issue"></a>Problem

Bei der Ausführung von `Set-DscLocalConfigurationManager` oder eines anderen DSC-Cmdlets tritt der folgende Fehler auf:

```error
Registration of the Dsc Agent with the server
https://<location>-agentservice-prod-1.azure-automation.net/accounts/00000000-0000-0000-0000-000000000000 failed. The
underlying error is: Failed to register Dsc Agent with AgentId 00000000-0000-0000-0000-000000000000 with the server htt
ps://<location>-agentservice-prod-1.azure-automation.net/accounts/00000000-0000-0000-0000-000000000000/Nodes(AgentId='00000000-0000-0000-0000-000000000000'). .
    + CategoryInfo          : InvalidResult: (root/Microsoft/...gurationManager:String) [], CimException
    + FullyQualifiedErrorId : RegisterDscAgentCommandFailed,Microsoft.PowerShell.DesiredStateConfiguration.Commands.Re
   gisterDscAgentCommand
    + PSComputerName        : <computerName>
```

#### <a name="cause"></a>Ursache

Dieser Fehler wird normalerweise durch eine Firewall, durch einen Computer hinter einem Proxyserver oder durch andere Netzwerkfehler verursacht.

#### <a name="resolution"></a>Lösung

Stellen Sie sicher, dass Ihr Computer Zugriff auf die richtigen Endpunkte für Azure Automation DSC hat, und wiederholen Sie den Vorgang. Eine Liste der erforderlichen Ports und Adressen finden Sie unter [Übersicht über Azure Automation State Configuration](../automation-dsc-overview.md#network-planning).

### <a name="a-nameunauthorizedscenario-status-reports-return-response-code-unauthorized"></a><a name="unauthorized"><a/>Szenario: Antwortcode „Nicht autorisiert“ in Statusberichten

#### <a name="issue"></a>Problem

Wenn Sie einen Knoten mit State Configuration (DSC) registrieren, erhalten Sie eine der folgenden Fehlermeldungen:

```error
The attempt to send status report to the server https://{your Automation account URL}/accounts/xxxxxxxxxxxxxxxxxxxxxx/Nodes(AgentId='xxxxxxxxxxxxxxxxxxxxxxxxx')/SendReport returned unexpected response code Unauthorized.
```

```error
VM has reported a failure when processing extension 'Microsoft.Powershell.DSC / Registration of the Dsc Agent with the server failed.
```

### <a name="cause"></a>Ursache

Dieses Problem wird durch ein ungültiges oder abgelaufenes Zertifikat verursacht.  Weitere Informationen finden Sie unter [Ablauf und erneute Registrierung eines Zertifikats](../automation-dsc-onboarding.md#re-registering-a-node).

### <a name="resolution"></a>Lösung

Führen Sie folgenden Schritte aus, um den fehlerhaften DSC-Knoten neu zu registrieren:

Heben Sie zunächst die Registrierung des Knotens auf:

1. Navigieren Sie im Azure-Portal zu **Startseite** -> **Automation-Konten** -> {Ihr Automation-Konto} -> **State Configuration (DSC)** .
2. Klicken Sie auf „Knoten“ und anschließend auf den fehlerhaften Knoten.
3. Klicken Sie auf „Registrierung aufheben“, um die Registrierung des Knotens aufzuheben.

Deinstallieren Sie als Nächstes die DSC-Erweiterung auf dem Knoten.

1. Navigieren Sie im Azure-Portal zu **Startseite** -> **Virtueller Computer** > {fehlerhafter Knoten} > **Erweiterungen**.
2. Klicken Sie auf „Microsoft.Powershell.DSC“.
3. Klicken Sie auf „Deinstallieren“, um die PowerShell DSC-Erweiterung zu deinstallieren.

Entfernen Sie nun alle ungültigen oder abgelaufenen Zertifikate aus dem Knoten.

Führen Sie auf dem fehlerhaften Knoten den folgenden Befehl an einer PowerShell-Eingabeaufforderung mit erhöhten Rechten aus:

```powershell
$certs = @()
$certs += dir cert:\localmachine\my | ?{$_.FriendlyName -like "DSC"}
$certs += dir cert:\localmachine\my | ?{$_.FriendlyName -like "DSC-OaaS Client Authentication"}
$certs += dir cert:\localmachine\CA | ?{$_.subject -like "CN=AzureDSCExtension*"}
"";"== DSC Certificates found: " + $certs.Count
$certs | FL ThumbPrint,FriendlyName,Subject
If (($certs.Count) -gt 0)
{ 
    ForEach ($Cert in $certs) 
    {
        RD -LiteralPath ($Cert.Pspath) 
    }
}
```

Registrieren Sie abschließend den fehlerhaften Knoten wieder:

1. Navigieren Sie im Azure-Portal zu **Startseite** -> **Automation-Konten** -> {Ihr Automation-Konto} -> **State Configuration (DSC)** .
2. Klicken Sie auf „Knoten“.
3. Klicken Sie auf die Schaltfläche „Hinzufügen“.
4. Wählen Sie den fehlerhaften Knoten aus.
5. Klicken Sie auf „Verbinden“, und wählen Sie die gewünschten Optionen aus.

### <a name="scenario-node-is-in-failed-status-with-a-not-found-error"></a><a name="failed-not-found"></a>Szenario: Knoten hat den Fehlerstatus „Nicht gefunden“

#### <a name="issue"></a>Problem

Der Knoten hat den Status **Fehler** mit der folgenden Meldung zurückgegeben:

```error
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

#### <a name="cause"></a>Ursache

Dieser Fehler tritt normalerweise auf, wenn der Knoten einem Konfigurationsnamen (z.B. ABC) anstatt einem Knotenkonfigurationsnamen (z.B. ABC.WebServer) zugewiesen ist.

#### <a name="resolution"></a>Lösung

* Stellen Sie sicher, dass Sie den Knoten mit dem „Knotenkonfigurationsnamen“ und nicht mit dem „Konfigurationsnamen“ zuweisen.
* Einem Knoten können Sie über das Azure-Portal oder mit einem PowerShell-Cmdlet eine Knotenkonfiguration zuweisen.

  * Um über das Azure-Portal einem Knoten eine Knotenkonfiguration zuzuweisen, öffnen Sie die Seite **DSC-Knoten**, wählen Sie dann einen Knoten aus, und klicken Sie auf die Schaltfläche **Knotenkonfiguration zuweisen**.
  * Um einem Knoten mit einem PowerShell-Cmdlet eine Knotenkonfiguration zuzuweisen, verwenden Sie das Cmdlet **Set-AzureRmAutomationDscNode**.

### <a name="scenario-no-node-configurations-mof-files-were-produced-when-a-configuration-is-compiled"></a><a name="no-mof-files"></a>Szenario: Bei der Kompilierung einer Konfiguration wurden keine Knotenkonfigurationen (MOF-Dateien) erstellt

#### <a name="issue"></a>Problem

Ihr DSC-Kompilierungsauftrag wird mit folgendem Fehler abgebrochen:

```error
Compilation completed successfully, but no node configuration.mofs were generated.
```

#### <a name="cause"></a>Ursache

Wenn der Ausdruck nach dem Schlüsselwort **Node** in der DSC-Konfiguration mit `$null` ausgewertet wird, werden keine Knotenkonfigurationen erstellt.

#### <a name="resolution"></a>Lösung

Sie können dieses Problem mit jeder der folgenden Lösungen beheben:

* Stellen Sie sicher, dass der Ausdruck neben dem Schlüsselwort **Node** in der Konfigurationsdefinition nicht mit „$null“ ausgewertet wird.
* Wenn Sie bei der Kompilierung der Konfiguration ConfigurationData übergeben, stellen Sie sicher, dass Sie die erwarteten Werte übergeben, die für die Konfiguration aus [ConfigurationData](../automation-dsc-compile.md)erforderlich sind.

### <a name="scenario-the-dsc-node-report-becomes-stuck-in-progress-state"></a><a name="dsc-in-progress"></a>Szenario: Der DSC-Knotenbericht bleibt mit dem Status „In Bearbeitung“ hängen

#### <a name="issue"></a>Problem

Der DSC-Agent gibt Folgendes aus:

```error
No instance found with given property values
```

#### <a name="cause"></a>Ursache

Sie haben Ihre WMF-Version aktualisiert und WMI beschädigt.

#### <a name="resolution"></a>Lösung

Befolgen Sie zum Beheben des Problems die Anweisungen im Artikel [Bekannte Probleme und Einschränkungen bei DSC](https://docs.microsoft.com/powershell/scripting/wmf/known-issues/known-issues-dsc).

### <a name="scenario-unable-to-use-a-credential-in-a-dsc-configuration"></a><a name="issue-using-credential"></a>Szenario: In einer DSC-Konfiguration können keine Anmeldeinformationen verwendet werden

#### <a name="issue"></a>Problem

Ihr DSC-Kompilierungsauftrag wurde mit folgendem Fehler abgebrochen:

```error
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

#### <a name="cause"></a>Ursache

Sie haben Anmeldeinformationen in einer Konfiguration verwendet, aber keine ordnungsgemäßen **ConfigurationData** angegeben, über die **PSDscAllowPlainTextPassword** für jede Knotenkonfiguration auf TRUE festgelegt wird.

#### <a name="resolution"></a>Lösung

* Stellen Sie sicher, dass Sie die ordnungsgemäßen **ConfigurationData** übergeben, über die **PSDscAllowPlainTextPassword** für jede Knotenkonfiguration auf „true“ festgelegt wird. Weitere Informationen finden Sie unter [Kompilieren von DSC-Konfigurationen in Azure Automation State Configuration](../automation-dsc-compile.md).

### <a name="scenario-onboarding-from-dsc-extension-failure-processing-extension-error"></a><a name="failure-processing-extension"></a>Szenario: Onboarding über DSC-Erweiterung, Fehler beim Verarbeiten der Erweiterung

#### <a name="issue"></a>Problem

Beim Onboarding mithilfe der DSC-Erweiterung tritt der folgende Fehler auf:

```error
VM has reported a failure when processing extension 'Microsoft.Powershell.DSC'. Error message: \"DSC COnfiguration 'RegistrationMetaConfigV2' completed with error(s). Following are the first few: Registration of the Dsc Agent with the server <url> failed. The underlying error is: The attempt to register Dsc Agent with Agent Id <ID> with the server <url> return unexpected response code BadRequest. .\".
```

#### <a name="cause"></a>Ursache

Dieser Fehler tritt in der Regel auf, wenn dem Knoten ein Knotenkonfigurationsname zugewiesen wurde, der im Dienst nicht vorhanden ist.

#### <a name="resolution"></a>Lösung

* Stellen Sie sicher, dass Sie dem Knoten einen Knotenkonfigurationsnamen zuweisen, der genau mit dem im Dienst vorhandenen Namen übereinstimmt.
* Sie können festlegen, dass kein Knotenkonfigurationsname angegeben werden soll. In dem Fall erfolgt ein Onboarding des Knotens, aber keine Zuweisung einer Knotenkonfiguration.

### <a name="scenario-registering-a-node-with-powershell-returns-the-error-one-or-more-errors-occurred"></a><a name="cross-subscription"></a>Szenario: Fehler „Es ist mindestens ein Fehler aufgetreten.“ beim Registrieren eines Knotens mit PowerShell

#### <a name="issue"></a>Problem

Wenn Sie einen Knoten mithilfe von `Register-AzAutomationDSCNode` oder `Register-AzureRMAutomationDSCNode`registrieren, tritt der folgende Fehler auf:

```error
One or more errors occurred.
```

#### <a name="cause"></a>Ursache

Dieser Fehler tritt auf, wenn Sie versuchen, einen Knoten zu registrieren, der sich in einem anderen Abonnement befindet als das Automation-Konto.

#### <a name="resolution"></a>Lösung

Behandeln Sie den Knoten aus dem anderen Abonnement wie einen Knoten in einer separaten Cloud oder wie einen lokalen Knoten.

Gehen Sie wie folgt vor, um den Knoten zu registrieren:

* Windows: [Physische/virtuelle Windows-Computer, lokal oder in einer anderen Cloud als Azure/AWS](../automation-dsc-onboarding.md#onboarding-physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances)
* Linux: [Physische/virtuelle Linux-Computer, lokal oder in einer anderen Cloud als Azure](../automation-dsc-onboarding.md#onboarding-physicalvirtual-linux-machines-on-premises-or-in-a-cloud-other-than-azure)

### <a name="scenario-error-message---provisioning-failed"></a><a name="agent-has-a-problem"></a>Szenario: Fehlermeldung „Fehler beim Bereitstellen“

#### <a name="issue"></a>Problem

Beim Registrieren eines Knotens tritt der folgende Fehler auf:

```error
Provisioning has failed
```

#### <a name="cause"></a>Ursache

Diese Meldung ist auf ein Konnektivitätsproblem zwischen dem Knoten und Azure zurückzuführen.

#### <a name="resolution"></a>Lösung

Ermitteln Sie, ob sich Ihr Knoten in einem privaten virtuellen Netzwerk befindet oder ob andere Probleme beim Herstellen einer Verbindung mit Azure vorliegen.

Weitere Informationen finden Sie unter [Beheben von Fehlern beim Integrieren von Lösungen](onboarding.md).

### <a name="scenario-applying-a-configuration-in-linux-a-failure-occurs-with-a-general-error"></a><a name="failure-linux-temp-noexec"></a>Szenario: Anwenden einer Konfiguration in Linux, dabei tritt allgemeiner Fehler auf

#### <a name="issue"></a>Problem

Wenn Sie eine Konfiguration in Linux anwenden, bei der der folgende Fehler auftritt:

```error
This event indicates that failure happens when LCM is processing the configuration. ErrorId is 1. ErrorDetail is The SendConfigurationApply function did not succeed.. ResourceId is [resource]name and SourceInfo is ::nnn::n::resource. ErrorMessage is A general error occurred, not covered by a more specific error code..
```

#### <a name="cause"></a>Ursache

Kunden haben festgestellt, dass die aktuelle Version von DSC Konfigurationen nicht übernimmt, wenn der Speicherort `/tmp` auf `noexec` festgelegt ist.

#### <a name="resolution"></a>Lösung

* Entfernen Sie die Option `noexec` aus dem Speicherort `/tmp`.

### <a name="scenario-node-configuration-names-that-overlap-could-result-in-bad-release"></a><a name="compilation-node-name-overlap"></a>Szenario: Sich überschneidende Knotenkonfigurationsnamen können zu einem fehlerhaften Release führen.

#### <a name="issue"></a>Problem

Wenn zum Generieren mehrerer Knotenkonfigurationen ein einzelnes Skript verwendet wird und einige der Knotenkonfigurationen einen Namen besitzen, der in anderen enthalten ist, kann ein Problem beim Kompilierungsdienst dazu führen, dass die falsche Konfiguration zugewiesen wird.  Dies ist nur der Fall, wenn ein einzelnes Skript zum Generieren von Konfigurationen mit Konfigurationsdaten pro Knoten verwendet wird und die Namensüberschneidung am Anfang der Zeichenfolge auftritt.

Beispiel: Ein einzelnes Konfigurationsskript wird zum Generieren von Konfigurationen auf der Grundlage von Knotendaten verwendet, die unter Verwendung von Cmdlets als Hashtabelle übergeben werden, und die Knotendaten enthalten einen Server namens „server“ und „1server“.

#### <a name="cause"></a>Ursache

Bekanntes Problem beim Kompilierungsdienst

#### <a name="resolution"></a>Lösung

Sie umgehen das Problem am besten, indem Sie die Kompilierung lokal oder in einer CI/CD-Pipeline ausführen und die MOF-Dateien anschließend direkt in den Dienst hochladen.  Muss die Kompilierung im Dienst erfolgen, besteht die zweitbeste Problemumgehung darin, die Kompilierungsaufträge aufzuteilen, damit es keine Überschneidungen bei den Namen gibt.

### <a name="scenario-gateway-timeout-error-on-dsc-configuration-upload"></a><a name="gateway-timeout"></a>Szenario: Gatewaytimeoutfehler beim Hochladen der DSC-Konfiguration

#### <a name="issue"></a>Problem

Wenn eine DSC-Konfiguration hochgeladen wird, erhalten Sie einen `GatewayTimeout`-Fehler. 

#### <a name="cause"></a>Ursache

Bei DSC-Konfigurationen, deren Kompilierung lange Zeit in Anspruch nimmt, kann dieser Fehler verursacht werden.

#### <a name="resolution"></a>Lösung

Sie können die Analyse Ihrer DSC-Konfigurationen beschleunigen, indem Sie den `ModuleName`-Parameter explizit für alle `Import-DscResource`-Aufrufe einschließen. Weitere Informationen finden Sie unter [Verwenden von Import-DSCResource](https://docs.microsoft.com/powershell/scripting/dsc/configurations/import-dscresource?view=powershell-5.1).

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
* Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.
