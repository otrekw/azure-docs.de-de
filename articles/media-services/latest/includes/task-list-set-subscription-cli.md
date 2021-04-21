---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/17/2020
ms.author: inhenkel
ms.custom: CLI
ms.openlocfilehash: ffd053b98a54d3e23eec62427f5c3d82df58954d
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107800300"
---
<!-- List and set subscriptions -->

1. Rufen Sie eine Liste Ihrer Abonnements mithilfe des Befehls [az account list](/cli/azure/account#az_account_list) ab.

    ```
    az account list --output table
    ```

2. Verwenden Sie `az account set` mit der ID oder dem Namen des Abonnements, zu dem Sie wechseln möchten.

    ```
    az account set --subscription "My Demos"
    ```
