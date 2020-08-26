---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/17/2020
ms.author: inhenkel
ms.custom: CLI
ms.openlocfilehash: d567a4f7d9b9429887d6396cb2b03dfc34c6ac93
ms.sourcegitcommit: 37afde27ac137ab2e675b2b0492559287822fded
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2020
ms.locfileid: "88602391"
---
<!-- Create a resource group -->

Verwenden Sie den folgenden Befehl, um eine Ressourcengruppe zu erstellen. Wählen Sie die geografische Region aus, in der die Medien- und Metadaten-Datensätze für Ihr Media Services-Konto gespeichert werden sollen. Dieser Bereich wird zum Verarbeiten und Streamen Ihrer Medien verwendet.

```azurecli
az group create --name amsResourceGroup --location westus2
```
