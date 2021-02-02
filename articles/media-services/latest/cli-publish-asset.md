---
title: 'Azure CLI-Skriptbeispiel: Veröffentlichen eines Medienobjekts'
description: In diesem Artikel erfahren Sie, wie Sie mithilfe des Azure CLI-Skripts ein Medienobjekt veröffentlichen.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: how-to
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: devx-track-azurecli
ms.openlocfilehash: c283c39f1ea90275c42de1481a9cb9006f2b2c5f
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/27/2021
ms.locfileid: "98897087"
---
# <a name="cli-example-publish-an-asset"></a>CLI-Beispiel: Veröffentlichen eines Medienobjekts

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Das Azure CLI-Skript in diesem Artikel zeigt, wie ein Streaminglocator erstellt wird und Streaming-URLs zurückerhalten werden. 

## <a name="prerequisites"></a>Voraussetzungen 

[Erstellen Sie ein Media Services-Konto.](./create-account-howto.md)

[!INCLUDE [media-services-cli-instructions.md](../../../includes/media-services-cli-instructions.md)]

## <a name="example-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/media-services/publish-asset/Publish-Asset.sh "Publish an asset")]

## <a name="next-steps"></a>Nächste Schritte

[Media Services: Übersicht](media-services-overview.md)
