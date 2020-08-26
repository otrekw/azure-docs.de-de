---
title: Hochladen von Inhalten in ein Azure Media Services-Medienobjekt mithilfe der Azure CLI
description: Das Azure CLI-Skript in diesem Thema zeigt, wie ein Media Services-Medienobjekt erstellt wird, in das Inhalt hochgeladen werden kann.
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
ms.date: 08/18/2020
ms.author: inhenkel
ms.custom: devx-track-azurecli
ms.openlocfilehash: 4d1e5e48e70cee1cf75b1d6605837695d26dcbf0
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608679"
---
# <a name="create-an-asset"></a>Erstellen eines Medienobjekts

In diesem Artikel erfahren Sie, wie Sie ein Media Services-Medienobjekt erstellen.  Sie verwenden ein Medienobjekt zum Speichern von Medieninhalten für die Codierung und das Streaming.  Weitere Informationen zu Media Services-Medienobjekten finden Sie unter [Medienobjekte in Azure Media Services v3](assets-concept.md).

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte unter [Erstellen eines Media Services-Kontos](./create-account-howto.md) aus, um das erforderliche Media Services-Konto und die erforderliche Ressourcengruppe zum Erstellen eines Medienobjekts zu erstellen.

## <a name="methods"></a>Methoden

## <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/cli/)

[!INCLUDE [Create an asset with CLI](./includes/task-create-asset-cli.md)]

## <a name="cli-shell"></a>[CLI-Shell](#tab/clishell/)

[!INCLUDE [media-services-cli-instructions.md](../../../includes/media-services-cli-instructions.md)]

## <a name="example-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/media-services/create-asset/Create-Asset.sh "Create an asset")]

## <a name="rest"></a>[REST](#tab/rest/)

### <a name="using-rest"></a>Verwenden von REST

[!INCLUDE [media-services-cli-instructions.md](./includes/task-create-asset-rest.md)]

### <a name="using-curl"></a>Verwenden von cURL

[!INCLUDE [media-services-cli-instructions.md](./includes/task-create-asset-curl.md)]

## <a name="net"></a>[.NET](#tab/net/)

[!INCLUDE [media-services-cli-instructions.md](./includes/task-create-asset-dotnet.md)]

---

## <a name="next-steps"></a>Nächste Schritte

[Media Services: Übersicht](media-services-overview.md)
