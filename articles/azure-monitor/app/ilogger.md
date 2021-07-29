---
title: Application Insights-Protokollierung mit .NET
description: Erfahren Sie, wie Sie Application Insights mit der ILogger-Schnittstelle in .NET verwenden.
ms.topic: conceptual
ms.date: 05/20/2021
ms.reviewer: mbullwin
ms.openlocfilehash: 6b0c746ffda97c22dc42a4c2016413a80552d28d
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110461515"
---
# <a name="application-insights-logging-with-net"></a>Application Insights-Protokollierung mit .NET

In diesem Artikel erfahren Sie, wie Sie Protokolle mit Application Insights in .NET-Apps mithilfe mehrerer NuGet-Pakete erfassen:

- **Core-Paket:**
  - [`Microsoft.Extensions.Logging.ApplicationInsights`][nuget-ai]
- **Workloadpakete:**
  - [`Microsoft.ApplicationInsights.AspNetCore`][nuget-ai-anc]
  - [`Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel`][nuget-ai-ws-tc]

[nuget-ai]: https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights
[nuget-ai-anc]: https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore
[nuget-ai-ws]: https://www.nuget.org/packages/Microsoft.ApplicationInsights.WorkerService
[nuget-ai-ws-tc]: https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel

> [!TIP]
> Das [`Microsoft.ApplicationInsights.WorkerService`][nuget-ai-ws] NuGet-Paket geht über den Rahmen dieses Artikels hinaus. Es kann verwendet werden, um Application Insights für Hintergrunddienste zu aktivieren. Weitere Informationen finden Sie unter [Application Insights für Workerdienst-Apps](./worker-service.md).

Abhängig vom Application Insights-Protokollierungspaket, das Sie verwenden, gibt es verschiedene Möglichkeiten zum Registrieren von `ApplicationInsightsLoggerProvider`. Der `ApplicationInsightsLoggerProvider` ist eine Implementierung des <xref:Microsoft.Extensions.Logging.ILoggerProvider>, der für die Bereitstellung der <xref:Microsoft.Extensions.Logging.ILogger>- und <xref:Microsoft.Extensions.Logging.ILogger%601>-Implementierungen verantwortlich ist.

## <a name="aspnet-core-applications"></a>ASP.NET Core-Anwendungen

Verwenden Sie das `Microsoft.ApplicationInsights.AspNetCore` NuGet-Paket, um ASP.NET Core-Anwendungen Telemetriedaten hinzuzufügen. Dies kann über [Visual Studio als verbundener Dienst](/visualstudio/azure/azure-app-insights-add-connected-service) oder manuell konfiguriert werden.

