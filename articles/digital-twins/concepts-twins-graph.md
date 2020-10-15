---
title: Digitale Zwillinge und der Zwillingsgraph
titleSuffix: Azure Digital Twins
description: Enthält grundlegende Informationen zum Konzept eines digitalen Zwillings und die Erstellung eines Graphen basierend auf den entsprechenden Beziehungen.
author: baanders
ms.author: baanders
ms.date: 3/12/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 5821a1d1f6713ef39d7475fb004164e7c0fd71ec
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87062061"
---
# <a name="understand-digital-twins-and-their-twin-graph"></a>Grundlegendes zu digitalen Zwillingen und zum zugehörigen Zwillingsgraphen

In einer Azure Digital Twins-Lösung werden die Entitäten in Ihrer Umgebung durch **digitale Azure-Zwillinge** dargestellt. Ein digitaler Zwilling ist eine Instanz eines Ihrer benutzerdefinierten [Modelle](concepts-models.md). Er kann mit anderen digitalen Zwillingen über **Beziehungen** verbunden werden, um einen **Zwillingsgraphen** zu erhalten. Dieser Zwillingsgraph ist die Darstellung Ihrer gesamten Umgebung.

> [!TIP]
> „Azure Digital Twins“ ist der Name dieses Azure-Diensts. „Digitale Zwillinge“ oder einfach „Zwillinge“ bezieht sich einzelne Zwillingsknoten auf Ihrer Instanz des Diensts.

## <a name="digital-twins"></a>Digital Twins

Bevor Sie auf Ihrer Azure Digital Twins-Instanz einen digitalen Zwilling erstellen können, müssen Sie ein *Modell* in den Dienst hochladen. Mit einem Modell wird unter anderem die Gruppe mit den Eigenschaften, Telemetrienachrichten und Beziehungen beschrieben, über die ein bestimmter Zwilling verfügen kann. Informationen zu den Informationsarten, die in einem Modell definiert sind, finden Sie unter [*Konzepte: Grundlegendes zu Zwillingsmodellen in Azure Digital Twins*](concepts-models.md).

Nachdem Sie ein Modell erstellt und hochgeladen haben, kann Ihre Client-App eine Instanz des entsprechenden Typs erstellen. Hierbei handelt es sich um einen digitalen Zwilling. Nachdem Sie beispielsweise ein Modell vom Typ *Floor* (Etage) erstellt haben, können Sie einen oder mehrere digitale Zwillinge erstellen, die diesen Typ nutzen (z. B. ein Zwilling vom Typ *Floor* mit dem Namen *GroundFloor*, einen anderen mit dem Namen *Floor2* usw.). 

## <a name="relationships-a-graph-of-digital-twins"></a>Beziehungen: ein Graph für digitale Zwillinge

Zwillinge werden anhand ihrer Beziehungen zu einem Zwillingsgraphen vereint. Die Beziehungen, die ein Zwilling aufweisen kann, werden als Teil des Modells definiert.  

Beispielsweise kann für das Modell *Floor* (Etage) eine Beziehung vom Typ *contains* (enthält) definiert werden, die für Zwillinge vom Typ *Room* (Zimmer) bestimmt ist. Mit dieser Definition können Sie mit Azure Digital Twins *contains*-Beziehungen von allen *Floor*-Zwillingen zu allen *Room*-Zwillingen erstellen (einschließlich Zwillingen mit Untertypen von *Room*). 

Das Ergebnis dieses Prozesses ist eine Gruppe mit Knoten (die digitalen Zwillinge), die über Edges (die Beziehungen) in einem Graphen verbunden sind.

[!INCLUDE [visualizing with Azure Digital Twins explorer](../../includes/digital-twins-visualization.md)]

## <a name="create-with-the-apis"></a>Erstellen mit den APIs

In diesem Abschnitt wird gezeigt, wie das Erstellen digitaler Zwillinge und Beziehungen über eine Clientanwendung aussieht. Es sind .NET-Codebeispiele enthalten, die die [DigitalTwins-APIs](how-to-use-apis-sdks.md) nutzen, um zusätzlichen Kontext darüber bereitzustellen, was innerhalb der verschiedenen Konzepte passiert.

### <a name="create-digital-twins"></a>Erstellen digitaler Zwillinge

