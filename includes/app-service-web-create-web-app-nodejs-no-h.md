---
title: Datei einfügen
description: include file
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file, devx-track-azurecli
ms.openlocfilehash: f6fbfd521381706c0bc592ceb516e1864e96e4b8
ms.sourcegitcommit: 6f4378f2afa31eddab91d84f7b33a58e3e7e78c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2021
ms.locfileid: "113688638"
---
Erstellen Sie eine [Web-App](../articles/app-service/overview.md#app-service-on-linux) im App Service-Plan `myAppServicePlan`. 

In Cloud Shell können Sie den Befehl [`az webapp create`](/cli/azure/webapp) verwenden. Ersetzen Sie im folgenden Beispiel `<app-name>` durch einen global eindeutigen App-Namen (gültige Zeichen sind `a-z`, `0-9` und `-`). Die Runtime ist auf `NODE|14-lts` festgelegt. Führen Sie [`az webapp list-runtimes`](/cli/azure/webapp#az_webapp_list_runtimes) aus, um alle unterstützten Laufzeiten anzuzeigen. 

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --runtime 'NODE|14-lts' --deployment-local-git
```

Nach Erstellung der Web-App zeigt die Azure CLI eine Ausgabe wie im folgenden Beispiel an:

<pre>
Local git is configured with url of 'https://&lt;username&gt;@&lt;app-name&gt;.scm.azurewebsites.net/&lt;app-name&gt;.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "&lt;app-name&gt;.azurewebsites.net",
  "deploymentLocalGitUrl": "https://&lt;username&gt;@&lt;app-name&gt;.scm.azurewebsites.net/&lt;app-name&gt;.git",
  "enabled": true,
  &lt; JSON data removed for brevity. &gt;
}
</pre>

Sie haben eine leere Web-App mit aktivierter Git-Bereitstellung erstellt.

> [!NOTE]
> Die URL des Git-Remotespeicherorts wird in der `deploymentLocalGitUrl`-Eigenschaft im Format `https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git` angezeigt. Speichern Sie diese URL, da Sie sie später noch benötigen.
>
