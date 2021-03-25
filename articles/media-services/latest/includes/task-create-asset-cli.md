---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/18/2020
ms.author: inhenkel
ms.custom: CLI
ms.openlocfilehash: 9f079be76d4d75f05b4a8e132ac425255eed8558
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "88605969"
---
<!--Create a media services asset CLI-->

Der folgende Azure CLI-Befehl erstellt ein neues Media Services-Medienobjekt. Ersetzen Sie die Werte `assetName`, `amsAccountName` und `resourceGroup` durch aktuell verwendete Werte.

```azurecli
az ams asset create -n assetName -a amsAccountName -g resourceGroup
```
