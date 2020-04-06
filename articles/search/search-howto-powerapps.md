---
title: Abfragen von Azure Cognitive Search über Power Apps
titleSuffix: Azure Cognitive Search
description: Schritt-für-Schritt-Anleitung zum Erstellen eines benutzerdefinierten Connectors für Cognitive Search und Visualisieren dieses Connectors über eine Power App
author: luiscabrer
manager: eladz
ms.author: luisca
ms.service: cognitive-search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 03/25/2020
ms.openlocfilehash: c246f8652227a5ad2c0798880e530d6039cdeea8
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2020
ms.locfileid: "80385111"
---
# <a name="how-to-query-a-cognitive-search-index-from-power-apps"></a>Abfragen eines Cognitive Search-Index über Power Apps

In diesem Dokument wird gezeigt, wie Sie einen benutzerdefinierten Power Apps-Connector erstellen, damit Sie Suchergebnisse aus einem Suchindex abrufen können. Außerdem wird gezeigt, wie Sie eine Suchabfrage ausgeben und die Ergebnisse über eine Power App visualisieren. 

## <a name="prerequisites"></a>Voraussetzungen
*    Zugriff auf das Power Apps-Konto mit der Möglichkeit zum Erstellen von benutzerdefinierten Connectors.
*    Wir gehen davon aus, dass Sie bereits einen Azure-Suchindex erstellt haben.

## <a name="create-a-custom-connector-to-query-azure-search"></a>Erstellen eines benutzerdefinierten Connectors zum Abfragen von Azure Cognitive Search

