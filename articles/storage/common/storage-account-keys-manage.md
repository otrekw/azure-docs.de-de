---
title: Verwalten von Kontozugriffsschlüsseln
titleSuffix: Azure Storage
description: Erfahren Sie, wie Sie Ihre Speicherkonto-Zugriffsschlüssel anzeigen, verwalten und rotieren können.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 04/24/2020
ms.author: tamram
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: e5ea94fea00771b64634d6c28a7879fabb195f09
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "89069658"
---
# <a name="manage-storage-account-access-keys"></a>Verwalten von Speicherkonto-Zugriffsschlüsseln

Wenn Sie ein Speicherkonto erstellen, generiert Azure zwei 512-Bit-Speicherkonto-Zugriffsschlüssel. Mit diesen Schlüsseln können Sie den Zugriff auf Daten in Ihrem Speicherkonto per gemeinsam verwendetem Schlüssel autorisieren.

Microsoft empfiehlt die Verwendung von Azure Key Vault zum Verwalten Ihrer Zugriffsschlüssel sowie ein regelmäßiges Rotieren und Neugenerieren Ihrer Schlüssel. Mit Azure Key Vault können Sie die Schlüssel auf einfache Weise rotieren, ohne dass Ihre Anwendungen unterbrochen werden. Sie können Ihre Schlüssel auch manuell rotieren.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

## <a name="view-account-access-keys"></a>Anzeigen von Kontozugriffsschlüsseln

Sie können Ihre Kontozugriffsschlüssel mit dem Azure-Portal, PowerShell oder der Azure CLI anzeigen und kopieren. Das Azure-Portal stellt auch eine Verbindungszeichenfolge für Ihr Speicherkonto bereit, die Sie kopieren können.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Führen Sie folgende Schritte durch, um die Zugriffsschlüssel oder Verbindungszeichenfolge Ihres Speicherkontos aus dem Azure-Portal anzuzeigen oder zu kopieren:

