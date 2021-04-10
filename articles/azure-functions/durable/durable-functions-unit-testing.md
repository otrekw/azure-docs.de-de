---
title: Azure Durable Functions-Unittest
description: Erfahren Sie, wie Sie den Durable Functions-Unittest durchführen.
ms.topic: conceptual
ms.date: 11/03/2019
ms.openlocfilehash: 89b6419e95b3971b0d272490e19354f300204e1e
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "103491043"
---
# <a name="durable-functions-unit-testing"></a>Durable Functions-Unittest

Der Unittest ist ein wichtiger Bestandteil moderner Softwareentwicklungsverfahren. Mit Unittests wird das Verhalten der Geschäftslogik überprüft und verhindert, dass in der Zukunft unbemerkte gravierende Änderungen eingeführt werden. Die Komplexität von Durable Functions kann problemlos wachsen, sodass die Einführung von Unittests dazu beiträgt, gravierende Änderungen zu vermeiden. In den folgenden Abschnitten wird erläutert, wie Unittests der drei Funktionstypen durchgeführt werden: Orchestrierungsclient-, Orchestrator- und Aktivitätsfunktionen.

> [!NOTE]
> Dieser Artikel enthält Anleitungen für Unittests für Durable Functions-Apps, die für Durable Functions 2.x vorgesehen sind. Weitere Informationen zu den Unterschieden zwischen den Versionen finden Sie im Artikel [Durable Functions-Versionen](durable-functions-versions.md).

## <a name="prerequisites"></a>Voraussetzungen

Die Beispiele in diesem Artikel setzen Kenntnisse der folgenden Konzepte und Frameworks voraus:

* Komponententest

* Langlebige Funktionen

* [xUnit](https://github.com/xunit/xunit) – Testframework

* [moq](https://github.com/moq/moq4) – Simulationsframework

## <a name="base-classes-for-mocking"></a>Basisklassen für Simulation

Die Simulation wird über die folgenden Schnittstellen unterstützt:

* [IDurableOrchestrationClient](/dotnet/api/microsoft.azure.webjobs.IDurableOrchestrationClient), [IDurableEntityClient](/dotnet/api/microsoft.azure.webjobs.IDurableEntityClient) und [IDurableClient](/dotnet/api/microsoft.azure.webjobs.IDurableClient)

* [IDurableOrchestrationContext](/dotnet/api/microsoft.azure.webjobs.IDurableOrchestrationContext)

* [IDurableActivityContext](/dotnet/api/microsoft.azure.webjobs.IDurableActivityContext)
  
* [IDurableEntityContext](/dotnet/api/microsoft.azure.webjobs.IDurableEntityContext)

Diese Schnittstellen können mit den verschiedenen von Durable Functions unterstützten Auslösern und Bindungen verwendet werden. Bei Ausführung Ihrer Azure-Funktionen führt die Funktionsruntime den Funktionscode mit einer konkreten Implementierung dieser Schnittstellen aus. Bei Unittests können Sie eine simulierte Version dieser Schnittstellen übergeben, um Ihre Geschäftslogik zu testen.

## <a name="unit-testing-trigger-functions"></a>Unittest-Triggerfunktionen

In diesem Abschnitt überprüft der Unittest die Logik der folgenden HTTP-Triggerfunktion zum Starten neuer Orchestrierungen.

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpStart.cs)]

Die Unittestaufgabe überprüft den Wert des in der Antwortnutzlast bereitgestellten `Retry-After`-Headers. Dazu simuliert der Unittest einige der `IDurableClient`-Methoden, um vorhersagbares Verhalten sicherzustellen.

