---
title: Erstellen von Richtlinien für Gastkonfigurationen für Windows
description: Erfahren Sie, wie Sie eine Azure Policy-Richtlinie für Gastkonfigurationen für Windows erstellen.
ms.date: 03/20/2020
ms.topic: how-to
ms.openlocfilehash: 24069ff6518c4244026378e48216d4568fffeb8a
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80365471"
---
# <a name="how-to-create-guest-configuration-policies-for-windows"></a>Erstellen von Richtlinien für Gastkonfigurationen für Windows

Vor dem Erstellen benutzerdefinierter Richtlinien empfiehlt es sich, die allgemeinen Informationen zur [Azure Policy-Gastkonfiguration](../concepts/guest-configuration.md) zu lesen.
 
Informationen zum Erstellen von Richtlinien für Gastkonfigurationen für Linux finden Sie auf der Seite [Erstellen von Richtlinien für Gastkonfigurationen für Linux](./guest-configuration-create-linux.md)

Beim Überwachen von Windows wird für die Gastkonfiguration ein [DSC](/powershell/scripting/dsc/overview/overview)-Ressourcenmodul (Desired State Configuration) und eine Konfigurationsdatei verwendet. Die DSC-Konfiguration definiert den Zustand, in dem sich der Computer befinden soll.
Wenn bei der Auswertung der Konfiguration ein Fehler auftritt, wird die Richtlinienauswirkung **auditIfNotExists** ausgelöst, und der Computer wird als **nicht konform** eingestuft.

Die [Azure Policy-Gastkonfiguration](../concepts/guest-configuration.md) kann nur zur Überwachung von Einstellungen in Computern verwendet werden. Die Wiederherstellung von Einstellungen in Computern ist noch nicht verfügbar.

Verwenden Sie die folgenden Aktionen, um Ihre eigene Konfiguration zum Überprüfen des Zustands eines Azure- oder Nicht-Azure-Computers zu erstellen.

> [!IMPORTANT]
> Benutzerdefinierte Richtlinien für Gastkonfigurationen sind eine Previewfunktion.

## <a name="install-the-powershell-module"></a>Installieren des PowerShell-Moduls

