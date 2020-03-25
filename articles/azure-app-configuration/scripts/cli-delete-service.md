---
title: 'Azure CLI-Skriptbeispiel: Löschen eines Azure App Configuration-Speichers'
titleSuffix: Azure App Configuration
description: Verwenden eines Azure CLI-Skripts zum Löschen eines Azure App Configuration-Speichers
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.devlang: azurecli
ms.topic: sample
ms.date: 02/19/2020
ms.author: lcozzens
ms.openlocfilehash: 7f73de459d8ce9f74e3925789af630b7c804d605
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "77523571"
---
# <a name="delete-an-azure-app-configuration-store"></a>Löschen eines Azure-App-Konfigurationspeichers

Mit diesem Beispielskript wird eine Instanz der Azure-App-Konfiguration gelöscht.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Artikel die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Beispielskript

```azurecli-interactive
#/bin/bash

# Delete an App Configuration store named myTestAppConfigStore from the Resource Group myResourceGroup
az appconfig delete --name myTestAppConfigStore --resource-group myResourceGroup
```

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um einen App Configuration-Speicher zu löschen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az appconfig delete](/cli/azure/appconfig#az-appconfig-delete) | Löscht eine App Configuration-Speicherressource. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere CLI-Skriptbeispiele für App Configuration finden Sie in den [CLI-Beispielen für Azure App Configuration](../cli-samples.md).
