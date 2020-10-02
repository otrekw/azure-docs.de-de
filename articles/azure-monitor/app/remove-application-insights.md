---
title: Entfernen von Application Insights in Visual Studio – Azure Monitor
description: Hier erfahren Sie, wie Sie das Application Insights SDK für ASP.NET und ASP.NET Core in Visual Studio entfernen.
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: 5bfa6ee21cc1a55f653c0e79807a14ac34082e73
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90981467"
---
# <a name="how-to-remove-application-insights-in-visual-studio"></a>Entfernen von Application Insights in Visual Studio

In diesem Artikel erfahren Sie, wie Sie das Application Insights SDK für ASP.NET und ASP.NET Core in Visual Studio entfernen.

Zum Entfernen von Application Insights müssen Sie die NuGet-Pakete und -Verweise aus der API in Ihrer Anwendung entfernen. Sie können NuGet-Pakete mithilfe der Paket-Manager-Konsole oder der Verwaltung von NuGet-Paketen für Projektmappen in Visual Studio deinstallieren. In den folgenden Abschnitten werden zwei Möglichkeiten gezeigt, um NuGet-Pakete und automatisch hinzugefügte Elemente in Ihrem Projekt zu entfernen. Vergewissern Sie sich, dass die hinzugefügten Dateien und Bereiche im eigenen Code, in denen Aufrufe der API erfolgen, entfernt werden.

## <a name="uninstall-using-the-package-management-console"></a>Deinstallieren mithilfe der Paket-Manager-Konsole

# <a name="net"></a>[.NET](#tab/net)

1. Wählen Sie zum Öffnen der Paket-Manager-Konsole in der oberen Menüleiste „Extras“ > „NuGet-Paket-Manager“ > „Paket-Manager-Konsole“ aus.
     
    ![In der oberen Menüleiste auf „Extras“ > „NuGet-Paket-Manager“ > „Paket-Manager-Konsole“ klicken](./media/remove-application-insights/package-manager.png)

    > [!NOTE]
    > Wenn die Überwachungssammlung aktiviert ist, müssen Sie zuerst „Microsoft.ApplicationInsights.TraceListener“ deinstallieren. Geben Sie `Uninstall-package Microsoft.ApplicationInsights.TraceListener` ein, und führen Sie dann den folgenden Schritt aus, um „Microsoft.ApplicationInsights.Web“ zu entfernen.

1. Geben Sie den folgenden Befehl ein:  `Uninstall-Package Microsoft.ApplicationInsights.Web -RemoveDependencies`

    Nachdem Sie den Befehl eingegeben haben, werden das Application Insights-Paket und alle zugehörigen Abhängigkeiten aus dem Projekt deinstalliert.
    
    ![Befehl in der Konsole eingeben](./media/remove-application-insights/package-management-console.png)

# <a name="net-core"></a>[.NET Core](#tab/netcore)

1. Wählen Sie zum Öffnen der Paket-Manager-Konsole in der oberen Menüleiste „Extras“ > „NuGet-Paket-Manager“ > „Paket-Manager-Konsole“ aus.

    ![In der oberen Menüleiste auf „Extras“ > „NuGet-Paket-Manager“ > „Paket-Manager-Konsole“ klicken](./media/remove-application-insights/package-manager.png)

1. Geben Sie den folgenden Befehl ein:  ` Uninstall-Package Microsoft.ApplicationInsights.AspNetCore -RemoveDependencies`

    Nachdem Sie den Befehl eingegeben haben, werden das Application Insights-Paket und alle zugehörigen Abhängigkeiten aus dem Projekt deinstalliert.

---

## <a name="uninstall-using-the-visual-studio-nugetui"></a>Deinstallieren mithilfe der NuGet-Benutzeroberfläche von Visual Studio

# <a name="net"></a>[.NET](#tab/net)

1. Klicken Sie im  *Projektmappen-Explorer* auf der rechten Seite mit der rechten Maustaste auf  **Projektmappe** , und wählen Sie  **NuGet-Pakete für Projektmappe verwalten** aus.

    Daraufhin wird ein Bildschirm angezeigt, auf dem Sie alle NuGet-Pakete bearbeiten können, die Teil des Projekts sind.
    
     ![Im Projektmappen-Explorer mit der rechten Maustaste auf „Projektmappe“ klicken und dann „NuGet-Pakete für Projektmappe verwalten“ auswählen](./media/remove-application-insights/manage-nuget-framework.png)

    > [!NOTE]
    > Wenn die Überwachungssammlung aktiviert ist, müssen Sie zuerst „Microsoft.ApplicationInsights.TraceListener“ deinstallieren, ohne die ausgewählten Abhängigkeiten zu entfernen. Dann führen Sie die folgenden Schritte aus, um „Microsoft.ApplicationInsights.Web“ mit Entfernung der ausgewählten Abhängigkeiten zu deinstallieren.
    
