---
title: Datei einfügen
description: include file
services: azure-communication-services
author: peiliu
manager: vravikumar
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 06/30/2021
ms.topic: include
ms.custom: include file
ms.author: peiliu
ms.openlocfilehash: 983ce28f736bd36255e65073b202ff6c5761b059
ms.sourcegitcommit: 8b7d16fefcf3d024a72119b233733cb3e962d6d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2021
ms.locfileid: "114292429"
---
## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Die aktuelle Version des [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) für Ihr Betriebssystem.
- Eine aktive Communication Services-Ressource und eine Verbindungszeichenfolge. [Erstellen Sie eine Communication Services-Ressource](../create-communication-resource.md).
- Erstellen Sie im Azure-Portal eine [Application Insights-Ressource](../../../azure-monitor/app/create-new-resource.md).

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-c-application"></a>Erstellen einer neuen C#-Anwendung

Verwenden Sie in einem Konsolenfenster (z. B. cmd, PowerShell oder Bash) den Befehl `dotnet new` zum Erstellen einer neuen Konsolen-App mit dem Namen `TelemetryAppInsightsQuickstart`. Dieser Befehl erstellt ein einfaches „Hallo Welt“-C#-Projekt mit einer einzigen Quelldatei: **Program.cs**.

```console
dotnet new console -o TelemetryAppInsightsQuickstart
```

Wechseln Sie zum neu erstellten App-Ordner, und verwenden Sie den Befehl `dotnet build`, um Ihre Anwendung zu kompilieren.

```console
cd TelemetryAppInsightsQuickstart
dotnet build
```

### <a name="install-the-package"></a>Installieren des Pakets

Installieren Sie im Anwendungsverzeichnis mithilfe des Befehls `dotnet add package` die Azure Communication Services-Identitätsbibliothek für das .NET-Paket.

```console
dotnet add package Azure.Communication.Identity --version 1.0.0
```

Sie müssen auch die Azure Monitor Exporter for OpenTelemetry-Bibliothek installieren.

```console
dotnet add package Azure.Monitor.OpenTelemetry.Exporter
```

### <a name="set-up-the-app-framework"></a>Einrichten des App-Frameworks

Über das Projektverzeichnis:

1. Öffnen Sie die Datei **Program.cs** in einem Text-Editor.
2. Fügen Sie eine Anweisung vom Typ `using` hinzu, um den Namespace `Azure.Communication.Identity` einzuschließen.
3. Fügen Sie eine Anweisung vom Typ `using` hinzu, um den Namespace `Azure.Monitor.OpenTelemetry.Exporter` einzuschließen.
4. Fügen Sie eine Anweisung vom Typ `using` hinzu, um den Namespace `OpenTelemetry.Trace` einzuschließen.
5. Aktualisieren der `Main`-Methodendeklaration zur Unterstützung von asynchronem Code

Verwenden Sie zum Einstieg den folgenden Code:

```csharp
using System;
using Azure.Communication;
using Azure.Communication.Identity;
using Azure.Monitor.OpenTelemetry.Exporter;
using OpenTelemetry.Trace;

namespace TelemetryAppInsightsQuickstart
{
    class Program
    {
        static async Task Main(string[] args)
        {
            Console.WriteLine("Azure Communication Services - Export Telemetry to Application Insights");

            // Quickstart code goes here
        }
    }
}
```
## <a name="setting-up-the-telemetry-tracer-with-communication-identity-sdk-calls"></a>Einrichten der Telemetrie-Ablaufverfolgung mit SDK-Aufrufen für die Kommunikationsidentität

