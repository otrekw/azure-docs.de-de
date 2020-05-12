---
title: Erstellen und Ausführen von benutzerdefinierten Verfügbarkeitstests mit Azure Functions
description: In diesem Dokument wird beschrieben, wie Sie eine Azure-Funktion mit „TrackAvailability()“ erstellen, die gemäß der Konfiguration der Funktion „TimerTrigger“ regelmäßig ausgeführt wird. Die Ergebnisse dieses Tests werden an Ihre Application Insights-Ressource gesendet. Dort können Sie für die Ergebnisdaten zur Verfügbarkeit Abfragen durchführen und Warnungen einrichten. Mit angepassten Tests können Sie komplexere Verfügbarkeitstests als bei Verwendung der Portalbenutzeroberfläche schreiben, eine App in Ihrem virtuellen Azure-Netzwerk (VNET) überwachen, die Endpunktadresse ändern oder einen Verfügbarkeitstest erstellen, falls er in Ihrer Region nicht verfügbar ist.
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 05/04/2020
ms.openlocfilehash: 81040adf6cfbb8820ec7f306c7d614830e3a2613
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82791107"
---
# <a name="create-and-run-custom-availability-tests-using-azure-functions"></a>Erstellen und Ausführen von benutzerdefinierten Verfügbarkeitstests mit Azure Functions

In diesem Artikel wird beschrieben, wie Sie eine Azure-Funktion mit „TrackAvailability()“ erstellen, die gemäß der Konfiguration der Funktion „TimerTrigger“ regelmäßig mit Ihrer eigenen Geschäftslogik ausgeführt wird. Die Ergebnisse dieses Tests werden an Ihre Application Insights-Ressource gesendet. Dort können Sie für die Ergebnisdaten zur Verfügbarkeit Abfragen durchführen und Warnungen einrichten. So können Sie angepasste Tests auf ähnliche Weise wie per [Verfügbarkeitsüberwachung](../../azure-monitor/app/monitor-web-app-availability.md) im Portal erstellen. Mit angepassten Tests können Sie komplexere Verfügbarkeitstests als bei Verwendung der Portalbenutzeroberfläche schreiben, eine App in Ihrem virtuellen Azure-Netzwerk (VNET) überwachen, die Endpunktadresse ändern oder auch dann einen Verfügbarkeitstest erstellen, falls dieses Feature in Ihrer Region nicht verfügbar ist.

> [!NOTE]
> Dieses Beispiel wurde ausschließlich dazu entworfen, die Funktionsweise des API-Aufrufs „TrackAvailability()“ innerhalb einer Azure-Funktion zu veranschaulichen. Es dient nicht zur Veranschaulichung des Schreibens des zugrunde liegenden HTTP-Testcodes bzw. der Geschäftslogik, die erforderlich ist, um dies in einen voll funktionsfähigen Verfügbarkeitstest umzuwandeln. Beim Durchlaufen dieses Beispiels erstellen Sie normalerweise einen Verfügbarkeitstest, der immer einen Fehler generiert.

## <a name="create-timer-triggered-function"></a>Erstellen einer Funktion mit Auslösung per Timer

