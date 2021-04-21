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
ms.openlocfilehash: c059746262cdaf2901c765d3ce3848887b2e629c
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107781120"
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
[az-acr-task-show]: /cli/azure/acr/task#az_acr_task_show
