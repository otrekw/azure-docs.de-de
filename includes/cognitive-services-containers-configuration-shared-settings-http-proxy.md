---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: ce4cc68826b39b5707549afc799d2d214e8876c6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88638900"
---
Wenn Sie einen HTTP-Proxy für ausgehende Anforderungen konfigurieren müssen, verwenden Sie diese zwei Argumente:

| Name | Datentyp | BESCHREIBUNG |
|--|--|--|
|HTTPS_PROXY|string|Der zu verwendende Proxy, z. B. `https://proxy:8888`.<br>`<proxy-url>`|
|HTTPS_PROXY_CREDS|string|Beliebige Anmeldeinformationen, die zur Authentifizierung bei dem Proxy erforderlich sind, z. B. Benutzername:Kennwort.|
|`<proxy-user>`|string|Der Benutzer für den Proxy.|
|`<proxy-password>`|string|Das Kennwort, das dem `<proxy-user>` für den Proxy zugeordnet ist.|
||||


```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<endpoint> \
ApiKey=<api-key> \
HTTPS_PROXY=<proxy-url> \
HTTPS_PROXY_CREDS=<proxy-user>:<proxy-password> \
```