Es gibt zwei Hauptschritte, um eine Power App zu erhalten, in der Azure Cognitive Search-Ergebnisse angezeigt werden. Zunächst erstellen wir einen Connector, mit dem der Suchindex abgefragt werden kann. Im [nächsten Abschnitt](#visualize-results-from-the-custom-connector) aktualisieren wir die Power Apps-Anwendung, um die vom Connector zurückgegebenen Ergebnisse visuell darzustellen.

1. Wechseln Sie zu [make.powerapps.com](http://make.powerapps.com), und wählen Sie **Anmelden** aus.

1. Suchen Sie nach **Daten** > **Benutzerdefinierte Connectors**.
 
    :::image type="content" source="./media/search-howto-powerapps/1-2-custom-connector.png" alt-text="Menü „Benutzerdefinierte Connectors“" border="true":::

1. Klicken Sie auf **+ Neuer benutzerdefinierter Connector**, und wählen Sie dann **Ohne Vorlage erstellen** aus.

    :::image type="content" source="./media/search-howto-powerapps/1-3-create-blank.png" alt-text="Menü „Ohne Vorlage erstellen“" border="true":::

1. Versehen Sie den benutzerdefinierten Connector mit einem Namen. (*AzureSearchQuery*), und klicken Sie dann auf **Weiter**. Dadurch wird ein Assistent zum Erstellen des neuen Connectors geöffnet.

1. Geben Sie Informationen auf der Seite „Allgemeine Informationen“ ein.

    - Symbolhintergrundfarbe (z. B. #007ee5)
    - Beschreibung (z. B. „Connector für Azure Cognitive Search“)
    - In „Host“ müssen Sie die URL des Suchdiensts eingeben (z. B. `<yourservicename>.search.windows.net`).
    - Geben Sie für „Basis-URL“ einfach „/“ ein.
    
    :::image type="content" source="./media/search-howto-powerapps/1-5-general-info.png" alt-text="Dialogfeld „Allgemeine Informationen“" border="true":::

1. Legen Sie auf der Seite „Sicherheit“ *API-Schlüssel* als **Authentifizierungstyp** fest. Legen Sie in den Feldern „Parameterbezeichnung“ und „Parametername“ den Wert *api-key* fest. Wählen Sie für **Parameterstandort** *Header* aus, wie unten dargestellt.
 
    :::image type="content" source="./media/search-howto-powerapps/1-6-authentication-type.png" alt-text="Option „Authentifizierungstyp“" border="true":::

1. Wählen Sie auf der Seite „Definitionen“ **+ Neue Aktion** aus, um eine Aktion zu erstellen, mit der der Index abgefragt wird. Geben Sie für die Zusammenfassung und den Namen der Vorgangs-ID den Wert „Abfrage“ ein. Geben Sie als Beschreibung z. B. *„Fragt den Suchindex ab“* ein.
 
    :::image type="content" source="./media/search-howto-powerapps/1-7-new-action.png" alt-text="Optionen für „Neue Aktion“" border="true":::


1. Klicken Sie auf die Schaltfläche **+ Aus Beispiel importieren**, um die Parameter und Header zu definieren. Als Nächstes definieren Sie die Abfrageanforderung.  

    * Wählen Sie das Verb `GET` aus.
    * Geben Sie für die URL eine Beispielabfrage des Suchindex ein, z. B.:
       
    >https://yoursearchservicename.search.windows.net/indexes/yourindexname/docs?search= *&api-version=2019-05-06-Preview
    

    **Power Apps** verwendet die Syntax, um Parameter aus der Abfrage zu extrahieren. Beachten Sie, dass das Suchfeld explizit definiert wurde. 

    :::image type="content" source="./media/search-howto-powerapps/1-8-1-import-from-sample.png" alt-text="Aus Beispiel importieren" border="false":::

1.  Klicken Sie auf **Importieren**, um das Dialogfeld „Anforderung“ automatisch vorab aufzufüllen.

    :::image type="content" source="./media/search-howto-powerapps/1-8-2-import-from-sample.png" alt-text="Dialogfeld „Aus Beispiel importieren“" border="false":::


1. Klicken Sie auf das Symbol **...** neben den einzelnen Parametern, um das Festlegen der Parametermetadaten abzuschließen.

    - Für *search*: Legen Sie `*` als **Standardwert**, **Erforderlich** auf *Nein* und **Sichtbarkeit** auf *Keine* fest. 

    :::image type="content" source="./media/search-howto-powerapps/1-10-1-parameter-metadata-search.png" alt-text="Suchparametermetadaten" border="true":::

    - Für *api-version*: Legen Sie `2019-05-06-Preview` als **Standardwert** fest, legen Sie **Sichtbarkeit** auf „intern“ und **erforderlich** auf *Ja* fest.  

    :::image type="content" source="./media/search-howto-powerapps/1-10-2-parameter-metadata-version.png" alt-text="Metadaten des Versionsparameters" border="true":::

    - Entsprechend legen Sie *api-key*als **erforderlich** fest, mit *interner* **Sichtbarkeit**. Geben Sie den API-Schlüssel des Suchdiensts als **Standardwert** ein.
    
    Nachdem Sie diese Änderungen vorgenommen haben, aktivieren Sie die Ansicht **Swagger-Editor**. Im Abschnitt „parameters“ wird die folgende Konfiguration angezeigt:    

    ```
          parameters:
          - {name: search, in: query, required: false, type: string, default: '*'}
          - {name: api-version, in: query, required: true, type: string, default: 2019-05-06-Preview,
            x-ms-visibility: internal}
          - {name: api-key, in: header, required: true, type: string, default: YOURKEYGOESHERE,
            x-ms-visibility: internal}
    ```

1. Klicken Sie im Abschnitt „Antwort“ auf **Standardantwort hinzufügen**. Dies ist wichtig, da es das Interpretieren des Schemas der Antwort durch **Power Apps** erleichtert. Fügen Sie eine Beispielantwort ein.

    > [!TIP] 
    > Sie können für die JSON-Antwort nur eine begrenzte Anzahl von Zeichen eingeben. Deshalb sollten Sie den JSON-Code vereinfachen, bevor Sie ihn einfügen. Wichtig ist das Schema/Format der Antwort. Die tatsächlichen Werte in der Beispielantwort sind weniger wichtig und können vereinfacht werden, um die Anzahl der Zeichen zu verringern.
    

1.    Sie müssen oben rechts auf dem Bildschirm auf die Schaltfläche **Connector erstellen** klicken, bevor Sie den Test durchführen können.

1.  Klicken Sie auf der Seite „Testen“ auf **+ Neue Verbindung**, und geben Sie als Wert für *api-key* den Suchdienst-Abfrageschlüssel ein.

    Mit diesem Schritt verlassen Sie möglicherweise den Assistenten und gelangen zur Seite „Verbindungen“. Sie können zum Editor für „Benutzerdefinierte Verbindungen“ zurückkehren, um die Verbindung zu testen. Wechseln Sie zu **Benutzerdefinierter Connector** > wählen Sie den neu erstellten Connector aus > *...* > **Eigenschaften anzeigen** > **Bearbeiten** > **4. Testen**, um zur Seite „Testen“ zurückzukehren.

1.    Klicken Sie jetzt auf **Vorgang testen**, um sicherzustellen, dass Sie Ergebnisse aus dem Index erhalten. Wenn der Vorgang erfolgreich ausgeführt wurde, sollte der Status 200 angezeigt werden, und im Text der Antwort sollte JSON-Code angezeigt werden, der die Suchergebnisse beschreibt.




## <a name="visualize-results-from-the-custom-connector"></a>Visualisieren der Ergebnisse aus dem benutzerdefinierten Connector
In diesem Tutorial wird nicht erläutert, wie Sie mit Power Apps eine schöne Benutzeroberfläche erstellen, und das Layout der Benutzeroberfläche ist minimalistisch. Wir erstellen eine PowerApp mit einem Suchfeld und einer Suchschaltfläche und zeigen die Ergebnisse in einem Katalogsteuerelement an.  Die Power App stellt eine Verbindung mit unserem kürzlich erstellten benutzerdefinierten Connector her, um die Daten aus Azure Search abzurufen.

1. Erstellen Sie eine neue Power App. Wechseln Sie zum Abschnitt **Apps**, klicken Sie auf **+ Neue App**, und wählen Sie **Canvas** aus.

    :::image type="content" source="./media/search-howto-powerapps/2-1-create-canvas.png" alt-text="Erstellen einer Canvas-App" border="true":::

1. Wählen Sie den Typ der Anwendung aus, die Sie bereitstellen möchten. Erstellen Sie für dieses Tutorial eine **Leere App** mit **Telefonlayout**. **Power Apps Studio** wird angezeigt.

1. Wählen Sie in Studio die Registerkarte **Datenquellen** aus, und klicken Sie auf den neuen Connector, den Sie gerade erstellt haben. In unserem Fall lautet sein Name *AzureSearchQuery*. Klicken Sie auf **Verbindung hinzufügen**.

    :::image type="content" source="./media/search-howto-powerapps/2-3-connect-connector.png" alt-text="Verbinden des Connectors" border="true":::

    *AzureSearchQuery* ist nun eine Datenquelle, die für die Verwendung in Ihrer Anwendung verfügbar ist.
    
1. Navigieren Sie zur Registerkarte **Einfügen**, damit wir dem Formular einige Steuerelemente hinzufügen können.

    :::image type="content" source="./media/search-howto-powerapps/2-4-add-controls.png" alt-text="Einfügen von Steuerelementen" border="true":::

1.  Fügen Sie die folgenden Elemente ein:
    -   „Bezeichnungstext“ mit dem Wert „Abfrage:“
    -   „Texteingabe“ (benennen Sie sie mit *txtQuery*, Standardwert: „*“)
    -   Eine Schaltfläche mit dem Text „Suchen“ 
    -   „Vertical Gallery“ (Vertikaler Katalog) (benennen Sie ihn mit *galleryResults*)
    
    Das Formular könnte beispielsweise wie folgt aussehen:

    :::image type="content" source="./media/search-howto-powerapps/2-5-controls-layout.png" alt-text="Layout der Steuerelemente" border="true":::

1. Damit mit der **Schaltfläche „Suchen“** eine Abfrage gesendet wird, wählen Sie die Schaltfläche aus, und fügen Sie für **OnSelect** die folgende auszuführende Aktion ein:

    ```
    If(!IsBlank(txtQuery.Text),
        ClearCollect(azResult, AzureSearchQuery.Get({search: txtQuery.Text}).value))
    ```

    :::image type="content" source="./media/search-howto-powerapps/2-6-search-button-event.png" alt-text="OnSelect für Schaltfläche" border="true":::
 
    Diese Aktion bewirkt, dass die Schaltfläche eine neue Sammlung mit dem Namen *azResult* mit dem Ergebnis der Suchabfrage aktualisiert. Dabei wird der Text im Textfeld *txtQuery* als Abfragebegriff verwendet.
    
1.  Im nächsten Schritt verknüpfen wir den von uns erstellten vertikalen Katalog mit der Sammlung *azResult*. Wählen Sie das Katalogsteuerelement aus, und führen Sie im Bereich „Eigenschaften“ die folgenden Aktionen aus.

    -  Legen Sie **DataSource** auf *azResult* fest.
    
    -  Wählen Sie ein **Layout**, das für den Typ der Daten im Index geeignet ist. In diesem Fall haben wir das Layout *Titel, Untertitel und Text* verwendet.
    
    -  **Bearbeiten Sie die Felder**, und wählen Sie die Felder aus, die Sie visualisieren möchten.

    Da wir beim Definieren des Connectors ein Beispielergebnis angegeben haben, erkennt die App die im Index verfügbaren Felder.
    
    :::image type="content" source="./media/search-howto-powerapps/2-7-gallery-select-fields.png" alt-text="Katalogfelder" border="true":::   
 
1.  Drücken Sie **F5**, um eine Vorschau der App anzuzeigen.  

    Beachten Sie, dass die Felder auf berechnete Werte festgelegt werden können.      
    Beispielsweise wird das folgende Ergebnis erzeugt, wenn beim Festlegen das Layout *Bild, Titel und Untertitel* verwendet und die Funktion *Bild* als Verkettung des Stammpfads für die Daten und den Dateinamen (z. B. `"https://mystore.blob.core.windows.net/multilang/" & ThisItem.metadata_storage_name`) angegeben wird.

    :::image type="content" source="./media/search-howto-powerapps/2-8-2-final.png" alt-text="Fertiggestellte App" border="true":::        

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen und Onlinetraining finden Sie unter [Learning catalog for Power Apps](https://docs.microsoft.com/powerapps/learning-catalog/get-started) (Schulungskatalog für Power Apps, in englischer Sprache).

