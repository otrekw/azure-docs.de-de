---
title: Datenpersistenz und -serialisierung in Durable Functions – Azure
description: Erfahren Sie, wie Daten in der Durable Functions-Erweiterung für Azure Functions aufbewahrt werden.
author: ConnorMcMahon
ms.topic: conceptual
ms.date: 02/11/2021
ms.author: azfuncdf
ms.openlocfilehash: ea4aaa1cdbe10e2db9cf619452558d104a2293ab
ms.sourcegitcommit: f6193c2c6ce3b4db379c3f474fdbb40c6585553b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102449372"
---
# <a name="data-persistence-and-serialization-in-durable-functions-azure-functions"></a>Datenpersistenz und -serialisierung in Durable Functions (Azure Functions)

Durable Functions behält Funktionsparameter, Rückgabewerte und andere Zustände automatisch in einem dauerhaften Back-End-Speicher bei, um eine zuverlässige Ausführung sicherzustellen. Umfang und Häufigkeit der im dauerhaftem Speicher aufbewahrten Daten können sich jedoch auf die Leistung der Anwendung und die Kosten für Speichertransaktion auswirken. Abhängig von dem Datentyp, der von Ihrer Anwendung gespeichert wird, müssen auch die Richtlinien für die Datenaufbewahrung und den Datenschutz berücksichtigt werden.

## <a name="azure-storage"></a>Azure Storage

