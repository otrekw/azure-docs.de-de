---
title: Verwenden von PowerShell zum Konfigurieren von kundenseitig verwalteten Schlüsseln
titleSuffix: Azure Storage
description: Erfahren Sie, wie Sie PowerShell verwenden, um von Kunden verwaltete Schlüssel für die Azure Storage-Verschlüsselung zu konfigurieren.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 08/24/2020
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: 1c928056ec0e7b101d991c8d8c8db3bd659251ba
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88799127"
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

Um einen neuen Schlüsseltresor mit PowerShell zu erstellen, installieren Sie Version 2.0.0 oder höher des [Az.KeyVault](https://www.powershellgallery.com/packages/Az.KeyVault/2.0.0) PowerShell-Moduls. Rufen Sie dann [New-AzKeyVault](/powershell/module/az.keyvault/new-azkeyvault) auf, um einen neuen Schlüsseltresor zu erstellen.

Für den Schlüsseltresor, den Sie zum Speichern von Kunden verwalteter Schlüssel für die Azure Storage-Verschlüsselung verwenden, müssen zwei wichtige Schutzeinstellungen aktiviert sein: **Vorläufiges Löschen** und **Do Not Purge** (Nicht bereinigen). In Version 2.0.0 und höher des Az.KeyVault-Moduls ist das vorläufige Löschen standardmäßig aktiviert, wenn Sie einen neuen Schlüsseltresor erstellen.

Im folgenden Beispiel wird ein neuer Schlüsseltresor mit den Eigenschaften **Soft Delete** (Vorläufiges Löschen) und **Do Not Purge** (Nicht bereinigen) erstellt. Denken Sie daran, die Platzhalterwerte in den spitzen Klammern durch Ihre eigenen Werte zu ersetzen.

```powershell
$keyVault = New-AzKeyVault -Name <key-vault> `
    -ResourceGroupName <resource_group> `
    -Location <location> `
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

Die Azure Storage-Verschlüsselung unterstützt RSA- und RSA-HSM-Schlüssel mit einer Größe von 2.048, 3.072 und 4.096 Bit. Weitere Informationen zu Schlüsseln finden Sie unter **Key Vault-Schlüssel** in [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten in Azure Key Vault](../../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys).

## <a name="configure-encryption-with-customer-managed-keys"></a>Konfigurieren der Verschlüsselung mit von Kunden verwalteten Schlüsseln

Die Azure Storage-Verschlüsselung verwendet standardmäßig von Microsoft verwaltete Schlüssel. In diesem Schritt konfigurieren Sie Ihr Azure Storage-Konto für kundenseitig verwaltete Schlüssel mit Azure Key Vault. Legen Sie dann den Schlüssel fest, der dem Speicherkonto zugeordnet werden soll.

Beim Konfigurieren der Verschlüsselung mit kundenseitig verwalteten Schlüsseln können Sie festlegen, dass der für die Verschlüsselung verwendete Schlüssel automatisch aktualisiert wird, wenn sich die Schlüsselversion im zugeordneten Schlüsseltresor ändert. Alternativ können Sie explizit eine Schlüsselversion angeben, die für die Verschlüsselung verwendet werden soll, bis die Schlüsselversion manuell aktualisiert wird.

> [!NOTE]
> Um einen Schlüssel zu drehen, erstellen Sie eine neue Version des Schlüssels in Azure Key Vault. Azure Storage handhabt die Rotation des Schlüssels nicht in Azure Key Vault, sodass Sie Ihren Schlüssel manuell drehen oder eine Funktion erstellen müssen, um ihn nach einem Zeitplan zu drehen.

### <a name="configure-encryption-to-automatically-update-the-key-version"></a>Konfigurieren der Verschlüsselung zur automatischen Aktualisierung der Schlüsselversion

Um die Verschlüsselung mit kundenseitig verwalteten Schlüsseln zur automatischen Aktualisierung der Schlüsselversion zu konfigurieren, installieren Sie das Modul [Az.Storage](https://www.powershellgallery.com/packages/Az.Storage), Version 2.0.0 oder höher.

Um die Schlüsselversion für einen kundenseitig verwalteten Schlüssel automatisch zu aktualisieren, lassen Sie die Schlüsselversion aus, wenn Sie die Verschlüsselung mit kundenseitig verwalteten Schlüsseln für das Speicherkonto konfigurieren. Rufen Sie [Set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) auf, um die Verschlüsselungseinstellungen des Speicherkontos zu aktualisieren, wie im folgenden Beispiel gezeigt, und fügen Sie die Option **-KeyvaultEncryption** ein, um kundenseitig verwaltete Schlüssel für das Speicherkonto zu aktivieren. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen und die in den vorherigen Beispielen definierten Variablen zu verwenden.

```powershell
Set-AzStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName `
    -AccountName $storageAccount.StorageAccountName `
    -KeyvaultEncryption `
    -KeyName $key.Name `
    -KeyVaultUri $keyVault.VaultUri
```

### <a name="configure-encryption-for-manual-updating-of-key-versions"></a>Konfigurieren der Verschlüsselung für die manuelle Aktualisierung von Schlüsselversionen

Wenn Sie explizit eine Schlüsselversion für die Verschlüsselung angeben möchten, geben Sie diese Schlüsselversion beim Konfigurieren der Verschlüsselung mit kundenseitig verwalteten Schlüsseln für das Speicherkonto an. Rufen Sie [Set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) auf, um die Verschlüsselungseinstellungen des Speicherkontos zu aktualisieren, wie im folgenden Beispiel gezeigt, und fügen Sie die Option **-KeyvaultEncryption** ein, um kundenseitig verwaltete Schlüssel für das Speicherkonto zu aktivieren. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen und die in den vorherigen Beispielen definierten Variablen zu verwenden.

```powershell
Set-AzStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName `
    -AccountName $storageAccount.StorageAccountName `
    -KeyvaultEncryption `
    -KeyName $key.Name `
    -KeyVersion $key.Version `
    -KeyVaultUri $keyVault.VaultUri
```

Wenn Sie die Schlüsselversion manuell aktualisieren, müssen Sie die Verschlüsselungseinstellungen des Speicherkontos aktualisieren, damit die neue Version verwendet wird. Rufen Sie zunächst [Get-AzKeyVaultKey](/powershell/module/az.keyvault/get-azkeyvaultkey) auf, um die neueste Version des Schlüssels abzurufen. Rufen Sie dann [Set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) auf, um die Verschlüsselungseinstellungen des Speicherkontos zu aktualisieren, damit wie im vorherigen Beispiel gezeigt die neue Version des Schlüssels verwendet wird.

## <a name="use-a-different-key"></a>Verwenden eines anderen Schlüssels

Um den Schlüssel zu ändern, der für die Azure Storage-Verschlüsselung verwendet wird, rufen Sie [Set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) wie unter [Konfigurieren der Verschlüsselung mit vom Kunden verwalteten Schlüsseln](#configure-encryption-with-customer-managed-keys) beschrieben auf, und geben Sie den neuen Schlüsselnamen und die neue Version an. Wenn sich der neue Schlüssel in einem anderen Schlüsseltresor befindet, aktualisieren Sie auch den Schlüsseltresor-URI.

## <a name="revoke-customer-managed-keys"></a>Widerrufen von kundenseitig verwalteten Schlüsseln

Sie können kundenseitig verwaltete Schlüssel widerrufen, indem Sie die Schlüsseltresor-Zugriffsrichtlinie entfernen. Zum Widerrufen eines kundenseitig verwalteten Schlüssels rufen Sie den Befehl [Remove-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/remove-azkeyvaultaccesspolicy) auf, wie im folgenden Beispiel gezeigt. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen und die in den vorherigen Beispielen definierten Variablen zu verwenden.

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
