---
title: Speichern eines mehrzeiligen Geheimnisses in Azure Key Vault
description: In diesem Tutorial erfahren Sie, wie Sie mehrzeilige Geheimnisse aus Azure Key Vault mithilfe der Azure CLI und per PowerShell festlegen.
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.custom: mvc, seo-javascript-september2019, seo-javascript-october2019, devx-track-azurecli
ms.date: 03/17/2021
ms.author: mbaldwin
ms.openlocfilehash: e320795d5e8623dea9b97ac6371a0ffc6e6117f1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104608544"
---
# <a name="store-a-multi-line-secret-in-azure-key-vault"></a>Speichern eines mehrzeiligen Geheimnisses in Azure Key Vault

In den Schnellstartanleitungen für die [Azure CLI](quick-create-cli.md) und für [Azure PowerShell](quick-create-powershell.md) wird das Speichern eines einzeiligen Geheimnisses gezeigt.   Key Vault kann aber auch zum Speichern mehrzeiliger Geheimnisse verwendet werden (beispielsweise eine JSON-Datei oder ein privater RSA-Schlüssel).

Mehrzeilige Geheimnisse können nicht über die Befehlszeile an den Azure CLI-Befehl [az keyvault secret set](/cli/azure/keyvault/secret#az_keyvault_secret_set) oder an das Azure PowerShell-Cmdlet [Set-AzKeyVaultSecret](/powershell/module/az.keyvault/set-azkeyvaultsecret) übergeben werden. Stattdessen muss das mehrzeilige Geheimnis als Textdatei gespeichert werden. 

Sie können beispielsweise eine Textdatei namens „secretfile.txt“ erstellen, die folgende Zeilen enthält:

```bash
This is my
multi-line
secret
```

Diese Datei können Sie dann unter Verwendung des Parameters `--file` an den Azure CLI-Befehl [az keyvault secret set](/cli/azure/keyvault/secret#az_keyvault_secret_set) übergeben.

```azurecli-interactive
az keyvault secret set --vault-name "<your-unique-keyvault-name>" --name "MultilineSecret" --file "secretfile.txt"
```

Wenn Sie Azure PowerShell verwenden, muss die Datei zunächst mithilfe des Cmdlets [Get-Content](/powershell/module/microsoft.powershell.management/get-content) gelesen und anschließend mithilfe von [ConvertTo-SecureString](/powershell/module/microsoft.powershell.security/convertto-securestring) in eine sichere Zeichenfolge konvertiert werden. 

```azurepowershell-interactive
$RawSecret =  Get-Content "secretfile.txt" -Raw
$SecureSecret = ConvertTo-SecureString -String $RawSecret -AsPlainText -Force
```

Abschließend muss das Geheimnis mithilfe des Cmdlets [Set-AzKeyVaultSecret](/powershell/module/az.keyvault/set-azkeyvaultsecret) gespeichert werden.

```azurepowershell-interactive
$secret = Set-AzKeyVaultSecret -VaultName "<your-unique-keyvault-name>" -Name "MultilineSecret" -SecretValue $SecureSecret
```

In beiden Fällen können Sie das gespeicherte Geheimnis mithilfe des Azure CLI-Befehls [az keyvault secret show](/cli/azure/keyvault/secret#az_keyvault_secret_show) oder mithilfe des Azure PowerShell-Cmdlets [Get-AzKeyVaultSecret](/powershell/module/az.keyvault/get-azkeyvaultsecret) anzeigen.

```azurecli-interactive
az keyvault secret show --name "MultilineSecret" --vault-name "<your-unique-keyvault-name>" --query "value"
```

Das Geheimnis wird mit eingebetteten Zeilenumbrüchen zurückgegeben:

```bash
"This is\nmy multi-line\nsecret"
```

## <a name="next-steps"></a>Nächste Schritte

- [Was ist der Azure-Schlüsseltresor?](../general/overview.md)
- [Schnellstartanleitung für die Azure CLI](quick-create-cli.md)
- [Azure CLI-Befehle vom Typ „az keyvault“](/cli/azure/keyvault)
- [Schnellstartanleitung für Azure PowerShell](quick-create-powershell.md)
- [Azure PowerShell-Cmdlets vom Typ „Az.KeyVault“](/powershell/module/az.keyvault#key-vault)
