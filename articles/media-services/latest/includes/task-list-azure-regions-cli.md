---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 05/13/2021
ms.author: inhenkel
ms.custom: CLI, devx-track-azurecli
ms.openlocfilehash: 0e216c73efce9a4f11f1ba0886ddcdb9324ed107
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110103183"
---
<!--List Azure regions CLI-->

Verwenden Sie diesen Befehl, um die für Ihr Konto verfügbaren Regionen aufzulisten.

```azurecli
az account list-locations --query "[].{DisplayName:displayName, Name:name}" -o table
```
