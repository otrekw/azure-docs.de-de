---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/18/2020
ms.author: inhenkel
ms.custom: dotnet
ms.openlocfilehash: dcd2cda3bad2a13a83c5f3f6700e5a57471e2065
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88653879"
---
<!--Create a media services asset REST-->

Mit dem folgenden Azure .NET-Befehl wird ein neues Media Services-Medienobjekt erstellt. Ersetzen Sie die Werte `subscriptionID`, `resourceGroup` und `amsAccountName` durch aktuell verwendete Werte. Benennen Sie Ihr Medienobjekt, indem Sie hier `assetName` festlegen.

[!code-csharp[Main](../../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]
