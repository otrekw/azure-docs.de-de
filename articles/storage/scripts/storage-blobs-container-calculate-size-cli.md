---
title: Azure CLI-Skriptbeispiel – Berechnen der Größe des Blobcontainers | Microsoft-Dokumentation
description: Berechnen Sie die Größe eines Containers in Azure Blob Storage, indem Sie die Größe der Blobs im Container addieren.
services: storage
author: tamram
ms.service: storage
ms.subservice: blobs
ms.devlang: cli
ms.topic: sample
ms.date: 06/28/2017
ms.author: tamram
ms.custom: devx-track-azurecli
ms.openlocfilehash: 45712632ebfb2da4b713038503965ce908c1dfc6
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "87498887"
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>Berechnen der Größe eines Blob Storage-Containers

Mit diesem Skript berechnen Sie die Größe eines Containers in Azure Blob Storage, indem Sie die Größe der Blobs im Container addieren.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> Dieses CLI-Skript gibt eine geschätzte Größe für den Container an und sollte nicht für Abrechnungsberechnungen verwendet werden.

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/storage/calculate-container-size/calculate-container-size.sh?highlight=2-3 "Calculate container size")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den Container und alle zugehörigen Ressourcen zu entfernen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um die Größe des Blob Storage-Containers zu berechnen. Jedes Element in der Tabelle ist mit der Dokumentation des jeweiligen Befehls verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage blob upload](/cli/azure/storage/account) | Lädt die Dateien in einen Azure Blob Storage-Container hoch. |
| [az storage blob list](/cli/azure/storage/account/keys) | Listet die Dateien in einem Azure Blob Storage-Container auf. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere CLI-Skriptbeispiele für Speicher finden Sie in den [Azure CLI-Beispielen für Azure Blob Storage](../blobs/storage-samples-blobs-cli.md).
