---
title: Einrichten einer Azure Key Vault-Instanz für Linux-VMs
description: Einrichten eines Schlüsseltresors, der für einen mit Azure Resource Manager erstellten virtuellen Computer verwendet werden soll, mit der Azure CLI.
author: mimckitt
manager: vashan
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: article
ms.date: 02/24/2017
ms.author: mimckitt
ms.openlocfilehash: 9ae486ee522982b116af58cfb7cbfbca66a7ef4a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "81458742"
---
# <a name="how-to-set-up-key-vault-for-virtual-machines-with-the-azure-cli"></a>Einrichten eines Schlüsseltresors für virtuelle Computer mithilfe der Azure CLI

Im Azure Resource Manager-Stapel werden Geheimnisse/Zertifikate als Ressourcen modelliert, die durch Key Vault bereitgestellt werden. Weitere Informationen über Azure Schlüsseltresore finden Sie unter [Was ist der Azure-Schlüsseltresor?](../../key-vault/general/overview.md) Damit Key Vault mit Azure Resource Manager-VMs verwendet werden kann, müssen Sie die *EnabledForDeployment*-Eigenschaft in Key Vault auf TRUE festlegen. Dieser Artikel zeigt das Einrichten eines Schlüsseltresors mithilfe von Azure CLI für die Verwendung mit virtuellen Azure-Computern (Azure-VMs). 

Zum Ausführen dieser Schritte muss die neueste Version der [Azure CLI](/cli/azure/install-az-cli2) installiert sein, und Sie müssen mithilfe von [az login](/cli/azure/reference-index) bei einem Azure-Konto angemeldet sein.

## <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors
Erstellen eines Schlüsseltresors und Zuweisen der Bereitstellungsrichtlinie mit [az keyvault create](/cli/azure/keyvault). Das folgende Beispiel erstellt den Schlüsseltresor `myKeyVault` in der Ressourcengruppe `myResourceGroup`:

```azurecli
az keyvault create -l westus -n myKeyVault -g myResourceGroup --enabled-for-deployment true
```

## <a name="update-a-key-vault-for-use-with-vms"></a>Aktualisieren eines Schlüsseltresors für die Verwendung mit virtuellen Computern
Sie legen die Bereitstellungsrichtlinie für einen vorhandenen Schlüsseltresor mit [az keyvault update](/cli/azure/keyvault) fest. Das folgende Beispiel aktualisiert den Schlüsseltresor `myKeyVault` in der Ressourcengruppe `myResourceGroup`:

```azurecli
az keyvault update -n myKeyVault -g myResourceGroup --set properties.enabledForDeployment=true
```

## <a name="use-templates-to-set-up-key-vault"></a>Verwenden von Vorlagen zum Einrichten des Schlüsseltresors
Wenn Sie eine Vorlage verwenden, müssen Sie die `enabledForDeployment`-Eigenschaft für die Key Vault-Ressource wie folgt auf `true` festlegen:

```json
{
    "type": "Microsoft.KeyVault/vaults",
    "name": "ContosoKeyVault",
    "apiVersion": "2015-06-01",
    "location": "<location-of-key-vault>",
    "properties": {
    "enabledForDeployment": "true",
    ....
    ....
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Optionen, die Sie beim Erstellen eines Schlüsseltresors mithilfe von Vorlagen konfigurieren können, finden Sie unter [Create a Key Vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/) (Erstellen eines Schlüsseltresors).