1. Navigieren Sie zum Speicherkonto im [Azure-Portal](https://portal.azure.com).
1. Wählen Sie unter **Einstellungen** die Option **Zugriffsschlüssel** aus. Daraufhin werden Ihre Zugriffsschlüssel zusammen mit der jeweiligen vollständigen Verbindungszeichenfolge angezeigt.
1. Suchen Sie unter **key1** nach dem Wert für **Schlüssel**, und klicken Sie dann auf die Schaltfläche zum **Kopieren**, um den Kontoschlüssel zu kopieren.
1. Alternativ können Sie die gesamte Verbindungszeichenfolge kopieren. Suchen Sie unter **key1** nach dem Wert für die **Verbindungszeichenfolge**, und klicken Sie dann auf die Schaltfläche **Kopieren**, um die Verbindungszeichenfolge zu kopieren.

    :::image type="content" source="media/storage-account-keys-manage/portal-connection-string.png" alt-text="Screenshot, der zeigt, wie Sie Zugriffsschlüssel im Azure-Portal anzeigen":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Rufen Sie den Befehl [Get-AzStorageAccountKey](/powershell/module/az.Storage/Get-azStorageAccountKey) auf, um Ihre Kontozugriffsschlüssel mit PowerShell abzurufen.

Im folgenden Beispiel wird der erste Schlüssel abgerufen. Verwenden Sie `Value[1]` anstelle von `Value[0]`, um den zweiten Schlüssel abzurufen. Denken Sie daran, die Platzhalterwerte in den spitzen Klammern durch Ihre eigenen Werte zu ersetzen.

```powershell
$storageAccountKey = `
    (Get-AzStorageAccountKey `
    -ResourceGroupName <resource-group> `
    -Name <storage-account>).Value[0]
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Rufen Sie, wie im folgenden Beispiel zu sehen, den Befehl [az storage account keys list](/cli/azure/storage/account/keys#az-storage-account-keys-list) auf, um Ihre Kontozugriffsschlüssel mit der Azure CLI aufzulisten. Denken Sie daran, die Platzhalterwerte in den spitzen Klammern durch Ihre eigenen Werte zu ersetzen. 

```azurecli-interactive
az storage account keys list \
  --resource-group <resource-group> \
  --account-name <storage-account>
```

---

Sie können jeden der beiden Schlüssel verwenden, um auf Azure Storage zuzugreifen. Im Allgemeinen empfiehlt es sich jedoch, den ersten Schlüssel zu verwenden und den zweiten Schlüssel zur Verwendung bei der Rotation von Schlüsseln zu reservieren.

Zum Anzeigen oder Lesen der Zugriffsschlüssel eines Kontos muss der Benutzer entweder ein Dienstadministrator sein oder ihm muss eine Azure-Rolle zugewiesen sein, die **Microsoft.Storage/storageAccounts/listkeys/action** enthält. Einige integrierte Azure-Rollen, die diese Aktion beinhalten, sind **Besitzer**, **Mitwirkender** und die Dienstrolle **Speicherkonto-Schlüsseloperator**. Weitere Informationen zur Dienstadministratorrolle finden Sie unter [Administratorrollen für klassische Abonnements, Azure-Rollen und Azure AD-Rollen](../../role-based-access-control/rbac-and-directory-admin-roles.md). Ausführliche Informationen zu integrierten Rollen für Azure Storage finden Sie im Artikel [In Azure integrierte Rollen für Azure RBAC](../../role-based-access-control/built-in-roles.md#storage) im Abschnitt **Storage**.

## <a name="use-azure-key-vault-to-manage-your-access-keys"></a>Verwenden von Azure Key Vault zum Verwalten Ihrer Zugriffsschlüssel

Microsoft empfiehlt die Verwendung von Azure Key Vault zum Verwalten und Rotieren Ihrer Zugriffsschlüssel. Ihre Anwendung kann auf sichere Weise auf die Schlüssel in Key Vault zugreifen, sodass Sie diese nicht mit dem Anwendungscode speichern müssen. Weitere Informationen zur Verwendung von Key Vault für die Schlüsselverwaltung finden Sie in den folgenden Artikeln:

- [Verwalten von Speicherkontoschlüsseln mit Azure Key Vault und PowerShell](../../key-vault/secrets/overview-storage-keys-powershell.md)
- [Verwalten von Speicherkontoschlüsseln mit Azure Key Vault und der Azure-Befehlszeilenschnittstelle](../../key-vault/secrets/overview-storage-keys.md)

## <a name="manually-rotate-access-keys"></a>Manuelles Rotieren von Zugriffsschlüsseln

Microsoft empfiehlt, Ihre Zugriffsschlüssel regelmäßig zu rotieren, um die Sicherheit Ihres Speicherkontos zu gewährleisten. Verwenden Sie nach Möglichkeit Azure Key Vault zum Verwalten Ihrer Zugriffsschlüssel. Wenn Sie Key Vault nicht verwenden, müssen Sie die Schlüssel manuell rotieren.

Es werden zwei Zugriffsschlüssel zugewiesen, sodass Sie Ihre Schlüssel rotieren können. Durch das Vorhandensein von zwei Schlüsseln ist sichergestellt, dass der Zugriff Ihrer Anwendung auf Azure Storage während des Prozesses erhalten bleibt.

> [!WARNING]
> Das erneute Generieren Ihrer Zugriffsschlüssel kann sich auf Anwendungen oder Azure-Dienste auswirken, die von dem Speicherkontoschlüssel abhängig sind. Alle Clients, die den Kontoschlüssel verwenden, um auf das Speicherkonto zuzugreifen, müssen aktualisiert werden, damit der neue Schlüssel verwendet wird, einschließlich Media Services, Cloud-, Desktop- und mobiler Anwendungen sowie grafischer Benutzeroberflächenanwendungen für Azure Storage wie [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/).

# <a name="portal"></a>[Portal](#tab/azure-portal)

Gehen Sie wie folgt vor, um Ihre Speicherkonto-Zugriffsschlüssel im Azure-Portal zu rotieren:

1. Aktualisieren Sie die Verbindungszeichenfolgen im Anwendungscode, sodass sie auf den sekundären Zugriffsschlüssel des Speicherkontos verweisen.
1. Navigieren Sie zum Speicherkonto im [Azure-Portal](https://portal.azure.com).
1. Wählen Sie unter **Einstellungen** die Option **Zugriffsschlüssel** aus.
1. Klicken Sie auf die Schaltfläche **Neu generieren** neben dem primären Zugriffsschlüssel, um den primären Zugriffsschlüssel für Ihr Speicherkonto neu zu generieren.
1. Aktualisieren Sie die Verbindungszeichenfolgen in Ihrem Code, um auf den neuen primären Zugriffsschlüssel zu verweisen.
1. Generieren Sie den sekundären Zugriffsschlüssel auf die gleiche Weise neu.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Gehen Sie wie folgt vor, um Ihre Speicherkonto-Zugriffsschlüssel mit PowerShell zu rotieren:

1. Aktualisieren Sie die Verbindungszeichenfolgen im Anwendungscode, sodass sie auf den sekundären Zugriffsschlüssel des Speicherkontos verweisen.
1. Rufen Sie den Befehl [New-AzStorageAccountKey](/powershell/module/az.storage/new-azstorageaccountkey) auf, um den primären Zugriffsschlüssel neu zu generieren, wie im folgenden Beispiel zu sehen:

    ```powershell
    New-AzStorageAccountKey -ResourceGroupName <resource-group> `
      -Name <storage-account> `
      -KeyName key1
    ```

1. Aktualisieren Sie die Verbindungszeichenfolgen in Ihrem Code, um auf den neuen primären Zugriffsschlüssel zu verweisen.
1. Generieren Sie den sekundären Zugriffsschlüssel auf die gleiche Weise neu. Verwenden Sie `key2` anstelle von `key1` als Schlüsselnamen, um den sekundären Schlüssel neu zu generieren.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Gehen Sie wie folgt vor, um Ihre Speicherkonto-Zugriffsschlüssel mit der Azure CLI zu rotieren:

1. Aktualisieren Sie die Verbindungszeichenfolgen im Anwendungscode, sodass sie auf den sekundären Zugriffsschlüssel des Speicherkontos verweisen.
1. Rufen Sie den Befehl [az storage account keys renew](/cli/azure/storage/account/keys#az-storage-account-keys-renew) auf, um den primären Zugriffsschlüssel neu zu generieren, wie im folgenden Beispiel zu sehen:

    ```azurecli-interactive
    az storage account keys renew \
      --resource-group <resource-group> \
      --account-name <storage-account>
      --key primary
    ```

1. Aktualisieren Sie die Verbindungszeichenfolgen in Ihrem Code, um auf den neuen primären Zugriffsschlüssel zu verweisen.
1. Generieren Sie den sekundären Zugriffsschlüssel auf die gleiche Weise neu. Verwenden Sie `key2` anstelle von `key1` als Schlüsselnamen, um den sekundären Schlüssel neu zu generieren.

---

> [!NOTE]
> Es wird empfohlen, in allen Ihren Anwendungen jeweils nur einen Schlüssel gleichzeitig zu verwenden. Wenn Sie „Key 1“ an einigen Stellen und „Key 2“ an anderen verwenden, können Sie die Verwendung der Schlüssel nicht wechseln, ohne dass einige Anwendungen den Zugriff verlieren.

Zum Rotieren der Zugriffsschlüssel eines Kontos muss der Benutzer entweder ein Dienstadministrator sein oder ihm muss eine Azure-Rolle zugewiesen sein, die **Microsoft.Storage/storageAccounts/regeneratekey/action** enthält. Einige integrierte Azure-Rollen, die diese Aktion beinhalten, sind **Besitzer**, **Mitwirkender** und die Dienstrolle **Speicherkonto-Schlüsseloperator**. Weitere Informationen zur Dienstadministratorrolle finden Sie unter [Administratorrollen für klassische Abonnements, Azure-Rollen und Azure AD-Rollen](../../role-based-access-control/rbac-and-directory-admin-roles.md). Ausführliche Informationen zu integrierten Azure-Rollen für Azure Storage finden Sie im Artikel [In Azure integrierte Rollen für Azure RBAC](../../role-based-access-control/built-in-roles.md#storage) im Abschnitt **Storage**.

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über Azure Storage-Konten](storage-account-overview.md)
- [Erstellen eines Speicherkontos](storage-account-create.md)
