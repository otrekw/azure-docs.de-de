---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/18/2020
ms.author: inhenkel
ms.custom: CLI
ms.openlocfilehash: 9e5b344f15a92e7ac40182f8fc7ae3ca667f63a4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88608682"
---
<!--Create a media services asset REST-->

Der folgende Azure-REST-Befehl erstellt ein neues Media Services-Medienobjekt. Ersetzen Sie die Werte `subscriptionID`, `resourceGroup` und `amsAccountName` durch die aktuell verwendeten Werte. Benennen Sie Ihr Medienobjekt, indem Sie hier `assetName` festlegen.

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{amsAccountName}/assets/{assetName}?api-version=2018-07-01
```