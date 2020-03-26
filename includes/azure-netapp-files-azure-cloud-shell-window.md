---
title: include file
description: include file
services: azure-netapp-files
author: b-juche
ms.service: azure-netapp-files
ms.topic: include
ms.date: 09/10/2019
ms.author: b-juche
ms.custom: include file
ms.openlocfilehash: 3a63fd96b09910b0cd7ee8c3ab9947b034173c8f
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "71836288"
---
1. Geben Sie das Abonnement an, das für Azure NetApp Files in die Whitelist aufgenommen wurde:
    
    ```azurecli-interactive
    az account set --subscription <subscriptionId>
    ```

1. Registrieren des Azure-Ressourcenanbieters: 
    
    ```azurecli-interactive
    az provider register --namespace Microsoft.NetApp --wait  
    ```