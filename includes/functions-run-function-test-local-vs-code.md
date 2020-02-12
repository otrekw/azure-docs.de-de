---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 01/12/2020
ms.author: glenga
ms.openlocfilehash: f4af3c202d4f00c4ac3041921175c92226f0db7c
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2020
ms.locfileid: "76964104"
---
## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Dank der Integration zwischen Visual Studio Code und [Azure Functions Core Tools](../articles/azure-functions/functions-run-local.md) können Sie dieses Projekt vor der Veröffentlichung in Azure zunächst auf Ihrem lokalen Entwicklungscomputer ausführen.

1. Drücken Sie zum Aufrufen Ihrer Funktion F5, um das Funktions-App-Projekt zu starten. Die Ausgabe der Core Tools wird im Bereich **Terminal** angezeigt.

1. Falls Sie Azure Functions Core Tools noch nicht installiert haben, wählen Sie bei entsprechender Aufforderung die Option **Installieren** aus. Wenn die Core Tools installiert sind, wird Ihre App im Bereich **Terminal** gestartet. Der lokal ausgeführte URL-Endpunkt Ihrer über HTTP ausgelösten Funktion wird angezeigt. 

    ![Lokale Azure-Ausgabe](./media/functions-run-function-test-local-vs-code/functions-vscode-f5.png)

1. Navigieren Sie zur folgenden URL, während die Core Tools ausgeführt werden, um eine GET-Anforderung mit der Abfragezeichenfolge `?name=Functions` auszuführen.

    <http://localhost:7071/api/HttpExample?name=Functions>

1. Die zurückgegebene Antwort sieht in einem Browser in etwa wie folgt aus:

    ![localhost-Antwort der Funktion im Browser](./media/functions-run-function-test-local-vs-code/functions-test-local-browser.png)

1. Informationen zur Anforderung werden in Bereich **Terminal** angezeigt.

    ![Funktionsausführung im Bereich „Terminal“](./media/functions-run-function-test-local-vs-code/function-execution-terminal.png)

1. Drücken Sie STRG+C, um die Core Tools zu beenden und die Verbindung mit dem Debugger zu trennen.
