---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/18/2020
ms.author: inhenkel
ms.custom: CLI
ms.openlocfilehash: 9e5b344f15a92e7ac40182f8fc7ae3ca667f63a4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "88608682"
---
<!--Create a media services asset REST-->

Der folgende Azure-REST-Befehl erstellt ein neues Media Services-Medienobjekt. Ersetzen Sie die Werte `subscriptionID`, `resourceGroup` und `amsAccountName` durch die aktuell verwendeten Werte. Benennen Sie Ihr Medienobjekt, indem Sie hier `assetName` festlegen.

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{amsAccountName}/assets/{assetName}?api-version=2018-07-01
```