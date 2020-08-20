---
title: Verwalten benutzerdefinierter Modelle
titleSuffix: Azure Digital Twins
description: Informationen zum Erstellen, Bearbeiten und Löschen von Modellen in Azure Digital Twins
author: baanders
ms.author: baanders
ms.date: 3/12/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 919db9338917a9c2bedd7806eb251a2e5ef6187b
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2020
ms.locfileid: "88509993"
---
# <a name="manage-azure-digital-twins-models"></a>Verwalten von Azure Digital Twins-Modellen

Sie können [Modelle](concepts-models.md), die Ihrer Azure Digital Twins-Instanz bekannt sind, mithilfe der [**DigitalTwinsModels-APIs**](how-to-use-apis-sdks.md), dem [.NET (C#) SDK](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core) oder der [Azure Digital Twins-CLI](how-to-use-cli.md) verwalten. 

Zu den Verwaltungsvorgängen gehören das Hochladen, Überprüfen, Abrufen und Löschen von Modellen. 

## <a name="create-models"></a>Erstellen von Modellen

Azure Digital Twins-Modelle werden in DTDL geschrieben und als *JSON*-Dateien gespeichert. Für [Visual Studio Code](https://code.visualstudio.com/) gibt es auch eine [DTDL-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-dtdl), die die Syntaxüberprüfung und andere Features zum Schreiben von DTDL-Dokumenten umfasst.

Angenommen, ein Krankenhaus möchte seine Räume digital überwachen. Jeder Raum enthält einen intelligenten Seifenspender, der das Händewaschen überwacht, sowie Sensoren, die den Durchgangsverkehr im Raum nachverfolgen.

Der erste Schritt zur Lösung besteht darin, Modelle zu erstellen, die verschiedene Aspekte des Krankenhauses darstellen. In diesem Szenario kann ein Patientenzimmer wie folgt beschrieben werden:

```json
{
  "@id": "dtmi:com:contoso:PatientRoom;1",
  "@type": "Interface",
  "@context": "dtmi:dtdl:context;2",
  "displayName": "Patient Room",
  "contents": [
    {
      "@type": "Property",
      "name": "visitorCount",
      "schema": "double"
    },
    {
      "@type": "Property",
      "name": "handWashCount",
      "schema": "double"
    },
    {
      "@type": "Property",
      "name": "handWashPercentage",
      "schema": "double"
    },
    {
      "@type": "Relationship",
      "name": "hasDevices"
    }
  ]
}
```

> [!NOTE]
> Dies ist ein Beispieltext für eine JSON-Datei, in der ein Modell definiert und gespeichert wird, das als Teil eines Clientprojekts hochgeladen werden soll. Der REST-API-Aufruf hingegen verwendet ein Array mit Modelldefinitionen wie die obige (die einer `IEnumerable<string>` im .NET SDK zugeordnet ist). Damit Sie dieses Modell direkt in der REST-API verwenden können, sollten Sie es in Klammern setzen.

Dieses Modell definiert einen Namen und eine eindeutige ID für das Patientenzimmer sowie Eigenschaften zur Darstellung der Anzahl der Besucher und zum Status des Händewaschens. Diese Werte werden von Bewegungssensoren und intelligenten Seifenspendern aktualisiert und gemeinsam verwendet, um eine Eigenschaft für das *Händewaschen (in Prozent)* zu berechnen. Das Modell definiert außerdem eine *hasDevices*-Beziehung, die verwendet wird, um einen [digitalen Zwilling](concepts-twins-graph.md) basierend auf dem *Raummodell* mit den tatsächlichen Geräten zu verbinden.

Mit dieser Methode können Sie Modelle für die Stationen oder Bereiche des Krankenhauses oder sogar das gesamte Krankenhaus definieren.

### <a name="validate-syntax"></a>Überprüfen der Syntax

[!INCLUDE [Azure Digital Twins: validate models info](../../includes/digital-twins-validate.md)]

## <a name="manage-models-with-apis"></a>Verwalten von Modellen mit APIs

In den folgenden Abschnitten wird erläutert, wie Sie unterschiedliche Modellverwaltungsvorgänge mithilfe der [Azure Digital Twins-APIs und -SDKs](how-to-use-apis-sdks.md) durchführen.

> [!NOTE]
> In den nachfolgenden Beispielen wird aus Gründen der Übersichtlichkeit nicht auf die Fehlerbehandlung eingegangen. Es wird jedoch dringend empfohlen, in Ihren Projekten Dienstaufrufe in Try/Catch-Blöcken zu umschließen.

> [!TIP] 
> Es gibt für alle SDK-Methoden synchrone und asynchrone Versionen. Bei Pagingaufrufen geben die asynchronen Methoden `AsyncPageable<T>` zurück, während die synchronen Versionen `Pageable<T>` zurückgeben.

### <a name="upload-models"></a>Hochladen von Modellen

Nachdem Sie Modelle erstellt haben, können Sie sie in die Azure Digital Twins-Instanz hochladen.

> [!TIP]
> Es wird empfohlen, Ihre Modelle offline zu validieren, bevor Sie sie in Ihre Azure Digital Twins-Instanz hochladen. Sie können die [clientseitige DTDL-Parserbibliothek](https://nuget.org/packages/Microsoft.Azure.DigitalTwins.Parser/) und das [Beispiel für das DTDL-Validierungssteuerelement](https://docs.microsoft.com/samples/azure-samples/dtdl-validator/dtdl-validator) verwenden, die im Artikel [ *Analysieren und Überprüfen von Modellen mit der DTDL-Parserbibliothek*](how-to-parse-models.md) beschrieben sind, um Ihre Modelle vor dem Hochladen in den Dienst zu überprüfen.

Wenn Sie zum Hochladen eines Modells bereit sind, können Sie den folgenden Codeausschnitt verwenden:

```csharp
// 'client' is an instance of DigitalTwinsClient
// Read model file into string (not part of SDK)
StreamReader r = new StreamReader("MyModelFile.json");
string dtdl = r.ReadToEnd(); r.Close();
string[] dtdls = new string[] { dtdl };
client.CreateModels(dtdls);
```

Die Methode `CreateModels` akzeptiert mehrere Dateien in einer einzigen Transaktion. Hier ein Beispiel zur Veranschaulichung:

```csharp
var dtdlFiles = Directory.EnumerateFiles(sourceDirectory, "*.json");

List<string> dtdlStrings = new List<string>();
foreach (string fileName in dtdlFiles)
{
    // Read model file into string (not part of SDK)
    StreamReader r = new StreamReader(fileName);
    string dtdl = r.ReadToEnd(); r.Close();
    dtdlStrings.Add(dtdl);
}
client.CreateModels(dtdlStrings);
```

Modelldateien können mehr als ein einzelnes Modell enthalten. In diesem Fall müssen die Modelle in einem JSON-Array platziert werden. Beispiel:

```json
[
  {
    "@id": "dtmi:com:contoso:Planet",
    "@type": "Interface",
    //...
  },
  {
    "@id": "dtmi:com:contoso:Moon",
    "@type": "Interface",
    //...
  }
]
```
 
Modelldateien werden beim Hochladen vom Dienst überprüft.

### <a name="retrieve-models"></a>Abrufen von Modellen

Sie können Modelle auflisten und abrufen, die auf Ihrer Azure Digital Twins-Instanz gespeichert sind. 

Dafür haben Sie die folgenden Möglichkeiten:
* Alle Modelle abrufen
* Ein einzelnes Modell abrufen
* Ein einzelnes Modell mit Abhängigkeiten abrufen
* Metadaten für Modelle abrufen

Hier sehen Sie ein paar Beispielaufrufe:

```csharp
// 'client' is a valid DigitalTwinsClient object

// Get a single model, metadata and data
ModelData md1 = client.GetModel(id);

// Get a list of the metadata of all available models
Pageable<ModelData> pmd2 = client.GetModels();

// Get a list of metadata and full model definitions
Pageable<ModelData> pmd3 = client.GetModels(null, true);

// Get models and metadata for a model ID, including all dependencies (models that it inherits from, components it references)
Pageable<ModelData> pmd4 = client.GetModels(new string[] { modelId }, true);
```

Die API-Aufrufe zum Abrufen von Modellen geben alle `ModelData`-Objekte zurück. `ModelData` enthält Metadaten zum Modell, das in der Azure Digital Twins-Instanz gespeichert ist, z. B. Name, DTMI und Erstellungsdatum des Modells. Das Objekt `ModelData` kann auch das Modell selbst enthalten. Je nach Parametern können Sie daher die Abrufaufrufe verwenden, um entweder nur Metadaten (was z. B. nützlich ist, wenn Sie eine Liste verfügbarer Tools für die Benutzeroberfläche anzeigen möchten) oder das gesamte Modell abzurufen.

Der Aufruf `RetrieveModelWithDependencies` gibt nicht nur das angeforderte Modell zurück, sondern auch alle Modelle, von denen das angeforderte Modell abhängig ist.

Modelle werden nicht unbedingt genau in dem Dokumentformat zurückgegeben, in dem sie hochgeladen wurden. Azure Digital Twins gewährleistet nur, dass das Rückgabeformat semantisch gleichwertig ist. 

### <a name="remove-models"></a>Entfernen von Modellen

Ebenso gibt es zwei Möglichkeiten, Modelle aus dem Dienst zu entfernen:
* **Außerbetriebsetzung:** Wenn ein Modell außer Betrieb gesetzt wurde, können Sie es nicht mehr zum Erstellen neuer digitaler Zwillinge verwenden. Vorhandene digitale Zwillinge, die dieses Modell bereits verwenden, sind nicht betroffen. Deshalb können Sie sie weiterhin aktualisieren, indem Sie z. B. Eigenschaften ändern und Beziehungen hinzufügen oder löschen.
* **Löschung:** Dadurch wird das Modell vollständig aus der Lösung entfernt. Alle Zwillinge, die dieses Modell verwenden, sind keinem gültigen Modell mehr zugeordnet, weshalb sie so behandelt werden, als würden sie über gar kein Modell verfügen. Sie können diese Zwillinge zwar weiterhin lesen, aber Sie können erst wieder Updates an ihnen vornehmen, wenn sie einem anderen Modell zugewiesen werden.

Es handelt sich hierbei um separate Features, die einander nicht beeinträchtigen. Sie können aber zusammen verwendet werden, um ein Modell schrittweise zu entfernen. 

#### <a name="decommissioning"></a>Außerbetriebsetzung

Mithilfe des folgenden Codes können Sie Modelle außer Betrieb setzen:

```csharp
// 'client' is a valid DigitalTwinsClient  
client.DecommissionModel(dtmiOfPlanetInterface);
// Write some code that deletes or transitions digital twins
//...
```

Der Status „Außerbetriebsetzung“ eines Modells ist in den `ModelData`-Datensätzen enthalten, die von den APIs zum Abrufen von Modellen zurückgegeben werden.

#### <a name="deletion"></a>Löschen

Sie können entweder alle Modelle auf einmal aus der Instanz löschen oder einzeln bestimmte Modelle zum Löschen auswählen.

Ein Beispiel für das Löschen aller Modelle finden Sie in der Beispiel-App, die im folgenden Tutorial verwendet wird: [*Tutorial: Erkunden von Azure Digital Twins mit einer Beispielclient-App*](tutorial-command-line-app.md). In der Datei *CommandLoop.cs* wird dies in einer `CommandDeleteAllModels`-Funktion durchgeführt.

Im Rest dieses Abschnitts wird das Löschen von Modellen ausführlicher beschrieben, und Sie erfahren, wie Sie einzelne Modelle löschen können.

##### <a name="before-deletion-deletion-requirements"></a>Vor dem Löschen: Löschanforderungen

Generell können Modelle jederzeit gelöscht werden.

Allerdings stellen Modelle, von denen andere Modelle abhängig sind (durch eine `extends`-Beziehung oder weil es sich um eine Komponente handelt), eine Ausnahme dar. Wenn beispielsweise ein *ConferenceRoom*-Modell ein *Room*-Modell erweitert und über ein *ACUnit*-Modell als Komponente verfügt, können Sie die Modelle *Room* und *ACUnit* erst löschen, wenn die jeweiligen Verweise für das Modell *ConferenceRoom* entfernt werden. 

Hierzu können Sie das abhängige Modell aktualisieren, um die Abhängigkeiten zu entfernen, oder das abhängige Modell vollständig löschen.

##### <a name="during-deletion-deletion-process"></a>Beim Löschen: Löschvorgang

Selbst wenn ein Modell die Anforderungen zum sofortigen Löschen erfüllt, sollten Sie zunächst einige Maßnahmen ergreifen, um unbeabsichtigte Folgen für die übrigen Zwillinge zu vermeiden. Im Folgenden wird erläutert, wie Sie den Prozess verwalten können:
1. Setzen Sie zunächst das Modell außer Betrieb.
2. Warten Sie einige Minuten, um sicherzustellen, dass der Dienst alle in letzter Minute gesendeten Erstellungsanforderungen verarbeitet hat, die vor der Außerbetriebnahme gesendet wurden.
3. Fragen Sie die Zwillinge des Modells ab, um alle Zwillinge zu finden, die das außer Betrieb genommene Modell verwenden.
4. Löschen Sie die Zwillinge, wenn Sie sie nicht mehr benötigen, oder patchen Sie sie bei Bedarf an ein neues Modell. Sie können die Zwillinge auch unbeachtet lassen. Dann werden sie zu Zwillingen ohne Modell, sobald das Modell gelöscht wird. Weitere Informationen zu den Auswirkungen dieses Zustands finden Sie im nächsten Abschnitt.
5. Warten Sie einige Minuten, um sicherzustellen, dass die Änderungen übernommen wurden.
6. Löschen Sie das Modell. 

Dazu können Sie den folgenden Aufruf verwenden:
```csharp
// 'client' is a valid DigitalTwinsClient
await client.DeleteModelAsync(IDToDelete);
```

##### <a name="after-deletion-twins-without-models"></a>Nach dem Löschen: Zwillinge ohne Modelle

Nach dem Löschen eines Modells werden sämtliche digitale Zwillinge, die das Modell verwendet haben, als Zwillinge ohne Modell betrachtet. Es gibt zwar keine Abfrage, die Sie verwenden können, um eine Liste aller Zwillinge in diesem Zustand zu erhalten, aber Sie *können* die Zwillinge des jeweiligen gelöschten Modells abfragen, um herauszufinden, welche Zwillinge betroffen sind.

Im Folgenden finden Sie eine Übersicht darüber, wozu Zwillinge ohne Modell (nicht) verwendet werden können.

**Möglich:**
* Den Zwilling abfragen
* Lesen von Eigenschaften
* Ausgehende Beziehungen lesen
* Eingehende Beziehungen hinzufügen oder löschen (z. B. können andere Zwillinge weiterhin Beziehungen *zu* diesem Zwilling herstellen)
  - Das `target` in der Beziehungsdefinition kann weiterhin den DTMI des gelöschten Modells widerspiegeln. Auch eine Beziehung ohne festgelegtes Ziel kann hier funktionieren.
* Löschen von Beziehungen
* Den Zwilling löschen

**Nicht möglich:**
* Ausgehende Beziehungen bearbeiten (z. B. Beziehungen dieses Zwillings *zu* anderen Zwillingen)
* Eigenschaften bearbeiten

##### <a name="after-deletion-re-uploading-a-model"></a>Nach dem Löschen: Erneutes Hochladen eines Modells

Wenn Sie ein Modell löschen, können Sie später ein neues Modell mit derselben ID hochladen. Dann passiert Folgendes:
* Der Lösungsspeicher erkennt das Modell als ganz neues Modell an. Er erinnert sich nicht daran, dass das alte Modell hochgeladen wurde.   
* Wenn das Diagramm übrig gebliebene Zwillinge enthält, die auf das neue Modell verweisen, werden diese nicht mehr als verwaist angesehen. Die Modell-ID ist dann einschließlich einer neuen Definition wieder gültig. Wenn sich die neue Definition für das Modell jedoch von der gelöschten Modelldefinition unterscheidet, weisen diese Zwillinge möglicherweise Eigenschaften und Beziehungen auf, die der gelöschten Definition entsprechen und mit der neuen nicht gültig sind.

Azure Digital Twins geht dagegen nicht vor. Deshalb sollten Sie darauf achten, Twins richtig zu patchen, damit sie durch den Wechsel der Modelldefinition nicht ihre Gültigkeit verlieren.

## <a name="manage-models-with-cli"></a>Verwalten von Modellen mit der CLI

Modelle können auch mit der Azure Digital Twins-CLI verwaltet werden. Die Befehle finden Sie unter [*Vorgehensweise: Verwenden der Azure Digital Twins-Befehlszeilenschnittstelle*](how-to-use-cli.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Erstellen und Verwalten von digitalen Zwillingen basierend auf Ihren Modellen finden Sie unter
* [*Verwenden Verwalten digitaler Zwillinge*](how-to-manage-twin.md)