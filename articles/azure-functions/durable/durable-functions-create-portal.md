---
title: Erstellen von dauerhaften Funktionen mit dem Azure-Portal
description: Erfahren Sie, wie Sie die Durable Functions-Erweiterung für Azure Functions für die Portalentwicklung installieren.
ms.topic: conceptual
ms.date: 04/10/2020
ms.reviewer: azfuncdf
ms.openlocfilehash: 7aa283480d95693e2630e24d5642fc7cb909b34b
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2020
ms.locfileid: "83848759"
---
# <a name="create-durable-functions-using-the-azure-portal"></a>Erstellen von dauerhaften Funktionen mit dem Azure-Portal

Die Erweiterung [Durable Functions ](durable-functions-overview.md) für Azure Functions finden Sie im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask). Diese Erweiterung muss in Ihrer Funktions-App installiert werden. In diesem Artikel wird gezeigt, wie Sie dieses Paket installieren, um dauerhafte Funktionen im Azure-Portal zu entwickeln.

> [!NOTE]
> 
> * Wenn Sie dauerhafte Funktionen in C# entwickeln, sollten Sie stattdessen die [Entwicklung in Visual Studio 2019](durable-functions-create-first-csharp.md) in Betracht ziehen.
> * Wenn Sie dauerhafte Funktionen in JavaScript entwickeln, erwägen Sie stattdessen eine [Entwicklung in Visual Studio Code](./quickstart-js-vscode.md).

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

Sie müssen über eine Funktions-App zum Hosten der Ausführung einer Funktion verfügen. Sie können mit einer Funktions-App Ihre Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung, Skalierung und Freigabe von Ressourcen. Sie können eine .NET- oder JavaScript-App erstellen.

[!INCLUDE [Create function app Azure portal](../../../includes/functions-create-function-app-portal.md)]

Standardmäßig verwendet die erstellte Funktions-App Version 2.x der Azure Functions-Runtime. Die Durable Functions-Erweiterung funktioniert sowohl mit der Version 1.x als auch mit der Version 2.x der Azure Functions-Runtime in C# sowie mit Version 2.x in JavaScript. Vorlagen sind jedoch nur verfügbar, wenn Version 2.x der Runtime als Zielversion verwendet wird, unabhängig von der ausgewählten Sprache.

## <a name="install-the-durable-functions-npm-package-javascript-only"></a>Installieren des npm-Pakets „durable-functions“ (nur JavaScript)

