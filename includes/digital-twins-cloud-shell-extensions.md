---
author: baanders
description: 'Includedatei für Azure Digital Twins: Einrichten der aktuellen IoT-Erweiterung'
ms.service: digital-twins
ms.topic: include
ms.date: 7/31/2020
ms.author: baanders
ms.openlocfilehash: 7e43ab6d419d6a08973864838c66aef0941d74f5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90606556"
---
Führen Sie zuerst diesen Befehl aus, um eine Liste mit allen bereits installierten Erweiterungen anzuzeigen.

```azurecli
az extension list
```

Die Ausgabe enthält ein Array mit allen Erweiterungen, die Sie derzeit besitzen. Suchen Sie für jeden Listeneintrag nach dem Feld `"name"`, um die Namen der Erweiterungen anzuzeigen.

Ermitteln Sie anhand der Ausgabe, welcher der folgenden Befehle für die Einrichtung der Erweiterung ausgeführt werden muss (ggf. auch mehr als ein Befehl).
* Wenn `azure-iot` in der Liste enthalten ist: Die Erweiterung ist bereits vorhanden. Führen Sie diesen Befehl aus, um sicherzustellen, dass Sie über das neueste Update verfügen und keine weiteren Updates verfügbar sind:

   ```azurecli
   az extension update --name azure-iot
   ```

* Wenn `azure-iot` **nicht** in der Liste enthalten ist: Sie müssen die Erweiterung installieren. Verwenden Sie diesen Befehl:

    ```azurecli
    az extension add --name azure-iot
    ```

* Wenn `azure-iot-cli-ext` in der Liste enthalten ist: Dies ist die Legacy-Version der Erweiterung. Da jeweils nur eine Version der Erweiterung installiert sein sollte, sollten Sie die Legacy-Erweiterung deinstallieren. Verwenden Sie diesen Befehl:

   ```azurecli
   az extension remove --name azure-cli-iot-ext
   ```