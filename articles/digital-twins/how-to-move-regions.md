---
title: Verschieben einer Instanz in eine andere Azure-Region
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie eine Instanz von Azure Digital Twins aus einer Azure-Region in eine andere verschieben.
author: baanders
ms.author: baanders
ms.date: 08/26/2020
ms.topic: how-to
ms.custom: subject-moving-resources
ms.service: digital-twins
ms.openlocfilehash: c8f78af8753de0eadc26585adacf04f54c2eb750
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89300615"
---
# <a name="move-an-azure-digital-twins-instance-to-a-different-azure-region"></a>Verschieben einer Azure Digital Twins-Instanz in eine andere Azure-Region

Wenn Sie Ihre Azure Digital Twins-Instanz von einer Region in eine andere verschieben müssen, besteht der aktuelle Prozess darin, **Ihre Ressourcen in der neuen Region neu zu erstellen** und dann (optional) die ursprünglichen Ressourcen zu löschen. Am Ende dieses Prozesses werden Sie mit einer neuen Azure Digital Twins-Instanz arbeiten, die bis auf den aktualisierten Speicherort mit der ersten identisch ist.

Dieser Artikel bietet einen Leitfaden, wie Sie eine vollständige Verschiebung durchführen, indem Sie alles kopieren, was Sie benötigen, damit die neue Instanz mit dem Original übereinstimmt.

Dieser Vorgang umfasst die folgenden Schritte:
1. Vorbereiten: Laden Sie Ihre ursprünglichen Modelle, Zwillinge und Graphen herunter.
2. Verschieben: Erstellen Sie eine neue Azure Digital Twins-Instanz in einer neuen Region.
3. Verschieben: Füllen Sie die neue Azure Digital Twins-Instanz neu auf.
    - Laden Sie die ursprünglichen Modelle, Zwillinge und Graphen hoch.
    - Erstellen Sie Endpunkte und Routen neu.
    - Verknüpfen Sie verbundene Ressourcen neu.
4. Bereinigen Sie Quellressourcen (optional): Löschen Sie die ursprüngliche Instanz.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie versuchen, Ihre Azure Digital Twins-Instanz neu zu erstellen, ist es eine gute Idee, die Komponenten Ihrer ursprünglichen Instanz durchzugehen und sich eine klare Vorstellung von allen Komponenten zu machen, die neu erstellt werden müssen.

Hier sind einige Fragen, die Sie möglicherweise berücksichtigen möchten:
* Welche **Modelle** werden in meine Instanz hochgeladen? Wie viele sind es?
* Welche sind in meiner Instanz die **Zwillinge**? Wie viele sind es?
* Wie ist die allgemeine Form des **Graphen** in meiner Instanz? Wie viele Beziehungen gibt es?
* Über welche **Endpunkte** verfüge ich in meiner Instanz?
* Über welche **Routen** verfüge ich in meiner Instanz? Besitzen sie Filter?
* Wo wird meine Instanz **mit anderen Azure-Diensten** verbunden? Einige gemeinsame Integrationspunkte sind...
    - Event Grid, Event Hub oder Service Bus
    - Azure-Funktionen
    - Logic Apps
    - Time Series Insights
    - Azure Maps
    - Device Provisioning Service (DPS)
* Welche anderen **persönlichen oder Unternehmens-Apps** besitze ich, die eine Verbindung mit meiner Instanz herstellen?

