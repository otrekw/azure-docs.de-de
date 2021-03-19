---
title: Leitfaden für isolierte .NET-Prozesse für .NET 5.0 in Azure Functions
description: Hier erfahren Sie, wie Sie einen isolierten .NET-Prozess verwenden, um C#-Funktionen für .NET 5.0 prozessextern in Azure auszuführen.
ms.service: azure-functions
ms.topic: conceptual
ms.date: 03/01/2021
ms.custom: template-concept
ms.openlocfilehash: dacf3436ce98d839ad5b45361f1573c98c62d3e7
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102563641"
---
# <a name="guide-for-running-functions-on-net-50-in-azure"></a>Leitfaden: Ausführen von .NET 5.0-Funktionen in Azure

Dieser Artikel ist eine Einführung in die Verwendung von C#, um isolierte .NET-Prozessfunktionen zu entwickeln, die prozessextern in Azure Functions ausgeführt werden. Durch die prozessexterne Ausführung können Sie den Funktionscode von der Azure Functions-Runtime entkoppeln. Sie ermöglicht zudem die Erstellung und Ausführung von Funktionen, die auf das aktuelle .NET 5.0-Release ausgelegt sind. 

| Erste Schritte | Konzepte| Beispiele |
|--|--|--| 
| <ul><li>[Verwendung von Visual Studio Code](dotnet-isolated-process-developer-howtos.md?pivots=development-environment-vscode)</li><li>[Verwenden von Befehlszeilentools](dotnet-isolated-process-developer-howtos.md?pivots=development-environment-cli)</li><li>[Verwenden von Visual Studio](dotnet-isolated-process-developer-howtos.md?pivots=development-environment-vs)</li></ul> | <ul><li>[Hostingoptionen](functions-scale.md)</li><li>[Überwachung](functions-monitoring.md)</li> | <ul><li>[Referenzbeispiele](https://github.com/Azure/azure-functions-dotnet-worker/tree/main/samples)</li></ul> |

Wenn Sie .NET 5.0 nicht unterstützen müssen oder Ihre Funktionen prozessextern ausführen, sollten Sie stattdessen [eine C#-Klassenbibliotheksfunktion entwickeln](functions-dotnet-class-library.md).

## <a name="why-net-isolated-process"></a>Gründe für isolierte .NET-Prozesse

Zuvor hat Azure Functions nur einen stark integrierten Modus für .NET-Funktionen unterstützt, die als [Klassenbibliothek](functions-dotnet-class-library.md) im selben Prozess wie der Host ausgeführt werden. Dieser Modus ermöglicht die enge Verzahnung zwischen dem Hostprozess und den Funktionen. Funktionen der .NET-Klassenbibliothek können beispielsweise Bindungs-APIs und Bindungstypen freigeben. Diese Integration erfordert jedoch auch eine engere Kopplung zwischen dem Hostprozess und der .NET-Funktion. Prozessintern ausgeführte .NET-Funktionen müssen beispielsweise die gleiche .NET-Version wie die Functions-Runtime aufweisen. Sie können diese Einschränkungen jetzt umgehen, indem Sie sie in einem isolierten Prozess ausführen. Dank dieser Prozessisolierung können Sie Funktionen für aktuelle .NET-Releases (wie .NET 5.0) entwickeln, die von der Functions-Runtime nicht nativ unterstützt werden.

Da diese Funktionen in einem separaten Prozess ausgeführt werden, bestehen einige [Feature- und Funktionsunterschiede](#differences-with-net-class-library-functions) zwischen Apps mit isolierten .NET-Funktionen und Apps mit Funktionen der .NET-Klassenbibliothek.

### <a name="benefits-of-running-out-of-process"></a>Vorteile der prozessexternen Ausführung

Bei der prozessexternen Ausführung bestehen die folgenden Vorteile für Ihre .NET-Funktionen: 

+ Weniger Konflikte: Da die Funktionen in einem separaten Prozess ausgeführt werden, geraten die Assemblys Ihrer App nicht mit unterschiedlichen Versionen der gleichen Assemblys in Konflikt, die vom Hostprozess verwendet werden.  
+ Vollständige Kontrolle über den Prozess: Sie steuern den Start der App, die verwendeten Konfigurationen und die Middleware.
+ Dependency Injection: Da Sie die vollständige Kontrolle über den Prozess haben, können Sie aktuelle .NET-Verhaltensweisen für die Dependency Injection und die Einbindung von Middleware in Ihre Funktions-App nutzen. 

## <a name="supported-versions"></a>Unterstützte Versionen

Derzeit wird lediglich .NET 5.0 für die prozessexterne Ausführung unterstützt.

## <a name="net-isolated-project"></a>Isoliertes .NET-Projekt

Ein Projekt mit isolierten .NET-Funktionen ist im Grunde ein Projekt für eine .NET-Konsolen-App mit .NET 5.0 als Ziel. Die folgenden Dateien werden in jedem isolierten .NET-Projekt benötigt:

+ [host.json](functions-host-json.md)
+ [local.settings.json](functions-run-local.md#local-settings-file)
+ C#-Projektdatei (.csproj), definiert das Projekt und die Abhängigkeiten
+ Program.cs, der Einstiegspunkt für die App

## <a name="package-references"></a>Paketverweise

Bei der prozessexternen Ausführung verwendet Ihr .NET-Projekt spezifische Pakete, die die Kernfunktionalitäten und die Bindungserweiterungen implementieren. 

### <a name="core-packages"></a>Erforderliche Pakete 

Die folgenden Pakete werden benötigt, damit Ihre .NET-Funktionen in einem isolierten Prozess ausgeführt werden können:

+ [Microsoft.Azure.Functions.Worker](https://www.nuget.org/packages/Microsoft.Azure.Functions.Worker/)
+ [Microsoft.Azure.Functions.Worker.Sdk](https://www.nuget.org/packages/Microsoft.Azure.Functions.Worker.Sdk/)

### <a name="extension-packages"></a>Erweiterungspakete

Da in einem isolierten .NET-Prozess ausgeführte Funktionen andere Bindungstypen verwenden, benötigen sie spezifische Pakete mit Bindungserweiterungen. 

Diese Erweiterungspakete finden Sie unter [Microsoft.Azure.Functions.Worker.Extensions](https://www.nuget.org/packages?q=Microsoft.Azure.Functions.Worker.Extensions).

## <a name="start-up-and-configuration"></a>Start und Konfiguration 

Wenn Sie isolierte .NET-Funktionen verwenden, haben Sie Zugriff auf die Startklasse für Ihre Funktions-App, die sich üblicherweise in „Program.cs“ befindet. Sie sind dafür verantwortlich, eine eigene Hostinstanz zu erstellen und zu starten. Sie haben daher auch direkten Zugriff auf die Konfigurationspipeline für Ihre App. Bei der prozessexternen Ausführung sind die Dependency Injection und die Ausführung von Middleware einfacher. 

Im Folgenden sehen Sie ein Beispiel für eine `HostBuilder`-Pipeline:

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/FunctionApp/Program.cs" id="docsnippet_startup":::

Eine `HostBuilder`-Pipeline wird verwendet, um eine vollständig initialisierte Instanz von `IHost` zu erstellen und zurückzugeben. Diese führen Sie asynchron aus, um Ihre Funktions-App zu starten. 

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/FunctionApp/Program.cs" id="docsnippet_host_run":::

### <a name="configuration"></a>Konfiguration

Da Sie Zugriff auf die HostBuilder-Pipeline haben, können Sie App-spezifische Konfigurationen während der Initialisierung festlegen. Diese Konfigurationen gelten für die in einem separaten Prozess ausgeführte Funktions-App. Wenn Sie Änderungen am Funktionshost oder -trigger und der Bindungskonfiguration vornehmen möchten, müssen Sie weiterhin die Datei [host.json](functions-host-json.md) verwenden.      

Im folgenden Beispiel wird gezeigt, wie Sie eine Konfigurationsargumente hinzufügen, die als Befehlszeilenargumente gelesen werden: 
 
:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/FunctionApp/Program.cs" id="docsnippet_configure_app" :::

Die Methode `ConfigureAppConfiguration` wird verwendet, um die restlichen Konfigurationen für den Buildprozess und die Anwendung vorzunehmen. In diesem Beispiel wird auch eine [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder?view=dotnet-plat-ext-5.0&preserve-view=true)-Schnittstelle verwendet, die das Hinzufügen mehrerer Konfigurationselemente vereinfacht. Da `ConfigureAppConfiguration` die gleiche Instanz von [`IConfiguration`](/dotnet/api/microsoft.extensions.configuration.iconfiguration?view=dotnet-plat-ext-5.0&preserve-view=true) zurückgibt, können Sie diese Methode mehrfach aufrufen, um mehrere Konfigurationselemente hinzuzufügen. Über [`HostBuilderContext.Configuration`](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration?view=dotnet-plat-ext-5.0&preserve-view=true) und [`IHost.Services`](/dotnet/api/microsoft.extensions.hosting.ihost.services?view=dotnet-plat-ext-5.0&preserve-view=true) können Sie auf sämtliche Konfigurationen zugreifen.

Weitere Informationen zur Konfiguration finden Sie unter [Konfiguration in ASP.NET Core](/aspnet/core/fundamentals/configuration/?view=aspnetcore-5.0&preserve-view=true). 

### <a name="dependency-injection"></a>Dependency Injection

Im Vergleich zu .NET-Klassenbibliotheken ist die Dependency Injection vereinfacht. Sie müssen keine Startklasse für die Dienstregistrierung erstellen, sondern nur `ConfigureServices` in der HostBuilder-Pipeline aufrufen und die Erweiterungsmethode in [`IServiceCollection`](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection?view=dotnet-plat-ext-5.0&preserve-view=true) verwenden, um bestimmte Dienste einzufügen. 

Im folgenden Beispiel wird eine Dependency Injection für einen Singletondienst durchgeführt:  
 
:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/FunctionApp/Program.cs" id="docsnippet_dependency_injection" :::

Weitere Informationen finden Sie unter [Dependency Injection in ASP.NET Core](/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-5.0&preserve-view=true).

<!--### Middleware

.NET isolated also supports middleware registration, again by using a model similar to what exists in ASP.NET. This model gives you the ability to inject logic into the invocation pipeline, and before and after functions execute.

While the full middleware registration set of APIs is not yet exposed, we do support middleware registration and have added an example to the sample application under the Middleware folder.

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/FunctionApp/Program.cs" id="docsnippet_middleware" :::-->

## <a name="execution-context"></a>Ausführungskontext

Der isolierte .NET-Prozess übergibt ein `FunctionContext`-Objekt an Ihre Funktionsmethoden. Mit diesem Objekt können Sie durch Aufrufen der Methode `GetLogger` und Übergeben einer `categoryName`-Zeichenfolge eine Instanz von [`ILogger`](/dotnet/api/microsoft.extensions.logging.ilogger?view=dotnet-plat-ext-5.0&preserve-view=true) abrufen, in die Protokolle geschrieben werden sollen. Weitere Informationen finden Sie unter [Protokollierung](#logging). 

## <a name="bindings"></a>Bindungen 

Bindungen werden mithilfe von Attributen in Methoden, Parametern und Rückgabetypen definiert. Eine Funktionsmethode ist eine Methode, die eine `Function`-Schnittstelle und ein Triggerattribut enthält, das auf einen Eingabeparameter angewendet wird. Dies wird im folgenden Beispiel veranschaulicht:

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/SampleApp/Queue/QueueFunction.cs" id="docsnippet_queue_trigger" :::

Das Trigger-Attribut gibt den Triggertyp an und bindet die Eingabedaten an einen Methodenparameter. Die zuvor gezeigte Beispielfunktion wird durch eine Warteschlangennachricht ausgelöst, und die Warteschlangennachricht wird im Parameter `myQueueItem` an die Methode übergeben.

Das Attribut `Function` kennzeichnet die Methode als Funktionseinstiegspunkt. Der Name muss innerhalb eines Projekts eindeutig sein, mit einem Buchstaben beginnen und darf nur Buchstaben, Ziffern, `_` und `-` enthalten. Bis zu 127 Zeichen sind zulässig. Projektvorlagen erstellen oft eine Methode namens `Run`, aber der Name der Methode kann ein beliebiger gültiger C#-Methodennamen sein.

Da isolierte .NET-Projekte in einem separaten Workerprozess ausgeführt werden, können Bindungen keine umfangreichen Bindungsklassen wie `ICollector<T>`, `IAsyncCollector<T>` und `CloudBlockBlob` verwenden. Außerdem gibt es keine direkte Unterstützung für von zugrunde liegenden Dienst-SDKs abgeleitete Typen wie [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient) und [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). Bindungen basieren stattdessen auf Zeichenfolgen, Arrays und serialisierbaren Typen wie Plain Old CLR Objects (POCOs). 

Bei HTTP-Triggern müssen `HttpRequestData` und `HttpResponseData` verwendet werden, um auf die Anforderungs- und Antwortdaten zuzugreifen. Das liegt daran, dass Sie bei der prozessexternen Ausführung nicht auf die ursprüngliche HTTP-Anforderung und die Antwortobjekte zugreifen können. 

### <a name="input-bindings"></a>Eingabebindungen

Eine Funktion kann über null oder mehr Eingabebindungen verfügen, die Daten an eine andere Funktion übergeben können. Genau wie Trigger werden Eingabebindungen definiert, indem ein Bindungsattribut auf einen Eingabeparameter angewendet wird. Wenn die Funktion ausgeführt wird, versucht die Runtime, die in der Bindung angegebenen Daten abzurufen. Die angeforderten Daten sind häufig von den Informationen abhängig, die vom Triggern mithilfe von Bindungsparametern bereitgestellt werden.  

### <a name="output-bindings"></a>Ausgabebindungen

Wenn Sie eine Ausgabebindung schreiben möchten, müssen Sie ein Ausgabebindungsattribut an die Funktionsmethode anfügen, in der definiert ist, wie in den gebundenen Dienst geschrieben werden soll. Der von der Methode zurückgegebene Wert wird in die Ausgabebindung geschrieben. Im folgenden Beispiel wird ein Zeichenfolgenwert mithilfe einer Ausgabebindung in die Nachrichtenwarteschlange `functiontesting2` geschrieben:

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/SampleApp/Queue/QueueFunction.cs" id="docsnippet_queue_output_binding" :::

### <a name="multiple-output-bindings"></a>Mehrere Ausgabebindungen

Die Daten, die in eine Ausgabebindung geschrieben werden, sind immer der Rückgabewert der Funktion. Wenn Sie in mehr als eine Ausgabebindung schreiben müssen, müssen Sie einen benutzerdefinierten Rückgabetyp erstellen. Bei diesem Rückgabetyp muss das Ausgabebindungsattribut auf eine oder mehrere Eigenschaften der Klasse angewendet werden. Im folgenden Beispiel wird sowohl in eine HTTP-Antwort als auch in eine Ausgabebindung für eine Warteschlange geschrieben:

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/FunctionApp/Function1/Function1.cs" id="docsnippet_multiple_outputs":::

### <a name="http-trigger"></a>HTTP-Trigger

HTTP-Trigger übersetzen eingehende HTTP-Anforderungsnachrichten in ein `HttpRequestData`-Objekt, das an die Funktion übergeben wird. Dieses Objekt liefert die Anforderungsdaten wie `Headers`, `Cookies`, `Identities`, `URL` und optional `Body` (Nachrichtentext). Dieses Objekt ist eine Darstellung des HTTP-Anforderungsobjekts, nicht der Anforderung selbst. 

Entsprechend gibt die Funktion ein `HttpReponseData`-Objekt zurück, das die Daten zur Erstellung der HTTP-Antwort enthält, zum Beispiel `StatusCode` (Statuscode für die Nachricht), `Headers` und optional `Body` (Nachrichtentext).  

Der folgende Code stellt einen HTTP-Trigger dar: 

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/SampleApp/Http/HttpFunction.cs" id="docsnippet_http_trigger" :::

## <a name="logging"></a>Protokollierung

In einem isolierten .NET-Prozess können Sie in Protokolle schreiben, indem Sie eine Instanz von [`ILogger`](/dotnet/api/microsoft.extensions.logging.ilogger?view=dotnet-plat-ext-5.0&preserve-view=true) verwenden, die von einem an Ihre Funktion übergebenen `FunctionContext`-Objekt abgerufen wurde. Rufen Sie die Methode `GetLogger` auf, und übergeben Sie dabei einen Zeichenfolgenwert, der dem Namen der Kategorie entspricht, in die die Protokolle geschrieben werden. Die Kategorie ist normalerweise der Name der spezifischen Funktion, aus der die Protokolle geschrieben werden. Weitere Informationen zu Kategorien finden Sie im [Artikel zur Überwachung](functions-monitoring.md#log-levels-and-categories). 

Im folgenden Beispiel wird gezeigt, wie `ILogger` abgerufen wird und Protokolle in einer Funktion geschrieben werden:

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/SampleApp/Http/HttpFunction.cs" id="docsnippet_logging" ::: 

Verwenden Sie verschiedene Methoden von `ILogger`, um verschiedene Protokolliergrade wie `LogWarning` oder `LogError` zu schreiben. Weitere Informationen zu den Protokolliergraden finden Sie im [Artikel zur Überwachung](functions-monitoring.md#log-levels-and-categories).

Auch bei der [Dependency Injection](#dependency-injection) wird eine [`ILogger`](/dotnet/api/microsoft.extensions.logging.ilogger?view=dotnet-plat-ext-5.0&preserve-view=true)-Schnittstelle bereitgestellt.

## <a name="differences-with-net-class-library-functions"></a>Unterschiede zu Funktionen der .NET-Klassenbibliothek

In diesem Abschnitt werden die aktuellen Funktions- und Verhaltensunterschiede zwischen prozessextern ausgeführten .NET 5.0-Funktionen und prozessintern ausgeführten Funktionen der .NET-Klassenbibliothek erläutert:

| Feature/Verhalten |  Prozessintern (.NET Core 3.1) | Prozessextern (.NET 5.0) |
| ---- | ---- | ---- |
| .NET-Versionen | LTS (.NET Core 3.1) | Aktuell (.NET 5.0) |
| Erforderliche Pakete | [Microsoft.NET.Sdk.Functions](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions/) | [Microsoft.Azure.Functions.Worker](https://www.nuget.org/packages/Microsoft.Azure.Functions.Worker/)<br/>[Microsoft.Azure.Functions.Worker](https://www.nuget.org/packages/Microsoft.Azure.Functions.Worker.Sdk) | 
| Pakete für Bindungserweiterungen | [`Microsoft.Azure.WebJobs.Extensions.*`](https://www.nuget.org/packages?q=Microsoft.Azure.WebJobs.Extensions)  | Unter [`Microsoft.Azure.Functions.Worker.Extensions.*`](https://www.nuget.org/packages?q=Microsoft.Azure.Functions.Worker.Extensions) | 
| Protokollierung | An die Funktion übergebene Schnittstelle [`ILogger`](/dotnet/api/microsoft.extensions.logging.ilogger?view=dotnet-plat-ext-5.0&preserve-view=true) | Aus [ abgerufene Schnittstelle `ILogger`](/dotnet/api/microsoft.extensions.logging.ilogger?view=dotnet-plat-ext-5.0&preserve-view=true)`FunctionContext` |
| Abbruchtoken | [Unterstützt](functions-dotnet-class-library.md#cancellation-tokens) | Nicht unterstützt |
| Ausgabebindungen | Out-Parameter | Rückgabewerte |
| Ausgabebindungstypen |  `IAsyncCollector`, [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet&preserve-view=true), [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azure-dotnet&preserve-view=true) und andere clientspezifische Typen | Einfache Typen, serialisierbare JSON-Typen und Arrays |
| Mehrere Ausgabebindungen | Unterstützt | [Unterstützt](#multiple-output-bindings) |
| HTTP-Trigger | [`HttpRequest`](/dotnet/api/microsoft.aspnetcore.http.httprequest?view=aspnetcore-5.0&preserve-view=true)/[`ObjectResult`](/dotnet/api/microsoft.aspnetcore.mvc.objectresult?view=aspnetcore-5.0&preserve-view=true) | `HttpRequestData`/`HttpResponseData` |
| Langlebige Funktionen | [Unterstützt](durable/durable-functions-overview.md) | Nicht unterstützt | 
| Imperative Bindungen | [Unterstützt](functions-dotnet-class-library.md#binding-at-runtime) | Nicht unterstützt |
| function.json-Artefakt | Generiert | Nicht generiert |
| Konfiguration | [host.json](functions-host-json.md) | [host.json](functions-host-json.md) und [benutzerdefinierte Initialisierung](#configuration) |
| Dependency Injection | [Unterstützt](functions-dotnet-dependency-injection.md)  | [Unterstützt](#dependency-injection) |
| Middleware | Nicht unterstützt | Unterstützt |
| Kaltstartdauer | Typisch | Länger (aufgrund des Just-In-Time-Starts), Ausführung unter Linux anstelle von Windows, um potenzielle Verzögerungen zu vermeiden |
| ReadyToRun | [Unterstützt](functions-dotnet-class-library.md#readytorun) | _TBD_ |

## <a name="known-issues"></a>Bekannte Probleme

Auf [dieser Seite](https://aka.ms/AAbh18e) finden Sie Informationen zu Problemumgehungen bis hin zu bekannten Problemen bei der Ausführung von Funktionen in isolierten .NET-Prozessen. Wenn Sie ein Problem melden möchten, [erstellen Sie ein Issue in diesem GitHub-Repository](https://github.com/Azure/azure-functions-dotnet-worker/issues/new/choose).  

## <a name="next-steps"></a>Nächste Schritte

+ [Weitere Informationen zu Triggern und Bindungen](functions-triggers-bindings.md)
+ [Weitere Informationen zu bewährten Methoden für Azure Functions](functions-best-practices.md)
