---
title: Verwenden der Azure CLI zum Konfigurieren von kundenseitig verwalteten Schlüsseln
titleSuffix: Azure Storage
description: Erfahren Sie, wie Sie mithilfe der Azure-Befehlszeilenschnittstelle von Kunden verwaltete Schlüssel mit Azure Key Vault für die Azure Storage-Verschlüsselung konfigurieren.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 08/24/2020
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.custom: devx-track-azurecli
ms.openlocfilehash: 25ee5d389bc70d82730c7056c752de393a6bf4c5
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88799144"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-by-using-azure-cli"></a>Konfigurieren von kundenseitig verwalteten Schlüsseln mit Azure Key Vault mithilfe der Azure CLI

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

In diesem Artikel wird beschrieben, wie Sie eine Azure Key Vault-Instanz mit von Kunden verwalteten Schlüsseln mithilfe der Azure-Befehlszeilenschnittstelle konfigurieren. Informationen zum Erstellen eines Schlüsseltresors über die Azure-Befehlszeilenschnittstelle finden Sie unter [Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe der Azure CLI](../../key-vault/secrets/quick-create-cli.md).

## <a name="assign-an-identity-to-the-storage-account"></a>Zuweisen einer Identität zum Speicherkonto

Sie müssen zunächst eine systemseitig zugewiesene verwaltete Identität zum Speicherkonto hinzufügen, um von Kunden verwaltete Schlüssel für Ihr Speicherkonto zu aktivieren. Sie verwenden diese Identität, um dem Speicherkonto die Zugriffsberechtigungen für den Schlüsseltresor zu gewähren.

