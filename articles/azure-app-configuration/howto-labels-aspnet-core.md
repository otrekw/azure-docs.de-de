---
title: Verwenden von Bezeichnungen zum Bereitstellen von Konfigurationswerten pro Umgebung.
titleSuffix: Azure App Configuration
description: In diesem Artikel wird beschrieben, wie Sie Bezeichnungen verwenden, um App-Konfigurationswerte für die Umgebung abzurufen, in der die App derzeit ausgeführt wird.
ms.service: azure-app-configuration
author: AlexandraKemperMS
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.date: 3/12/2020
ms.author: alkemper
ms.openlocfilehash: e6d9aadff5fba66aef260c674f5a01904b289da2
ms.sourcegitcommit: b11257b15f7f16ed01b9a78c471debb81c30f20c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2021
ms.locfileid: "111592356"
---
# <a name="use-labels-to-provide-per-environment-configuration-values"></a>Verwenden von Bezeichnungen zum Bereitstellen von Konfigurationswerten pro Umgebung.

Viele Anwendungen müssen verschiedene Konfigurationen für unterschiedliche Umgebungen verwenden. Angenommen, eine Anwendung verfügt über einen Konfigurationswert, der die Verbindungszeichenfolge definiert, die für ihre Back-End-Datenbank verwendet werden soll. Die Entwickler der Anwendung verwenden eine andere Datenbank als die, die in der Produktion verwendet wird. Die von der Anwendung verwendete Datenbankverbindungszeichenfolge muss sich ändern, wenn die Anwendung von der Entwicklung zur Produktion übergeht.

In Azure App Configuration können Sie *Bezeichnungen* verwenden, um unterschiedliche Werte für denselben Schlüssel zu definieren. Beispielsweise können Sie einen einzelnen Schlüssel mit unterschiedlichen Werten für Entwicklung und Produktion definieren. Sie können angeben, welche Bezeichnung beim Herstellen einer Verbindung mit App Configuration geladen werden soll.

Um diese Funktionalität zu veranschaulichen, ändern wir die in [Schnellstart: Erstellen einer ASP.NET Core-App mit Azure App Configuration](./quickstart-aspnet-core-app.md) erstellte Web-App so, dass verschiedene Konfigurationseinstellungen für Entwicklung und Produktion verwendet werden. Arbeiten Sie zuerst diesen Schnellstart durch, bevor Sie fortfahren.

## <a name="specify-a-label-when-adding-a-configuration-value"></a>Geben Sie beim Hinzufügen eines Konfigurationswerts eine Bezeichnung an.

Wechseln Sie im Azure-Portal in den **Konfigurations-Explorer**, und suchen Sie den Schlüssel *TestApp:Settings:FontColor*, den Sie in dem Schnellstart erstellt haben. Wählen Sie sein Kontextmenü aus, und wählen Sie dann **Wert hinzufügen** aus.

> [!div class="mx-imgBorder"]
> ![Menüelement „Wert hinzufügen“](media/labels-add-value.png)

Geben Sie im Bildschirm **Wert hinzufügen** den **Wert** **Rot** und die **Bezeichnung** **Entwicklung** ein. Lassen Sie **Inhaltstyp** leer. Wählen Sie **Übernehmen**.

## <a name="load-configuration-values-with-a-specified-label"></a>Laden von Konfigurationswerten mit einer angegebenen Bezeichnung

Standardmäßig lädt Azure App Configuration nur Konfigurationswerte ohne Bezeichnung. Wenn Sie Bezeichnungen für Ihre Konfigurationswerte definiert haben, sollten Sie die Bezeichnung angeben, die beim Herstellen einer Verbindung mit App Configuration verwendet werden soll.

Im vorherigen Abschnitt haben Sie einen anderen Konfigurationswert für die Entwicklungsumgebung erstellt. Verwenden Sie die Variable `HostingEnvironment.EnvironmentName`, um dynamisch zu ermitteln, in welcher Umgebung die App aktuell ausgeführt wird. Weitere Informationen finden Sie unter [Verwenden von mehreren Umgebungen in ASP.NET Core](/aspnet/core/fundamentals/environments).

Fügen Sie einen Verweis auf den Namespace [Microsoft.Extensions.Configuration.AzureAppConfiguration](/dotnet/api/microsoft.extensions.configuration.azureappconfiguration) hinzu, um auf die Klassen [KeyFilter](/dotnet/api/microsoft.extensions.configuration.azureappconfiguration.keyfilter) und [LabelFilter](/dotnet/api/microsoft.extensions.configuration.azureappconfiguration.labelfilter) zuzugreifen.

