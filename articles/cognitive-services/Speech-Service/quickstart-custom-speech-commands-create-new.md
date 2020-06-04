---
title: 'Schnellstart: Erstellen eines benutzerdefinierten Befehls (Vorschauversion) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erstellen und testen Sie eine gehostete Anwendung für benutzerdefinierte Befehle.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: donkim
ms.openlocfilehash: f31d7279b73bab7aefda4c4b6570500d05cb89d7
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82872529"
---
# <a name="quickstart-create-a-custom-commands-app-preview"></a>Schnellstart: Erstellen einer App für benutzerdefinierte Befehle (Vorschau)

In diesem Schnellstart erfahren Sie, wie Sie eine Anwendung für benutzerdefinierte Befehle erstellen und testen.
Die erstellte Anwendung verarbeitet Äußerungen wie „Schalte den Fernseher ein“ und antwortet mit einer einfachen Nachricht „OK, schalte Fernseher ein“.

## <a name="prerequisites"></a>Voraussetzungen

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Erstellen einer Azure Speech-Ressource<span class="docon docon-navigate-external x-hidden-focus"></span></a>

  > [!NOTE]
  > Zum gegenwärtigen Zeitpunkt unterstützen die benutzerdefinierten Befehle nur Sprachabonnements in den Regionen „westus“, „westus2“ und „neur“.

## <a name="go-to-the-speech-studio-for-custom-commands"></a>Wechseln Sie zum Speech Studio für benutzerdefinierte Befehle.

