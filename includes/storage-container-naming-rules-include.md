---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 3e5afeffa6f47ff79e31f189abc6d5e26d8f2586
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86050363"
---
Jeder Blob im Azure-Speicher muss sich in einem Container befinden. Der Container ist Teil des Blob-Namens. Beispiel: `mycontainer` ist der Name des Containers in diesen Beispiel-Blob-URIs:

```http
https://storagesample.blob.core.windows.net/mycontainer/blob1.txt
https://storagesample.blob.core.windows.net/mycontainer/photos/myphoto.jpg
```

Ein Containername muss ein gültiger DNS-Name sein und den folgenden Benennungsregeln entsprechen:

1. Containernamen müssen mit einem Buchstaben oder einer Zahl beginnen und dürfen nur Buchstaben, Zahlen und Bindestriche (-) enthalten.
2. Jedem Bindestrich (-) muss unmittelbar ein Buchstabe oder eine Zahl vorangehen und folgen von einem Buchstaben oder einer Zahl; zudem dürfen nicht mehrere Bindestriche direkt aufeinander folgen.
3. Der Containername darf ausschließlich Kleinbuchstaben enthalten.
4. Containernamen müssen zwischen 3 und 63 Zeichen lang sein.

> [!IMPORTANT]
> Beachten Sie, dass der Name eines Containers immer aus Kleinbuchstaben bestehen muss. Wenn der Containername einen Großbuchstaben enthält oder anderweitig gegen die Benennungsregeln für Container verstößt, wird möglicherweise der Fehlercode 400 (Ungültige Anforderung) zurückgegeben. 
> 
> 

