---
title: include file
description: include file
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 07/06/2020
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 1b7c8487eb42204f2741679c9ef6eb2717c272cd
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86057352"
---
In der Befehlsausgabe zeigt der Abschnitt `identity` an, dass eine Identität vom Typ `SystemAssigned` in der Aufgabe festgelegt ist. Die `principalId` ist die Prinzipal-ID der Aufgabenidentität:

```console
[...]
  "identity": {
    "principalId": "xxxxxxxx-2703-42f9-97d0-xxxxxxxxxxxx",
    "tenantId": "xxxxxxxx-86f1-41af-91ab-xxxxxxxxxxxx",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
  },
  "location": "eastus",
[...]
``` 
Verwenden Sie den Befehl [az acr task show][az-acr-task-show], um die PrinzipalId in einer Variablen zu speichern und in späteren Befehlen zu verwenden. Ersetzen Sie im folgenden Befehl den Namen Ihres Tasks und Ihrer Registrierung:

```azurecli
principalID=$(az acr task show \
  --name <task_name> --registry <registry_name> \
  --query identity.principalId --output tsv)
```

<!-- LINKS - Internal -->
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show
