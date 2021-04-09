---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/17/2020
ms.author: inhenkel
ms.custom: CLI
ms.openlocfilehash: aa76f7b85302651f6874747610c3355f0572a7ee
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "94884710"
---
<!-- List and set subscriptions -->

1. Rufen Sie eine Liste Ihrer Abonnements mithilfe des Befehls [az account list](/cli/azure/account#az-account-list) ab.

    ```
    az account list --output table
    ```

2. Verwenden Sie `az account set` mit der ID oder dem Namen des Abonnements, zu dem Sie wechseln möchten.

    ```
    az account set --subscription "My Demos"
    ```
