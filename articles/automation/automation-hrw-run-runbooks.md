---
title: Ausführen von Runbooks für Hybrid Runbook Worker in Azure Automation
description: Dieser Artikel enthält Informationen zur Ausführung von Runbooks auf Computern in Ihrem lokalen Datencenter oder bei Ihrem Cloudanbieter mit der Hybrid Runbook Worker-Rolle.
services: automation
ms.subservice: process-automation
ms.date: 01/29/2019
ms.topic: conceptual
ms.openlocfilehash: 9b9196cde45686e42d1baf7faedf94bdb73acccc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79367056"
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a>Ausführen von Runbooks auf einem Hybrid Runbook Worker

Runbooks, die für einen Hybrid Runbook Worker konzipiert sind, verwalten in der Regel Ressourcen auf dem lokalen Computer oder in der lokalen Umgebung, in der der Worker bereitgestellt ist. Runbooks in Azure Automation verwalten üblicherweise Ressourcen in der Azure-Cloud. Auch wenn sie unterschiedlich verwendet werden, sind Runbooks, die in Azure Automation ausgeführt werden, und Runbooks, die auf einem Hybrid Runbook Worker ausgeführt werden, von der Struktur her identisch.

Wenn Sie ein Runbook zur Ausführung auf einem Hybrid Runbook Worker erstellen, sollten Sie es auf dem Computer testen, der den Worker hostet. Auf dem Hostcomputer sind alle PowerShell-Module und Netzwerkzugriff vorhanden, die Sie benötigen, um die lokalen Ressourcen zu verwalten und darauf zuzugreifen. Sobald Sie das Runbook auf dem Hybrid Runbook Worker-Computer getestet haben, können Sie es in die Azure Automation-Umgebung hochladen, wo es auf dem Worker ausgeführt werden kann. 

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="runbook-permissions-for-a-hybrid-runbook-worker"></a>Runbook-Berechtigungen für einen Hybrid Runbook Worker

Da sie auf Nicht-Azure-Ressourcen zugreifen, können Runbooks, die auf einem Hybrid Runbook Worker laufen, nicht den Authentifizierungsmechanismus verwenden, der normalerweise von Runbooks zur Authentifizierung bei Azure-Ressourcen verwendet wird. Ein Runbook stellt entweder eine eigene Authentifizierung für lokale Ressourcen bereit oder konfiguriert die Authentifizierung mithilfe von [verwalteten Identitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager). Alternativ hierzu können Sie auch ein ausführendes Konto angeben, um einen Benutzerkontext für alle Runbooks bereitzustellen.

### <a name="runbook-authentication"></a>Authentifizierung von Runbooks

Standardmäßig laufen Runbooks auf dem lokalen Computer. Unter Windows werden sie im Kontext des lokalen Kontos „System“ ausgeführt. Unter Linux werden sie im Kontext des speziellen Benutzerkontos **nxautomation** ausgeführt. In beiden Szenarien müssen die Runbooks ihre eigene Authentifizierung bei Ressourcen ermöglichen, auf die sie zugreifen.

Sie können die Objekte [Credential](automation-credentials.md) und [Certificate](automation-certificates.md) in Ihrem Runbook mit Cmdlets verwenden, mit denen Sie Anmeldeinformationen angeben können, damit sich das Runbook bei verschiedenen Ressourcen authentifizieren kann. Das folgende Beispiel zeigt einen Teil eines Runbooks, mit dem ein Computer neu gestartet wird. Es werden Anmeldeinformationen aus einem Anmeldeinformationsobjekt und der Name des Computers aus einem Variablenobjekt abgerufen. Anschließend werden diese Werte im Cmdlet `Restart-Computer` eingesetzt.

```powershell
$Cred = Get-AutomationPSCredential -Name "MyCredential"
$Computer = Get-AutomationVariable -Name "ComputerName"

Restart-Computer -ComputerName $Computer -Credential $Cred
```