1. Klicken Sie auf das Paket „Microsoft.ApplicationInsights.Web“. Aktivieren Sie auf der rechten Seite das Kontrollkästchen neben  *Projekt* , um alle Projekte auszuwählen.
    
1. Um beim Deinstallieren alle Abhängigkeiten zu entfernen, wählen Sie die Dropdownschaltfläche  **Optionen**  unterhalb des Abschnitts aus, in dem Sie das Projekt ausgewählt haben.

    Aktivieren Sie unter  *Deinstallationsoptionen* das Kontrollkästchen neben  *Abhängigkeiten entfernen*.

1. Wählen Sie **Deinstallieren** aus.
    
    ![Screenshot des Fensters „Microsoft.ApplicationInsights.Web“ mit aktivierten Optionen „Abhängigkeiten entfernen“ und „Deinstallieren“](./media/remove-application-insights/uninstall-framework.png)

    Es wird ein Dialogfeld angezeigt, in dem alle Abhängigkeiten aufgelistet sind, die aus der Anwendung entfernt werden. Wählen Sie zum Deinstallieren  **OK**  aus.
    
    ![Screenshot eines Dialogfelds mit zu entfernenden Abhängigkeiten](./media/remove-application-insights/preview-uninstall-framework.png)
    
1.  Nachdem alles deinstalliert wurde, werden im  *Projektmappen-Explorer* möglicherweise weiterhin „ApplicationInsights.config“ und „AiHandleErrorAttribute.cs“ angezeigt. Sie können die beiden Dateien manuell löschen.

# <a name="net-core"></a>[.NET Core](#tab/netcore)

1. Klicken Sie im  *Projektmappen-Explorer* auf der rechten Seite mit der rechten Maustaste auf  **Projektmappe** , und wählen Sie  **NuGet-Pakete für Projektmappe verwalten** aus.

    Daraufhin wird ein Bildschirm angezeigt, auf dem Sie alle NuGet-Pakete bearbeiten können, die Teil des Projekts sind.

    ![Im Projektmappen-Explorer mit der rechten Maustaste auf „Projektmappe“ klicken und dann „NuGet-Pakete für Projektmappe verwalten“ auswählen](./media/remove-application-insights/manage-nuget-core.png)

1. Klicken Sie auf das Paket „Microsoft.ApplicationInsights.AspNetCore“. Aktivieren Sie auf der rechten Seite das Kontrollkästchen neben *Projekt*, um alle Projekte auszuwählen. Dann wählen Sie **Deinstallieren** aus.

    ![„Abhängigkeiten entfernen“ aktivieren und dann „Deinstallieren“ auswählen](./media/remove-application-insights/uninstall-core.png)

---

## <a name="what-is-created-when-you-add-application-insights"></a>Beim Hinzufügen von Application Insights erstellte Elemente

Wenn Sie Ihrem Projekt Application Insights hinzufügen, werden Dateien erstellt und einigen Dateien wird Code hinzugefügt. Durch alleiniges Deinstallieren der NuGet-Pakete werden die Dateien und der Code nicht immer verworfen. Um Application Insights vollständig zu entfernen, sollten Sie den hinzugefügten Code bzw. die hinzugefügten Dateien sowie alle API-Aufrufe, die Sie in Ihrem Projekt hinzugefügt haben, überprüfen und manuell löschen.

# <a name="net"></a>[.NET](#tab/net)

Wenn Sie Application Insights-Telemetrie zu einem ASP.NET-Projekt in Visual Studio hinzufügen, werden folgende Dateien hinzugefügt:

- ApplicationInsights.config
- AiHandleErrorAttribute.cs

Die folgenden Codeelemente werden hinzugefügt:

- [Name Ihres Projekts].csproj

    ```C#
     <ApplicationInsightsResourceId>/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Default-ApplicationInsights-EastUS/providers/microsoft.insights/components/WebApplication4</ApplicationInsightsResourceId>
    ```

