---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/06/2020
ms.author: glenga
ms.openlocfilehash: b4b2409928b6a4196738c7cc6c7040e781d34686
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "80056596"
---
1. Drücken Sie in Visual Studio die Taste F5, um Ihre Funktion auszuführen. Sie müssen unter Umständen eine Firewallausnahme aktivieren, damit die Tools HTTP-Anforderungen verarbeiten können. Autorisierungsebenen werden niemals erzwungen, wenn Sie eine Funktion lokal ausführen.

2. Kopieren Sie die URL Ihrer Funktion aus der Azure Functions-Laufzeitausgabe.

    ![Lokale Azure-Laufzeit](./media/functions-run-function-test-local-vs/functions-debug-local-vs.png)

3. Fügen Sie die URL der HTTP-Anforderung in die Adresszeile des Browsers ein. Hängen Sie anschließend die Abfragezeichenfolge `?name=<YOUR_NAME>` an diese URL an, und führen Sie die Anforderung aus. In der folgenden Abbildung ist die Antwort des Browsers auf die von der Funktion zurückgegebene lokale GET-Anforderung dargestellt: 

    ![localhost-Antwort der Funktion im Browser](./media/functions-run-function-test-local-vs/functions-run-browser-local-vs.png)

4. Drücken Sie in Visual Studio UMSCHALT+F5, um das Debuggen zu beenden.