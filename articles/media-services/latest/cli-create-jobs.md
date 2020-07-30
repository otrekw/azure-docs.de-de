---
title: 'Azure CLI-Skriptbeispiel: Erstellen und Übermitteln eines Auftrags | Microsoft-Dokumentation'
description: Das Azure CLI-Skript in diesem Thema zeigt, wie Sie einen Auftrag mithilfe einer HTTPS-URL an eine einfache Codierungstransformation übermitteln.
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
ms.openlocfilehash: b9d9f66b638eba2503f1650ba64fcd6b4652dc24
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87023415"
---
# <a name="cli-example-create-and-submit-a-job"></a>CLI-Beispiel: Erstellen und Übermitteln eines Auftrags

Wenn Sie in Media Services v3 Aufträge zur Verarbeitung Ihrer Videos übermitteln, müssen Sie Media Services mitteilen, wo sich das Eingabevideo befindet. Eine der Optionen ist die Angabe einer HTTPS-URL als Auftragseingabe (wie in diesem Artikel gezeigt). 

## <a name="prerequisites"></a>Voraussetzungen 

[Erstellen Sie ein Media Services-Konto.](./create-account-howto.md)

[!INCLUDE [media-services-cli-instructions.md](../../../includes/media-services-cli-instructions.md)]

## <a name="example-script"></a>Beispielskript

Beim Ausführen von `az ams job start` können Sie eine Bezeichnung für die Ausgabe des Auftrags festlegen. Die Bezeichnung kann später verwendet werden, um den Verwendungszweck des Ausgabemedienobjekts festzustellen. 

- Wenn Sie der Bezeichnung einen Wert zuweisen, legen Sie „--output-assets“ auf „assetname=label“ fest.
- Wenn Sie der Bezeichnung keinen Wert zuweisen, legen Sie „--output-assets“ auf „assetname=“ fest.
  Beachten Sie, dass Sie `output-assets` ein Gleichheitszeichen (=) hinzufügen. 

```azurecli
az ams job start \
  --name testJob001 \
  --transform-name testEncodingTransform \
  --base-uri 'https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/' \
  --files 'Ignite-short.mp4' \
  --output-assets testOutputAssetName= \
  -a amsaccount \
  -g amsResourceGroup 
```

Sie erhalten eine Antwort, die in etwa wie folgt aussieht:

```
{
  "correlationData": {},
  "created": "2019-02-15T05:08:26.266104+00:00",
  "description": null,
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/transforms/testEncodingTransform/jobs/testJob001",
  "input": {
    "baseUri": "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/",
    "files": [
      "Ignite-short.mp4"
    ],
    "label": null,
    "odatatype": "#Microsoft.Media.JobInputHttp"
  },
  "lastModified": "2019-02-15T05:08:26.266104+00:00",
  "name": "testJob001",
  "outputs": [
    {
      "assetName": "testOutputAssetName",
      "error": null,
      "label": "",
      "odatatype": "#Microsoft.Media.JobOutputAsset",
      "progress": 0,
      "state": "Queued"
    }
  ],
  "priority": "Normal",
  "resourceGroup": "amsResourceGroup",
  "state": "Queued",
  "type": "Microsoft.Media/mediaservices/transforms/jobs"
}
```

## <a name="next-steps"></a>Nächste Schritte

[az ams job (CLI)](/cli/azure/ams/job?view=azure-cli-latest)