Unten ist ein Ausschnitt des Clientcodes angegeben, in dem die [DigitalTwins-APIs](how-to-use-apis-sdks.md) zum Instanziieren eines Zwillings vom Typ *Room* (Zimmer) verwendet wird.

In der aktuellen Vorschauversion von Azure Digital Twins müssen alle Eigenschaften eines Zwillings initialisiert werden, bevor der Zwilling erstellt werden kann. Hierfür wird ein JSON-Dokument erstellt, das die benötigten Initialisierungswerte enthält.

```csharp
public Task<boolean> CreateRoom(string id, double temperature, double humidity) 
{
    // Define the model for the twin to be created
    Dictionary<string, object> meta = new Dictionary<string, object>()
    {
      { "$model", "dtmi:com:contoso:Room;2" }
    };
    // Initialize the twin properties
    Dictionary<string, object> initData = new Dictionary<string, object>()
    {
      { "$metadata", meta },
      { "Temperature", temperature},
      { "Humidity", humidity},
    };
    try
    {
      await client.DigitalTwins.AddAsync(id, initData);
      return true;
    }
    catch (ErrorResponseException e)
    {
      Console.WriteLine($"*** Error creating twin {id}: {e.Response.StatusCode}");
      return false;
    }
}
```

### <a name="create-relationships"></a>Erstellen von Beziehungen

Hier ist ein Beispiel für Clientcode angegeben, in dem die [DigitalTwins-APIs](how-to-use-apis-sdks.md) verwendet werden, um eine Beziehung zwischen einem digitalen Zwilling vom Typ *Floor* (Etage) mit dem Namen *GroundFloor* und einem digitalen Zwilling vom Typ *Room* (Zimmer) mit dem Namen *Cafe* zu erstellen.

```csharp
// Create Twins, using functions similar to the previous sample
await CreateRoom("Cafe", 70, 66);
await CreateFloor("GroundFloor", averageTemperature=70);
// Create relationships
Dictionary<string, object> targetrec = new Dictionary<string, object>()
{
    { "$targetId", "Cafe" }
};
try
{
    await client.DigitalTwins.AddEdgeAsync("GroundFloor", "contains", "GF-to-Cafe", targetrec);
} catch(ErrorResponseException e)
{
    Console.WriteLine($"*** Error creating relationship: {e.Response.StatusCode}");
}
```

## <a name="json-representations-of-graph-elements"></a>JSON-Darstellungen von Graphelementen

Daten zu digitalen Zwillingen und Beziehungsdaten werden jeweils im JSON-Format gespeichert. Dies bedeutet Folgendes: Wenn Sie auf Ihrer Azure Digital Twins-Instanz den [Zwillingsgraphen abfragen](how-to-query-graph.md), ist das Ergebnis eine JSON-Darstellung der von Ihnen erstellten digitalen Zwillinge und Beziehungen.

### <a name="digital-twin-json-format"></a>JSON-Format von digitalen Zwillingen

Bei der Darstellung als JSON-Objekt zeigt ein digitaler Zwilling die folgenden Felder an:

| Feldname | BESCHREIBUNG |
| --- | --- |
| `$dtId` | Eine vom Benutzer angegebene Zeichenfolge zur Darstellung der ID des digitalen Zwillings |
| `$etag` | Ein vom Webserver zugewiesenes Standard-HTTP-Feld |
| `$conformance` | Eine Enumeration mit dem Konformitätsstatus dieses digitalen Zwillings (*conformant* (konform), *non-conformant* (nicht konform), *unknown* (unbekannt)) |
| `{propertyName}` | Den Wert einer Eigenschaft im JSON-Format (`string`, Zahlentyp oder Objekt) |
| `$relationships` | Die URL des Pfads zur Sammlung mit den Beziehungen. Dieses Feld ist nicht vorhanden, wenn der digitale Zwilling keine ausgehenden Beziehungsedges aufweist. |
| `$metadata.$model` | [Optional] Die ID der Modellschnittstelle, die diesen digitalen Zwilling kennzeichnet |
| `$metadata.{propertyName}.desiredValue` | [Nur für schreibbare Eigenschaften] Der gewünschte Wert der angegebenen Eigenschaft |
| `$metadata.{propertyName}.desiredVersion` | [Nur für schreibbare Eigenschaften] Die Version des gewünschten Werts |
| `$metadata.{propertyName}.ackVersion` | Die von der Geräte-App, die den digitalen Zwilling implementiert, bestätigte Version |
| `$metadata.{propertyName}.ackCode` | [Nur für schreibbare Eigenschaften] Der von der Geräte-App, die den digitalen Zwilling implementiert, zurückgegebene `ack`-Code |
| `$metadata.{propertyName}.ackDescription` | [Nur für schreibbare Eigenschaften] Die von der Geräte-App, die den digitalen Zwilling implementiert, zurückgegebene `ack`-Beschreibung |
| `{componentName}` | Ein JSON-Objekt mit den Eigenschaftswerten und Metadaten der Komponente, die denen des Stammobjekts ähneln. Dieses Objekt ist auch vorhanden, wenn die Komponente keine Eigenschaften hat. |
| `{componentName}.{propertyName}` | Den Wert der Komponenteneigenschaft im JSON-Format (`string`, Zahlentyp oder Objekt) |
| `{componentName}.$metadata` | Die Metadateninformationen für die Komponente, die dem Objekt `$metadata` auf der Stammebene ähneln |

