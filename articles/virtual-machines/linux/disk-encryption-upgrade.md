---
title: Upgrade von Azure Disk Encryption auf einem Datenträger
description: Dieser Artikel enthält Anweisungen zum Upgrade von Azure Disk Encryption auf einem Datenträger.
author: msmbaldwin
ms.service: virtual-machines
ms.subservice: disks
ms.topic: how-to
ms.author: mbaldwin
ms.date: 05/27/2021
ms.custom: seodec18, devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 0cdc32e303de7d9a26a99d1f08188dc54539930b
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111758241"
---
# <a name="upgrading-the-azure-disk-encryption-version"></a>Upgrade der Azure Disk Encryption-Version

Anders als die erste Version von Azure Disk Encryption (ADE) verwendet die aktuelle Version nicht mehr Azure Active Directory (AAD) zur Authentifizierung.  Wir empfehlen dringend die Verwendung der neuesten Version.

## <a name="determine-ade-version"></a>Bestimmen der ADE-Version

Die für die Migration relevanten Versionen von ADE sind:
- **Windows**: 1.1.* (ADE auf der VM muss auf 2.2 aktualisiert werden)
- **Linux**: 0.1.* (ADE auf der VM muss auf 1.2 aktualisiert werden)

Sie können die Version von ADE, mit der eine VM verschlüsselt wurde, mithilfe von Azure CLI, Azure PowerShell oder dem Azure-Portal ermitteln.

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/CLI)

Um die ADE-Version zu ermitteln, führen Sie den Azure CLI-Befehl [az vm get-instance-view](/cli/azure/vm#az_vm_get_instance_view) aus.

```azurecli-interactive
az vm get-instance-view --resource-group  <ResourceGroupName> --name <VMName> 
```

Suchen Sie die Erweiterung „AzureDiskEncryption“ in der Ausgabe und ermitteln Sie die Versionsnummer im Ausgabefeld „TypeHandlerVersion“.

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

Um die ADE-Version zu ermitteln, führen Sie den Azure PowerShell-Befehl [Get-AzVM](/powershell/module/az.compute/get-azvm) aus.

```azurepowershell-interactive
Get-AzVM -ResourceGroupName <ResourceGroupName> -Name <VMName> -Status
```

Suchen Sie die Erweiterung „AzureDiskEncryption“ in der Ausgabe und ermitteln Sie die Versionsnummer im Ausgabefeld „TypeHandlerVersion“.

# <a name="portal"></a>[Portal](#tab/Portal)

Wechseln Sie im Azure-Portal für Ihre VM zum Blatt „Erweiterungen“.

:::image type="content" source="../media/disk-encryption/ade-version-1.png" alt-text="Screenshot des Portals: Suchen der ADE-Version 1":::

Wählen Sie die Erweiterung „AzureDiskEncryption“ für Windows oder die Erweiterung „AzureDiskEncryptionForLinux“ für Linux aus, und suchen Sie die Versionsnummer im Feld „Version“.

:::image type="content" source="../media/disk-encryption/ade-version-2.png" alt-text="Screenshot des Portals: Suchen der ADE-Version 2":::

---

## <a name="how-to-migrate"></a>Vorgehensweise zum Migrieren

Die Migration von Azure Disk Encryption (mit AAD) zu Azure Disk Encryption (ohne AAD) ist nur über Azure PowerShell möglich. Stellen Sie sicher, dass die neueste Version von Azure PowerShell und mindestens [Azure PowerShell Az Modulversion 5.9.0](/powershell/azure/new-azureps-module-az) installiert ist.

Um ein Upgrade von Azure Disk Encryption (mit AAD) auf Azure Disk Encryption (ohne AAD) auszuführen, verwenden Sie das PowerShell-Cmdlet [Set-AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension). 

> [!WARNING]
> Das Cmdlet Set-AzVMDiskEncryptionExtension darf nur auf VMs verwendet werden, die mit Azure Disk Encryption (mit AAD) verschlüsselt sind. Der Versuch, eine unverschlüsselte VM oder eine mit Azure Disk Encryption (ohne AAD) verschlüsselte VM zu migrieren, führt zu einem Terminalfehler.

```azurepowershell-interactive
Set-AzVMDiskEncryptionExtension -ResourceGroupName <resourceGroupName> -VMName <vmName> -Migrate
```

Wenn Sie vom Cmdlet zur Bestätigung aufgefordert werden, geben Sie „Y“ ein.  Die ADE-Version wird aktualisiert und die VM neu gestartet. Die Ausgabe ist mit folgender Zeichenfolge vergleichbar:

```bash
> Set-AzVMDiskEncryptionExtension -ResourceGroupName myResourceGroup -VMName myVM -Migrate

Update AzureDiskEncryption version?
This cmdlet updates Azure Disk Encryption version to single pass (Azure Disk Encryption without AAD). This may reboot
the machine and takes 10-15 minutes to finish. Are you sure you want to continue?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
Azure Disk Encryption Extension Public Settings
"KeyVaultResourceId": /subscriptions/ea500758-3163-4849-bd2c-3e50f06efa7a/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault
"SequenceVersion":
"MigrateFlag": Migrate
"KeyVaultURL": https://myKeyVault.vault.azure.net/
"AADClientID": d29edf8c-3fcb-42e7-8410-9e39fdf0dd70
"KeyEncryptionKeyURL":
"KekVaultResourceId":
"EncryptionOperation": EnableEncryption
"AADClientCertThumbprint":
"VolumeType":
"KeyEncryptionAlgorithm":

Running ADE extension (with AAD) for -Migrate..
ADE extension (with AAD) is now complete. Updating VM model..
Running ADE extension (without AAD) for -Migrate..
ADE extension (without AAD) is now complete. Clearing VM model..

RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK
```


> [!IMPORTANT]
> Das Upgrade dauert mindestens 10 bis 15 Minuten. Brechen Sie das Cmdlet nicht ab, während das Upgrade ausgeführt wird. Dies würde die Integrität der VM gefährden.

## <a name="next-steps"></a>Nächste Schritte

- [Azure Disk Encryption-Problembehandlung](disk-encryption-troubleshooting.md)
