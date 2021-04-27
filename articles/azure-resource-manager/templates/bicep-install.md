---
title: Richten Sie die Bicep-Entwicklungs- und Einsatzumgebungen ein
description: Konfigurieren von Bicep-Entwicklungs- und -Bereitstellungsumgebungen
ms.topic: conceptual
ms.date: 03/26/2021
ms.openlocfilehash: 2c905a3885fcfc5c9eb9d9db4004126882798611
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107313554"
---
# <a name="install-bicep-preview"></a>Installieren Sie den BICEP (Vorschau)

Erfahren Sie, wie Sie Bicep-Entwicklungs- und -Bereitstellungsumgebungen einrichten.

## <a name="development-environment"></a>Entwicklungsumgebung

Für eine optimale Entwicklung mit Bicep benötigen Sie zwei Komponenten:

- **Bicep-Erweiterung für Visual Studio Code:** Zum Erstellen von Bicep-Dateien benötigen Sie einen guten Bicep-Editor. Wir empfehlen [Visual Studio Code](https://code.visualstudio.com/) mit der [Bicep-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-bicep). Diese Tools bieten Sprachunterstützung und die automatische Vervollständigung von Ressourcen. Sie unterstützen Sie beim Erstellen und Überprüfen von Bicep-Dateien. Weitere Informationen zur Verwendung von Visual Studio Code und der BICEP-Erweiterung finden Sie unter [Schnellstart: Erstellen von BICEP-Dateien mit Visual Studio Code](./quickstart-create-bicep-use-visual-studio-code.md).
- **Bicep-Befehlszeilenschnittstelle:** Sie verwenden die Bicep-Befehlszeilenschnittstelle, um Bicep-Dateien in ARM-JSON-Vorlagen zu kompilieren und ARM-JSON-Vorlagen zurück in Bicep-Dateien zu dekompilieren. Anweisungen zur Installation finden Sie unter [Installieren von Bicep CLI](#install-manually).

## <a name="deployment-environment"></a>Bereitstellungsumgebung

Zum Bereitstellen von lokalen BICEP-Dateien benötigen Sie zwei Komponenten:

- **Azure CLI Version 2.20.0 oder höher oder Azure PowerShell Version 5.6.0 oder** höher. Die Installationsanweisungen finden Sie unter:

  - [Installieren von Azure PowerShell](/powershell/azure/install-az-ps)
  - [Installieren der Azure CLI unter Windows](/cli/azure/install-azure-cli-windows)
  - [Installieren der Azure CLI unter Linux](/cli/azure/install-azure-cli-linux)
  - [Installieren der Azure CLI unter macOS](/cli/azure/install-azure-cli-macos)

  > [!NOTE]
  > Derzeit können sowohl die Azure-Befehlszeilenschnittstelle als auch Azure PowerShell nur lokale Bicep-Dateien bereitstellen. Weitere Informationen zum Bereitstellen von Bicep-Dateien mithilfe der Azure-Befehlszeilenschnittstelle finden Sie unter [Bereitstellen: Befehlszeilenschnittstelle](./deploy-cli.md#deploy-remote-template). Weitere Informationen zum Bereitstellen von Bicep-Dateien mithilfe von Azure PowerShell finden Sie unter [Bereitstellen: PowerShell]( ./deploy-powershell.md#deploy-remote-template).

- **Bicep-Befehlszeilenschnittstelle:** Die BICEP-CLI ist erforderlich, um BICEP-Dateien vor der Bereitstellung in JSON-Vorlagen zu kompilieren. Anweisungen zur Installation finden Sie unter [Installieren von Bicep CLI](#install-bicep-cli).

Nachdem die Komponenten installiert wurden, können Sie eine BICEP-Datei bereitstellen mit:

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name ExampleDeployment `
  -ResourceGroupName ExampleGroup `
  -TemplateFile <path-to-template-or-bicep> `
  -storageAccountType Standard_GRS
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file <path-to-template-or-bicep> \
  --parameters storageAccountType=Standard_GRS
```

---

## <a name="install-bicep-cli"></a>Installieren der Bicep-Befehlszeilenschnittstelle

- Informationen zum Verwenden der BICEP-CLI zum Kompilieren und dekompilieren von BICEP-Dateien finden Sie unter [Manuell installieren](#install-manually).
- Informationen zum Verwenden von Azure CLI zum Bereitstellen von BICEP-Dateien finden Sie unter [Mit Azure CLI verwenden](#use-with-azure-cli).
- Informationen zum Verwenden von Azure PowerShell zum Bereitstellen von BICEP-Dateien finden Sie unter [Verwendung mit Azure PowerShell](#use-with-azure-powershell).

### <a name="use-with-azure-cli"></a>Mit der Azure-CLI verwenden

Wenn Azure CLI Version 2.20.0 oder höher installiert ist, wird die Bicep CLI automatisch installiert, wenn ein Befehl ausgeführt wird, der davon abhängt. Beispiel:

```azurecli
az deployment group create --template-file azuredeploy.bicep --resource-group myResourceGroup
```

oder

```azurecli
az bicep ...
```

Sie können die Befehlszeilenschnittstelle auch manuell mithilfe integrierter Befehle installieren:

```bash
az bicep install
```

So führen Sie ein Upgrade auf die aktuelle Version aus

```bash
az bicep upgrade
```

So installieren Sie eine bestimmte Version

```bash
az bicep install --version v0.3.255
```

> [!IMPORTANT]
> Die Azure CLI installiert eine separate Version von Bicep CLI, die nicht mit anderen Bicep-Installationen, die Sie möglicherweise haben, konfliktiert, und Azure CLI fügt Bicep CLI nicht zu Ihrem PATH hinzu. Zur Verwendung der Bicep CLI zum Kompilieren/Dekompilieren von Bicep-Dateien oder zur Verwendung von Azure PowerShell zur Bereitstellung von Bicep-Dateien finden Sie unter [Manuelle Installation](#install-manually) oder [die Verwendung mit Azure Powershell](#use-with-azure-powershell).

So listen Sie alle verfügbaren Versionen der Bicep-Befehlszeilenschnittstelle auf

```bash
az bicep list-versions
```

So zeigen Sie die installierten Versionen an

```bash
az bicep version
```

### <a name="use-with-azure-powershell"></a>Verwendung mit Azure PowerShell

Azure PowerShell bietet noch keine Funktionen für das Installieren der Bicep-Befehlszeilenschnittstelle. Bei Azure PowerShell ab Version 5.6.0 können Sie davon ausgehen, dass die Bicep-Befehlszeilenschnittstelle bereits installiert und über die Umgebungsvariable für den Pfad verfügbar ist. Befolgen Sie eine der [manuellen Installationsmethoden](#install-manually).

Zum Bereitstellen von Bicep-Dateien ist Bicep CLI Version 0.3.1 oder höher erforderlich. So überprüfen Sie die Version der Bicep-CLI:

```cmd
bicep --version
```

> [!IMPORTANT]
> Azure CLI installiert eine eigene eigenständige Version der Bicep-CLI. Die Azure PowerShell Bereitstellung schlägt auch dann fehl, wenn die erforderlichen Versionen für den Azure CLI installiert sind.

Wenn die Bicep-Befehlszeilenschnittstelle installiert ist, wird sie immer dann aufgerufen, wenn sie für ein Bereitstellungs-Cmdlet benötigt wird. Beispiel:

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName myResourceGroup -TemplateFile azuredeploy.bicep
```

### <a name="install-manually"></a>Manuelle Installation

Mit den folgenden Methoden wird die Bicep-Befehlszeilenschnittstelle installiert und Ihrem Pfad hinzugefügt.

#### <a name="linux"></a>Linux

```sh
# Fetch the latest Bicep CLI binary
curl -Lo bicep https://github.com/Azure/bicep/releases/latest/download/bicep-linux-x64
# Mark it as executable
chmod +x ./bicep
# Add bicep to your PATH (requires admin)
sudo mv ./bicep /usr/local/bin/bicep
# Verify you can now access the 'bicep' command
bicep --help
# Done!

```

#### <a name="macos"></a>macOS

##### <a name="via-homebrew"></a>Über Homebrew

```sh
# Add the tap for bicep
brew tap azure/bicep

# Install the tool
brew install bicep
```

##### <a name="macos-manual-install"></a>Manuelle Installation unter macOS

```sh
# Fetch the latest Bicep CLI binary
curl -Lo bicep https://github.com/Azure/bicep/releases/latest/download/bicep-osx-x64
# Mark it as executable
chmod +x ./bicep
# Add Gatekeeper exception (requires admin)
sudo spctl --add ./bicep
# Add bicep to your PATH (requires admin)
sudo mv ./bicep /usr/local/bin/bicep
# Verify you can now access the 'bicep' command
bicep --help
# Done!

```

#### <a name="windows"></a>Windows

##### <a name="windows-installer"></a>Windows Installer

Laden Sie das [neueste Windows-Installationsprogramm](https://github.com/Azure/bicep/releases/latest/download/bicep-setup-win-x64.exe) herunter, und führen Sie es aus. Für das Installationsprogramm sind keine Administratorrechte erforderlich. Nach der Installation wird die Bicep-Befehlszeilenschnittstelle dem Benutzerpfad hinzugefügt. Schließen Sie alle geöffneten Befehlsshellfenster, und öffnen Sie sie erneut, damit die Änderung am Pfad wirksam wird.

##### <a name="chocolatey"></a>Chocolatey

```powershell
choco install bicep
```

##### <a name="winget"></a>Winget

```powershell
winget install -e --id Microsoft.Bicep
```

##### <a name="manual-with-powershell"></a>Manuell mit PowerShell

```powershell
# Create the install folder
$installPath = "$env:USERPROFILE\.bicep"
$installDir = New-Item -ItemType Directory -Path $installPath -Force
$installDir.Attributes += 'Hidden'
# Fetch the latest Bicep CLI binary
(New-Object Net.WebClient).DownloadFile("https://github.com/Azure/bicep/releases/latest/download/bicep-win-x64.exe", "$installPath\bicep.exe")
# Add bicep to your PATH
$currentPath = (Get-Item -path "HKCU:\Environment" ).GetValue('Path', '', 'DoNotExpandEnvironmentNames')
if (-not $currentPath.Contains("%USERPROFILE%\.bicep")) { setx PATH ($currentPath + ";%USERPROFILE%\.bicep") }
if (-not $env:path.Contains($installPath)) { $env:path += ";$installPath" }
# Verify you can now access the 'bicep' command.
bicep --help
# Done!
```

## <a name="install-the-nightly-builds"></a>Installieren der nächtlichen Builds

Wenn Sie die neuesten Vorabversionen von Bicep testen möchten, bevor sie veröffentlicht werden, finden Sie unter [Installieren von nächtlichen Builds](https://github.com/Azure/bicep/blob/main/docs/installing-nightly.md) weitere Informationen.

> [!WARNING]
> Diese Vorabversionen weisen sehr wahrscheinlich bekannte oder unbekannte Fehler auf.

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie mit dem [Bicep-Schnellstart](./quickstart-create-bicep-use-visual-studio-code.md).
