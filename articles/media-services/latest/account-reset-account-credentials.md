---
title: 'Zurücksetzen Ihrer Kontoanmeldeinformationen: CLI'
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
ms.openlocfilehash: d5604f177484d33255d2923d72b00fae124c0f9a
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107783591"
---
# <a name="azure-cli-example-reset-the-account-credentials"></a>Azure CLI-Beispiel: Zurücksetzen der Kontoanmeldeinformationen

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Das Azure CLI-Skript in diesem Artikel zeigt, wie Sie die Anmeldeinformationen für Ihr Konto zurücksetzen und die Einstellungen von „app.config“ wiederherstellen.

## <a name="prerequisites"></a>Voraussetzungen

[Erstellen Sie ein Media Services-Konto.](./create-account-howto.md)

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
* [Zurücksetzen von Anmeldeinformationen](/cli/azure/ams/account/sp#az_ams_account_sp_reset_credentials)
