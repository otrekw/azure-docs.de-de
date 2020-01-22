---
title: Erstellen eines Kontos, das kundenseitig verwaltete Schlüssel für Tabellen und Warteschlangen unterstützt
titleSuffix: Azure Storage
description: Hier erfahren Sie, wie Sie ein Speicherkonto erstellen, das die Konfiguration von kundenseitig verwalteten Schlüsseln für Tabellen und Warteschlangen unterstützt. Verwenden Sie die Azure-Befehlszeilenschnittstelle (Azure CLI) oder eine Azure Resource Manager-Vorlage zum Erstellen eines Speicherkontos, das den Kontoverschlüsselungsschlüssel zur Azure Storage-Verschlüsselung verwendet. Anschließend können Sie kundenseitig verwaltete Schlüssel für das Konto konfigurieren.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 01/10/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 8cf1f8ecb68e31f93c19d93d6ebc4f8ef37724e7
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "76028450"
---
# <a name="create-an-account-that-supports-customer-managed-keys-for-tables-and-queues"></a>Erstellen eines Kontos, das kundenseitig verwaltete Schlüssel für Tabellen und Warteschlangen unterstützt

Azure Storage verschlüsselt alle Daten in einem ruhenden Speicherkonto. Standardmäßig verwenden Queue Storage und Table Storage einen Schlüssel, der dem Dienst zugeordnet ist und von Microsoft verwaltet wird. Sie können auch kundenseitig verwaltete Schlüssel verwenden, um Warteschlangen- oder Tabellendaten zu verschlüsseln. Wenn Sie kundenseitig verwaltete Schlüssel bei Warteschlangen und Tabellen verwenden möchten, müssen Sie zuerst ein Speicherkonto erstellen, das einen dem Konto (und nicht dem Dienst) zugeordneten Verschlüsselungsschlüssel verwendet. Nachdem Sie ein Speicherkonto erstellt haben, das den Kontoverschlüsselungsschlüssel für Warteschlangen- und Tabellendaten verwendet, können Sie kundenseitig verwaltete Schlüssel mit Azure Key Vault für dieses Konto konfigurieren.

In diesem Artikel wird beschrieben, wie Sie ein Speicherkonto erstellen, das einen dem Konto zugeordneten Schlüssel verwendet. Wenn das Konto zum ersten Mal erstellt wird, verwendet Microsoft den Kontoschlüssel zum Verschlüsseln der Daten im Konto und verwaltet diesen Schlüssel auch. Anschließend können Sie kundenseitig verwaltete Schlüssel für das Konto konfigurieren, um diese Vorteile zu nutzen – einschließlich der Möglichkeit, Ihre eigenen Schlüssel bereitzustellen, die Schlüsselversion zu aktualisieren, die Schlüssel zu rotieren und Zugriffssteuerungen zu widerrufen.

## <a name="about-the-feature"></a>Informationen zum Feature

Zum Erstellen eines Speicherkontos, das den Kontoverschlüsselungsschlüssel für Queue Storage und Table Storage verwendet, müssen Sie sich zuerst registrieren, um dieses Feature bei Azure nutzen zu können. Beachten Sie, dass es aufgrund von eingeschränkter Kapazität mehrere Monate dauern kann, bis Zugriffsanforderungen genehmigt werden.

Sie können ein Speicherkonto erstellen, das den Kontoverschlüsselungsschlüssel für Queue Storage und Table Storage in den folgenden Regionen verwendet:

- East US
- USA Süd Mitte
- USA, Westen 2  

### <a name="register-to-use-the-account-encryption-key"></a>Registrieren zur Verwendung des Kontoverschlüsselungsschlüssels

