---
title: Kompilieren von Konfigurationen in Azure Automation DSC
description: In diesem Artikel wird das Kompilieren von DSC-Konfigurationen (Desired State Configuration, Konfiguration des gewünschten Zustands) für Azure Automation beschrieben.
services: automation
ms.subservice: dsc
ms.date: 09/10/2018
ms.topic: conceptual
ms.openlocfilehash: a4bdd28d2ad8f692b561d414af15b90b1609bac4
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77462121"
---
# <a name="compiling-dsc-configurations-in-azure-automation-state-configuration"></a>Kompilieren von DSC-Konfigurationen in Azure Automation DSC

Sie können DSC-Konfigurationen (Desired State Configuration, Konfiguration des gewünschten Zustands) auf zwei Weisen mit Azure Automation State Configuration kompilieren: in Azure und mit Windows PowerShell. Die folgenden Informationen helfen bei der Entscheidung, welche Methode sich in welcher Situation am besten eignet:

- Kompilierungsdienst für Azure State Configuration
  - Einfache Methode mit interaktiver Benutzeroberfläche
   - Einfaches Nachverfolgen des Auftragsstatus

- Windows PowerShell
  - Aufruf in Windows PowerShell auf der lokalen Arbeitsstation oder im Builddienst
  - Integration in Pipeline für Entwicklungstests
  - Bereitstellen komplexer Parameterwerte
  - Arbeiten mit Knoten- und Nicht-Knotendaten nach Maß
  - Beträchtliche Leistungsverbesserung

