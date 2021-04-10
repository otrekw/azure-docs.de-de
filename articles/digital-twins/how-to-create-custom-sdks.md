---
title: Erstellen von SDKs mit benutzerdefinierter Sprache mithilfe von AutoRest
titleSuffix: Azure Digital Twins
description: Hier erfahren Sie, wie Sie mithilfe von AutoRest SDKs für eine benutzerdefinierte Sprache generieren, um Code für Azure Digital Twins in anderen Sprachen zu schreiben, für die bisher keine SDKs veröffentlicht wurden.
author: baanders
ms.author: baanders
ms.date: 3/9/2021
ms.topic: how-to
ms.service: digital-twins
ms.custom:
- devx-track-js
- contperf-fy21q3
ms.openlocfilehash: 35cf54199f8f2c187ad397c21fb941111f07c4a3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102561839"
---
# <a name="create-custom-language-sdks-for-azure-digital-twins-using-autorest"></a>Erstellen von SDKs für eine benutzerdefinierte Sprache für Azure Digital Twins mit AutoRest

Wenn Sie mit Azure Digital Twins arbeiten müssen und eine Sprache verwenden, für die es kein [veröffentlichtes Azure Digital Twins-SDK](how-to-use-apis-sdks.md) gibt, erfahren Sie in diesem Artikel, wie Sie mit AutoRest Ihr eigenes SDK in der Sprache Ihrer Wahl generieren können. 

