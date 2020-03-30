---
title: Anmeldeinformationsobjekte in Azure Automation
description: Anmeldeinformationsobjekte in Azure Automation enthalten Sicherheitsanmeldeinformationen, die zur Authentifizierung von Ressourcen verwendet werden können, auf die über das Runbook oder die DSC-Konfiguration zugegriffen wird. Dieser Artikel beschreibt, wie Sie Anmeldeinformationsobjekte erstellen und in einem Runbook oder einer DSC-Konfiguration verwenden.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 01/31/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 767c1fddbc3d1f46d4341a70c990c2b57ad40e54
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79227538"
---
# <a name="credential-assets-in-azure-automation"></a>Anmeldeinformationsobjekte in Azure Automation

Ein Automation-Anmeldeinformationsobjekt enthält ein Objekt, das Sicherheitsanmeldeinformationen wie beispielsweise einen Benutzernamen und ein Kennwort umfasst. Runbooks und DSC-Konfigurationen können Cmdlets verwenden, die zur Authentifizierung ein PSCredential-Objekt akzeptieren, oder sie extrahieren den Benutzernamen und das Kennwort aus dem PSCredential-Objekt, um diese Informationen für eine Anwendung oder einen Dienst bereitzustellen, die bzw. der eine Authentifizierung erfordert. Die Eigenschaften für ein Anmeldeinformationsobjekt werden sicher in Azure Automation gespeichert und können im Runbook oder in der DSC-Konfiguration über die Aktivität [Get-AutomationPSCredential](#activities) abgerufen werden.

[!INCLUDE [gdpr-dsr-and-stp-note.md](../../../includes/gdpr-dsr-and-stp-note.md)]

> [!NOTE]
> Zu den sicheren Objekten in Azure Automation gehören Anmeldeinformationen, Zertifikate, Verbindungen und verschlüsselte Variablen. Diese Objekte werden mithilfe eines eindeutigen Schlüssels verschlüsselt und in Azure Automation gespeichert, der für jedes Automation-Konto generiert wird. Dieser Schlüssel wird in Key Vault gespeichert. Vor dem Speichern eines sicheren Objekts wird der Schlüssel aus Key Vault geladen und dann zum Verschlüsseln des Objekts verwendet.

## <a name="azure-powershell-az-cmdlets"></a>Az-Cmdlets der Azure PowerShell

Im Az-Modul der Azure PowerShell werden die Cmdlets in der folgenden Tabelle zum Erstellen und Verwalten von Automation-Anmeldeinformationsobjekten mit Windows PowerShell verwendet. Sie gehören zum Funktionsumfang des [AzureAz.Automation-Moduls](/powershell/azure/new-azureps-module-az?view=azps-1.1.0), das zur Verwendung in Automation-Runbooks und DSC-Konfigurationen verfügbar ist.

| Cmdlets | BESCHREIBUNG |
|:--- |:--- |
| [Get-AzAutomationCredential](/powershell/module/az.automation/get-azautomationcredential?view=azps-3.3.0) |Ruft Informationen zu einem Anmeldeinformationsobjekt ab. Dies gibt kein PSCredential-Objekt zurück.  |
| [New-AzAutomationCredential](/powershell/module/az.automation/new-azautomationcredential?view=azps-3.3.0) |Erstellt ein neues Anmeldeinformationsobjekt. |
| [Remove-AzAutomationCredential](/powershell/module/az.automation/remove-azautomationcredential?view=azps-3.3.0) |Entfernt ein Anmeldeinformationsobjekt. |
| [Set-AzAutomationCredential](/powershell/module/az.automation/set-azautomationcredential?view=azps-3.3.0) |Legt die Eigenschaften für ein vorhandenes Anmeldeinformationsobjekt fest. |

## <a name="activities"></a>activities

Die Aktivitäten in der folgenden Tabelle werden für den Zugriff auf Anmeldeinformationen in einem Runbook und DSC-Konfigurationen verwendet.

| activities | BESCHREIBUNG |
|:--- |:--- |
| Get-AutomationPSCredential |Ruft Anmeldeinformationen zur Verwendung in einem Runbook oder einer DSC-Konfiguration ab. Gibt ein [System.Management.Automation.PSCredential](/dotnet/api/system.management.automation.pscredential) -Objekt zurück. |

> [!NOTE]
> Vermeiden Sie die Verwendung von Variablen im Parameter „–Name“ von „Get-AutomationPSCredential“ in einem Runbook, da dies die Ermittlung von Abhängigkeiten zwischen Runbooks oder DSC-Konfigurationen und Verbindungsobjekten zur Entwurfszeit erschweren kann.

## <a name="python2-functions"></a>Python2-Funktionen

Mit der Funktion in der folgenden Tabelle wird auf Anmeldeinformationen in einem Python2-Runbook zugegriffen.

| Funktion | BESCHREIBUNG |
|:---|:---|
| automationassets.get_automation_credential | Ruft Informationen zu einem Anmeldeinformationsobjekt ab. |

> [!NOTE]
> Sie müssen das Modul „automationassets“ oben im Python-Runbook importieren, um auf die Assetfunktionen zugreifen zu können.

## <a name="creating-a-new-credential-asset"></a>Erstellen eines neuen Anmeldeinformationsobjekts

### <a name="to-create-a-new-credential-asset-with-the-azure-portal"></a>So erstellen Sie ein neues Anmeldeinformationsobjekt über das Azure-Portal

1. Wählen Sie in Ihrem Automation-Konto unter **Freigegebene Ressourcen** die Option **Anmeldeinformationen** aus.
1. Wählen Sie **Anmeldeinformationen hinzufügen** aus.
1. Geben Sie die erforderlichen Daten ein, und wählen Sie **Erstellen** aus, um die neuen Anmeldeinformationen zu speichern.

> [!NOTE]
> Benutzerkonten, die mehrstufige Authentifizierung verwenden, werden in Azure Automation nicht unterstützt.

### <a name="to-create-a-new-credential-asset-with-windows-powershell"></a>So erstellen Sie ein neues Anmeldeinformationsobjekt mithilfe von Windows PowerShell

Die folgenden Beispielbefehle zeigen, wie Sie ein neues Automation-Anmeldeinformationsobjekt erstellen. Es wird zunächst ein PSCredential-Objekt mit Name und Kennwort erstellt, anschließend werden diese Informationen zum Erstellen des Anmeldeinformationsobjekts verwendet. Alternativ können Sie das Cmdlet **Get-Credential** verwenden, um zur Eingabe von Name und Kennwort aufgefordert zu werden.

```powershell
$user = "MyDomain\MyUser"
$pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
$cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred
```

## <a name="using-a-powershell-credential"></a>Verwenden von PowerShell-Anmeldeinformationen

In einem Runbook oder einer DSC-Konfiguration rufen Sie ein Anmeldeinformationsobjekt mit der Aktivität **Get-AutomationPSCredential** ab. Diese Aktivität gibt ein [PSCredential-Objekt](/dotnet/api/system.management.automation.pscredential) zurück, das Sie mit einer Aktivität oder einem Cmdlet verwenden können, die oder das einen PSCredential-Parameter erfordert. Sie können auch die Eigenschaften des Anmeldeinformationsobjekts abrufen, um diese einzeln zu verwenden. Das Objekt besitzt eine Eigenschaft für den Benutzernamen und das sichere Kennwort. Oder Sie verwenden die Methode **GetNetworkCredential**, um ein [NetworkCredential](/dotnet/api/system.net.networkcredential)-Objekt zurückzugeben, das eine unsichere Version des Kennworts bereitstellt.

> [!NOTE]
> **Get-AzAutomationCredential** gibt kein **PSCredential** zurück, das für die Authentifizierung verwendet werden kann. Es bietet lediglich Informationen zu den Anmeldeinformationen. Wenn Sie Anmeldeinformationen in einem Runbook verwenden müssen, müssen Sie **Get-AutomationPSCredential** verwenden, um das **PSCredential**-Objekt abzurufen.

### <a name="textual-runbook-sample"></a>Beispiel für ein Textrunbook

Die folgenden Beispielbefehle zeigen, wie Sie ein PowerShell-Anmeldeinformationsobjekt in einem Runbook verwenden. In diesem Beispiel wird das Anmeldeinformationsobjekt abgerufen, und der darin enthaltene Benutzername sowie das Kennwort werden Variablen zugewiesen.

```azurepowershell
$myCredential = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCredential.UserName
$securePassword = $myCredential.Password
$password = $myCredential.GetNetworkCredential().Password
```

Sie können Anmeldeinformationen auch zum Authentifizieren mit [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?view=azps-3.3.0) in Azure verwenden. In den meisten Fällen sollten Sie ein [ausführendes Konto](../manage-runas-account.md) verwenden und mit [Get-AzAutomationConnection](../automation-connections.md) abrufen.

```azurepowershell
$myCred = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCred.UserName
$securePassword = $myCred.Password
$password = $myCred.GetNetworkCredential().Password

$myPsCred = New-Object System.Management.Automation.PSCredential ($userName,$password)

Connect-AzureRmAccount -Credential $myPsCred
```

### <a name="graphical-runbook-sample"></a>Beispiel für ein grafisches Runbook

Sie können einem grafischen Runbook eine **Get-AutomationPSCredential**-Aktivität hinzufügen, indem Sie im Bibliotheksbereich des grafischen Editors mit der rechten Maustaste auf die Anmeldeinformationen klicken und **Zum Zeichenbereich hinzufügen** auswählen.

![Anmeldeinformationen zum Zeichenbereich hinzufügen](../media/credentials/credential-add-canvas.png)

Die folgende Abbildung zeigt ein Beispiel für die Verwendung eines Anmeldeinformationsobjekts in einem grafischen Runbook. In diesem Fall wird das Objekt zur Bereitstellung von Authentifizierungsinformationen für ein Runbook in Azure-Ressourcen verwendet, wie unter [Authentifizieren von Runbooks mit Azure AD-Benutzerkonto](../automation-create-aduser-account.md) beschrieben. Die erste Aktivität ruft die Anmeldeinformationen ab, die Zugriff auf das Azure-Abonnement haben. Die Aktivität **Connect-AzureRmAccount** verwendet diese Anmeldeinformationen anschließend, um eine Authentifizierung für alle nachfolgenden Aktivitäten bereitzustellen. Es wird eine [Pipelineverknüpfung](../automation-graphical-authoring-intro.md#links-and-workflow) verwendet, da **Get-AutomationPSCredential** ein einzelnes Objekt erwartet.  

![Anmeldeinformationen zum Zeichenbereich hinzufügen](../media/credentials/get-credential.png)

## <a name="using-a-powershell-credential-in-dsc"></a>Verwenden von PowerShell-Anmeldeinformationen in DSC

DSC-Konfigurationen in Azure Automation können mithilfe von **Get-AutomationPSCredential** auf Anmeldeinformationen verweisen. Diese Informationen können aber bei Bedarf auch über Parameter übergeben werden. Weitere Informationen finden Sie unter [Kompilieren von Konfigurationen in Azure Automation DSC](../automation-dsc-compile.md#credential-assets).

## <a name="using-credentials-in-python2"></a>Verwenden von Anmeldeinformationen in Python2

Im folgenden Beispiel wird der Zugriff auf Anmeldeinformationen in Python2-Runbooks veranschaulicht.

```python
import automationassets
from automationassets import AutomationAssetNotFound

# get a credential
cred = automationassets.get_automation_credential("credtest")
print cred["username"]
print cred["password"]
```

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Links bei der grafischen Inhaltserstellung finden Sie unter [Links bei der grafischen Erstellung](../automation-graphical-authoring-intro.md#links-and-workflow)
* Informationen zu den verschiedenen Authentifizierungsmethoden bei Automation finden Sie unter [Azure Automation-Sicherheit](../automation-security-overview.md)
* Informationen zu den ersten Schritten mit grafischen Runbooks finden Sie unter [Mein erstes grafisches Runbook](../automation-first-runbook-graphical.md)
* Die ersten Schritte mit PowerShell-Workflow-Runbooks sind unter [Mein erstes PowerShell-Workflow-Runbook](../automation-first-runbook-textual.md)
* Informationen zu den ersten Schritten mit Python2-Runbooks finden Sie unter [My first Python2 runbook](../automation-first-runbook-textual-python2.md) (Mein erstes Python2-Runbook). 