Standardmäßig haben ASP.NET Core-Anwendungen Application Insights-Protokollierungsanbieter registriert, wenn sie mit [Code](./asp-net-core.md) oder mithilfe eines [codelosen](./azure-web-apps.md?tabs=netcore#enable-agent-based-monitoring) Ansatzes konfiguriert wurden. Der registrierte Anbieter ist so konfiguriert, dass Protokollereignisse mit einem Schweregrad von <xref:Microsoft.Extensions.Logging.LogLevel.Warning?displayProperty=nameWithType> oder höher automatisch erfasst werden. Schweregrad und Kategorien können angepasst werden. Weitere Informationen finden Sie unter [Steuern des Protokolliergrads](#control-logging-level).

1. Vergewissern Sie sich, dass das NuGet-Paket installiert ist:

   ```xml
    <ItemGroup>
        <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.17.0" />
    </ItemGroup>
   ```

1. Stellen Sie sicher, dass die `Startup.ConfigureServices`-Methode `services.AddApplicationInsightsTelemetry` aufruft:

    ```csharp
    using Microsoft.AspNetCore.Builder;
    using Microsoft.AspNetCore.Hosting;
    using Microsoft.AspNetCore.Http;
    using Microsoft.Extensions.DependencyInjection;
    using Microsoft.Extensions.Hosting;
    using Microsoft.Extensions.Configuration;
    
    namespace WebApplication
    {
        public class Startup
        {
            public Startup(IConfiguration configuration)
            {
                Configuration = configuration;
            }

            public IConfiguration Configuration { get; }

            public void ConfigureServices(IServiceCollection services)
            {
                services.AddApplicationInsightsTelemetry(
                    Configuration["APPINSIGHTS_CONNECTIONSTRING"]);

                // An alternative overload, when not using appsettings.json or user secrets.
                // services.AddApplicationInsightsTelemetry();
            }

            public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
            {
                // omitted for brevity
            }
        }
    }
    ```

Wenn das NuGet-Paket installiert ist und der Anbieter mit Abhängigkeitsinjektion registriert wird, kann die App Protokollierungen vornehmen. Erfordern Sie bei der Konstruktorinjektion entweder <xref:Microsoft.Extensions.Logging.ILogger> oder die generische Alternative <xref:Microsoft.Extensions.Logging.ILogger%601>. Wenn diese Implementierungen aufgelöst werden, stellt `ApplicationInsightsLoggerProvider` diese zur Verfügung. Protokollierte Meldungen oder Ausnahmen werden an Application Insights gesendet. Betrachten Sie den folgenden Beispielcontroller:

```csharp
public class ValuesController : ControllerBase
{
    private readonly ILogger _logger;

    public ValuesController(ILogger<ValuesController> logger)
    {
        _logger = logger;
    }

    [HttpGet]
    public ActionResult<IEnumerable<string>> Get()
    {
        _logger.LogWarning("An example of a Warning trace..");
        _logger.LogError("An example of an Error level message");

        return new string[] { "value1", "value2" };
    }
}
```

Weitere Informationen finden Sie unter [Logging in ASP.NET Core](/aspnet/core/fundamentals/logging) (Protokollierung in ASP.NET Core).

### <a name="capture-logs-within-aspnet-core-startup-code"></a>Erfassen von Protokollen im ASP.NET Core-Startcode

In einigen Szenarien müssen Protokolle als Teil der App-Startroutine erfasst werden, bevor die Anforderung/Antwort-Pipeline bereit ist, Anforderungen zu akzeptieren. `ILogger`-Implementierungen sind jedoch nicht einfach über die Abhängigkeitsinjektion in *Program.cs* und *Startup.cs* verfügbar. Weitere Informationen finden Sie unter [Protokollierung in .NET: Erstellen von Protokollen in `Main`](/dotnet/core/extensions/logging?tabs=command-line#create-logs-in-main).

Bei der Protokollierung über *Program.cs* und *Startup.cs* gelten mehrere Einschränkungen:

* Telemetriedaten werden über den Telemetriekanal [InMemoryChannel](./telemetry-channels.md) gesendet.
* Auf Telemetriedaten wird keine [Stichprobenentnahme](./sampling.md) angewendet.
* Standard-[Telemetrieinitialisierer oder -Telemetrieprozessoren](./api-filtering-sampling.md) sind nicht verfügbar.

Die folgenden Beispiele veranschaulichen dies, indem *Program.cs* und *Startup.cs* explizit instanziiert und konfiguriert werden:

#### <a name="example-programcs"></a>Beispiel für „Program.cs“

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Logging.ApplicationInsights;

namespace WebApplication
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = CreateHostBuilder(args).Build();

            var logger = host.Services.GetRequiredService<ILogger<Program>>();
            logger.LogInformation("From Program, running the host now.");

            host.Run();
        }

        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                })
                .ConfigureLogging((context, builder) =>
                {
                    // Providing an instrumentation key is required if you're using the
                    // standalone Microsoft.Extensions.Logging.ApplicationInsights package,
                    // or when you need to capture logs during application startup, for example
                    // in the Program.cs or Startup.cs itself.
                    builder.AddApplicationInsights(
                        context.Configuration["APPINSIGHTS_CONNECTIONSTRING"]);

                    // Capture all log-level entries from Program
                    builder.AddFilter<ApplicationInsightsLoggerProvider>(
                        typeof(Program).FullName, LogLevel.Trace);

                    // Capture all log-level entries from Startup
                    builder.AddFilter<ApplicationInsightsLoggerProvider>(
                        typeof(Startup).FullName, LogLevel.Trace);
                });
    }
}
```

Im vorangehenden Code wird `ApplicationInsightsLoggerProvider` mit Ihrer `"APPINSIGHTS_CONNECTIONSTRING"`-Verbindungszeichenfolge konfiguriert, und es werden Filter angewendet, die die Protokollebene auf <xref:Microsoft.Extensions.Logging.LogLevel.Trace?displayProperty=nameWithType> festlegen.

> [!IMPORTANT]
> [Verbindungszeichenfolgen](./sdk-connection-string.md?tabs=net) werden gegenüber Instrumentierungsschlüsseln empfohlen. Neue Azure-Regionen **erfordern** die Verwendung von Verbindungszeichenfolgen anstelle von Instrumentierungsschlüsseln. Die Verbindungszeichenfolge identifiziert die Ressource, der Sie Ihre Telemetriedaten zuordnen möchten. Hier können Sie die Endpunkte ändern, die Ihre Ressource als Ziel für die Telemetrie verwendet. Sie müssen die Verbindungszeichenfolge kopieren und dem Code Ihrer Anwendung oder einer Umgebungsvariable hinzufügen.

#### <a name="example-startupcs"></a>Beispiel für „Startup.cs“

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;

namespace WebApplication
{
    public class Startup
    {
        public Startup(IConfiguration configuration)
        {
            Configuration = configuration;
        }

        public IConfiguration Configuration { get; }

        public void ConfigureServices(IServiceCollection services)
        {
            services.AddApplicationInsightsTelemetry(
                Configuration["APPINSIGHTS_CONNECTIONSTRING"]);
        }

        // The ILogger<Startup> is resolved by dependency injection
        // and available in Startup.Configure.
        public void Configure(
            IApplicationBuilder app, IWebHostEnvironment env, ILogger<Startup> logger)
        {
            logger.LogInformation(
                "Configuring for {Environment} environment",
                env.EnvironmentName);

            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }

            app.UseRouting();
            app.UseEndpoints(endpoints =>
            {
                endpoints.MapGet("/", async context =>
                {
                    await context.Response.WriteAsync("Hello World!");
                });
            });
        }
    }
}
```