Weitere Informationen zur Kompilierung finden Sie in [Erweiterung zum Konfigurieren des gewünschten Zustands mit Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/virtual-machines/extensions/dsc-template#details).

## <a name="compiling-a-dsc-configuration-in-azure-state-configuration"></a>Kompilieren einer DSC-Konfiguration in Azure State Configuration

### <a name="portal"></a>Portal

1. Klicken Sie im Automation-Konto auf **Zustandskonfiguration (DSC)** .
1. Klicken Sie auf die Registerkarte **Konfigurationen** und dann auf den Namen der Konfiguration, die kompiliert werden soll.
1. Klicken Sie auf **Kompilieren**.
1. Wenn die Konfiguration keine Parameter enthält, werden Sie dazu aufgefordert, zu bestätigen, dass Sie die Konfiguration kompilieren möchten. Wenn die Konfiguration Parameter enthält, wird das Blatt **Konfiguration kompilieren** geöffnet, auf dem Sie Parameterwerte angeben können.
1. Die Seite „Kompilierungsauftrag“ wird geöffnet, sodass Sie den Status des Kompilierungsauftrags nachverfolgen können. Sie können diese Seite auch verwenden, um die Knotenkonfigurationen (MOF-Konfigurationsdokumente) nachzuverfolgen, die der Auftrag auf dem Pullserver für die Azure Automation State Configuration platziert.

### <a name="azure-powershell"></a>Azure PowerShell

Sie können [Start-AzAutomationDscCompilationJob](/powershell/module/az.automation/start-azautomationdsccompilationjob) verwenden, um die Kompilierung mit Windows PowerShell zu beginnen. Der folgende Beispielcode startet die Kompilierung einer DSC-Konfiguration namens SampleConfig.

```powershell
Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'SampleConfig'
```

**Start-AzAutomationDscCompilationJob** gibt ein Kompilierungsauftragsobjekt zurück, das zum Nachverfolgen des Auftragsstatus verwendet werden kann. Anschließend können Sie dieses Kompilierungsauftragsobjekt mit [Get-AzAutomationDscCompilationJob](/powershell/module/az.automation/get-azautomationdsccompilationjob) verwenden, um den Status des Kompilierungsauftrags zu ermitteln, und mit [Get-AzAutomationDscCompilationJobOutput](/powershell/module/az.automation/get-azautomationdscconfiguration), um seine Streams (Ausgabe) anzuzeigen. Der folgende Beispielcode startet die Kompilierung der SampleConfig-Konfiguration, wartet, bis die Kompilierung abgeschlossen ist, und zeigt dann die Datenströme an.

```powershell
$CompilationJob = Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'SampleConfig'

while($null -eq $CompilationJob.EndTime -and $null -eq $CompilationJob.Exception)
{
    $CompilationJob = $CompilationJob | Get-AzAutomationDscCompilationJob
    Start-Sleep -Seconds 3
}

$CompilationJob | Get-AzAutomationDscCompilationJobOutput –Stream Any
```

### <a name="declare-basic-parameters"></a>Deklarieren grundlegender Parameter

Die Parameterdeklaration in DSC-Konfigurationen, einschließlich der Parametertypen und -eigenschaften, funktioniert genauso wie in Azure Automation-Runbooks. Informationen zu Runbookparametern finden Sie unter [Starten eines Runbooks in Azure Automation](automation-starting-a-runbook.md) .

Das folgende Beispiel verwendet zwei Parameter namens **FeatureName** und **IsPresent**, um die Werte der Eigenschaften in der während der Kompilierung generierten **ParametersExample.sample**-Knotenkonfiguration zu ermitteln.

```powershell
Configuration ParametersExample
{
    param(
        [Parameter(Mandatory=$true)]
        [string] $FeatureName,

        [Parameter(Mandatory=$true)]
        [boolean] $IsPresent
    )

    $EnsureString = 'Present'
    if($IsPresent -eq $false)
    {
        $EnsureString = 'Absent'
    }

    Node 'sample'
    {
        WindowsFeature ($FeatureName + 'Feature')
        {
            Ensure = $EnsureString
            Name   = $FeatureName
        }
    }
}
```

Sie können DSC-Konfigurationen kompilieren, die grundlegende Parameter im Azure Automation DSC-Portal oder in Azure PowerShell verwenden:

#### <a name="portal"></a>Portal

Im Portal können Sie Parameterwerte eingeben, nachdem Sie auf **Kompilieren**geklickt haben.

![Konfigurationsparameter für das Kompilieren](./media/automation-dsc-compile/DSC_compiling_1.png)

#### <a name="azure-powershell"></a>Azure PowerShell

PowerShell erfordert Parameter in einer [Hashtabelle](/powershell/module/microsoft.powershell.core/about/about_hash_tables) , in der der Schlüssel dem Parameternamen entspricht und der Wert gleich dem Parameterwert ist.

```powershell
$Parameters = @{
    'FeatureName' = 'Web-Server'
    'IsPresent' = $False
}

Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'ParametersExample' -Parameters $Parameters
```

Informationen zum Übergeben von PSCredentials als Parameter finden Sie unten unter [Anmeldeinformationen](#credential-assets).

### <a name="compile-configurations-containing-composite-resources-in-azure-automation"></a>Kompilieren von Konfigurationen, die zusammengesetzte Ressourcen enthalten, in Azure Automation

Das Feature **Zusammengesetzte Ressourcen** ermöglicht Ihnen, die DSC-Konfigurationen als geschachtelte Ressourcen innerhalb einer Konfiguration zu verwenden. Dadurch können Sie mehrere Konfigurationen auf eine einzelne Ressource anwenden. Unter [Zusammengesetzte Ressourcen: Verwenden einer DSC-Konfiguration als Ressource](/powershell/scripting/dsc/resources/authoringresourcecomposite) erfahren Sie mehr über zusammengesetzte Ressourcen.

> [!NOTE]
> Damit Konfigurationen mit zusammengesetzten Ressourcen ordnungsgemäß kompiliert werden, müssen Sie zunächst sicherstellen, dass alle DSC-Ressourcen, auf denen die zusammengesetzten Elemente basieren, in Azure Automation importiert werden.

Das Hinzufügen einer zusammengesetzten DSC-Ressource unterscheidet sich nicht vom Hinzufügen eines PowerShell-Moduls zu Azure Automation. Der Prozess ist in [Verwenden von Modulen in Azure Automation](/azure/automation/shared-resources/modules) dokumentiert.

### <a name="manage-configurationdata-when-compiling-configurations-in-azure-automation"></a>Verwalten von ConfigurationData beim Kompilieren von Konfigurationen in Azure Automation

Mit dem Feature **ConfigurationData** können Sie bei Verwendung von PowerShell DSC die Konfiguration der Struktur von jeglicher umgebungsspezifischen Konfiguration trennen. Informationen zu ConfigurationData finden Sie unter [Separating „What“ from „Where“ in PowerShell DSC](https://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx)(Trennen von „Was“ von „Wo“ in PowerShell DSC).

> [!NOTE]
> Sie können ConfigurationData beim Kompilieren in Azure Automation State Configuration mit Azure PowerShell verwenden, jedoch nicht im Azure-Portal.

Die folgende DSC-Beispielkonfiguration verwendet ConfigurationData über die Schlüsselwörter $ConfigurationData und $AllNodes. Für dieses Beispiel benötigen Sie außerdem das [xWebAdministration-Modul](https://www.powershellgallery.com/packages/xWebAdministration/):

```powershell
Configuration ConfigurationDataSample
{
    Import-DscResource -ModuleName xWebAdministration -Name MSFT_xWebsite

    Write-Verbose $ConfigurationData.NonNodeData.SomeMessage

    Node $AllNodes.Where{$_.Role -eq 'WebServer'}.NodeName
    {
        xWebsite Site
        {
            Name         = $Node.SiteName
            PhysicalPath = $Node.SiteContents
            Ensure       = 'Present'
        }
    }
}
```

Sie können die vorherige DSC-Konfiguration mit Windows PowerShell kompilieren. Das folgende Skript fügt dem Pulldienst von Azure Automation State Configuration die beiden Knotenkonfigurationen hinzu: ConfigurationDataSample.MyVM1 und ConfigurationDataSample.MyVM3 hinzu.

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = 'MyVM1'
            Role = 'WebServer'
        },
        @{
            NodeName = 'MyVM2'
            Role = 'SQLServer'
        },
        @{
            NodeName = 'MyVM3'
            Role = 'WebServer'
        }
    )

    NonNodeData = @{
        SomeMessage = 'I love Azure Automation State Configuration and DSC!'
    }
}

Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'ConfigurationDataSample' -ConfigurationData $ConfigData
```

### <a name="work-with-assets-in-azure-automation-during-compilation"></a>Arbeiten mit Ressourcen in Azure Automation während der Kompilierung

Objektverweise in Azure Automation State Configuration und Runbooks sind gleich. Weitere Informationen finden Sie unter

- [Zertifikate](automation-certificates.md)
- [Verbindungen](automation-connections.md)
- [Anmeldeinformationen](automation-credentials.md)
- [Variablen](automation-variables.md)

#### <a name="credential-assets"></a>Anmeldeinformationen

DSC-Konfigurationen in Azure Automation können mithilfe des Cmdlets **Get-AutomationPSCredential** auf Automation-Anmeldeinformationsobjekte verweisen. Wenn eine Konfiguration einen Parameter aufweist, der einen PSCredential-Typ enthält, können Sie **Get-AutomationPSCredential** verwenden, indem Sie den Zeichenfolgennamen eines Azure Automation-Anmeldeinformationsobjekts übergeben, um die Anmeldeinformationen abzurufen. Sie können dieses Objekt anschließend für den Parameter verwenden, der das PSCredential-Objekt erfordert. Im Hintergrund wird das Azure Automation-Anmeldeinformationsobjekt mit diesem Namen abgerufen und an die Konfiguration übergeben. Das folgende Beispiel stellt die praktische Anwendung dieses Vorgangs dar.

Um die Sicherheit von Anmeldeinformationen in Knotenkonfigurationen zu gewährleisten, müssen diese Informationen in der MOF-Datei der Knotenkonfiguration verschlüsselt werden. Sie müssen PowerShell DSC über die Berechtigung zum Ausgeben von Anmeldeinformationen im Klartext während der MOF-Generierung der Knotenkonfiguration informieren. PowerShell DSC weiß nicht, dass Azure Automation die gesamte MOF-Datei nach ihrer Generierung über einen Kompilierungsauftrag verschlüsselt.

Um PowerShell DSC die Berechtigung zu erteilen, Anmeldeinformationen mithilfe von Konfigurationsdaten in die generierten MOF-Dateien der Knotenkonfiguration im Klartext auszugeben, übergeben Sie `PSDscAllowPlainTextPassword = $true`. Sie können diese Informationen über ConfigurationData für den Namen jedes Knotenblocks übergeben, der in der DSC-Konfiguration angezeigt wird und Anmeldeinformationen verwendet.

Das folgende Beispiel zeigt eine DSC-Konfiguration, die ein Automation-Anmeldeinformationsobjekt verwendet.

```powershell
Configuration CredentialSample
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration
    $Cred = Get-AutomationPSCredential 'SomeCredentialAsset'

    Node $AllNodes.NodeName
    {
        File ExampleFile
        {
            SourcePath      = '\\Server\share\path\file.ext'
            DestinationPath = 'C:\destinationPath'
            Credential      = $Cred
        }
    }
}
```

Sie können die vorherige DSC-Konfiguration über PowerShell kompilieren. 

Der folgende PowerShell-Befehl fügt dem Azure Automation State Configuration-Pullserver zwei Knotenkonfigurationen hinzu: CredentialSample.MyVM1 und CredentialSample.MyVM2.

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = '*'
            PSDscAllowPlainTextPassword = $True
        },
        @{
            NodeName = 'MyVM1'
        },
        @{
            NodeName = 'MyVM2'
        }
    )
}

Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'CredentialSample' -ConfigurationData $ConfigData
```

