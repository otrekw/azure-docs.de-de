---
title: include file
description: include file
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 12/20/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: f3d558736751d3c50e3c007e3aebb369093ef856
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102244722"
---
Erstellen Sie in Cloud Shell mit dem Befehl [`az appservice plan create`](/cli/azure/appservice/plan#az-appservice-plan-create) einen App Service-Plan in der Ressourcengruppe.

<!-- [!INCLUDE [app-service-plan](app-service-plan-linux.md)] -->

Im folgenden Beispiel wird ein App Service-Plan mit dem Namen `myAppServicePlan` in einem Linux-Container (`--is-linux`) für den Tarif **Free** (`--sku F1`) erstellt.

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku F1 --is-linux
```

Nach Erstellung des App Service-Plans zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "linux",
  "location": "West Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  <JSON data removed for brevity.>
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
```
