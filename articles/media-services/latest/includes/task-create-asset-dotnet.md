---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/18/2020
ms.author: inhenkel
ms.custom: dotnet
ms.openlocfilehash: 820353ffbb5e23e27c3039ad27a8f6507b9d2167
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608652"
---
<!--Create a media services asset REST-->

Mit dem folgenden Azure .NET-Befehl wird ein neues Media Services-Medienobjekt erstellt. Ersetzen Sie die Werte `subscriptionID`, `resourceGroup` und `amsAccountName` durch die aktuell verwendeten Werte. Benennen Sie Ihr Medienobjekt, indem Sie hier `assetName` festlegen.

```csharp
 Asset asset = await client.Assets.CreateOrUpdateAsync(resourceGroupName, accountName, assetName, new Asset());
```
