---
title: Verwenden verwalteter Identitäten für den Zugriff auf App Configuration
titleSuffix: Azure App Configuration
description: Authentifizieren bei Azure App Configuration mit verwalteten Identitäten
author: AlexandraKemperMS
ms.author: alkemper
ms.service: azure-app-configuration
ms.custom: devx-track-csharp, fasttrack-edit
ms.topic: conceptual
ms.date: 04/08/2021
ms.openlocfilehash: ff7c2b6ced87c8254283923a9163e51f06ae6ef6
ms.sourcegitcommit: 8b7d16fefcf3d024a72119b233733cb3e962d6d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2021
ms.locfileid: "114298172"
---
# <a name="use-managed-identities-to-access-app-configuration"></a>Verwenden verwalteter Identitäten für den Zugriff auf App Configuration

Mit [verwalteten Identitäten](../active-directory/managed-identities-azure-resources/overview.md) von Azure Active Directory wird die Verwaltung von Geheimnissen für Ihre Cloudanwendung vereinfacht. Mit einer verwalteten Identität kann Ihr Code den Dienstprinzipal verwenden, der für den Azure-Dienst, auf dem er ausgeführt wird, erstellt wurde. Eine verwaltete Identität wird anstelle von separaten Anmeldeinformationen verwendet, die in Azure Key Vault oder in einer lokalen Verbindungszeichenfolge gespeichert sind.

Azure App Configuration und die zugehörigen .NET Core-, .NET Framework- und Java Spring-Clientbibliotheken verfügen über integrierte Unterstützung für die verwaltete Dienstidentität. Zwar müssen Sie die verwaltete Identität nicht verwenden, jedoch entfällt durch deren Verwendung die Notwendigkeit für ein Zugriffstoken mit Geheimnissen. Ihr Code kann nur mithilfe des Dienstendpunkts auf den App Configuration-Speicher zuzugreifen. Sie können diese URL direkt in Ihren Code einbetten, ohne Geheimnisse offenzulegen.

In diesem Artikel wird veranschaulicht, wie Sie die verwaltete Identität für den Zugriff auf App Configuration nutzen können. Dies baut auf der Web-App auf, die in den Schnellstartanleitungen vorgestellt wurde. Bevor Sie fortfahren, [erstellen Sie eine ASP.NET Core-App mit App Configuration](./quickstart-aspnet-core-app.md).

> [!IMPORTANT]
> Eine verwaltete Identität kann nicht zum Authentifizieren lokal ausgeführter Anwendungen verwendet werden. Ihre Anwendung muss in einem Azure-Dienst bereitgestellt werden, der verwaltete Identitäten unterstützt. In diesem Artikel wird Azure App Service als Beispiel verwendet. Das Konzept gilt aber ebenso für alle anderen Azure-Dienste, die verwaltete Identitäten unterstützen, z. B. [Azure Kubernetes Service](../aks/use-azure-ad-pod-identity.md), [Azure Virtual Machine](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md) und [Azure Container Instances](../container-instances/container-instances-managed-identity.md). Wenn Ihre Workload in einem dieser Dienste gehostet wird, können Sie auch die Unterstützung dieses Diensts für verwaltete Identitäten nutzen.