## <a name="console-application"></a>Konsolenanwendung

Installierte Pakete:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.Extensions.DependencyInjection" Version="5.0.0" />
  <PackageReference Include="Microsoft.Extensions.Logging.ApplicationInsights" Version="2.17.0"/>
</ItemGroup>
```

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Logging.ApplicationInsights;
using System;
using System.Threading.Tasks;

namespace ConsoleApp
{
    class Program
    {
        static async Task Main(string[] args)
        {
            using var channel = new InMemoryChannel();

            try
            {
                IServiceCollection services = new ServiceCollection();
                services.Configure<TelemetryConfiguration>(config => config.TelemetryChannel = channel);
                services.AddLogging(builder =>
                {
                    // Only Application Insights is registered as a logger provider
                    builder.AddApplicationInsights("<YourInstrumentationKey>");
                });

                IServiceProvider serviceProvider = services.BuildServiceProvider();
                ILogger<Program> logger = serviceProvider.GetRequiredService<ILogger<Program>>();

                logger.LogInformation("Logger is working...");
            }
            finally
            {
                // Explicitly call Flush() followed by delay is required in Console Apps.
                // This is to ensure that even if application terminates, telemetry is sent to the back-end.
                channel.Flush();

                await Task.Delay(TimeSpan.FromMilliseconds(1000));
            }
        }
    }
}

```

Im vorherigen Beispiel wird das `Microsoft.Extensions.Logging.ApplicationInsights`-Paket verwendet. Standardmäßig verwendet diese Konfiguration für `TelemetryConfiguration` das absolute Minimum zum Senden von Daten an Application Insights. Absolutes Minimum bedeutet, dass als Kanal `InMemoryChannel` verwendet wird. Es erfolgt keine Stichprobenentnahme, und es gibt keinen Standard-`TelemetryInitializer`. Dieses Verhalten kann für eine Konsolenanwendung außer Kraft gesetzt werden. Dies wird im folgenden Beispiel gezeigt.

Installieren Sie dieses zusätzliche Paket:

```xml
<PackageReference Include="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" Version="2.17.0" />
```

Im folgenden Abschnitt wird gezeigt, wie die Standard-`TelemetryConfiguration` mithilfe der Methode <xref:Microsoft.Extensions.Options.ConfigureOptions%601.Configure(%600)> außer Kraft gesetzt werden kann. In diesem Beispiel werden `ServerTelemetryChannel` und die Stichprobenentnahme eingerichtet. Der TelemetryConfiguration wird ein benutzerdefinierter ITelemetryInitializer hinzugefügt.

