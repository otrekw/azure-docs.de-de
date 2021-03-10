---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 07/20/2020
ms.author: msmbaldwin
ms.openlocfilehash: d1a67a131a94bc017eaf2199546ef08f0291cd54
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102244494"
---
Erstellen Sie ein Geheimnis mit dem Namen **mySecret** und dem Wert **Success!** . Bei einem Geheimnis kann es sich beispielsweise um ein Kennwort, eine SQL-Verbindungszeichenfolge oder um beliebige andere Informationen handeln, die sowohl sicher aufbewahrt werden als auch für Ihre Anwendung verfügbar sein müssen. 

Fügen Sie Ihrem neu erstellten Schlüsseltresor mit dem Azure CLI-Befehl [az keyvault secret set](/cli/azure/keyvault/secret#az-keyvault-secret-set) ein Geheimnis hinzu.

```azurecli
az keyvault secret set --vault-name "<your-unique-keyvault-name>" --name "mySecret" --value "Success!"
```