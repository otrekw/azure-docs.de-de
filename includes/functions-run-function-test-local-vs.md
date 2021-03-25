---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/06/2020
ms.author: glenga
ms.openlocfilehash: bff2f05a95faf9c475189cb5a8003cb7fd9f69be
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101701393"
---
1. Drücken Sie in Visual Studio die Taste <kbd>F5</kbd>, um Ihre Funktion auszuführen. Sie müssen unter Umständen eine Firewallausnahme aktivieren, damit die Tools HTTP-Anforderungen verarbeiten können. Autorisierungsebenen werden niemals erzwungen, wenn Sie eine Funktion lokal ausführen.

2. Kopieren Sie die URL Ihrer Funktion aus der Azure Functions-Laufzeitausgabe.

    ![Lokale Azure-Laufzeit](./media/functions-run-function-test-local-vs/functions-debug-local-vs.png)

3. Fügen Sie die URL zu der HTTP-Anforderung in die Adressleiste Ihres Browsers ein. Hängen Sie anschließend die Abfragezeichenfolge `?name=<YOUR_NAME>` an diese URL an, und führen Sie die Anforderung aus. Im folgenden Bild wird die Antwort auf die lokale GET-Anforderung im Browser angezeigt, die von der Funktion zurückgegeben wird: 

    ![localhost-Antwort der Funktion im Browser](./media/functions-run-function-test-local-vs/functions-run-browser-local-vs.png)

4. Wenn das Debuggen angehalten werden soll, drücken Sie in Visual Studio <kbd>UMSCHALTTASTE</kbd>+<kbd>F5</kbd>.