```csharp
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Logging.ApplicationInsights;
using System;
using System.Threading.Tasks;

namespace ConsoleApp
{
    class Program
    {
        static async Task Main(string[] args)
        {
            using var channel = new ServerTelemetryChannel();

            try
            {
                IServiceCollection services = new ServiceCollection();
                services.Configure<TelemetryConfiguration>(
                    config =>
                    {
                        config.TelemetryChannel = channel;

                        // Optional: implement your own TelemetryInitializer and configure it here
                        // config.TelemetryInitializers.Add(new MyTelemetryInitializer());

                        config.DefaultTelemetrySink.TelemetryProcessorChainBuilder.UseSampling(5);
                        channel.Initialize(config);
                    });

                services.AddLogging(builder =>
                {
                    // Only Application Insights is registered as a logger provider
                    builder.AddApplicationInsights("<YourInstrumentationKey>");
                });

                IServiceProvider serviceProvider = services.BuildServiceProvider();
                ILogger<Program> logger = serviceProvider.GetRequiredService<ILogger<Program>>();

                logger.LogInformation("Logger is working...");
            }
            finally
            {
                // Explicitly call Flush() followed by delay is required in Console Apps.
                // This is to ensure that even if application terminates, telemetry is sent to the back-end.
                channel.Flush();

                await Task.Delay(TimeSpan.FromMilliseconds(1000));
            }
        }
    }
}
```

## <a name="control-logging-level"></a>Steuern des Protokolliergrads

`ILogger`-Implementierungen verfügen über einen integrierten Mechanismus zum Anwenden der [Protokollfilterung](/dotnet/core/extensions/logging#how-filtering-rules-are-applied). Damit können Sie die Protokolle steuern, die an jeden registrierten Anwender gesendet werden, einschließlich des Application Insights-Anbieters. Die Filterung kann entweder in der Konfiguration, normalerweise mit einer Datei *appsettings.json*, oder im Code erfolgen.

In den folgenden Beispielen wird die Anwendung von Filterregeln auf `ApplicationInsightsLoggerProvider` veranschaulicht.

### <a name="create-filter-rules-in-configuration-with-appsettingsjson"></a>Erstellen von Filterregeln in der Konfiguration mit „appsettings.json“

Der Alias von `ApplicationInsightsLoggerProvider` lautet "ApplicationInsights". Im folgenden Abschnitt von *appsettings.json* wird die Standardprotokollebene <xref:Microsoft.Extensions.Logging.LogLevel.Warning?displayProperty=nameWithType> von Application Insights überschrieben, um Kategorien zu protokollieren, die mit "Microsoft" auf der Ebene <xref:Microsoft.Extensions.Logging.LogLevel.Error?displayProperty=nameWithType> und höher beginnen.

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    },
    "ApplicationInsights": {
      "LogLevel": {
        "Microsoft": "Error"
      }
    }
  }
}
```

### <a name="create-filter-rules-in-code"></a>Erstellen von Filterregeln im Code

Der folgende Codeausschnitt konfiguriert, dass Protokolle für <xref:Microsoft.Extensions.Logging.LogLevel.Warning?displayProperty=nameWithType> und höher aus allen Kategorien und für <xref:Microsoft.Extensions.Logging.LogLevel.Error?displayProperty=nameWithType> und höher aus Kategorien, die mit „Microsoft“ beginnen, an `ApplicationInsightsLoggerProvider` gesendet werden.

```csharp
Host.CreateDefaultBuilder(args)
    .UseStartup<Startup>()
    .ConfigureLogging(builder =>
    {
        builder.AddFilter<ApplicationInsightsLoggerProvider>("", LogLevel.Warning);
        builder.AddFilter<ApplicationInsightsLoggerProvider>("Microsoft", LogLevel.Error);
    });
