---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/17/2020
ms.author: inhenkel
ms.custom: CLI
ms.openlocfilehash: 5c0341087cdd348e973da5faaa1f90081780c9c8
ms.sourcegitcommit: 37afde27ac137ab2e675b2b0492559287822fded
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2020
ms.locfileid: "88602530"
---
<!--Create a media services account -->

Der folgende Azure CLI-Befehl erstellt ein neues Media Services-Konto. Sie können die folgenden Werte ersetzen: `amsaccount`, `storageaccountforams` (muss dem Wert für Ihr Speicherkonto entsprechen) und `amsResourceGroup` (muss dem Wert für Ihre Ressourcengruppe entsprechen).

```azurecli
az ams account create --name amsaccount -g amsResourceGroup --storage-account storageaccountforams -l westus2
```