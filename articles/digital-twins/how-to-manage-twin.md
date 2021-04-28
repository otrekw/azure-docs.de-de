---
title: Verwalten digitaler Zwillinge
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie einzelne Zwillinge und Beziehungen abrufen, aktualisieren und löschen.
author: baanders
ms.author: baanders
ms.date: 10/21/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 02ea87bba8c28f0217479ffc65ccc65903fc0442
ms.sourcegitcommit: b4032c9266effb0bf7eb87379f011c36d7340c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107903036"
---
# <a name="manage-digital-twins"></a>Verwalten digitaler Zwillinge

Entitäten in Ihrer Umgebung werden durch [digital Zwillinge](concepts-twins-graph.md) dargestellt. Die Verwaltung digitaler Zwillinge kann das Erstellen, Ändern und Löschen umfassen. Für diese Vorgänge können Sie die [**Digital Twins-APIs**](/rest/api/digital-twins/dataplane/twins), das [.NET (C#) SDK](/dotnet/api/overview/azure/digitaltwins/client) oder die [Azure Digital Twins-Befehlszeilenschnittstelle](how-to-use-cli.md) verwenden.

In diesem Artikel wird die Verwaltung digitaler Zwillinge beschrieben. Informationen zum Arbeiten mit Beziehungen und dem [Zwillingsgraph](concepts-twins-graph.md) insgesamt finden Sie unter [*Gewusst wie: Verwalten des Zwillingsgraphen mit Beziehungen*](how-to-manage-graph.md).

> [!TIP]
> Alle SDK-Funktionen sind in synchronen und asynchronen Versionen enthalten.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [digital-twins-prereq-instance.md](../../includes/digital-twins-prereq-instance.md)]

## <a name="ways-to-manage-twins"></a>Möglichkeiten zum Verwalten von Zwillingen

[!INCLUDE [digital-twins-ways-to-manage.md](../../includes/digital-twins-ways-to-manage.md)]

## <a name="create-a-digital-twin"></a>Erstellen eines digitalen Zwillings

Wenn Sie einen Zwilling erstellen möchten, verwenden Sie die `CreateOrReplaceDigitalTwinAsync()`-Methode auf dem Serviceclient wie folgt:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_sample.cs" id="CreateTwinCall":::

Zum Erstellen eines digitalen Zwillings müssen Sie Folgendes angeben:
* Die gewünschte ID für den digitalen Zwilling
* Das zu verwendende [Modell](concepts-models.md)

Optional können Sie Anfangswerte für alle Eigenschaften des digitalen Zwillings bereitstellen. Eigenschaften werden als optional behandelt und können später festgelegt werden. **Sie werden jedoch erst als Teil eines Zwillings angezeigt, nachdem sie festgelegt wurden.**

>[!NOTE]
>Obwohl Zwillingseigenschaften nicht initialisiert werden müssen, **müssen** alle [Komponenten](concepts-models.md#elements-of-a-model) auf dem Zwilling beim Erstellen des Zwillings festgelegt werden. Es kann sich um leere Objekte handeln, aber die Komponenten selbst müssen vorhanden sein.

Die Modell- und alle ursprünglichen Eigenschaftswerte werden über den `initData`-Parameter bereitgestellt. Dabei handelt es sich um eine JSON-Zeichenfolge, die die relevanten Daten enthält. Weitere Informationen zum Strukturieren dieses Objekts finden Sie im nächsten Abschnitt.

> [!TIP]
> Nachdem Sie einen Zwilling erstellt oder aktualisiert haben, kann es bis zu 10 Sekunden dauern, bis sich die Änderungen in [Abfragen](how-to-query-graph.md) widerspiegeln. Bei der `GetDigitalTwin`-API (siehe [weiter unten in diesem Artikel](#get-data-for-a-digital-twin)) tritt diese Verzögerung nicht auf. Wenn Sie eine sofortige Antwort benötigen, verwenden Sie daher den API-Befehl anstelle von Abfragen, um die neu erstellten Zwillinge anzuzeigen. 

### <a name="initialize-model-and-properties"></a>Initialisieren des Modells und der Eigenschaften

Sie können die Eigenschaften eines Zwillings zum Zeitpunkt der Erstellung des Zwillings initialisieren. 

Die Zwillingserstellungs-API akzeptiert ein Objekt, das in eine gültige JSON-Beschreibung der Zwillingseigenschaften serialisiert wird. Unter [*Konzepte: digitale Zwillinge und das Zwillingsdiagramm*](concepts-twins-graph.md) finden Sie eine Beschreibung des JSON-Formats für einen Zwilling. 

Zunächst können Sie ein Datenobjekt erstellen, das den Zwilling und seine Eigenschaftsdaten darstellt. Sie können ein Parameterobjekt entweder manuell oder mithilfe einer bereitgestellten Hilfsklasse erstellen. Im Folgenden finden Sie Beispiele für beides.

#### <a name="create-twins-using-manually-created-data"></a>Erstellen von Zwillingen mithilfe manuell erstellter Daten

Sie können die Eigenschaften eines Zwillings ohne benutzerdefinierte Hilfsklassen in `Dictionary<string, object>` darstellen, wobei `string` der Name der Eigenschaft ist und `object` ein Objekt, das die Eigenschaft und ihren Wert darstellt.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_other.cs" id="CreateTwin_noHelper":::

#### <a name="create-twins-with-the-helper-class"></a>Erstellen von Zwillingen mithilfe der Hilfsklasse

Die Hilfsklasse `BasicDigitalTwin` ermöglicht es Ihnen, Eigenschaftsfelder in einem Zwillingsobjekt direkt zu speichern. Sie sollten dennoch die Liste der Eigenschaften mit `Dictionary<string, object>` erstellen, was dann dem Zwillingsobjekt direkt als `CustomProperties` hinzugefügt werden kann.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_sample.cs" id="CreateTwin_withHelper":::

>[!NOTE]
> `BasicDigitalTwin`-Objekte verfügen über ein `Id`-Feld. Sie können dieses Feld leer lassen. Wenn Sie einen ID-Wert hinzufügen, muss dieser jedoch dem an den `CreateOrReplaceDigitalTwinAsync()`-Aufruf übergebenen ID-Parameter entsprechen. Zum Beispiel:
>
>```csharp
>twin.Id = "myRoomId";
>```

## <a name="get-data-for-a-digital-twin"></a>Abrufen von Daten für einen digitalen Zwilling

Sie können auf die Details jedes digitalen Zwillings zugreifen, indem Sie die Methode `GetDigitalTwin()` wie folgt aufrufen:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_sample.cs" id="GetTwinCall":::

Dieser Aufruf gibt Zwillingsdaten als einen stark typisierten Objekttyp (z. B. `BasicDigitalTwin`) zurück. `BasicDigitalTwin` ist eine Serialisierungshilfsklasse, die im SDK enthalten ist und die die zentralen Zwillingsmetadaten und -eigenschaften in einem vorab analysierten Format zurückgibt. Hier folgt ein Beispiel zur Vorgehensweise beim Anzeigen der Zwillingsdetails:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_sample.cs" id="GetTwin" highlight="2":::

Beim Abrufen eines Zwillings mit der Methode `GetDigitalTwin()` werden nur Eigenschaften zurückgegeben, die mindestens einmal festgelegt wurden.

>[!TIP]
>Der `displayName` für einen Zwilling ist ein Teil seiner Modellmetadaten und wird daher nicht angezeigt, wenn Daten für die Zwillingsinstanz abgerufen werden. Um diesen Wert zu sehen, können Sie [ihn aus dem Modell abrufen](how-to-manage-model.md#retrieve-models).

Informationen zum Abrufen mehrerer Zwillinge mithilfe eines einzelnen API-Aufrufs finden Sie in den Beispielen zur Abfrage-API unter [*Gewusst wie: des Zwillingsdiagramms von Azure Digital Twins*](how-to-query-graph.md).

Beachten Sie das folgende Modell (geschrieben in [Digital Twins Definition Language (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl/tree/master/DTDL)), das einen *Moon* definiert:

:::code language="json" source="~/digital-twins-docs-samples/models/Moon.json":::

Das Ergebnis des Aufrufs von `object result = await client.GetDigitalTwinAsync("my-moon");` für einen Zwilling vom Typ *Moon* könnte wie folgt aussehen:

```json
{
  "$dtId": "myMoon-001",
  "$etag": "W/\"e59ce8f5-03c0-4356-aea9-249ecbdc07f9\"",
  "radius": 1737.1,
  "mass": 0.0734,
  "$metadata": {
    "$model": "dtmi:example:Moon;1",
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

Die definierten Eigenschaften des digitalen Zwillings werden als Eigenschaften der obersten Ebene für den digitalen Zwilling zurückgegeben. Metadaten oder Systeminformationen, die nicht Teil der DTDL-Definition sind, werden mit einem `$`-Präfix zurückgegeben. Metadateneigenschaften umfassen die folgenden Werte:
* `$dtId`: ID des digitalen Zwillings in dieser Azure Digital Twins-Instanz
* `$etag`: ein vom Webserver zugewiesenes Standard-HTTP-Feld. Dieser Wert wird bei jeder Aktualisierung des Zwillings geändert. Er kann damit hilfreich sein, um zu ermitteln, ob die Daten des Zwillings seit einer früheren Überprüfung auf dem Server aktualisiert wurden. Sie können `If-Match` verwenden, um Aktualisierungen und Löschungen durchführen, die nur dann abgeschlossen werden, wenn der Etag mit dem bereitgestellten Etag übereinstimmt. Weitere Informationen zu diesen Vorgängen finden Sie in der Dokumentation zu [DigitalTwins Update](/rest/api/digital-twins/dataplane/twins/digitaltwins_update) und [DigitalTwins Delete](/rest/api/digital-twins/dataplane/twins/digitaltwins_delete).
* `$metadata`: verschiedene andere Eigenschaften, einschließlich:
  - Der DTMI des Modells für den digitalen Zwilling
  - Der Synchronisierungsstatus für jede schreibbare Eigenschaft. Dies ist besonders hilfreich für Geräte, bei denen es möglich ist, dass der Dienst und das Gerät abweichende Status aufweisen (z. B. wenn ein Gerät offline ist). Derzeit gilt diese Eigenschaft nur für physische Geräte, die mit IoT Hub verbunden sind. Mit den Daten im Metadatenabschnitt erfahren Sie den vollständigen Status einer Eigenschaft sowie die Zeitstempel der letzten Änderung. Weitere Informationen zum Synchronisierungsstatus erhalten Sie in [diesem IoT Hub-Tutorial](../iot-hub/tutorial-device-twins.md) zum Synchronisieren des Gerätestatus.
  - Dienstspezifische Metadaten, wie z. B. aus IoT Hub oder Azure Digital Twins 

Weitere Informationen zu den Serialisierungshilfsklassen wie `BasicDigitalTwin` finden Sie unter [ *Verwenden der Azure Digital Twins-APIs und SDKs*](how-to-use-apis-sdks.md).

## <a name="view-all-digital-twins"></a>Anzeigen aller digitalen Zwillinge

Verwenden Sie zum Anzeigen aller digitalen Zwillinge in Ihrer Instanz eine [Abfrage](how-to-query-graph.md). Sie können eine Abfrage mit den [Abfrage-APIs](/rest/api/digital-twins/dataplane/query) oder den [CLI-Befehlen](how-to-use-cli.md) ausführen.

Im Folgenden finden Sie den Text der grundlegenden Abfrage, die eine Liste aller digitalen Zwillinge in der Instanz zurückgibt:

:::code language="sql" source="~/digital-twins-docs-samples/queries/examples.sql" id="GetAllTwins":::

## <a name="update-a-digital-twin"></a>Aktualisieren eines digitalen Zwillings

Um die Eigenschaften eines digitalen Zwillings zu aktualisieren, schreiben Sie die Informationen, die Sie ersetzen möchten, im [JSON Patch](http://jsonpatch.com/)-Format. So können Sie mehrere Eigenschaften gleichzeitig ersetzen. Anschließend übergeben Sie das JSON Patch-Dokument an eine `UpdateDigitalTwin()`-Methode:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_sample.cs" id="UpdateTwinCall":::

Ein Patchaufruf kann beliebig viele Eigenschaften auf einem einzelnen Zwilling aktualisieren (sogar alle). Wenn Sie Eigenschaften für mehrere Zwillinge aktualisieren müssen, benötigen Sie für jeden Zwilling einen separaten Updateaufruf.

> [!TIP]
> Nachdem Sie einen Zwilling erstellt oder aktualisiert haben, kann es bis zu 10 Sekunden dauern, bis sich die Änderungen in [Abfragen](how-to-query-graph.md) widerspiegeln. Bei der `GetDigitalTwin`-API (siehe [weiter oben in diesem Artikel](#get-data-for-a-digital-twin)) tritt diese Verzögerung nicht auf. Verwenden Sie daher den API-Befehl anstelle von Abfragen, um die neu aktualisierten Zwillinge anzuzeigen, wenn Sie eine sofortige Antwort benötigen. 

Im Folgenden finden Sie ein Beispiel für JSON Patch-Code. Dieses Dokument ersetzt die Eigenschaftswerte *mass* und *radius* des digitalen Zwillings, auf den es angewendet wird.

:::code language="json" source="~/digital-twins-docs-samples/models/patch.json":::

Patches können mithilfe von [JsonPatchDocument](/dotnet/api/azure.jsonpatchdocument) des Azure .NET SDK erstellt werden. Beispiel:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_other.cs" id="UpdateTwin":::

### <a name="update-properties-in-digital-twin-components"></a>Aktualisieren von Eigenschaften in Komponenten digitaler Zwillinge

Denken Sie daran, dass ein Modell Komponenten enthalten kann, sodass es aus anderen Modellen besteht. 

Zum Patchen von Eigenschaften in den Komponenten eines digitalen Zwillings können Sie die Pfadsyntax im JSON-Patch verwenden:

:::code language="json" source="~/digital-twins-docs-samples/models/patch-component.json":::

### <a name="update-a-digital-twins-model"></a>Aktualisieren eines Modells eines digitalen Zwillings

Die `UpdateDigitalTwin()`-Funktion kann auch verwendet werden, um einen digitalen Zwilling zu einem anderen Modell zu migrieren. 

Sehen Sie sich beispielsweise das folgende JSON Patch-Dokument an, das das `$model`-Metadatenfeld des digitalen Zwillings ersetzt:

:::code language="json" source="~/digital-twins-docs-samples/models/patch-model-1.json":::

Dieser Vorgang ist nur erfolgreich, wenn der vom Patch geänderte digitale Zwilling dem neuen Modell entspricht. 

Betrachten Sie das folgenden Beispiel:
1. Stellen Sie sich einen digitalen Zwilling mit dem Modell *foo_old* vor. *foo_old* definiert die erforderliche *mass*-Eigenschaft.
2. Das neue Modell *foo_new* definiert die Eigenschaften „mass“ und fügt die neue erforderliche Eigenschaft *temperature* hinzu.
3. Nach dem Patch muss der digitale Zwilling die beiden Eigenschaften „mass“ und „temperature“ aufweisen. 

Der Patch für diese Situation muss sowohl das Modell als auch die Eigenschaft „temperature“ des Zwillings wie folgt aktualisieren:

:::code language="json" source="~/digital-twins-docs-samples/models/patch-model-2.json":::

### <a name="handle-conflicting-update-calls"></a>Verarbeitung von in Konflikt stehenden Updateaufrufen

Azure Digital Twins stellt sicher, dass alle eingehenden Anforderungen nacheinander verarbeitet werden. Dies bedeutet, dass Sie selbst dann, wenn mehrere Funktionen versuchen, dieselbe Eigenschaft für einen Zwilling gleichzeitig zu aktualisieren, **keinen** expliziten Sperrcode schreiben müssen, um den Konflikt zu beheben.

Dieses Verhalten erfolgt für jeden Zwilling einzeln. 

Stellen Sie sich beispielsweise ein Szenario vor, in dem die folgenden drei Aufrufe gleichzeitig eingehen: 
*   Schreiben von Eigenschaft A auf *Zwilling1*
*   Schreiben von Eigenschaft B auf *Zwilling1*
*   Schreiben von Eigenschaft A auf *Zwilling2*

Die beiden Aufrufe, die *Zwilling1* ändern, werden nacheinander ausgeführt, und für jede Änderung werden Änderungsnachrichten generiert. Der Aufruf zum Ändern von *Zwilling2* kann ohne Konflikt zeitgleich ausgeführt werden, sobald er eingeht.

## <a name="delete-a-digital-twin"></a>Löschen eines digitalen Zwillings

Sie können Zwillinge mithilfe der `DeleteDigitalTwin()`-Methode löschen. Sie können einen Zwilling jedoch nur löschen, wenn er keine weiteren Beziehungen aufweist. Daher löschen Sie zuerst die ein- und ausgehenden Beziehungen des Zwillings.

Hier folgt ein Beispiel für den Code zum Löschen von Zwillingen und ihren Beziehungen. Der `DeleteDigitalTwin`-SDK-Aufruf ist hervorgehoben, um zu verdeutlichen, inwiefern er Teil des breiteren Beispielkontexts ist.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_sample.cs" id="DeleteTwin" highlight="7":::

### <a name="delete-all-digital-twins"></a>Löschen aller digitalen Zwillinge

Ein Beispiel für das gleichzeitige Löschen aller Zwillinge finden Sie in der Beispiel-App im [*Tutorial: Erkunden von Azure Digital Twins mit einer Beispielclient-App*](tutorial-command-line-app.md). In der Datei *CommandLoop.cs* wird dies in einer `CommandDeleteAllTwins()`-Funktion durchgeführt.

## <a name="runnable-digital-twin-code-sample"></a>Ausführbares Codebeispiel für digitale Zwillinge

Sie können das folgende ausführbare Codebeispiel verwenden, um einen Zwilling zu erstellen, seine Details zu aktualisieren und den Zwilling zu löschen. 

### <a name="set-up-the-runnable-sample"></a>Einrichten des ausführbaren Beispiels

Der Codeausschnitt verwendet die [Room.json](https://github.com/Azure-Samples/digital-twins-samples/blob/master/AdtSampleApp/SampleClientApp/Models/Room.json)-Modelldefinition aus dem [*Tutorial: Erkunden von Azure Digital Twins mit einer Beispielclient-App*](tutorial-command-line-app.md). Sie können diesen Link verwenden, um direkt zu der Datei zu wechseln, oder sie als Teil des vollständigen End-to-End-Beispielprojekts [hier](/samples/azure-samples/digital-twins-samples/digital-twins-samples/) herunterladen.

Bevor Sie das Beispiel ausführen, gehen Sie wie folgt vor:
1. Laden Sie die Modelldatei herunter, platzieren Sie sie in Ihrem Projekt, und ersetzen Sie den Platzhalter `<path-to>` im Code unten, um Ihrem Programm mitzuteilen, wo sie zu finden ist.
2. Ersetzen Sie den Platzhalter `<your-instance-hostname>` durch den Hostnamen Ihrer Azure Digital Twins-Instanz.
3. Als Nächstes fügen Sie Ihrem Projekt zwei Abhängigkeiten hinzu, die erforderlich sind, um Azure Digital Twins verwenden zu können. Bei der ersten handelt es sich um das Paket für das [Azure Digital Twins-SDK für .NET](/dotnet/api/overview/azure/digitaltwins/client), die zweite stellt Tools bereit, die die Authentifizierung bei Azure unterstützen.

      ```cmd/sh
      dotnet add package Azure.DigitalTwins.Core
      dotnet add package Azure.Identity
      ```

Sie müssen auch lokale Anmeldeinformationen einrichten, wenn Sie das Beispiel direkt ausführen möchten. Im nächsten Abschnitt werden diese Schritte erläutert.
[!INCLUDE [Azure Digital Twins: local credentials prereq (outer)](../../includes/digital-twins-local-credentials-outer.md)]

### <a name="run-the-sample"></a>Ausführen des Beispiels

Nachdem Sie die obigen Schritte ausgeführt haben, können Sie den folgenden Beispielcode direkt ausführen.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_sample.cs":::

Hier ist die Konsolenausgabe des obigen Programms: 

:::image type="content" source="./media/how-to-manage-twin/console-output-manage-twins.png" alt-text="Konsolenausgabe, die zeigt, dass der Zwilling erstellt, aktualisiert und gelöscht wurde" lightbox="./media/how-to-manage-twin/console-output-manage-twins.png":::

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie Beziehungen zwischen Ihren digitalen Zwillingen erstellen und verwalten können:
* [*Verwenden Verwalten des Zwillingsgraphen mit Beziehungen*](how-to-manage-graph.md)