---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/28/2020
ms.author: glenga
ms.openlocfilehash: 43da0ea4ddfc5410425465d436522523739218fe
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91408549"
---
## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Dank der Integration zwischen Visual Studio Code und [Azure Functions Core Tools](../articles/azure-functions/functions-run-local.md) können Sie dieses Projekt vor der Veröffentlichung in Azure zunächst auf Ihrem lokalen Entwicklungscomputer ausführen.

1. Drücken Sie zum Aufrufen Ihrer Funktion <kbd>F5</kbd>, um das Funktions-App-Projekt zu starten. Die Ausgabe der Core Tools wird im Bereich **Terminal** angezeigt.

1. Falls Sie Azure Functions Core Tools noch nicht installiert haben, wählen Sie bei entsprechender Aufforderung die Option **Installieren** aus. Wenn die Core Tools installiert sind, wird Ihre App im Bereich **Terminal** gestartet. Der lokal ausgeführte URL-Endpunkt Ihrer über HTTP ausgelösten Funktion wird angezeigt.

    ![VS Code-Ausgabe der lokalen Funktion](./media/functions-run-function-test-local-vs-code/functions-vscode-f5.png)

1. Navigieren Sie zur folgenden URL, während die Core Tools ausgeführt werden, um eine GET-Anforderung mit der Abfragezeichenfolge `?name=Functions` auszuführen.

    `http://localhost:7071/api/HttpExample?name=Functions`

1. Die zurückgegebene Antwort sieht in einem Browser in etwa wie folgt aus:

    ![Browser: Localhost-Beispielausgabe](./media/functions-run-function-test-local-vs-code/functions-test-local-browser.png)

1. Informationen zur Anforderung werden in Bereich **Terminal** angezeigt.

    ![Taskhoststart: VS Code-Terminalausgabe](./media/functions-run-function-test-local-vs-code/function-execution-terminal.png)

1. Drücken Sie <kbd>STRG+C</kbd>, um die Core Tools zu beenden und die Verbindung mit dem Debugger zu trennen.
