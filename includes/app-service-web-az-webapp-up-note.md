---
title: include file
description: include file
services: app-service
author: msangapu
ms.service: app-service
ms.topic: include
ms.date: 02/27/2019
ms.author: msangapu
ms.custom: include file
ms.openlocfilehash: d285b48606485fbd7f53511a15be1e2a31791829
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102244671"
---
> [!NOTE]
> Der Befehl `az webapp up` bewirkt Folgendes:
>
>- Erstellen einer [Standardressourcengruppe](/cli/azure/group#az-group-create)
>
>- Erstellen eines standardmäßigen [App Service-Plans](/cli/azure/appservice/plan#az-appservice-plan-create)
>
>- [Erstellen einer App](/cli/azure/webapp#az-webapp-create) mit dem angegebenen Namen
>
>- [Bereitstellen von ZIP-Dateien](../articles/app-service/deploy-zip.md) aus dem aktuellen Arbeitsverzeichnis für die App
>