---
title: 'Azure CLI-Skriptbeispiel: Zurücksetzen der Anmeldeinformationen für Ihr Konto | Microsoft-Dokumentation'
description: Verwenden Sie das Azure CLI-Skript zum Zurücksetzen der Anmeldeinformationen für Ihr Konto und zum Wiederherstellen der Einstellungen von „app.config“.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/20/2019
ms.author: juliako
ms.custom: devx-track-azurecli
ms.openlocfilehash: 16eaf5ab9b0af97e18a7253d132516a16003d25b
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87489978"
---
# <a name="azure-cli-example-reset-the-account-credentials"></a>Azure CLI-Beispiel: Zurücksetzen der Kontoanmeldeinformationen

Das Azure CLI-Skript in diesem Artikel zeigt, wie Sie die Anmeldeinformationen für Ihr Konto zurücksetzen und die Einstellungen von „app.config“ wiederherstellen.

## <a name="prerequisites"></a>Voraussetzungen

[Erstellen Sie ein Media Services-Konto.](./create-account-howto.md)

[!INCLUDE [media-services-cli-instructions.md](../../../includes/media-services-cli-instructions.md)]

## <a name="example-script"></a>Beispielskript

```azurecli-interactive
# Update the following variables for your own settings:
resourceGroup=amsResourceGroup
amsAccountName=amsmediaaccountname

az ams account sp reset-credentials \
  --account-name $amsAccountName \
  --resource-group $resourceGroup
 ```

## <a name="next-steps"></a>Nächste Schritte

* [az ams](/cli/azure/ams)
* [Zurücksetzen von Anmeldeinformationen](/cli/azure/ams/account/sp#az-ams-account-sp-reset-credentials)
