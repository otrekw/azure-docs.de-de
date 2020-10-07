---
title: 'Schnellstart: Erstellen einer neuen App im LUIS-Portal'
description: In dieser Schnellstartanleitung erstellen Sie die grundlegenden Komponenten einer App sowie Absichten und Entitäten. Außerdem verwenden Sie zu Testzwecken eine Beispieläußerung im LUIS-Portal.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 05/19/2020
ms.openlocfilehash: 38fffd7793e1f5bd59ac6dde4499b2eb25009b52
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91303847"
---
# <a name="quickstart-create-a-new-app-in-the-luis-portal"></a>Schnellstart: Erstellen einer neuen App im LUIS-Portal

In diesem Schnellstart erstellen Sie eine neue App im LUIS-Portal. Erstellen Sie zunächst die Grundbestandteile einer App sowie **Absichten** und **Entitäten**. Testen Sie anschließend die App, indem Sie im interaktiven Testbereich eine Beispielbenutzeräußerung eingeben, um die vorhergesagte Absicht zu erhalten.

[!INCLUDE [Sign in to LUIS](./includes/sign-in-process.md)]

## <a name="create-an-app"></a>Erstellen einer App

1. Wählen Sie auf der Kontextsymbolleiste die Option **+ New app for conversation** (+ Neue App für die Konversation) und anschließend erneut **+ New app for conversation** (+ Neue App für die Konversation) aus.

    > [!div class="mx-imgBorder"]
    > [![Screenshot: Erstellen einer neuen App im LUIS-Portal](./media/create-app-in-portal.png)](./media/create-app-in-portal.png#lightbox)

1. Konfigurieren Sie die App im Popupfenster mit den folgenden Einstellungen, und wählen Sie dann **Fertig** aus.

   |Einstellungsname| Wert | Zweck|
   |--|--|--|
   |Name|`myEnglishApp`|Eindeutiger Name der LUIS-App<br>required|
   |Kultur|**Englisch**|Sprache der Äußerungen von Benutzern, **en-us**<br>required|
   |Beschreibung (optional)|`App made with LUIS Portal`|Beschreibung der App<br>optional|
   |Vorhersageressource (optional) |-  |Nicht auswählen. Sie erhalten von LUIS einen Startschlüssel für die kostenlose Erstellung sowie für 1.000 Vorhersageendpunktanforderungen. |

   ![Screenshot: Eingabe neuer App-Einstellungen](./media/get-started-portal-build-app/create-new-app-settings.png)

## <a name="create-intents"></a>Erstellen von Absichten

Nachdem die LUIS-App erstellt wurde, müssen Sie Absichten erstellen. Absichten dienen zum Klassifizieren der Texteingaben von Benutzern. Beispielsweise kann eine Personalverwaltungs-App zwei Funktionen aufweisen. Personen dabei zu helfen:

 1. Stellen zu finden und sich auf sie zu bewerben
 1. Formulare für die Bewerbung auf Stellen zu finden

Die zwei verschiedenen _Absichten_ der App richten sich an den folgenden Absichten aus:

|Intent|Beispieltext vom Benutzer<br>bezeichnet als _Äußerung_|
|--|--|
|ApplyForJob|`I want to apply for the new software engineering position in Cairo.`|
|FindForm|`Where is the job transfer form hrf-123456?`|

Führen Sie die folgenden Schritte aus, um Absichten zu erstellen:

1. Nachdem die App erstellt wurde, wird die Seite **Intents** (Absichten) des Abschnitts **Erstellen** angezeigt. Klicken Sie auf **Erstellen**.

   [![Screenshot: Auswählen von „Erstellen“ zum Erstellen einer neuen Absicht](./media/get-started-portal-build-app/create-new-intent-button.png)](./media/get-started-portal-build-app/create-new-intent-button.png#lightbox)

1. Geben Sie den Namen der Absicht ein, `FindForm`, und wählen Sie dann **Fertig** aus.

## <a name="add-an-example-utterance"></a>Hinzufügen einer Beispieläußerung

Nach dem Erstellen von Absichten fügen Sie Beispieläußerungen hinzu. Beispieläußerungen sind Text, den ein Benutzer in einen Chatbot oder eine andere Clientanwendung eingibt. Sie bilden die Absicht des Texts des Benutzers auf eine LUIS-Absicht ab.

Für die `FindForm`-Absicht dieser Beispielanwendung gehört zu den Beispieläußerungen die Formularnummer. Die Clientanwendung benötigt die Formularnummer, um die Anforderung des Benutzers zu erfüllen, daher ist es wichtig, sie in die Äußerung einzuschließen.

> [!div class="mx-imgBorder"]
> [![Screenshot: Eingeben von Beispieläußerungen für die Absicht „FindForm“](./media/get-started-portal-build-app/add-example-utterance.png)](./media/get-started-portal-build-app/add-example-utterance.png#lightbox)

Fügen Sie der `FindForm`-Absicht die folgenden 15 Beispieläußerungen hinzu.

|#|Beispiele für Äußerungen|
|--|--|
|1|`Looking for hrf-123456`|
|2|`Where is the human resources form hrf-234591?`|
|3|`hrf-345623, where is it`|
|4|`Is it possible to send me hrf-345794`|
|5|`Do I need hrf-234695 to apply for an internal job?`|
|6|`Does my manager need to know I'm applying for a job with hrf-234091`|
|7|`Where do I send hrf-234918? Do I get an email response it was received?`|
|8|`hrf-234555`|
|9|`When was hrf-234987 updated?`|
|10|`Do I use form hrf-876345 to apply for engineering positions`|
|11|`Was a new version of hrf-765234 submitted for my open req?`|
|12|`Do I use hrf-234234 for international jobs?`|
|13|`hrf-234598 spelling mistake`|
|14|`will hrf-234567 be edited for new requirements`|
|15|`hrf-123456, hrf-123123, hrf-234567`|

Diese Beispieläußerungen unterscheiden sich entwurfsbedingt in folgenden Punkten:

* Länge der Äußerung
* [Interpunktion](luis-reference-application-settings.md#punctuation-normalization)
* Wortwahl
* Zeitform des Verbs (is, was, will be)
* Reihenfolge der Wörter


## <a name="create-a-regular-expression-entity"></a>Erstellen einer Entität vom Typ „Regulärer Ausdruck“

Um in der Vorhersageantwort zur Laufzeit die Formularnummer zurückzugeben, muss die Formularnummer als Entität extrahiert werden. Da der Text mit der Formularnummer sehr strukturiert ist, können Sie ihn eine Entität vom Typ „RegEx“ verwenden. Gehen Sie wie folgt vor, um die Entität vom Typ „RegEx“ zu erstellen:

1. Wählen Sie im Menü links die Option **Entitäten** aus.

1. Wählen Sie auf der Seite **Entitäten** die Option **Erstellen** aus.

1. Geben Sie den Namen `FormNumber` ein, und wählen Sie den Entitätstyp **Regex** aus.

1. Geben Sie den regulären Ausdruck `hrf-[0-9]{6}` in das Feld **Regex** ein. Dieser Eintrag entspricht den Literalzeichen `hrf-` und lässt genau sechs Ziffern zu. Wählen Sie anschließend **Erstellen** aus.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Erstellen einer Entität vom Typ „RegEx“](./media/get-started-portal-build-app/create-regular-expression-entity.png)


    Diese Entität extrahiert jeden Text, der mit dem regulären Ausdruck in einer der Absichten übereinstimmt.

## <a name="add-example-utterances-to-the-none-intent"></a>Hinzufügen von Beispieläußerungen zur Absicht „None“

Die Absicht **None** ist die Fallbackabsicht und sollte nicht leer gelassen werden. Diese Absicht sollte eine Äußerung für jeweils 10 Beispieläußerungen enthalten, die Sie für die anderen Absichten der App hinzugefügt haben.

Die Beispieläußerungen der Absicht **None** dürfen nicht im Bereich der Clientanwendung enthalten sein.

1. Klicken Sie im linken Menü auf **Intents** (Absichten), und wählen Sie dann in der Liste der Absichten **None** (Keine) aus.

1. Fügen Sie der Absicht die folgenden Beispieläußerungen hinzu.

   |Beispieläußerungen für die Absicht „None“|
   |--|
   |`Barking dogs are annoying`|
   |`Penguins in the ocean`|

   Diese Beispieläußerungen sind für die aktuelle App nicht relevant. Falls Begriffe für Tiere oder den Ozean für Ihre App relevant sind, müssen für die Absicht **None** andere Beispieläußerungen verwendet werden.

## <a name="train-the-app"></a>Trainieren der App

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="look-at-the-regular-expression-entity-in-the-example-utterances"></a>Überprüfen der Entität vom Typ „Regulärer Ausdruck“ in den Beispieläußerungen

1. Überprüfen Sie, ob die Entität in der Absicht **FindForm** gefunden wird, indem Sie im linken Menü **Intents** (Absichten) auswählen. Wählen Sie dann die Absicht **FindForm** aus.

   Die Entität ist an den Stellen markiert, an denen sie in den Beispieläußerungen vorkommt.

   > [!div class="mx-imgBorder"]
   > [![Screenshot: Alle Beispieläußerungen mit markierten Entitäten](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png)](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png#lightbox)

## <a name="test-your-new-app-with-the-interactive-test-pane"></a>Testen der neuen App im interaktiven Testbereich

Verwenden Sie den interaktiven **Testbereich** im LUIS-Portal, um zu überprüfen, ob die Entität aus neuen Äußerungen, die die App noch nicht kennt, extrahiert wird.

1. Wählen Sie im Menü oben rechts die Option **Testen** aus.

1. Fügen Sie eine neue Äußerung hinzu, und drücken Sie dann die EINGABETASTE:

   ```Is there a form named hrf-234098```

    Wählen Sie **Überprüfen** aus, um Entitätsvorhersagen anzuzeigen.

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Testen neuer Äußerungen im Testbereich](./media/get-started-portal-build-app/test-new-utterance.png)

   Die am stärksten vorhergesagte Absicht ist richtigerweise **FindForm** mit einer Zuverlässigkeit von mehr als 90 % (0,977). Die Entität **FormNumber** wird mit dem Wert „hrf-234098“ extrahiert.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit diesem Schnellstart fertig sind und nicht mit dem nächsten Schnellstart fortfahren möchten, wählen Sie im oberen Navigationsmenü **Meine Apps** aus. Aktivieren Sie dann in der Liste das Kontrollkästchen links neben der App, und wählen Sie auf der Kontextsymbolleiste über der Liste die Option **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [2. Bereitstellen einer App](get-started-portal-deploy-app.md)
