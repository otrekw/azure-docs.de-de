---
title: 'Tutorial: Verwenden der dynamischen Konfiguration von Azure App Configuration in einer Azure Functions-App | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie die Konfigurationsdaten für Azure Functions-Apps dynamisch aktualisieren.
services: azure-app-configuration
documentationcenter: ''
author: zhenlan
manager: qingye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 11/17/2019
ms.author: zhenlwa
ms.custom: devx-track-csharp, azure-functions
ms.tgt_pltfrm: Azure Functions
ms.openlocfilehash: e603aa8ba85fdd214c04de515f405bcf9028791e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88207111"
---
# <a name="tutorial-use-dynamic-configuration-in-an-azure-functions-app"></a>Tutorial: Verwenden der dynamischen Konfiguration in einer Azure Functions-App

Der .NET Standard-Konfigurationsanbieter App Configuration unterstützt das dynamische Zwischenspeichern und Aktualisieren der Konfiguration durch Anwendungsaktivitäten. In diesem Tutorial wird veranschaulicht, wie Sie dynamische Konfigurationsupdates in Ihrem Code implementieren können. Es baut auf der Azure Functions-App auf, die in den Schnellstarts vorgestellt wurde. Absolvieren Sie zunächst [Erstellen einer Azure Functions-App mit Azure App Configuration](./quickstart-azure-functions-csharp.md), bevor Sie fortfahren.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten Ihrer Azure Functions-App für die Aktualisierung der Konfiguration als Reaktion auf Änderungen in einem App Configuration-Speicher
> * Einfügen der neuesten Konfiguration in Ihre Azure Functions-Aufrufe

## <a name="prerequisites"></a>Voraussetzungen

- Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
- [Visual Studio 2019](https://visualstudio.microsoft.com/vs) mit der Workload **Azure-Entwicklung**
- [Azure Functions-Tools](../azure-functions/functions-develop-vs.md#check-your-tools-version)
- Abgeschlossener Schnellstart: [Erstellen einer Azure Functions-App mit Azure App Configuration](./quickstart-azure-functions-csharp.md)

## <a name="reload-data-from-app-configuration"></a>Erneutes Laden von Daten aus App Configuration

1. Öffnen Sie *Function1.cs*. Fügen Sie zusätzlich zur `static`-Eigenschaft `Configuration` die neue `static`-Eigenschaft `ConfigurationRefresher` hinzu, um eine Singletoninstanz von `IConfigurationRefresher` beizubehalten, die später zum Signalisieren von Konfigurationsänderungen bei Funktionsaufrufen verwendet wird.

    ```csharp
    private static IConfiguration Configuration { set; get; }
    private static IConfigurationRefresher ConfigurationRefresher { set; get; }
    ```

2. Aktualisieren Sie den Konstruktor, und verwenden Sie die `ConfigureRefresh`-Methode, um die Einstellung anzugeben, die aus dem App Configuration-Speicher aktualisiert werden soll. Eine Instanz von `IConfigurationRefresher` wird mit der `GetRefresher`-Methode abgerufen. Optional können Sie auch das Zeitfenster für den Ablauf des Konfigurationscaches auf 1 Minute ändern (Standardwert: 30 Sekunden).

    ```csharp
    static Function1()
    {
        var builder = new ConfigurationBuilder();
        builder.AddAzureAppConfiguration(options =>
        {
            options.Connect(Environment.GetEnvironmentVariable("ConnectionString"))
                   .ConfigureRefresh(refreshOptions =>
                        refreshOptions.Register("TestApp:Settings:Message")
                                      .SetCacheExpiration(TimeSpan.FromSeconds(60))
            );
            ConfigurationRefresher = options.GetRefresher();
        });
        Configuration = builder.Build();
    }
    ```

3. Aktualisieren Sie die `Run`-Methode, und signalisieren Sie die Aktualisierung der Konfiguration mithilfe der `TryRefreshAsync`-Methode am Anfang des Funktionsaufrufs. Dies ist nur optional, wenn das Zeitfenster für den Cacheablauf erreicht wird. Entfernen Sie den `await`-Operator, wenn die Konfiguration ohne Blockierung aktualisiert werden soll.

    ```csharp
    public static async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)] HttpRequest req, ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");

        await ConfigurationRefresher.TryRefreshAsync(); 

        string keyName = "TestApp:Settings:Message";
        string message = Configuration[keyName];
            
        return message != null
            ? (ActionResult)new OkObjectResult(message)
            : new BadRequestObjectResult($"Please create a key-value with the key '{keyName}' in App Configuration.");
    }
    ```

## <a name="test-the-function-locally"></a>Lokales Testen der Funktion

1. Legen Sie eine Umgebungsvariable mit dem Namen **ConnectionString** fest, und geben Sie dafür den Zugriffsschlüssel für Ihren App-Konfigurationsspeicher an. Führen Sie bei Verwendung einer Windows-Eingabeaufforderung den folgenden Befehl aus, und starten Sie die Eingabeaufforderung neu, damit die Änderung wirksam wird:

    ```console
    setx ConnectionString "connection-string-of-your-app-configuration-store"
    ```

    Führen Sie bei Verwendung von Windows PowerShell den folgenden Befehl aus:

    ```powershell
    $Env:ConnectionString = "connection-string-of-your-app-configuration-store"
    ```

    Führen Sie bei Verwendung von macOS oder Linux den folgenden Befehl aus:

    ```console
    export ConnectionString='connection-string-of-your-app-configuration-store'
    ```

2. Drücken Sie F5, um Ihre Funktion zu testen. Akzeptieren Sie die entsprechende Aufforderung von Visual Studio zum Herunterladen und Installieren der **Azure Functions Core (CLI)** -Tools. Sie müssen möglicherweise auch eine Firewallausnahme aktivieren, damit die Tools HTTP-Anforderungen verarbeiten können.

3. Kopieren Sie die URL Ihrer Funktion aus der Azure Functions-Laufzeitausgabe.

    ![Schnellstart: Debuggen von Funktionen in VS](./media/quickstarts/function-visual-studio-debugging.png)

4. Fügen Sie die URL der HTTP-Anforderung in die Adresszeile des Browsers ein. In der folgenden Abbildung sehen Sie die Antwort des Browsers auf die von der Funktion zurückgegebene lokale GET-Anforderung.

    ![Schnellstart: Lokales Starten von Funktionen](./media/quickstarts/dotnet-core-function-launch-local.png)

5. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Klicken Sie auf **Alle Ressourcen**, und wählen Sie dann die Instanz des App Configuration-Speichers aus, die Sie in der Schnellstartanleitung erstellt haben.

6. Wählen Sie den **Konfigurations-Explorer** aus, und aktualisieren Sie die Werte der folgenden Schlüssel:

    | Schlüssel | Wert |
    |---|---|
    | TestApp:Settings:Message | Daten aus Azure App Configuration: Aktualisiert |

7. Aktualisieren Sie den Browser mehrmals. Wenn die zwischengespeicherte Einstellung nach einer Minute abläuft, zeigt die Seite die Antwort des Funktionsaufrufs mit aktualisiertem Wert an.

    ![Schnellstart: Lokales Aktualisieren von Funktionen](./media/quickstarts/dotnet-core-function-refresh-local.png)

Der in diesem Tutorial verwendete Beispielcode kann aus dem [GitHub-Repository für App Configuration](https://github.com/Azure/AppConfiguration/tree/master/examples/DotNetCore/AzureFunction) heruntergeladen werden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Ihre Azure Functions-App aktiviert, um Konfigurationseinstellungen dynamisch aus App Configuration zu aktualisieren. Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie eine von Azure verwaltete Identität hinzufügen, um den Zugriff auf App Configuration zu optimieren.

> [!div class="nextstepaction"]
> [Integration der verwalteten Identität](./howto-integrate-azure-managed-service-identity.md)
