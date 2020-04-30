---
title: Verwenden von PowerShell zum Konfigurieren von kundenseitig verwalteten Schlüsseln
titleSuffix: Azure Storage
description: Erfahren Sie, wie Sie PowerShell verwenden, um von Kunden verwaltete Schlüssel für die Azure Storage-Verschlüsselung zu konfigurieren.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 04/02/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: bfc2e256396904456a7ee0fd8b6173c00a5f53d7
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81456395"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-by-using-powershell"></a>Konfigurieren von kundenseitig verwalteten Schlüsseln mit Azure Key Vault mithilfe von PowerShell

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

In diesem Artikel wird beschrieben, wie Sie eine Azure Key Vault-Instanz mit von Kunden verwalteten Schlüsseln mithilfe von PowerShell konfigurieren. Informationen zum Erstellen eines Schlüsseltresors über die Azure-Befehlszeilenschnittstelle finden Sie unter [Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe von PowerShell](../../key-vault/secrets/quick-create-powershell.md).

## <a name="assign-an-identity-to-the-storage-account"></a>Zuweisen einer Identität zum Speicherkonto

Sie müssen zunächst eine systemseitig zugewiesene verwaltete Identität zum Speicherkonto hinzufügen, um von Kunden verwaltete Schlüssel für Ihr Speicherkonto zu aktivieren. Sie verwenden diese Identität, um dem Speicherkonto die Zugriffsberechtigungen für den Schlüsseltresor zu gewähren.

Rufen Sie für das Zuweisen einer verwalteten Identität mithilfe von PowerShell [Set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) auf. Denken Sie daran, die Platzhalterwerte in den spitzen Klammern durch Ihre eigenen Werte zu ersetzen.

```powershell
$storageAccount = Set-AzStorageAccount -ResourceGroupName <resource_group> `
    -Name <storage-account> `
    -AssignIdentity
```

Weitere Informationen zur Konfiguration systemseitig zugewiesener verwalteter Identitäten mit PowerShell finden Sie unter [Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer mithilfe von PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md).

## <a name="create-a-new-key-vault"></a>Erstellen eines neuen Schlüsseltresors

Rufen Sie zum Erstellen eines neuen Schlüsseltresors mit PowerShell [New-AzKeyVault](/powershell/module/az.keyvault/new-azkeyvault) auf. Für den Schlüsseltresor, den Sie zum Speichern von Kunden verwalteter Schlüssel für die Azure Storage-Verschlüsselung verwenden, müssen zwei wichtige Schutzeinstellungen aktiviert sein: **Vorläufiges Löschen** und **Do Not Purge** (Nicht bereinigen).

Denken Sie daran, die Platzhalterwerte in den spitzen Klammern durch Ihre eigenen Werte zu ersetzen.

```powershell
$keyVault = New-AzKeyVault -Name <key-vault> `
    -ResourceGroupName <resource_group> `
    -Location <location> `
    -EnableSoftDelete `
    -EnablePurgeProtection
```

Informationen zum Aktivieren von **Vorläufiges Löschen** und **Nicht bereinigen** in einem vorhandenen Schlüsseltresor mit PowerShell finden Sie in den Abschnitten **Aktivieren des vorläufigen Löschens** und **Aktivieren des Bereinigungsschutzes** unter [Verwenden des vorläufigen Löschens mit PowerShell](../../key-vault/general/soft-delete-powershell.md).

## <a name="configure-the-key-vault-access-policy"></a>Konfigurieren der Zugriffsrichtlinie für den Schlüsseltresor

Konfigurieren Sie als Nächstes die Zugriffsrichtlinie für den Schlüsseltresor, sodass das Speicherkonto über die Berechtigungen für den Zugriff auf diesen verfügt. In diesem Schritt verwenden Sie die verwaltete Identität, die Sie zuvor dem Speicherkonto zugewiesen haben.

Rufen Sie zum Festlegen der Zugriffsrichtlinie für den Schlüsseltresor [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) auf. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen und die in den vorherigen Beispielen definierten Variablen zu verwenden.

```powershell
Set-AzKeyVaultAccessPolicy `
    -VaultName $keyVault.VaultName `
    -ObjectId $storageAccount.Identity.PrincipalId `
    -PermissionsToKeys wrapkey,unwrapkey,get
