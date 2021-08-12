---
title: Erstellen und Ausführen von benutzerdefinierten Verfügbarkeitstests mit Azure Functions
description: In diesem Dokument wird beschrieben, wie Sie eine Azure-Funktion mit „TrackAvailability()“ erstellen, die gemäß der Konfiguration der Funktion „TimerTrigger“ regelmäßig ausgeführt wird. Die Ergebnisse dieses Tests werden an Ihre Application Insights-Ressource gesendet. Dort können Sie für die Ergebnisdaten zur Verfügbarkeit Abfragen durchführen und Warnungen einrichten. Mit angepassten Tests können Sie komplexere Verfügbarkeitstests als bei Verwendung der Portalbenutzeroberfläche schreiben, eine App in Ihrem virtuellen Azure-Netzwerk (VNET) überwachen, die Endpunktadresse ändern oder einen Verfügbarkeitstest erstellen, falls er in Ihrer Region nicht verfügbar ist.
ms.topic: conceptual
ms.date: 05/06/2021
ms.openlocfilehash: 891f69bd42dc228e01e7b341630e7f37522f9828
ms.sourcegitcommit: c1b0d0b61ef7635d008954a0d247a2c94c1a876f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2021
ms.locfileid: "109628153"
---
# <a name="create-and-run-custom-availability-tests-using-azure-functions"></a>Erstellen und Ausführen von benutzerdefinierten Verfügbarkeitstests mit Azure Functions

In diesem Artikel wird beschrieben, wie Sie eine Azure-Funktion mit „TrackAvailability()“ erstellen, die gemäß der Konfiguration der Funktion „TimerTrigger“ regelmäßig mit Ihrer eigenen Geschäftslogik ausgeführt wird. Die Ergebnisse dieses Tests werden an Ihre Application Insights-Ressource gesendet. Dort können Sie für die Ergebnisdaten zur Verfügbarkeit Abfragen durchführen und Warnungen einrichten. So können Sie angepasste Tests auf ähnliche Weise wie per [Verfügbarkeitsüberwachung](./monitor-web-app-availability.md) im Portal erstellen. Mit angepassten Tests können Sie komplexere Verfügbarkeitstests als bei Verwendung der Portalbenutzeroberfläche schreiben, eine App in Ihrem virtuellen Azure-Netzwerk (VNET) überwachen, die Endpunktadresse ändern oder auch dann einen Verfügbarkeitstest erstellen, falls dieses Feature in Ihrer Region nicht verfügbar ist.

> [!NOTE]
> Dieses Beispiel wurde ausschließlich dazu entworfen, die Funktionsweise des API-Aufrufs „TrackAvailability()“ innerhalb einer Azure-Funktion zu veranschaulichen. Es dient nicht zur Veranschaulichung des Schreibens des zugrunde liegenden HTTP-Testcodes bzw. der Geschäftslogik, die erforderlich ist, um dies in einen voll funktionsfähigen Verfügbarkeitstest umzuwandeln. Wenn Sie dieses Beispiel durchgehen, erstellen Sie standardmäßig einen HTTP GET-Test für einfache Verfügbarkeit.

## <a name="create-a-timer-trigger-function"></a>Erstellen einer Timertriggerfunktion

