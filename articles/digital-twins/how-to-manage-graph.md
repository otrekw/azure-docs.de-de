---
title: Verwalten des Zwillingsgraphen mit Beziehungen
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie einen Graphen von digitalen Zwillingen verwalten, indem Sie sie über Beziehungen verbinden.
author: baanders
ms.author: baanders
ms.date: 11/03/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 037e7fd13f55a0f5de939197f71324221392bd55
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98601063"
---
# <a name="manage-a-graph-of-digital-twins-using-relationships"></a>Verwalten eines Graphen von digitalen Zwillingen mithilfe von Beziehungen

Das Herzstück von Azure Digital Twins ist der [Zwillingsgraph](concepts-twins-graph.md), der Ihre gesamte Umgebung darstellt. Der Zwillingsgraph setzt sich aus einzelnen digitalen Zwillingen zusammen, die über **Beziehungen** verbunden sind. 

Sobald Sie über eine funktionierende [Azure Digital Twins-Instanz](how-to-set-up-instance-portal.md) verfügen und [Authentifizierungscode](how-to-authenticate-client.md) in Ihrer Client-App eingerichtet haben, können Sie die [**DigitalTwins-APIs**](/rest/api/digital-twins/dataplane/twins) verwenden, um digitale Zwillinge und ihre Beziehungen in einer Azure Digital Twins-Instanz zu erstellen, zu ändern und zu löschen. Sie können auch das [.NET (C#) SDK](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true) oder die [Azure Digital Twins-CLI](how-to-use-cli.md) verwenden.

Dieser Artikel konzentriert sich auf die Verwaltung von Beziehungen und den Graphen als Ganzes. Für die Arbeit mit einzelnen digitalen Zwillingen finden Sie weitere Informationen unter [ *Verwalten digitaler Zwillinge*](how-to-manage-twin.md).

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [digital-twins-prereq-instance.md](../../includes/digital-twins-prereq-instance.md)]

## <a name="ways-to-manage-graph"></a>Möglichkeiten zum Verwalten von Graphen

[!INCLUDE [digital-twins-ways-to-manage.md](../../includes/digital-twins-ways-to-manage.md)]

Sie können den Graphen auch mit dem Beispiel „ADT Explorer“ (Azure Digital Twins Explorer) ändern, mit dem Sie Ihre Zwillinge und Graphen visualisieren und das SDK im Hintergrund verwenden können. Im nächsten Abschnitt wird dieses Beispiel ausführlich beschrieben.

[!INCLUDE [visualizing with Azure Digital Twins explorer](../../includes/digital-twins-visualization.md)]

## <a name="create-relationships"></a>Erstellen von Beziehungen

Beziehungen beschreiben, wie verschiedene digitale Zwillinge miteinander verbunden sind, was die Grundlage des Zwillingsgraphen bildet.

Beziehungen werden mit dem `CreateOrReplaceRelationshipAsync()`-Aufruf erstellt. 

Sie müssen Folgendes angeben, um eine Beziehung zu erstellen:
* Die ID des Quellzwillings (`srcId` im nachfolgenden Codebeispiel): Die ID des Zwillings, von dem die Beziehung ausgeht.
* Die ID des Zielzwillings (`targetId` im nachfolgenden Codebeispiel): Die ID des Zwillings, an den die Beziehung gerichtet ist.
* Ein Beziehungsname (`relName` im nachfolgenden Codebeispiel): Der generische Typ der Beziehung, z. B. _Enthält_.
* Eine Beziehungs-ID (`relId` im nachfolgenden Codebeispiel): Der spezifische Name für diese Beziehung, z. B. _Beziehung1_.

Die Beziehungs-ID muss innerhalb des angegebenen Quellzwillings eindeutig sein. Sie muss nicht global eindeutig sein.
Für den Zwilling *foo* muss z. B. jede bestimmte Beziehungs-ID eindeutig sein. Ein weiterer Zwilling *bar* kann jedoch eine ausgehende Beziehung aufweisen, die derselben ID einer *foo*-Beziehung entspricht.

Im folgenden Codebeispiel wird veranschaulicht, wie Sie in Ihrer Azure Digital Twins-Instanz eine Beziehung erstellen.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/graph_operations_sample.cs" id="CreateRelationshipMethod":::

In Ihrer Hauptmethode können Sie jetzt die `CreateRelationship()`-Funktion aufrufen, um eine _Enthält_-Beziehung wie die folgende zu erstellen: 

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/graph_operations_sample.cs" id="UseCreateRelationship":::

Wenn Sie mehrere Beziehungen erstellen möchten, können Sie Aufrufe derselben Methode wiederholen und dabei verschiedene Beziehungstypen an das Argument übergeben. 

Weitere Informationen zur Hilfsklasse `BasicRelationship` finden Sie unter [ *Verwenden der Azure Digital Twins-APIs und SDKs*](how-to-use-apis-sdks.md#serialization-helpers).

### <a name="create-multiple-relationships-between-twins"></a>Erstellen mehrerer Beziehungen zwischen Zwillingen

Beziehungen können wie folgt klassifiziert werden: 

* Ausgehende Beziehungen: Beziehungen, die zu diesem Zwilling gehören, die nach außen gerichtet sind, um eine Verbindung mit anderen Zwillingen herzustellen. Die Methode `GetRelationshipsAsync()` wird verwendet, um ausgehende Beziehungen eines Zwillings abzurufen.
* Eingehende Beziehungen: Beziehungen, die zu anderen Zwillingen gehören, die auf diesen Zwilling verweisen, um eine „eingehende“ Verbindung zu erstellen. Die Methode `GetIncomingRelationshipsAsync()` wird verwendet, um eingehende Beziehungen eines Zwillings abzurufen.

Zwischen Zwillingen können beliebig viele Beziehungen vorhanden sein. 

Das bedeutet, Sie können gleichzeitig verschiedene Arten von Beziehungen zwischen zwei Zwillingen ausdrücken. Ein Beispiel: *Zwilling A* kann sowohl eine *gespeicherte* als auch eine *erstellte* Beziehung mit *Zwilling B* haben.

Auf Wunsch können sogar mehrere Instanzen der gleichen Art von Beziehung zwischen den gleichen beiden Zwillingen erstellt werden. In diesem Beispiel könnte *Zwilling A* zwei verschiedene *gespeicherte* Beziehungen mit *Zwilling B* aufweisen, solange die Beziehungen unterschiedliche Beziehungs-IDs verwenden.

## <a name="list-relationships"></a>Auflisten von Beziehungen

Sie können die Methode `GetRelationships()` wie folgt verwenden, um auf die Liste der **ausgehenden** Beziehungen für einen bestimmten Zwilling im Graphen zuzugreifen:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/graph_operations_sample.cs" id="GetRelationshipsCall":::

Dies gibt ein `Azure.Pageable<T>` oder `Azure.AsyncPageable<T>` zurück, je nachdem, ob Sie die synchrone oder asynchrone Version des Aufrufs verwenden.

Im Folgenden finden Sie ein Beispiel, das eine Liste von Beziehungen abruft:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/graph_operations_sample.cs" id="FindOutgoingRelationshipsMethod":::

Sie können diese Methode jetzt aufrufen, um die ausgehenden Beziehungen der Zwillinge wie folgt anzuzeigen:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/graph_operations_sample.cs" id="UseFindOutgoingRelationships":::

Sie können die abgerufenen Beziehungen verwenden, um zu anderen Zwillingen in Ihrem Graphen zu navigieren. Lesen Sie dazu das Feld `target` aus der zurückgegebenen Beziehung, und verwenden Sie es als ID für Ihren nächsten Aufruf von `GetDigitalTwin()`.

### <a name="find-incoming-relationships-to-a-digital-twin"></a>Suchen von eingehenden Beziehungen zu einem digitalen Zwilling

Azure Digital Twins verfügt auch über eine API, um alle **eingehenden** Beziehungen für einen bestimmten Zwilling zu finden. Dies ist oft nützlich bei der umgekehrten Navigation oder beim Löschen eines Zwillings.

Das Codebeispiel oben hat sich auf die Suche nach ausgehenden Beziehungen von einem Zwilling konzentriert. Das folgende Beispiel ist ähnlich strukturiert, findet aber stattdessen *eingehende* Beziehungen vom Zwilling.

Beachten Sie, dass die `IncomingRelationship`-Aufrufe nicht den vollständigen Text der Beziehung zurückgeben.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/graph_operations_sample.cs" id="FindIncomingRelationshipsMethod":::

Sie können diese Methode jetzt aufrufen, um die eingehenden Beziehungen der Zwillinge wie folgt anzuzeigen:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/graph_operations_sample.cs" id="UseFindIncomingRelationships":::

### <a name="list-all-twin-properties-and-relationships"></a>Auflisten aller Zwillingseigenschaften und -beziehungen

Mithilfe der oben genannten Methoden zur Auflistung der aus- und eingehenden Beziehungen zu einem Zwilling können Sie eine Methode erstellen, die vollständige Zwillingsinformationen ausgibt, einschließlich der Eigenschaften des Zwillings und beider Arten seiner Beziehungen. Hier folgt eine Beispielmethode namens `FetchAndPrintTwinAsync()`, die die Vorgehensweise veranschaulicht.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/graph_operations_sample.cs" id="FetchAndPrintMethod":::

Sie können diese Funktion jetzt in Ihrer Hauptmethode wie folgt aufrufen: 

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/graph_operations_sample.cs" id="UseFetchAndPrint":::

## <a name="delete-relationships"></a>Löschen von Beziehungen

Der erste Parameter gibt den Quellzwilling an (der Zwilling, von dem die Beziehung ausgeht). Der andere Parameter ist die Beziehungs-ID. Sie benötigen sowohl die Zwillings-ID als auch die Beziehungs-ID, da Beziehungs-IDs nur innerhalb des Bereichs eines Zwillings eindeutig sind.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/graph_operations_sample.cs" id="DeleteRelationshipMethod":::

Sie können jetzt diese Methode aufrufen, um eine Beziehung wie folgt zu löschen:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/graph_operations_sample.cs" id="UseDeleteRelationship":::

## <a name="runnable-twin-graph-sample"></a>Ausführbares Zwillingsgraphenbeispiel

Der folgende ausführbare Codeausschnitt verwendet die Beziehungsvorgänge aus diesem Artikel, um einen Zwillingsgraphen aus digitalen Zwillingen und Beziehungen zu erstellen.

### <a name="set-up-the-runnable-sample"></a>Einrichten des ausführbaren Beispiels

Der Codeausschnitt verwendet die Modelldefinitionen [*Room.json*](https://github.com/Azure-Samples/digital-twins-samples/blob/master/AdtSampleApp/SampleClientApp/Models/Room.json) und [*Floor.json*](https://github.com/azure-Samples/digital-twins-samples/blob/master/AdtSampleApp/SampleClientApp/Models/Floor.json) aus dem [*Tutorial: Erkunden von Azure Digital Twins mit einer Beispielclient-App*](tutorial-command-line-app.md). Sie können diese Links verwenden, um direkt zu der Datei zu wechseln, oder sie als Teil des vollständigen End-to-End-Beispielprojekts [hier](/samples/azure-samples/digital-twins-samples/digital-twins-samples/) herunterladen. 

Bevor Sie das Beispiel ausführen, gehen Sie wie folgt vor:
1. Laden Sie die Modelldatei herunter, platzieren Sie sie in Ihrem Projekt, und ersetzen Sie den Platzhalter `<path-to>` im Code unten, um Ihrem Programm mitzuteilen, wo sie zu finden sind.
2. Ersetzen Sie den Platzhalter `<your-instance-hostname>` durch den Hostnamen Ihrer Azure Digital Twins-Instanz.
3. Als Nächstes fügen Sie Ihrem Projekt zwei Abhängigkeiten hinzu, die erforderlich sind, um Azure Digital Twins verwenden zu können. Bei der ersten handelt es sich um das Paket für das [Azure Digital Twins-SDK für .NET](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true), die zweite stellt Tools bereit, die die Authentifizierung bei Azure unterstützen.

      ```cmd/sh
      dotnet add package Azure.DigitalTwins.Core
      dotnet add package Azure.Identity
      ```

Sie müssen auch lokale Anmeldeinformationen einrichten, wenn Sie das Beispiel direkt ausführen möchten. Im nächsten Abschnitt werden diese Schritte erläutert.
[!INCLUDE [Azure Digital Twins: local credentials prereq (outer)](../../includes/digital-twins-local-credentials-outer.md)]

### <a name="run-the-sample"></a>Ausführen des Beispiels

Nachdem Sie die obigen Schritte ausgeführt haben, können Sie den folgenden Beispielcode direkt ausführen.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/graph_operations_sample.cs":::

Hier ist die Konsolenausgabe des obigen Programms: 

:::image type="content" source="./media/how-to-manage-graph/console-output-twin-graph.png" alt-text="Konsolenausgabe, die die Zwillingsdetails, ein- und ausgehende Beziehungen der Zwillinge anzeigt." lightbox="./media/how-to-manage-graph/console-output-twin-graph.png":::

> [!TIP]
> Der Zwillingsgraph ist ein Konzept zur Erstellung von Beziehungen zwischen Zwillingen. Wenn Sie sich die visuelle Darstellung des Zwillingsgraphen ansehen möchten, lesen Sie den Abschnitt [*Visualisierung*](how-to-manage-graph.md#visualization) dieses Artikels. 

## <a name="create-graph-from-a-csv-file"></a>Erstellen eines Graphen aus einer CSV-Datei

In praktischen Anwendungsfällen werden Zwillingshierarchien oft aus Daten erstellt, die in einer anderen Datenbank oder vielleicht in einem Arbeitsblatt oder einer CSV-Datei gespeichert sind. In diesem Abschnitt wird gezeigt, wie Sie Daten aus einer CSV-Datei lesen und einen Zwillingsgraphen daraus erstellen.

Betrachten Sie die folgende Datentabelle, in der eine Reihe von digitalen Zwillingen und Beziehungen beschrieben werden.

|  Modell-ID    | Zwillings-ID (muss eindeutig sein) | Beziehungsname  | Zielzwillings-ID  | Initialisierungsdaten des Zwillings |
| --- | --- | --- | --- | --- |
| dtmi:example:Floor;1    | Floor1 (Etage1) | contains | Room1 | |
| dtmi:example:Floor;1    | Floor0 (Etage0) | contains | Room0 | |
| dtmi:example:Room;1    | Room1 | | | {"Temperature": 80} |
| dtmi:example:Room;1    | Room0 | | | {"Temperature": 70} |

Eine Möglichkeit zum Übernehmen dieser Daten in Azure Digital Twins besteht darin, die Tabelle in eine CSV-Datei zu konvertieren und Code zu schreiben, um die Datei in Befehle zum Erstellen von Zwillingen und Beziehungen zu interpretieren. Das folgende Codebeispiel veranschaulicht das Lesen der Daten aus der CSV-Datei und das Erstellen eines Zwillingsgraphen in Azure Digital Twins.

Im folgenden Code hat die CSV-Datei den Namen *data.csv*, und es gibt einen Platzhalter, der für den **Hostnamen** Ihrer Azure Digital Twins-Instanz steht. Das Beispiel nutzt auch mehrere Pakete, die Sie dem Projekt zur Unterstützung dieses Prozesses hinzufügen können.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/graphFromCSV.cs":::

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Abfrage eines Azure Digital Twins-Zwillingsgraphen:
* [*Konzepte: Abfragesprache*](concepts-query-language.md)
* [*Verwenden Abfragen des Zwillingsgraphen*](how-to-query-graph.md)