Sie können auch eine Aktivität des Typs [InlineScript](automation-powershell-workflow.md#inlinescript) verwenden. `InlineScript` ermöglicht, dass Sie Codeblöcke mit den im [allgemeinen Parameter PSCredential](/powershell/module/psworkflow/about/about_workflowcommonparameters) angegebenen Anmeldeinformationen auf einem anderen Computer ausführen können.

### <a name="run-as-account"></a>Ausführendes Konto

Um zu vermeiden, dass Ihre Runbooks ihre eigene Authentifizierung bei lokalen Ressourcen bereitstellen müssen, können Sie für eine Hybrid Runbook Worker-Gruppe ein ausführendes Konto angeben. Dazu müssen Sie ein [Anmeldeinformationsobjekt](automation-credentials.md) mit Zugriff auf lokale Ressourcen definieren. Zu diesen Ressourcen gehören Zertifikatsspeicher und alle Runbooks, die unter diesen Anmeldeinformationen auf einem Hybrid Runbook Worker in der Gruppe ausgeführt werden.

Der Benutzername für die Anmeldeinformationen muss eines der folgenden Formate haben:

* Domäne\Benutzername
* username@domain
* Benutzername (für Konten, die für den lokalen Computer lokal sind)

Befolgen Sie das folgende Verfahren, um ein ausführendes Konto für eine Hybrid Runbook Worker-Gruppe anzugeben.

1. Erstellen Sie ein [Anmeldeinformationsobjekt](automation-credentials.md) mit Zugriff auf lokale Ressourcen.
2. Wählen Sie im Azure-Portal das Automation-Konto aus.
3. Wählen Sie die Kachel **Hybrid Worker-Gruppen** und dann die Gruppe aus.
4. Wählen Sie **Alle Einstellungen** gefolgt von **Einstellungen für Hybrid Worker-Gruppe** aus.
5. Ändern Sie die Einstellung von **Ausführen als** von **Standard** in **Benutzerdefiniert**.
6. Wählen Sie die Anmeldeinformationen aus, und klicken Sie auf **Speichern**.

### <a name="managed-identities-for-azure-resources"></a><a name="managed-identities-for-azure-resources"></a>Verwaltete Identitäten für Azure-Ressourcen

Für Hybrid Runbook Worker, die auf virtuellen Azure-Computern ausgeführt werden, können verwaltete Identitäten genutzt werden, um Azure-Ressourcen bei anderen Azure-Ressourcen zu authentifizieren. Der Einsatz verwalteter Identitäten für Azure-Ressourcen anstelle ausführender Konten bietet Vorteile, da die folgenden Schritte nicht erforderlich sind:

* Das Exportieren des Zertifikats für das ausführende Konto und anschließende Importieren in den Hybrid Runbook Worker.
* Das Verlängern des vom ausführenden Konto verwendeten Zertifikats.
* Das Verarbeiten des Verbindungsobjekts des ausführenden Kontos.

Führen Sie die folgenden Schritte aus, um eine verwaltete Identität für Azure-Ressourcen auf einem Hybrid Runbook Worker zu verwenden.

1. Erstellen Sie eine Azure-VM.
2. Konfigurieren Sie verwaltete Identitäten für Azure-Ressourcen auf der VM. Weitere Informationen finden Sie unter [Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Computer über das Azure-Portal](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md#enable-system-assigned-managed-identity-on-an-existing-vm).
3. Gewähren Sie der VM Zugriff auf eine Ressourcengruppe in Resource Manager. Weitere Informationen finden Sie unter [Verwenden einer systemseitig zugewiesenen verwalteten Identität eines virtuellen Windows-Computers für den Zugriff auf Resource Manager](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager).
4. Installieren Sie den Hybrid Runbook Worker auf der VM. Weitere Informationen finden sie unter [Bereitstellen eines Windows Hybrid Runbook Workers](automation-windows-hrw-install.md).
5. Aktualisieren Sie das Runbook so, dass das Cmdlet [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.5.0) mit dem Parameter `Identity` zur Authentifizierung bei Azure-Ressourcen verwendet wird. Diese Konfiguration reduziert die Notwendigkeit, ein ausführendes Konto zu verwenden und die zugehörige Kontoverwaltung durchzuführen.

```powershell
    # Connect to Azure using the managed identities for Azure resources identity configured on the Azure VM that is hosting the hybrid runbook worker
    Connect-AzAccount -Identity

    # Get all VM names from the subscription
    Get-AzVM | Select Name
```

> [!NOTE]
> `Connect-AzAccount -Identity` funktioniert für einen Hybrid Runbook Worker mit einer vom System und einer einzelnen vom Benutzer zugewiesenen Identität. Wenn Sie auf dem Hybrid Runbook Worker mehrere vom Benutzer zugewiesene Identitäten verwenden, muss Ihr Runbook für `Connect-AzAccount` den Parameter *AccountId* angeben, um eine bestimmte vom Benutzer zugewiesene Identität auszuwählen.

### <a name="automation-run-as-account"></a><a name="runas-script"></a>Ausführendes Automation-Konto

Im Rahmen Ihres automatisierten Buildprozesses für die Bereitstellung von Ressourcen in Azure ist es unter Umständen erforderlich, auf lokale Systeme zuzugreifen, um eine Aufgabe oder eine Reihe von Schritten in Ihrer Bereitstellungssequenz zu unterstützen. Um die Authentifizierung bei Azure mit einem ausführenden Konto zu ermöglichen, müssen Sie das Zertifikat für das ausführende Konto installieren.

Das folgende PowerShell-Runbook, **Export-RunAsCertificateToHybridWorker**, exportiert das Zertifikat für das ausführende Konto aus Ihrem Azure Automation-Konto. Es lädt das Zertifikat herunter und importiert es in den Zertifikatspeicher für den lokalen Computer auf einem Hybrid Runbook Worker, der mit dem gleichen Konto verbunden ist. Nach diesem Schritt überprüft das Runbook, ob der Worker sich unter Verwendung des ausführenden Kontos erfolgreich bei Azure authentifizieren kann.

```azurepowershell-interactive
<#PSScriptInfo
.VERSION 1.0
.GUID 3a796b9a-623d-499d-86c8-c249f10a6986
.AUTHOR Azure Automation Team
.COMPANYNAME Microsoft
.COPYRIGHT
.TAGS Azure Automation
.LICENSEURI
.PROJECTURI
.ICONURI
.EXTERNALMODULEDEPENDENCIES
.REQUIREDSCRIPTS
.EXTERNALSCRIPTDEPENDENCIES
.RELEASENOTES
#>

<#
.SYNOPSIS
Exports the Run As certificate from an Azure Automation account to a hybrid worker in that account.

.DESCRIPTION
This runbook exports the Run As certificate from an Azure Automation account to a hybrid worker in that account. Run this runbook on the hybrid worker where you want the certificate installed. This allows the use of the AzureRunAsConnection to authenticate to Azure and manage Azure resources from runbooks running on the hybrid worker.

.EXAMPLE
.\Export-RunAsCertificateToHybridWorker

.NOTES
LASTEDIT: 2016.10.13
#>

# Generate the password used for this certificate
Add-Type -AssemblyName System.Web -ErrorAction SilentlyContinue | Out-Null
$Password = [System.Web.Security.Membership]::GeneratePassword(25, 10)

# Stop on errors
$ErrorActionPreference = 'stop'

# Get the management certificate that will be used to make calls into Azure Service Management resources
$RunAsCert = Get-AutomationCertificate -Name "AzureRunAsCertificate"

# location to store temporary certificate in the Automation service host
$CertPath = Join-Path $env:temp  "AzureRunAsCertificate.pfx"

# Save the certificate
$Cert = $RunAsCert.Export("pfx",$Password)
Set-Content -Value $Cert -Path $CertPath -Force -Encoding Byte | Write-Verbose

Write-Output ("Importing certificate into $env:computername local machine root store from " + $CertPath)
$SecurePassword = ConvertTo-SecureString $Password -AsPlainText -Force
Import-PfxCertificate -FilePath $CertPath -CertStoreLocation Cert:\LocalMachine\My -Password $SecurePassword -Exportable | Write-Verbose

# Test to see if authentication to Azure Resource Manager is working
$RunAsConnection = Get-AutomationConnection -Name "AzureRunAsConnection"

Connect-AzAccount `
    -ServicePrincipal `
    -Tenant $RunAsConnection.TenantId `
    -ApplicationId $RunAsConnection.ApplicationId `
    -CertificateThumbprint $RunAsConnection.CertificateThumbprint | Write-Verbose

Set-AzContext -Subscription $RunAsConnection.SubscriptionID | Write-Verbose

# List automation accounts to confirm that Azure Resource Manager calls are working
Get-AzAutomationAccount | Select-Object AutomationAccountName
```

>[!NOTE]
>Für PowerShell-Runbooks sind `Add-AzAccount` und `Add-AzureRMAccount` Aliase für `Connect-AzAccount`. Wenn `Connect-AzAccount` beim Durchsuchen der Bibliothekselemente nicht angezeigt wird, können Sie `Add-AzAccount` verwenden oder Ihre Module in Ihrem Automation-Konto aktualisieren.

So schließen Sie die Vorbereitung des ausführenden Kontos ab

1. Speichern Sie das Runbook **Export-RunAsCertificateToHybridWorker** mit der Erweiterung **.ps1** auf Ihrem Computer.
2. Importieren Sie es in Ihr Automation-Konto.
3. Bearbeiten Sie das Runbook, indem Sie den Wert der Variablen `Password` oder Ihr eigenes Kennwort ändern. 
4. Veröffentlichen Sie das Runbook.
5. Führen Sie das Runbook dann mit der Hybrid Runbook Worker-Gruppe als Ziel aus, die Runbooks mithilfe des ausführenden Kontos ausführt und authentifiziert. 
6. Der Auftragsdatenstrom meldet den Versuch, das Zertifikat in den Speicher des lokalen Computers zu importieren, und fügt weitere Zeilen mit Informationen hinzu. Dieses Verhalten hängt von der Anzahl der Automation-Konten, die Sie in Ihrem Abonnement definieren, und vom Grad des Erfolgs der Authentifizierung ab.

## <a name="job-behavior-on-hybrid-runbook-workers"></a>Auftragsverhalten auf Hybrid Runbook Workern

Azure Automation behandelt Aufträge in Hybrid Runbook Workern etwas anders als Aufträge, die in Azure-Sandboxes ausgeführt werden. Ein Hauptunterschied besteht darin, dass die Auftragsdauer auf den Runbook Workern nicht begrenzt ist. In Azure-Sandboxes ausgeführte Runbooks sind zur Sicherstellung einer [gleichmäßigen Verteilung](automation-runbook-execution.md#fair-share) auf drei Stunden beschränkt.

Wenn Sie über ein Runbook mit langer Ausführungsdauer verfügen, stellen Sie sicher, dass es gegenüber einem möglichen Neustart resilient ist (z. B. wenn der Computer neu gestartet wird, der den Worker hostet). Wenn der Hostcomputer des Hybrid Runbook Workers neu gestartet wird, werden alle ausgeführten Runbookaufträge von vorn oder – im Fall von PowerShell-Workflow-Runbooks – beim letzten Prüfpunkt neu gestartet. Nachdem ein Runbookauftrag mehr als dreimal neu gestartet wurde, wird er angehalten.

Denken Sie daran, dass Aufträge für Hybrid Runbook Worker unter Windows unter dem lokalen Konto „System“ oder unter Linux unter dem Konto **nxautomation** ausgeführt werden. Unter Linux müssen Sie sicherstellen, dass das Konto **nxautomation** Zugriff auf den Speicherort der Runbookmodule hat. Wenn Sie das Cmdlet [Install-Module](/powershell/module/powershellget/install-module) verwenden, stellen Sie sicher, dass Sie „AllUsers“ für den Parameter `Scope` angeben, um sicherzustellen, dass das Konto **nxautomation** Zugriff hat. Weitere Informationen zu PowerShell unter Linux finden Sie unter [Bekannte Probleme bei PowerShell auf anderen Plattformen als Windows](https://docs.microsoft.com/powershell/scripting/whats-new/known-issues-ps6?view=powershell-6#known-issues-for-powershell-on-non-windows-platforms).

## <a name="starting-a-runbook-on-a-hybrid-runbook-worker"></a>Starten eines Runbooks auf einem Hybrid Runbook Worker

Unter [Starten eines Runbooks in Azure Automation](automation-starting-a-runbook.md) werden die verschiedenen Methoden zum Starten eines Runbooks beschrieben. Beim Start eines Runbooks auf einem Hybrid Runbook Worker wird die Option **Ausführen auf** verwendet, die es Ihnen ermöglicht, den Namen einer Hybrid Runbook Worker-Gruppe anzugeben. Wenn eine Gruppe angegeben wird, ruft einer der Worker in dieser Gruppe das Runbook ab und führt es aus. Wenn Ihr Runbook diese Option nicht angibt, führt Azure Automation das Runbook wie gewohnt aus.

Wenn Sie ein Runbook im Azure-Portal starten, wird die Option **Ausführen auf** angezeigt, für die Sie entweder **Azure** oder **Hybrid Worker** auswählen können. Bei Auswahl von **Hybrid Worker** können Sie anschließend in einer Dropdownliste die gewünschte Hybrid Runbook Worker-Gruppe wählen.

Verwenden Sie den `RunOn`-Parameter mit dem `Start-AzureAutomationRunbook`-Cmdlet. Das folgende Beispiel verwendet Windows PowerShell, um ein Runbook namens **Test-Runbook** in einer Hybrid Runbook Worker-Gruppe namens MyHybridGroup zu starten.

```azurepowershell-interactive
Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"
```

> [!NOTE]
> Der Parameter `RunOn` wurde in Version 0.9.1 von Microsoft Azure PowerShell zu `Start-AzureAutomationRunbook` hinzugefügt. Sie sollten die [neueste Version herunterladen](https://azure.microsoft.com/downloads/) , wenn auf Ihrem System eine Vorgängerversion installiert ist. Installieren Sie diese Version nur auf der Arbeitsstation, auf der Sie das Runbook über PowerShell starten. Eine Installation auf dem Hybrid Runbook Worker-Computer ist nur erforderlich, wenn Sie Runbooks auf diesem Computer starten möchten.

## <a name="working-with-signed-runbooks-on-a-windows-hybrid-runbook-worker"></a>Arbeiten mit signierten Runbooks auf einem Windows Hybrid Runbook Worker

Sie können eine Windows Hybrid Runbook Worker so konfigurieren, dass nur signierte Runbooks ausgeführt werden.

> [!IMPORTANT]
> Sobald Sie einen Hybrid Runbook Worker dafür konfiguriert haben, nur signierte Runbooks auszuführen, schlägt die Ausführung von Runbooks, die nicht signiert sind, auf dem Worker fehl.

### <a name="create-signing-certificate"></a>Auswählen des Signaturzertifikats

In folgendem Beispiel wird ein selbstsigniertes Zertifikat erstellt, das für das Signieren von Runbooks verwendet werden kann. Mit diesem Code wird das Zertifikat erstellt und exportiert, damit der Runbook Worker es später importieren kann. Der Fingerabdruck wird auch für die spätere Verwendung beim Verweisen auf das Zertifikat zurückgegeben.

```powershell
# Create a self-signed certificate that can be used for code signing
$SigningCert = New-SelfSignedCertificate -CertStoreLocation cert:\LocalMachine\my `
                                        -Subject "CN=contoso.com" `
                                        -KeyAlgorithm RSA `
                                        -KeyLength 2048 `
                                        -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
                                        -KeyExportPolicy Exportable `
                                        -KeyUsage DigitalSignature `
                                        -Type CodeSigningCert


# Export the certificate so that it can be imported to the hybrid workers
Export-Certificate -Cert $SigningCert -FilePath .\hybridworkersigningcertificate.cer

# Import the certificate into the trusted root store so the certificate chain can be validated
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\Root

# Retrieve the thumbprint for later use
$SigningCert.Thumbprint
```

### <a name="import-certificate-and-configure-workers-for-signature-validation"></a>Importieren von Zertifikaten und Konfigurieren von Workern für die Signaturprüfung

Kopieren Sie das erstellte Zertifikat in jeden Hybrid Runbook Worker in einer Gruppe. Führen Sie das folgende Skript aus, um das Zertifikat zu importieren und die Worker so zu konfigurieren, dass die Signaturüberprüfung auf Runbooks verwendet wird.

```powershell
# Install the certificate into a location that will be used for validation.
New-Item -Path Cert:\LocalMachine\AutomationHybridStore
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\AutomationHybridStore

# Import the certificate into the trusted root store so the certificate chain can be validated
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\Root

# Configure the hybrid worker to use signature validation on runbooks.
Set-HybridRunbookWorkerSignatureValidation -Enable $true -TrustedCertStoreLocation "Cert:\LocalMachine\AutomationHybridStore"
```

### <a name="sign-your-runbooks-using-the-certificate"></a>Signieren von Runbooks mithilfe des Zertifikats

Da die Hybrid Runbook Worker nur für die Verwendung signierter Runbooks konfiguriert sind, müssen Sie Runbooks signieren, die auf dem Hybrid Runbook Worker verwendet werden sollen. Verwenden Sie den folgenden PowerShell-Beispielcode, um diese Runbooks zu signieren.

```powershell
$SigningCert = ( Get-ChildItem -Path cert:\LocalMachine\My\<CertificateThumbprint>)
Set-AuthenticodeSignature .\TestRunbook.ps1 -Certificate $SigningCert
```

Nachdem ein Runbook signiert wurde, muss es in Ihr Automation-Konto importiert und mithilfe des Signaturblocks veröffentlicht werden. Weitere Informationen zum Importieren von Runbooks finden Sie unter [Importieren eines Runbooks aus einer Datei in Azure Automation](manage-runbooks.md#import-a-runbook).

## <a name="working-with-signed-runbooks-on-a-linux-hybrid-runbook-worker"></a>Arbeiten mit signierten Runbooks auf einem Linux Hybrid Runbook Worker

Um mit signierten Runbooks arbeiten zu können, muss ein Linux Hybrid Runbook Worker auf dem lokalen Computer über die [GPG](https://gnupg.org/index.html)-Datei verfügen.

> [!IMPORTANT]
> Sobald Sie einen Hybrid Runbook Worker dafür konfiguriert haben, nur signierte Runbooks auszuführen, schlägt die Ausführung von Runbooks, die nicht signiert sind, auf dem Worker fehl.

### <a name="create-a-gpg-keyring-and-keypair"></a>Erstellen eines GPG-Schlüsselbunds und -Schlüsselpaars

Verwenden Sie das Hybrid Runbook Worker-Konto **nxautomation**, um den GPG-Schlüsselbund und das Schlüsselpaar zu erstellen.

1. Verwenden Sie die Anwendung sudo, um sich mit dem Konto **nxautomation** anzumelden.

    ```bash
    sudo su – nxautomation
    ```

2. Sobald Sie **nxautomation** verwenden, generieren Sie das GPG-Schlüsselpaar. GPG führt Sie durch die Schritte. Sie müssen Name, E-Mail-Adresse, Ablaufzeit und Passphrase angeben. Warten Sie anschließend, bis die Entropie auf dem Computer ausreicht, um den Schlüssel zu generieren.

    ```bash
    sudo gpg --generate-key
    ```

3. Da das GPG-Verzeichnis mit sudo generiert wurde, müssen Sie seinen Besitzer mit dem folgenden Befehl in **nxautomation** ändern.

    ```bash
    sudo chown -R nxautomation ~/.gnupg
    ```

### <a name="make-the-keyring-available-to-the-hybrid-runbook-worker"></a>Bereitstellen des Schlüsselbunds für den Hybrid Runbook Worker

Nachdem der Schlüsselbund erstellt wurde, stellen Sie ihn dem Hybrid Runbook Worker zur Verfügung. Ändern Sie die Einstellungsdatei **/var/opt/microsoft/omsagent/state/automationworker/diy/worker.conf** so, dass sie unter dem Dateiabschnitt `[worker-optional]` den folgenden Beispielcode enthält.

```bash
gpg_public_keyring_path = /var/opt/microsoft/omsagent/run/.gnupg/pubring.kbx
```

### <a name="verify-that-signature-validation-is-on"></a>Überprüfen, ob die Signaturüberprüfung aktiviert ist

Wenn die Signaturprüfung auf dem Computer deaktiviert wurde, müssen Sie sie durch Ausführen des folgenden sudo-Befehls aktivieren. Ersetzen Sie `<LogAnalyticsworkspaceId>` durch die ID Ihres Arbeitsbereichs.

```bash
sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --true <LogAnalyticsworkspaceId>
```

### <a name="sign-a-runbook"></a>Signieren eines Runbooks

Nachdem Sie die Signaturprüfung konfiguriert haben, signieren Sie ein Runbook mit dem folgenden GPG-Befehl.

```bash
gpg –-clear-sign <runbook name>
```

Das signierte Runbook heißt `<runbook name>.asc`.

Sie können nun das signierte Runbook in Azure Automation hochladen und es wie ein herkömmliches Runbook ausführen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den Methoden zum Starten eines Runbooks finden Sie unter [Starten eines Runbooks in Azure Automation](automation-starting-a-runbook.md).
* Informationen über die Verwendung des Text-Editors für das Arbeiten mit PowerShell-Runbooks in Azure Automation finden Sie unter [Bearbeiten eines Runbooks in Azure Automation](automation-edit-textual-runbook.md).
* Wenn Ihre Runbooks nicht erfolgreich ausgeführt werden, lesen Sie den Problembehandlungsleitfaden zu [Fehlern bei der Ausführung von Runbooks](troubleshoot/hybrid-runbook-worker.md#runbook-execution-fails).
* Weitere Informationen zu PowerShell, einschließlich Sprachreferenz und Lernmodulen, finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/scripting/overview).