- Packages.config

    ```xml
    <packages>
    ...
    
      <package id="Microsoft.ApplicationInsights" version="2.12.0" targetFramework="net472" />
      <package id="Microsoft.ApplicationInsights.Agent.Intercept" version="2.4.0" targetFramework="net472" />
      <package id="Microsoft.ApplicationInsights.DependencyCollector" version="2.12.0" targetFramework="net472" />
      <package id="Microsoft.ApplicationInsights.PerfCounterCollector" version="2.12.0" targetFramework="net472" />
      <package id="Microsoft.ApplicationInsights.Web" version="2.12.0" targetFramework="net472" />
      <package id="Microsoft.ApplicationInsights.WindowsServer" version="2.12.0" targetFramework="net472" />
      <package id="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" version="2.12.0" targetFramework="net472" />
    
      <package id="Microsoft.AspNet.TelemetryCorrelation" version="1.0.7" targetFramework="net472" />
    
      <package id="System.Buffers" version="4.4.0" targetFramework="net472" />
      <package id="System.Diagnostics.DiagnosticSource" version="4.6.0" targetFramework="net472" />
      <package id="System.Memory" version="4.5.3" targetFramework="net472" />
      <package id="System.Numerics.Vectors" version="4.4.0" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
    ...
    </packages>
    ```

- Layout.cshtml

    Wenn Ihr Projekt über eine Datei „Layout.cshtml“ verfügt, wird der folgende Code hinzugefügt.
    
    ```html
    <head>
    ...
        <script type = 'text/javascript' >
            var appInsights=window.appInsights||function(config)
            {
                function r(config){ t[config] = function(){ var i = arguments; t.queue.push(function(){ t[config].apply(t, i)})} }
                var t = { config:config},u=document,e=window,o='script',s=u.createElement(o),i,f;for(s.src=config.url||'//az416426.vo.msecnd.net/scripts/a/ai.0.js',u.getElementsByTagName(o)[0].parentNode.appendChild(s),t.cookie=u.cookie,t.queue=[],i=['Event','Exception','Metric','PageView','Trace','Ajax'];i.length;)r('track'+i.pop());return r('setAuthenticatedUserContext'),r('clearAuthenticatedUserContext'),config.disableExceptionTracking||(i='onerror',r('_'+i),f=e[i],e[i]=function(config, r, u, e, o) { var s = f && f(config, r, u, e, o); return s !== !0 && t['_' + i](config, r, u, e, o),s}),t
            }({
                instrumentationKey:'00000000-0000-0000-0000-000000000000'
            });
            
            window.appInsights=appInsights;
            appInsights.trackPageView();
        </script>
    ...
    </head>
    ```

- ConnectedService.json

    ```json
    {
      "ProviderId": "Microsoft.ApplicationInsights.ConnectedService.ConnectedServiceProvider",
      "Version": "16.0.0.0",
      "GettingStartedDocument": {
        "Uri": "https://go.microsoft.com/fwlink/?LinkID=613413"
      }
    }
    ```

- FilterConfig.cs

    ```csharp
            public static void RegisterGlobalFilters(GlobalFilterCollection filters)
            {
                filters.Add(new ErrorHandler.AiHandleErrorAttribute());// This line was added
            }
    ```

# <a name="net-core"></a>[.NET Core](#tab/netcore)

Wenn Sie Application Insights-Telemetrie zu einem ASP.NET Core-Vorlagenprojekt in Visual Studio hinzufügen, wird folgender Code hinzugefügt:

- [Name Ihres Projekts].csproj

    ```csharp
      <PropertyGroup>
        <TargetFramework>netcoreapp3.1</TargetFramework>
        <ApplicationInsightsResourceId>/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Default-ApplicationInsights-EastUS/providers/microsoft.insights/components/WebApplication4core</ApplicationInsightsResourceId>
      </PropertyGroup>
    
      <ItemGroup>
        <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.12.0" />
      </ItemGroup>
    
      <ItemGroup>
        <WCFMetadata Include="Connected Services" />
      </ItemGroup>
    ```

- Appsettings.json:

    ```json
    "ApplicationInsights": {
        "InstrumentationKey": "00000000-0000-0000-0000-000000000000"
    ```

- ConnectedService.json
    
    ```json
    {
      "ProviderId": "Microsoft.ApplicationInsights.ConnectedService.ConnectedServiceProvider",
      "Version": "16.0.0.0",
      "GettingStartedDocument": {
        "Uri": "https://go.microsoft.com/fwlink/?LinkID=798432"
      }
    }
    ```
- Startup.cs

    ```csharp
       public void ConfigureServices(IServiceCollection services)
            {
                services.AddRazorPages();
                services.AddApplicationInsightsTelemetry(); // This is added
            }
    ```

---

## <a name="next-steps"></a>Nächste Schritte

- [Azure Monitor](../overview.md)
