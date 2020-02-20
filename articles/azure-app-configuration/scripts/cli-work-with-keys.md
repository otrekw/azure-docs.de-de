---
title: 'Azure CLI-Skriptbeispiel: Verwenden von Schlüssel-Wert-Paaren in einem App Configuration-Speicher'
titleSuffix: Azure App Configuration
description: Informationen zur Verwendung von Schlüssel-Wert-Paaren in einem Azure App Configuration-Speicher
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: maiye
editor: ''
ms.service: azure-app-configuration
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: azure-app-configuration
ms.date: 11/08/2019
ms.author: lcozzens
ms.custom: mvc
ms.openlocfilehash: 931c2f738556404e2d58381f7152acb0613a057b
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77468149"
---
# <a name="work-with-key-values-in-an-azure-app-configuration-store"></a>Verwenden von Schlüssel-Wert-Paaren in einem Azure App Configuration-Speicher

Dieses Beispielskript erstellt ein neues Schlüssel-Wert-Paar in einem Azure App Configuration-Speicher, führt alle vorhandenen Schlüssel-Wert-Paare auf, aktualisiert den Wert des neu erstellten Schlüssels und löscht ihn schließlich.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Artikel die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Beispielskript

```azurecli-interactive
#!/bin/bash

appConfigName=myTestAppConfigStore
newKey="TestKey"
refKey="KeyVaultReferenceTestKey"
uri="[URL to value stored in Key Vault]"
uri2="[URL to another value stored in Key Vault]"

# Create a new key-value 
az appconfig kv set --name $appConfigName --key $newKey --value "Value 1"

# List current key-values
az appconfig kv list --name $appConfigName

# Update new key's value
az appconfig kv set --name $appConfigName --key $newKey --value "Value 2"

# List current key-values
az appconfig kv list --name $appConfigName

# Create a new key-value referencing a value stored in Azure Key Vault
az appconfig kv set --name $appConfigName --key $refKey --content-type "application/vnd.microsoft.appconfig.keyvaultref+json;charset=utf-8" --value "{\"uri\":\"$uri\"}"

# List current key-values
az appconfig kv list --name $appConfigName

# Update Key Vault reference
az appconfig kv set --name $appConfigName --key $refKey --value "{\"uri\":\"$uri2\"}"

# List current key-values
az appconfig kv list --name $appConfigName

# Delete new key
az appconfig kv delete  --name $appConfigName --key $newKey

# Delete Key Vault reference
az appconfig kv delete --name $appConfigName --key $refKey

# List current key-values
az appconfig kv list --name $appConfigName
```

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um Vorgänge für Schlüssel-Wert-Paare in einem App Configuration-Speicher auszuführen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az appconfig kv set](/cli/azure/appconfig/kv#az-appconfig-kv-set) | Erstellt oder aktualisiert ein Schlüssel-Wert-Paar. |
| [az appconfig kv list](/cli/azure/appconfig/kv#az-appconfig-kv-list) | Listet Schlüssel-Wert-Paare in einem App Configuration-Speicher auf. |
| [az appconfig kv delete](/cli/azure/appconfig/kv#az-appconfig-kv-delete) | Löscht ein Schlüssel-Wert-Paar. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere CLI-Skriptbeispiele für App Configuration finden Sie in den [CLI-Beispielen für Azure App Configuration](../cli-samples.md).
