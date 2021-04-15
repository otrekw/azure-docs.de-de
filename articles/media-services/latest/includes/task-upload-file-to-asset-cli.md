---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/17/2020
ms.author: inhenkel
ms.custom: CLI
ms.openlocfilehash: affd55d65ed8454ebfcdf14f697849badf8e5d3c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "88657082"
---
<!-- ### Upload files with the CLI -->

Mit dem folgenden Befehl wird eine einzelne Datei hochgeladen:  

Ändern Sie die folgenden Werte:

1. Ändern Sie `/path/to/file` in den lokalen Pfad der Datei.  
1. Ändern Sie `MyContainer` mit der Ressourcen-ID (nicht mit dem Namen) in eine zuvor erstellte Ressource.
1. Ändern Sie `MyBlob` in den Namen, den Sie für die hochgeladene Datei verwenden möchten.
1. Ändern Sie `MyStorageAccountName` in den Namen des von Ihnen verwendeten Speicherkontos.
1. Ändern Sie `MyStorageAccountKey` in den Zugriffsschlüssel für Ihr Speicherkonto.

    ```azurecli
    az storage blob upload -f /path/to/file -c MyContainer -n MyBlob --acount-name MyStorageAccountName --account-key MyStorageAccountKey
    ```