1. Erstellen Sie eine Azure Functions-Ressource.
    - Wenn Sie schon eine Application Insights-Ressource verwenden:
        - Standardmäßig wird von Azure Functions eine Application Insights-Ressource erstellt, aber falls Sie eine Ihrer bereits erstellten Ressourcen verwenden möchten, müssen Sie dies bei der Erstellung angeben.
        - Folgen Sie den Anweisungen zur [Erstellung einer Azure Functions-Ressource](../../azure-functions/functions-create-scheduled-function.md#create-a-function-app) mit den folgenden Modifikationen:
            - Klicken Sie in der Registerkarte **Überwachung** auf das Application Insights-Dropdownfeld. Dann geben Sie den Namen Ihrer Ressource ein bzw. wählen Sie ihn aus.
                :::image type="content" source="media/availability-azure-functions/app-insights-resource.png" alt-text="Wählen Sie in der Registerkarte Überwachung Ihre vorhandene Application Insights-Ressource aus":::.
    - Falls Sie noch keine Application Insights-Ressource für die vom Timer ausgelöste Funktion erstellt haben:
        - Wenn Sie Ihre Azure Functions-Anwendung erstellen, wird standardmäßig eine Application Insights-Ressource für Sie erstellt. Folgen Sie den Anweisungen zur [Erstellung einer Azure Functions-Ressource](../../azure-functions/functions-create-scheduled-function.md#create-a-function-app).
    > [!NOTE]
    > Sie können Ihre Funktionen in einem Verbrauchs-, Premium- oder App Service-Plan hosten. Wenn Sie hinter einem V-NET testen oder nicht öffentliche Endpunkte testen, müssen Sie anstelle des Verbrauchs den Premium-Plan verwenden. Wählen Sie Ihren Plan in der Registerkarte **Hosting** aus.
2. Erstellen Sie eine Timertriggerfunktion.
    1. Wählen Sie in Ihrer Funktions-App die Registerkarte **Funktionen** aus.
    1. Wählen Sie **Hinzufügen** und in der Registerkarte „Funktion hinzufügen“ die folgenden Konfigurationen aus:
        1. Entwicklungsumgebung: *Im Portal entwickeln*
        1. Auswählen der Vorlage: *Timertrigger*
    1. Wählen Sie **Hinzufügen** aus, um die Timertriggerfunktion zu erstellen.

    :::image type="content" source="media/availability-azure-functions/add-function.png" alt-text="Screenshot: Hinzufügen einer Timertriggerfunktion zu Ihrer Funktions-App" lightbox="media/availability-azure-functions/add-function.png":::

## <a name="add-and-edit-code-in-the-app-service-editor"></a>Hinzufügen und Bearbeiten von Code im App Service-Editor

Navigieren Sie zu Ihrer bereitgestellten Funktions-App und wählen Sie unter *Entwicklungstools* die Registerkarte **App Service-Editor** aus.

Um eine neue Datei zu erstellen, klicken Sie mit der rechten Maustaste unter Ihrer Timertriggerfunktion (z. B. „TimerTrigger1“) und wählen Sie **Neue Datei** aus. Geben Sie dann den Namen der Datei ein und drücken Sie die Eingabetaste.

1. Erstellen Sie eine neue Datei namens „function.proj“ und fügen Sie den folgenden Code ein:

    ```xml
    <Project Sdk="Microsoft.NET.Sdk"> 
        <PropertyGroup> 
            <TargetFramework>netstandard2.0</TargetFramework> 
        </PropertyGroup> 
        <ItemGroup> 
            <PackageReference Include="Microsoft.ApplicationInsights" Version="2.15.0" /> <!-- Ensure you’re using the latest version --> 
        </ItemGroup> 
    </Project> 
    
    ```

     :::image type="content" source="media/availability-azure-functions/function-proj.png" alt-text="Screenshot der Datei „function.proj“ im App Service-Editor" lightbox="media/availability-azure-functions/function-proj.png":::

2. Erstellen Sie eine neue Datei namens „runAvailabilityTest.csx“ und fügen Sie den folgenden Code ein:

    ```csharp
    using System.Net.Http; 
    
    public async static Task RunAvailabilityTestAsync(ILogger log) 
    { 
        using (var httpClient = new HttpClient()) 
        { 
            // TODO: Replace with your business logic 
            await httpClient.GetStringAsync("https://www.bing.com/"); 
        } 
    } 
    ```

3. Kopieren Sie den unten angegebenen Code in die Datei „run.csx“ (ersetzt den vorhandenen Code):

    ```csharp
    #load "runAvailabilityTest.csx" 
    
    using System; 
    
    using System.Diagnostics; 
    
    using Microsoft.ApplicationInsights; 
    
    using Microsoft.ApplicationInsights.Channel; 
    
    using Microsoft.ApplicationInsights.DataContracts; 
    
    using Microsoft.ApplicationInsights.Extensibility; 
    
    private static TelemetryClient telemetryClient; 
    
    // ============================================================= 
    
    // ****************** DO NOT MODIFY THIS FILE ****************** 
    
    // Business logic must be implemented in RunAvailabilityTestAsync function in runAvailabilityTest.csx 
    
    // If this file does not exist, please add it first 
    
    // ============================================================= 
    
    public async static Task Run(TimerInfo myTimer, ILogger log, ExecutionContext executionContext) 
    
    { 
        if (telemetryClient == null) 
        { 
            // Initializing a telemetry configuration for Application Insights based on connection string 
    
            var telemetryConfiguration = new TelemetryConfiguration(); 
            telemetryConfiguration.ConnectionString = Environment.GetEnvironmentVariable("APPLICATIONINSIGHTS_CONNECTION_STRING"); 
            telemetryConfiguration.TelemetryChannel = new InMemoryChannel(); 
            telemetryClient = new TelemetryClient(telemetryConfiguration); 
        } 
    
        string testName = executionContext.FunctionName; 
        string location = Environment.GetEnvironmentVariable("REGION_NAME"); 
        var availability = new AvailabilityTelemetry 
        { 
            Name = testName, 
    
            RunLocation = location, 
    
            Success = false, 
        }; 
    
        availability.Context.Operation.ParentId = Activity.Current.SpanId.ToString(); 
        availability.Context.Operation.Id = Activity.Current.RootId; 
        var stopwatch = new Stopwatch(); 
        stopwatch.Start(); 
    
        try 
        { 
            using (var activity = new Activity("AvailabilityContext")) 
            { 
                activity.Start(); 
                availability.Id = Activity.Current.SpanId.ToString(); 
                // Run business logic 
                await RunAvailabilityTestAsync(log); 
            } 
            availability.Success = true; 
        } 
    
        catch (Exception ex) 
        { 
            availability.Message = ex.Message; 
            throw; 
        } 
    
        finally 
        { 
            stopwatch.Stop(); 
            availability.Duration = stopwatch.Elapsed; 
            availability.Timestamp = DateTimeOffset.UtcNow; 
            telemetryClient.TrackAvailability(availability); 
            telemetryClient.Flush(); 
        } 
    } 
    
    ```

## <a name="check-availability"></a>Überprüfen der Verfügbarkeit

Um sicherzustellen, dass alles funktioniert, können Sie sich das Diagramm auf der Registerkarte „Verfügbarkeit“ Ihrer Application Insights-Ressource ansehen.

> [!NOTE]
> Bei Tests, die mit TrackAvailability() erstellt sind, wird neben dem Testnamen **Benutzerdefiniert** angezeigt.

 :::image type="content" source="media/availability-azure-functions/availability-custom.png" alt-text="Registerkarte „Verfügbarkeit“ mit erfolgreichen Ergebnissen" lightbox="media/availability-azure-functions/availability-custom.png":::

Wählen Sie zum Anzeigen der End-to-End-Transaktionsdetails unter „Drilldown“ die Option **Erfolgreich** oder **Fehler** aus, und wählen Sie anschließend ein Beispiel aus. Sie können auf die End-to-End-Transaktionsdetails auch zugreifen, indem Sie im Graphen einen Datenpunkt auswählen.

:::image type="content" source="media/availability-azure-functions/sample.png" alt-text="Auswählen eines Verfügbarkeitstestbeispiels":::

:::image type="content" source="media/availability-azure-functions/end-to-end.png" alt-text="End-to-End-Transaktionsdetails" lightbox="media/availability-azure-functions/end-to-end.png":::

## <a name="query-in-logs-analytics"></a>Abfragen in „Protokolle (Analytics)“

Sie können „Protokolle (Analytics)“ verwenden, um Ihre Verfügbarkeitsergebnisse, Abhängigkeiten und mehr anzuzeigen. Weitere Informationen zu Protokollen finden Sie unter [Übersicht über Protokollabfragen](../logs/log-query-overview.md).

:::image type="content" source="media/availability-azure-functions/availabilityresults.png" alt-text="Verfügbarkeitsergebnisse" lightbox="media/availability-azure-functions/availabilityresults.png":::

:::image type="content" source="media/availability-azure-functions/dependencies.png" alt-text="Screenshot der Registerkarte „Neue Abfrage“ mit den auf 50 beschränkten Abhängigkeiten" lightbox="media/availability-azure-functions/dependencies.png":::

## <a name="next-steps"></a>Nächste Schritte

- [Anwendungszuordnung](./app-map.md)
- [Transaktionsdiagnose](./transaction-diagnostics.md)
