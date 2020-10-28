---
title: include file
description: include file
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/27/2019
ms.author: cephalin
ms.custom: include file, devx-track-azurecli
ms.openlocfilehash: e2c5794e5ce6e23b60bff513562f69c9333d6e34
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92743791"
---
Sie können auf die Konsolenprotokolle zugreifen, die aus dem Container generiert werden.

Aktivieren Sie zuerst Containerprotokollierung, indem Sie den folgenden Befehl ausführen:

```azurecli-interactive
az webapp log config --name <app-name> --resource-group <resource-group-name> --docker-container-logging filesystem
```

Ersetzen Sie `<app-name>` und `<resource-group-name>` durch die entsprechenden Namen für Ihre Web-App.

Führen Sie den folgenden Befehl aus, um den Protokolldatenstrom anzuzeigen, nachdem die Containerprotokollierung aktiviert wurde:

```azurecli-interactive
az webapp log tail --name <app-name> --resource-group <resource-group-name>
```

Falls Sie nicht sofort Konsolenprotokolle sehen, können Sie es nach 30 Sekunden noch einmal versuchen.

Zum Beenden des Protokollstreamings können Sie jederzeit **STRG**+**C** drücken.

Sie können die Protokolldateien auch in einem Browser unter `https://<app-name>.scm.azurewebsites.net/api/logs/docker` untersuchen.
