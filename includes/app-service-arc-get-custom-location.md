---
title: Datei einfügen
description: Datei einfügen
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 05/12/2021
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: d4d6783645f719f21d97f18abc9bc900a368fd8f
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112361363"
---
Erhalten Sie von Ihrem Cluster-Administrator die folgenden Informationen bezüglich des benutzerdefinierten Speicherorts (siehe [Erstellen eines benutzerdefinierten Speicherorts](../articles/app-service/manage-create-arc-environment.md#create-a-custom-location)).

```azurecli-interactive
customLocationGroup="<resource-group-containing-custom-location>"
customLocationName="<name-of-custom-location>"
```

Erhalten Sie die benutzerdefinierte Standort-ID für den nächsten Schritt.

```azurecli-interactive
customLocationId=$(az customlocation show \
    --resource-group $customLocationGroup \
    --name $customLocationName \
    --query id \
    --output tsv)
```