Wenn Sie Durable Functions in JavaScript erstellen, müssen Sie das [npm-Paket `durable-functions`](https://www.npmjs.com/package/durable-functions) installieren:

1. Wählen Sie auf der Seite ihrer Funktions-App im linken Bereich unter **Entwicklungstools** die Option **Erweiterte Tools** aus.

   :::image type="content" source="./media/durable-functions-create-portal/function-app-platform-features-choose-kudu.png" alt-text="„Plattformfeatures“ für Funktionen, „Kudu“ auswählen":::

2. Wählen Sie auf der Seite **Erweiterte Tools** die Option **Go** aus.

3. Wählen Sie in der Kudu-Konsole den Eintrag **Debugkonsole** und dann **CMD** aus.

   :::image type="content" source="./media/durable-functions-create-portal/kudu-choose-debug-console.png" alt-text="Kudu-Debugkonsole":::

3. Die Dateiverzeichnisstruktur Ihrer Funktions-App sollte angezeigt werden. Navigieren Sie zum Ordner `site/wwwroot`. Von dort aus können Sie eine `package.json` Datei hochladen, indem Sie sie in das Dateiverzeichnisfenster ziehen und dort ablegen. Unten finden Sie eine `package.json`-Beispieldatei:

    ```json
    {
      "dependencies": {
        "durable-functions": "^1.3.1"
      }
    }
    ```

   :::image type="content" source="./media/durable-functions-create-portal/kudu-choose-debug-console.png" alt-text="Kudu-Upload von „package.json“":::

4. Sobald Ihre `package.json` hochgeladen ist, führen Sie den `npm install`-Befehl in der Kudu-Remoteausführungskonsole aus.

   ![Ausführen von „npm install“ in Kudu](./media/durable-functions-create-portal/kudu-npm-install.png)
   
5. Schließlich (aktivieren Sie den Kompatibilitätsmodus)[https://docs.microsoft.com/en-us/azure/azure-functions/durable/quickstart-js-vscode#enable-compatibility-mode-1 ] durch Hinzufügen der App-Einstellung `FUNCTIONS_V2_COMPATIBILITY_MODE` mit dem Wert `true`.

## <a name="create-an-orchestrator-function"></a>Erstellen einer Orchestratorfunktion

1. Wählen Sie in Ihrer Funktions-App im linken Bereich **Funktionen** und dann im obersten Menü **Funktionen** aus. 

1. Geben Sie `durable` in das Suchfeld der Seite **Neue Funktion** ein, und wählen Sie dann die Vorlage **HTTP-Starter für Durable Functions** aus.

   :::image type="content" source="./media/durable-functions-create-portal/durable-functions-http-starter-template.png" alt-text="HTTP-Starter für Durable Functions auswählen":::

1. Geben Sie als Name für **Neue Funktion** `HttpStart` ein, und wählen Sie dann **Funktion erstellen** aus.

   Die erstellte Funktion wird zum Starten der Orchestrierung verwendet.

1. Erstellen Sie eine weitere Funktion in der Funktions-App, dieses Mal mit der Vorlage **Orchestrator für Durable Functions**. Geben Sie der neuen Funktion den Namen `HelloSequence`.

1. Erstellen Sie mithilfe der Vorlage `Hello`Aktivität für Durable Functions**eine dritte Funktion namens**.

## <a name="test-the-durable-function-orchestration"></a>Testen der Orchestrierung für die dauerhafte Funktion

1. Wechseln Sie zurück zur Funktion **HttpStart**, wählen Sie **Funktions-URL abrufen** und dann das Symbol **In Zwischenablage kopieren** aus, um die URL zu kopieren. Sie verwenden diese URL, um die Funktion **HelloSequence** zu starten.

1. Verwenden Sie ein HTTP-Tool wie Postman oder cURL, um eine POST-Anforderung an die kopierte URL zu senden. Das folgende Beispiel zeigt einen cURL-Befehl, der eine POST-Anforderung an die dauerhafte Funktion sendet:

    ```bash
    curl -X POST https://{your-function-app-name}.azurewebsites.net/api/orchestrators/HelloSequence
    ```

    In diesem Beispiel ist `{your-function-app-name}` die Domäne, die den Namen Ihrer Funktions-App repräsentiert. Die Antwortnachricht enthält einen Satz aus URI-Endpunkten, die Sie zum Überwachen und Verwalten der Ausführung verwenden können. Sie sieht ähnlich wie im folgenden Beispiel aus:

    ```json
    {  
       "id":"10585834a930427195479de25e0b952d",
       "statusQueryGetUri":"https://...",
       "sendEventPostUri":"https://...",
       "terminatePostUri":"https://...",
       "rewindPostUri":"https://..."
    }
    ```

1. Rufen Sie den Endpunkt-URI `statusQueryGetUri` auf, um den aktuellen Status der dauerhaften Funktion anzuzeigen. Dies kann wie im folgenden Beispiel aussehen:

    ```json
        {
            "runtimeStatus": "Running",
            "input": null,
            "output": null,
            "createdTime": "2017-12-01T05:37:33Z",
            "lastUpdatedTime": "2017-12-01T05:37:36Z"
        }
    ```

1. Setzen Sie den Aufruf des `statusQueryGetUri`-Endpunkts fort, bis sich der Status in **Completed** ändert. Daraufhin wird eine Antwort ähnlich wie im folgenden Beispiel angezeigt:

    ```json
    {
            "runtimeStatus": "Completed",
            "input": null,
            "output": [
                "Hello Tokyo!",
                "Hello Seattle!",
                "Hello London!"
            ],
            "createdTime": "2017-12-01T05:38:22Z",
            "lastUpdatedTime": "2017-12-01T05:38:28Z"
        }
    ```

Ihre erste dauerhafte Funktion steht nun in Azure zur Verwendung bereit.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zu gängigen Mustern für dauerhafte Funktionen](durable-functions-overview.md#application-patterns)
