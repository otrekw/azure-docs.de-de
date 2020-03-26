---
title: include file
description: include file
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/02/2020
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 6fc1f4152b2e16e1597c018e5af6e0245b075c3b
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "78255830"
---
## <a name="robots933456-in-logs"></a>„robots933456“ in Protokollen

Möglicherweise wird die folgende Meldung in den Containerprotokollen angezeigt:

```
2019-04-08T14:07:56.641002476Z "-" - - [08/Apr/2019:14:07:56 +0000] "GET /robots933456.txt HTTP/1.1" 404 415 "-" "-"
```

Diese Meldung können Sie problemlos ignorieren. `/robots933456.txt` ist ein Dummy-URL-Pfad, den App Service verwendet, um zu überprüfen, ob der Container in der Lage ist, Anforderungen zu verarbeiten. Eine 404-Antwort zeigt lediglich an, dass der Pfad nicht vorhanden ist, informiert App Service aber darüber, dass der Container fehlerfrei und bereit ist, um auf Anforderungen zu antworten.