```

Dieser vorangehende Code entspricht funktional dem vorherigen Abschnitt in *appsettings.json*. Weitere Informationen finden Sie unter [Konfiguration in .NET](/dotnet/core/extensions/configuration).

## <a name="logging-scopes"></a>Protokollierungsbereiche

Der `ApplicationInsightsLoggingProvider` unterstützt [Protokollbereiche](/dotnet/core/extensions/logging#log-scopes), und Bereiche sind standardmäßig aktiviert. Bei einem Bereich vom Typ `IReadOnlyCollection<KeyValuePair<string,object>>` wird jedes Schlüssel-Wert-Paar in der Sammlung als benutzerdefinierte Eigenschaft zur Application Insights-Telemetrie hinzugefügt. Im folgenden Beispiel werden Protokolle als `TraceTelemetry` aufgezeichnet und enthalten `("MyKey", "MyValue")` in den Eigenschaften.

```csharp
using (_logger.BeginScope(new Dictionary<string, object> { ["MyKey"] = "MyValue" }))
{
    _logger.LogError("An example of an Error level message");
}
```

Wenn ein anderer Typ als Bereich verwendet wird, wird dieser unter der Eigenschaft „Scope“ in der Application Insights-Telemetrie gespeichert. Im folgenden Beispiel verfügt `TraceTelemetry` über eine Eigenschaft namens "Scope", die den Bereich enthält.

```csharp
    using (_logger.BeginScope("hello scope"))
    {
        _logger.LogError("An example of an Error level message");
    }
```

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="what-are-the-old-and-new-versions-of-applicationinsightsloggerprovider"></a>Welche ist die alte und welche die neue Version von ApplicationInsightsLoggerProvider?

[Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) enthielt einen integrierten ApplicationInsightsLoggerProvider (Microsoft.ApplicationInsights.AspNetCore.Logging.ApplicationInsightsLoggerProvider), der über **ILoggerFactory**-Erweiterungsmethoden aktiviert wurde. Dieser Anbieter ist seit Version 2.7.1 als veraltet markiert. Er wird mit der Umstellung auf die nächste Hauptversion komplett entfernt. Das Paket [Microsoft.ApplicationInsights.AspNetCore 2.6.1](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) selbst ist nicht veraltet. Es wird benötigt zum Überwachen von Anforderungen, Abhängigkeiten usw.

Die empfohlene Alternative ist das neue eigenständige Paket [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights), das einen verbesserten ApplicationInsightsLoggerProvider (Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider) und Erweiterungsmethoden in ILoggerBuilder für dessen Aktivierung enthält.

[Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) Version 2.7.1 ist vom neuen Paket abhängig und aktiviert die ILogger-Erfassung automatisch.

### <a name="why-are-some-ilogger-logs-shown-twice-in-application-insights"></a>Warum werden einige ILogger-Protokolle in Application Insights zweimal angezeigt?

Duplizierung kann auftreten, wenn die ältere (nun veraltete) Version von ApplicationInsightsLoggerProvider durch Aufrufen von `AddApplicationInsights` für `ILoggerFactory` aktiviert wurde. Überprüfen Sie, ob Ihre **Configure**-Methode Folgendes enthält, und entfernen Sie den Eintrag:

```csharp
 public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
 {
     loggerFactory.AddApplicationInsights(app.ApplicationServices, LogLevel.Warning);
     // ..other code.
 }
```

Wenn Sie beim Debuggen aus Visual Studio doppelte Protokollierung feststellen, legen Sie im Code, der Application Insights aktiviert, `EnableDebugLogger` wie folgt auf `false` fest. Diese Duplizierung und die Behebung des Problems betreffen nur das Debuggen der Anwendung.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var options = new ApplicationInsightsServiceOptions
    {
        EnableDebugLogger = false
    }
    services.AddApplicationInsightsTelemetry(options);
    // ...
}
```

### <a name="i-updated-to-microsoftapplicationinsightsaspnet-sdk-version-271-and-logs-from-ilogger-are-captured-automatically-how-do-i-turn-off-this-feature-completely"></a>Ich habe ein Update auf [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) Version 2.7.1 durchgeführt, und Protokolle aus ILogger werden automatisch erfasst. Wie kann ich dieses Feature vollständig deaktivieren?

