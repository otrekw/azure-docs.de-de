---
title: include file
description: include file
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/29/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 458cd36a35ea37b2a317fe98fdeb5acc69a36ce8
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91822773"
---
Um eine direkte SSH-Sitzung mit Ihrem Container zu öffnen, sollte Ihre App ausgeführt werden.

Fügen Sie die folgende URL in Ihren Browser ein, und ersetzen Sie `<app-name>` durch den Namen Ihrer App:

```
https://<app-name>.scm.azurewebsites.net/webssh/host
```

Wenn Sie noch nicht authentifiziert sind, müssen Sie sich mit Ihrem Azure-Abonnement für die Verbindung authentifizieren. Nach der Authentifizierung wird im Browser eine Shell angezeigt, über die Sie Befehle innerhalb Ihres Containers ausführen können.

![SSH-Verbindung](./media/app-service-web-ssh-connect-no-h/app-service-linux-ssh-connection.png)
