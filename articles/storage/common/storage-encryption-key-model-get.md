---
title: Ermitteln des für das Speicherkonto verwendeten Verschlüsselungsschlüsselmodells
titleSuffix: Azure Storage
description: Verwenden Sie das Azure-Portal, PowerShell oder die Azure CLI, um zu überprüfen, wie Verschlüsselungsschlüssel für das Speicherkonto verwaltet werden. Schlüssel können von Microsoft (Standard) oder vom Kunden verwaltet werden. Vom Kunden verwaltete Schlüssel müssen in Azure Key Vault gespeichert werden.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 01/03/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 3806fead9226978c277e87f3d97b14ee38d9552d
ms.sourcegitcommit: 2c59a05cb3975bede8134bc23e27db5e1f4eaa45
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2020
ms.locfileid: "75665407"
---
# <a name="determine-which-azure-storage-encryption-key-model-is-in-use-for-the-storage-account"></a>Ermitteln des für das Speicherkonto verwendeten Azure Storage-Verschlüsselungsschlüsselmodells

Daten in Ihrem Speicherkonto werden automatisch von Azure Storage verschlüsselt. Die Azure Storage-Verschlüsselung bietet zwei Optionen für die Verwaltung von Verschlüsselungsschlüsseln auf Speicherkontoebene:

- **Von Microsoft verwaltete Schlüssel.** Standardmäßig verwaltet Microsoft die Schlüssel, die zum Verschlüsseln Ihres Speicherkontos verwendet werden.
- **Vom Kunden verwaltete Schlüssel.** Optional können Sie Verschlüsselungsschlüssel für Ihr Speicherkonto verwalten. Vom Kunden verwaltete Schlüssel müssen in Azure Key Vault gespeichert werden.

Darüber hinaus können Sie für einige Blob Storage-Vorgänge einen Verschlüsselungsschlüssel auf der Ebene einer einzelnen Anforderung bereitstellen. Wenn ein Verschlüsselungsschlüssel für die Anforderung angegeben ist, überschreibt dieser Schlüssel den für das Speicherkonto aktiven Verschlüsselungsschlüssel. Weitere Informationen finden Sie unter [Angeben eines vom Kunden bereitgestellten Schlüssels für eine Anforderung in Blob Storage](../blobs/storage-blob-customer-provided-key.md).

Weitere Informationen zu Verschlüsselungsschlüsseln finden Sie unter [Azure Storage-Verschlüsselung für ruhende Daten](storage-service-encryption.md).

## <a name="check-the-encryption-key-model-for-the-storage-account"></a>Überprüfen des Verschlüsselungsschlüsselmodells für das Speicherkonto

Verwenden Sie eine der folgenden Methoden, um zu ermitteln, ob ein Speicherkonto von Microsoft verwaltete Schlüssel oder vom Kunden verwaltete Schlüssel für die Verschlüsselung verwendet.

# <a name="azure-portaltabportal"></a>[Azure portal](#tab/portal)

Führen Sie die folgenden Schritte aus, um das Verschlüsselungsmodell für das Speicherkonto mithilfe des Azure-Portals zu überprüfen:

1. Navigieren Sie im Azure-Portal zu Ihrem Speicherkonto.
1. Wählen Sie die Einstellung **Verschlüsselung** aus, und sehen Sie sich die Einstellung an.

Die folgende Abbildung zeigt ein Speicherkonto, bei dem vom Kunden verwaltete Schlüssel für die Verschlüsselung verwendet werden:

![Screenshot der Einstellung für Verschlüsselungsschlüssel im Azure-Portal](media/storage-encryption-key-model-get/customer-managed-encryption-key-setting-portal.png)

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

Wenn Sie das Verschlüsselungsmodell für das Speicherkonto mithilfe von PowerShell überprüfen möchten, rufen Sie den Befehl [Get-AzStorageAccount](/powershell/module/az.storage/get-azstorageaccount) auf, und überprüfen Sie dann die **KeySource**-Eigenschaft für das Konto.

```powershell
$account = Get-AzStorageAccount -ResourceGroupName <resource-group> `
    -Name <storage-account>
$account.Encryption.KeySource
```

Wenn der Wert der **KeySource**-Eigenschaft `Microsoft.Storage` lautet, wird das Konto mit von Microsoft verwalteten Schlüsseln verschlüsselt. Wenn der Wert der **KeySource**-Eigenschaft `Microsoft.Keyvault` lautet, wird das Konto mit vom Kunden verwalteten Schlüsseln verschlüsselt.

# <a name="azure-clitabcli"></a>[Azure-Befehlszeilenschnittstelle](#tab/cli)

Wenn Sie das Verschlüsselungsmodell für das Speicherkonto mithilfe der Azure CLI überprüfen möchten, rufen Sie den Befehl [az storage account show](/cli/azure/storage/account#az-storage-account-show) auf, und überprüfen Sie dann die **keySource**-Eigenschaft für das Konto.

```azurecli-interactive
key_source=$(az storage account show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --query encryption.keySource \
    --output tsv)
```

Wenn der Wert der **keySource**-Eigenschaft `Microsoft.Storage` lautet, wird das Konto mit von Microsoft verwalteten Schlüsseln verschlüsselt. Wenn der Wert der **keySource**-Eigenschaft `Microsoft.Keyvault` lautet, wird das Konto mit vom Kunden verwalteten Schlüsseln verschlüsselt.

---

## <a name="next-steps"></a>Nächste Schritte

[Azure Storage encryption for data at rest (Azure Storage-Verschlüsselung für ruhende Daten)](storage-service-encryption.md)