Zunächst wird ein Simulationsframework (in diesem Fall [moq](https://github.com/moq/moq4)) verwendet, um `IDurableClient` zu simulieren:

```csharp
// Mock IDurableClient
var durableClientMock = new Mock<IDurableClient>();
```

> [!NOTE]
> Sie können zwar Schnittstellen implementieren, indem Sie die Schnittstelle direkt als Klasse implementieren, aber Simulationsframeworks vereinfachen den Prozess in verschiedener Hinsicht. Wenn beispielsweise der Schnittstelle über mehrere Nebenversionen hinweg eine neue Methode hinzugefügt wird, erfordert moq im Gegensatz zu konkreten Implementierungen keine Codeänderungen.

Dann wird die `StartNewAsync`-Methode simuliert, um eine gut bekannte Instanz-ID zurückzugeben.

```csharp
// Mock StartNewAsync method
durableClientMock.
    Setup(x => x.StartNewAsync(functionName, It.IsAny<object>())).
    ReturnsAsync(instanceId);
```

Als Nächstes wird `CreateCheckStatusResponse` simuliert, um immer eine leere HTTP 200-Antwort zurückzugeben.

```csharp
// Mock CreateCheckStatusResponse method
durableClientMock
    // Notice that even though the HttpStart function does not call IDurableClient.CreateCheckStatusResponse() 
    // with the optional parameter returnInternalServerErrorOnFailure, moq requires the method to be set up
    // with each of the optional parameters provided. Simply use It.IsAny<> for each optional parameter
    .Setup(x => x.CreateCheckStatusResponse(It.IsAny<HttpRequestMessage>(), instanceId, returnInternalServerErrorOnFailure: It.IsAny<bool>())
    .Returns(new HttpResponseMessage
    {
        StatusCode = HttpStatusCode.OK,
        Content = new StringContent(string.Empty),
        Headers =
        {
            RetryAfter = new RetryConditionHeaderValue(TimeSpan.FromSeconds(10))
        }
    });
```

`ILogger` wird auch simuliert:

```csharp
// Mock ILogger
var loggerMock = new Mock<ILogger>();
```  

Jetzt wird die `Run`-Methode vom Unittest aufgerufen:

```csharp
// Call Orchestration trigger function
var result = await HttpStart.Run(
    new HttpRequestMessage()
    {
        Content = new StringContent("{}", Encoding.UTF8, "application/json"),
        RequestUri = new Uri("http://localhost:7071/orchestrators/E1_HelloSequence"),
    },
    durableClientMock.Object,
    functionName,
    loggerMock.Object);
 ```

 Der letzte Schritt ist der Vergleich der Ausgabe mit dem erwarteten Wert:

```csharp
// Validate that output is not null
Assert.NotNull(result.Headers.RetryAfter);

// Validate output's Retry-After header value
Assert.Equal(TimeSpan.FromSeconds(10), result.Headers.RetryAfter.Delta);
```

Nach der Kombination aller Schritte hat der Unittest folgenden Code:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HttpStartTests.cs)]

## <a name="unit-testing-orchestrator-functions"></a>Unittest-Orchestratorfunktionen

Orchestratorfunktionen sind für Unittests noch interessanter, da sie in der Regel viel mehr Geschäftslogik aufweisen.

In diesem Abschnitt überprüfen die Unittests die Ausgabe der `E1_HelloSequence`-Orchestratorfunktion:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

Der Unittestcode beginnt mit dem Erstellen einer Simulation:

```csharp
var durableOrchestrationContextMock = new Mock<IDurableOrchestrationContext>();
```

Dann werden die Aktivitätsmethodenaufrufe simuliert:

```csharp
durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "Tokyo")).ReturnsAsync("Hello Tokyo!");
durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "Seattle")).ReturnsAsync("Hello Seattle!");
durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "London")).ReturnsAsync("Hello London!");
```

Als Nächstes ruft der Unittest die `HelloSequence.Run`-Methode auf:

```csharp
var result = await HelloSequence.Run(durableOrchestrationContextMock.Object);
```

Schließlich wird die Ausgabe überprüft:

```csharp
Assert.Equal(3, result.Count);
Assert.Equal("Hello Tokyo!", result[0]);
Assert.Equal("Hello Seattle!", result[1]);
Assert.Equal("Hello London!", result[2]);
```

Nach der Kombination aller Schritte hat der Unittest folgenden Code:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HelloSequenceOrchestratorTests.cs)]

## <a name="unit-testing-activity-functions"></a>Unittest-Aktivitätsfunktionen

Der Unittest von Aktivitätsfunktionen kann in gleicher Weise wie der nicht dauerhafter Funktionen durchgeführt werden.

In diesem Abschnitt überprüft der Unittest das Verhalten der `E1_SayHello`-Aktivitätsfunktion:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

Die Komponententests überprüfen auch das Format der Ausgabe. Die Unittests können die Parametertypen direkt oder über die Pseudoklasse `IDurableActivityContext` verwenden:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HelloSequenceActivityTests.cs)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Weitere Informationen zu xUnit](https://xunit.net/docs/getting-started/netcore/cmdline)
> 
> [Weitere Informationen zu moq](https://github.com/Moq/moq4/wiki/Quickstart)
