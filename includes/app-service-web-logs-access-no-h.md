---
title: Datei einfügen
description: include file
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 07/13/2021
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 766e135cbccf86884786c2b888fcb1ea9d98ecc7
ms.sourcegitcommit: 6f4378f2afa31eddab91d84f7b33a58e3e7e78c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2021
ms.locfileid: "113688628"
---
Aktivieren Sie die Diagnoseprotokollierung, indem Sie den folgenden Befehl in [Cloud Shell](https://shell.azure.com)ausführen, um auf die Konsolenprotokolle zuzugreifen, die innerhalb des Anwendungscodes in App Service generiert wurden:

```azurecli-interactive
az webapp log config --resource-group <resource-group-name> --name <app-name> --docker-container-logging filesystem --level Verbose
```

Mögliche Werte für `--level` sind `Error`, `Warning` `Info` oder `Verbose`. Jede nachfolgende Ebene enthält die vorherige Ebene. Beispiel: `Error` enthält nur Fehlermeldungen, und `Verbose` enthält alle Meldungen.

Führen Sie den folgenden Befehl aus, um den Protokolldatenstrom anzuzeigen, nachdem die Diagnoseprotokollierung aktiviert wurde:

```azurecli-interactive
az webapp log tail --resource-group <resource-group-name> --name <app-name>
```

Falls Sie nicht sofort Konsolenprotokolle sehen, können Sie es nach 30 Sekunden noch einmal versuchen.

> [!NOTE]
> Sie können die Protokolldateien auch im Browser unter `https://<app-name>.scm.azurewebsites.net/api/logs/docker` untersuchen.

Um das Protokollstreaming jederzeit zu beenden, geben Sie `Ctrl`+`C` ein.
