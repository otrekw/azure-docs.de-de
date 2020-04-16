---
title: Verwalten von Modulen in Azure Automation
description: In diesem Artikel erfahren Sie, wie Sie Module in Azure Automation verwalten.
services: automation
ms.service: automation
author: mgoedtel
ms.author: magoedte
ms.date: 01/31/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 605d1bc72406a9aeecc9273f9bd2d7fd2b30ab11
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2020
ms.locfileid: "80618689"
---
# <a name="manage-modules-in-azure-automation"></a>Verwalten von Modulen in Azure Automation

Sie können PowerShell-Module in Azure Automation importieren, damit ihre Cmdlets in Runbooks und ihre DSC-Ressourcen in DSC-Konfigurationen verfügbar sind. Azure Automation speichert diese Module im Hintergrund. Automation lädt sie während der Ausführung der Runbook- und DSC-Kompilierungsaufträge in die Azure Automation-Sandboxes, wo Runbooks ausgeführt und DSC-Konfigurationen kompiliert werden. DSC-Ressourcen in Modulen werden ebenfalls automatisch auf dem Automation DSC-Pullserver gespeichert. Computer können diese beim Anwenden von DSC-Konfigurationen per Pull abrufen.

Bei in Azure Automation verwendeten Modulen kann es sich um benutzerdefinierte, von Ihnen erstellte Module, Module aus dem PowerShell-Katalog oder die AzureRM- und Az-Module für Azure handeln. Bei der Erstellung eines Automation-Kontos werden einige Module standardmäßig importiert.

## <a name="default-modules"></a>Standardmodule

In der folgenden Tabelle sind die Module aufgeführt, die beim Erstellen eines Automation-Kontos standardmäßig importiert werden. Automation kann neuere Versionen dieser Module importieren. Sie können jedoch die ursprüngliche Version nicht aus Ihrem Automation-Konto entfernen, selbst wenn Sie eine neuere Version löschen.

|Modulname|Version|
|---|---|
| AuditPolicyDsc | 1.1.0.0 |
| Azure | 1.0.3 |
| Azure.Storage | 1.0.3 |
| AzureRM.Automation | 1.0.3 |
| AzureRM.Compute | 1.2.1 |
| AzureRM.Profile | 1.0.3 |
| AzureRM.Resources | 1.0.3 |
| AzureRM.Sql | 1.0.3 |
| AzureRM.Storage | 1.0.3 |
| ComputerManagementDsc | 5.0.0.0 |
| GPRegistryPolicyParser | 0.2 |
| Microsoft.PowerShell.Core | 0 |
| Microsoft.PowerShell.Diagnostics |  |
| Microsoft.PowerShell.Management |  |
| Microsoft.PowerShell.Security |  |
| Microsoft.PowerShell.Utility |  |
| Microsoft.WSMan.Management |  |
| Orchestrator.AssetManagement.Cmdlets | 1 |
| PSDscResources | 2.9.0.0 |
| SecurityPolicyDsc | 2.1.0.0 |
| StateConfigCompositeResources | 1 |
| xDSCDomainjoin | 1.1 |
| xPowerShellExecutionPolicy | 1.1.0.0 |
| xRemoteDesktopAdmin | 1.1.0.0 |

## <a name="internal-cmdlets"></a>Interne Cmdlets

In der folgenden Tabelle finden Sie eine Liste der Cmdlets im internen `Orchestrator.AssetManagement.Cmdlets`-Modul, das in jedes Automation-Konto importiert wird. Diese Cmdlets sind in Ihren Runbooks und DSC-Konfigurationen verfügbar und ermöglichen Ihnen die Interaktion mit Ressourcen in Ihrem Automation-Konto. Zudem ermöglichen die internen Cmdlets das Abrufen von Geheimnissen aus verschlüsselten Variablen, Anmeldeinformationen und verschlüsselten Verbindungen. Die Azure PowerShell-Cmdlets können diese Geheimnisse nicht abrufen. Wenn Sie diese Cmdlets verwenden, müssen Sie keine implizite Verbindung mit Azure herstellen, wie z. B. „Als Konto ausführen“, um sich bei Azure zu authentifizieren.

