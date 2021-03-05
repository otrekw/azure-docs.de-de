---
title: Azure Functions-Trigger für die Aufwärmphase
description: Erfahren Sie, wie der Trigger für die Aufwärmphase in Azure Functions verwendet wird.
documentationcenter: na
author: craigshoemaker
manager: gwallace
keywords: azure functions, functions, ereignisverarbeitung, aufwärmphase, kaltstart, premium, dynamisches compute, serverlose architektur
ms.service: azure-functions
ms.topic: reference
ms.custom: devx-track-csharp
ms.date: 11/08/2019
ms.author: cshoe
ms.openlocfilehash: ea418576ab8fe06964a61e48f16393e1a0566ce8
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102182246"
---
# <a name="azure-functions-warm-up-trigger"></a>Azure Functions-Trigger für die Aufwärmphase

Dieser Artikel erläutert das Arbeiten mit dem Trigger für die Aufwärmphase in Azure Functions. Wenn eine Instanz hinzugefügt wird, um eine Funktions-App zu skalieren, wird ein Trigger für die Aufwärmphase aufgerufen. Sie können einen Trigger für die Aufwärmphase verwenden, um während dem [vorab aufgewärmten Prozess](./functions-premium-plan.md#pre-warmed-instances) benutzerdefinierte Abhängigkeiten vorab zu laden, damit Ihre Funktionen sofort mit der Verarbeitung von Anforderungen beginnen können. 

> [!NOTE]
> Der Trigger für die Aufwärmphase wird nur für Funktions-Apps unterstützt, die in einem Verbrauchsplan ausgeführt werden.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x-and-higher"></a>Pakete: Functions 2.x oder höher

Das NuGet-Paket [Microsoft.Azure.WebJobs.Extensions](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions) der Version **3.0.5 oder höher** ist erforderlich. Den Quellcode für das Paket finden Sie im GitHub-Repository [azure-webjobs-sdk-extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/tree/main/src/WebJobs.Extensions/Extensions/Warmup). 

[!INCLUDE [functions-package](../../includes/functions-package-auto.md)]

## <a name="trigger"></a>Trigger

Mithilfe des Triggers für die Aufwärmphase können Sie eine Funktion definieren, die auf einer neuen Instanz ausgeführt wird, wenn sie Ihrer laufenden App hinzugefügt wird. Sie können eine Aufwärmfunktion verwenden, um Verbindungen zu öffnen, Abhängigkeiten zu laden oder eine andere benutzerdefinierte Logik auszuführen, bevor die App Datenverkehr empfängt. 

Der Trigger für die Aufwärmphase dient zum Erstellen von gemeinsamen Abhängigkeiten, die von den anderen Funktionen Ihrer App verwendet werden. [Beispiele zu gemeinsamen Abhängigkeiten finden Sie hier.](./manage-connections.md#client-code-examples)

Beachten Sie, dass der Trigger für die Aufwärmphase nur während der horizontalen Skalierung aufgerufen wird und nicht bei Neustarts oder anderen nicht skalierbaren Starts. Sie müssen sicherstellen, dass Ihre Logik alle notwendigen Abhängigkeiten laden kann, ohne den Trigger für die Aufwärmphase zu verwenden. Lazy Loading ist ein gutes Muster, um dies zu erreichen.

## <a name="trigger---example"></a>Trigger: Beispiel

# <a name="c"></a>[C#](#tab/csharp)

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die auf jeder neuen Instanz ausgeführt wird, wenn diese Ihrer App hinzugefügt wird. Ein Attribut des Rückgabewerts ist nicht erforderlich.


* Ihre Funktion muss ```warmup``` (ohne Berücksichtigung der Groß-/Kleinschreibung) heißen, und pro App kann nur eine Aufwärmfunktion vorhanden sein.
* Wenn Sie die Aufwärmphase als eine .NET-Klassenbibliotheksfunktion verwenden, stellen Sie sicher, dass Sie über einen Paketverweis zu **Microsoft.Azure.WebJobs.Extensions >= 3.0.5** verfügen.
    * ```<PackageReference Include="Microsoft.Azure.WebJobs.Extensions" Version="3.0.5" />```


Platzhalterkommentare zeigen an, wo in der Anwendung gemeinsame Abhängigkeiten deklariert und initialisiert werden. 
[Weitere Informationen zu gemeinsamen Abhängigkeiten finden Sie hier.](./manage-connections.md#client-code-examples)

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Extensions.Logging;
 
namespace WarmupSample
{

    //Declare shared dependencies here

    public static class Warmup
    {
        [FunctionName("Warmup")]
        public static void Run([WarmupTrigger()] WarmupContext context,
            ILogger log)
        {
            //Initialize shared dependencies here
            
            log.LogInformation("Function App instance is warm 🌞🌞🌞");
        }
    }
}
```
# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)


Das folgende Beispiel zeigt einen Trigger für die Aufwärmphase in einer *function.json*-Datei und eine [C#-Skriptfunktion](functions-reference-csharp.md), die auf jeder neuen Instanz ausgeführt wird, wenn diese Ihrer App hinzugefügt wird.

Ihre Funktion muss ```warmup``` (ohne Berücksichtigung der Groß-/Kleinschreibung) heißen, und pro App kann nur eine Aufwärmfunktion vorhanden sein.

Die Datei *function.json* sieht wie folgt aus:

```json
{
    "bindings": [
        {
            "type": "warmupTrigger",
            "direction": "in",
            "name": "warmupContext"
        }
    ]
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#trigger---configuration).

```cs
public static void Run(WarmupContext warmupContext, ILogger log)
{
    log.LogInformation("Function App instance is warm 🌞🌞🌞");  
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Das folgende Beispiel zeigt einen Trigger für die Aufwärmphase in einer *function.json*-Datei und eine [JavaScript-Funktion](functions-reference-node.md), die auf jeder neuen Instanz ausgeführt wird, wenn diese Ihrer App hinzugefügt wird.

Ihre Funktion muss ```warmup``` (ohne Berücksichtigung der Groß-/Kleinschreibung) heißen, und pro App kann nur eine Aufwärmfunktion vorhanden sein.

Die Datei *function.json* sieht wie folgt aus:

```json
{
    "bindings": [
        {
            "type": "warmupTrigger",
            "direction": "in",
            "name": "warmupContext"
        }
    ]
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#trigger---configuration).

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = async function (context, warmupContext) {
    context.log('Function App instance is warm 🌞🌞🌞');
};
```

# <a name="python"></a>[Python](#tab/python)

Das folgende Beispiel zeigt einen Trigger für die Aufwärmphase in einer *function.json*-Datei und eine [Python-Funktion](functions-reference-python.md), die auf jeder neuen Instanz ausgeführt wird, wenn diese Ihrer App hinzugefügt wird.

Ihre Funktion muss ```warmup``` (ohne Berücksichtigung der Groß-/Kleinschreibung) heißen, und pro App kann nur eine Aufwärmfunktion vorhanden sein.

Die Datei *function.json* sieht wie folgt aus:

```json
{
    "bindings": [
        {
            "type": "warmupTrigger",
            "direction": "in",
            "name": "warmupContext"
        }
    ]
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#trigger---configuration).

Dies ist der Python-Code:

```python
import logging
import azure.functions as func


def main(warmupContext: func.Context) -> None:
    logging.info('Function App instance is warm 🌞🌞🌞')
```

# <a name="java"></a>[Java](#tab/java)

Das folgende Beispiel zeigt einen Aufwärmtrigger, der immer ausgeführt wird, wenn Ihrer App eine neue Instanz hinzugefügt wird.

Ihre Funktion muss `warmup` (ohne Berücksichtigung der Groß-/Kleinschreibung) heißen, und pro App kann nur eine Aufwärmfunktion vorhanden sein.

```java
@FunctionName("Warmup")
public void run( ExecutionContext context) {
       context.getLogger().info("Function App instance is warm 🌞🌞🌞");
}
```

---

## <a name="trigger---attributes"></a>Trigger: Attribute

In [C#-Klassenbibliotheken](functions-dotnet-class-library.md) ist das `WarmupTrigger`-Attribut für die Konfiguration der Funktion verfügbar.

# <a name="c"></a>[C#](#tab/csharp)

In diesem Beispiel wird veranschaulicht, wie das [Aufwärmphase](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions/Extensions/Warmup/Trigger/WarmupTriggerAttribute.cs)-Attribut verwendet wird.

Beachten Sie, dass Ihre Funktion ```Warmup``` heißen muss, und pro App kann nur eine Aufwärmfunktion vorhanden sein.

```csharp
 [FunctionName("Warmup")]
        public static void Run(
            [WarmupTrigger()] WarmupContext context, ILogger log)
        {
            ...
        }
```

Ein vollständiges Beispiel finden Sie unter [Triggerbeispiel](#trigger---example).

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Attribute werden von C#-Skript nicht unterstützt.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Attribute werden von JavaScript nicht unterstützt.

# <a name="python"></a>[Python](#tab/python)

Attribute werden von Python nicht unterstützt.

# <a name="java"></a>[Java](#tab/java)

Der Trigger für die Aufwärmphase wird in Java nicht als Attribut unterstützt.

---

## <a name="trigger---configuration"></a>Trigger: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `WarmupTrigger` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
| **type** | –| Erforderlich – muss auf `warmupTrigger` festgelegt sein. |
| **direction** | –| Erforderlich – muss auf `in` festgelegt sein. |
| **name** | –| Erforderlich – der Variablenname, der im Funktionscode verwendet wird|

## <a name="trigger---usage"></a>Trigger: Verwendung

Wenn eine Aufwärmfunktion aufgerufen wird, werden keine weiteren Informationen bereitgestellt.

## <a name="trigger---limits"></a>Trigger – Grenzwerte

* Der Trigger für die Aufwärmphase ist nur für Apps verfügbar, die im [Premium-Plan](./functions-premium-plan.md) ausgeführt werden.
* Der Trigger für die Aufwärmphase wird nur während des horizontalen Skalierens aufgerufen und nicht bei Neustarts oder anderen nicht skalierbaren Startups. Sie müssen sicherstellen, dass Ihre Logik alle notwendigen Abhängigkeiten laden kann, ohne den Trigger für die Aufwärmphase zu verwenden. Lazy Loading ist ein gutes Muster, um dies zu erreichen.
* Der Trigger für die Aufwärmphase kann nicht aufgerufen werden, sobald eine Instanz bereits ausgeführt wird.
* Pro Funktions-App kann nur eine Triggerfunktion für die Aufwärmphase vorhanden sein.

## <a name="next-steps"></a>Nächste Schritte

[Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md)
