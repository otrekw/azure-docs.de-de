---
title: Erstellen von Richtlinien für Gastkonfigurationen für Windows
description: Erfahren Sie, wie Sie eine Azure Policy-Richtlinie für Gastkonfigurationen für Windows erstellen.
ms.date: 03/31/2021
ms.topic: how-to
ms.openlocfilehash: 78607b3e1694ecbfb8be58c4415d3dd79c4ac53b
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112286973"
---
# <a name="how-to-create-guest-configuration-policies-for-windows"></a>Erstellen von Richtlinien für Gastkonfigurationen für Windows

Vor dem Erstellen von benutzerdefinierten Richtliniendefinitionen empfiehlt es sich, die allgemeinen Informationen zur [Azure Policy-Gastkonfiguration](../concepts/guest-configuration.md) zu lesen.

Informationen zum Erstellen von Richtlinien für Gastkonfigurationen für Linux finden Sie auf der Seite [Erstellen von Richtlinien für Gastkonfigurationen für Linux](./guest-configuration-create-linux.md)

Beim Überwachen von Windows wird für die Gastkonfiguration ein [DSC](/powershell/scripting/dsc/overview/overview)-Ressourcenmodul (Desired State Configuration) zum Erstellen der Konfigurationsdatei verwendet. Die DSC-Konfiguration definiert den Zustand, in dem sich der Computer befinden soll. Wenn bei der Auswertung der Konfiguration ein Fehler auftritt, wird die Richtlinienauswirkung **auditIfNotExists** ausgelöst, und der Computer wird als **nicht konform** eingestuft.

Die [Azure Policy-Gastkonfiguration](../concepts/guest-configuration.md) kann nur zur Überwachung von Einstellungen in Computern verwendet werden. Die Wiederherstellung von Einstellungen in Computern ist noch nicht verfügbar.

Verwenden Sie die folgenden Aktionen, um Ihre eigene Konfiguration zum Überprüfen des Zustands eines Azure- oder Nicht-Azure-Computers zu erstellen.

> [!IMPORTANT]
> Bei benutzerdefinierten Richtliniendefinitionen mit Gastkonfiguration in den Umgebungen „Azure Government“ und „Azure China 21Vianet“ handelt es sich um eine Previewfunktion.
>
> Die Gastkonfigurationserweiterung ist zum Durchführen von Überprüfungen in virtuellen Azure-Computern erforderlich. Weisen Sie die folgenden Richtliniendefinitionen zu, um die Erweiterung auf allen Windows-Computern im gewünschten Umfang bereitzustellen: `Deploy prerequisites to enable Guest Configuration Policy on Windows VMs`
>
> Verwenden Sie in Paketen mit benutzerdefiniertem Inhalt keine Geheimnisse oder vertraulichen Informationen.

## <a name="install-the-powershell-module"></a>Installieren des PowerShell-Moduls

Mit dem Modul „Gastkonfiguration“ wird die Erstellung von benutzerdefinierten Inhalten automatisiert, z. B.:

- Erstellen eines Inhaltsartefakts für die Gastkonfiguration (ZIP-Datei)
- Durchführen eines automatisierten Tests des Artefakts
- Erstellen einer Richtliniendefinition
- Veröffentlichen der Richtlinie

