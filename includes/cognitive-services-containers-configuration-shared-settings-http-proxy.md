---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 7f51b7fe95445cbd3a8aff530f89ce55b5abfb1e
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105629969"
---
Wenn Sie einen HTTP-Proxy für ausgehende Anforderungen konfigurieren müssen, verwenden Sie diese zwei Argumente:

| Name | Datentyp | BESCHREIBUNG |
|--|--|--|
|HTTP_PROXY|string|Der zu verwendende Proxy, z. B. `http://proxy:8888`.<br>`<proxy-url>`|
|HTTP_PROXY_CREDS|Zeichenfolge|Beliebige Anmeldeinformationen, die zur Authentifizierung bei dem Proxy erforderlich sind, z. B. `username:password`. Dieser Wert **muss in Kleinbuchstaben eingegeben werden**. |
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