Das Erstellen eines Gastkonfigurationsartefakts, das automatisierte Testen des Artefakts, das Erstellen einer Richtliniendefinition und das Veröffentlichen der Richtlinie kann mit dem Modul für Gastkonfigurationen in PowerShell vollständig automatisiert werden. Das Modul kann auf einem Computer installiert werden, auf dem Windows, macOS oder Linux mit PowerShell 6.2 oder höher lokal oder mit [Azure Cloud Shell](https://shell.azure.com) oder dem [Azure PowerShell Core Docker-Image](https://hub.docker.com/r/azuresdk/azure-powershell-core) ausgeführt wird.

> [!NOTE]
> Die Kompilierung von Konfigurationen wird unter Linux noch nicht unterstützt.

### <a name="base-requirements"></a>Basisanforderungen

Betriebssysteme, unter denen das Modul installiert werden kann:

- Linux
- macOS
- Windows

Für das Ressourcenmodul für Gastkonfigurationen wird die folgende Software benötigt:

- PowerShell 6.2 oder höher. Falls es noch nicht installiert ist, befolgen Sie [diese Anweisungen](/powershell/scripting/install/installing-powershell).
- Azure PowerShell 1.5.0 oder höher. Falls es noch nicht installiert ist, befolgen Sie [diese Anweisungen](/powershell/azure/install-az-ps).
  - Nur die AZ-Module „Az.Accounts“ und „Az.Resources“ sind erforderlich.

### <a name="install-the-module"></a>Installieren des Moduls

Führen Sie zum Installieren des Moduls **GuestConfiguration** in PowerShell die folgenden Schritte aus:

1. Führen Sie an einer PowerShell-Eingabeaufforderung den folgenden Befehl aus:

   ```azurepowershell-interactive
   # Install the Guest Configuration DSC resource module from PowerShell Gallery
   Install-Module -Name GuestConfiguration
   ```

1. Überprüfen Sie, ob das Modul importiert wurde:

   ```azurepowershell-interactive
   # Get a list of commands for the imported GuestConfiguration module
   Get-Command -Module 'GuestConfiguration'
   ```

## <a name="guest-configuration-artifacts-and-policy-for-windows"></a>Artefakte und Richtlinien für Gastkonfigurationen für Windows

Bei Gastkonfigurationen wird PowerShell DSC (Desired State Configuration) als Sprachabstraktion verwendet, um anzugeben, was in Windows überwacht werden soll. Da der Agent eine eigenständige Instanz von PowerShell 6.2 lädt, treten keine Konflikte bei Verwendung von PowerShell DSC in Windows PowerShell 5.1 auf. Außerdem ist es nicht erforderlich, PowerShell 6.2 oder höher vorab zu installieren.

Eine Übersicht über DSC-Konzepte und die Terminologie finden Sie in der [Übersicht über PowerShell DSC](/powershell/scripting/dsc/overview/overview).

### <a name="how-guest-configuration-modules-differ-from-windows-powershell-dsc-modules"></a>Unterschiede zwischen Gastkonfigurationsmodulen und Windows PowerShell DSC-Modulen

Wenn die Gastkonfiguration einen Computer überwacht, führt sie zunächst `Test-TargetResource` aus, um festzustellen, ob er den richtigen Zustand aufweist. Der von der Funktion zurückgegebene boolesche Wert bestimmt, ob der Zustand von Azure Resource Manager für die Gastzuweisung konform/nicht konform sein soll. Im nächsten Schritt führt der Anbieter `Get-TargetResource` aus, um den aktuellen Zustand der einzelnen Einstellungen zurückzugeben. Dadurch sind Details dazu verfügbar, weshalb ein Computer nicht konform ist, bzw. Angaben dazu, dass der aktuelle Zustand konform ist.

### <a name="get-targetresource-requirements"></a>Get-TargetResource-Anforderungen

Die Funktion `Get-TargetResource` hat spezielle Anforderungen an die Gastkonfiguration, die für die Windows Desired State Configuration nicht benötigt wurden.

- Die zurückgegebene Hashtabelle muss eine Eigenschaft namens **Reasons** enthalten.
- Die Reasons-Eigenschaft muss ein Array sein.
- Jedes Element im Array sollte eine Hashtabelle mit Schlüsseln namens **Code** und **Phrase** sein.

Die Reasons-Eigenschaft wird vom Dienst verwendet, um die Darstellung von Informationen zu standardisieren, wenn ein Computer nicht mehr konform ist. Sie können sich jedes Element in Reasons als „Grund“ vorstellen, dass die Ressource nicht konform ist. Die Eigenschaft ist ein Array, da eine Ressource aus mehr als einem Grund nicht konform sein könnte.

Die Eigenschaften **Code** und **Phrase** werden vom Dienst erwartet. Wenn Sie eine benutzerdefinierte Ressource erstellen, legen Sie den auszugebenden Text (typischerweise stdout) als Grund für die fehlende Konformität der Ressource als Wert für **Phrase** fest. **Code** weist bestimmte Formatierungsanforderungen auf, sodass die Berichterstellung eindeutig Informationen zur Ressource anzeigen kann, die für die Überwachung verwendet wird. Diese Lösung macht die Gastkonfiguration erweiterbar. Sie können beliebige Befehle ausführen. Die einzige Voraussetzung ist, dass die Ausgabe des Befehls als Zeichenfolgenwert für die Eigenschaft **Phrase** zurückgegeben werden kann.

- **Code** (Zeichenfolge): Der Name der Ressource, wiederholt, und dann ein Kurzname ohne Leerzeichen als Bezeichner für den Grund. Diese drei Werte sollten ohne Leerzeichen durch Doppelpunkte getrennt sein.
  - Ein Beispiel wäre `registry:registry:keynotpresent`
- **Phrase** (Zeichenfolge): Für Menschen lesbarer Text, um zu erläutern, warum die Einstellung nicht konform ist.
  - Ein Beispiel wäre `The registry key $key is not present on the machine.`

```powershell
$reasons = @()
$reasons += @{
  Code = 'Name:Name:ReasonIdentifer'
  Phrase = 'Explain why the setting is not compliant'
}
return @{
    reasons = $reasons
}
```
### <a name="configuration-requirements"></a>Konfigurationsanforderungen

Der Name der benutzerdefinierten Konfiguration muss überall einheitlich sein. Der Name der ZIP-Datei für das Inhaltspaket, der Konfigurationsname in der MOF-Datei und der Name der Gastzuweisung in der Resource Manager-Vorlage müssen identisch sein.

### <a name="scaffolding-a-guest-configuration-project"></a>Gerüstbau für ein Gastkonfigurationsprojekt

Entwickler, die den Einstieg und die Arbeit mit Beispielcode beschleunigen möchten, können ein Communityprojekt namens **Guest Configuration Project** (Gastkonfigurationsprojekt) installieren. Mit diesem Projekt wird eine Vorlage für das PowerShell-Modul [Plaster](https://github.com/powershell/plaster) installiert. Dieses Tool kann verwendet werden, um ein Projekt mit einer funktionierenden Konfiguration und einer Beispielressource sowie einem Satz von [Pester](https://github.com/pester/pester)-Tests zur Überprüfung des Projekts einzurichten. Die Vorlage enthält auch Aufgabenausführungen für Visual Studio Code, um die Erstellung und Überprüfung des Gastkonfigurationspakets zu automatisieren. Weitere Informationen finden Sie im GitHub-Projekt [Guest Configuration Project](https://github.com/microsoft/guestconfigurationproject) (Gastkonfigurationsprojekt).

Weitere Informationen zu allgemeinen Aufgaben, die bei Konfigurationen ausgeführt werden, finden Sie unter [Schreiben, Kompilieren und Anwenden einer Konfiguration](/powershell/scripting/dsc/configurations/write-compile-apply-configuration).

### <a name="expected-contents-of-a-guest-configuration-artifact"></a>Erwarteter Inhalt eines Gastkonfigurationsartefakts

Das fertige Paket wird von der Gastkonfiguration verwendet, um die Azure Policy-Definitionen zu erstellen. Das Paket enthält Folgendes:

- Kompilierte DSC-Konfiguration als MOF-Datei
- Ordner „Modules“
  - GuestConfiguration-Modul
  - DscNativeResources-Modul
  - (Windows) Für die MOF-Datei sind DSC-Ressourcenmodule erforderlich

Zum Erstellen des Pakets können PowerShell-Cmdlets verwendet werden.
Es ist kein Ordner auf Stammebene oder Versionsordner erforderlich.
Das Paketformat muss eine ZIP-Datei sein.

### <a name="storing-guest-configuration-artifacts"></a>Speichern von Gastkonfigurationsartefakten

Das ZIP-Paket muss an einem Speicherort gespeichert werden, auf den von den verwalteten virtuellen Computern zugegriffen werden kann.
Beispiele hierfür sind GitHub-Repositorys, ein Azure-Repository oder Azure Storage. Falls Sie das Paket nicht öffentlich zugänglich machen möchten, können Sie ein [SAS-Token](../../../storage/common/storage-dotnet-shared-access-signature-part-1.md) in die URL einfügen.
Sie können auch einen [Dienstendpunkt](../../../storage/common/storage-network-security.md#grant-access-from-a-virtual-network) für Computer in einem privaten Netzwerk implementieren. Diese Konfiguration gilt aber nur für den Zugriff auf das Paket und nicht für die Kommunikation mit dem Dienst.

## <a name="step-by-step-creating-a-custom-guest-configuration-audit-policy-for-windows"></a>Schrittanleitung: Erstellen einer benutzerdefinierten Überwachungsrichtlinie für Gastkonfigurationen für Windows

Erstellen Sie eine DSC-Konfiguration. Im folgenden PowerShell-Beispielskript wird eine Konfiguration mit dem Namen **AuditBitLocker** erstellt, das Ressourcenmodul **PsDscResources** importiert und die Ressource `Service` verwendet, um eine Überwachung für einen ausgeführten Dienst durchzuführen. Das Konfigurationsskript kann von einem Windows- oder macOS-Computer aus ausgeführt werden.

```powershell
# Define the DSC configuration and import GuestConfiguration
Configuration AuditBitLocker
{
    Import-DscResource -ModuleName 'PSDscResources'

    Node AuditBitlocker {
      Service 'Ensure BitLocker service is present and running'
      {
          Name = 'BDESVC'
          Ensure = 'Present'
          State = 'Running'
      }
    }
}

# Compile the configuration to create the MOF files
AuditBitLocker -out ./Config
```

Der Befehl `Node AuditBitlocker` ist aus technischer Sicht nicht erforderlich, doch wird damit eine Datei namens `AuditBitlocker.mof` anstelle der Standarddatei `localhost.mof` erstellt. Wenn der Name der MOF-Datei der Konfiguration folgt, können beim Arbeiten in großem Umfang viele Dateien problemlos organisiert werden.

Nachdem die MOF-Datei kompiliert wurde, müssen die unterstützenden Dateien in einem Paket zusammengefasst werden. Das fertige Paket wird von der Gastkonfiguration verwendet, um die Azure Policy-Definitionen zu erstellen.

Mit dem Cmdlet `New-GuestConfigurationPackage` wird das Paket erstellt. Parameter des Cmdlets `New-GuestConfigurationPackage` beim Erstellen von Windows-Inhalten:

- **Name**: Name des Pakets mit der Gastkonfiguration.
- **Konfiguration:** Vollständiger Pfad für das kompilierte DSC-Konfigurationsdokument.
- **Pfad**: Pfad des Ausgabeordners. Dieser Parameter ist optional. Wenn er nicht angegeben ist, wird das Paket im aktuellen Verzeichnis erstellt.

Führen Sie den folgenden Befehl aus, um ein Paket mit der im vorherigen Schritt angegebenen Konfiguration zu erstellen:

```azurepowershell-interactive
New-GuestConfigurationPackage `
  -Name 'AuditBitlocker' `
  -Configuration './Config/AuditBitlocker.mof'
```

Nach dem Erstellen des Konfigurationspakets – aber vor der Veröffentlichung in Azure – können Sie das Paket über Ihre Arbeitsstation oder die CI/CD-Umgebung testen. Das GuestConfiguration-Cmdlet `Test-GuestConfigurationPackage` enthält denselben Agent in Ihrer Entwicklungsumgebung, der auch auf Azure-Computern genutzt wird. Mit dieser Lösung können Sie Integrationstests lokal durchführen, bevor die Veröffentlichung für kostenpflichtige Cloudumgebungen erfolgt.

Da der Agent tatsächlich die lokale Umgebung auswertet, müssen Sie in den meisten Fällen das Test-Cmdlet auf derselben Betriebssystemplattform ausführen, die Sie überwachen möchten.

Parameter des Cmdlets `Test-GuestConfigurationPackage`:

- **Name**: Name der Richtlinie für Gastkonfigurationen.
- **Parameter**: Richtlinienparameter im Hashtabellenformat.
- **Pfad**: Vollständiger Pfad des Pakets mit der Gastkonfiguration.

Führen Sie den folgenden Befehl aus, um das im vorherigen Schritt erstellte Paket zu testen:

```azurepowershell-interactive
Test-GuestConfigurationPackage `
  -Path ./AuditBitlocker.zip
```

Das Cmdlet unterstützt auch Eingaben aus der PowerShell-Pipeline. Fügen Sie die Ausgabe des Cmdlets `New-GuestConfigurationPackage` per Pipezeichen an das Cmdlet `Test-GuestConfigurationPackage` an.

```azurepowershell-interactive
New-GuestConfigurationPackage -Name AuditBitlocker -Configuration ./Config/AuditBitlocker.mof | Test-GuestConfigurationPackage
```

Im nächsten Schritt wird die Datei im Blobspeicher veröffentlicht. Das folgende Skript enthält eine Funktion, mit der Sie diese Aufgabe automatisieren können. Für die in der `publish`-Funktion verwendeten Befehle ist das `Az.Storage`-Modul erforderlich.

```azurepowershell-interactive
function publish {
    param(
    [Parameter(Mandatory=$true)]
    $resourceGroup,
    [Parameter(Mandatory=$true)]
    $storageAccountName,
    [Parameter(Mandatory=$true)]
    $storageContainerName,
    [Parameter(Mandatory=$true)]
    $filePath,
    [Parameter(Mandatory=$true)]
    $blobName
    )

    # Get Storage Context
    $Context = Get-AzStorageAccount -ResourceGroupName $resourceGroup `
        -Name $storageAccountName | `
        ForEach-Object { $_.Context }

    # Upload file
    $Blob = Set-AzStorageBlobContent -Context $Context `
        -Container $storageContainerName `
        -File $filePath `
        -Blob $blobName `
        -Force

    # Get url with SAS token
    $StartTime = (Get-Date)
    $ExpiryTime = $StartTime.AddYears('3')  # THREE YEAR EXPIRATION
    $SAS = New-AzStorageBlobSASToken -Context $Context `
        -Container $storageContainerName `
        -Blob $blobName `
        -StartTime $StartTime `
        -ExpiryTime $ExpiryTime `
        -Permission rl `
        -FullUri

    # Output
    return $SAS
}

# replace the $storageAccountName value below, it must be globally unique
$resourceGroup        = 'policyfiles'
$storageAccountName   = 'youraccountname'
$storageContainerName = 'artifacts'

$uri = publish `
  -resourceGroup $resourceGroup `
  -storageAccountName $storageAccountName `
  -storageContainerName $storageContainerName `
  -filePath ./AuditBitlocker.zip `
  -blobName 'AuditBitlocker'
```

Nachdem ein benutzerdefiniertes Richtlinienpaket für Gastkonfigurationen erstellt und hochgeladen wurde, erstellen Sie die Richtliniendefinition für Gastkonfigurationen. Das Cmdlet `New-GuestConfigurationPolicy` verwendet ein benutzerdefiniertes Richtlinienpaket und erstellt eine Richtliniendefinition.

Parameter des Cmdlets `New-GuestConfigurationPolicy`:

- **ContentUri**: Öffentlicher HTTP(S)-URI des Pakets mit dem Inhalt der Gastkonfiguration.
- **DisplayName**: Anzeigename der Richtlinie.
- **Beschreibung**: Beschreibung der Richtlinie.
- **Parameter**: Richtlinienparameter im Hashtabellenformat.
- **Version**: Version der Richtlinie.
- **Pfad**: Zielpfad, unter dem Richtliniendefinitionen erstellt werden.
- **Plattform**: Zielplattform (Windows/Linux) für das Paket mit den Richtlinien und dem Inhalt der Gastkonfiguration.

Im folgenden Beispiel werden die Richtliniendefinitionen in einem angegebenen Pfad aus einem benutzerdefinierten Richtlinienpaket erstellt:

```azurepowershell-interactive
New-GuestConfigurationPolicy `
    -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditBitLocker.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
    -DisplayName 'Audit BitLocker Service.' `
    -Description 'Audit if BitLocker is not enabled on Windows machine.' `
    -Path './policies' `
    -Platform 'Windows' `
    -Version 1.0.0 `
    -Verbose
```

Mit `New-GuestConfigurationPolicy` werden die folgenden Dateien erstellt:

- **auditIfNotExists.json**
- **deployIfNotExists.json**
- **Initiative.json**

In der Ausgabe des Cmdlets wird ein Objekt zurückgegeben, das den Anzeigenamen der Initiative und den Pfad der Richtliniendateien enthält.

Abschließend veröffentlichen Sie die Richtliniendefinitionen mit dem Cmdlet `Publish-GuestConfigurationPolicy`. Das Cmdlet verfügt nur über den Parameter **Path**, mit dem auf den Speicherort der JSON-Dateien verwiesen wird, die mit `New-GuestConfigurationPolicy` erstellt werden.

Um den Veröffentlichungsbefehl auszuführen, benötigen Sie Zugriff zum Erstellen von Richtlinien in Azure. Die entsprechenden Autorisierungsanforderungen sind auf der Seite mit der [Übersicht über Azure Policy](../overview.md) dokumentiert. Die beste integrierte Rolle ist **Mitwirkender bei Ressourcenrichtlinien**.

```azurepowershell-interactive
Publish-GuestConfigurationPolicy -Path '.\policyDefinitions'
```

Das Cmdlet `Publish-GuestConfigurationPolicy` akzeptiert den Pfad von der PowerShell-Pipeline. Dank dieses Features können Sie die Richtliniendateien erstellen und veröffentlichen, indem Sie nur eine Befehlszeile verwenden, in der die Befehle per Pipezeichen verknüpft sind.

```azurepowershell-interactive
New-GuestConfigurationPolicy `
 -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditBitLocker.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
  -DisplayName 'Audit BitLocker service.' `
  -Description 'Audit if the BitLocker service is not enabled on Windows machine.' `
  -Path './policies' `
 | Publish-GuestConfigurationPolicy
```

Bei der in Azure erstellten Richtlinie ist der letzte Schritt das Zuweisen der Initiative. Informieren Sie sich darüber, wie Sie die Initiative per [Portal](../assign-policy-portal.md), [Azure CLI](../assign-policy-azurecli.md) oder [Azure PowerShell](../assign-policy-powershell.md) zuweisen können.

> [!IMPORTANT]
> Richtlinien für Gastkonfigurationen müssen **immer** über die Initiative zugewiesen werden, in der die Richtlinien _AuditIfNotExists_ und _DeployIfNotExists_ kombiniert sind. Wenn nur die Richtlinie _AuditIfNotExists_ zugewiesen wird, werden die erforderlichen Komponenten nicht bereitgestellt, und die Richtlinie zeigt immer an, dass „0“ Server konform sind.

Das Zuweisen einer Richtliniendefinition mit der Auswirkung _DeployIfNotExists_ erfordert eine zusätzliche Zugriffsebene. Zum Erteilen der geringsten Berechtigung können Sie eine benutzerdefinierte Rollendefinition erstellen, die **Mitwirkender bei Ressourcenrichtlinien** erweitert. Im folgenden Beispiel wird eine Rolle mit dem Namen **Resource Policy Contributor DINE** mit der zusätzlichen Berechtigung _Microsoft.Authorization/roleAssignments/write_ erstellt.

```azurepowershell-interactive
$subscriptionid = '00000000-0000-0000-0000-000000000000'
$role = Get-AzRoleDefinition "Resource Policy Contributor"
$role.Id = $null
$role.Name = "Resource Policy Contributor DINE"
$role.Description = "Can assign Policies that require remediation."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Authorization/roleAssignments/write")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/$subscriptionid")
New-AzRoleDefinition -Role $role
```

### <a name="using-parameters-in-custom-guest-configuration-policies"></a>Verwenden von Parametern in benutzerdefinierten Richtlinien für Gastkonfigurationen

Die Gastkonfiguration unterstützt das Außerkraftsetzen von Eigenschaften einer Konfiguration während der Laufzeit. Dieses Feature bewirkt, dass die Werte in der MOF-Datei im Paket nicht als statisch angesehen werden müssen. Die Überschreibungswerte werden über Azure Policy bereitgestellt und wirken sich nicht darauf aus, wie die Konfigurationen erstellt oder kompiliert werden.

Die Cmdlets `New-GuestConfigurationPolicy` und `Test-GuestConfigurationPolicyPackage` enthalten einen Parameter mit dem Namen **Parameters**. Für diesen Parameter wird eine Hashtabellendefinition verwendet, die alle Details zu den einzelnen Parametern enthält, und es werden die erforderlichen Abschnitte der einzelnen Dateien erstellt, die für die Azure Policy-Definition verwendet werden.

Im folgenden Beispiel wird eine Richtliniendefinition zum Überwachen eines Diensts erstellt, bei der der Benutzer bei der Zuweisung der Richtlinie eine Auswahl aus einer Liste trifft.

```azurepowershell-interactive
$PolicyParameterInfo = @(
    @{
        Name = 'ServiceName'                                            # Policy parameter name (mandatory)
        DisplayName = 'windows service name.'                           # Policy parameter display name (mandatory)
        Description = "Name of the windows service to be audited."      # Policy parameter description (optional)
        ResourceType = "Service"                                        # DSC configuration resource type (mandatory)
        ResourceId = 'windowsService'                                   # DSC configuration resource property name (mandatory)
        ResourcePropertyName = "Name"                                   # DSC configuration resource property name (mandatory)
        DefaultValue = 'winrm'                                          # Policy parameter default value (optional)
        AllowedValues = @('BDESVC','TermService','wuauserv','winrm')    # Policy parameter allowed values (optional)
    }
)

New-GuestConfigurationPolicy
    -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditBitLocker.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
    -DisplayName 'Audit Windows Service.' `
    -Description 'Audit if a Windows Service is not enabled on Windows machine.' `
    -Path '.\policyDefinitions' `
    -Parameters $PolicyParameterInfo `
    -Version 1.0.0
```

## <a name="policy-lifecycle"></a>Lebenszyklus von Richtlinien

Wenn Sie eine Aktualisierung der Richtliniendefinition freigeben möchten, sind zwei Felder zu beachten.

- **Version**: Beim Ausführen des Cmdlets `New-GuestConfigurationPolicy` müssen Sie eine Versionsnummer angeben, die höher als die der derzeitigen Veröffentlichung ist. Die Eigenschaft aktualisiert die Version der Gastkonfigurationszuweisung, damit der Agent das aktualisierte Paket erkennt.
- **contentHash**: Diese Eigenschaft wird vom Cmdlet `New-GuestConfigurationPolicy` automatisch aktualisiert. Es handelt sich um einen Hashwert des Pakets, das mit `New-GuestConfigurationPackage` erstellt wurde. Diese Eigenschaft muss für die von Ihnen veröffentlichte Datei vom Typ `.zip` stimmen. Wenn nur die Eigenschaft **contentUri** aktualisiert wird, akzeptiert die Erweiterung das Inhaltspaket nicht.

Die einfachste Möglichkeit zum Freigeben eines aktualisierten Pakets ist das Wiederholen des Prozesses in diesem Artikel und das Angeben einer aktualisierten Versionsnummer. Mit dieser Vorgehensweise wird sichergestellt, dass alle Eigenschaften richtig aktualisiert wurden.

## <a name="converting-windows-group-policy-content-to-azure-policy-guest-configuration"></a>Konvertieren des Inhalts der Windows-Gruppenrichtlinie in eine Azure Policy-Gastkonfiguration

Bei der Überwachung von Windows-Computern ist die Gastkonfiguration eine Implementierung der Desired State Configuration-Syntax von PowerShell. Von der DSC-Community wurde ein Tool zum Konvertieren exportierter Gruppenrichtlinienvorlagen in das DSC-Format veröffentlicht. Sie können dieses Tool zusammen mit den oben beschriebenen Gastkonfigurations-Cmdlets verwenden, um den Inhalt der Windows-Gruppenrichtlinie zu konvertieren und zur Überwachung für Azure Policy zu packen und zu veröffentlichen. Ausführliche Informationen zur Verwendung des Tools finden Sie im Artikel [Schnellstart: Konvertieren von Gruppenrichtlinien in DSC](/powershell/scripting/dsc/quickstarts/gpo-quickstart).
Nach dem Konvertieren des Inhalts sind die obigen Schritte, mit denen ein Paket erstellt und in Azure Policy veröffentlicht wird, die gleichen wie bei jedem anderen DSC-Inhalt.

## <a name="optional-signing-guest-configuration-packages"></a>Optional: Signieren von Paketen für Gastkonfigurationen

Benutzerdefinierte Richtlinien für Gastkonfigurationen verwenden SHA256-Hash, um zu überprüfen, ob sich das Richtlinienpaket nicht geändert hat.
Optional können Kunden auch ein Zertifikat nutzen, um Pakete zu signieren und für die Gastkonfigurationserweiterung zu erzwingen, dass nur signierte Inhalte zulässig sind.

Für dieses Szenario müssen Sie zwei Schritte ausführen. Führen Sie das Cmdlet zum Signieren des Inhaltspakets aus, und fügen Sie ein Tag an die Computer an, auf denen das Signieren von Code erzwungen werden soll.

Führen Sie zum Verwenden des Features für die Signaturüberprüfung das Cmdlet `Protect-GuestConfigurationPackage` aus, um das Paket vor der Veröffentlichung zu signieren. Für dieses Cmdlet ist ein Zertifikat für das „Codesignieren“ erforderlich.

```azurepowershell-interactive
$Cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object {($_.Subject-eq "CN=mycert") }
Protect-GuestConfigurationPackage -Path .\package\AuditWindowsService\AuditWindowsService.zip -Certificate $Cert -Verbose
```

Parameter des Cmdlets `Protect-GuestConfigurationPackage`:

- **Pfad**: Vollständiger Pfad des Pakets mit der Gastkonfiguration.
- **Zertifikat**: Codesignaturzertifikat zum Signieren des Pakets. Dieser Parameter wird nur beim Signieren von Inhalt für Windows unterstützt.

Vom GuestConfiguration-Agent wird erwartet, dass der öffentliche Schlüssel des Zertifikats auf Windows-Computern unter „Vertrauenswürdige Stammzertifizierungsstellen“ und auf Linux-Computern unter dem Pfad `/usr/local/share/ca-certificates/extra` vorhanden ist. Damit auf dem Knoten signierter Inhalt überprüft werden kann, müssen Sie den öffentlichen Schlüssel des Zertifikats auf dem Computer installieren, bevor Sie die benutzerdefinierte Richtlinie anwenden. Dieser Prozess kann mit einem beliebigen Verfahren auf der VM oder mit Azure Policy durchgeführt werden. Eine Beispielvorlage finden Sie [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-push-certificate-windows).
In der Key Vault-Zugriffsrichtlinie muss es für den Computeressourcenanbieter zulässig sein, bei Bereitstellungen auf Zertifikate zuzugreifen. Informationen zu den ausführlichen Schritten finden Sie unter [Einrichten des Schlüsseltresors für virtuelle Computer in Azure Resource Manager](../../../virtual-machines/windows/key-vault-setup.md#use-templates-to-set-up-key-vault).

Hier ist ein Beispiel für den Export des öffentlichen Schlüssels aus einem Signaturzertifikat angegeben, um ihn dann auf dem Computer importieren zu können.

```azurepowershell-interactive
$Cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object {($_.Subject-eq "CN=mycert3") } | Select-Object -First 1
$Cert | Export-Certificate -FilePath "$env:temp\DscPublicKey.cer" -Force
```

Fügen Sie nach dem Veröffentlichen Ihres Inhalts ein Tag mit dem Namen `GuestConfigPolicyCertificateValidation` und dem Wert `enabled` an alle virtuellen Computer an, für die das Codesignieren erforderlich ist. Informationen darüber, wie Tags mithilfe von Azure Policy im großen Stil bereitgestellt werden können, finden Sie in den [Tagbeispielen](../samples/built-in-policies.md#tags). Wenn dieses Tag vorhanden ist, ermöglicht die Richtliniendefinition, die mit dem Cmdlet `New-GuestConfigurationPolicy` generiert wurde, die Anforderung über die Gastkonfigurationserweiterung.

## <a name="troubleshooting-guest-configuration-policy-assignments-preview"></a>Problembehandlung für Richtlinienzuweisungen für die Gastkonfiguration (Vorschauversion)

Es gibt eine Vorschauversion eines Tools, das Sie bei der Problembehandlung für Zuweisungen für die Azure Policy-Gastkonfiguration unterstützt. Das Tool befindet sich in der Vorschauphase und wurde als Modul namens [GuestConfigurationTroubleshooter](https://www.powershellgallery.com/packages/GuestConfigurationTroubleshooter/) im PowerShell-Katalog veröffentlicht.

Verwenden Sie zum Abrufen weiterer Informationen zu den Cmdlets in diesem Tool den Befehl „Get-Help“ in PowerShell, um den integrierten Leitfaden anzuzeigen. Da das Tool regelmäßig aktualisiert wird, ist dies die beste Option, um aktuelle Informationen zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die Überprüfung von VMs mit [Gastkonfiguration](../concepts/guest-configuration.md).
- Informieren Sie sich über das [programmgesteuerte Erstellen von Richtlinien](programmatically-create.md).
- Informieren Sie sich über das [Abrufen von Konformitätsdaten](get-compliance-data.md).
