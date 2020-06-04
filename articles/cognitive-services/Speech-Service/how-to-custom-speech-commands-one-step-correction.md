---
title: 'Gewusst wie: Hinzufügen einer Korrektur mit einem Schritt zu einem benutzerdefinierten Befehl (Vorschau) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel wird erläutert, wie Sie Korrekturen in einem Schritt zu einem benutzerdefinierten Befehl implementieren.
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: encorona
ms.openlocfilehash: f43c28d314cb8a0211496664cd20d2c380e4d5b0
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858280"
---
# <a name="how-to-add-a-one-step-correction-to-a-custom-command-preview"></a>Anleitung: Hinzufügen einer Korrektur mit einem Schritt zu einem benutzerdefinierten Befehl (Vorschau)

In diesem Artikel erfahren Sie, wie Sie einem Befehl eine Bestätigung in einem Schritt hinzufügen.

Eine Korrektur in einem Schritt wird verwendet, um einen Befehl zu aktualisieren, der gerade abgeschlossen wurde. Das heißt, wenn Sie gerade einen Alarm eingerichtet haben, können Sie Ihre Meinung ändern und die Alarmzeit aktualisieren. Dies wird im folgenden Beispiel verdeutlicht:

- Eingabe: Set alarm for tomorrow at noon (Alarm für morgen Mittag festlegen)
- Ausgabe: „Ok, alarm set for 2020-05-02 12:00:00“ (OK, Alarm für 02.05.2020 12 Uhr festgelegt)
- Eingabe: Nein, morgen um 13 Uhr
- Ausgabe: OK,

Ein reales Szenario, das normalerweise damit einhergeht, dass der Client als Ergebnis eines abgeschlossenen Befehls eine Aktion ausführt – dieser Artikel geht davon aus, dass Sie als Entwickler die Möglichkeit haben, den Alarm in Ihrer Back-End-Anwendung zu aktualisieren.

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen die Schritte in den folgenden Artikeln durchgeführt haben:
> [!div class="checklist"]

> * [Schnellstart: Erstellen eines benutzerdefinierten Befehls (Vorschau)](./quickstart-custom-speech-commands-create-new.md)
> * [Schnellstart: Erstellen eines benutzerdefinierten Befehls mit Parametern (Vorschau)](./quickstart-custom-speech-commands-create-parameters.md)
> * [Vorgehensweise: Hinzufügen einer Bestätigung zu einem Benutzerdefinierten Befehl (Vorschau)](./how-to-custom-speech-commands-confirmations.md)


## <a name="add-interaction-rules-for-one-step-correction"></a>Hinzufügen von Interaktionsregeln für eine Korrektur in einem Schritt 

Um die Korrektur in einem Schritt zu veranschaulichen, erweitern wir den Befehl **SetAlarm**, den wir in [Gewusst wie: Hinzufügen einer Bestätigung zu einem Benutzerdefinierten Befehl (Vorschau)](./how-to-custom-speech-commands-confirmations.md) erstellt haben.
1. Fügen Sie eine Interaktionsregel hinzu, um den zuvor festgelegten Alarm zu aktualisieren. 

    Diese Regel fordert den Benutzer dazu auf, das Datum und die Uhrzeit des Alarms zu bestätigen, und erwartet im nächsten Durchgang eine Bestätigung (ja/nein).

   | Einstellung               | Vorgeschlagener Wert                                                  | BESCHREIBUNG                                        |
   | --------------------- | ---------------------------------------------------------------- | -------------------------------------------------- |
   | Regelname             | Vorherigen Alarm aktualisieren                                            | Ein Name, der den Zweck der Regel beschreibt.          |
   | Bedingungen            | Der vorherige Befehl muss aktualisiert werden und Erforderlicher Parameter > DateTime                | Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann.    |   
   | Aktionen               | Festlegen der Sprachantwort > Einfacher Editor > Aktualisieren des vorherigen Alarms auf {DateTime}      | Die durchzuführende Aktion, wenn die Bedingungen der Regel erfüllt sind (TRUE). |
   | Status nach der Ausführung | Der Befehl ist abgeschlossen.        | Status des Benutzers nach dem Turn                   |

1. Verschieben Sie die soeben erstellte Regel an die oberste Stelle der Interaktionsregeln (wählen Sie die Regel im Bereich aus, und klicken Sie oben im mittleren Bereich auf den Aufwärtspfeil unter dem Symbol `...`).
   > [!div class="mx-imgBorder"]
   > ![Hinzufügen einer Bereichsvalidierung](media/custom-speech-commands/one-step-correction-rules.png)
    > [!NOTE]
    > In einer realen Anwendung senden Sie im Abschnitt „Actions“ dieser Regel auch eine Aktivität an den Client zurück oder rufen einen HTTP-Endpunkt auf, um den Alarm in Ihrem System zu aktualisieren.

## <a name="try-it-out"></a>Ausprobieren

Wählen Sie `Train` aus, warten Sie auf den Abschluss des Trainings, und wählen Sie `Test` aus.

- Eingabe: Set alarm for tomorrow at noon (Alarm für morgen Mittag festlegen)
- Ausgabe: "Are you sure you want to set an alarm for 2020-05-02 12:00:00?" (Möchten Sie einen Alarm für 02.05.2020 12 Uhr festlegen?)
- Eingabe: Ja
- Ausgabe: „Ok, alarm set for 2020-05-02 12:00:00“ (OK, Alarm für 02.05.2020 12 Uhr festgelegt)
- Eingabe: Nein, morgen um 14 Uhr
- Ausgabe: „Updating previous alarm to 2020-05-02 14:00:00“ (Vorheriger Alarm wird auf 02/05/2020 14:00:00 aktualisiert)
