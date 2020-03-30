---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 84cd8ed79281b005407b5a857398b5669635c072
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "68320490"
---
Wenn Sie einen HTTP-Proxy für ausgehende Anforderungen konfigurieren müssen, verwenden Sie diese zwei Argumente:

| Name | Datentyp | BESCHREIBUNG |
|--|--|--|
|HTTP_PROXY|string|Der zu verwendende Proxy, z. B. `http://proxy:8888`.<br>`<proxy-url>`|
|HTTP_PROXY_CREDS|string|Beliebige Anmeldeinformationen, die zur Authentifizierung bei dem Proxy erforderlich sind, z. B. Benutzername:Kennwort.|
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
HTTP_PROXY=<proxy-url> \
HTTP_PROXY_CREDS=<proxy-user>:<proxy-password> \
```
