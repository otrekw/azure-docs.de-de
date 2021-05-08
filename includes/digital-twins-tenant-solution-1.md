---
author: baanders
description: Umfasst Datei, die eine Tokenlösung für die mandantenübergreifende Einschränkung mit Azure Digital Twins beschreibt
ms.service: digital-twins
ms.topic: include
ms.date: 4/13/2021
ms.author: baanders
ms.openlocfilehash: 5bc7e8af356ebe5968fe8a74783edc215788e5b4
ms.sourcegitcommit: 272351402a140422205ff50b59f80d3c6758f6f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2021
ms.locfileid: "107589335"
---
Eine Möglichkeit hierzu ist der folgende CLI-Befehl, wobei `<home-tenant-ID>` die ID des Azure AD-Mandanten ist, der die Azure Digital Twins-Instanz enthält:

```azurecli-interactive
az account get-access-token --tenant <home-tenant-ID> --resource https://digitaltwins.azure.net
```

Nach der Anforderung erhält die Identität ein Token, das für die *https://digitaltwins.azure.net* Azure AD-Ressource ausgestellt wurde, die über einen entsprechenden Mandanten-ID-Anspruch für die Azure Digital Twins verfügt. Die Verwendung dieses Tokens in API-Anforderungen oder mit Ihrem `Azure.Identity`-Code sollte der Verbundidentität den Zugriff auf die Azure Digital Twins ermöglichen.