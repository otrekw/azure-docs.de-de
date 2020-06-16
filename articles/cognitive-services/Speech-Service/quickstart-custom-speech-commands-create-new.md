---
title: 'Schnellstart: Erstellen einer App für benutzerdefinierte Befehle (Vorschau) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erstellen und testen Sie eine gehostete Anwendung für benutzerdefinierte Befehle (Vorschau). Die Anwendung verarbeitet Äußerungen.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: donkim
ms.openlocfilehash: 80111ff370f3a5412b45adc04c82c9dee103c01d
ms.sourcegitcommit: f0b206a6c6d51af096a4dc6887553d3de908abf3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84142349"
---
# <a name="quickstart-create-a-custom-commands-preview-app"></a>Schnellstart: Erstellen einer App für benutzerdefinierte Befehle (Vorschau)

In diesem Schnellstart erfahren Sie, wie Sie eine Anwendung für benutzerdefinierte Befehle erstellen und testen.
Die Anwendung verarbeitet Äußerungen wie „Schalte den Fernseher ein“ und antwortet mit einer einfachen Nachricht wie „OK, schalte Fernseher ein“.

## <a name="prerequisites"></a>Voraussetzungen

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Erstellen einer Azure Speech-Ressource<span class="docon docon-navigate-external x-hidden-focus"></span></a>

  > [!NOTE]
  > Zum gegenwärtigen Zeitpunkt unterstützen die benutzerdefinierten Befehle nur Sprachabonnements in den Regionen „westus“, „westus2“ und „neur“.

## <a name="go-to-the-speech-studio-for-custom-commands"></a>Wechseln Sie zum Speech Studio für benutzerdefinierte Befehle.