Das Modul kann auf einem Computer installiert werden, auf dem Windows, macOS oder Linux mit PowerShell 6.2 oder höher lokal oder mit [Azure Cloud Shell](https://shell.azure.com) oder dem [Azure PowerShell Core Docker-Image](https://hub.docker.com/r/azuresdk/azure-powershell-core) ausgeführt wird.

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
  - Nur die Az-Module „Az.Accounts“ und „Az.Resources“ sind erforderlich.

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

Wenn die Gastkonfiguration einen Computer überwacht, weicht die Abfolge der Ereignisse von der Abfolge in Windows PowerShell DSC ab.

1. Der Agent führt zuerst `Test-TargetResource` aus, um zu ermitteln, ob die Konfiguration den richtigen Status aufweist.
1. Der von der Funktion zurückgegebene boolesche Wert bestimmt, ob der Zustand von Azure Resource Manager für die Gastzuweisung konform/nicht konform sein soll.
1. Der Anbieter führt `Get-TargetResource` aus, um den aktuellen Zustand der einzelnen Einstellungen zurückzugeben. Dadurch sind Details dazu verfügbar, warum ein Computer nicht konform ist, bzw. eine Bestätigung, dass der aktuelle Zustand konform ist.

Parameter in Azure Policy, die Werte an Gastkonfigurationszuweisungen übergeben, müssen den Typ _Zeichenfolge_ aufweisen. Arrays können nicht über Parameter übergeben werden. Dies gilt selbst dann, wenn die DSC-Ressource Arrays unterstützt.

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

Die Reasons-Eigenschaft muss dem Schema-MOF für die Ressource als eingebettete Klasse hinzugefügt werden.

```mof
[ClassVersion("1.0.0.0")]
class Reason
{
    [Read] String Phrase;
    [Read] String Code;
};

[ClassVersion("1.0.0.0"), FriendlyName("ResourceName")]
class ResourceName : OMI_BaseResource
{
    [Key, Description("Example description")] String Example;
    [Read, EmbeddedInstance("Reason")] String Reasons[];
};
```

Wenn die Ressource über erforderliche Eigenschaften verfügt, müssen diese Eigenschaften von `Get-TargetResource` parallel zur `reasons`Klasse zurückgegeben werden. Wenn `reasons` nicht eingeschlossen ist, enthält der Dienst ein „Catch-All“-Verhalten, das die Werteingaben mit `Get-TargetResource` und den von `Get-TargetResource` zurückgegebenen Werten vergleicht und einen ausführlichen Vergleich als `reasons` bereitstellt.

### <a name="configuration-requirements"></a>Konfigurationsanforderungen

Der Name der benutzerdefinierten Konfiguration muss überall einheitlich sein. Der Name der ZIP-Datei für das Inhaltspaket, der Konfigurationsname in der MOF-Datei und der Name der Gastzuweisung in der Azure Resource Manager-Vorlage (ARM-Vorlage) müssen identisch sein.

### <a name="policy-requirements"></a>Richtlinienanforderungen

Der Abschnitt `metadata` der Richtliniendefinition muss zwei Eigenschaften für den Gastkonfigurationsdienst enthalten, um die Bereitstellung und die Berichterstellung von Gastkonfigurationszuweisungen zu automatisieren. Die `category`-Eigenschaft muss auf „Guest Configuration“ festgelegt werden, und ein Abschnitt mit dem Namen `Guest Configuration` muss Informationen zur Gastkonfigurationszuweisung enthalten. Mit dem `New-GuestConfigurationPolicy`-Cmdlet wird dieser Text automatisch erstellt.
Weitere Informationen finden Sie in den Anleitungen auf dieser Seite.

Das folgende Beispiel veranschaulicht die Verwendung des Abschnitts `metadata`.

```json
    "metadata": {
      "category": "Guest Configuration",
      "guestConfiguration": {
        "name": "test",
        "version": "1.0.0",
        "contentType": "Custom",
        "contentUri": "CUSTOM-URI-HERE",
        "contentHash": "CUSTOM-HASH-VALUE-HERE",
        "configurationParameter": {}
      }
    },
```

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

Zum Erstellen des Pakets können PowerShell-Cmdlets verwendet werden. Es ist kein Ordner auf Stammebene oder Versionsordner erforderlich. Das Paketformat muss eine ZIP-Datei sein, deren Gesamtgröße unkomprimiert 100 MB nicht überschreiten darf.

### <a name="storing-guest-configuration-artifacts"></a>Speichern von Gastkonfigurationsartefakten

Das ZIP-Paket muss an einem Speicherort gespeichert werden, auf den von den verwalteten virtuellen Computern zugegriffen werden kann.
Beispiele hierfür sind GitHub-Repositorys, ein Azure-Repository oder Azure Storage. Falls Sie das Paket nicht öffentlich zugänglich machen möchten, können Sie ein [SAS-Token](../../../storage/common/storage-sas-overview.md) in die URL einfügen. Sie können auch einen [Dienstendpunkt](../../../storage/common/storage-network-security.md#grant-access-from-a-virtual-network) für Computer in einem privaten Netzwerk implementieren. Diese Konfiguration gilt aber nur für den Zugriff auf das Paket und nicht für die Kommunikation mit dem Dienst.

## <a name="step-by-step-creating-a-custom-guest-configuration-audit-policy-for-windows"></a>Schrittanleitung: Erstellen einer benutzerdefinierten Überwachungsrichtlinie für Gastkonfigurationen für Windows

Erstellen Sie eine DSC-Konfiguration zum Überwachen von Einstellungen. Im folgenden PowerShell-Beispielskript wird eine Konfiguration mit dem Namen **AuditBitLocker** erstellt, das Ressourcenmodul **PsDscResources** importiert und die Ressource `Service` verwendet, um eine Überwachung für einen ausgeführten Dienst durchzuführen. Das Konfigurationsskript kann von einem Windows- oder macOS-Computer aus ausgeführt werden.

```powershell
# Add PSDscResources module to environment
Install-Module 'PSDscResources'

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
AuditBitLocker
```

Führen Sie dieses Skript in einem PowerShell-Terminal aus, oder speichern Sie diese Datei mit dem Namen `config.ps1` im Projektordner. Führen Sie sie in PowerShell aus, indem Sie `./config.ps1` im Terminal ausführen. Eine neue MOF-Datei wird erstellt.

Der Befehl `Node AuditBitlocker` ist aus technischer Sicht nicht erforderlich, doch wird damit eine Datei namens `AuditBitlocker.mof` anstelle der Standarddatei `localhost.mof` erstellt. Wenn der Name der MOF-Datei der Konfiguration folgt, können beim Arbeiten in großem Umfang viele Dateien problemlos organisiert werden.

Nachdem die MOF-Datei kompiliert wurde, müssen die unterstützenden Dateien in einem Paket zusammengefasst werden. Das fertige Paket wird von der Gastkonfiguration verwendet, um die Azure Policy-Definitionen zu erstellen.

Mit dem Cmdlet `New-GuestConfigurationPackage` wird das Paket erstellt. Module, die für die Konfiguration erforderlich sind, müssen in `$Env:PSModulePath` verfügbar sein. Parameter des Cmdlets `New-GuestConfigurationPackage` beim Erstellen von Windows-Inhalten:

- **Name**: Name des Pakets mit der Gastkonfiguration.
- **Konfiguration:** Vollständiger Pfad für das kompilierte DSC-Konfigurationsdokument.
- **Pfad**: Pfad des Ausgabeordners. Dieser Parameter ist optional. Wenn er nicht angegeben ist, wird das Paket im aktuellen Verzeichnis erstellt.

Führen Sie den folgenden Befehl aus, um ein Paket mit der im vorherigen Schritt angegebenen Konfiguration zu erstellen:

```azurepowershell-interactive
New-GuestConfigurationPackage `
  -Name 'AuditBitlocker' `
  -Configuration './AuditBitlocker/AuditBitlocker.mof'
```

Nach dem Erstellen des Konfigurationspakets – und vor der Veröffentlichung in Azure – können Sie das Paket über Ihre Arbeitsstation oder CI/CD-Umgebung (Continuous Integration/Continuous Deployment) testen. Das GuestConfiguration-Cmdlet `Test-GuestConfigurationPackage` enthält denselben Agent in Ihrer Entwicklungsumgebung, der auch auf Azure-Computern genutzt wird. Mit dieser Lösung können Sie Integrationstests lokal durchführen, bevor die Veröffentlichung für kostenpflichtige Cloudumgebungen erfolgt.

Da der Agent tatsächlich die lokale Umgebung auswertet, müssen Sie in den meisten Fällen das Test-Cmdlet auf derselben Betriebssystemplattform ausführen, die Sie überwachen möchten. Für den Test werden nur im Inhaltspaket enthaltene Module verwendet.

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
New-GuestConfigurationPackage -Name AuditBitlocker -Configuration ./AuditBitlocker/AuditBitlocker.mof | Test-GuestConfigurationPackage
```

Im nächsten Schritt wird die Datei in Azure Blob Storage veröffentlicht. Es werden keine besonderen Anforderungen an das Speicherkonto gestellt, aber es ist ratsam, die Datei in einer Region in der Nähe Ihrer Computer zu hosten. Wenn Sie nicht über ein Speicherkonto verfügen, verwenden Sie das folgende Beispiel. Die folgenden Befehle, einschließlich `Publish-GuestConfigurationPackage`, erfordern das Modul `Az.Storage`.

```azurepowershell-interactive
# Creates a new resource group, storage account, and container
New-AzResourceGroup -name myResourceGroupName -Location WestUS
New-AzStorageAccount -ResourceGroupName myResourceGroupName -Name myStorageAccountName -SkuName 'Standard_LRS' -Location 'WestUs' | New-AzStorageContainer -Name guestconfiguration -Permission Blob
```

Parameter des Cmdlets `Publish-GuestConfigurationPackage`:

- **Pfad**: Speicherort des Pakets, das veröffentlicht werden soll
- **ResourceGroupName**: Name der Ressourcengruppe, in der sich das Speicherkonto befindet
- **StorageAccountName**: Der Name des Speicherkontos, in dem das Paket veröffentlicht werden soll
- **StorageContainerName:** (Standard: _guestconfiguration_) Name des Speichercontainers im Speicherkonto
- **Force:** Vorhandenes Paket im Speicherkonto mit demselben Namen überschreiben

Im nachfolgenden Beispiel wird das Paket im Speichercontainer „guestconfiguration“ veröffentlicht.

```azurepowershell-interactive
Publish-GuestConfigurationPackage -Path ./AuditBitlocker.zip -ResourceGroupName myResourceGroupName -StorageAccountName myStorageAccountName
```

Nachdem ein benutzerdefiniertes Richtlinienpaket für Gastkonfigurationen erstellt und hochgeladen wurde, erstellen Sie die Richtliniendefinition für Gastkonfigurationen. Das Cmdlet `New-GuestConfigurationPolicy` verwendet ein benutzerdefiniertes Richtlinienpaket und erstellt eine Richtliniendefinition.

Parameter des Cmdlets `New-GuestConfigurationPolicy`:

- **ContentUri**: Öffentlicher HTTP-/HTTPS-URI des Pakets mit dem Inhalt der Gastkonfiguration.
- **DisplayName**: Anzeigename der Richtlinie.
- **Beschreibung**: Beschreibung der Richtlinie.
- **Parameter**: Richtlinienparameter im Hashtabellenformat.
- **Version**: Version der Richtlinie.
- **Pfad**: Zielpfad, unter dem Richtliniendefinitionen erstellt werden.
- **Plattform**: Zielplattform (Windows/Linux) für das Paket mit den Richtlinien und dem Inhalt der Gastkonfiguration.
- Mit **Tag** werden der Richtliniendefinition ein oder mehrere Tags hinzugefügt.
- Mit **Category** wird das Feld mit den Kategoriemetadaten in der Richtliniendefinition festgelegt.

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

In der Ausgabe des Cmdlets wird ein Objekt zurückgegeben, das den Anzeigenamen der Initiative und den Pfad der Richtliniendateien enthält.

Abschließend veröffentlichen Sie die Richtliniendefinitionen mit dem Cmdlet `Publish-GuestConfigurationPolicy`. Das Cmdlet verfügt nur über den Parameter **Path**, mit dem auf den Speicherort der JSON-Dateien verwiesen wird, die mit `New-GuestConfigurationPolicy` erstellt werden.

Um den Veröffentlichungsbefehl auszuführen, benötigen Sie Zugriff zum Erstellen von Richtlinien in Azure. Die entsprechenden Autorisierungsanforderungen sind auf der Seite mit der [Übersicht über Azure Policy](../overview.md) dokumentiert. Die beste integrierte Rolle ist **Mitwirkender bei Ressourcenrichtlinien**.

```azurepowershell-interactive
Publish-GuestConfigurationPolicy -Path '.\policies'
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

Bei der in Azure erstellten Richtlinie ist der letzte Schritt das Zuweisen der Definition. Informieren Sie sich darüber, wie Sie die Definition über das [Portal](../assign-policy-portal.md), die [Azure CLI](../assign-policy-azurecli.md) oder über [Azure PowerShell](../assign-policy-powershell.md) zuweisen können.

### <a name="filtering-guest-configuration-policies-using-tags"></a>Filtern von Richtlinien der Gastkonfiguration mit Tags

Die mit Cmdlets im Gastkonfigurationsmodul erstellten Richtliniendefinitionen können optional einen Filter für Tags enthalten. Der Parameter **Tag** von `New-GuestConfigurationPolicy` unterstützt ein Array mit Hashtabellen, die die einzelnen Tageinträge enthalten. Die Tags werden dem Abschnitt `If` der Richtliniendefinition hinzugefügt und können nicht per Richtlinienzuweisung geändert werden.

Unten ist ein Beispielcodeausschnitt einer Richtliniendefinition angegeben, mit der nach Tags gefiltert wird.

```json
"if": {
  "allOf" : [
    {
      "allOf": [
        {
          "field": "tags.Owner",
          "equals": "BusinessUnit"
        },
        {
          "field": "tags.Role",
          "equals": "Web"
        }
      ]
    },
    {
      // Original Guest Configuration content
    }
  ]
}
```

### <a name="using-parameters-in-custom-guest-configuration-policy-definitions"></a>Verwenden von Parametern in benutzerdefinierten Richtliniendefinitionen für Gastkonfigurationen

Die Gastkonfiguration unterstützt das Außerkraftsetzen von Eigenschaften einer Konfiguration während der Laufzeit. Dieses Feature bewirkt, dass die Werte in der MOF-Datei im Paket nicht als statisch angesehen werden müssen. Die Überschreibungswerte werden über Azure Policy bereitgestellt und ändern nicht, wie die Konfigurationen erstellt oder kompiliert werden.

Die Cmdlets `New-GuestConfigurationPolicy` und `Test-GuestConfigurationPolicyPackage` enthalten einen Parameter mit dem Namen **Parameter**. Für diesen Parameter wird eine Hashtabellendefinition verwendet, die alle Details zu den einzelnen Parametern enthält, und es werden die erforderlichen Abschnitte der einzelnen Dateien erstellt, die für die Azure Policy-Definition verwendet werden.

Im folgenden Beispiel wird eine Richtliniendefinition zum Überwachen eines Diensts erstellt, bei der der Benutzer bei der Zuweisung der Richtlinie eine Auswahl aus einer Liste trifft.

```azurepowershell-interactive
# This DSC Resource text:
Service 'UserSelectedNameExample'
      {
          Name = 'ParameterValue'
          Ensure = 'Present'
          State = 'Running'
      }

# Would require the following hashtable:
$PolicyParameterInfo = @(
    @{
        Name = 'ServiceName'                                            # Policy parameter name (mandatory)
        DisplayName = 'windows service name.'                           # Policy parameter display name (mandatory)
        Description = "Name of the windows service to be audited."      # Policy parameter description (optional)
        ResourceType = "Service"                                        # DSC configuration resource type (mandatory)
        ResourceId = 'UserSelectedNameExample'                          # DSC configuration resource id (mandatory)
        ResourcePropertyName = "Name"                                   # DSC configuration resource property name (mandatory)
        DefaultValue = 'winrm'                                          # Policy parameter default value (optional)
        AllowedValues = @('BDESVC','TermService','wuauserv','winrm')    # Policy parameter allowed values (optional)
    }
)

New-GuestConfigurationPolicy
    -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditBitLocker.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
    -DisplayName 'Audit Windows Service.' `
    -Description 'Audit if a Windows Service is not enabled on Windows machine.' `
    -Path '.\policies' `
    -Parameter $PolicyParameterInfo `
    -Version 1.0.0
```

## <a name="extending-guest-configuration-with-third-party-tools"></a>Erweitern der Gastkonfiguration mit Drittanbietertools

Die Artefaktpakete für die Gastkonfiguration können um Drittanbietertools erweitert werden.
Für die Erweiterung der Gastkonfiguration müssen zwei Komponenten entwickelt werden.

- Eine Desired State Configuration-Ressource (DSC) zum Verarbeiten aller Verwaltungsaktivitäten für das Drittanbietertool
  - Installieren
  - Invoke
  - Ausgabe konvertieren
- Inhalt im richtigen Format für die native Nutzung durch das Tool

Für die DSC-Ressource muss eine benutzerdefinierte Entwicklung durchgeführt werden, falls noch keine Communitylösung vorhanden ist.
Sie können überprüfen, ob Communitylösungen verfügbar sind, indem Sie im PowerShell-Katalog nach dem Tag [GuestConfiguration](https://www.powershellgallery.com/packages?q=Tags%3A%22GuestConfiguration%22) suchen.

> [!NOTE]
> Für die Erweiterbarkeit der Gastkonfiguration gilt das Szenario „BYOL (Bring Your Own License)“. Stellen Sie sicher, dass Sie die für Drittanbietertools geltenden Bedingungen erfüllt haben, bevor Sie diese verwenden.

Nachdem die DSC-Ressource in der Entwicklungsumgebung installiert wurde, sollten Sie den Parameter **FilesToInclude** für `New-GuestConfigurationPackage` verwenden, um den Inhalt für die Drittanbieterplattform in das Inhaltsartefakt einzubinden.

## <a name="policy-lifecycle"></a>Lebenszyklus von Richtlinien

Wenn Sie ein Update für die Richtlinie freigeben möchten, ändern Sie die Details für das Gastkonfigurationspaket und die Azure Policy-Definition.

> [!NOTE]
> Die Eigenschaft `version` der Gastkonfigurationszuweisung wirkt sich nur auf Pakete aus, die von Microsoft gehostet werden. Bei der Versionsverwaltung für benutzerdefinierte Inhalte hat sich die Best Practice etabliert, die Version in den Dateinamen aufzunehmen.

Geben Sie zunächst beim Ausführen von `New-GuestConfigurationPackage` einen Namen für das Paket an, der es gegenüber früheren Versionen eindeutig kennzeichnet. Sie können z. B. eine Versionsnummer in den Namen einschließen wie in `PackageName_1.0.0`. Die Zahl in diesem Beispiel dient nur dazu, das Paket eindeutig zu machen, und nicht dazu, das Paket als neuer oder älter als andere Pakete zu kennzeichnen.

Aktualisieren Sie als anschließend die Parameter für das Cmdlet `New-GuestConfigurationPolicy` gemäß den folgenden Erläuterungen.

- **Version**: Beim Ausführen des Cmdlets `New-GuestConfigurationPolicy` müssen Sie eine Versionsnummer angeben, die höher als die der derzeitigen Veröffentlichung ist.
- **contentUri**: Wenn Sie das Cmdlet `New-GuestConfigurationPolicy` ausführen, müssen Sie einen URI zum Speicherort des Pakets angeben. Durch Einschließen einer Paketversion in den Dateinamen wird sichergestellt, dass sich der Wert dieser Eigenschaft in jedem Release ändert.
- **contentHash**: Diese Eigenschaft wird vom Cmdlet `New-GuestConfigurationPolicy` automatisch aktualisiert. Es handelt sich um einen Hashwert des Pakets, das mit `New-GuestConfigurationPackage` erstellt wurde. Diese Eigenschaft muss für die von Ihnen veröffentlichte Datei vom Typ `.zip` stimmen. Wenn nur die Eigenschaft **contentUri** aktualisiert wird, akzeptiert die Erweiterung das Inhaltspaket nicht.

Die einfachste Möglichkeit zum Freigeben eines aktualisierten Pakets ist das Wiederholen des Prozesses in diesem Artikel und das Angeben einer aktualisierten Versionsnummer. Mit dieser Vorgehensweise wird sichergestellt, dass alle Eigenschaften richtig aktualisiert wurden.

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

Vom GuestConfiguration-Agent wird erwartet, dass der öffentliche Schlüssel des Zertifikats auf Windows-Computern unter „Vertrauenswürdige Stammzertifizierungsstellen“ und auf Linux-Computern unter dem Pfad `/usr/local/share/ca-certificates/extra` vorhanden ist. Damit auf dem Knoten signierter Inhalt überprüft werden kann, müssen Sie den öffentlichen Schlüssel des Zertifikats auf dem Computer installieren, bevor Sie die benutzerdefinierte Richtlinie anwenden. Dieser Prozess kann mit einem beliebigen Verfahren auf der VM oder mit Azure Policy durchgeführt werden. Eine Beispielvorlage finden Sie [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.compute/vm-push-certificate-windows).
In der Key Vault-Zugriffsrichtlinie muss es für den Computeressourcenanbieter zulässig sein, bei Bereitstellungen auf Zertifikate zuzugreifen. Informationen zu den ausführlichen Schritten finden Sie unter [Einrichten des Schlüsseltresors für virtuelle Computer in Azure Resource Manager](../../../virtual-machines/windows/key-vault-setup.md#use-templates-to-set-up-key-vault).

Hier ist ein Beispiel für den Export des öffentlichen Schlüssels aus einem Signaturzertifikat angegeben, um ihn dann auf dem Computer importieren zu können.

```azurepowershell-interactive
$Cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object {($_.Subject-eq "CN=mycert3") } | Select-Object -First 1
$Cert | Export-Certificate -FilePath "$env:temp\DscPublicKey.cer" -Force
```

Fügen Sie nach dem Veröffentlichen Ihres Inhalts ein Tag mit dem Namen `GuestConfigPolicyCertificateValidation` und dem Wert `enabled` an alle virtuellen Computer an, für die das Codesignieren erforderlich ist. Informationen darüber, wie Tags mithilfe von Azure Policy im großen Stil bereitgestellt werden können, finden Sie in den [Tagbeispielen](../samples/built-in-policies.md#tags). Wenn dieses Tag vorhanden ist, ermöglicht die Richtliniendefinition, die mit dem Cmdlet `New-GuestConfigurationPolicy` generiert wurde, die Anforderung über die Gastkonfigurationserweiterung.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die Überprüfung von VMs mit [Gastkonfiguration](../concepts/guest-configuration.md).
- Informieren Sie sich über das [programmgesteuerte Erstellen von Richtlinien](./programmatically-create.md).
- Informieren Sie sich über das [Abrufen von Konformitätsdaten](./get-compliance-data.md).
