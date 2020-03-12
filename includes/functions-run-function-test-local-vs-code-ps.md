---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/19/2020
ms.author: glenga
ms.openlocfilehash: edf5fc33ec14d41630462cca1a4ace0663473196
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/29/2020
ms.locfileid: "78191028"
---
## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Azure Functions Core Tools arbeitet mit Visual Studio Code zusammen, um das lokale Ausführen und Debuggen von Azure Functions-Projekten zu ermöglichen. Ausführliche Informationen zum Debuggen in Visual Studio Code finden Sie unter [Lokales Debuggen von PowerShell Azure Functions](../articles/azure-functions/functions-debug-powershell-local.md). 

1. Drücken Sie F5, um das Funktions-App-Projekt zu starten. Die Ausgabe der Core Tools wird im Bereich **Terminal** angezeigt.

1. Kopieren Sie im Bereich **Terminal** den URL-Endpunkt Ihrer über HTTP ausgelösten Funktion.

    ![Lokale Azure-Ausgabe](./media/functions-run-function-test-local-vs-code-ps/functions-vscode-f5.png)

1. Fügen Sie anschließend die Abfragezeichenfolge `?name=<yourname>` an diese URL an, und verwenden Sie `Invoke-RestMethod` an einer zweiten PowerShell-Eingabeaufforderung, um die Anforderung auszuführen:

    ```powershell
    PS > Invoke-RestMethod -Method Get -Uri http://localhost:7071/api/HttpTrigger?name=PowerShell
    Hello PowerShell
    ```

    Die GET-Anforderung kann auch über einen Browser über die folgende URL ausgeführt werden.

    <http://localhost:7071/api/HttpExample?name=PowerShell>

    Wenn Sie den Endpunkt „HttpTrigger“ aufrufen, ohne einen Parameter vom Typ `name` zu übergeben (entweder als Abfrageparameter oder im Text), gibt die Funktion den Fehler `BadRequest` zurück. Wenn Sie den Code in „run.ps1“ überprüfen, sehen Sie, dass dieser Fehler beabsichtigt ist.

1. Informationen zur Anforderung werden in Bereich **Terminal** angezeigt.

    ![Funktionsausführung im Bereich „Terminal“](./media/functions-run-function-test-local-vs-code-ps/function-execution-terminal.png)

1. Drücken Sie nach Abschluss des Vorgangs **STRG+C**, um Core Tools zu beenden.

Nachdem Sie sichergestellt haben, dass die Funktion auf Ihrem lokalen Computer richtig ausgeführt wird, können Sie das Projekt in Azure veröffentlichen.