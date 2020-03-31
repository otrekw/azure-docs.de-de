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
ms.date: 12/09/2019
ms.author: donkim
ms.openlocfilehash: 4ae8f13b4887bbc41b17defa3f9a20c07ed0cb45
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "76155586"
---
# <a name="quickstart-create-a-custom-command-preview"></a>Schnellstart: Erstellen eines benutzerdefinierten Befehls (Vorschau)

In diesem Artikel erfahren Sie, wie Sie eine gehostete Anwendung für benutzerdefinierte Befehle erstellen und testen.
Die Anwendung erkennt eine Äußerung wie „Schalte Fernseher ein“ und antwortet mit einer einfachen Meldung „OK, schalte Fernseher ein“.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Speech-Abonnement.

Wenn Sie nicht über ein Speech-Abonnement verfügen, können Sie eines erstellen, indem Sie zu [Speech Studio](https://speech.microsoft.com/) navigieren und **Speech-Ressource erstellen** auswählen.

  > [!div class="mx-imgBorder"]
  > [ ![Erstellen eines Projekts](media/custom-speech-commands/create-new-subscription.png) ](media/custom-speech-commands/create-new-subscription.png#lightbox)

  > [!NOTE]
  > Während der Vorschau wird nur die Region „westus2“ unterstützt.

## <a name="go-to-the-speech-studio-for-custom-commands"></a>Wechseln Sie zum Speech Studio für benutzerdefinierte Befehle.

1. Öffnen Sie Ihren Webbrowser, und navigieren Sie zum [Speech Studio](https://speech.microsoft.com/).
1. Geben Sie Ihre Anmeldeinformationen ein, um sich beim Portal anzumelden.

   - Die Standardansicht ist Ihre Liste der Speech-Abonnements.
     > [!NOTE]
     > Wenn die Seite „Abonnement auswählen“ nicht angezeigt wird, können Sie dorthin navigieren, indem Sie „Speech-Ressourcen“ aus dem Einstellungsmenü in der oberen Leiste auswählen.

1. Wählen Sie Ihr Speech-Abonnement und dann **Zu Studio wechseln** aus.
1. Wählen Sie **Benutzerdefinierte Befehle (Vorschau)** aus.

Die Standardansicht ist eine Liste der von Ihnen erstellten Anwendungen für benutzerdefinierte Befehle.

## <a name="create-a-custom-commands-project"></a>Erstellen eines Projekts für benutzerdefinierte Befehle

1. Wählen Sie **Neues Projekt** aus, um ein neues Projekt zu erstellen.

   > [!div class="mx-imgBorder"]
   > ![Erstellen eines Projekts](media/custom-speech-commands/create-new-project.png)

1. Geben Sie den Projektnamen und die Sprache ein.
1. Wählen Sie eine Erstellungsressource aus. Sind keine gültigen Erstellungsressourcen vorhanden, erstellen Sie eine, indem Sie **Neue Ressource erstellen** auswählen.

   > [!div class="mx-imgBorder"]
   > ![Erstellen einer Ressource](media/custom-speech-commands/create-new-resource.png)

   1. Geben Sie den Ressourcennamen, die Gruppe, den Speicherort und den Tarif ein.

         > [!NOTE]
         > Sie können Ressourcengruppen erstellen, indem Sie den gewünschten Ressourcengruppennamen in das Feld „Ressourcengruppe“ eingeben. Die Ressourcengruppe wird erstellt, wenn **Erstellen** ausgewählt wird.

1. Klicken Sie auf **Erstellen**, um Ihr Projekt zu erstellen.
1. Wählen Sie nach der Erstellung Ihr Projekt aus.

Ihre Ansicht sollte jetzt eine Übersicht über Ihre Anwendung für benutzerdefinierte Befehle darstellen.

## <a name="update-luis-resources-optional"></a>Aktualisieren von LUIS-Ressourcen (optional)

Sie können im Fenster „Neues Projekt“ die festgelegte Erstellungsressource aktualisieren und eine Vorhersageressource festlegen, mit der Eingaben während der Laufzeit erkannt werden.

> [!NOTE]
> Sie müssen eine Vorhersageressource festlegen, bevor Ihre Anwendung mehr als die von der Erstellungsressource bereitgestellten 1.000 Anforderungen ausführt.

> [!div class="mx-imgBorder"]
> ![Festlegen von LUIS-Ressourcen](media/custom-speech-commands/set-luis-resources.png)

1. Navigieren Sie zum Bereich „LUIS-Ressourcen“, indem Sie im linken Bereich **Einstellungen** und dann im mittleren Bereich **LUIS-Ressourcen** auswählen.
1. Wählen Sie eine Vorhersageressource aus, oder erstellen Sie eine, indem Sie **Neue Ressource erstellen** auswählen.
1. Wählen Sie **Speichern** aus.

## <a name="create-a-new-command"></a>Erstellen eines neuen Befehls

Jetzt können Sie einen Befehl erstellen. Lassen Sie uns ein Beispiel verwenden, das eine einzelne Äußerung, `turn on the tv`, übernimmt und mit der Nachricht `Ok, turning on the TV` antwortet.

1. Erstellen Sie einen neuen Befehl, indem Sie das Symbol `+` neben den Befehlen auswählen und ihm den Namen `TurnOn` geben.
1. Wählen Sie **Speichern** aus.

> [!div class="mx-imgBorder"]
> ![Erstellen eines Befehls](media/custom-speech-commands/create-add-command.png)

Ein Befehl ist ein Satz von:

| Group            | BESCHREIBUNG                                                                                                                 |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| Beispielsätze | Beispieläußerungen, die der Benutzer äußern kann, um diesen Befehl auszulösen.                                                                 |
| Parameter       | Informationen, die zur Vervollständigung des Befehls erforderlich sind.                                                                                |
| Vervollständigungsregeln | Die Aktionen, die zur Erfüllung des Befehls durchzuführen sind. Beispielsweise, um dem Benutzer zu antworten oder mit einem anderen Webdienst zu kommunizieren. |
| Erweiterte Regeln   | Zusätzliche Regeln für den Umgang mit spezifischeren oder komplexeren Situationen.                                                              |

### <a name="add-a-sample-sentence"></a>Hinzufügen eines Beispielsatzes

Lassen Sie uns mit Beispielsätzen beginnen und ein Beispiel dafür bereitstellen, was der Benutzer sagen kann:

```
turn on the tv
```

Im Moment verfügen wir über keine Parameter, sodass wir zu den Vervollständigungsregeln übergehen können.

### <a name="add-a-completion-rule"></a>Hinzufügen einer Vervollständigungsregel

Fügen Sie jetzt eine Vervollständigungsregel hinzu, um dem Benutzer zu antworten, die anzeigt, dass eine Aktion ausgeführt wird.

1. Erstellen Sie eine neue Vervollständigungsregel, indem Sie neben „Vervollständigungsregel“ das Symbol `+` auswählen.
1. Eingeben des Regelnamens
1. Hinzufügen einer Aktion
   1. Erstellen Sie eine neue Sprachantwortaktion, indem Sie neben „Aktionen“ das Symbol `+` und dann `SpeechResponse` auswählen.
   1. Eingeben der Antwort

   > [!NOTE]
   > Normaler Text muss mit einem Bindestrich beginnen. Ausführlichere Informationen finden Sie [hier](https://aka.ms/sc-lg-format).

   > [!div class="mx-imgBorder"]
   > ![Erstellen einer Sprachantwort](media/custom-speech-commands/create-speech-response-action.png)

1. Klicken Sie zum Speichern der Regel auf **Speichern**.

> [!div class="mx-imgBorder"]
> ![Erstellen einer Vervollständigungsregel](media/custom-speech-commands/create-basic-completion-response-rule.png)

| Einstellung    | Vorgeschlagener Wert                          | BESCHREIBUNG                                        |
| ---------- | ---------------------------------------- | -------------------------------------------------- |
| Regelname  | „ConfirmationResponse“                   | Ein Name, der den Zweck der Regel beschreibt.          |
| Bedingungen | Keine                                     | Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann.    |
| Aktionen    | SpeechResponse „OK, schalte Fernseher ein“ | Die durchzuführende Aktion, wenn die Bedingung der Regel erfüllt ist (true). |

## <a name="try-it-out"></a>Ausprobieren

Testen Sie das Verhalten über den Testchatbereich.

> [!div class="mx-imgBorder"]
> ![Testen mit Webchat](media/custom-speech-commands/create-basic-test-chat.png)

- Sie geben Folgendes ein: „Schalte Fernseher ein“
- Erwartete Antwort: „OK, schalte Fernseher ein“

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Schnellstart: Erstellen eines benutzerdefinierten Befehls mit Parametern (Vorschau)](./quickstart-custom-speech-commands-create-parameters.md)
