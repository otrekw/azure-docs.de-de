---
title: Verwalten digitaler Zwillinge
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie einzelne Zwillinge und Beziehungen abrufen, aktualisieren und löschen.
author: baanders
ms.author: baanders
ms.date: 4/10/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 9f140594ef18df7f9a6a3b919998962c966cde76
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2020
ms.locfileid: "88587598"
---
# <a name="manage-digital-twins"></a>Verwalten digitaler Zwillinge

Entitäten in Ihrer Umgebung werden durch [digital Zwillinge](concepts-twins-graph.md) dargestellt. Die Verwaltung digitaler Zwillinge kann das Erstellen, Ändern und Löschen umfassen. Für diese Vorgänge können Sie die [**Digital Twins-APIs**](how-to-use-apis-sdks.md), das [.NET (C#) SDK](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core) oder die [Azure Digital Twins-Befehlszeilenschnittstelle](how-to-use-cli.md) verwenden.

In diesem Artikel wird die Verwaltung digitaler Zwillinge beschrieben. Informationen zum Arbeiten mit Beziehungen und dem [Zwillingsgraph](concepts-twins-graph.md) insgesamt finden Sie unter [*Gewusst wie: Verwalten des Zwillingsgraphen mit Beziehungen*](how-to-manage-graph.md).

> [!TIP]
> Alle SDK-Funktionen sind in synchronen und asynchronen Versionen enthalten.

## <a name="create-a-digital-twin"></a>Erstellen eines digitalen Zwillings

Wenn Sie einen Zwilling erstellen möchten, verwenden Sie die `CreateDigitalTwin`-Methode auf dem Serviceclient wie folgt:

```csharp
await client.CreateDigitalTwinAsync("myNewTwinID", initData);
```

Zum Erstellen eines digitalen Zwillings müssen Sie Folgendes angeben:
* Die gewünschte ID für den digitalen Zwilling
* Das zu verwendende [Modell](concepts-models.md) 

Optional können Sie Anfangswerte für alle Eigenschaften des digitalen Zwillings bereitstellen. 

Die Modell- und ursprünglichen Eigenschaftswerte werden über den `initData`-Parameter bereitgestellt. Dabei handelt es sich um eine JSON-Zeichenfolge, die die relevanten Daten enthält. Weitere Informationen zum Strukturieren dieses Objekts finden Sie im nächsten Abschnitt.

> [!TIP]
> Nachdem Sie einen Zwilling erstellt oder aktualisiert haben, kann es bis zu 10 Sekunden dauern, bis sich die Änderungen in [Abfragen](how-to-query-graph.md) widerspiegeln. Bei der `GetDigitalTwin`-API (siehe [weiter unten in diesem Artikel](#get-data-for-a-digital-twin)) tritt diese Verzögerung nicht auf. Verwenden Sie daher den API-Befehl anstelle von Abfragen, um die neu erstellten Zwillinge anzuzeigen, wenn Sie eine sofortige Antwort benötigen. 

### <a name="initialize-model-and-properties"></a>Initialisieren des Modells und der Eigenschaften

Die Zwillingserstellungs-API akzeptiert ein Objekt, das in eine gültige JSON-Beschreibung der Zwillingseigenschaften serialisiert wird. Unter [*Konzepte: digitale Zwillinge und das Zwillingsdiagramm*](concepts-twins-graph.md) finden Sie eine Beschreibung des JSON-Formats für einen Zwilling. 

Daher erstellen Sie zunächst ein Datenobjekt, das den Zwilling und seine Eigenschaftsdaten darstellt. Anschließend können Sie mit `JsonSerializer` eine serialisierte Version davon an den API-Aufruf für den `initdata`-Parameter übergeben.

Sie können ein Parameterobjekt entweder manuell oder mithilfe einer bereitgestellten Hilfsklasse erstellen. Im Folgenden finden Sie Beispiele für beides.

#### <a name="create-twins-using-manually-created-data"></a>Erstellen von Zwillingen mithilfe manuell erstellter Daten

Sie können die Eigenschaften eines Zwillings ohne benutzerdefinierte Hilfsklassen in `Dictionary<string, object>` darstellen, wobei `string` der Name der Eigenschaft ist und `object` ein Objekt, das die Eigenschaft und ihren Wert darstellt.

```csharp
// Define the model type for the twin to be created
Dictionary<string, object> meta = new Dictionary<string, object>()
{
    { "$model", "dtmi:com:contoso:Room;1" }
};
// Initialize the twin properties
Dictionary<string, object> twin = new Dictionary<string, object>()
{
    { "$metadata", meta },
    { "Temperature", temperature},
    { "Humidity", humidity},
};
client.CreateDigitalTwin("myNewRoomID", JsonSerializer.Serialize<Dictionary<string, object>>(twin));
```

#### <a name="create-twins-with-the-helper-class"></a>Erstellen von Zwillingen mithilfe der Hilfsklasse

Die Hilfsklasse `BasicDigitalTwin` ermöglicht es Ihnen, Eigenschaftsfelder in einem Zwillingsobjekt direkter zu speichern. Sie sollten dennoch die Liste der Eigenschaften mit `Dictionary<string, object>` erstellen, was dann dem Zwillingsobjekt direkt als `CustomProperties` hinzugefügt werden kann.

```csharp
BasicDigitalTwin twin = new BasicDigitalTwin();
twin.Metadata = new DigitalTwinMetadata();
twin.Metadata.ModelId = "dtmi:example:Room;1";
// Initialize properties
Dictionary<string, object> props = new Dictionary<string, object>();
props.Add("Temperature", 25.0);
props.Add("Humidity", 50.0);
twin.CustomProperties = props;

client.CreateDigitalTwin("myNewRoomID", JsonSerializer.Serialize<BasicDigitalTwin>(twin));
```

>[!NOTE]
> `BasicDigitalTwin`-Objekte verfügen über ein `Id`-Feld. Sie können dieses Feld leer lassen. Wenn Sie einen ID-Wert hinzufügen, muss dieser jedoch dem an den `CreateDigitalTwin`-Aufruf übergebenen ID-Parameter entsprechen. Für das obige Beispiel sähe dies so aus:
>
>```csharp
>twin.Id = "myNewRoomID";
>```

## <a name="get-data-for-a-digital-twin"></a>Abrufen von Daten für einen digitalen Zwilling

Sie können auf die vollständigen Daten eines beliebigen digitalen Zwillings zugreifen, indem Sie Folgendes aufrufen:

```csharp
object result = await client.GetDigitalTwin(id);
```

Dieser Aufruf gibt Zwillingsdaten als JSON-Zeichenfolge zurück. 

> [!TIP]
> Beim Abrufen eines Zwillings mit `GetDigitalTwin` werden nur Eigenschaften zurückgegeben, die mindestens einmal festgelegt wurden.

Informationen zum Abrufen mehrerer Zwillinge mithilfe eines einzelnen API-Aufrufs finden Sie in den Beispielen zur Abfrage-API unter [*Gewusst wie: des Zwillingsdiagramms von Azure Digital Twins*](how-to-query-graph.md).

Beachten Sie das folgende Modell (geschrieben in [Digital Twins Definition Language (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl/tree/master/DTDL)), das einen *Moon* definiert:

```json
{
    "@id": " dtmi:com:contoso:Moon;1",
    "@type": "Interface",
    "@context": "dtmi:dtdl:context;2",
    "contents": [
        {
            "@type": "Property",
            "name": "radius",
            "schema": "double",
            "writable": true
        },
        {
            "@type": "Property",
            "name": "mass",
            "schema": "double",
            "writable": true
        }
    ]
}
```

Das Ergebnis des Aufrufs von `object result = await client.DigitalTwins.GetByIdAsync("my-moon");` für einen Zwilling vom Typ *Moon* könnte wie folgt aussehen:

```json
{
  "$dtId": "myMoon-001",
  "$etag": "W/\"e59ce8f5-03c0-4356-aea9-249ecbdc07f9\"",
  "radius": 1737.1,
  "mass": 0.0734,
  "$metadata": {
    "$model": "dtmi:com:contoso:Moon;1",
    "radius": {
      "desiredValue": 1737.1,
      "desiredVersion": 5,
      "ackVersion": 4,
      "ackCode": 200,
      "ackDescription": "OK"
    },
    "mass": {
      "desiredValue": 0.0734,
      "desiredVersion": 8,
      "ackVersion": 8,
      "ackCode": 200,
      "ackDescription": "OK"
    }
  }
}
```

Die definierten Eigenschaften des digitalen Zwillings werden als Eigenschaften der obersten Ebene für den digitalen Zwilling zurückgegeben. Metadaten oder Systeminformationen, die nicht Teil der DTDL-Definition sind, werden mit einem `$`-Präfix zurückgegeben. Metadateneigenschaften umfassen Folgendes:
* Die ID des digitalen Zwillings in dieser Instanz von Azure Digital Twins, wie etwa `$dtId`
* `$etag`, ein vom Webserver zugewiesenes Standard-HTTP-Feld
* Andere Eigenschaften in einem `$metadata`-Abschnitt. Dazu gehören:
    - Der DTMI des Modells für den digitalen Zwilling
    - Der Synchronisierungsstatus für jede schreibbare Eigenschaft. Dies ist besonders hilfreich für Geräte, bei denen es möglich ist, dass der Dienst und das Gerät abweichende Status aufweisen (z. B. wenn ein Gerät offline ist). Derzeit gilt diese Eigenschaft nur für physische Geräte, die mit IoT Hub verbunden sind. Mit den Daten im Metadatenabschnitt erfahren Sie den vollständigen Status einer Eigenschaft sowie die Zeitstempel der letzten Änderung. Weitere Informationen zum Synchronisierungsstatus erhalten Sie in [diesem IoT Hub-Tutorial](../iot-hub/tutorial-device-twins.md) zum Synchronisieren des Gerätestatus.
    - Dienstspezifische Metadaten, wie z. B. aus IoT Hub oder Azure Digital Twins 

Sie können den zurückgegebenen JSON-Code für den Zwilling mithilfe einer JSON-Analysebibliothek Ihrer Wahl analysieren, wie z. B. mit `System.Text.Json`.

Sie können auch die Serialisierungshilfsklasse `BasicDigitalTwin` verwenden, die im SDK enthalten ist und die die zentralen Zwillingsmetadaten und -eigenschaften in einem vorab analysierten Format zurückgibt. Beispiel:

```csharp
Response<string> res = client.GetDigitalTwin(twin_id);
BasicDigitalTwin twin = JsonSerializer.Deserialize<BasicDigitalTwin>(res.Value);
Console.WriteLine($"Model id: {twin.Metadata.ModelId}");
foreach (string prop in twin.CustomProperties.Keys)
{
    if (twin.CustomProperties.TryGetValue(prop, out object value))
        Console.WriteLine($"Property '{prop}': {value}");
}
```

Weitere Informationen zu den Serialisierungshilfsklassen finden Sie unter [*Gewusst wie: Verwenden der Azure Digital Twins-APIs und SDKs*](how-to-use-apis-sdks.md).

## <a name="update-a-digital-twin"></a>Aktualisieren eines digitalen Zwillings

Um die Eigenschaften eines digitalen Zwillings zu aktualisieren, schreiben Sie die Informationen, die Sie ersetzen möchten, im [JSON Patch](http://jsonpatch.com/)-Format. So können Sie mehrere Eigenschaften gleichzeitig ersetzen. Anschließend übergeben Sie das JSON Patch-Dokument an eine `Update`-Methode:

```csharp
await client.UpdateDigitalTwin(id, patch);
```

Ein Patchaufruf kann beliebig viele Eigenschaften auf einem einzelnen Zwilling aktualisieren (sogar alle). Wenn Sie Eigenschaften für mehrere Zwillinge aktualisieren müssen, benötigen Sie für jeden Zwilling einen separaten Updateaufruf.

> [!TIP]
> Nachdem Sie einen Zwilling erstellt oder aktualisiert haben, kann es bis zu 10 Sekunden dauern, bis sich die Änderungen in [Abfragen](how-to-query-graph.md) widerspiegeln. Bei der `GetDigitalTwin`-API (siehe [weiter oben in diesem Artikel](#get-data-for-a-digital-twin)) tritt diese Verzögerung nicht auf. Verwenden Sie daher den API-Befehl anstelle von Abfragen, um die neu aktualisierten Zwillinge anzuzeigen, wenn Sie eine sofortige Antwort benötigen. 

Im Folgenden finden Sie ein Beispiel für JSON Patch-Code. Dieses Dokument ersetzt die Eigenschaftswerte *mass* und *radius* des digitalen Zwillings, auf den es angewendet wird.

```json
[
  {
    "op": "replace",
    "path": "/mass",
    "value": 0.0799
  },
  {
    "op": "replace",
    "path": "/radius",
    "value": 0.800
  }
]
```

Sie können Patches manuell oder mithilfe einer Serialisierungshilfsklasse im [SDK](how-to-use-apis-sdks.md) erstellen. Im Folgenden finden Sie Beispiele für beides.

#### <a name="create-patches-manually"></a>Manuelles Erstellen von Patches

```csharp
List<object> twinData = new List<object>();
twinData.Add(new Dictionary<string, object>() {
    { "op", "add"},
    { "path", "/Temperature"},
    { "value", 25.0}
});

await client.UpdateDigitalTwinAsync(twinId, JsonConvert.SerializeObject(twinData));
```

#### <a name="create-patches-using-the-helper-class"></a>Erstellen von Patches mithilfe der Hilfsklasse

```csharp
UpdateOperationsUtility uou = new UpdateOperationsUtility();
uou.AppendAddOp("/Temperature", 25.0);
await client.UpdateDigitalTwinAsync(twinId, uou.Serialize());
```

### <a name="update-properties-in-digital-twin-components"></a>Aktualisieren von Eigenschaften in Komponenten digitaler Zwillinge

Denken Sie daran, dass ein Modell Komponenten enthalten kann, sodass es aus anderen Modellen besteht. 

Zum Patchen von Eigenschaften in den Komponenten eines digitalen Zwillings verwenden Sie die Pfadsyntax im JSON-Patch:

```json
[
  {
    "op": "replace",
    "path": "/mycomponentname/mass",
    "value": 0.0799
  }
]
```

### <a name="update-a-digital-twins-model"></a>Aktualisieren eines Modells eines digitalen Zwillings

Die `Update`-Funktion kann auch verwendet werden, um einen digitalen Zwilling zu einem anderen Modell zu migrieren. 

Sehen Sie sich beispielsweise das folgende JSON Patch-Dokument an, das das `$model`-Metadatenfeld des digitalen Zwillings ersetzt:

```json
[
  {
    "op": "replace",
    "path": "/$metadata/$model",
    "value": "dtmi:com:contoso:foo;1"
  }
]
```

Dieser Vorgang ist nur erfolgreich, wenn der vom Patch geänderte digitale Zwilling dem neuen Modell entspricht. 

Betrachten Sie das folgenden Beispiel:
1. Stellen Sie sich einen digitalen Zwilling mit dem Modell *foo_old* vor. *foo_old* definiert die erforderliche *mass*-Eigenschaft.
2. Das neue Modell *foo_new* definiert die Eigenschaften „mass“ und fügt die neue erforderliche Eigenschaft *temperature* hinzu.
3. Nach dem Patch muss der digitale Zwilling die beiden Eigenschaften „mass“ und „temperature“ aufweisen. 

Der Patch für diese Situation muss sowohl das Modell als auch die Eigenschaft „temperature“ des Zwillings wie folgt aktualisieren:

```json
[
  {
    "op": "replace",
    "path": "$metadata.$model",
    "value": "dtmi:com:contoso:foo_new"
  },
  {
    "op": "add",
    "path": "temperature",
    "value": 60
  }
]
```

### <a name="handle-conflicting-update-calls"></a>Verarbeitung von in Konflikt stehenden Updateaufrufen

Azure Digital Twins stellt sicher, dass alle eingehenden Anforderungen nacheinander verarbeitet werden. Dies bedeutet, dass Sie selbst dann, wenn mehrere Funktionen versuchen, dieselbe Eigenschaft für einen Zwilling gleichzeitig zu aktualisieren, **keinen** expliziten Sperrcode schreiben müssen, um den Konflikt zu beheben.

Dieses Verhalten erfolgt für jeden Zwilling einzeln. 

Stellen Sie sich beispielsweise ein Szenario vor, in dem die folgenden drei Aufrufe gleichzeitig eingehen: 
*   Schreiben von Eigenschaft A auf *Zwilling1*
*   Schreiben von Eigenschaft B auf *Zwilling1*
*   Schreiben von Eigenschaft A auf *Zwilling2*

Die beiden Aufrufe, die *Zwilling1* ändern, werden nacheinander ausgeführt, und für jede Änderung werden Änderungsnachrichten generiert. Der Aufruf zum Ändern von *Zwilling2* kann ohne Konflikt zeitgleich ausgeführt werden, sobald er eingeht.

## <a name="delete-a-digital-twin"></a>Löschen eines digitalen Zwillings

Sie können Zwillinge mithilfe von `DeleteDigitalTwin(ID)` löschen. Sie können einen Zwilling jedoch nur löschen, wenn er keine weiteren Beziehungen aufweist. Sie müssen zuerst alle Beziehungen löschen. 

Hier sehen Sie ein Beispiel für den dazugehörigen Code:

```csharp
static async Task DeleteTwin(string id)
{
    await FindAndDeleteOutgoingRelationshipsAsync(id);
    await FindAndDeleteIncomingRelationshipsAsync(id);
    try
    {
        await client.DeleteDigitalTwin(id);
    } catch (RequestFailedException exc)
    {
        Console.WriteLine($"*** Error:{exc.Error}/{exc.Message}");
    }
}

public async Task FindAndDeleteOutgoingRelationshipsAsync(string dtId)
{
    // Find the relationships for the twin

    try
    {
        // GetRelationshipsAsync will throw an error if a problem occurs
        AsyncPageable<string> relsJson = client.GetRelationshipsAsync(dtId);

        await foreach (string relJson in relsJson)
        {
            var rel = System.Text.Json.JsonSerializer.Deserialize<BasicRelationship>(relJson);
            await client.DeleteRelationshipAsync(dtId, rel.Id).ConfigureAwait(false);
            Log.Ok($"Deleted relationship {rel.Id} from {dtId}");
        }
    }
    catch (RequestFailedException ex)
    {
        Log.Error($"*** Error {ex.Status}/{ex.ErrorCode} retrieving or deleting relationships for {dtId} due to {ex.Message}");
    }
}

async Task FindAndDeleteIncomingRelationshipsAsync(string dtId)
{
    // Find the relationships for the twin

    try
    {
        // GetRelationshipssAsync will throw an error if a problem occurs
        AsyncPageable<IncomingRelationship> incomingRels = client.GetIncomingRelationshipsAsync(dtId);

        await foreach (IncomingRelationship incomingRel in incomingRels)
        {
            await client.DeleteRelationshipAsync(incomingRel.SourceId, incomingRel.RelationshipId).ConfigureAwait(false);
            Log.Ok($"Deleted incoming relationship {incomingRel.RelationshipId} from {dtId}");
        }
    }
    catch (RequestFailedException ex)
    {
        Log.Error($"*** Error {ex.Status}/{ex.ErrorCode} retrieving or deleting incoming relationships for {dtId} due to {ex.Message}");
    }
}
```

### <a name="delete-all-digital-twins"></a>Löschen aller digitalen Zwillinge

Ein Beispiel für das gleichzeitige Löschen aller Zwillinge finden Sie in der Beispiel-App im [*Tutorial: Erkunden von Azure Digital Twins mit einer Beispielclient-App*](tutorial-command-line-app.md). In der Datei *CommandLoop.cs* wird dies in einer `CommandDeleteAllTwins`-Funktion durchgeführt.

## <a name="manage-twins-with-cli"></a>Verwalten von Zwillingen mit der Befehlszeilenschnittstelle

Zwillinge können auch über die Azure Digital Twins-Befehlszeilenschnittstelle verwaltet werden. Die Befehle finden Sie unter [*Vorgehensweise: Verwenden der Azure Digital Twins-Befehlszeilenschnittstelle*](how-to-use-cli.md).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie Beziehungen zwischen Ihren digitalen Zwillingen erstellen und verwalten können:
* [*Verwenden Verwalten des Zwillingsgraphen mit Beziehungen*](how-to-manage-graph.md)