Standardmäßig speichert Durable Functions Daten in Warteschlangen, Tabellen und Blobs in einem angegebenen [Azure Storage](https://azure.microsoft.com/services/storage/) Konto.

### <a name="queues"></a>Warteschlangen

Durable Functions verwendet Azure Storage-Warteschlangen, um alle Funktionsausführungen zuverlässig zu planen. Diese Warteschlangennachrichten enthalten Funktionseingaben oder -ausgaben. Dies ist davon abhängig, ob mit der Nachricht eine Ausführung geplant oder ein Wert an eine aufrufende Funktion zurückgegeben werden soll. Außerdem enthalten diese Warteschlangennachrichten zusätzliche Metadaten, die Durable Functions für interne Zwecke verwendet (z. B. Routing und End-to-End-Korrelation). Nachdem die Ausführung einer Funktion als Antwort auf eine empfangene Nachricht abgeschlossen wurde, wird diese Nachricht gelöscht. Auch das Ergebnis der Ausführung kann dann in Azure Storage-Tabellen oder Azure Storage-Blobs persistent gespeichert werden.

In einem einzelnen [Aufgabenhub](durable-functions-task-hubs.md) erstellt Durable Functions Nachrichten und fügt diese Warteschlangen hinzu: der Warteschlange `<taskhub>-workitem` mit *Arbeitselementen* sowie *Steuerungswarteschlangen* mit der Bezeichnung `<taskhub>-control-##` für die Planung oder Fortsetzung von Orchestrator- und Entitätsfunktionen. Die Anzahl der Steuerungswarteschlangen ist gleich der Anzahl der für Ihre Anwendung konfigurierten Partitionen. Weitere Informationen zu Warteschlangen und Partitionen finden Sie in der [Dokumentation zur Leistung und Skalierbarkeit](durable-functions-perf-and-scale.md).

### <a name="tables"></a>Tabellen

Nachdem die Nachrichten des Orchestrierungsprozesses erfolgreich verarbeitet wurden, werden Datensätze der resultierenden Aktionen in der *Verlaufstabelle* mit dem Namen `<taskhub>History` persistent gespeichert. Die Ein- und Ausgaben der Orchestrierung sowie benutzerdefinierte Statusdaten werden ebenfalls persistent in der *Instanzentabelle* mit dem Namen `<taskhub>Instances` gespeichert.

### <a name="blobs"></a>BLOBs

In den meisten Fällen nutzt Durable Functions keine Azure Storage-Blobs für das Speichern der Daten. Allerdings gelten für Warteschlangen und Tabellen [Größenbeschränkungen](../../azure-resource-manager/management/azure-subscription-service-limits.md#azure-queue-storage-limits), die verhindern können, dass Durable Functions alle erforderlichen Daten dauerhaft in einer Speicherzeile oder Warteschlangennachricht speichern kann. Wenn beispielsweise ein Datenelement, das in einer Warteschlange gespeichert werden soll, nach der Serialisierung größer als 45 KB ist, werden die Daten von Durable Functions komprimiert und stattdessen in einem Blob gespeichert. Wenn Daten auf diese Weise in Blob Storage gespeichert werden, speichert Durable Functions einen Verweis auf dieses Blob in der Tabellenzeile bzw. der Warteschlangennachricht. Wenn Durable Functions die Daten abrufen muss, erfolgt dies automatisch aus dem Blob. Diese Blobs werden im Blobcontainer `<taskhub>-largemessages` gespeichert.

> [!NOTE]
> Die zusätzlichen Schritte für die Komprimierung und Speicherung im Blob bei großen Nachrichten können hohe Kosten in Bezug auf CPU und E/A-Latenz verursachen. Darüber hinaus muss Durable Functions persistente Daten in den Arbeitsspeicher laden, und zwar möglicherweise für viele verschiedene Funktionsausführungen gleichzeitig. Folglich kann das Beibehalten umfangreicher Nutzdaten auch zu einer hohen Speicherauslastung führen. Um den Arbeitsspeicherverbrauch zu minimieren, sollten Sie umfangreiche Nutzdaten manuell speichern (z. B. in Blob Storage) und stattdessen Verweise auf diese Daten übergeben. Auf diese Weise kann Ihr Code die Daten erst bei Bedarf laden und so redundante Lasten während der [Wiedergabe von Orchestratorfunktionen](durable-functions-orchestrations.md#reliability) vermeiden. Das Speichern von Nutzdaten auf einem Datenträger wird jedoch *nicht* empfohlen, da die Verfügbarkeit des Datenträgers nicht garantiert ist. Dies liegt daran, dass Funktionen während ihrer Lebensdauer auf unterschiedlichen VMs ausgeführt werden können.

### <a name="types-of-data-that-is-serialized-and-persisted"></a>Typen von Daten, die serialisiert und persistent gespeichert werden
Im Folgenden finden Sie eine Liste der unterschiedlichen Typen von Daten, die serialisiert und persistent gespeichert werden, wenn Sie Features von Durable Functions nutzen:

- Alle Ein- und Ausgaben von Orchestrator-, Aktivitäts- und Entitätsfunktionen, einschließlich IDs und Ausnahmefehler
- Namen der Orchestrator-, Aktivitäts- und Entitätsfunktionen
- Namen und Nutzdaten externer Ereignisse
- Statusnutzdaten der benutzerdefinierten Orchestrierung
- Nachrichten zur Beendigung der Orchestrierung
- Dauerhafte Timernutzdaten
- Dauerhafte HTTP-Anforderungen und Antwort-URLs, Header und Nutzdaten
- Nutzdaten zu Entitätsaufrufen und Signalen
- Nutzdaten zum Entitätszustand

### <a name="working-with-sensitive-data"></a>Arbeiten mit vertraulichen Daten
Wenn Sie Azure Storage verwenden, werden alle Daten automatisch im Ruhezustand verschlüsselt. Jeder Benutzer, der Zugriff auf das Speicherkonto hat, kann die Daten jedoch in unverschlüsselter Form lesen. Wenn Sie vertrauliche Daten stärker schützen möchten, sollten Sie in Erwägung ziehen, die Daten zunächst mit Ihren eigenen Verschlüsselungsschlüsseln zu verschlüsseln, damit Durable Functions die Daten in einer vorverschlüsselten Form speichert.

Alternativ können .NET-Benutzer benutzerdefinierte Serialisierungsanbieter implementieren, die eine automatische Verschlüsselung bereitstellen. Ein Beispiel für die benutzerdefinierte Serialisierung mit Verschlüsselung finden Sie in [diesem GitHub-Beispiel](https://github.com/charleszipp/azure-durable-entities-encryption).

> [!NOTE]
> Wenn Sie sich für die Implementierung der Verschlüsselung auf Anwendungsebene entscheiden, beachten Sie, dass Orchestrierungen und Entitäten für eine unbegrenzte Zeit vorhanden sein können. Dies ist wichtig, wenn Sie Ihre Verschlüsselungsschlüssel rotieren müssen, da eine Orchestrierung oder Entitäten länger ausgeführt werden können, als Ihre Richtlinie für die Schlüsselrotation gilt. Bei einer Schlüsselrotation ist der Schlüssel zum Verschlüsseln der Daten möglicherweise nicht mehr für die Entschlüsselung verfügbar, wenn die Orchestrierung oder Entität das nächste Mal ausgeführt wird. Daher wird die kundenseitige Verschlüsselung nur empfohlen, wenn davon ausgegangen werden kann, dass Orchestrierungen und Entitäten nur für relativ kurze Zeitabschnitte ausgeführt werden.

## <a name="customizing-serialization-and-deserialization"></a>Anpassen von Serialisierung und Deserialisierung

# <a name="c"></a>[C#](#tab/csharp)

### <a name="default-serialization-logic"></a>Standardserialisierungslogik

Durable Functions verwendet [Json.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) zum Serialisieren von Orchestrierungs- und Entitätsdaten in JSON. Durable Functions verwendet die folgenden Standardeinstellungen für Json.NET:

**Eingaben, Ausgaben und Zustand:**

```csharp
JsonSerializerSettings
{
    TypeNameHandling = TypeNameHandling.None,
    DateParseHandling = DateParseHandling.None,
}
```

**Ausnahmen:**

```csharp
JsonSerializerSettings
{
    ContractResolver = new ExceptionResolver(),
    TypeNameHandling = TypeNameHandling.Objects,
    ReferenceLoopHandling = ReferenceLoopHandling.Ignore,
}
```

Eine ausführlichere Dokumentation zu `JsonSerializerSettings` finden Sie [hier](https://www.newtonsoft.com/json/help/html/SerializationSettings.htm).

## <a name="customizing-serialization-with-net-attributes"></a>Anpassen der Serialisierung mit .NET-Attributen

Beim Serialisieren von Daten sucht Json.NET nach [verschiedenen Attributen](https://www.newtonsoft.com/json/help/html/SerializationAttributes.htm) in Klassen und Eigenschaften, mit denen gesteuert wird, wie die Daten aus JSON serialisiert und deserialisiert werden. Wenn Sie den Quellcode für den an die Durable Functions-APIs übergebenen Datentyp besitzen, sollten Sie dem Typ diese Attribute hinzufügen, um Serialisierung und Deserialisierung anzupassen.

## <a name="customizing-serialization-with-dependency-injection"></a>Anpassen der Serialisierung per Abhängigkeitsinjektion

Bei Funktions-Apps, die auf .NET abzielen und in der Functions V3-Runtime ausgeführt werden, können Sie per [Abhängigkeitsinjektion](../functions-dotnet-dependency-injection.md) anpassen, wie Daten und Ausnahmen serialisiert werden. Der folgende Beispielcode veranschaulicht die Verwendung der Abhängigkeitsinjektion zum Überschreiben der Standardeinstellungen von Json.NET für die Serialisierung mithilfe von benutzerdefinierten Implementierungen der Dienstschnittstellen `IMessageSerializerSettingsFactory` und `IErrorSerializerSettingsFactory`.

```csharp
using Microsoft.Azure.Functions.Extensions.DependencyInjection;
using Microsoft.Azure.WebJobs.Extensions.DurableTask;
using Microsoft.Extensions.DependencyInjection;
using Newtonsoft.Json;
using System.Collections.Generic;

[assembly: FunctionsStartup(typeof(MyApplication.Startup))]
namespace MyApplication
{
    public class Startup : FunctionsStartup
    {
        public override void Configure(IFunctionsHostBuilder builder)
        {
            builder.Services.AddSingleton<IMessageSerializerSettingsFactory, CustomMessageSerializerSettingsFactory>();
            builder.Services.AddSingleton<IErrorSerializerSettingsFactory, CustomErrorSerializerSettingsFactory>();
        }

        /// <summary>
        /// A factory that provides the serialization for all inputs and outputs for activities and
        /// orchestrations, as well as entity state.
        /// </summary>
        internal class CustomMessageSerializerSettingsFactory : IMessageSerializerSettingsFactory
        {
            public JsonSerializerSettings CreateJsonSerializerSettings()
            {
                // Return your custom JsonSerializerSettings here
            }
        }

        /// <summary>
        /// A factory that provides the serialization for all exceptions thrown by activities
        /// and orchestrations
        /// </summary>
        internal class CustomErrorSerializerSettingsFactory : IErrorSerializerSettingsFactory
        {
            public JsonSerializerSettings CreateJsonSerializerSettings()
            {
                // Return your custom JsonSerializerSettings here
            }
        }
    }
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

### <a name="serialization-and-deserialization-logic"></a>Serialisierungs- und Deserialisierungslogik

Azure Functions-Knotenanwendungen nutzen [`JSON.stringify()` für die Serialisierung](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) und [`JSON.Parse()` für die Deserialisierung](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse). Die meisten Typen sollten nahtlos serialisiert und deserialisiert werden. Falls die Standardlogik unzureichend ist, können Sie die Serialisierungslogik durch Definieren einer `toJSON()`-Methode im Objekt überschreiben. Für die Objektdeserialisierung steht jedoch keine solche Umgehung zur Verfügung.

Für eine vollständige Anpassung der Serialisierungs-/Deserialisierungspipeline sollten Sie die Serialisierung und Deserialisierung mit Ihrem eigenen Code verarbeiten und die Daten dabei als Zeichenfolgen übergeben.


# <a name="python"></a>[Python](#tab/python)

### <a name="serialization-and-deserialization-logic"></a>Serialisierungs- und Deserialisierungslogik

Es wird dringend empfohlen, Typanmerkungen zu verwenden, um sicherzustellen, dass Durable Functions Ihre Daten ordnungsgemäß serialisiert und deserialisiert. Obwohl viele integrierte Typen automatisch verarbeitet werden, benötigen einige integrierte Datentypen Typanmerkungen, um den Typ während der Deserialisierung beizubehalten.

Bei benutzerdefinierten Datentypen müssen Sie die JSON-Serialisierung und -Deserialisierung eines Datentyps definieren, indem Sie statische Methoden `to_json` und `from_json` aus Ihrer Klasse exportieren.

---