Allgemeine Informationen zum Filtern von Protokollen finden Sie im Abschnitt [Steuern des Protokolliergrads](#control-logging-level). Zum Deaktivieren von ApplicationInsightsLoggerProvider verwenden Sie `LogLevel.None`:

In Ihrem Aufruf zur Konfiguration der Protokollierung, wobei `builder` ein <xref:Microsoft.Extensions.Logging.ILoggingBuilder> ist:

```csharp
builder.AddFilter<ApplicationInsightsLoggerProvider>("", LogLevel.None);
```

Über die Datei *appsettings.json*:

```json
{
    "Logging": {
        "ApplicationInsights": {
            "LogLevel": {
                "Default": "None"
            }
        }
    }
}
```

### <a name="why-do-some-ilogger-logs-not-have-the-same-properties-as-others"></a>Warum weisen einige ILogger-Protokolle nicht dieselben Eigenschaften wie andere auf?

Application Insights erfasst und sendet ILogger-Protokolle mit derselben TelemetryConfiguration, die auch für andere Telemetriedaten verwendet wird. Es gibt jedoch eine Ausnahme. In der Standardeinstellung ist TelemetryConfiguration nicht vollständig eingerichtet, wenn Sie aus *Program.cs* oder *Startup.cs* protokollieren. Protokolle aus diesen Quellen weisen nicht die Standardkonfiguration auf, daher führen sie nicht alle TelemetryInitializers und TelemetryProcessors aus.

### <a name="im-using-the-standalone-package-microsoftextensionsloggingapplicationinsights-and-i-want-to-log-some-additional-custom-telemetry-manually-how-should-i-do-that"></a>Ich verwende das eigenständige Paket „Microsoft.Extensions.Logging.ApplicationInsights“ und möchte einige zusätzliche benutzerdefinierte Telemetriedaten manuell protokollieren. Wie sollte ich dazu vorgehen?

Bei Verwendung des eigenständigen Pakets wird `TelemetryClient` nicht in den DI-Container eingefügt. Deshalb müssen Sie eine neue Instanz von `TelemetryClient` erstellen und die gleiche Konfiguration wie der Protokollanbieter verwenden. Dies wird im folgenden Code veranschaulicht. So wird sichergestellt, dass dieselbe Konfiguration für alle benutzerdefinierten Telemetriedaten sowie für die Telemetriedaten von ILogger verwendet wird.

```csharp
public class MyController : ApiController
{
   // This TelemetryClient can be used to track additional telemetry using TrackXXX() api.
   private readonly TelemetryClient _telemetryClient;
   private readonly ILogger _logger;

   public MyController(IOptions<TelemetryConfiguration> options, ILogger<MyController> logger)
   {
        _telemetryClient = new TelemetryClient(options.Value);
        _logger = logger;
   }  
}
```

> [!NOTE]
> Wenn Sie Application Insights mit dem Paket `Microsoft.ApplicationInsights.AspNetCore` aktivieren, ändern Sie diesen Code, um `TelemetryClient` direkt in den Konstruktor abzurufen. Ein Beispiel finden Sie in diesen [häufig gestellten Fragen](./asp-net-core.md#frequently-asked-questions).

### <a name="what-application-insights-telemetry-type-is-produced-from-ilogger-logs-or-where-can-i-see-ilogger-logs-in-application-insights"></a>Welcher Typ von Application Insights-Telemetriedaten wird anhand von ILogger-Protokollen erstellt? Wo kann ich ILogger-Protokolle in Application Insights einsehen?

ApplicationInsightsLoggerProvider erfasst ILogger-Protokolle und erstellt daraus TraceTelemetry. Wenn ein Ausnahmeobjekt an die `Log`-Methode in `ILogger` übergeben wird, wird anstelle von TraceTelemetry *ExceptionTelemetry* erstellt. Diese Telemetrieelemente sind an denselben Orten wie jede andere TraceTelemetry oder ExceptionTelemetry für Application Insights zu finden, einschließlich Portal, Analysen oder lokalem Visual Studio-Debugger.

Wenn Sie immer `TraceTelemetry` senden möchten, verwenden Sie diesen Codeausschnitt:

```csharp
builder.AddApplicationInsights(
    options => options.TrackExceptionsAsExceptionTelemetry = false);
```

### <a name="i-dont-have-the-sdk-installed-and-i-use-the-azure-web-apps-extension-to-enable-application-insights-for-my-aspnet-core-applications-how-do-i-use-the-new-provider"></a>Ich habe das SDK nicht installiert und verwende die Azure-Web-Apps-Erweiterung, um Application Insights für meine ASP.NET Core-Anwendungen zu aktivieren. Wie verwende ich den neuen Anbieter? 

Die Application Insights-Erweiterung in Azure-Web-Apps verwendet den neuen Anbieter. Sie können die Filterregeln in der Datei *appsettings.json* für die Anwendung ändern.

## <a name="next-steps"></a>Nächste Schritte

* [Protokollierung in .NET](/dotnet/core/extensions/logging)
* [Protokollierung in ASP.NET Core](/aspnet/core/fundamentals/logging)
* [.NET-Ablaufverfolgungsprotokolle in Application Insights](./asp-net-trace-logs.md)