- Bei Verwendung einer Application Insights-Ressource:
    - Standardmäßig wird von Azure Functions eine Application Insights-Ressource erstellt, aber falls Sie eine Ihrer bereits erstellten Ressourcen verwenden möchten, müssen Sie dies bei der Erstellung angeben.
    - Befolgen Sie die Anleitung zum [Erstellen einer Funktion in Azure, die von einem Timer ausgelöst wird](https://docs.microsoft.com/azure/azure-functions/functions-create-scheduled-function) (Stopp vor Bereinigung) mit den folgenden Auswahlmöglichkeiten.
        -  Wählen Sie oben die Registerkarte **Überwachung** aus.

            ![ Erstellen einer Azure Functions-App mit Ihrer eigenen App Insights-Ressource](media/availability-azure-functions/create-function-app.png)

        - Klicken Sie auf das Application Insights-Dropdownfeld, und geben Sie den Namen Ihrer Ressource ein, oder wählen Sie ihn aus.

            ![Auswählen einer vorhandenen Application Insights-Ressource](media/availability-azure-functions/app-insights-resource.png)

        - Klicken Sie auf **Überprüfen + erstellen**.
- Falls Sie noch keine Application Insights-Ressource für die vom Timer ausgelöste Funktion erstellt haben:
    - Wenn Sie Ihre Azure Functions-Anwendung erstellen, wird standardmäßig eine Application Insights-Ressource für Sie erstellt.
    - Befolgen Sie die Anleitung zum [Erstellen einer Funktion in Azure, die von einem Timer ausgelöst wird](https://docs.microsoft.com/azure/azure-functions/functions-create-scheduled-function) (Stopp vor Bereinigung).

## <a name="sample-code"></a>Beispielcode

Kopieren Sie den unten angegebenen Code in die Datei „run.csx“ (ersetzt den vorhandenen Code). Navigieren Sie hierzu zu Ihrer Azure Functions-Anwendung, und wählen Sie links Ihre Funktion mit Auslösung per Timer aus.

>[!div class="mx-imgBorder"]
>![„Run.csx“ einer Azure-Funktion im Azure-Portal](media/availability-azure-functions/runcsx.png)

> [!NOTE]
> Als Endpunktadresse verwenden Sie Folgendes: `EndpointAddress= https://dc.services.visualstudio.com/v2/track`. Falls sich Ihre Ressource in einer Region wie „Azure Government“ oder „Azure China“ befindet, sollten Sie sich den Artikel zum Thema [Außerkraftsetzen der Standardendpunkte](https://docs.microsoft.com/azure/azure-monitor/app/custom-endpoints#regions-that-require-endpoint-modification) durchlesen und den entsprechenden Telemetriekanal-Endpunkt für Ihre Region auswählen.

```C#
#load "runAvailabilityTest.csx"
 
using System;
using System.Diagnostics;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.ApplicationInsights.Extensibility;
 
// The Application Insights Instrumentation Key can be changed by going to the overview page of your Function App, selecting configuration, and changing the value of the APPINSIGHTS_INSTRUMENTATIONKEY Application setting.
// DO NOT replace the code below with your instrumentation key, the key's value is pulled from the environment variable/application setting key/value pair.
private static readonly string instrumentationKey = Environment.GetEnvironmentVariable("APPINSIGHTS_INSTRUMENTATIONKEY");
 
//[CONFIGURATION_REQUIRED]
// If your resource is in a region like Azure Government or Azure China, change the endpoint address accordingly.
// Visit https://docs.microsoft.com/azure/azure-monitor/app/custom-endpoints#regions-that-require-endpoint-modification for more details.
private const string EndpointAddress = "https://dc.services.visualstudio.com/v2/track";
 
private static readonly TelemetryConfiguration telemetryConfiguration = new TelemetryConfiguration(instrumentationKey, new InMemoryChannel { EndpointAddress = EndpointAddress });
private static readonly TelemetryClient telemetryClient = new TelemetryClient(telemetryConfiguration);
 
public async static Task Run(TimerInfo myTimer, ILogger log)
{
    log.LogInformation($"Entering Run at: {DateTime.Now}");
 
    if (myTimer.IsPastDue)
    {
        log.LogWarning($"[Warning]: Timer is running late! Last ran at: {myTimer.ScheduleStatus.Last}");
    }
 
    // [CONFIGURATION_REQUIRED] provide {testName} accordingly for your test function
    string testName = "AvailabilityTestFunction";
 
    // REGION_NAME is a default environment variable that comes with App Service
    string location = Environment.GetEnvironmentVariable("REGION_NAME");
 
    log.LogInformation($"Executing availability test run for {testName} at: {DateTime.Now}");
    string operationId = Guid.NewGuid().ToString("N");
 
    var availability = new AvailabilityTelemetry
    {
        Id = operationId,
        Name = testName,
        RunLocation = location,
        Success = false
    };
 
    var stopwatch = new Stopwatch();
    stopwatch.Start();
 
    try
    {
        await RunAvailbiltyTestAsync(log);
        availability.Success = true;
    }
    catch (Exception ex)
    {
        availability.Message = ex.Message;
 
        var exceptionTelemetry = new ExceptionTelemetry(ex);
        exceptionTelemetry.Context.Operation.Id = operationId;
        exceptionTelemetry.Properties.Add("TestName", testName);
        exceptionTelemetry.Properties.Add("TestLocation", location);
        telemetryClient.TrackException(exceptionTelemetry);
    }
    finally
    {
        stopwatch.Stop();
        availability.Duration = stopwatch.Elapsed;
        availability.Timestamp = DateTimeOffset.UtcNow;
 
        telemetryClient.TrackAvailability(availability);
        // call flush to ensure telemetry is sent
        telemetryClient.Flush();
    }
}

```

Wählen Sie rechts unter „Dateien anzeigen“ die Option **Hinzufügen** aus. Geben Sie der neuen Datei den Namen **function.proj**, und verwenden Sie die folgende Konfiguration.

```C#
<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
    </PropertyGroup>
    <ItemGroup>
        <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.8.2" /> <!-- Ensure you’re using the latest version -->
    </ItemGroup>
</Project>

```

>[!div class="mx-imgBorder"]
>![Klicken Sie rechts auf Hinzufügen, und nennen Sie die Datei „function.proj“](media/availability-azure-functions/addfile.png)

Wählen Sie rechts unter „Dateien anzeigen“ die Option **Hinzufügen** aus. Geben Sie der neuen Datei den Namen **runAvailabilityTest.csx**, und legen Sie die folgende Konfiguration fest.

```C#
public async static Task RunAvailbiltyTestAsync(ILogger log)
{
    // Add your business logic here.
    throw new NotImplementedException();
}

```

## <a name="check-availability"></a>Überprüfen der Verfügbarkeit

Um sicherzustellen, dass alles funktioniert, können Sie sich das Diagramm auf der Registerkarte „Verfügbarkeit“ Ihrer Application Insights-Ressource ansehen.

> [!NOTE]
> Wenn Sie Ihre eigene Geschäftslogik in „runAvailabilityTest.csx“ implementiert haben, werden Ihnen erfolgreiche Ergebnisse wie in den folgenden Screenshots angezeigt. Wenn Sie das nicht getan haben, werden Ihnen fehlerhafte Ergebnisse angezeigt. Bei mit `TrackAvailability()` erstellten Tests wird neben dem Testnamen **Benutzerdefiniert** angezeigt.

>[!div class="mx-imgBorder"]
>![Registerkarte „Verfügbarkeit“ mit erfolgreichen Ergebnissen](media/availability-azure-functions/availability-custom.png)

Wählen Sie zum Anzeigen der End-to-End-Transaktionsdetails unter „Drilldown“ die Option **Erfolgreich** oder **Fehler** aus, und wählen Sie anschließend ein Beispiel aus. Sie können auf die End-to-End-Transaktionsdetails auch zugreifen, indem Sie im Graphen einen Datenpunkt auswählen.

>[!div class="mx-imgBorder"]
>![Auswählen eines Beispielverfügbarkeitstests](media/availability-azure-functions/sample.png)

>[!div class="mx-imgBorder"]
>![End-to-End-Transaktionsdetails](media/availability-azure-functions/end-to-end.png)

Wenn Sie alles unverändert ausgeführt haben (ohne Geschäftslogik hinzuzufügen), schlägt der Test fehl.

## <a name="query-in-logs-analytics"></a>Abfragen in „Protokolle (Analytics)“

Sie können „Protokolle (Analytics)“ verwenden, um Ihre Verfügbarkeitsergebnisse, Abhängigkeiten und mehr anzuzeigen. Weitere Informationen zu Protokollen finden Sie unter [Übersicht über Protokollabfragen](../../azure-monitor/log-query/log-query-overview.md).

>[!div class="mx-imgBorder"]
>![Verfügbarkeitsergebnisse](media/availability-azure-functions/availabilityresults.png)

>[!div class="mx-imgBorder"]
>![Abhängigkeiten](media/availability-azure-functions/dependencies.png)

## <a name="next-steps"></a>Nächste Schritte

- [Anwendungszuordnung](../../azure-monitor/app/app-map.md)
- [Transaktionsdiagnose](../../azure-monitor/app/transaction-diagnostics.md)