1. Wechseln Sie in einem Webbrowser zu [Speech Studio](https://speech.microsoft.com/).
1. Geben Sie Ihre Anmeldeinformationen ein, um sich beim Portal anzumelden.

   Die Standardansicht ist Ihre Liste der Speech-Abonnements.
    > [!NOTE]
    > Wenn die Seite „Abonnement auswählen“ nicht angezeigt wird, können Sie dorthin navigieren, indem Sie **Speech-Ressourcen** aus dem Einstellungenmenü am oberen Rand des Bildschirms auswählen.

1. Wählen Sie Ihr Speech-Abonnement und dann **Zu Studio wechseln** aus.
1. Wählen Sie **Benutzerdefinierte Befehle** aus.

     Die Standardansicht ist eine Liste der Anwendungen für benutzerdefinierte Befehle, die in Ihrem ausgewählten Abonnement vorhanden sind.

## <a name="create-a-custom-commands-project"></a>Erstellen eines Projekts für benutzerdefinierte Befehle

1. Wählen Sie **Neues Projekt** aus, um ein Projekt zu erstellen.

   > [!div class="mx-imgBorder"]
   > ![Erstellen eines Projekts](media/custom-speech-commands/create-new-project.png)

1. Geben Sie in das Feld **Name** einen Projektnamen ein.
1. Wählen Sie in der Liste **Sprache** eine Sprache aus.
1. Wählen Sie in der Liste **LUIS-Erstellungsressource** eine Erstellungsressource aus. Sind keine gültigen Erstellungsressourcen vorhanden, erstellen Sie eine, indem Sie **Neue LUIS-Erstellungsressource erstellen** auswählen.

   > [!div class="mx-imgBorder"]
   > ![Erstellen einer Ressource](media/custom-speech-commands/create-new-resource.png)

   1. Geben Sie im Feld **Ressourcenname** den Namen der Ressource ein.
   1. Wählen Sie in der Liste **Ressourcengruppe** eine Ressourcengruppe aus.
   1. Wählen Sie in der Liste **Standort** einen Standort aus.
   1. Wählen Sie in der Liste **Tarif** einen Tarif aus.

      > [!NOTE]
      > Sie können eine Ressourcengruppe erstellen, indem Sie einen Ressourcengruppennamen in das Feld **Ressourcengruppe** eingeben. Die Ressourcengruppe wird erstellt, wenn Sie **Erstellen** auswählen.

1. Klicken Sie auf **Erstellen**.
1. Sobald das Projekt erstellt ist, wählen Sie es aus.

    Sie sollten jetzt eine Übersicht über Ihre neue Anwendung für benutzerdefinierte Befehle sehen.

## <a name="update-luis-resources-optional"></a>Aktualisieren von LUIS-Ressourcen (optional)

Sie können die ausgewählte Erstellungsressource im Fenster **Neues Projekt** aktualisieren und eine Vorhersageressource festlegen. Die Vorhersageressource wird zur Erkennung verwendet, wenn Ihre Anwendung für benutzerdefinierte Befehle veröffentlicht ist. Während der Entwicklungs- und Testphasen benötigen Sie keine Vorhersageressource.

1. Wählen Sie im linken Bereich **Einstellungen** aus, und wählen Sie dann im mittleren Bereich **LUIS-Ressourcen** aus.
1. Wählen Sie eine Vorhersageressource aus, oder erstellen Sie eine, indem Sie **Neue Ressource erstellen** auswählen.
1. Wählen Sie **Speichern** aus.
    
    > [!div class="mx-imgBorder"]
    > ![Festlegen von LUIS-Ressourcen](media/custom-speech-commands/set-luis-resources.png)


> [!NOTE]
> Da die Erstellungsressource nur 1.000 Anforderungen von Vorhersage-Endpunkten pro Monat unterstützt, müssen Sie vor dem Veröffentlichen Ihrer Anwendung für benutzerdefinierte Befehle eine LUIS-Vorhersageressource festlegen.


## <a name="create-a-command"></a>Erstellen eines Befehls

Erstellen wir einen einfachen Befehl, der eine einzelne Äußerung `turn on the tv` akzeptiert und mit der Nachricht `Ok, turning on the tv` antwortet.

1. Erstellen Sie einen Befehl, indem Sie oben im linken Bereich **Neuer Befehl** auswählen. Das Fenster **Neuer Befehl** wird geöffnet.
1. Geben Sie in das Feld **Name** **TurnOn** ein.
1. Klicken Sie auf **Erstellen**.

Im mittleren Bereich werden die Eigenschaften des Befehls aufgelistet:


| Konfiguration            | BESCHREIBUNG                                                                                                                 |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **Beispielsätze** | Beispieläußerungen, die der Benutzer äußern kann, um diesen Befehl auszulösen.                                                                 |
| **Parameter**       | Informationen, die zur Vervollständigung des Befehls erforderlich sind.                                                                                |
| **Vervollständigungsregeln** | Aktionen, die zur Erfüllung des Befehls durchgeführt werden. Beispielsweise, um dem Benutzer zu antworten oder mit einem anderen Webdienst zu kommunizieren. |
| **Interaktionsregeln**   | Zusätzliche Regeln für den Umgang mit spezifischeren oder komplexeren Situationen.                                                              |


> [!div class="mx-imgBorder"]
> ![Erstellen eines Befehls](media/custom-speech-commands/create-add-command.png)


### <a name="add-example-sentences"></a>Hinzufügen von Beispielsätzen

Beginnen wir mit dem Abschnitt **Beispielsätze**. Wir geben Ihnen ein Beispiel für mögliche Äußerungen des Benutzers.

1. Wählen Sie im mittleren Bereich **Beispielsätze** aus. 
1. Fügen Sie im rechten Bereich Beispiele hinzu:

    ```
    turn on the tv
    ```

1. Wählen Sie oben im Bereich **Speichern** aus.

Im Moment verfügen wir nicht über Parameter, sodass wir zum Abschnitt mit **Vervollständigungsregeln** übergehen können.

### <a name="add-a-completion-rule"></a>Hinzufügen einer Vervollständigungsregel

Fügen Sie nun eine Vervollständigungsregel mit der folgenden Konfiguration hinzu. Diese Regel teilt dem Benutzer mit, dass ein Vervollständigungsvorgang durchgeführt wird.


| Einstellung    | Vorgeschlagener Wert                          | Beschreibung                                        |
| ---------- | ---------------------------------------- | -------------------------------------------------- |
| **Name**  | **ConfirmationResponse**                  | Ein Name, der den Zweck der Regel beschreibt.          |
| **Bedingungen** | Keine                                     | Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann.    |
| **Aktionen**    | **Sprachantwort senden -> OK, schalte Fernseher ein** | Die durchzuführende Aktion, wenn die Bedingung der Regel erfüllt ist (true). |

1. Erstellen Sie eine neue Vervollständigungsregel, indem Sie oben im mittleren Bereich **Hinzufügen** auswählen.
1. Geben Sie im Feld **Name** einen Namen ein.
1. Hinzufügen einer Aktion.
   1. Erstellen Sie eine Aktion, indem Sie im Abschnitt **Aktionen** **Neue Aktion hinzufügen** auswählen.
   1. Wählen Sie im Fenster **Aktion bearbeiten** in der Liste **Typ** die Option **Sprachantwort senden** aus.
   1. Wählen Sie unter **Antwort** die Option **Einfacher Editor** aus. Geben Sie im Feld **Erste Variation** die Option **OK, schalte Fernseher ein** ein.

   > [!div class="mx-imgBorder"]
   > ![Erstellen einer Antwort](media/custom-speech-commands/create-speech-response-action.png)

1. Wählen Sie **Speichern** aus, um die Regel zu speichern.
1. Wählen Sie wieder im Abschnitt **Vervollständigungsregeln** **Speichern** aus, um alle Änderungen zu speichern. 

> [!div class="mx-imgBorder"]
> ![Erstellen einer Vervollständigungsregel](media/custom-speech-commands/create-basic-completion-response-rule.png)



## <a name="try-it-out"></a>Ausprobieren

Testen Sie das Verhalten über den Testchatbereich.
1. Klicken Sie oben im rechten Bereich auf **Trainieren**.
1. Wenn das Training abgeschlossen ist, wählen Sie **Testen** aus. Es wird ein neues Fenster **Testen Ihrer Anwendung** angezeigt.
    - Geben Sie **Schalte den Fernseher ein** ein.
    - Erwartete Antwort: **OK, schalte Fernseher ein**


> [!div class="mx-imgBorder"]
> ![Testen des Verhaltens](media/custom-speech-commands/create-basic-test-chat.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Schnellstart: Erstellen einer Anwendung für benutzerdefinierte Befehle (Vorschau) mit Parametern](./quickstart-custom-speech-commands-create-parameters.md)