>[!NOTE]
>Wenn die Kompilierung abgeschlossen ist, wird möglicherweise eine Fehlermeldung angezeigt: **Das Modul „Microsoft.PowerShell.Management“ wurde nicht importiert, da das Snap-In „Microsoft.PowerShell.Management“ bereits importiert war.** Sie können diese Warnung problemlos ignorieren.

## <a name="compiling-a-dsc-configuration-in-windows-powershell"></a>Kompilieren einer DSC-Konfiguration in Windows PowerShell

Der Prozess zum Kompilieren von DSC-Konfigurationen in Windows PowerShell ist im Artikel [Schreiben, Kompilieren und Anwenden einer Konfiguration](/powershell/scripting/dsc/configurations/write-compile-apply-configuration#compile-the-configuration) zu PowerShell DSC beschrieben.
Sie können den Prozess auf der Arbeitsstation eines Entwicklers oder in einem Builddienst wie [Azure DevOps](https://dev.azure.com) ausführen.
Anschließend können Sie die MOF-Dateien für die resultierenden Knotenkonfigurationen direkt in den Azure State Configuration-Dienst importieren. 

>[!NOTE]
>Knotenkonfigurationsdateien dürfen nicht größer als 1 MB sein, damit sie in Azure Automation importiert werden können.

Sie können auch Knotenkonfigurationen (MOF-Dateien) importieren, die außerhalb von Azure kompiliert wurden. Dieser Ansatz bietet mehrere Vorteile, z. B. für Leistung und Zuverlässigkeit.

Das Kompilieren in Windows PowerShell bietet die Option zum Signieren von Konfigurationsinhalten, wobei der DSC-Agent eine signierte Knotenkonfiguration lokal auf einem verwalteten Knoten überprüft. Durch die Überprüfung wird sichergestellt, dass die auf den Knoten angewendete Konfiguration aus einer autorisierten Quelle stammt. Weitere Informationen zum Signieren von Knotenkonfigurationen finden Sie unter [Verbesserungen in WMF 5.1 – So signieren Sie Konfigurationen und Module](/powershell/scripting/wmf/whats-new/dsc-improvements#dsc-module-and-configuration-signing-validations).

### <a name="import-a-node-configuration-in-the-azure-portal"></a>Importieren von Knotenkonfigurationen im Azure-Portal

1. Klicken Sie in Ihrem Automation-Konto unter **Konfigurationsverwaltung** auf **Zustandskonfiguration (DSC)** .
1. Klicken Sie auf der Seite „Zustandskonfiguration (DSC)“ auf die Registerkarte **Konfigurationen** und dann auf **+ Hinzufügen**.
1. Klicken Sie auf der Seite „Importieren“ auf das Ordnersymbol neben dem Textfeld **Knotenkonfigurationsdatei**, um eine Knotenkonfigurationsdatei (MOF) auf dem lokalen Computer zu suchen.

   ![Suchen einer lokalen Datei](./media/automation-dsc-compile/import-browse.png)

1. Geben Sie im Textfeld **Konfigurationsname** einen Namen ein. Dieser Name muss mit dem Namen der Konfiguration übereinstimmen, aus der die Knotenkonfiguration kompiliert wurde.
1. Klicken Sie auf **OK**.

### <a name="import-a-node-configuration-with-azure-powershell"></a>Importieren einer Knotenkonfiguration mit Azure PowerShell

Sie können das Cmdlet [Import-AzAutomationDscNodeConfiguration](/powershell/module/az.automation/import-azautomationdscnodeconfiguration) verwenden, um eine Knotenkonfiguration in Ihr Automation-Konto zu importieren.

```powershell
Import-AzAutomationDscNodeConfiguration -AutomationAccountName 'MyAutomationAccount' -ResourceGroupName 'MyResourceGroup' -ConfigurationName 'MyNodeConfiguration' -Path 'C:\MyConfigurations\TestVM1.mof'
```

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung finden Sie unter [Erste Schritte mit Azure Automation State Configuration](automation-dsc-getting-started.md).
- Wie Sie DSC-Konfigurationen kompilieren und anschließend Zielknoten zuweisen, erfahren Sie unter [Kompilieren von DSC-Konfigurationen in Azure Automation DSC](automation-dsc-compile.md).
- Eine PowerShell-Cmdlet-Referenz ist unter [Azure Automation State Configuration-Cmdlets](/powershell/module/az.automation) verfügbar.
- Eine Preisübersicht finden Sie unter [Automation – Preise](https://azure.microsoft.com/pricing/details/automation/).
- Ein Verwendungsbeispiel für Azure Automation State Configuration in einer Continuous Deployment-Pipeline finden Sie unter [Continuous Deployment mit Azure Automation State Configuration und Chocolatey](automation-dsc-cd-chocolatey.md).
