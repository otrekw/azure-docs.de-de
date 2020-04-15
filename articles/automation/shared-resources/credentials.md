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
ms.openlocfilehash: c8b63a2676690004d23094b490fea0ef150ab9cb
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2020
ms.locfileid: "80546394"
---
# <a name="credential-assets-in-azure-automation"></a>Anmeldeinformationsobjekte in Azure Automation

Ein Automation-Anmeldeinformationsobjekt enthält ein Objekt, das Sicherheitsanmeldeinformationen wie beispielsweise einen Benutzernamen und ein Kennwort umfasst. Runbooks und DSC-Konfigurationen verwenden Cmdlets, die ein [PSCredential-](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?view=pscore-6.2.0)-Objekt für die Authentifizierung akzeptieren. Alternativ können sie den Benutzernamen und das Kennwort des `PSCredential`-Objekts extrahieren, um diese einer Anwendung oder einem Dienst bereitzustellen, die eine Authentifizierung erfordern. 

Azure Automation speichert die Eigenschaften von Anmeldeinformationen sicher. Der Zugriff auf die Eigenschaften mithilfe eines Runbooks oder einer DSC-Konfiguration verwendet die [Get-AutomationPSCredential](#activities-used-to-access-credentials)-Aktivität.

> [!NOTE]
> Zu den sicheren Objekten in Azure Automation gehören Anmeldeinformationen, Zertifikate, Verbindungen und verschlüsselte Variablen. Diese Objekte werden mithilfe eines eindeutigen Schlüssels verschlüsselt und in Azure Automation gespeichert, der für jedes Automation-Konto generiert wird. Dieser Schlüssel wird in Key Vault gespeichert. Vor dem Speichern eines sicheren Objekts wird der Schlüssel aus Key Vault geladen und dann zum Verschlüsseln des Objekts verwendet.

[!INCLUDE [gdpr-dsr-and-stp-note.md](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="azure-powershell-az-cmdlets-used-for-credential-assets"></a>Für Anmeldeinformationsobjekte verwendete Az-Cmdlets der Azure PowerShell

Als Bestandteil des Az-Moduls der Azure PowerShell werden die Cmdlets in der folgenden Tabelle zum Erstellen und Verwalten von Automation-Anmeldeinformationsobjekten mit Windows PowerShell verwendet. Sie gehören zum Lieferumfang des [Az.Automation-Moduls](/powershell/azure/new-azureps-module-az?view=azps-1.1.0), das zur Verwendung in Automation-Runbooks und DSC-Konfigurationen verfügbar ist. Siehe [Unterstützung für Az-Module in Azure Automation](https://docs.microsoft.com/azure/automation/az-modules).

| Cmdlet | BESCHREIBUNG |
|:--- |:--- |
| [Get-AzAutomationCredential](/powershell/module/az.automation/get-azautomationcredential?view=azps-3.3.0) |Ruft Informationen zu einem Anmeldeinformationsobjekt ab. Dieses Cmdlet gibt kein `PSCredential`-Objekt zurück.  |
| [New-AzAutomationCredential](/powershell/module/az.automation/new-azautomationcredential?view=azps-3.3.0) |Erstellt ein neues Anmeldeinformationsobjekt. |
| [Remove-AzAutomationCredential](/powershell/module/az.automation/remove-azautomationcredential?view=azps-3.3.0) |Entfernt ein Anmeldeinformationsobjekt. |
| [Set-AzAutomationCredential](/powershell/module/az.automation/set-azautomationcredential?view=azps-3.3.0) |Legt die Eigenschaften für ein vorhandenes Anmeldeinformationsobjekt fest. |

## <a name="activities-used-to-access-credentials"></a>Für den Zugriff auf Anmeldeinformationen verwendete Aktivitäten

Die Aktivitäten in der folgenden Tabelle werden für den Zugriff auf Anmeldeinformationen in Runbooks und DSC-Konfigurationen verwendet.

| Aktivität | BESCHREIBUNG |
|:--- |:--- |
| `Get-AutomationPSCredential` |Ruft Anmeldeinformationen zur Verwendung in einem Runbook oder einer DSC-Konfiguration ab. Die Anmeldeinformationen haben die Form eines `PSCredential`-Objekts. |
| [Get-Credential](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-credential?view=powershell-7) |Ruft Anmeldeinformationen mit einer Abfrage von Benutzername und Kennwort ab. |
| [New-AzureAutomationCredential](https://docs.microsoft.com/powershell/module/servicemanagement/azure/new-azureautomationcredential?view=azuresmps-4.0.0) | Erstellt ein Anmeldeinformationsobjekt. |

Für die lokale Entwicklung mit dem Azure Automation-Erstellungstoolkit ist das Cmdlet `Get-AutomationPSCredential` Bestandteil der Assembly [AzureAutomationAuthoringToolkit](https://www.powershellgallery.com/packages/AzureAutomationAuthoringToolkit/0.2.3.9). Für Azure, das mit dem Automation-Kontext arbeitet, befindet sich das Cmdlet in `Orchestrator.AssetManagement.Cmdlets`. Siehe [Verwalten von Modulen in Azure Automation](modules.md).

Zum Abrufen von `PSCredential`-Objekten in Ihrem Code können Sie das [Microsoft Azure Automation-ISE-Add-On für die PowerShell-ISE](https://github.com/azureautomation/azure-automation-ise-addon) installieren.

```azurepowershell
Install-Module AzureAutomationAuthoringToolkit -Scope CurrentUser -Force
```

Ihr Skript kann bei Bedarf auch das erforderliche Modul importieren, wie im folgenden Beispiel gezeigt: 

```azurepowershell
Import-Module Orchestrator.AssetManagement.Cmdlets -ErrorAction SilentlyContinue
```

> [!NOTE]
> Vermeiden Sie die Verwendung von Variablen im Parameter `Name` von `Get-AutomationPSCredential`. Durch deren Verwendung kann die Ermittlung von Abhängigkeiten zwischen Runbooks oder DSC-Konfigurationen und Anmeldeinformationsobjekten zur Entwurfszeit erschwert werden.

## <a name="python2-functions-that-access-credentials"></a>Python2-Funktionen, die auf Anmeldeinformationen zugreifen

Mit der Funktion in der folgenden Tabelle wird auf Anmeldeinformationen in einem Python2-Runbook zugegriffen.

| Funktion | BESCHREIBUNG |
|:---|:---|
| `automationassets.get_automation_credential` | Ruft Informationen zu einem Anmeldeinformationsobjekt ab. |

> [!NOTE]
> Importieren Sie das Modul `automationassets` oben im Python-Runbook, um auf die Ressourcenfunktionen zugreifen zu können.

## <a name="creating-a-new-credential-asset"></a>Erstellen eines neuen Anmeldeinformationsobjekts

Sie können ein neues Anmeldeinformationsobjekt im Azure-Portal oder mit Windows PowerShell erstellen.

### <a name="create-a-new-credential-asset-with-the-azure-portal"></a>Erstellen eines neuen Anmeldeinformationsobjekts über das Azure-Portal

1. Wählen Sie in Ihrem Automation-Konto unter **Freigegebene Ressourcen** die Option **Anmeldeinformationen** aus.
1. Wählen Sie **Anmeldeinformationen hinzufügen** aus.
2. Geben Sie im Bereich „Neue Anmeldeinformationen“ einen geeigneten Anmeldeinformationsnamen ein, der Ihre Benennungsstandards einhält. 
3. Geben Sie Ihre Zugriffs-ID in das Feld **Benutzername** ein. 
4. Geben Sie für beide Kennwortfelder Ihren geheimen Zugriffsschlüssel ein.

    ![Erstellen neuer Anmeldeinformationen](../media/credentials/credential-create.png)

5. Wenn das Kontrollkästchen für mehrstufige Authentifizierung aktiviert ist, deaktivieren Sie es. 
6. Klicken Sie auf **Erstellen**, um das neue Anmeldeinformationsobjekt zu speichern.

> [!NOTE]
> Azure Automation unterstützt keine Benutzerkonten, die mehrstufige Authentifizierung verwenden.

### <a name="create-a-new-credential-asset-with-windows-powershell"></a>Erstellen eines neuen Anmeldeinformationsobjekts mithilfe von Windows PowerShell

Das folgenden Beispiel zeigt, wie Sie ein neues Automation-Anmeldeinformationsobjekt erstellen. Es wird zunächst ein `PSCredential`-Objekt mit Name und Kennwort erstellt, anschließend werden diese Informationen zum Erstellen des Anmeldeinformationsobjekts verwendet. Stattdessen können Sie das Cmdlet `Get-Credential` verwenden, um den Benutzer zur Eingabe eines Namens und Kennworts aufzufordern.

```powershell
$user = "MyDomain\MyUser"
$pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
$cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred
```

## <a name="using-a-powershell-credential"></a>Verwenden von PowerShell-Anmeldeinformationen

Ein Runbook oder eine DSC-Konfiguration ruft ein Anmeldeinformationsobjekt mit der Aktivität `Get-AutomationPSCredential` ab. Diese Aktivität ruft ein `PSCredential`-Objekt ab, das Sie mit einer Aktivität oder einem Cmdlet verwenden können, die oder das Anmeldeinformationen erfordert. Sie können auch die Eigenschaften des Anmeldeinformationsobjekts abrufen, um diese einzeln zu verwenden. Das Objekt verfügt über Eigenschaften für den Benutzernamen und das sichere Kennwort. Alternativ können Sie die Methode [GetNetworkCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential.getnetworkcredential?view=pscore-6.2.0) verwenden, um ein [NetworkCredential](/dotnet/api/system.net.networkcredential)-Objekt abzurufen, das eine unsichere Version des Kennworts darstellt.

> [!NOTE]
> `Get-AzAutomationCredential` ruft kein `PSCredential`-Objekt ab, das für die Authentifizierung verwendet werden kann. Es bietet lediglich Informationen zu den Anmeldeinformationen. Wenn Sie ein Anmeldeinformationsobjekt in einem Runbook verwenden müssen, müssen Sie dieses mithilfe von `Get-AutomationPSCredential` als `PSCredential`-Objekt abrufen.

### <a name="textual-runbook-example"></a>Beispiel für ein Textrunbook

Das folgende Beispiel zeigt, wie Sie ein PowerShell-Anmeldeinformationsobjekt in einem Runbook verwenden. Es ruft das Anmeldeinformationsobjekt ab und weist seinen Benutzernamen und das Kennwort Variablen zu.


```azurepowershell
$myCredential = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCredential.UserName
$securePassword = $myCredential.Password
$password = $myCredential.GetNetworkCredential().Password
```

Sie können Anmeldeinformationen auch zum Authentifizieren mit [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?view=azps-3.3.0) in Azure verwenden. In den meisten Fällen sollten Sie ein [ausführendes Konto](../manage-runas-account.md) verwenden und die Verbindung mit [Get-AzAutomationConnection](../automation-connections.md) abrufen.


```azurepowershell
$myCred = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCred.UserName
$securePassword = $myCred.Password
$password = $myCred.GetNetworkCredential().Password

$myPsCred = New-Object System.Management.Automation.PSCredential ($userName,$password)

Connect-AzAccount -Credential $myPsCred
```

### <a name="graphical-runbook-example"></a>Beispiel für ein grafisches Runbook

Sie können einem grafischen Runbook eine `Get-AutomationPSCredential`-Aktivität hinzufügen, indem Sie im Bibliotheksbereich des grafischen Editors mit der rechten Maustaste auf die Anmeldeinformationen klicken und **Zum Zeichenbereich hinzufügen** auswählen.

![Anmeldeinformationen zum Zeichenbereich hinzufügen](../media/credentials/credential-add-canvas.png)

Die folgende Abbildung zeigt ein Beispiel für die Verwendung eines Anmeldeinformationsobjekts in einem grafischen Runbook. In diesem Fall stellt das Anmeldeinformationsobjekt Authentifizierungsinformationen für ein Runbook in Azure-Ressourcen bereit, wie unter [Verwenden von Azure AD in Azure Automation für die Authentifizierung bei Azure](../automation-use-azure-ad.md) beschrieben. Die erste Aktivität ruft die Anmeldeinformationen ab, die Zugriff auf das Azure-Abonnement haben. Die Kontoverbindungsaktivität verwendet diese Anmeldeinformationen anschließend, um eine Authentifizierung für alle nachfolgenden Aktivitäten bereitzustellen. Es wird eine [Pipelineverknüpfung](../automation-graphical-authoring-intro.md#links-and-workflow) verwendet, da `Get-AutomationPSCredential` ein einzelnes Objekt erwartet.  

![Anmeldeinformationen zum Zeichenbereich hinzufügen](../media/credentials/get-credential.png)

## <a name="using-credentials-in-a-dsc-configuration"></a>Verwenden von Anmeldeinformationen in einer DSC-Konfiguration

DSC-Konfigurationen in Azure Automation können mithilfe von `Get-AutomationPSCredential` mit Anmeldeinformationsobjekten arbeiten, können aber gleichzeitig auch Anmeldeinformationsobjekte über Parameter übergeben. Weitere Informationen finden Sie unter [Kompilieren von Konfigurationen in Azure Automation DSC](../automation-dsc-compile.md#credential-assets).

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

* Weitere Informationen zu Links bei der grafischen Inhaltserstellung finden Sie unter [Links bei der grafischen Erstellung](../automation-graphical-authoring-intro.md#links-and-workflow).
* Informationen zu den verschiedenen Authentifizierungsmethoden für Automation finden Sie unter [Azure Automation-Sicherheit](../automation-security-overview.md).
* Informationen zu den ersten Schritten mit grafischen Runbooks finden Sie unter [Mein erstes grafisches Runbook](../automation-first-runbook-graphical.md).
* Die ersten Schritte mit PowerShell-Workflow-Runbooks sind unter [Mein erstes PowerShell-Workflow-Runbook](../automation-first-runbook-textual.md)beschrieben.
* Informationen zu den ersten Schritten mit Python2-Runbooks finden Sie unter [My first Python2 runbook](../automation-first-runbook-textual-python2.md) (Mein erstes Python2-Runbook). 
