---
title: 'Gewusst wie: Erstellen einer Anwendung mit einfachen Befehlen – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erfahren Sie, wie Sie eine gehostete Anwendung für benutzerdefinierte Befehle mithilfe von einfachen Befehlen erstellen und testen.
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: sausin
ms.openlocfilehash: d166257dd28773d89a4f1fd56de3cb1a22242523
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87284165"
---
# <a name="create-application-with-simple-commands"></a>Erstellen einer Anwendung mit einfachen Befehlen

In diesem Artikel werden folgende Vorgehensweisen behandelt:
 - Erstellen einer leeren Anwendung
 - Aktualisieren von LUIS-Ressourcen
 - Hinzufügen einiger grundlegender Befehle zu Ihrer Anwendung für benutzerdefinierte Befehle

## <a name="create-empty-application"></a>Erstellen einer leeren Anwendung
Erstellen Sie eine leere Anwendung für benutzerdefinierte Befehle. Weitere Informationen finden Sie in der [Schnellstartanleitung](quickstart-custom-commands-application.md). Anstatt ein Projekt zu importieren, erstellen Sie diesmal ein leeres Projekt.

1. Geben Sie im Feld **Name** den Projektnamen als `Smart-Room-Lite` (oder etwas ähnliches) ein.
1. Wählen Sie in der Liste **Sprache** die Option **Englisch (USA)** aus.
1. Wählen Sie eine LUIS-Ressource Ihrer Wahl aus, oder erstellen Sie eine.

   > [!div class="mx-imgBorder"]
   > ![Erstellen eines Projekts](media/custom-commands/create-new-project.png)

## <a name="update-luis-resources-optional"></a>Aktualisieren von LUIS-Ressourcen (optional)

Sie können die ausgewählte Erstellungsressource im Fenster **Neues Projekt** aktualisieren und eine Vorhersageressource festlegen. Die Vorhersageressource wird zur Erkennung verwendet, wenn Ihre Anwendung für benutzerdefinierte Befehle veröffentlicht ist. Während der Entwicklungs- und Testphasen benötigen Sie keine Vorhersageressource.

## <a name="add-turnon-command"></a>Hinzufügen des Befehls „TurnOn“

Fügen Sie in der Anwendung **Smart-Room-Lite** für benutzerdefinierte Befehle, die Sie soeben erstellt haben, einen einfachen Befehl hinzu, der eine Äußerung `turn on the tv` verarbeitet und mit der Nachricht `Ok, turning the tv on` antwortet.

1. Erstellen Sie einen neuen Befehl, indem Sie oben im linken Bereich **Neuer Befehl** auswählen. Das Fenster **Neuer Befehl** wird geöffnet.
1. Geben Sie den Wert für das Feld **Name** als **TurnOn** ein.
1. Klicken Sie auf **Erstellen**.

Im mittleren Bereich werden die verschiedenen Eigenschaften des Befehls aufgelistet. Sie konfigurieren die folgenden Eigenschaften des Befehls. Eine Erläuterung aller Konfigurationseigenschaften eines Befehls finden Sie unter [Referenzen](./custom-commands-references.md).

| Konfiguration            | BESCHREIBUNG                                                                                                                 |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **Beispielsätze** | Beispieläußerungen, die der Benutzer äußern kann, um diesen Befehl auszulösen.                                                                 |
| **Parameter**       | Informationen, die zur Vervollständigung des Befehls erforderlich sind.                                                                                |
| **Vervollständigungsregeln** | Die Aktionen, die zur Erfüllung des Befehls durchzuführen sind. Beispielsweise, um dem Benutzer zu antworten oder mit einem anderen Webdienst zu kommunizieren. |
| **Interaktionsregeln**   | Zusätzliche Regeln für den Umgang mit spezifischeren oder komplexeren Situationen.                                                              |


> [!div class="mx-imgBorder"]
> ![Erstellen eines Befehls](media/custom-commands/add-new-command.png)

### <a name="add-example-sentences"></a>Hinzufügen von Beispielsätzen

Lassen Sie uns mit dem Abschnitt **Beispielsatz** beginnen und ein Beispiel dafür bereitstellen, was der Benutzer sagen kann.

1. Wählen Sie im mittleren Bereich den Abschnitt **Beispielsätze** aus.
1. Fügen Sie im rechten Bereich entsprechende Beispiele hinzu:

    ```
    turn on the tv
    ```

1.  Wählen Sie oben im Bereich **Speichern** aus.

Im Moment verfügen wir nicht über Parameter, sodass wir zum Abschnitt mit **Vervollständigungsregeln** übergehen können.

### <a name="add-a-completion-rule"></a>Hinzufügen einer Vervollständigungsregel

Als nächstes muss der Befehl über eine Vervollständigungsregel verfügen. Diese Regel teilt dem Benutzer mit, dass ein Vervollständigungsvorgang durchgeführt wird. Weitere Informationen zu Regeln und Vervollständigungsregeln finden Sie unter [Referenzen](./custom-commands-references.md).

