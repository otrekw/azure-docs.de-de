---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/09/2018
ms.author: wesmc
ms.openlocfilehash: e5166934c4f7e1ecc917d2a5afcbb26ea9a637a2
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "67177895"
---
## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach der Ausführung des Skriptbeispiels können Sie den folgenden Befehl verwenden, um die Ressourcengruppe, die Azure Cache for Redis-Instanz und alle dazugehörigen Ressourcen in der Ressourcengruppe zu entfernen.

```azurecli
az group delete --name contosoGroup
```