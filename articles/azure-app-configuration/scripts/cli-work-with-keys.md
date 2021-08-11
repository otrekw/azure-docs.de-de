---
title: 'Azure CLI-Skriptbeispiel: Verwenden von Schlüssel-Wert-Paaren in einem App Configuration-Speicher'
titleSuffix: Azure App Configuration
description: Hier erfahren Sie, wie Sie ein Azure CLI-Skript verwenden, um Schlüsselwerte im App Configuration-Speicher zu erstellen, anzuzeigen, zu aktualisieren und zu löschen.
services: azure-app-configuration
author: AlexandraKemperMS
ms.service: azure-app-configuration
ms.devlang: azurecli
ms.topic: sample
ms.date: 02/19/2020
ms.author: alkemper
ms.custom: devx-track-azurecli
ms.openlocfilehash: 144b324dcde0c0bdcbaf0a64840b56c6c618a19e
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114441583"
---
# <a name="work-with-key-values-in-an-azure-app-configuration-store"></a>Verwenden von Schlüssel-Wert-Paaren in einem Azure App Configuration-Speicher

In diesem Beispielskript wird Folgendes gezeigt:
* Erstellen eines neuen Schlüssel-Wert-Paars
* Auflisten aller vorhandenen Schlüssel-Wert-Paare
* Aktualisieren des Werts eines neu erstellten Schlüssels
* Löschen des neuen Schlüssel-Wert-Paars

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - Für dieses Tutorial ist mindestens Version 2.0 der Azure CLI erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert.
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
az appconfig kv set-keyvault  --name $appConfigName --key $refKey --secret-identifier $uri

# List current key-values
az appconfig kv list --name $appConfigName

# Update Key Vault reference
az appconfig kv set-keyvault --name $appConfigName --key $refKey --secret-identifier $uri2

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

Die folgende Tabelle enthält die im Beispielskript verwendeten Befehle: 

| Get-Help | Notizen |
|---|---|
| [az appconfig kv set](/cli/azure/appconfig/kv#az_appconfig_kv_set) | Dient zum Erstellen oder Aktualisieren eines Schlüssel-Wert-Paars. |
| [az appconfig kv list](/cli/azure/appconfig/kv#az_appconfig_kv_list) | Dient zum Auflisten von Schlüssel-Wert-Paaren in einem App Configuration-Speicher. |
| [az appconfig kv delete](/cli/azure/appconfig/kv#az_appconfig_kv_delete) | Dient zum Löschen eines Schlüssel-Wert-Paars. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere CLI-Skriptbeispiele für App Configuration finden Sie in den [CLI-Beispielen für Azure App Configuration](../cli-samples.md).