In den Beispielen in diesem Artikel wird die Erstellung eines [Datenebenen-SDK](how-to-use-apis-sdks.md#overview-data-plane-apis) gezeigt. Mit dieser Vorgehensweise kann jedoch auch ein [Steuerungsebenen-SDK](how-to-use-apis-sdks.md#overview-control-plane-apis) generiert werden.

## <a name="prerequisites"></a>Voraussetzungen

Zum Generieren eines SDK müssen Sie zuerst die folgenden Einrichtungsschritte auf Ihrem lokalen Computer ausführen:
* Installieren Sie [**AutoRest**](https://github.com/Azure/autorest), Version 2.0.4413 (Version 3 wird derzeit nicht unterstützt).
* Installieren Sie [**Node.js**](https://nodejs.org). Dies ist eine Voraussetzung für die Verwendung von AutoRest.
* Installieren Sie [**Visual Studio**](https://visualstudio.microsoft.com/downloads/).
* Laden Sie im [Swagger-Ordner für die Datenebene](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/digitaltwins/data-plane/Microsoft.DigitalTwins) die neueste **Swagger-Datei für die Datenebene** von Azure Digital Twins (OpenAPI) sowie die zugehörigen Ordner mit Beispielen herunter. Die Swagger-Datei ist *digitaltwins.json*.

>[!TIP]
> Wenn Sie stattdessen ein **Steuerungsebenen-SDK** erstellen möchten, führen Sie die Schritte in diesem Artikel mit der neuesten **Swagger-Datei für die Steuerungsebene** (OpenAPI) im [Swagger-Ordner für die Steuerungsebene](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/digitaltwins/resource-manager/Microsoft.DigitalTwins/) statt dem für die Datenebene aus.

Wenn Ihr Computer über alles auf der obigen Liste verfügt, können Sie mit AutoRest ein SDK erstellen.

## <a name="create-the-sdk-using-autorest"></a>Erstellen des SDK mit AutoRest 

Wenn Sie Node.js installiert haben, können Sie den folgenden Befehl ausführen, um sicherzustellen, dass die richtige Version von AutoRest installiert ist:
```cmd/sh
npm install -g autorest@2.0.4413
```

Führen Sie die folgenden Schritte aus, um AutoRest für die Azure Digital Twins-Swagger-Datei auszuführen:
1. Kopieren Sie die Azure Digital Twins-Swagger-Datei und den zugehörigen Ordner mit Beispielen in ein Arbeitsverzeichnis.
2. Wechseln Sie in einem Eingabeaufforderungsfenster zu diesem Arbeitsverzeichnis.
3. Führen Sie AutoRest mithilfe des folgenden Befehls aus. Ersetzen Sie den `<language>`-Platzhalter durch Ihre bevorzugte Sprache: `python`, `java`, `go` usw. (Die vollständige Liste der Optionen finden Sie in der [AutoRest-Infodatei](https://github.com/Azure/autorest).)

```cmd/sh
autorest --input-file=digitaltwins.json --<language> --output-folder=DigitalTwinsApi --add-credentials --azure-arm --namespace=DigitalTwinsApi
```

Daraufhin wird in Ihrem Arbeitsverzeichnis ein neuer Ordner mit dem Namen *DigitalTwinsApi* angezeigt. Die generierten SDK-Dateien haben den Namespace *DigitalTwinsApi*. Sie verwenden diesen Namespace auch in den restlichen Verwendungsbeispielen in diesem Artikel.

AutoRest unterstützt eine breite Palette von Sprachcodegeneratoren.

## <a name="make-the-sdk-into-a-class-library"></a>Erstellen einer auf dem SDK basierenden Klassenbibliothek

Sie können die von AutoRest generierten Dateien direkt in eine .NET-Projektmappe einschließen. Wahrscheinlich möchten Sie jedoch das Azure Digital Twins-SDK in mehrere separate Projekte einbinden (z. B. Ihre Client-Apps und Azure Functions-Apps). Aus diesem Grund kann es hilfreich sein, ein separates Projekt (eine .NET-Klassenbibliothek) aus den generierten Dateien zu erstellen. Sie können dieses Klassenbibliotheksprojekt dann als Projektverweis in mehrere Projektmappen einbinden.

Dieser Abschnitt enthält Anweisungen zum Erstellen des SDK als Klassenbibliothek, wobei es sich um ein eigenes Projekt handelt, das in andere Projekte eingebunden werden kann. Für die Schritte in diesem Abschnitt benötigen Sie **Visual Studio**.

Im Folgenden werden die Schritte aufgeführt:

1. Erstellen einer neuen Visual Studio-Projektmappe für eine Klassenbibliothek
2. Verwenden von *DigitalTwinsApi* als Projektname
3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt *DigitalTwinsApi* der generierten Projektmappe, und wählen Sie *Hinzufügen > Vorhandenes Element...* aus.
4. Suchen Sie den Ordner, in dem Sie das SDK generiert haben, und wählen Sie die Dateien auf der Stammebene aus.
5. Klicken Sie auf „OK“.
6. Fügen Sie dem Projekt einen Ordner hinzu (klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie *Hinzufügen > Neuer Ordner*).
7. Geben Sie dem Ordner den Namen *Models*.
8. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner *Models*, und wählen Sie *Hinzufügen > Vorhandenes Element* aus.
9. Wählen Sie die Dateien im Ordner *Models* des generierten SDK aus, und klicken Sie auf „OK“.

Um das SDK erfolgreich zu erstellen, benötigt das Projekt die folgenden Verweise:
* `Microsoft.Rest.ClientRuntime`
* `Microsoft.Rest.ClientRuntime.Azure`

Um diese Verweise hinzuzufügen, öffnen Sie *Tools > NuGet-Paket-Manager > NuGet-Pakete für Projektmappe verwalten*.

1. Vergewissern Sie sich im Bereich, dass die Registerkarte *Durchsuchen* ausgewählt ist.
2. Suchen Sie nach *Microsoft.Rest*.
3. Wählen Sie die `ClientRuntime`- und `ClientRuntime.Azure`-Pakete aus, und fügen Sie sie der Projektmappe hinzu.

Sie können nun das Projekt erstellen und als Projektverweis in eine beliebige Azure Digital Twins-Anwendung einschließen, die Sie schreiben.

## <a name="tips-for-using-the-sdk"></a>Tipps für die Verwendung des SDK

Dieser Abschnitt enthält allgemeine Informationen und Richtlinien zur Verwendung des generierten SDK.

### <a name="synchronous-and-asynchronous-calls"></a>Synchrone und asynchrone Aufrufe

Alle SDK-Funktionen sind in synchronen und asynchronen Versionen enthalten.

### <a name="typed-and-untyped-data"></a>Typisierte und nicht typisierte Daten

REST-API-Aufrufe geben im Allgemeinen stark typisierte Objekte zurück. Da Benutzer jedoch mit Azure Digital Twins eigene benutzerdefinierte Typen für Zwillinge definieren können, gibt es keine Möglichkeit, statische Rückgabedaten für viele Aufrufe von Azure Digital Twins vorab zu definieren. Stattdessen geben die APIs stark typisierte Wrappertypen zurück, sofern zutreffend, und die benutzertypisierten Zwillingsdaten befinden sich in Json.NET-Objekten (wird überall dort verwendet, wo der Datentyp „object" in den API-Signaturen angezeigt wird). Sie können diese Objekte entsprechend in Ihren Code umwandeln.

### <a name="error-handling"></a>Fehlerbehandlung

Wenn ein Fehler im SDK auftritt (einschließlich HTTP-Fehlern wie z. B. 404), löst das SDK eine Ausnahme aus. Aus diesem Grund ist es wichtig, alle API-Aufrufe in try/catch-Blöcken zu kapseln.

Hier sehen Sie einen Codeausschnitt, der versucht, einen Zwilling hinzuzufügen und Fehler in diesem Prozess abfängt:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_other.cs" id="CreateTwin_errorHandling":::

### <a name="paging"></a>Paging

AutoRest generiert zwei Arten von Pagingmustern für das SDK:
* Eine für alle APIs außer der Abfrage-API
* Eine für die Abfrage-API

Im nicht abfragenden Pagingmuster zeigt die folgende Beispielmethode, wie Sie eine ausgelagerte Liste von ausgehenden Beziehungen von Azure Digital Twins abrufen:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/graph_operations_sample.cs" id="FindOutgoingRelationshipsMethod":::

Das zweite Muster wird nur für die Abfrage-API generiert. Es verwendet explizit ein `continuationToken`.

>[!TIP]
> Ein Hauptgrund für das Abrufen von Seiten ist die Berechnung der [Gebühren für Abfrageeinheiten](concepts-query-units.md) für einen Abfrage-API-Befehl.

Hier ist ein Beispiel mit diesem Muster:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/queries.cs" id="PagedQuery":::

## <a name="next-steps"></a>Nächste Schritte

Führen Sie die Schritte zum Erstellen einer Client-App aus, in der Sie Ihr SDK verwenden können:
* [*Tutorial: Codieren einer Client-App*](tutorial-code.md)