Rufen Sie [az storage account update](/cli/azure/storage/account#az-storage-account-update) auf, um eine verwaltete Identität mit der Azure CLI zuzuweisen. Denken Sie daran, die Platzhalterwerte in den spitzen Klammern durch Ihre eigenen Werte zu ersetzen.

```azurecli-interactive
az login
az account set --subscription <subscription-id>

az storage account update \
    --name <storage-account> \
    --resource-group <resource_group> \
    --assign-identity
```

Weitere Informationen über die Konfiguration systemseitig zugewiesener verwalteter Identitäten mit der Azure CLI finden Sie unter [Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer mithilfe der Azure-Befehlszeilenschnittstelle](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md).

## <a name="create-a-new-key-vault"></a>Erstellen eines neuen Schlüsseltresors

Für den Schlüsseltresor, den Sie zum Speichern von Kunden verwalteter Schlüssel für die Azure Storage-Verschlüsselung verwenden, müssen zwei wichtige Schutzeinstellungen aktiviert sein: **Vorläufiges Löschen** und **Do Not Purge** (Nicht bereinigen). Führen Sie die folgenden Befehle aus, um mithilfe von PowerShell oder der Azure CLI einen neuen Schlüsseltresor zu erstellen, für den diese Einstellungen aktiviert sind. Denken Sie daran, die Platzhalterwerte in den spitzen Klammern durch Ihre eigenen Werte zu ersetzen.

Rufen Sie [az keyvault create](/cli/azure/keyvault#az-keyvault-create) auf, um einen neuen Schlüsseltresor mit der Azure CLI zu erstellen. Denken Sie daran, die Platzhalterwerte in den spitzen Klammern durch Ihre eigenen Werte zu ersetzen.

```azurecli-interactive
az keyvault create \
    --name <key-vault> \
    --resource-group <resource_group> \
    --location <region> \
    --enable-soft-delete \
    --enable-purge-protection
```

Informationen zum Aktivieren von **Vorläufiges Löschen** und **Nicht bereinigen** in einem vorhandenen Schlüsseltresor mithilfe der Azure-Befehlszeilenschnittstelle finden Sie in den Abschnitten **Aktivieren des vorläufigen Löschens** und **Aktivieren des Löschschutzes** unter [Verwenden des vorläufigen Löschens mit der Befehlszeilenschnittstelle](../../key-vault/general/soft-delete-cli.md).

## <a name="configure-the-key-vault-access-policy"></a>Konfigurieren der Zugriffsrichtlinie für den Schlüsseltresor

Konfigurieren Sie als Nächstes die Zugriffsrichtlinie für den Schlüsseltresor, sodass das Speicherkonto über die Berechtigungen für den Zugriff auf diesen verfügt. In diesem Schritt verwenden Sie die verwaltete Identität, die Sie zuvor dem Speicherkonto zugewiesen haben.

Rufen Sie [az keyvault set-policy](/cli/azure/keyvault#az-keyvault-set-policy) auf, um die Zugriffsrichtlinie für den Schlüsseltresor festzulegen. Denken Sie daran, die Platzhalterwerte in den spitzen Klammern durch Ihre eigenen Werte zu ersetzen.

```azurecli-interactive
storage_account_principal=$(az storage account show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --query identity.principalId \
    --output tsv)
az keyvault set-policy \
    --name <key-vault> \
    --resource-group <resource_group>
    --object-id $storage_account_principal \
    --key-permissions get unwrapKey wrapKey
```

## <a name="create-a-new-key"></a>Erstellen eines neuen Schlüssels

Als Nächstes erstellen Sie einen Schlüssel im Schlüsseltresor. Rufen Sie hierzu den Befehl [az keyvault key create](/cli/azure/keyvault/key#az-keyvault-key-create) auf. Denken Sie daran, die Platzhalterwerte in den spitzen Klammern durch Ihre eigenen Werte zu ersetzen.

```azurecli-interactive
az keyvault key create \
    --name <key> \
    --vault-name <key-vault>
```

Die Azure Storage-Verschlüsselung unterstützt RSA- und RSA-HSM-Schlüssel mit einer Größe von 2.048, 3.072 und 4.096 Bit. Weitere Informationen zu Schlüsseln finden Sie unter **Key Vault-Schlüssel** in [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten in Azure Key Vault](../../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys).

## <a name="configure-encryption-with-customer-managed-keys"></a>Konfigurieren der Verschlüsselung mit von Kunden verwalteten Schlüsseln

Die Azure Storage-Verschlüsselung verwendet standardmäßig von Microsoft verwaltete Schlüssel. In diesem Schritt konfigurieren Sie Ihr Azure Storage-Konto für kundenseitig verwaltete Schlüssel mit Azure Key Vault. Legen Sie dann den Schlüssel fest, der dem Speicherkonto zugeordnet werden soll.

Beim Konfigurieren der Verschlüsselung mit kundenseitig verwalteten Schlüsseln können Sie festlegen, dass der für die Verschlüsselung verwendete Schlüssel automatisch aktualisiert wird, wenn sich die Schlüsselversion im zugeordneten Schlüsseltresor ändert. Alternativ können Sie explizit eine Schlüsselversion angeben, die für die Verschlüsselung verwendet werden soll, bis die Schlüsselversion manuell aktualisiert wird.

> [!NOTE]
> Um einen Schlüssel zu drehen, erstellen Sie eine neue Version des Schlüssels in Azure Key Vault. Azure Storage handhabt die Rotation des Schlüssels nicht in Azure Key Vault, sodass Sie Ihren Schlüssel manuell drehen oder eine Funktion erstellen müssen, um ihn nach einem Zeitplan zu drehen.

### <a name="configure-encryption-to-automatically-update-the-key-version"></a>Konfigurieren der Verschlüsselung zur automatischen Aktualisierung der Schlüsselversion

Um die Verschlüsselung mit kundenseitig verwalteten Schlüsseln zur automatischen Aktualisierung der Schlüsselversion zu konfigurieren, installieren Sie [Azure CLI, Version 2.4.0](/cli/azure/release-notes-azure-cli#april-21-2020) oder höher. Weitere Informationen finden Sie unter [Installieren der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

Um die Schlüsselversion für einen kundenseitig verwalteten Schlüssel automatisch zu aktualisieren, lassen Sie die Schlüsselversion aus, wenn Sie die Verschlüsselung mit kundenseitig verwalteten Schlüsseln für das Speicherkonto konfigurieren. Rufen Sie [az storage account update](/cli/azure/storage/account#az-storage-account-update) wie im folgenden Beispiel gezeigt auf, um die Verschlüsselungseinstellungen des Speicherkontos zu aktualisieren. Fügen Sie den Parameter `--encryption-key-source` ein, und legen Sie ihn auf `Microsoft.Keyvault` fest, um kundenseitig verwaltete Schlüssel für das Speicherkonto zu aktivieren. Denken Sie daran, die Platzhalterwerte in den spitzen Klammern durch Ihre eigenen Werte zu ersetzen.

```azurecli-interactive
key_vault_uri=$(az keyvault show \
    --name <key-vault> \
    --resource-group <resource_group> \
    --query properties.vaultUri \
    --output tsv)
az storage account update
    --name <storage-account> \
    --resource-group <resource_group> \
    --encryption-key-name <key> \
    --encryption-key-source Microsoft.Keyvault \
    --encryption-key-vault $key_vault_uri
```

### <a name="configure-encryption-for-manual-updating-of-key-versions"></a>Konfigurieren der Verschlüsselung für die manuelle Aktualisierung von Schlüsselversionen

Wenn Sie explizit eine Schlüsselversion für die Verschlüsselung angeben möchten, geben Sie diese Schlüsselversion beim Konfigurieren der Verschlüsselung mit kundenseitig verwalteten Schlüsseln für das Speicherkonto an. Rufen Sie [az storage account update](/cli/azure/storage/account#az-storage-account-update) wie im folgenden Beispiel gezeigt auf, um die Verschlüsselungseinstellungen des Speicherkontos zu aktualisieren. Fügen Sie den Parameter `--encryption-key-source` ein, und legen Sie ihn auf `Microsoft.Keyvault` fest, um kundenseitig verwaltete Schlüssel für das Speicherkonto zu aktivieren. Denken Sie daran, die Platzhalterwerte in den spitzen Klammern durch Ihre eigenen Werte zu ersetzen.

```azurecli-interactive
key_vault_uri=$(az keyvault show \
    --name <key-vault> \
    --resource-group <resource_group> \
    --query properties.vaultUri \
    --output tsv)
key_version=$(az keyvault key list-versions \
    --name <key> \
    --vault-name <key-vault> \
    --query [-1].kid \
    --output tsv | cut -d '/' -f 6)
az storage account update
    --name <storage-account> \
    --resource-group <resource_group> \
    --encryption-key-name <key> \
    --encryption-key-version $key_version \
    --encryption-key-source Microsoft.Keyvault \
    --encryption-key-vault $key_vault_uri
```

Wenn Sie die Schlüsselversion manuell aktualisieren, müssen Sie die Verschlüsselungseinstellungen des Speicherkontos aktualisieren, damit die neue Version verwendet wird. Rufen Sie zunächst [az keyvault show](/cli/azure/keyvault#az-keyvault-show) zum Abfragen des Schlüsseltresor-URIs und [az keyvault key list-versions](/cli/azure/keyvault/key#az-keyvault-key-list-versions) zum Abfragen der Schlüsselversion auf. Rufen Sie dann [az storage account update](/cli/azure/storage/account#az-storage-account-update) auf, um die Verschlüsselungseinstellungen des Speicherkontos zu aktualisieren, damit wie im vorherigen Beispiel gezeigt die neue Version des Schlüssels verwendet wird.

## <a name="use-a-different-key"></a>Verwenden eines anderen Schlüssels

Um den Schlüssel zu ändern, der für die Azure Storage-Verschlüsselung verwendet wird, rufen Sie [az storage account update](/cli/azure/storage/account#az-storage-account-update) wie unter [Konfigurieren der Verschlüsselung mit vom Kunden verwalteten Schlüsseln](#configure-encryption-with-customer-managed-keys) beschrieben auf, und geben Sie den neuen Schlüsselnamen und die neue Version an. Wenn sich der neue Schlüssel in einem anderen Schlüsseltresor befindet, aktualisieren Sie auch den Schlüsseltresor-URI.

## <a name="revoke-customer-managed-keys"></a>Widerrufen von kundenseitig verwalteten Schlüsseln

Sie können kundenseitig verwaltete Schlüssel widerrufen, indem Sie die Schlüsseltresor-Zugriffsrichtlinie entfernen. Zum Widerrufen eines kundenseitig verwalteten Schlüssels rufen Sie den Befehl [az keyvault delete-policy](/cli/azure/keyvault#az-keyvault-delete-policy) auf, wie im folgenden Beispiel gezeigt. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen und die in den vorherigen Beispielen definierten Variablen zu verwenden.

```azurecli-interactive
az keyvault delete-policy \
    --name <key-vault> \
    --object-id $storage_account_principal
```

## <a name="disable-customer-managed-keys"></a>Deaktivieren von vom Kunden verwalteten Schlüsseln

Wenn Sie kundenseitig verwaltete Schlüssel deaktivieren, wird Ihr Speicherkonto wieder mit von Microsoft verwalteten Schlüsseln verschlüsselt. Um vom Kunden verwaltete Schlüssel zu deaktivieren, rufen Sie [az storage account update](/cli/azure/storage/account#az-storage-account-update) auf, und legen Sie `--encryption-key-source parameter` auf `Microsoft.Storage` fest, wie im folgenden Beispiel gezeigt. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen und die in den vorherigen Beispielen definierten Variablen zu verwenden.

```azurecli-interactive
az storage account update
    --name <storage-account> \
    --resource-group <resource_group> \
    --encryption-key-source Microsoft.Storage
```

## <a name="next-steps"></a>Nächste Schritte

- [Azure Storage encryption for data at rest (Azure Storage-Verschlüsselung für ruhende Daten)](storage-service-encryption.md) 
- [What is Azure Key Vault? (Was ist Azure Key Vault?)](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