```

## <a name="create-a-new-key"></a>Erstellen eines neuen Schlüssels

Als Nächstes erstellen Sie einen Schlüssel im Schlüsseltresor. Rufen Sie zum Erstellen eines neuen Schlüssels [Add-AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey) auf. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen und die in den vorherigen Beispielen definierten Variablen zu verwenden.

```powershell
$key = Add-AzKeyVaultKey -VaultName $keyVault.VaultName -Name <key> -Destination 'Software'
```

Für die Azure Storage-Verschlüsselung werden nur 2048-Bit-RSA- und RSA-HSM-Schlüssel unterstützt. Weitere Informationen zu Schlüsseln finden Sie unter **Key Vault-Schlüssel** in [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten in Azure Key Vault](../../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys).

## <a name="configure-encryption-with-customer-managed-keys"></a>Konfigurieren der Verschlüsselung mit von Kunden verwalteten Schlüsseln

Die Azure Storage-Verschlüsselung verwendet standardmäßig von Microsoft verwaltete Schlüssel. In diesem Schritt konfigurieren Sie Ihr Azure Storage-Konto für von Kunden verwaltete Schlüssel und legen den Schlüssel fest, der dem Speicherkonto zugeordnet werden soll.

Rufen Sie [Set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) wie im folgenden Beispiel gezeigt auf, um die Verschlüsselungseinstellungen des Speicherkontos zu aktualisieren. Fügen Sie die Option **-KeyvaultEncryption** ein, um vom Kunden verwaltete Schlüssel für das Speicherkonto zu aktivieren. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen und die in den vorherigen Beispielen definierten Variablen zu verwenden.

```powershell
Set-AzStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName `
    -AccountName $storageAccount.StorageAccountName `
    -KeyvaultEncryption `
    -KeyName $key.Name `
    -KeyVersion $key.Version `
    -KeyVaultUri $keyVault.VaultUri
```

## <a name="update-the-key-version"></a>Aktualisieren der Schlüsselversion

Wenn Sie eine neue Version eines Schlüssels erstellen, müssen Sie das Speicherkonto aktualisieren, damit dieses die neue Version nutzt. Rufen Sie zunächst [Get-AzKeyVaultKey](/powershell/module/az.keyvault/get-azkeyvaultkey) auf, um die neueste Version des Schlüssels abzurufen. Rufen Sie dann [Set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) auf, um die Verschlüsselungseinstellungen des Speicherkontos zu aktualisieren, damit wie im vorherigen Abschnitt gezeigt die neue Version des Schlüssels verwendet wird.

## <a name="use-a-different-key"></a>Verwenden eines anderen Schlüssels

Um den Schlüssel zu ändern, der für die Azure Storage-Verschlüsselung verwendet wird, rufen Sie [Set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) wie unter [Konfigurieren der Verschlüsselung mit vom Kunden verwalteten Schlüsseln](#configure-encryption-with-customer-managed-keys) beschrieben auf, und geben Sie den neuen Schlüsselnamen und die neue Version an. Wenn sich der neue Schlüssel in einem anderen Schlüsseltresor befindet, aktualisieren Sie auch den Schlüsseltresor-URI.

## <a name="revoke-customer-managed-keys"></a>Widerrufen von kundenseitig verwalteten Schlüsseln

Wenn Sie den Verdacht haben, dass ein Schlüssel kompromittiert wurde, können Sie kundenseitig verwaltete Schlüssel widerrufen, indem Sie die Key Vault-Zugriffsrichtlinie entfernen. Zum Widerrufen eines kundenseitig verwalteten Schlüssels rufen Sie den Befehl [Remove-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/remove-azkeyvaultaccesspolicy) auf, wie im folgenden Beispiel gezeigt. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen und die in den vorherigen Beispielen definierten Variablen zu verwenden.

```powershell
Remove-AzKeyVaultAccessPolicy -VaultName $keyVault.VaultName `
    -ObjectId $storageAccount.Identity.PrincipalId `
```

## <a name="disable-customer-managed-keys"></a>Deaktivieren von vom Kunden verwalteten Schlüsseln

Wenn Sie kundenseitig verwaltete Schlüssel deaktivieren, wird Ihr Speicherkonto wieder mit von Microsoft verwalteten Schlüsseln verschlüsselt. Um vom Kunden verwaltete Schlüssel zu deaktivieren, rufen Sie [Set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) mit der Option `-StorageEncryption` auf, wie es im folgenden Beispiel gezeigt ist. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen und die in den vorherigen Beispielen definierten Variablen zu verwenden.

```powershell
Set-AzStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName `
    -AccountName $storageAccount.StorageAccountName `
    -StorageEncryption  
```

## <a name="next-steps"></a>Nächste Schritte

- [Azure Storage encryption for data at rest (Azure Storage-Verschlüsselung für ruhende Daten)](storage-service-encryption.md)
- [What is Azure Key Vault? (Was ist Azure Key Vault?)](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
