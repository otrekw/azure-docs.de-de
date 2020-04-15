---
title: 'Tutorial: Kontextbezogene Daten mit Rollen: LUIS'
description: Es wird beschrieben, wie Sie verwandte Daten anhand des Kontexts finden. Beispielsweise sind der Ausgangsort und der Zielort für eine physische Verlegung aus einem Gebäude und Büro in ein anderes Gebäude und Büro aufeinander bezogen.
ms.topic: tutorial
ms.date: 03/30/2020
ms.openlocfilehash: fdb463896e531619ea7ebe7c384729763dc84138
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2020
ms.locfileid: "80475826"
---
# <a name="tutorial-extract-contextually-related-data-from-an-utterance"></a>Tutorial: Extrahieren von Daten mit ähnlichem Kontext aus einer Äußerung

In diesem Tutorial wird das kontextbasierte Finden aufeinander bezogener Teildaten behandelt. Ein Beispiel wären etwa Ursprungs- und Zielorte bei der Versetzung in eine andere Stadt. Beide Teilinformationen können erforderlich sein und stehen miteinander in Verbindung.

Eine Rolle kann mit allen vordefinierten oder benutzerdefinierten Entitätstypen und sowohl in Beispieläußerungen als auch in Mustern verwendet werden.

**In diesem Tutorial lernen Sie Folgendes:**

> [!div class="checklist"]
> * Erstellen einer neuen App
> * Hinzufügen einer Absicht
> * Abrufen von Ursprungs- und Zielinformationen mithilfe von Rollen
> * Trainieren
> * Veröffentlichen
> * Abrufen von Absichten und Entitätsrollen vom Endpunkt

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="related-data"></a>Verwandte Daten

Diese App erkennt, ob ein Mitarbeiter vom Ursprungsort an den Zielort versetzt werden soll. Es wird eine vordefinierte GeographyV2-Entität verwendet, um die Städtenamen zu identifizieren, und es werden Rollen genutzt, um die Standorttypen (Ursprung und Ziel) in der Äußerung zu ermitteln.

Eine Rolle sollte verwendet werden, wenn für die zu extrahierenden Entitätsdaten Folgendes gilt:

* Sie sind im Kontext der Äußerung miteinander verknüpft.
* Es werden bestimmte Wörter verwendet, um die einzelnen Rollen anzugeben. Beispiele für diese Wörter sind: von/nach, Verlassen/Ziel.
* Beide Rolle kommen häufig in derselben Äußerung vor, sodass LUIS von dieser häufigen kontextbezogenen Nutzung lernen kann.
* Müssen gruppiert und von einer Client-App als eine Informationseinheit verarbeitet werden.

## <a name="create-a-new-app"></a>Erstellen einer neuen App