Für die Ausführung der Schritte dieses Tutorials können Sie einen beliebigen Code-Editor verwenden. [Visual Studio Code](https://code.visualstudio.com/) ist eine hervorragende Option, die auf Windows-, macOS- und Linux-Plattformen verfügbar ist.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Gewähren des Zugriffs auf App Configuration für eine verwaltete Identität.
> * Konfigurieren Ihrer App für die Verwendung einer verwalteten Identität bei der Verbindungsherstellung mit App Configuration.


## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* [.NET Core SDK](https://dotnet.microsoft.com/download).
* [Konfigurierte Azure Cloud Shell](../cloud-shell/quickstart.md).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="add-a-managed-identity"></a>Hinzufügen einer verwalteten Identität

Um eine verwaltete Entität im Portal einzurichten, erstellen Sie zuerst eine Anwendung und aktivieren dann das Feature.

1. Greifen Sie im [Azure-Portal](https://portal.azure.com) auf Ihre App Services-Ressource zu. Wenn Sie über keine vorhandene App Services-Ressource verfügen, mit der Sie arbeiten können, erstellen Sie eine. 

1. Scrollen Sie im linken Bereich nach unten zur Gruppe **Einstellungen**, und wählen Sie **Identität**.

1. Ändern Sie auf der Registerkarte **Systemseitig zugewiesen** den **Status** in **Ein**, und wählen Sie **Speichern** aus.

1. Antworten Sie mit **Ja**, wenn Sie gefragt werden, ob Sie die systemseitig zugewiesene verwaltete Identität aktivieren möchten.

    ![Festlegen der verwalteten Identität in App Service](./media/set-managed-identity-app-service.png)

## <a name="grant-access-to-app-configuration"></a>Gewähren des Zugriffs auf App Configuration

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Alle Ressourcen**, und wählen Sie dann den App Configuration-Speicher aus, den Sie in der Schnellstartanleitung erstellt haben.

1. Wählen Sie die Option **Zugriffssteuerung (IAM)** aus.

1. Wählen Sie auf der Registerkarte **Zugriff überprüfen** im Kartenelement **Rollenzuweisung hinzufügen** den Befehl **Hinzufügen** aus.

1. Wählen Sie unter **Rolle** die Option **App Configuration-Datenleser** aus. Wählen Sie unter **Zugriff zuweisen zu** unter **Systemseitig zugewiesene verwaltete Identität** die Option **App Service** aus.

1. Wählen Sie unter **Abonnement** Ihr Azure-Abonnement aus. Wählen Sie die App Service-Ressource für Ihre App aus.

1. Wählen Sie **Speichern** aus.

    ![Hinzufügen einer verwalteten Identität](./media/add-managed-identity.png)


## <a name="use-a-managed-identity"></a>Verwenden einer verwalteten Identität

1. Fügen Sie einen Verweis auf das Paket *Azure.Identity* hinzu.

    ```bash
    dotnet add package Azure.Identity
    ```

1. Suchen Sie den Endpunkt Ihres App Configuration-Speichers. Diese URL wird auf der Registerkarte **Zugriffsschlüssel** für den Speicher im Azure-Portal aufgelistet.

1. Öffnen Sie die Datei *appsettings.json*, und fügen Sie das folgende Skript hinzu. Ersetzen Sie *\<service_endpoint>* (einschließlich der spitzen Klammern) durch die URL für Ihren App Configuration-Speicher.

    ```json
    "AppConfig": {
        "Endpoint": "<service_endpoint>"
    }
    ```

1. Öffnen Sie *Program.cs*, und fügen Sie einen Verweis auf die Namespaces `Azure.Identity` und `Microsoft.Azure.Services.AppAuthentication` hinzu:

    ```csharp-interactive
    using Azure.Identity;
    ```

1. Wenn Sie nur auf Werte zugreifen möchten, die direkt in App Configuration gespeichert sind, aktualisieren Sie die `CreateWebHostBuilder`-Methode, indem Sie die `config.AddAzureAppConfiguration()`-Methode (im Paket `Microsoft.Azure.AppConfiguration.AspNetCore`) ersetzen.

    > [!IMPORTANT]
    > `CreateHostBuilder` ersetzt `CreateWebHostBuilder` in .NET Core 3.0.  Wählen Sie auf der Grundlage ihrer Umgebung die richtige Syntax aus.

    ### <a name="net-core-5x"></a>[.NET Core 5.x](#tab/core5x)

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
                webBuilder.ConfigureAppConfiguration((hostingContext, config) =>
                {
                    var settings = config.Build();
                    config.AddAzureAppConfiguration(options =>
                        options.Connect(new Uri(settings["AppConfig:Endpoint"]), new ManagedIdentityCredential()));
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
                        options.Connect(new Uri(settings["AppConfig:Endpoint"]), new ManagedIdentityCredential()));
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
                       options.Connect(new Uri(settings["AppConfig:Endpoint"]), new ManagedIdentityCredential()));
               })
               .UseStartup<Startup>();
    ```

    ---

    > [!NOTE]
    > Wenn Sie eine **benutzerseitig zugewiesene verwaltete Identität** verwenden möchten, achten Sie darauf, dass Sie beim Erstellen von [ManagedIdentityCredential](/dotnet/api/azure.identity.managedidentitycredential) die clientId angeben.
    >```csharp
    >config.AddAzureAppConfiguration(options =>
    >       {
    >           options.Connect(new Uri(settings["AppConfig:Endpoint"]), new ManagedIdentityCredential("<your_clientId>"))
    >        });
    >```
    >Wie in den [Häufig gestellten Fragen zu verwalteten Identitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/known-issues.md) erläutert, gibt es eine Standardmethode, um aufzulösen, welche verwaltete Identität verwendet wird. In diesem Fall erzwingt die Azure-Identitätsbibliothek, dass Sie die gewünschte Identität angeben, um in Zukunft mögliche Laufzeitprobleme zu vermeiden (z. B. wenn eine neue benutzerseitig zugewiesene verwaltete Identität hinzugefügt wird, oder wenn die systemseitig zugewiesene verwaltete Identität aktiviert ist). Daher müssen Sie die clientId auch dann angeben, wenn nur eine benutzerseitig zugewiesene verwaltete Identität definiert ist und keine systemseitig zugewiesene verwaltete Identität vorhanden ist.
 
    

## <a name="deploy-your-application"></a>Bereitstellen der Anwendung

Die Verwendung verwalteter Identitäten erfordert, dass Sie Ihre App in einem Azure-Dienst bereitstellen. Verwaltete Identitäten können nicht für die Authentifizierung lokal ausgeführter Apps verwendet werden. Um die .NET Core-App bereitzustellen, die Sie im Schnellstart [Erstellen einer ASP.NET Core-App mit App Configuration](./quickstart-aspnet-core-app.md) erstellt und zur Verwendung verwalteter Identitäten geändert haben, befolgen Sie die Anleitung unter [Veröffentlichen Ihrer Web-App](../app-service/quickstart-dotnetcore.md?pivots=development-environment-vs&tabs=netcore31#publish-your-web-app).

Neben App Service unterstützen viele andere Azure-Dienste verwaltete Identitäten. Weitere Informationen finden Sie unter [Dienste, die verwaltete Identitäten für Azure-Ressourcen unterstützen](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie eine verwaltete Azure-Identität hinzugefügt, um den Zugriff auf App Configuration zu optimieren und die Verwaltung der Anmeldeinformationen für Ihre App zu verbessern. Fahren Sie mit den Azure CLI-Beispielen fort, um mehr über die Verwendung von App Configuration zu erfahren.

> [!div class="nextstepaction"]
> [CLI-Beispiele](./cli-samples.md)