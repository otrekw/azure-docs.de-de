---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/19/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4e958026b1167d65f47bddbe5e89ec4d6fed7ee3
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92217969"
---
Mit dem Befehl [az network vpn-connection show](/cli/azure/network/vpn-connection) können Sie überprüfen, ob die Verbindung erfolgreich hergestellt wurde. In dem Beispiel verweist „-Name“ auf den Namen der Verbindung, die Sie testen möchten. Während der Verbindungsherstellung wird als Verbindungsstatus „Connecting“ (Wird verbunden...) angezeigt. Sobald die Verbindung hergestellt wurde, ändert sich der Status in „Connected“ (Verbunden).

```azurecli-interactive
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```
