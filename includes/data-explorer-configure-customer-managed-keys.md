---
author: orspod
ms.service: data-explorer
ms.topic: include
ms.date: 01/07/2020
ms.author: orspodek
ms.openlocfilehash: 5443ee6912c30b89cee6fdb43f84f3bc1fbcfe68
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "76020953"
---
Azure Data Explorer verschlüsselt alle Daten in einem Speicherkonto für ruhende Daten. Standardmäßig werden Daten mit von Microsoft verwalteten Schlüsseln verschlüsselt. Um zusätzliche Kontrolle über die Verschlüsselungsschlüssel zu erhalten, können Sie kundenseitig verwaltete Schlüssel für die Datenverschlüsselung bereitstellen. Kundenseitig verwaltete Schlüssel müssen in [Azure Key Vault](/azure/key-vault/key-vault-overview) gespeichert werden. Sie können eigene Schlüssel erstellen und in einer Key Vault-Instanz speichern oder eine Azure Key Vault-API verwenden, um Schlüssel zu generieren. Der Azure Data Explorer-Cluster und die Key Vault-Instanz müssen sich in der gleichen Region befinden, können aber zu verschiedenen Abonnements gehören. Eine ausführliche Erläuterung zu kundenseitig verwalteten Schlüsseln finden Sie unter [Azure Storage-Verschlüsselung für ruhende Daten](/azure/storage/common/storage-service-encryption) im Abschnitt „Von Kunden verwaltete Schlüssel mit Azure Key Vault“. Der vorliegende Artikel veranschaulicht das Konfigurieren von kundenseitig verwalteten Schlüsseln.

> [!Note]
> Um kundenseitig verwaltete Schlüssel mit Azure Data Explorer zu konfigurieren, müssen Sie [zwei Eigenschaften in Key Vault festlegen](/azure/key-vault/key-vault-ovw-soft-delete): **Vorläufiges Löschen** und **Do Not Purge** (Nicht bereinigen). Diese Eigenschaften sind standardmäßig nicht aktiviert. Um diese Eigenschaften zu aktivieren, verwenden Sie entweder [PowerShell](/azure/key-vault/key-vault-soft-delete-powershell) oder die [Azure CLI](/azure/key-vault/key-vault-soft-delete-cli). Es werden ausschließlich RSA-Schlüssel und die Schlüsselgröße 2048 unterstützt.

## <a name="assign-an-identity-to-the-cluster"></a>Zuweisen einer Identität zum Cluster

Um kundenseitig verwaltete Schlüssel für Ihren Cluster zu aktivieren, weisen Sie dem Cluster zunächst eine vom System zugewiesene verwaltete Identität hinzu. Sie verwenden diese verwaltete Identität, um dem Cluster Berechtigungen für den Zugriff auf die Key Vault-Instanz zu gewähren. Informationen zum Konfigurieren systemseitig zugewiesener verwalteter Identitäten finden Sie unter [Verwaltete Identitäten](/azure/data-explorer/managed-identities).

## <a name="create-a-new-key-vault"></a>Erstellen eines neuen Schlüsseltresors

Rufen Sie zum Erstellen eines neuen Schlüsseltresors mit PowerShell [New-AzKeyVault](/powershell/module/az.keyvault/new-azkeyvault) auf. In der Key Vault-Instanz, die Sie zum Speichern der kundenseitig verwalteten Schlüssel für die Azure Data Explorer-Verschlüsselung verwenden, müssen zwei Einstellungen zum Schutz der Schlüssel aktiviert sein: **Vorläufiges Löschen** und **Nicht bereinigen**. Ersetzen Sie im Beispiel unten die Platzhalterwerte in Klammern durch Ihre eigenen Werte.

```azurepowershell-interactive
$keyVault = New-AzKeyVault -Name <key-vault> `
    -ResourceGroupName <resource_group> `
    -Location <location> `
    -EnableSoftDelete `
    -EnablePurgeProtection
```

## <a name="configure-the-key-vault-access-policy"></a>Konfigurieren der Zugriffsrichtlinie für den Schlüsseltresor

Konfigurieren Sie als Nächstes die Zugriffsrichtlinie für die Key Vault-Instanz, sodass der Cluster die Berechtigung für den Zugriff darauf erhält. In diesem Schritt verwenden Sie die systemseitig zugewiesene verwaltete Identität, die Sie zuvor dem Cluster zugewiesen haben. Rufen Sie zum Festlegen der Zugriffsrichtlinie für den Schlüsseltresor [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) auf. Ersetzen Sie die Platzhalterwerte in Klammern durch Ihre eigenen Werte, und verwenden Sie die in den vorherigen Beispielen definierten Variablen.

```azurepowershell-interactive
Set-AzKeyVaultAccessPolicy `
    -VaultName $keyVault.VaultName `
    -ObjectId $cluster.Identity.PrincipalId `
    -PermissionsToKeys wrapkey,unwrapkey,get,recover
```

## <a name="create-a-new-key"></a>Erstellen eines neuen Schlüssels

Als Nächstes erstellen Sie einen Schlüssel im Schlüsseltresor. Rufen Sie zum Erstellen eines neuen Schlüssels [Add-AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey) auf. Ersetzen Sie die Platzhalterwerte in Klammern durch Ihre eigenen Werte, und verwenden Sie die in den vorherigen Beispielen definierten Variablen.

```azurepowershell-interactive
$key = Add-AzKeyVaultKey -VaultName $keyVault.VaultName -Name <key> -Destination 'Software'
```