1. Wählen Sie die Standardvervollständigungsregel **Erledigt** aus, und bearbeiten Sie sie wie folgt:

    
    | Einstellung    | Vorgeschlagener Wert                          | Beschreibung                                        |
    | ---------- | ---------------------------------------- | -------------------------------------------------- |
    | **Name**       | ConfirmationResponse                  | Ein Name, der den Zweck der Regel beschreibt.          |
    | **Bedingungen** | Keine                                     | Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann.    |
    | **Aktionen**    | Sprachantwort senden > Einfacher Editor > Erste Variation > `Ok, turning the tv on` | Die durchzuführende Aktion, wenn die Bedingung der Regel erfüllt ist (true). |
    



   > [!div class="mx-imgBorder"]
   > ![Erstellen einer Sprachantwort](media/custom-commands/create-speech-response-action.png)

1. Wählen Sie **Speichern** aus, um die Aktion zu speichern.
1. Wählen Sie wieder im Abschnitt **Vervollständigungsregeln** **Speichern** aus, um alle Änderungen zu speichern. 


    > [!NOTE]
    > Es ist nicht erforderlich, die Standard-Vervollständigungsregel für den Befehl zu verwenden. Bei Bedarf können Sie die vorhandene Standard-Vervollständigungsregel löschen und eine eigene Regel hinzufügen.

### <a name="try-it-out"></a>Ausprobieren

Testen Sie das Verhalten über den Testchatbereich.
1. Wählen Sie das **Trainieren**-Symbol aus, das oben im rechten Bereich angezeigt wird.
1. Nachdem das Training abgeschlossen ist, wählen Sie **Testen** aus. Probieren Sie die folgende Äußerung per Sprache/Text aus:
    - Eingabe: Schalte Fernseher ein
    - Ausgabe: OK, Fernseher wird eingeschaltet


> [!div class="mx-imgBorder"]
> ![Testen mit Webchat](media/custom-commands/create-basic-test-chat.png)

> [!TIP]
> Im Testbereich können Sie **Rundendetails** auswählen, um Informationen darüber zu erhalten, wie diese Sprach-/Texteingabe verarbeitet wurde.  

## <a name="add-settemperature-command"></a>Hinzufügen des Befehls „SetTemperature“

Fügen Sie jetzt einen weiteren **SetTemperature**-Befehl hinzu, der eine einzelne Äußerung, `set the temperature to 40 degrees`, erfordert und mit der Nachricht `Ok, setting temperature to 40 degrees` antwortet.

Befolgen Sie die Schritte wie für den Befehl **TurnOn** dargestellt, um einen neuen Befehl mit dem Beispielsatz „**Stelle die Temperatur auf 5 Grad ein**“ zu erstellen.

Bearbeiten Sie dann die vorhandenen **Erledigt**-Vervollständigungsregeln wie folgt:

| Einstellung    | Vorgeschlagener Wert                          |
| ---------- | ---------------------------------------- |
| Name  | ConfirmationResponse                  |
| Bedingungen | Keine                                     |
| Aktionen    | Sprachantwort senden > Einfacher Editor > Erste Variation > `Ok, setting temperature to 40 degrees` |

Wählen Sie **Speichern** aus, um sämtliche Änderungen am Befehl zu speichern.

## <a name="add-setalarm-command"></a>Hinzufügen des Befehls „SetAlarm“
Erstellen Sie einen neuen **SetAlarm**-Befehl mit dem Beispielsatz „**Alarm für morgen 9 Uhr einstellen**“. Bearbeiten Sie dann die vorhandenen **Erledigt**-Vervollständigungsregeln wie folgt:

| Einstellung    | Vorgeschlagener Wert                          |
| ---------- | ---------------------------------------- |
| Regelname  | ConfirmationResponse                  |
| Bedingungen | Keine                                     |
| Aktionen    | Sprachantwort senden > Einfacher Editor > Erste Variation > `Ok, setting an alarm for 9 am tomorrow` |

Wählen Sie **Speichern** aus, um sämtliche Änderungen am Befehl zu speichern.

## <a name="try-it-out"></a>Ausprobieren

Testen Sie das Verhalten über den Testchatbereich.
1. Wählen Sie **Trainieren** aus. Nach erfolgreichem Training wählen Sie **Testen** aus, und probieren Sie dann Folgendes aus:
    - Eingabe: Stelle die Temperatur auf 5 Grad ein
    - Erwartete Antwort: OK, stelle die Temperatur auf 5 Grad ein
    - Sie geben ein: Schalte den Fernseher ein
    - Erwartete Antwort: OK, Fernseher wird eingeschaltet
    - Eingabe: Alarm für morgen 9 Uhr einstellen
    - Erwartete Antwort: OK, lege einen Alarm für morgen 9 Uhr fest

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Vorgehensweise: Hinzufügen von Parametern zu Befehlen](./how-to-custom-commands-add-parameters-to-commands.md)
