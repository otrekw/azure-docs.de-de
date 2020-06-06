---
title: 'Skriptbeispiel: Deaktivieren des vorläufigen Löschens für Dateifreigaben'
description: Hier erfahren Sie, wie das vorläufige Löschen für Dateifreigaben in einem Speicherkonto mit einem Skript deaktivieren.
ms.topic: sample
ms.date: 02/02/2020
ms.openlocfilehash: 172a7bd19ffd07ca7900a457c447362343e5f185
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84121283"
---
# <a name="disable-soft-delete-for-file-shares-in-a-storage-account"></a>Deaktivieren des vorläufigen Löschens für Dateifreigaben in einem Speicherkonto

In diesem Dokument wird das Verfahren zum Deaktivieren des vorläufigen Löschens für Dateifreigaben in einem Speicherkonto erläutert.

Folgen Sie diesen Schritten:

1. Installieren Sie ARMClient. Weitere Informationen zum Installieren von ARMClient finden Sie unter [diesem Link](https://github.com/projectkudu/ARMClient).

2. Speichern Sie die folgenden beiden Anforderungstextdateien in einem Ordner auf Ihrem Computer.

    ```json
    rqbody-enableSoftDelete.json

    {
    "properties": {
        "shareDeleteRetentionPolicy": {
        "enabled":true,
        "days": 14
        }
    },
    "cors": {
        "corsRules": []
    }
    }

    rqbody-disableSoftDelete.json

    {
    "properties": {
        "shareDeleteRetentionPolicy": {
        "enabled":false,
        "days": 0
        }
    },
    "cors": {
        "corsRules": []
    }
    }
    ```

3. Halten Sie Ihre Azure Resource Manager (ARM)-ID für Ihr Speicherkonto bereit. Beispiel: `/subscriptions/37aa2d43-d4f5-4322-bae0-6ee11c627f50/resourceGroups/afsshare/providers/Microsoft.Storage/storageAccounts/inquirytest`

4. Melden Sie sich mit Ihren Anmeldeinformationen an, indem Sie **armclient login** ausführen.

5. Rufen Sie die aktuellen Eigenschaften für vorläufiges Löschen von Dateifreigaben im Speicherkonto ab.

    Die folgende GET-Operation ruft die Eigenschaften für vorläufiges Löschen für Dateifreigaben im Konto *inquirytest* ab:

    ```cmd
    armclient get /subscriptions/37aa2d43-d4f5-4322-bae0-6ee11c627f50/resourceGroups/afsshare /providers/Microsoft.Storage/storageAccounts/inquirytest/fileServices/default?api-version=2019-04-01
    ```

    ```output
    {
    "id": "/subscriptions/37aa2d43-d4f5-4322-bae0-6ee11c627f50/resourceGroups/Bugbash/providers/Microsoft.Storage/storageAccounts/inquirytest/fileServices/de
    fault",
    "name": "default",
    "type": "Microsoft.Storage/storageAccounts/fileServices",
    "properties": {
        "cors": {
        "corsRules": []
        },
        "shareDeleteRetentionPolicy": {
        "enabled": true,
        "days": 14
        }
    }
    }
    ```

6. Deaktivieren Sie das vorläufige Löschen für Dateifreigaben in einem Speicherkonto.

    Die folgende PUT-Operation deaktiviert die Eigenschaften für vorläufiges Löschen für Dateifreigaben im Konto *inquirytest*:

    ```cmd
    armclient put /subscriptions/37aa2d43-d4f5-4322-bae0-6ee11c627f50/resourceGroups/afsshare /providers/Microsoft.Storage/storageAccounts/inquirytest/fileServices/default?api-version=2019-04-01 .\rqbody-disableSoftDelete.json
    ```

    ```Output
    {
    "id": "/subscriptions/37aa2d43-d4f5-4322-bae0-6ee11c627f50/resourceGroups/Bugbash/providers/Microsoft.Storage/storageAccounts/inquirytest/fileServices/de
    fault",
    "name": "default",
    "type": "Microsoft.Storage/storageAccounts/fileServices",
    "properties": {
        "shareDeleteRetentionPolicy": {
        "enabled": false,
        "days": 0
        }
    }
    }
    ```

7. Wenn Sie das vorläufige Löschen wieder aktivieren möchten, verwenden Sie das folgende Beispiel.

    Die folgende PUT-Operation aktiviert die Eigenschaften für vorläufiges Löschen für Dateifreigaben im Konto „inquirytest“.

    ```cmd
    armclient put /subscriptions/37aa2d43-d4f5-4322-bae0-6ee11c627f50/resourceGroups/afsshare /providers/Microsoft.Storage/storageAccounts/inquirytest/fileServices/default?api-version=2019-04-01 .\rqbody-EnableSoftDelete.json
    ```

    ```Output
    {
    "id": "/subscriptions/37aa2d43-d4f5-4322-bae0-6ee11c627f50/resourceGroups/Bugbash/providers/Microsoft.Storage/storageAccounts/inquirytest/fileServices/default",
    "name": "default",
    "type": "Microsoft.Storage/storageAccounts/fileServices",
    "properties": {
        "shareDeleteRetentionPolicy": {
        "enabled": true,
        "days": 14
        }
    }
    }
    ```