```csharp
using Microsoft.Extensions.Configuration.AzureAppConfiguration;
``` 

Laden Sie Konfigurationswerte mit der Bezeichnung, die der aktuellen Umgebung entspricht, indem Sie den Umgebungsnamen an die `Select`-Methode übergeben:

### <a name="net-core-5x"></a>[.NET Core 5.x](#tab/core5x)

```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        webBuilder.ConfigureAppConfiguration((hostingContext, config) =>
        {
            var settings = config.Build();
            config.AddAzureAppConfiguration(options =>
                options
                    .Connect(settings.GetConnectionString("AppConfig"))
                    // Load configuration values with no label
                    .Select(KeyFilter.Any, LabelFilter.Null)
                    // Override with any configuration values specific to current hosting env
                    .Select(KeyFilter.Any, hostingContext.HostingEnvironment.EnvironmentName)
            );
        })
        .UseStartup<Startup>());
```

### <a name="net-core-3x"></a>[.NET Core 3.x](#tab/core3x)

```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        webBuilder.ConfigureAppConfiguration((hostingContext, config) =>
        {
            var settings = config.Build();
            config.AddAzureAppConfiguration(options =>
                options
                    .Connect(settings.GetConnectionString("AppConfig"))
                    // Load configuration values with no label
                    .Select(KeyFilter.Any, LabelFilter.Null)
                    // Override with any configuration values specific to current hosting env
                    .Select(KeyFilter.Any, hostingContext.HostingEnvironment.EnvironmentName)
            );
        })
        .UseStartup<Startup>());
```

### <a name="net-core-2x"></a>[.NET Core 2.x](#tab/core2x)

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            var settings = config.Build();
            config.AddAzureAppConfiguration(options =>
                options
                    .Connect(settings.GetConnectionString("AppConfig"))
                    // Load configuration values with no label
                    .Select(KeyFilter.Any, LabelFilter.Null)
                    // Override with any configuration values specific to current hosting env
                    .Select(KeyFilter.Any, hostingContext.HostingEnvironment.EnvironmentName)
            );
        })
        .UseStartup<Startup>();
```
---


> [!IMPORTANT]
> Im vorangehenden Codeausschnitt wird das Geheimnisverwaltertool verwendet, um Verbindungszeichenfolge für App Configuration zu laden. Informationen zum Speichern der Verbindungszeichenfolge mithilfe des Geheimnisverwalters finden Sie unter [Schnellstart für Azure App Configuration mit ASP.NET Core](quickstart-aspnet-core-app.md).

Die `Select`-Methode wird zweimal aufgerufen. Beim ersten Mal werden Konfigurationswerte ohne Bezeichnung geladen. Danach werden Konfigurationswerte mit der Bezeichnung geladen, die der aktuellen Umgebung entspricht. Diese umgebungsspezifischen Werte setzen alle entsprechenden Werte ohne Bezeichnung außer Kraft. Sie müssen nicht für jeden Schlüssel umgebungsspezifische Werte definieren. Wenn ein Schlüssel keinen Wert mit einer Bezeichnung besitzt, die der aktuellen Umgebung entspricht, wird der Wert ohne Bezeichnung verwendet.

## <a name="test-in-different-environments"></a>Testen in unterschiedlichen Umgebungen

Öffnen Sie die Datei `launchSettings.json` im Verzeichnis `Properties`. Suchen Sie den Eintrag `config` unter `profiles`. Legen Sie im Abschnitt `environmentVariables` den die Variable `ASPNETCORE_ENVIRONMENT` auf `Production` fest.

Erstellen Sie Ihre Anwendung mit den neuen Werten, und führen Sie sie aus.

```dotnetcli
dotnet build
dotnet run
```

Wechseln Sie in einem Webbrowser zu `http://localhost:5000`. Sie werden feststellen, dass die Schriftfarbe Schwarz ist.

![Webanwendung, die mit Produktionskonfiguration ausgeführt wird](media/labels-website-prod.png)

Aktualisieren Sie `launchSettings.json`, um die Variable `ASPNETCORE_ENVIRONMENT` auf `Development` festzulegen. Führen Sie `dotnet run` erneut aus. 

Sie werden feststellen, dass die Schriftfarbe Rot ist. Dies liegt daran, dass die Anwendung jetzt den Wert von `TestApp:Settings:FontColor` verwendet, die die Bezeichnung `Development` verwendet. Alle anderen Konfigurationswerte bleiben identisch mit ihren Produktionswerten.

![Webanwendung, die mit Entwicklungskonfiguration ausgeführt wird](media/labels-website-dev.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konfiguration in ASP.NET Core](/aspnet/core/fundamentals/configuration/)