1. Öffnen Sie Ihren Webbrowser, und navigieren Sie zum [Speech Studio](https://speech.microsoft.com/).
1. Geben Sie Ihre Anmeldeinformationen ein, um sich beim Portal anzumelden.

   - Die Standardansicht ist Ihre Liste der Speech-Abonnements.
     > [!NOTE]
     > Wenn die Seite „Abonnement auswählen“ nicht angezeigt wird, können Sie dorthin navigieren, indem Sie „Speech-Ressourcen“ aus dem Einstellungsmenü in der oberen Leiste auswählen.

1. Wählen Sie Ihr Speech-Abonnement und dann **Zu Studio wechseln** aus.
1. Wählen Sie **Benutzerdefinierte Befehle** aus.

     - Die Standardansicht ist eine Liste der Anwendungen für benutzerdefinierte Befehle, die in Ihrem ausgewählten Abonnement vorhanden sind.

## <a name="create-a-custom-commands-project"></a>Erstellen eines Projekts für benutzerdefinierte Befehle

1. Wählen Sie **Neues Projekt** aus, um ein neues Projekt zu erstellen.

   > [!div class="mx-imgBorder"]
   > ![Erstellen eines Projekts](media/custom-speech-commands/create-new-project.png)

1. Geben Sie den Projektnamen ein.
1. Wählen Sie in der Dropdownliste eine Sprache aus.
1. Wählen Sie in der Dropdownliste eine Erstellungsressource aus. Sind keine gültigen Erstellungsressourcen vorhanden, erstellen Sie eine, indem Sie auf **Neue LUIS-Erstellungsressource erstellen** klicken.

   > [!div class="mx-imgBorder"]
   > ![Erstellen einer Ressource](media/custom-speech-commands/create-new-resource.png)

   - Geben Sie den Ressourcennamen und die Ressourcengruppe ein.
   - Wählen Sie in der Dropdownliste Werte für Speicherort und Preisstufe aus.

      > [!NOTE]
      > Sie können Ressourcengruppen erstellen, indem Sie den gewünschten Ressourcengruppennamen in das Feld „Ressourcengruppe“ eingeben. Die Ressourcengruppe wird erstellt, wenn **Erstellen** ausgewählt wird.

1. Wählen Sie als nächstes **Erstellen** aus, um das Projekt zu erstellen.
1. Wählen Sie nach der Erstellung Ihr Projekt aus.

    - Ihre Ansicht sollte jetzt eine Übersicht über Ihre neu erstellte Anwendung für benutzerdefinierte Befehle darstellen.

## <a name="update-luis-resources-optional"></a>Aktualisieren von LUIS-Ressourcen (optional)

Sie können im Fenster „Neues Projekt“ die festgelegte Erstellungsressource aktualisieren und eine Vorhersageressource festlegen. Die Vorhersageressource wird zur Erkennung verwendet, sobald Ihre Anwendung für benutzerdefinierte Befehle veröffentlicht ist. Für die Entwicklungs- und Testphasen benötigen Sie keine Vorhersageressource.

1. Wählen Sie im linken Bereich **Einstellungen** aus, und navigieren Sie dann zum Abschnitt **LUIS-Ressourcen** im mittleren Bereich.
1. Wählen Sie eine Vorhersageressource aus, oder erstellen Sie eine, indem Sie **Neue Ressource erstellen** auswählen.
1. Wählen Sie **Speichern** aus.
    
    > [!div class="mx-imgBorder"]
    > ![Festlegen von LUIS-Ressourcen](media/custom-speech-commands/set-luis-resources.png)


> [!NOTE]
> Da die Erstellungsressource nur 1.000 Anforderungen von Vorhersage-Endpunkten pro Monat unterstützt, ist das Festlegen einer LUIS-Vorhersageressource vor dem Veröffentlichen Ihrer Anwendung für benutzerdefinierte Befehle obligatorisch.


## <a name="create-a-new-command"></a>Erstellen eines neuen Befehls

Erstellen wir einen einfachen Befehl, der eine einzelne Äußerung `turn on the tv` akzeptiert und mit der Nachricht `Ok, turning on the tv` antwortet.

1. Erstellen Sie einen neuen Befehl, indem Sie das `+ New command`-Symbol auswählen, das oben im äußerst linken Abschnitt angezeigt wird. Es wird ein neues Popupfenster mit dem Titel **Neuer Befehl** angezeigt.
1. Geben Sie den Wert für das Feld **Name** als `TurnOn` ein.
1. Klicken Sie auf **Erstellen**.

Im mittleren Bereich werden die verschiedenen Eigenschaften eines Befehls aufgelistet:


| Konfiguration            | BESCHREIBUNG                                                                                                                 |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| Beispielsätze | Beispieläußerungen, die der Benutzer äußern kann, um diesen Befehl auszulösen.                                                                 |
| Parameter       | Informationen, die zur Vervollständigung des Befehls erforderlich sind.                                                                                |
| Vervollständigungsregeln | Die Aktionen, die zur Erfüllung des Befehls durchzuführen sind. Beispielsweise, um dem Benutzer zu antworten oder mit einem anderen Webdienst zu kommunizieren. |
| Interaktionsregeln   | Zusätzliche Regeln für den Umgang mit spezifischeren oder komplexeren Situationen.                                                              |


> [!div class="mx-imgBorder"]
> ![Erstellen eines Befehls](media/custom-speech-commands/create-add-command.png)


### <a name="add-example-sentences"></a>Hinzufügen von Beispielsätzen

Lassen Sie uns mit dem Beispielsatzabschnitt beginnen und ein Beispiel dafür bereitstellen, was der Benutzer sagen kann.

1. Wählen Sie im mittleren Bereich den Abschnitt **Beispielsätze** aus, und fügen Sie im äußerst rechten Bereich Beispiele hinzu:

    ```
    turn on the tv
    ```

1. Wählen Sie das `Save`-Symbol aus, das oben in diesem Bereich angezeigt wird.

Im Moment verfügen wir über keine Parameter, sodass wir zu dem Abschnitt mit den **Vervollständigungsregeln** übergehen können.

### <a name="add-a-completion-rule"></a>Hinzufügen einer Vervollständigungsregel

Fügen Sie jetzt eine Vervollständigungsregel mit der folgenden Konfiguration hinzu. Diese Regel zeigt dem Benutzer an, dass ein Vervollständigungsvorgang durchgeführt wird.


| Einstellung    | Vorgeschlagener Wert                          | BESCHREIBUNG                                        |
| ---------- | ---------------------------------------- | -------------------------------------------------- |
| Regelname  | ConfirmationResponse                  | Ein Name, der den Zweck der Regel beschreibt.          |
| Bedingungen | Keine                                     | Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann.    |
| Aktionen    | SpeechResponse „OK, schalte Fernseher ein“ | Die durchzuführende Aktion, wenn die Bedingung der Regel erfüllt ist (true). |

1. Erstellen Sie eine neue Vervollständigungsregel, indem Sie das `+Add`-Symbol oben im mittleren Bereich auswählen.
1. Geben Sie einen Wert im Abschnitt **Name** an.
1. Hinzufügen einer Aktion
   1. Erstellen Sie eine neue Aktion, indem Sie im Abschnitt **Aktionen** **Neue Aktion hinzufügen** auswählen.
   1. Wählen Sie im Popup **Neue Aktion** in den Dropdownoptionen für **Typ** `Send speech response` aus.
   1. Wählen Sie `Simple editor` für das Feld **Antwort** aus.
       - Geben Sie im Feld **Erste Variation** den Wert für die Antwort als `Ok, turning on the tv` ein.

   > [!div class="mx-imgBorder"]
   > ![Erstellen einer Sprachantwort](media/custom-speech-commands/create-speech-response-action.png)

1. Klicken Sie auf **Speichern**, um die Regel zu speichern.
1. Wählen Sie wieder im Abschnitt **Vervollständigungsregeln** **Speichern** aus, um alle Änderungen zu speichern. 

> [!div class="mx-imgBorder"]
> ![Erstellen einer Vervollständigungsregel](media/custom-speech-commands/create-basic-completion-response-rule.png)



## <a name="try-it-out"></a>Ausprobieren

Testen Sie das Verhalten über den Testchatbereich.
1. Wählen Sie das `Train`-Symbol aus, das oben im rechen Bereich angezeigt wird.
1. Nachdem das Training abgeschlossen ist, wählen Sie `Test` aus. Es wird ein neues Fenster **Testen Ihrer Anwendung** angezeigt.
    - Sie geben ein: Schalte den Fernseher ein
    - Erwartete Antwort: OK, schalte Fernseher ein


> [!div class="mx-imgBorder"]
> ![Testen mit Webchat](media/custom-speech-commands/create-basic-test-chat.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Schnellstart: Erstellen eines benutzerdefinierten Befehls mit Parametern (Vorschau)](./quickstart-custom-speech-commands-create-parameters.md)