Sie können diese Informationen über das [Azure-Portal](https://portal.azure.com), mithilfe der [Azure Digital Twins-APIs und -SDKs](how-to-use-apis-sdks.md), den [Befehlen der Azure Digital Twins-CLI](how-to-use-cli.md) oder das Beispiel [ADT-Explorer (Azure Digital Twins)](https://docs.microsoft.com/samples/azure-samples/digital-twins-explorer/digital-twins-explorer/) sammeln.

## <a name="prepare"></a>Vorbereiten

In diesem Abschnitt bereiten Sie sich darauf vor, Ihre Instanz neu zu erstellen, indem Sie **Ihre ursprünglichen Modelle, Zwillinge und Graphen** von Ihrer ursprünglichen Instanz herunterladen. Dazu verwenden Sie das Beispiel [ADT-Explorer (Azure Digital Twins)](https://docs.microsoft.com/samples/azure-samples/digital-twins-explorer/digital-twins-explorer/).

>[!NOTE]
>Möglicherweise verfügen Sie bereits über Dateien, die die Modelle und/oder den Graphen in Ihrer Instanz enthalten. Sofern dies der Fall ist, brauchen Sie nicht alles erneut herunterzuladen – nur die fehlenden Teile oder Komponenten, die sich seit dem ursprünglichen Hochladen dieser Dateien geändert haben könnten (z. B. Zwillinge, die möglicherweise mit neuen Daten aktualisiert wurden).

### <a name="set-up-adt-explorer-application"></a>Einrichten der ADT Explorer-Anwendung

Laden Sie zunächst den Beispielcode der Anwendung herunter, und richten Sie ihn so ein, dass er auf Ihrem Computer ausgeführt wird. 

Navigieren Sie hier zu dem Beispiel: [ADT Explorer (Azure Digital Twins)](https://docs.microsoft.com/samples/azure-samples/digital-twins-explorer/digital-twins-explorer/). Klicken Sie auf die Schaltfläche *Download ZIP* (ZIP herunterladen), um eine *ZIP-Datei* des Beispielcodes als _**ADT_Explorer.zip**_ auf Ihren Computer herunterzuladen. Entzippen Sie die Datei.

Richten Sie als nächstes die Berechtigungen für ADT Explorer für die Ausführung auf Ihrem Computer ein. Befolgen Sie dazu die Schritte im Abschnitt [*Festlegen der ADT Explorer-Berechtigungen*](quickstart-adt-explorer.md#set-adt-explorer-permissions) des Azure Digital Twins-Schnellstarts.

Führen Sie schließlich ADT Explorer aus und konfigurieren Sie ihn so, dass er sich mit Ihrer ursprünglichen Azure Digital Twins-Instanz verbindet. Folgen Sie den Schritten im Abschnitt [*Ausführen und Konfigurieren von ADT Explorer*](quickstart-adt-explorer.md#run-and-configure-adt-explorer) des Schnellstarts.

Jetzt sollte die ADT Explorer-Beispiel-App in einem Browser auf Ihrem Computer ausgeführt werden. Das Beispiel sollte mit Ihrer ursprünglichen Azure Digital Twins-Instanz verbunden sein.

:::image type="content" source="media/how-to-move-regions/explorer-blank.png" alt-text="Browserfenster mit einer unter „localhost:3000“ ausgeführten App. Die App heißt ADT Explorer und enthält Felder für einen Abfrage-Explorer, eine Modellansicht, eine Graphansicht und einen Eigenschaften-Explorer. Es sind noch keine Daten vorhanden." lightbox="media/how-to-move-regions/explorer-blank.png":::

Zur Überprüfung der Verbindung können Sie die Schaltfläche *Run Query* (Abfrage ausführen) drücken, um die Standardabfrage auszuführen, die alle Zwillinge und Beziehungen im Graphen im Feld *GRAPH EXPLORER* (Graph-Tester) anzeigt.

:::image type="content" source="media/how-to-move-regions/run-query.png" alt-text="Hervorgehobene Schaltfläche „Run Query“ (Abfrage ausführen) im oberen Bereich des Fensters" lightbox="media/how-to-move-regions/run-query.png":::

Sie können ADT Explorer weiterhin ausführen, da Sie ihn später in diesem Artikel erneut verwenden werden, um diese Elemente erneut auf Ihre neue Instanz in der Zielregion hochzuladen.

### <a name="download-models-twins-and-graph"></a>Herunterladen von Modellen, Zwillingen und Graphen

Laden Sie als nächstes die Modelle, Zwillinge und Graphen in Ihrer Lösung auf Ihren Computer herunter.

Um alle diese Dateien auf einmal herunterzuladen, stellen Sie zunächst sicher, dass der vollständige Graph im Feld *GRAPH VIEW* (Graphansicht) angezeigt wird (Sie können dazu die Standardabfrage von `SELECT * FROM digitaltwins` im Feld *QUERY EXPLORER* (Abfrage-Explorer) erneut ausführen).
 
Klicken Sie dann auf das Symbol *Export graph* (Graph exportieren) im Feld *GRAPH VIEW* (Graphansicht).

:::image type="content" source="media/how-to-move-regions/export-graph.png" alt-text="Feld „Graph View“ (Graphansicht) mit einem hervorgehobenen Symbol. Es zeigt einen Pfeil, der aus einer Wolke heraus nach unten zeigt." lightbox="media/how-to-move-regions/export-graph.png":::

Dadurch wird ein *Download*-Link in *GRAPH VIEW* (Graphansicht) aktiviert. Wählen Sie ihn aus, um eine JSON-basierte Darstellung des Abfrageergebnisses, einschließlich Ihrer Modelle, Zwillinge und Beziehungen, herunterzuladen. Dies sollte eine *JSON*-Datei auf Ihren Computer herunterladen.

>[!NOTE]
>Wenn die heruntergeladene Datei eine andere Dateierweiterung aufweist, versuchen Sie, die Erweiterung direkt zu bearbeiten und sie in *JSON* zu ändern.

## <a name="move"></a>Move

Als nächstes schließen Sie die „Verschiebung“ Ihrer Instanz ab, indem Sie eine neue Instanz in der Zielregion erstellen und diese mit den Daten und Komponenten Ihrer ursprünglichen Instanz füllen.

### <a name="create-a-new-instance"></a>Erstellen einer neuen Instanz

Erstellen Sie zunächst **eine neue Azure Digital Twins-Instanz in Ihrer Zielregion**. Führen Sie dazu die Schritte in [*Gewusst wie: Einrichten einer Instanz und der Authentifizierung*](how-to-set-up-instance-scripted.md) aus, wobei Sie diese Hinweise im Auge behalten:
* Sie können denselben Namen für die neue Instanz beibehalten, **wenn** sie sich in einer anderen Ressourcengruppe befindet. Wenn Sie dieselbe Ressourcengruppe verwenden müssen, die Ihre ursprüngliche Instanz enthält, dann benötigt Ihre neue Instanz einen eigenen, eindeutigen Namen.
* Geben Sie die neue Zielregion ein, wenn Sie zur Eingabe eines Standorts aufgefordert werden.
* Sie müssen die Registrierung der App **nicht** erneut erstellen. Ihre neue Instanz kann dieselbe App-Registrierung wiederverwenden, über die Sie bereits verfügen.
    - Wenn Sie den [skriptbasierten](how-to-set-up-instance-scripted.md) Setup-Artikel verwenden, können Sie die Details Ihrer bestehenden App-Registrierung erneut eingeben, anstatt einen neuen Namen einzugeben, wenn Sie dazu aufgefordert werden.
    - Wenn Sie die Artikel für das manuelle Setup des [Portals](how-to-set-up-instance-portal.md) oder der [CLI](how-to-set-up-instance-cli.md) verwenden, können Sie nach den Schritten *Erstellen der Azure Digital Twins-Instanz* und *Einrichten von Benutzerzugriffsberechtigungen* anhalten. Es ist nicht mehr notwendig, *Einrichten von Zugriffsberechtigungen für Clientanwendungen* durchzuarbeiten.

Sobald dies abgeschlossen ist, benötigen Sie den **Hostnamen** Ihrer neuen Instanz, um die Einrichtung mit Ihren Daten fortzusetzen. Falls Sie dies bei der Einrichtung nicht notiert haben, können Sie [diese Anweisungen](how-to-set-up-instance-portal.md#verify-success-and-collect-important-values) befolgen, um sie jetzt vom Azure-Portal abzurufen.

### <a name="repopulate-old-instance"></a>Erneutes Auffüllen alter Instanzen

Als nächstes richten Sie die neue Instanz so ein, dass Sie eine Kopie der ursprünglichen ist.

#### <a name="upload-original-models-twins-and-graph-using-adt-explorer"></a>Hochladen ursprünglicher Modelle, Zwillinge und Graphen mit ADT Explorer

In diesem Abschnitt können Sie die Modelle, Zwillinge und Graphen erneut in die neue Instanz hochladen. Wenn Sie in Ihrer ursprünglichen Instanz keine Modelle, Zwillinge oder Graphen besitzen oder Sie diese nicht in die neue Instanz verschieben möchten, können Sie zum [nächsten Abschnitt](#recreate-endpoints-and-routes) springen.

Andernfalls kehren Sie zum Fortfahren zum Browserfenster zurück, in dem **ADT Explorer** ausgeführt wird, und führen Sie die folgenden Schritte aus.

##### <a name="connect-to-the-new-instance"></a>Herstellen einer Verbindung mit der neuen Instanz

Derzeit ist ADT Explorer mit Ihrer ursprünglichen Azure Digital Twins-Instanz verbunden. Wechseln Sie die Verbindung so, dass sie auf Ihre neue Instanz zeigt, indem Sie oben im Fenster auf die Schaltfläche *Anmelden* klicken. 

:::image type="content" source="media/how-to-move-regions/sign-in.png" alt-text="ADT Explorer mit hervorgehobenem Anmeldesymbol im oberen Bereich des Fensters. Bei dem Symbol handelt es sich um eine einfache Silhouette einer Person mit der Silhouette eines Schlüssels im Vordergrund." lightbox="media/how-to-move-regions/sign-in.png":::

Da Sie die App-Registrierung erneut verwenden, müssen Sie nur die *ADT-URL* ersetzen. Ändern Sie diesen Wert in *https://{neuer Instanzhostname}* .

Drücken Sie auf *Verbinden*. Möglicherweise werden Sie aufgefordert, sich erneut mit Ihren Anmeldeinformationen in Azure anzumelden und/oder dieser Anwendung die Zustimmung für Ihre Instanz zu erteilen.

##### <a name="upload-models-twins-and-graph"></a>Hochladen von Modellen, Zwillingen und Graphen

Als nächstes laden Sie die Lösungskomponenten, die Sie zuvor heruntergeladen haben, auf Ihre neue Instanz hoch.

Um Ihre **Modelle, Zwillinge und Graphen** hochzuladen, klicken Sie auf das Symbol *Import Graph* (Graph importieren) im Feld *GRAPH VIEW* (Graphansicht). Mit dieser Option werden alle drei dieser Komponenten auf einmal hochgeladen (auch Modelle, die derzeit nicht im Graphen verwendet werden).

:::image type="content" source="media/how-to-move-regions/import-graph.png" alt-text="Feld „Graph View“ (Graphansicht) mit einem hervorgehobenen Symbol. Es zeigt einen auf eine Wolke gerichteten Pfeil." lightbox="media/how-to-move-regions/import-graph.png":::

Navigieren Sie im Dateiauswahlfeld zu Ihrem heruntergeladenen Graphen. Wählen Sie die *JSON*-Graphdatei aus, und drücken Sie auf *Öffnen*.

Nach wenigen Sekunden wird von ADT Explorer eine Importansicht mit einer Vorschau des zu ladenden Graphen angezeigt.

Klicken Sie zum Bestätigen des Graphuploads rechts oben in *GRAPH VIEW* (GRAPHANSICHT) auf das Symbol *Speichern*:

:::row:::
    :::column:::
        :::image type="content" source="media/how-to-move-regions/graph-preview-save.png" alt-text="Hervorgehobenes Speichersymbol im Bereich mit der Graphvorschau" lightbox="media/how-to-move-regions/graph-preview-save.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

ADT Explorer wird jetzt Ihre Modelle und Graphen (einschließlich der Zwillinge und Beziehungen) in Ihre neue Azure Digital Twins-Instanz hochladen. Es sollte eine Erfolgsmeldung angezeigt werden, aus der hervorgeht, wie viele Modelle, Zwillinge und Beziehungen hochgeladen wurden:

:::row:::
    :::column:::
        :::image type="content" source="media/how-to-move-regions/import-success.png" alt-text="Dialogfeld mit der Information, dass der Graph erfolgreich importiert wurde. Der Text lautet: „Import successful. 2 models imported. 4 twins imported. 2 relationships imported.“ (Import erfolgreich. 2 Modelle imporiert. 4 Zwillinge importiert. 2 Beziehungen importiert.)" lightbox="media/how-to-move-regions/import-success.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Um zu überprüfen, ob alles erfolgreich hochgeladen wurde, drücken Sie die Schaltfläche *Run Query* (Abfrage ausführen) im Feld *GRAPH EXPLORER* (Graph-Tester), um die Standardabfrage auszuführen, die alle Zwillinge und Beziehungen im Graphen anzeigt. Dadurch wird auch die Liste der Modelle in der *MODEL VIEW* (Modellansicht) aktualisiert.

:::image type="content" source="media/how-to-move-regions/run-query.png" alt-text="Hervorhebung um dieselbe vorherige Schaltfläche „Run Query“ (Abfrage ausführen), am oberen Fensterrand" lightbox="media/how-to-move-regions/run-query.png":::

Der Graph sollte mit all seinen Zwillingen und Beziehungen im Feld *GRAPH EXPLORER* (Graph-Tester) angezeigt werden. Ihre Modelle sollten auch im Feld *MODEL VIEW* (Modellansicht) aufgelistet sein.

:::image type="content" source="media/how-to-move-regions/post-upload.png" alt-text="Eine Ansicht von ADT Explorer mit zwei Modellen, die im Feld „Model View“ (Modellansicht) hervorgehoben sind, und ein Graph, der im Feld „Graph Explorer“ (Graph-Tester) hervorgehoben ist" lightbox="media/how-to-move-regions/post-upload.png":::

Damit wird bestätigt, dass Ihre Modelle, Zwillinge und Graphen erneut auf die neue Instanz in der Zielregion hochgeladen wurden.

#### <a name="recreate-endpoints-and-routes"></a>Erneutes Erstellen von Endpunkten und Routen

Wenn Sie **Endpunkte und/oder Routen** in Ihrer ursprünglichen Instanz besitzen, müssen Sie diese in Ihrer neuen Instanz neu erstellen. Wenn Sie in Ihrer ursprünglichen Instanz keine Endpunkte oder Routen besitzen oder Sie diese nicht in die neue Instanz verschieben möchten, können Sie zum [nächsten Abschnitt](#re-link-connected-resources) springen.

Andernfalls fahren Sie fort. Folgen Sie den Schritten in [*Gewusst wie: Verwalten von Endpunkten und Routen*](how-to-manage-routes-portal.md), wobei Sie die neue Instanz verwenden und diese Hinweise im Auge behalten: 
* Sie müssen **nicht** die Event Grid-, Event Hub- oder Service Bus-Ressource, die Sie für den Endpunkt verwenden, neu erstellen (Abschnitte zur *Voraussetzung* in den Endpunktanweisungen). Sie müssen den Endpunkt nur auf der Azure Digital Twins-Instanz neu erstellen.
* Sie können Endpunkt- und Routen**namen** wiederverwenden, da sie auf eine andere Instanz begrenzt sind.
* Denken Sie daran, alle erforderlichen **Filter** zu den von Ihnen erstellten Routen hinzuzufügen.

#### <a name="re-link-connected-resources"></a>Erneutes Verknüpfen verbundener Ressourcen

Wenn Sie andere Apps oder Azure-Ressourcen besitzen, die mit Ihrer ursprünglichen Azure Digital Twins-Instanz verbunden sind, müssen Sie die Verbindung bearbeiten, sodass sie stattdessen Ihre neue Instanz erreichen. Dies kann andere Azure-Dienste oder persönliche bzw. Unternehmens-Apps umfassen, die Sie für die Zusammenarbeit mit Azure Digital Twins eingerichtet haben.

Wenn mit Ihrer ursprünglichen Instanz keine anderen Ressourcen verbunden sind oder Sie diese nicht in die neue Instanz verschieben möchten, können Sie zum [nächsten Abschnitt](#verify) springen.

Andernfalls sollten Sie zum Fortfahren die verbundenen Ressourcen in Ihrem Szenario berücksichtigen. Sie brauchen keine verbundenen Ressourcen zu löschen und neu zu erstellen. Stattdessen müssen Sie nur die Punkte bearbeiten, an denen sie sich mit einer Azure Digital Twins-Instanz über ihren **Hostnamen** verbinden, und diesen aktualisieren, um den Hostnamen der neuen Instanz anstelle des ursprünglichen zu verwenden.

Die genauen Ressourcen, die Sie bearbeiten müssen, hängen von Ihrem Szenario ab, aber hier folgen einige allgemeine Integrationspunkte:
* Azure Functions. Wenn Sie über eine Azure-Funktion verfügen, deren Code den Hostnamen der ursprünglichen Instanz enthält, sollten Sie diesen Wert auf den Hostnamen der neuen Instanz aktualisieren und die Funktion erneut veröffentlichen.
* Event Grid, Event Hubs oder Service Bus
* Logic Apps
* Time Series Insights
* Azure Maps
* Device Provisioning Service (DPS)
* Persönliche oder Unternehmens-Apps außerhalb von Azure, z. B. die **Client-App**, die im [*Tutorial: Codieren einer Client-App*](tutorial-code.md) erstellt wurde, die eine Verbindung mit der Instanz herstellt und Azure Digital Twins-APIs aufruft.

Nachdem Sie diesen Schritt abgeschlossen haben, sollte Ihre neue Instanz in der Zielregion eine Kopie der ursprünglichen Instanz sein.

## <a name="verify"></a>Überprüfung

Zum Überprüfen, ob Ihre neue Instanz ordnungsgemäß eingerichtet wurde, können Sie die folgenden Tools verwenden:
* Das [**Azure-Portal**](https://portal.azure.com) (geeignet, um zu überprüfen, ob Ihre neue Instanz existiert und sich in der richtigen Zielregion befindet sowie ebenfalls geeignet, um Endpunkte, Routen und Verbindungen mit anderen Azure-Diensten zu überprüfen)
* Die [Azure Digital Twins-**CLI-Befehle**](how-to-use-cli.md) (geeignet, um zu überprüfen, ob Ihre neue Instanz existiert und sich in der richtigen Zielregion befindet; kann auch zum Überprüfen von Instanzdaten verwendet werden)
* [**ADT Explorer**](https://docs.microsoft.com/samples/azure-samples/digital-twins-explorer/digital-twins-explorer/) (geeignet, um Instanzdaten wie Modelle, Zwillinge und Graphen zu überprüfen)
* Die [Azure Digital Twins-APIs und -SDKs](how-to-use-apis-sdks.md) (geeignet, um Instanzdaten wie Modelle, Zwillinge und Graphen zu überprüfen sowie ebenfalls geeignet, um Endpunkte und Routen zu überprüfen)

Sie können auch versuchen, alle benutzerdefinierten Apps oder End-to-End-Flows, die Sie mit Ihrer ursprünglichen Instanz ausgeführt haben, auszuführen, um zu überprüfen, ob sie mit der neuen Instanz ordnungsgemäß funktionieren.

## <a name="clean-up-source-resources-optional"></a>Bereinigen von Quellressourcen (optional)

Nachdem Ihre neue Instanz jetzt in der Zielregion mit einer Kopie der Daten und Verbindungen der ursprünglichen Instanz eingerichtet ist, können Sie **die ursprüngliche Instanz bei Bedarf löschen**.

Sie können dazu das [Azure-Portal](https://portal.azure.com), die [CLI](how-to-use-cli.md) oder die [APIs der Steuerungsebene](how-to-use-apis-sdks.md#overview-control-plane-apis) verwenden.

Um die Instanz über das Azure-Portal zu löschen, [öffnen Sie das Portal](https://portal.azure.com) in einem Browserfenster, und navigieren Sie zu Ihrer ursprünglichen Azure Digital Twins-Instanz, indem Sie in der Portalsuchleiste nach ihrem Namen suchen.

Klicken Sie auf die Schaltfläche *Löschen*, und folgen Sie den Aufforderungen, um den Löschvorgang abzuschließen.

:::image type="content" source="media/how-to-move-regions/delete-instance.png" alt-text="Ansicht der Details der Azure Digital Twins-Instanz im Azure-Portal auf der Registerkarte „Übersicht“. Die Schaltfläche „Löschen“ ist hervorgehoben.":::