Initialisieren Sie einen Kommunikationsidentitätsclient (`CommunicationIdentityClient`) mit Ihrer Verbindungszeichenfolge. Informationen zur [Verwaltung der Verbindungszeichenfolge Ihrer Ressource](../create-communication-resource.md#store-your-connection-string).

Nachdem der Client erstellt wurde, müssen wir eine `Activity Source` definieren, die alle Aktivitäten nachverfolgt. Anschließend können Sie die `Activity Source` verwenden, um eine `Activity` zu starten, die zum Nachverfolgen des `CreateUserAsync`-SDK-Aufrufs verwendet wird. Beachten Sie, dass Sie auch benutzerdefinierte Eigenschaften definieren können, die in jeder `Activity` mithilfe der `SetTag`-Methode nachverfolgt werden sollen.

Ein ähnliches Ablaufverfolgungsmuster wird für die `GetTokenAsync`-Funktion ausgeführt.

Erstellen Sie eine neue Funktion namens `TracedSample`, und fügen Sie den folgenden Code hinzu:

```csharp
public static async Task TracedSample()
{
    // This code demonstrates how to fetch your connection string
    // from an environment variable.
    string connectionString = Environment.GetEnvironmentVariable("COMMUNICATION_SERVICES_CONNECTION_STRING");
    var client = new CommunicationIdentityClient(connectionString);

    using var source = new ActivitySource("Quickstart.IdentityTelemetry");
    CommunicationUserIdentifier identity = null;

    using (var activity = source.StartActivity("Create User Activity"))
    {
        var identityResponse = await client.CreateUserAsync();

        identity = identityResponse.Value;
        Console.WriteLine($"\nCreated an identity with ID: {identity.Id}");
        activity?.SetTag("Identity id", identity.Id);
    }

    using (var activity = source.StartActivity("Get Token Activity"))
    {
        var tokenResponse = await client.GetTokenAsync(identity, scopes: new[] { CommunicationTokenScope.Chat });

        activity?.SetTag("Token value", tokenResponse.Value.Token);
        activity?.SetTag("Expires on", tokenResponse.Value.ExpiresOn);

        Console.WriteLine($"\nIssued an access token with 'chat' scope that expires at {tokenResponse.Value.ExpiresOn}:");
    }
}
```

## <a name="funneling-telemetry-data-to-application-insights"></a>Exportieren von Telemetriedaten in Application Insights

Nachdem die SDK-Aufrufe von Aktivitäten umschlossen wurden, können Sie den OpenTelemetry-Ablaufverfolgungsexporter hinzufügen und die Daten in die Application Insights-Ressource exportieren.

Sie haben die Möglichkeit, ein Wörterbuch mit einigen Ressourcenattributen zu definieren, die in Application Insights angezeigt werden.
Rufen Sie dann `AddSource` auf, und verwenden Sie den gleichen Aktivitätsquellnamen, der in `TracedSample` definiert wurde.
Außerdem müssen Sie die Verbindungszeichenfolge aus Ihrer Application Insights-Ressource entnehmen und an `AddAzureMonitorTraceExporter()` übergeben. Dadurch werden die Telemetriedaten in Ihre Application Insights-Ressource exportiert.

Rufen Sie schließlich die Funktion `TracedSample()` mit „await“ dort auf, wo sich die SDK-Aufrufe befinden.

Fügen Sie der `Main`-Methode folgenden Code hinzu:

```csharp
var resourceAttributes = new Dictionary<string, object> { { "service.name", "<service-name>" }, { "service.instance.id", "<service-instance-id>" } };
var resourceBuilder = ResourceBuilder.CreateDefault().AddAttributes(resourceAttributes);

using var tracerProvider = Sdk.CreateTracerProviderBuilder()
    .SetResourceBuilder(resourceBuilder)
    .AddSource("Quickstart.IdentityTelemetry")
    .AddAzureMonitorTraceExporter(o =>
    {
        o.ConnectionString = Environment.GetEnvironmentVariable("APPLICATION_INSIGHTS_CONNECTION_STRING");
    })
    .Build();

await TracedSample();
```

## <a name="run-the-code"></a>Ausführen des Codes

Führen Sie die Anwendung mit dem Befehl `dotnet run` aus dem Anwendungsverzeichnis aus.

```console
dotnet run
```