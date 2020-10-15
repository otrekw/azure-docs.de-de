---
title: 'Azure CLI-Skriptbeispiel: Zurücksetzen der Anmeldeinformationen für Ihr Konto | Microsoft-Dokumentation'
description: Verwenden Sie das Azure CLI-Skript zum Zurücksetzen der Anmeldeinformationen für Ihr Konto und zum Wiederherstellen der Einstellungen von „app.config“.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: troubleshooting
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: devx-track-azurecli
ms.openlocfilehash: da997ec71655231876749d9f3aa65ba06fd3a1f8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89295834"
---
# <a name="azure-cli-example-reset-the-account-credentials"></a>Azure CLI-Beispiel: Zurücksetzen der Kontoanmeldeinformationen

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

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
