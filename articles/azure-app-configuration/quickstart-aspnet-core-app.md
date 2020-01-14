---
title: Schnellstart für Azure App Configuration mit ASP.NET Core | Microsoft-Dokumentation
description: Schnellstartanleitung für die Verwendung von Azure App Configuration mit ASP.NET Core-Apps.
services: azure-app-configuration
author: jpconnock
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.date: 01/04/2020
ms.author: jeconnoc
ms.openlocfilehash: f625135f036ec8fc816bc3c3eb6c76c635c51fe9
ms.sourcegitcommit: 2f8ff235b1456ccfd527e07d55149e0c0f0647cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/07/2020
ms.locfileid: "75690216"
---
# <a name="quickstart-create-an-aspnet-core-app-with-azure-app-configuration"></a>Schnellstart: Erstellen einer ASP.NET Core-App mit Azure App Configuration

In dieser Schnellstartanleitung verwenden Sie Azure App Configuration, um die Speicherung und Verwaltung von Anwendungseinstellungen für eine ASP.NET Core-Anwendung zu zentralisieren. Mit ASP.NET Core wird ein Konfigurationsobjekt, das auf nur einem Schlüssel-Wert-Paar basiert, mit Einstellungen einer oder mehrerer Datenquellen erstellt, die von einer Anwendung angegeben werden. Diese Datenquellen werden als *Konfigurationsanbieter* bezeichnet. Da der .NET Core-Client von App Configuration als Konfigurationsanbieter implementiert wird, wird der Dienst wie jede andere Datenquelle angezeigt.

## <a name="prerequisites"></a>Voraussetzungen

- Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
- [.NET Core SDK](https://dotnet.microsoft.com/download)

>[!TIP]
> Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Befehlszeilenanweisungen in diesem Artikel ausführen können.  Für sie sind allgemeine Azure-Tools einschließlich des .NET Core SDK vorinstalliert. Wenn Sie bei Ihrem Azure-Abonnement angemeldet sind, starten Sie [Azure Cloud Shell](https://shell.azure.com) über shell.azure.com.  Weitere Informationen zu Azure Cloud Shell finden Sie in der [Dokumentation](../cloud-shell/overview.md).

## <a name="create-an-app-configuration-store"></a>Erstellen eines App Configuration-Speichers

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Wählen Sie **Konfigurations-Explorer** > **Erstellen** aus, um die folgenden Schlüssel-Wert-Paare hinzuzufügen:

    | Key | value |
    |---|---|
    | TestApp:Settings:BackgroundColor | White |
    | TestApp:Settings:FontSize | 24 |
    | TestApp:Settings:FontColor | Schwarz |
    | TestApp:Settings:Message | Daten aus Azure App Configuration |

    Lassen Sie **Bezeichnung** und **Inhaltstyp** vorerst leer.

## <a name="create-an-aspnet-core-web-app"></a>Erstellen einer ASP.NET Core-Web-App

Verwenden Sie die [.NET Core-Befehlszeilenschnittstelle (CLI)](https://docs.microsoft.com/dotnet/core/tools/), um ein neues Projekt vom Typ „ASP.NET Core MVC-Web-App“ zu erstellen. Die [Azure Cloud Shell](https://shell.azure.com) stellt Ihnen diese Tools zur Verfügung.  Sie sind auch auf der Windows-, macOS- und Linux-Plattform verfügbar.

1. Erstellen Sie einen neuen Ordner für Ihr Projekt. Für diese Schnellstartanleitung verwenden wir den Namen *TestAppConfig*.

1. Führen Sie im neuen Ordner den folgenden Befehl aus, um ein neues Projekt vom Typ „ASP.NET Core MVC-Web-App“ zu erstellen:

```dotnetcli
dotnet new mvc --no-https
```

## <a name="add-secret-manager"></a>Hinzufügen des Geheimnis-Managers

Fügen Sie der *CSPROJ*-Datei ein `UserSecretsId`-Element hinzu, um des Geheimnis-Manager zu verwenden.

Öffnen Sie die Datei *.csproj*. Fügen Sie ein `UserSecretsId`-Element hinzu, wie hier gezeigt. Sie können dieselbe GUID verwenden, oder Sie können diesen Wert durch ihre eigenen Werte ersetzen. Speichern Sie die Datei .

> [!IMPORTANT]
> `CreateHostBuilder` ersetzt `CreateWebHostBuilder` in .NET Core 3.0.  Wählen Sie auf der Grundlage ihrer Umgebung die richtige Syntax aus.

#### <a name="net-core-2xtabcore2x"></a>[.NET Core 2.x](#tab/core2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

    <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
    </PropertyGroup>

    <ItemGroup>
        <PackageReference Include="Microsoft.AspNetCore.App" />
        <PackageReference Include="Microsoft.AspNetCore.Razor.Design" Version="2.1.2" PrivateAssets="All" />
    </ItemGroup>

</Project>
```

#### <a name="net-core-3xtabcore3x"></a>[.NET Core 3.x](#tab/core3x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
    
    <PropertyGroup>
        <TargetFramework>netcoreapp3.1</TargetFramework>
        <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
    </PropertyGroup>

</Project>
```
---

Im Geheimnis-Manager-Tool werden sensible Daten für die Entwicklungsarbeit außerhalb Ihrer Projektstruktur gespeichert. Mit diesem Ansatz können Sie verhindern, dass App-Geheimnisse versehentlich im Quellcode angegeben werden. Weitere Informationen zum Geheimnis-Manager finden Sie unter [Sichere Speicherung von App-Geheimnissen in der Entwicklung in ASP.net Core](https://docs.microsoft.com/aspnet/core/security/app-secrets).

## <a name="connect-to-an-app-configuration-store"></a>Herstellen einer Verbindung mit einem App Configuration-Speicher

1. Fügen Sie einen Verweis auf das NuGet-Paket `Microsoft.Azure.AppConfiguration.AspNetCore` hinzu, indem Sie den folgenden Befehl ausführen:

    ```dotnetcli
    dotnet add package Microsoft.Azure.AppConfiguration.AspNetCore --version 3.0.0-preview-010560002-1165
    ```
1. Führen Sie den folgenden Befehl aus, um Pakete für Ihr Projekt wiederherzustellen:

    ```dotnetcli
    dotnet restore
    ```
1. Fügen Sie dem Geheimnis-Manager ein Geheimnis mit dem Namen *ConnectionStrings:AppConfig* hinzu.

    Dieses Geheimnis enthält die Verbindungszeichenfolge für den Zugriff auf Ihren App Configuration-Speicher. Ersetzen Sie den Wert im folgenden Befehl durch die Verbindungszeichenfolge für Ihren App Configuration-Speicher.

    Dieser Befehl muss in dem Verzeichnis ausgeführt werden, in dem die *.csproj*-Datei enthalten ist.

    ```dotnetcli
    dotnet user-secrets set ConnectionStrings:AppConfig <your_connection_string>
    ```

    > [!IMPORTANT]
    > Einige Shells kürzen die Verbindungszeichenfolge, sofern Sie nicht in Anführungszeichen eingeschlossen ist. Stellen Sie sicher, dass die Ausgabe des `dotnet user-secrets`-Befehls die gesamte Verbindungszeichenfolge anzeigt. Wenn dies nicht der Fall ist, führen Sie den Befehl erneut aus und schließen die Verbindungszeichenfolge in Anführungszeichen ein.

    Der Geheimnis-Manager wird nur verwendet, um die Web-App lokal zu testen. Nach dem Bereitstellen der App (etwa in [Azure App Service](https://azure.microsoft.com/services/app-service/web)) verwenden Sie die Anwendungseinstellung **Verbindungszeichenfolgen** in App Service statt des Geheimnis-Managers, um die Verbindungszeichenfolge zu speichern.

    Greifen Sie mithilfe der Konfigurations-API auf dieses Geheimnis zu. Ein Doppelpunkt (:) kann im Konfigurationsnamen mit der Konfigurations-API auf allen unterstützten Plattformen verwendet werden. Siehe [Konfiguration nach Umgebung](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/index?tabs=basicconfiguration&view=aspnetcore-2.0).

1. Öffnen Sie *Program.cs*, und fügen Sie einen Verweis auf den App Configuration-Anbieter für .NET Core hinzu.

    ```csharp
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

1. Aktualisieren Sie die `CreateWebHostBuilder`-Methode für die Verwendung von App Configuration, indem Sie die Methode `config.AddAzureAppConfiguration()` aufrufen.
    
    > [!IMPORTANT]
    > `CreateHostBuilder` ersetzt `CreateWebHostBuilder` in .NET Core 3.0.  Wählen Sie auf der Grundlage ihrer Umgebung die richtige Syntax aus.

    #### <a name="net-core-2xtabcore2x"></a>[.NET Core 2.x](#tab/core2x)

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(settings["ConnectionStrings:AppConfig"]);
            })
            .UseStartup<Startup>();
    ```

    #### <a name="net-core-3xtabcore3x"></a>[.NET Core 3.x](#tab/core3x)
    
    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        webBuilder.ConfigureAppConfiguration((hostingContext, config) =>
        {
            var settings = config.Build();
            config.AddAzureAppConfiguration(settings["ConnectionStrings:AppConfig"]);
        })
        .UseStartup<Startup>());
    ```
    ---

1. Navigieren Sie zu *<app root>/Views/Home*, und öffnen Sie die Datei *Index.cshtml*. Ersetzen Sie ihren Inhalt durch den folgenden Code:

    ```HTML
    @using Microsoft.Extensions.Configuration
    @inject IConfiguration Configuration

    <style>
        body {
            background-color: @Configuration["TestApp:Settings:BackgroundColor"]
        }
        h1 {
            color: @Configuration["TestApp:Settings:FontColor"];
            font-size: @Configuration["TestApp:Settings:FontSize"];
        }
    </style>

    <h1>@Configuration["TestApp:Settings:Message"]</h1>
    ```

1. Navigieren Sie zu *<app root>/Views/Shared*, und öffnen Sie die Datei *_Layout.cshtml*. Ersetzen Sie ihren Inhalt durch den folgenden Code:

    ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>@ViewData["Title"] - hello_world</title>

        <link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
        <link rel="stylesheet" href="~/css/site.css" />
    </head>
    <body>
        <div class="container body-content">
            @RenderBody()
        </div>

        <script src="~/lib/jquery/dist/jquery.js"></script>
        <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
        <script src="~/js/site.js" asp-append-version="true"></script>

        @RenderSection("Scripts", required: false)
    </body>
    </html>
    ```

## <a name="build-and-run-the-app-locally"></a>Lokales Erstellen und Ausführen der App

1. Um die App mit der .NET Core-CLI zu erstellen, navigieren Sie zum Stammverzeichnis Ihrer Anwendung, und führen Sie den folgenden Befehl in der Befehlsshell aus:

    ```dotnetcli
    dotnet build
    ```

1. Führen Sie nach erfolgreicher Erstellung den folgenden Befehl aus, um die Web-App lokal auszuführen:

    ```dotnetcli
    dotnet run
    ```

1. Wenn Sie auf dem lokalen Computer arbeiten, verwenden Sie einen Browser, um zu `http://localhost:5000` zu navigieren. Dies ist die Standard-URL für die lokal gehostete Web-App.  

Wenn Sie in der Azure Cloud Shell arbeiten, wählen Sie die Schaltfläche *Webvorschau* und dann *Konfigurieren* aus.  

![Position der Schaltfläche „Webvorschau“](./media/quickstarts/cloud-shell-web-preview.png)

Wenn Sie zum Konfigurieren des Ports für die Vorschau aufgefordert werden, geben Sie „5000“ ein, und wählen Sie *Öffnen und Durchsuchen* aus.  Auf der Webseite ist „Daten aus Azure App Configuration“ zu lesen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie über den [Anbieter App Configuration](https://go.microsoft.com/fwlink/?linkid=2074664) einen neuen App Configuration-Speicher erstellt und mit einer ASP.NET Core-Web-App verwendet. Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihre ASP.NET Core-App für das dynamische Aktualisieren der Konfigurationseinstellungen konfigurieren.

> [!div class="nextstepaction"]
> [Tutorial: Verwenden der dynamischen Konfiguration in einer .NET Framework-App](./enable-dynamic-configuration-aspnet-core.md)