>[!NOTE]
>Diese internen Cmdlets sind auf einem Windows Hybrid Runbook Worker verfügbar, jedoch nicht auf einem Linux Hybrid Runbook Worker. Verwenden Sie die entsprechenden Cmdlets [AzureRM.Automation](https://docs.microsoft.com/powershell/module/AzureRM.Automation/?view=azurermps-6.13.0) oder [Az-Module](../az-modules.md) für Runbooks, die direkt auf dem Computer oder mit Ressourcen in Ihrer Umgebung ausgeführt werden. 

|Name|BESCHREIBUNG|
|---|---|
|Get-AutomationCertificate|`Get-AutomationCertificate [-Name] <string> [<CommonParameters>]`|
|Get-AutomationConnection|`Get-AutomationConnection [-Name] <string> [-DoNotDecrypt] [<CommonParameters>]` |
|Get-AutomationPSCredential|`Get-AutomationPSCredential [-Name] <string> [<CommonParameters>]` |
|Get-AutomationVariable|`Get-AutomationVariable [-Name] <string> [-DoNotDecrypt] [<CommonParameters>]`|
|Set-AutomationVariable|`Set-AutomationVariable [-Name] <string> -Value <Object> [<CommonParameters>]` |
|Start-AutomationRunbook|`Start-AutomationRunbook [-Name] <string> [-Parameters <IDictionary>] [-RunOn <string>] [-JobId <guid>] [<CommonParameters>]`|
|Wait-AutomationJob|`Wait-AutomationJob -Id <guid[]> [-TimeoutInMinutes <int>] [-DelayInSeconds <int>] [-OutputJobsTransitionedToRunning] [<CommonParameters>]`|

## <a name="importing-modules"></a>Importieren von Modulen

Sie haben mehrere Möglichkeiten, ein Modul in Ihr Automation-Konto zu importieren. In den folgenden Abschnitte zeigen wir Ihnen verschiedene Möglichkeiten zum Importieren von Modulen.

> [!NOTE]
> Die maximale Pfadlänge für Dateien in einem Modul in Azure Automation beträgt 140 Zeichen. Automation kann keine Datei mit einer Pfadlänge über 140 Zeichen in die PowerShell-Sitzung mit `Import-Module` importieren.

### <a name="import-modules-in-azure-portal"></a>Importieren von Modulen im Azure-Portal

So importieren Sie Module im Azure-Portal:

1. Navigieren Sie zu Ihrem Automation-Konto.
2. Wählen Sie unter **Freigegebene Ressourcen** die Option **Module** aus.
3. Klicken Sie auf **Modul hinzufügen**. 
4. Wählen Sie die **ZIP**-Datei aus, die Ihr Modul enthält.
5. Klicken Sie auf **OK**, um den Importvorgang zu starten.

### <a name="import-modules-using-powershell"></a>Importieren von Modulen mithilfe von PowerShell

Sie können das Cmdlet [New-AzureRmAutomationModule](/powershell/module/azurerm.automation/new-azurermautomationmodule) zum Importieren von Modulen in Ihr Automation-Konto verwenden. Das Cmdlet benötigt eine URL für ein Modul-ZIP-Paket.

```azurepowershell-interactive
New-AzureRmAutomationModule -Name <ModuleName> -ContentLinkUri <ModuleUri> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName>
```

Sie können das gleiche Cmdlet verwenden, um ein Modul direkt aus dem PowerShell-Katalog zu importieren. Rufen Sie `ModuleName` und `ModuleVersion` aus dem [PowerShell-Katalog](https://www.powershellgallery.com) ab.

```azurepowershell-interactive
$moduleName = <ModuleName>
$moduleVersion = <ModuleVersion>
New-AzureRmAutomationModule -AutomationAccountName <AutomationAccountName> -ResourceGroupName <ResourceGroupName> -Name $moduleName -ContentLinkUri "https://www.powershellgallery.com/api/v2/package/$moduleName/$moduleVersion"
```
### <a name="import-modules-from-powershell-gallery"></a>Importieren von Modulen aus dem PowerShell-Katalog

Sie können Module im [PowerShell-Katalog](https://www.powershellgallery.com) entweder direkt aus dem Katalog oder aus Ihrem Automation-Konto importieren.

So importieren Sie ein Modul direkt aus dem PowerShell-Katalog:

1. Wechseln Sie zu https://www.powershellgallery.com, und suchen Sie nach dem zu importierenden Modul.
2. Klicken Sie auf der Registerkarte **Azure Automation** unter **Installation Options** (Installationsoptionen) auf **Deploy to Azure Automation** (In Azure Automation bereitstellen). Das Azure-Portal wird geöffnet. 
3. Wählen Sie auf der Seite „Importieren“ Ihr Automation-Konto aus, und klicken Sie auf **OK**.

![PowerShell-Katalog – Importmodul](../media/modules/powershell-gallery.png)

So importieren Sie ein Modul im PowerShell-Katalog direkt aus Ihrem Automation-Konto:

1. Wählen Sie unter **Freigegebene Ressourcen** die Option **Module** aus. 
2. Klicken Sie auf der Seite „Module“ auf **Katalog durchsuchen**, und durchsuchen Sie dann den Katalog nach einem Modul. 
3. Wählen Sie das zu importierende Modul aus, und klicken Sie auf **Importieren**. 
4. Klicken Sie auf der Seite „Importieren“ auf **OK**, um den Importvorgang zu starten.

![PowerShell-Katalog – Import aus Azure-Portal](../media/modules/gallery-azure-portal.png)

## <a name="deleting-modules"></a>Löschen von Modulen

Wenn Sie Probleme mit einem Modul haben oder ein Rollback auf eine frühere Version eines Moduls durchführen müssen, können Sie es aus Ihrem Automation-Konto löschen. Die ursprünglichen Versionen von [Standardmodulen](#default-modules), die beim Erstellen eines Automation-Kontos importiert werden, können nicht gelöscht werden. Wenn das zu löschende Modul eine neuere Version als die [Standardmodule](#default-modules) aufweist, wird ein Rollback auf die Version durchgeführt, die mit Ihrem Automation-Konto installiert wurde. Andernfalls werden alle Module, die Sie aus Ihrem Automation-Konto löschen, entfernt.

### <a name="delete-modules-in-azure-portal"></a>Löschen von Modulen im Azure-Portal

So entfernen Sie ein Modul im Azure-Portal:

1. Navigieren Sie zu Ihrem Automation-Konto, und wählen Sie unter **Freigegebene Ressourcen** die Option **Module** aus. 
2. Wählen Sie das Modul aus, das Sie entfernen möchten. 
3. Wählen Sie auf der Seite **Modul** den Befehl **Löschen** aus. Handelt es sich bei dem Modul um eines der [Standardmodule](#default-modules), wird ein Rollback auf die Version durchgeführt, die beim Erstellen des Automation-Kontos vorhanden war.

### <a name="delete-modules-using-powershell"></a>Löschen von Modulen mithilfe von PowerShell

Führen Sie zum Entfernen eines Moduls mithilfe von PowerShell den folgenden Befehl aus:

```azurepowershell-interactive
Remove-AzureRmAutomationModule -Name <moduleName> -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName>
```
## <a name="adding-a-connection-type-to-your-module"></a>Hinzufügen eines Verbindungstyps zu Ihrem Modul

Sie können einen benutzerdefinierten [Verbindungstyp](../automation-connections.md) für die Verwendung in Ihrem Automation-Konto festlegen, indem Sie Ihrem Modul eine optionale Metadatendatei hinzufügen. Mit dieser Datei legen Sie einen Azure Automation-Verbindungstyp fest, den Sie mit dem Cmdlets des Moduls in Ihrem Automation-Konto verwenden möchten. Dazu müssen Sie ein PowerShell-Modell erstellen. Siehe [Schreiben eines PowerShell-Skriptmoduls](/powershell/scripting/developer/module/how-to-write-a-powershell-script-module).

![Verwenden einer benutzerdefinierten Verbindung im Azure-Portal](../media/modules/connection-create-new.png)

Die Datei, die Verbindungstypeigenschaften angibt, heißt **&lt;Modulname&gt;-Automation.json** und befindet sich im Modulordner der komprimierten **ZIP**-Datei. Diese Datei enthält die Felder einer Verbindung, die zum Herstellen einer Verbindung mit dem entsprechenden System oder Dienst des Moduls erforderlich sind. Die Konfiguration ermöglicht das Erstellen eines Verbindungstyps in Azure Automation. Mit dieser Datei können Sie für den Verbindungstyp des Moduls die Feldnamen und -typen festlegen und angeben, ob die Felder verschlüsselt oder optional sein sollen. Das folgende Beispiel ist eine Vorlage im **JSON**-Dateiformat, die Eigenschaften für Benutzername und Kennwort definiert:

```json
{
   "ConnectionFields": [
   {
      "IsEncrypted":  false,
      "IsOptional":  true,
      "Name":  "Username",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  true,
      "IsOptional":  false,
      "Name":  "Password",
      "TypeName":  "System.String"
   }
   ],
   "ConnectionTypeName":  "MyModuleConnection",
   "IntegrationModuleName":  "MyModule"
}
```

## <a name="best-practices-for-authoring-modules"></a>Bewährte Methoden für das Erstellen von Modulen

Es wird empfohlen, dass Sie die Empfehlungen in diesem Abschnitt befolgen, wenn Sie ein PowerShell-Modul für die Verwendung in Azure Automation erstellen.

### <a name="version-folder"></a>Versionsordner

Schließen Sie KEINEN Versionsordner in das **ZIP**-Paket für Ihr Modul ein.  Dieses Problem ist für Runbooks weniger von Bedeutung, führt aber zu einem Problem mit dem State Configuration-Dienst. Azure Automation erstellt automatisch den Versionsordner, wenn das Modul an von DSC verwaltete Knoten verteilt wird. Wenn ein Versionsordner vorhanden ist, erhalten Sie am Ende zwei Instanzen. Hier ist ein Beispiel für eine Ordnerstruktur für ein DSC-Modul:

```powershell
myModule
  - DSCResources
    - myResourceFolder
      myResourceModule.psm1
      myResourceSchema.mof
  myModuleManifest.psd1
```

### <a name="help-information"></a>Hilfeinformationen

Fügen Sie eine Zusammenfassung, eine Beschreibung und einen Hilfe-URI für jedes Cmdlet in Ihrem Modul ein. In PowerShell können Sie mithilfe des Cmdlets `Get-Help` Hilfeinformationen für Cmdlets definieren. Im folgenden Beispiel sehen Sie, wie Sie eine Zusammenfassung und einen Hilfe-URI für eine Moduldatei vom Typ **.psm1** erstellen:

  ```powershell
  <#
       .SYNOPSIS
        Gets a Contoso User account
  #>
  function Get-ContosoUser {
  [CmdletBinding](DefaultParameterSetName='UseConnectionObject', `
  HelpUri='https://www.contoso.com/docs/information')]
  [OutputType([String])]
  param(
     [Parameter(ParameterSetName='UserAccount', Mandatory=true)]
     [ValidateNotNullOrEmpty()]
     [string]
     $UserName,

     [Parameter(ParameterSetName='UserAccount', Mandatory=true)]
     [ValidateNotNullOrEmpty()]
     [string]
     $Password,

     [Parameter(ParameterSetName='ConnectionObject', Mandatory=true)]
     [ValidateNotNullOrEmpty()]
     [Hashtable]
     $Connection
  )

  switch ($PSCmdlet.ParameterSetName) {
     "UserAccount" {
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $UserName, $Password
        Connect-Contoso -Credential $cred
     }
     "ConnectionObject" {
        Connect-Contoso -Connection $Connection
    }
  }
  }
  ```

  Bei Angabe dieser Informationen wird Hilfetext über das Cmdlet `Get-Help` in der PowerShell-Konsole angezeigt. Dieser Text wird auch im Azure-Portal angezeigt.

  ![Hilfe zu Integrationsmodulen](../media/modules/module-activity-description.png)

### <a name="connection-type"></a>Verbindungstyp

Wenn das Modul eine Verbindung mit einem externen Dienst herstellt, definieren Sie einen [Verbindungstyp](#adding-a-connection-type-to-your-module). Jedes Cmdlet im Modul muss ein Verbindungsobjekt (eine Instanz dieses Verbindungstyps) als Parameter akzeptieren. Benutzer ordnen Parameter des Verbindungsobjekts bei jedem Aufruf eines Cmdlets den entsprechenden Parametern des Cmdlets zu. Im obigen Runbook-Beispiel wird ein Verbindungsobjekt namens `ContosoConnection` verwendet, um auf Contoso-Ressourcen zuzugreifen und Daten vom externen Dienst zurückzugeben.

  Im folgenden Beispiel werden die Felder den Eigenschaften `UserName` und `Password` eines Objekts vom Typ `PSCredential` zugeordnet und dann an das Cmdlet übergeben.

  ```powershell
  $contosoConnection = Get-AutomationConnection -Name 'ContosoConnection'

  $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $contosoConnection.UserName, $contosoConnection.Password
  Connect-Contoso -Credential $cred
  }
  ```

  Eine einfachere und bessere Herangehensweise für dieses Verhalten besteht jedoch darin, das Verbindungsobjekt direkt an das Cmdlet zu übergeben:

  ```powershell
  $contosoConnection = Get-AutomationConnection -Name 'ContosoConnection'

  Connect-Contoso -Connection $contosoConnection
  }
  ```

  Sie können ein solches Verhalten für Ihre Cmdlets aktivieren, indem Sie zulassen, dass ein Verbindungsobjekt direkt als Parameter akzeptiert wird, und nicht nur Verbindungsfelder für Parameter. In der Regel empfiehlt sich jeweils ein Parametersatz, damit ein Benutzer, der Azure Automation nicht benutzt, Ihre Cmdlets aufrufen kann, ohne eine Hashtabelle erstellen zu müssen, die als Verbindungsobjekt dient. Mit dem Parametersatz `UserAccount` werden die Eigenschaften des Verbindungsfelds übergeben. Mit `ConnectionObject` können Sie die Verbindung direkt übergeben.

### <a name="output-type"></a>Ausgabetyp

Definieren Sie den Ausgabetyp für alle Cmdlets in Ihrem Modul. Wenn Sie einen Ausgabetyp für ein Cmdlet definieren, können Sie bei der Erstellung mithilfe von IntelliSense die Ausgabeeigenschaften des Cmdlets festlegen, die während der Erstellung verwendet werden sollen. Diese Vorgehensweise ist bei der grafischen Erstellung von Automation-Runbooks hilfreich, da Kenntnisse zum Zeitpunkt des Entwurfs für die Benutzerfreundlichkeit Ihres Moduls von zentraler Bedeutung sind.

Fügen Sie `[OutputType([<MyOutputType>])]` hinzu, wobei `MyOutputType` ein gültiger Typ ist. Weitere Informationen zu `OutputType` finden Sie unter [About Functions OutputTypeAttribute](/powershell/module/microsoft.powershell.core/about/about_functions_outputtypeattribute) (Informationen zu Functions: OutputTypeAttribute). Der folgende Code ist ein Beispiel, wie Sie `OutputType` einem Cmdlet hinzufügen:

  ```powershell
  function Get-ContosoUser {
  [OutputType([String])]
  param(
     [string]
     $Parameter1
  )
  # <script location here>
  }
  ```

  ![Ausgabetyp für grafische Runbooks](../media/modules/runbook-graphical-module-output-type.png)

  Dieses Verhalten ähnelt der Textvervollständigung der Ausgabe eines Cmdlets in der PowerShell ISE, ohne dass diese ausgeführt werden muss.

  ![POSH IntelliSense](../media/modules/automation-posh-ise-intellisense.png)

### <a name="cmdlet-state"></a>Cmdlet-Status

Legen Sie alle Cmdlets in Ihrem Modul als statusfrei fest. Mehrere Runbookaufträge können gleichzeitig in derselben `AppDomain`, im gleichen Prozess und in der gleichen Sandbox ausgeführt werden. Wenn diese Ebenen den gleichen Status verwenden, können die Aufträge sich gegenseitig beeinflussen. Dieses Verhalten kann zu vorübergehenden und schwierig zu diagnostizierenden Problemen führen. Hier sehen Sie ein Beispiel für eine FALSCHE Vorgehensweise:

  ```powershell
  $globalNum = 0
  function Set-GlobalNum {
     param(
         [int] $num
     )

     $globalNum = $num
  }
  function Get-GlobalNumTimesTwo {
     $output = $globalNum * 2

     $output
  }
  ```

### <a name="module-dependency"></a>Modulabhängigkeit

Stellen Sie sicher, dass das Modul vollständig in einem Xcopy-fähigen Paket enthalten ist. Azure Automation-Module werden an die Automation-Sandboxes verteilt, wenn Runbooks ausgeführt werden. Die Module müssen unabhängig vom Host funktionieren, auf dem Sie ausgeführt werden. 

Es muss daher möglich sein, ein Modulpaket zu komprimieren, zu verschieben und nach dem Import in die PowerShell-Umgebung eines anderen Hosts ganz normal zu verwenden. Stellen Sie dazu sicher, dass das Modul nicht von Dateien außerhalb des Modulordners abhängt, der beim Importieren des Moduls in Azure Automation komprimiert wird. 

Das Modul darf nicht von konkreten Registrierungseinstellungen auf einem Host abhängen. Ein Beispiel hierfür sind die Einstellungen, die beim Installieren eines Produkts festgelegt werden. 

Stellen Sie sicher, dass alle Dateien im Modul Pfade mit einer Länge von weniger als 140 Zeichen aufweisen. Pfade mit einer Länge von mehr als 140 Zeichen bewirken Probleme beim Importieren von Runbooks. Wenn Sie diese bewährte Methode nicht befolgen, kann das Modul in Azure Automation nicht verwendet werden.  

### <a name="references-to-azurerm-and-az"></a>Verweise auf AzureRM und Az

Falls Sie in Ihrem Modul auf das [Azure PowerShell Az-Modul](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) verweisen, achten Sie darauf, dass Sie nicht auch auf AzureRM verweisen. Das Az-Modul kann nicht in Verbindung mit dem AzureRM-Modul verwendet werden. Az wird in Runbooks zwar unterstützt, aber standardmäßig nicht importiert. Weitere Informationen zum Az-Modul sowie wichtige zu berücksichtigende Aspekte finden Sie unter [Unterstützung für Az-Module in Azure Automation](../az-modules.md).

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Erstellen von PowerShell-Modulen finden Sie unter [Schreiben eines Windows PowerShell-Moduls](/powershell/scripting/developer/windows-powershell).