Hier ist ein Beispiel für einen digitalen Zwilling angegeben, der als JSON-Objekt formatiert ist:

```json
{
  "$dtId": "Cafe",
  "$etag": "W/\"e59ce8f5-03c0-4356-aea9-249ecbdc07f9\"",
  "Temperature": 72,
  "Location": {
    "x": 101,
    "y": 33
  },
  "component": {
    "TableOccupancy": 1,
    "$metadata": {
      "TableOccupancy": {
        "desiredValue": 1,
        "desiredVersion": 3,
        "ackVersion": 2,
        "ackCode": 200,
        "ackDescription": "OK"
      }
    }
  },
  "$metadata": {
    "$model": "dtmi:com:contoso:Room;1",
    "Temperature": {
      "desiredValue": 72,
      "desiredVersion": 5,
      "ackVersion": 4,
      "ackCode": 200,
      "ackDescription": "OK"
    },
    "Location": {
      "desiredValue": {
        "x": 101,
        "y": 33,
      },
      "desiredVersion": 8,
      "ackVersion": 8,
      "ackCode": 200,
      "ackDescription": "OK"
    }
  }
}
```

### <a name="relationship-json-format"></a>Beziehung im JSON-Format

Bei der Darstellung als JSON-Objekt zeigt die Beziehung eines digitalen Zwillings die folgenden Felder an:

| Feldname | BESCHREIBUNG |
| --- | --- |
| `$relationshipId` | Eine vom Benutzer angegebene Zeichenfolge zur Darstellung der ID dieser Beziehung. Diese Zeichenfolge ist im Kontext des digitalen Quellzwillings eindeutig. Dies bedeutet, dass `sourceId` + `relationshipId` im Kontext der Azure Digital Twins-Instanz eindeutig ist. |
| `$etag` | Ein vom Webserver zugewiesenes Standard-HTTP-Feld |
| `$sourceId` | Die ID des digitalen Quellzwillings |
| `$targetId` | Die ID des digitalen Zielzwillings |
| `$relationshipName` | Den Namen der Beziehung |
| `{propertyName}` | [Optional] Den Wert einer Eigenschaft dieser Beziehung im JSON-Format (`string`, Zahlentyp oder Objekt) |

Hier ist ein Beispiel für eine Beziehung angegeben, die als JSON-Objekt formatiert ist:

```json
{
  "$relationshipId": "relationship-01",
  "$etag": "W/\"506e8391-2b21-4ac9-bca3-53e6620f6a90\"",
  "$sourceId": "GroundFloor",
  "$targetId": "Cafe",
  "$relationshipName": "contains",
  "startDate": "2020-02-04"
}
```

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich darüber, wie Sie Graphelemente mit Azure Digital Twin-APIs verwalten können:
* [*Verwenden Verwalten digitaler Zwillinge*](how-to-manage-twin.md)
* [*Verwenden Verwalten des Zwillingsgraphen mit Beziehungen*](how-to-manage-graph.md)

Sie können sich auch damit vertraut machen, wie Sie Informationen für einen Azure Digital Twins-Zwillingsgraphen abfragen:
* [*Konzepte: Abfragesprache*](concepts-query-language.md)