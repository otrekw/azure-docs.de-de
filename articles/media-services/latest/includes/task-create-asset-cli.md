---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/18/2020
ms.author: inhenkel
ms.custom: CLI, devx-track-azurecli
ms.openlocfilehash: 76c6a67b0bad6095a6340ccab4fbd49c7cad1698
ms.sourcegitcommit: 5ce88326f2b02fda54dad05df94cf0b440da284b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107893156"
---
<!--Create a media services asset CLI-->

Der folgende Azure CLI-Befehl erstellt ein neues Media Services-Medienobjekt. Ersetzen Sie die Werte `assetName`, `amsAccountName` und `resourceGroup` durch aktuell verwendete Werte.

```azurecli
az ams asset create -n assetName -a amsAccountName -g resourceGroup
```
