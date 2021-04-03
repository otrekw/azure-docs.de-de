---
author: baanders
description: 'Includedatei für Azure Digital Twins: Einrichten der aktuellen IoT-Erweiterung'
ms.service: digital-twins
ms.topic: include
ms.date: 7/31/2020
ms.author: baanders
ms.openlocfilehash: 984739a728f6ac5e28eeb561e0d7b6ec0485ca13
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "90606556"
---
Führen Sie zuerst diesen Befehl aus, um eine Liste mit allen bereits installierten Erweiterungen anzuzeigen.

```azurecli-interactive
az extension list
```

Die Ausgabe enthält ein Array mit allen Erweiterungen, die Sie derzeit besitzen. Suchen Sie für jeden Listeneintrag nach dem Feld `"name"`, um die Namen der Erweiterungen anzuzeigen.

Ermitteln Sie anhand der Ausgabe, welcher der folgenden Befehle für die Einrichtung der Erweiterung ausgeführt werden muss (ggf. auch mehr als ein Befehl).
* Wenn `azure-iot` in der Liste enthalten ist: Die Erweiterung ist bereits vorhanden. Führen Sie diesen Befehl aus, um sicherzustellen, dass Sie über das neueste Update verfügen und keine weiteren Updates verfügbar sind:

   ```azurecli-interactive
   az extension update --name azure-iot
   ```

* Wenn `azure-iot` **nicht** in der Liste enthalten ist: Sie müssen die Erweiterung installieren. Verwenden Sie diesen Befehl:

    ```azurecli-interactive
    az extension add --name azure-iot
    ```

* Wenn `azure-iot-cli-ext` in der Liste enthalten ist: Dies ist die Legacy-Version der Erweiterung. Da jeweils nur eine Version der Erweiterung installiert sein sollte, sollten Sie die Legacy-Erweiterung deinstallieren. Verwenden Sie diesen Befehl:

   ```azurecli-interactive
   az extension remove --name azure-cli-iot-ext
   ```