Zum Registrieren bei Azure CLI rufen Sie den Befehl „[az feature register](/cli/azure/feature#az-feature-register)“ auf.

So registrieren Sie sich für die Verwendung des Kontoverschlüsselungsschlüssels bei Queue Storage:

```azurecli
az feature register --namespace Microsoft.Storage --name AllowAccountEncryptionKeyForQueues
```

So registrieren Sie sich für die Verwendung des Kontoverschlüsselungsschlüssels bei Table Storage:

```azurecli
az feature register --namespace Microsoft.Storage --name AllowAccountEncryptionKeyForTables
```

### <a name="check-the-status-of-your-registration"></a>Überprüfen des Registrierungsstatus

So überprüfen Sie den Status Ihrer Registrierung für Queue Storage:

```azurecli
az feature show --namespace Microsoft.Storage --name AllowAccountEncryptionKeyForQueues
```

So überprüfen Sie den Status Ihrer Registrierung für Table Storage:

```azurecli
az feature show --namespace Microsoft.Storage --name AllowAccountEncryptionKeyForTables
```

### <a name="re-register-the-azure-storage-resource-provider"></a>Erneutes Registrieren des Azure Storage-Ressourcenanbieters

Nachdem Ihre Registrierung genehmigt wurde, müssen Sie den Azure Storage-Ressourcenanbieter erneut registrieren. Rufen Sie den Befehl „[az provider register](/cli/azure/provider#az-provider-register)“ auf:

```azurecli
az provider register --namespace 'Microsoft.Storage'
```

## <a name="create-an-account-that-uses-the-account-encryption-key"></a>Erstellen eines Kontos, das den Kontoverschlüsselungsschlüssel verwendet

Zum Zeitpunkt der Erstellung des Speicherkontos müssen Sie ein neues Speicherkonto konfigurieren, um den Kontoverschlüsselungsschlüssel für Warteschlangen und Tabellen verwenden zu können. Der Gültigkeitsbereich des Verschlüsselungsschlüssels kann nicht mehr geändert werden, nachdem das Konto erstellt wurde.

Das Speicherkonto muss vom Typ „Allgemein v2 (GPv2)“ sein. Sie können das Speicherkonto erstellen und für die Verwendung des Kontoverschlüsselungsschlüssels konfigurieren, indem Sie entweder Azure CLI oder eine Azure Resource Manager-Vorlage verwenden.

> [!NOTE]
> Nur Queue Storage und Table Storage können optional so konfiguriert werden, dass Daten beim Erstellen des Speicherkontos mit dem Kontoverschlüsselungsschlüssel verschlüsselt werden. Blob Storage und Azure Files verwenden immer den Kontoverschlüsselungsschlüssel zum Verschlüsseln von Daten.

### <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Wenn Sie mithilfe der Azure-Befehlszeilenschnittstelle (Azure CLI) ein Speicherkonto erstellen möchten, das den Kontoverschlüsselungsschlüssel verwendet, müssen Sie Azure CLI, Version 2.0.80 oder höher, installiert haben. Weitere Informationen finden Sie unter [Installieren der Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli).

Erstellen Sie als Nächstes ein Speicherkonto vom Typ „Allgemein v2 (GPv2)“, indem Sie den Befehl „[az storage account create](/cli/azure/storage/account#az-storage-account-create)“ mit den entsprechenden Parametern aufrufen:

- Fügen Sie die Option `--encryption-key-type-for-queue` ein, und legen Sie deren Wert auf `Account` fest, um den Kontoverschlüsselungsschlüssel zum Verschlüsseln von Daten in Queue Storage verwenden zu können.
- Fügen Sie die Option `--encryption-key-type-for-table` ein, und legen Sie deren Wert auf `Account` fest, um den Kontoverschlüsselungsschlüssel zum Verschlüsseln von Daten in Table Storage verwenden zu können.

Das folgende Beispiel zeigt, wie Sie ein Speicherkonto vom Typ „Allgemein v2 (GPv2)“ erstellen, das für LRS konfiguriert wird und den Kontoverschlüsselungsschlüssel zum Verschlüsseln von Daten für Queue Storage und Table Storage verwendet. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen:

```azurecli
az storage account create \
    --name <storage-account> \
    --resource-group <resource-group> \
    --location <location> \
    --sku Standard_LRS \
    --kind StorageV2 \
    --encryption-key-type-for-table Account \
    --encryption-key-type-for-queue Account
```

### <a name="templatetabtemplate"></a>[Vorlage](#tab/template)

Mit dem folgenden JSON-Beispiel wird ein Speicherkonto vom Typ „Allgemein v2 (GPv2)“ erstellt, das für LRS konfiguriert wird und den Kontoverschlüsselungsschlüssel zum Verschlüsseln von Daten für Queue Storage und Table Storage verwendet. Denken Sie daran, die Platzhalterwerte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

```json
"resources": [
    {
        "type": "Microsoft.Storage/storageAccounts",
        "apiVersion": "2019-06-01",
        "name": "[parameters('<storage-account>')]",
        "location": "[parameters('<location>')]",
        "dependsOn": [],
        "tags": {},
        "sku": {
            "name": "[parameters('Standard_LRS')]"
        },
        "kind": "[parameters('StorageV2')]",
        "properties": {
            "accessTier": "[parameters('<accessTier>')]",
            "supportsHttpsTrafficOnly": "[parameters('supportsHttpsTrafficOnly')]",
            "largeFileSharesState": "[parameters('<largeFileSharesState>')]",
            "encryption": {
                "services": {
                    "queue": {
                        "keyType": "Account"
                    },
                    "table": {
                        "keyType": "Account"
                    }
                },
                "keySource": "Microsoft.Storage"
            }
        }
    }
],
```

---

Nachdem Sie ein Konto erstellt haben, das den Kontoverschlüsselungsschlüssel verwendet, lesen Sie einen der folgenden Artikel zum Konfigurieren von kundenseitig verwalteten Schlüsseln mit Azure Key Vault:

- [Konfigurieren von kundenseitig verwalteten Schlüsseln mit Azure Key Vault über das Azure-Portal](storage-encryption-keys-portal.md)
- [Konfigurieren von kundenseitig verwalteten Schlüsseln mit Azure Key Vault mithilfe von PowerShell](storage-encryption-keys-powershell.md)
- [Konfigurieren von kundenseitig verwalteten Schlüsseln mit Azure Key Vault mithilfe der Azure CLI](storage-encryption-keys-cli.md)

## <a name="verify-the-account-encryption-key"></a>Überprüfen des Kontoverschlüsselungsschlüssels

Wenn Sie überprüfen möchten, ob ein Dienst in einem Speicherkonto den Kontoverschlüsselungsschlüssel verwendet, rufen Sie den Azure CLI-Befehl „[az storage account](/cli/azure/storage/account#az-storage-account-show)“ auf. Dieser Befehl gibt einen Satz von Speicherkontoeigenschaften und deren Werte zurück. Suchen Sie in der Eigenschaft „Encryption“ bei jedem Dienst nach dem Feld `keyType`, und überprüfen Sie, ob dessen Wert auf `Account` festgelegt ist.

```azurecli
az storage account show /
    --name <storage-account> /
    --resource-group <resource-group>
```

## <a name="next-steps"></a>Nächste Schritte

- [Azure Storage encryption for data at rest (Azure Storage-Verschlüsselung für ruhende Daten)](storage-service-encryption.md) 
- [What is Azure Key Vault? (Was ist Azure Key Vault?)](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
