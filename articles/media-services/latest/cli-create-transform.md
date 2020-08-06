---
title: 'Azure CLI-Skriptbeispiel: Erstellen einer Transformation | Microsoft-Dokumentation'
description: Transformationen beschreiben einen einfachen Workflow von Aufgaben zur Verarbeitung Ihrer Video- oder Audiodateien (der häufig als „Rezept“ bezeichnet wird). Das Azure CLI-Skript in diesem Artikel zeigt, wie eine Transformation erstellt wird.
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
ms.date: 05/01/2019
ms.author: juliako
ms.custom: devx-track-azurecli
ms.openlocfilehash: 538f48d427a4d8b51f77ae50bb0bee95909f0b09
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87494467"
---
# <a name="cli-example-create-a-transform"></a>CLI-Beispiel: Erstellen einer Transformation

Das Azure CLI-Skript in diesem Artikel zeigt, wie eine Transformation erstellt wird. Transformationen beschreiben einen einfachen Workflow von Aufgaben zur Verarbeitung Ihrer Video- oder Audiodateien (der häufig als „Rezept“ bezeichnet wird). Sie müssen immer überprüfen, ob eine Transformation mit dem gewünschten Namen und „Rezept“ bereits vorhanden ist. Wenn dies der Fall ist, sollten Sie diese erneut verwenden.

## <a name="prerequisites"></a>Voraussetzungen 

[Erstellen Sie ein Media Services-Konto.](./create-account-howto.md)

[!INCLUDE [media-services-cli-instructions.md](../../../includes/media-services-cli-instructions.md)]

> [!NOTE]
> Sie können nur einen Pfad zur benutzerdefinierten JSON-Datei mit der Standard-Encoder-Voreinstellung für [StandardEncoderPreset](/rest/api/media/transforms/createorupdate#standardencoderpreset) angeben. Sehen Sie sich das Beispiel unter [Codieren mit einer benutzerdefinierten Transformation: CLI](custom-preset-cli-howto.md) an.
>
> Bei der Verwendung von [BuiltInStandardEncoderPreset](/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) können Sie keinen Dateinamen übergeben.

## <a name="example-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/media-services/create-transform/Create-Transform.sh "Create a transform")]

## <a name="next-steps"></a>Nächste Schritte

[az ams transform (CLI)](/cli/azure/ams/transform?view=azure-cli-latest)
