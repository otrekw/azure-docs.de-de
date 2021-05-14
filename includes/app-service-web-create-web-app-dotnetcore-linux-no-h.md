---
title: include file
description: include file
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 04/22/2020
ms.author: cephalin
ms.custom: include file, devx-track-azurecli
ms.openlocfilehash: 8f18b31b7e3618d4a39923792dc743c9a6782295
ms.sourcegitcommit: 5ce88326f2b02fda54dad05df94cf0b440da284b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107893143"
---
Erstellen Sie eine [Web-App](../articles/app-service/overview.md#app-service-on-linux) im App Service-Plan `myAppServicePlan`. 

In Cloud Shell können Sie den Befehl [`az webapp create`](/cli/azure/webapp) verwenden. Ersetzen Sie im folgenden Beispiel `<app-name>` durch einen global eindeutigen App-Namen (gültige Zeichen sind `a-z`, `0-9` und `-`). Die Runtime ist auf `DOTNETCORE|3.1` festgelegt. Führen Sie [`az webapp list-runtimes --linux`](/cli/azure/webapp) aus, um alle unterstützten Laufzeiten anzuzeigen. 

```azurecli-interactive
# Bash
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --runtime "DOTNETCORE|3.1" --deployment-local-git
# PowerShell
az --% webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --runtime "DOTNETCORE|3.1" --deployment-local-git
```

Nach Erstellung der Web-App zeigt die Azure CLI eine Ausgabe wie im folgenden Beispiel an:

<pre>
Local git is configured with url of 'https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app-name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
</pre>

Sie haben eine leere Web-App in einem Linux-Container mit aktivierter Git-Bereitstellung erstellt.

> [!NOTE]
> Die URL des Git-Remotespeicherorts wird in der `deploymentLocalGitUrl`-Eigenschaft im Format `https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git` angezeigt. Speichern Sie diese URL, da Sie sie später noch benötigen.
>
