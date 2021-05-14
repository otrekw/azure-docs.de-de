---
title: Anwenden einer Azure Resource Manager-Sperre auf ein Speicherkonto
titleSuffix: Azure Storage
description: Erfahren Sie, wie Sie eine Azure Resource Manager-Sperre auf ein Speicherkonto anwenden.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 03/09/2021
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 79b88ad58a2eb95a48a140b3b98d606af495cb94
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104799784"
---
# <a name="apply-an-azure-resource-manager-lock-to-a-storage-account"></a>Anwenden einer Azure Resource Manager-Sperre auf ein Speicherkonto

Microsoft empfiehlt, alle Ihre Speicherkonten mit einer Azure Resource Manager-Sperre zu sperren, um das versehentliche oder böswillige Löschen der Speicherkonten zu verhindern. Es gibt zwei Arten von Azure Resource Manager-Sperren:

- Die **CannotDelete**-Sperre hindert Benutzer daran, ein Speicherkonto zu löschen, lässt aber das Lesen und Ändern der Konfiguration zu.
- Die **ReadOnly**-Sperre hindert Benutzer daran, ein Speicherkonto zu löschen und die zugehörige Konfiguration zu ändern, lässt jedoch das Lesen der Konfiguration zu.

Weitere Informationen zu Azure Resource Manager-Sperren finden Sie unter [Sperren von Ressourcen, um Änderungen zu verhindern](../../azure-resource-manager/management/lock-resources.md).

> [!CAUTION]
> Das Sperren eines Speicherkontos schützt Container oder Blobs innerhalb dieses Kontos nicht davor, gelöscht oder überschrieben zu werden. Weitere Informationen über den Schutz von Blobdaten finden Sie unter [Übersicht über den Datenschutz](../blobs/data-protection-overview.md).

## <a name="configure-an-azure-resource-manager-lock"></a>Konfigurieren einer Azure Resource Manager-Sperre

# <a name="azure-portal"></a>[Azure-Portal](#tab/portal)

Führen Sie die folgenden Schritte aus, um eine Sperre für ein Speicherkonto im Azure-Portal zu konfigurieren:

1. Navigieren Sie zum Speicherkonto im Azure-Portal.
1. Wählen Sie im Bereich **Einstellungen** die Option **Sperren** aus.
1. Wählen Sie **Hinzufügen**.
1. Geben Sie einen Namen für die Ressourcensperre und den Typ der Sperre an. Optional können Sie einen Hinweis über die Sperre hinzufügen.

    :::image type="content" source="media/lock-account-resource/lock-storage-account.png" alt-text="Screenshot der Sperrung eines Speicherkontos mit einer CannotDelete-Sperre":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Um eine Sperre für ein Speicherkonto über PowerShell zu konfigurieren, müssen Sie zunächst sicherstellen, dass das [PowerShell-Modul „Az“](https://www.powershellgallery.com/packages/Az) installiert ist. Rufen Sie dann wie im folgenden Beispiel gezeigt den Befehl [New-AzResourceLock](/powershell/module/az.resources/new-azresourcelock) auf, und geben Sie den Typ der zu erstellenden Sperre an:

```azurepowershell
New-AzResourceLock -LockLevel CanNotDelete `
    -LockName <lock> `
    -ResourceName <storage-account> `
    -ResourceType Microsoft.Storage/storageAccounts `
    -ResourceGroupName <resource-group>
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Um eine Sperre für ein Speicherkonto über die Azure-Befehlszeilenschnittstelle zu konfigurieren, rufen Sie wie im folgenden Beispiel gezeigt den Befehl [az lock create](/cli/azure/lock#az_lock_create) auf, und geben Sie dann den Typ der zu erstellenden Sperre an:

```azurecli
az lock create \
    --name <lock> \
    --resource-group <resource-group> \
    --resource <storage-account> \
    --lock-type CanNotDelete \
    --resource-type Microsoft.Storage/storageAccounts
```

---

## <a name="authorizing-data-operations-when-a-readonly-lock-is-in-effect"></a>Autorisieren von Datenvorgängen bei einer aktiven ReadOnly-Sperre

Wenn eine **ReadOnly**-Sperre auf ein Speicherkonto angewandt wird, ist der Vorgang [List Keys](/rest/api/storagerp/storageaccounts/listkeys) für das Speicherkonto gesperrt. Beim Vorgang **List Keys** handelt es sich um einen HTTPS-POST-Vorgang. Wenn für das Konto eine **ReadOnly**-Sperre festgelegt ist, werden alle POST-Vorgänge verhindert. Beim Vorgang **List Keys** werden die Kontozugriffsschlüssel zurückgegeben, die dann zum Lesen und Schreiben von Daten im Speicherkonto verwendet werden können.

Wenn ein Client zu dem Zeitpunkt, an dem die Sperre auf das Speicherkonto angewandt wird, im Besitz der Kontozugriffsschlüssel ist, kann dieser Client weiterhin die Schlüssel für den Zugriff auf die Daten verwenden. Clients, die keinen Zugriff auf die Schlüssel haben, müssen jedoch Azure Active Directory-Anmeldeinformationen (Azure AD) verwenden, um auf Blob- oder Warteschlangendaten im Speicherkonto zuzugreifen.

Benutzer im Azure-Portal sind möglicherweise betroffen, wenn eine **ReadOnly**-Sperre angewandt wird, wenn sie zuvor im Portal mit den Kontozugriffsschlüsseln auf Blob- oder Warteschlangendaten zugegriffen haben. Nach dem Anwenden der Sperre müssen Portalbenutzer Azure AD-Anmeldeinformationen verwenden, um im Portal auf Blob- oder Warteschlangendaten zuzugreifen. Hierfür müssen einem Benutzer zwei oder mehr RBAC-Rollen zugewiesen werden: mindestens die Azure Resource Manager-Rolle „Leser“ sowie eine der Azure Storage-Rollen für den Datenzugriff. Weitere Informationen finden Sie in den folgenden Artikeln:

- [Auswählen der Autorisierung des Zugriffs auf Blobdaten im Azure-Portal](../blobs/authorize-data-operations-portal.md)
- [Auswählen der Autorisierung des Zugriffs auf Warteschlangendaten im Azure-Portal](../queues/authorize-data-operations-portal.md)

Möglicherweise können Clients nicht mehr auf den Azure Files- oder Table Storage-Dienst zugreifen, wenn sie zuvor mit den Kontoschlüsseln darauf zugegriffen haben. Wenn Sie eine **ReadOnly**-Sperre auf ein Speicherkonto anwenden möchten, empfiehlt es sich, Workloads des Azure Files- und Table Storage-Diensts in ein Speicherkonto zu verschieben, das nicht mit einer **ReadOnly**-Sperre gesperrt ist.

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht zum Datenschutz](../blobs/data-protection-overview.md)
- [Sperren von Ressourcen, um Änderungen zu verhindern](../../azure-resource-manager/management/lock-resources.md)