1. Melden Sie sich beim [LUIS-**Vorschauportal**](https://preview.luis.ai) an.

1. Wählen Sie **+ Neue App für Unterhaltung** aus, geben Sie den Namen `HumanResources` ein, und behalten Sie die Standardkultur **Englisch** bei. Lassen Sie die Felder für Beschreibung und Vorhersageressource leer. Wählen Sie **Fertig**aus.

## <a name="create-an-intent-to-move-employees-between-cities"></a>Erstellen einer Absicht zur Versetzung von Mitarbeitern an einen anderen Ort

Eine Absicht wird verwendet, um Benutzeräußerungen auf der Grundlage der anhand des Texts in natürlicher Sprache bestimmten Benutzerabsicht zu klassifizieren.

Um eine Äußerung klassifizieren zu können, werden Beispiele von Benutzeräußerungen benötigt, die mit dieser Absicht klassifiziert werden sollen.

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. Wählen Sie **+ Erstellen** aus.

1. Geben Sie im Popupdialogfeld die Zeichenfolge `MoveEmployeeToCity` ein, und klicken Sie anschließend auf **Fertig**.

    > [!div class="mx-imgBorder"]
    > ![Screenshot des Dialogfelds „Neue Absicht erstellen“](./media/tutorial-entity-roles/create-new-intent-move-employee-to-city.png)

1. Fügen Sie dieser Absicht mehrere voraussichtliche Beispieläußerungen von Benutzern hinzu.

    |Beispiele für Äußerungen|
    |--|
    |„move John W. Smith leaving Seattle headed to Orlando“ (John W. Smith von Seattle nach Orlando versetzen)|
    |„transfer Jill Jones from Seattle to Cairo“ (Jill Jones von Seattle nach Kairo transferieren)|
    |„Place John Jackson away from Tampa, coming to Atlanta“ (John Jackson von Tampa abziehen und nach Atlanta holen) |
    |„move Debra Doughtery to Tulsa from Chicago“ (Debra Doughtery von Chicago nach Tulsa versetzen)|
    |„mv Jill Jones leaving Cairo headed to Tampa“ (Jill Jones von Kairo nach Tampa holen)|
    |„Shift Alice Anderson to Oakland from Redmond“ (Alice Anderson von Redmond nach Oakland versetzen)|
    |„Carl Chamerlin from San Francisco to Redmond“ (Carl Chamerlin von San Francisco nach Redmond)|
    |„Transfer Steve Standish from San Diego toward Bellevue“ (Steve Standish von San Diego abziehen und nach Bellevue versetzen) |
    |„lift Tanner Thompson from Kansas city and shift to Chicago“ (Tanner Thompson von Kansas City nach Chicago versetzen)|

    > [!div class="mx-imgBorder"]
    > ![Screenshot: LUIS mit neuen Äußerungen in der MoveEmployee-Absicht](./media/tutorial-entity-roles/hr-enter-utterances.png)

## <a name="add-prebuilt-entity-geographyv2"></a>Hinzufügen der vordefinierten geographyV2-Entität

Mit der vordefinierten Entität **geographyV2** werden Standortinformationen extrahiert, u. a. Städtenamen. Da die Äußerungen zwei Städtenamen enthalten, die im Kontext miteinander in Beziehung stehen, sollten Sie Rollen zum Extrahieren dieses Kontexts verwenden.

1. Wählen Sie im linken Navigationsmenü die Option **Entitäten**.

1. Wählen Sie **+ Vordefinierte Entität hinzufügen** aus, und geben Sie dann in der Suchleiste `geo` ein, um die vordefinierten Entitäten zu filtern.

    > [!div class="mx-imgBorder"]
    > ![Hinzufügen einer vordefinierten geographyV2-Entität zur App](media/tutorial-entity-roles/add-geographyV2-prebuilt-entity.png)

1. Aktivieren Sie das Kontrollkästchen, und wählen Sie **Fertig**.

## <a name="add-roles-to-prebuilt-entity"></a>Hinzufügen von Rollen zur vordefinierten Entität

1. Wählen Sie in der Liste **Entitäten** die Option **geographyV2**, um die neue Entität zu öffnen.
1. Wählen Sie zum Hinzufügen einer Rolle **+** aus, und fügen Sie die beiden folgenden Rollen hinzu: `Origin`und `Destination`.

    > [!div class="mx-imgBorder"]
    > ![Hinzufügen von Rollen zur vordefinierten Entität](media/tutorial-entity-roles/add-roles-to-prebuilt-entity.png)

## <a name="label-entity-roles-in-example-utterances"></a>Bezeichnen von Entitätsrollen in Beispieläußerungen

1. Wählen Sie im Navigationsbereich auf der linken Seite die Option **Absichten** und dann die Absicht **MoveEmployeeToCity** aus. Beachten Sie, dass die Städtenamen mit der vordefinierten Entität **geographyV2** bezeichnet sind.
1. Wählen Sie in der Kontextsymbolleiste die **Entitätspalette** mit dem _Bleistiftsymbol_ aus.

    > [!div class="mx-imgBorder"]
    > ![Auswählen der Entitätspalette von der Inhaltssymbolleiste aus](media/tutorial-entity-roles/intent-detail-context-toolbar-select-entity-palette.png)

1. Wählen Sie die vorgefertigte Entität **geographyV2** und dann den **Entitätsinspektor** aus.
1. Wählen Sie im **Entitätsinspektor** eine Rolle aus: **Destination**. Dies ändert den Mauszeiger. Verwenden Sie den Cursor, um den Text in allen Äußerungen zu bezeichnen, bei denen es sich um den Zielort handelt.

    > [!div class="mx-imgBorder"]
    > ![Auswählen einer Rolle in der Entitätspalette](media/tutorial-entity-roles/entity-palette-select-entity-role.png)


1. Wechseln Sie zurück zum **Entitätsinspektor**, und wechseln Sie zur Rolle **Origin**. Verwenden Sie den Cursor, um den Text in allen Äußerungen zu bezeichnen, bei denen es sich um den Ursprungsort handelt.

## <a name="add-example-utterances-to-the-none-intent"></a>Hinzufügen von Beispieläußerungen zur Absicht „None“

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Trainieren der App, um die Absichtsänderungen testen zu können

Wählen Sie zum Trainieren der App **Trainieren** aus. Das Training wendet die Änderungen, z. B. die neuen Entitäten und die bezeichneten Äußerungen, auf das aktive Modell an.

## <a name="publish-the-app-to-access-it-from-the-http-endpoint"></a>Veröffentlichen der App für den Zugriff über den HTTP-Endpunkt

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]


## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Abrufen von Absicht und Entitätsvorhersage vom Endpunkt

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]


1. Ersetzen Sie in der Adressleiste am Ende der URL _IHRE_ABFRAGE_ durch `Please move Carl Chamerlin from Tampa to Portland`.

Diese Äußerung entspricht keiner der bezeichneten Äußerungen. Sie ist daher ein guter Test, und es sollte die `MoveEmployee`-Absicht mit extrahierter Entität zurückgegeben werden.

    ```json
    {
      "query": "Please move Carl Chamerlin from Tampa to Portland",
      "topScoringIntent": {
        "intent": "MoveEmployeeToCity",
        "score": 0.9706451
      },
      "intents": [
        {
          "intent": "MoveEmployeeToCity",
          "score": 0.9706451
        },
        {
          "intent": "None",
          "score": 0.0307451729
        }
      ],
      "entities": [
        {
          "entity": "tampa",
          "type": "builtin.geographyV2.city",
          "startIndex": 32,
          "endIndex": 36,
          "role": "Origin"
        },
        {
          "entity": "portland",
          "type": "builtin.geographyV2.city",
          "startIndex": 41,
          "endIndex": 48,
          "role": "Destination"
        }
      ]
    }
    ```

    The correct intent is predicted and the entities array has both the origin and destination roles in the corresponding **entities** property.

[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

## <a name="related-information"></a>Verwandte Informationen

* [Entitäten: Konzepte](luis-concept-entity-types.md)
* [Rollen: Konzepte](luis-concept-roles.md)
* [Liste mit vordefinierten Entitäten](luis-reference-prebuilt-entities.md)
* [Informationen zum Trainieren](luis-how-to-train.md)
* [Informationen zum Veröffentlichen](luis-how-to-publish-app.md)
* [Informationen zum Testen im LUIS-Portal](luis-interactive-test.md)
* [Rollen](luis-concept-roles.md)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurden eine neue Absicht erstellt und Beispieläußerungen für die im Kontext gelernten Daten von Ursprungs- und Zielort hinzugefügt. Nachdem die App trainiert und veröffentlicht ist, kann eine Clientanwendung diese Informationen für einen Verlegungsauftrag mit den relevanten Informationen verwenden.

> [!div class="nextstepaction"]
> [Hinzufügen einer zusammengesetzten Entität](luis-tutorial-composite-entity.md)
