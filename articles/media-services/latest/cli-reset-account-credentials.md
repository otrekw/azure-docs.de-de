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
ms.openlocfilehash: 63f2abe7a3890efbaf4c79186467a3eb20d8afda
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2020
ms.locfileid: "80382986"
---
# <a name="azure-cli-example-reset-the-account-credentials"></a>Azure CLI-Beispiel: Zurücksetzen der Kontoanmeldeinformationen

Das Azure CLI-Skript in diesem Artikel zeigt, wie Sie die Anmeldeinformationen für Ihr Konto zurücksetzen und die Einstellungen von „app.config“ wiederherstellen.

## <a name="prerequisites"></a>Voraussetzungen

[Erstellen Sie ein Media Services-Konto.](create-account-cli-how-to.md)

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
