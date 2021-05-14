---
author: baanders
description: Umfasst Datei, die eine Tokenlösung für die mandantenübergreifende Einschränkung mit Azure Digital Twins beschreibt
ms.service: digital-twins
ms.topic: include
ms.date: 4/13/2021
ms.author: baanders
ms.openlocfilehash: 08c48b3600a975f43a5ad94e2cc6efd88047d0d8
ms.sourcegitcommit: a5dd9799fa93c175b4644c9fe1509e9f97506cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108205652"
---
Eine Möglichkeit hierzu ist der folgende CLI-Befehl, wobei `<home-tenant-ID>` die ID des Azure AD-Mandanten ist, der die Azure Digital Twins-Instanz enthält:

```azurecli-interactive
az account get-access-token --tenant <home-tenant-ID> --resource https://digitaltwins.azure.net
```

Nach der Anforderung erhält die Identität ein Token, das für die Azure AD-Ressource `https://digitaltwins.azure.net` ausgestellt wurde, die über einen entsprechenden Mandanten-ID-Anspruch für die Azure Digital Twins-Instanz verfügt. Die Verwendung dieses Tokens in API-Anforderungen oder mit Ihrem `Azure.Identity`-Code sollte der Verbundidentität den Zugriff auf die Azure Digital Twins ermöglichen.
