---
title: 'Gewusst wie: Hinzufügen einer Bestätigung zu einem benutzerdefinierten Befehl (Vorschau)'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel wird beschrieben, wie Bestätigungen für einen Befehl in benutzerdefinierten Befehlen implementiert werden.
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: encorona
ms.openlocfilehash: afa197c83b4f66f12863de4185ef7763447f3ed9
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75453365"
---
# <a name="how-to-add-a-confirmation-to-a-custom-command-preview"></a>Anleitung: Hinzufügen einer Bestätigung zu einem benutzerdefinierten Befehl (Vorschau)

In diesem Artikel erfahren Sie, wie Sie einem Befehl eine Bestätigung hinzufügen.

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen die Schritte in den folgenden Artikeln durchgeführt haben:

- [Schnellstart: Erstellen eines benutzerdefinierten Befehls (Vorschau)](./quickstart-custom-speech-commands-create-new.md)
- [Schnellstart: Erstellen eines benutzerdefinierten Befehls mit Parametern (Vorschau)](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-setalarm-command"></a>Erstellen eines SetAlarm-Befehls

Um Validierungen zu veranschaulichen, erstellen wir einen neuen Befehl, mit dem der Benutzer einen Alarm festlegen kann.

1. Öffnen Sie Ihre zuvor erstellte Anwendung für benutzerdefinierte Befehle in [Speech Studio](https://speech.microsoft.com/).
1. Erstellen eines neuen Befehls vom Typ **SetAlarm**
1. Hinzufügen eines Parameters mit dem Namen „DateTime“

   | Einstellung           | Vorgeschlagener Wert                                          | BESCHREIBUNG                                                                                      |
   | ----------------- | ---------------------------------------------------------| ------------------------------------------------------------------------------------------------ |
   | Name              | Datetime                                                 | Ein aussagekräftiger Name für Ihren Befehlsparameter.                                                    |
   | Erforderlich          | true                                                     | Kontrollkästchen, das angibt, ob ein Wert für diesen Parameter vor dem Abschließen des Befehls erforderlich ist. |
   | Antwortvorlage | "- What time?" (Welche Uhrzeit)                                           | Eine Aufforderung, nach dem Wert dieses Parameters zu fragen, wenn er nicht bekannt ist.                              |
   | type              | Datetime                                                 | Der Typ des Parameters, z. B. Zahl, Zeichenfolge oder Datum/Uhrzeit.                                      |
   | Datumsstandardwerte     | If date is missing use today (Bei fehlendem Datum heutiges Datum verwenden)                             |                                                                                                  |
   | Uhrzeitstandardwerte     | If time is missing use start of day (Bei fehlender Uhrzeit Tagesbeginn verwenden)                      |                                                                                                  | 

1. Hinzufügen einiger Beispielsätze
   
   ```
    set an alarm for {DateTime}
    set alarm {DateTime}
    alarm for {DateTime}
   ```

1. Hinzufügen einer Vervollständigungsregel zur Bestätigung des Ergebnisses

   | Einstellung    | Vorgeschlagener Wert                                         | BESCHREIBUNG                                        |
   | ---------- | ------------------------------------------------------- | -------------------------------------------------- |
   | Regelname  | Set alarm (Alarm festlegen)                                               | Ein Name, der den Zweck der Regel beschreibt.          |
   | Aktionen    | SpeechResponse - "- Ok, alarm set for {DateTime}" (SpeechResponse: "- OK, Alarm festgelegt für {DateTime}")       | Die durchzuführende Aktion, wenn die Bedingung der Regel erfüllt ist (true). |

## <a name="try-it-out"></a>Ausprobieren

Wählen Sie den Testbereich aus, und testen Sie einige Interaktionen.

- Eingabe: Set alarm for tomorrow at noon (Alarm für morgen Mittag festlegen)
- Ausgabe: "Ok, alarm set for 12/06/2019 12:00:00" (OK, Alarm für 06.12.2019 12 Uhr festgelegt)

- Eingabe: Set an alarm (Alarm festlegen)
- Ausgabe: "What time?" (Welche Uhrzeit)
- Eingabe: 5pm (17 Uhr)
- Ausgabe: "Ok, alarm set for 12/05/2019 17:00:00" (OK, Alarm für 05.12.2019 17 Uhr festgelegt)

## <a name="add-the-advanced-rules-for-confirmation"></a>Hinzufügen der erweiterten Regeln für die Bestätigung

1. Fügen Sie eine erweiterte Regeln für die Bestätigung hinzu. 

    Diese Regel fordert den Benutzer dazu auf, das Datum und die Uhrzeit des Alarms zu bestätigen, und erwartet beim nächsten Turn eine Bestätigung (ja/nein).

   | Einstellung               | Vorgeschlagener Wert                                                                  | BESCHREIBUNG                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Regelname             | Confirm date time (Datum und Uhrzeit bestätigen)                                                                | Ein Name, der den Zweck der Regel beschreibt.          |
   | Bedingungen            | Required Parameter - DateTime (Erforderlicher Parameter: DateTime)                                                    | Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann.    |   
   | Aktionen               | SpeechResponse - "- Are you sure you want to set an alarm for {DateTime}?" (SpeechResponse: "- Möchten Sie einen Alarm für {DateTime} festlegen?")       | Die durchzuführende Aktion, wenn die Bedingung der Regel erfüllt ist (true). |
   | Status nach der Ausführung | Warten auf Eingabe                                                                   | Status für den Benutzer nach dem Turn                  |
   | Erwartungen          | Bestätigung                                                                     | Erwartung für den nächsten Turn                      |

1. Fügen Sie eine erweiterte Regel hinzu, um eine erfolgreiche Bestätigung (Benutzer hat mit „ja“ reagiert) zu verarbeiten

   | Einstellung               | Vorgeschlagener Wert                                                                  | BESCHREIBUNG                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Regelname             | Accepted confirmation (Akzeptierte Bestätigung)                                                            | Ein Name, der den Zweck der Regel beschreibt.          |
   | Bedingungen            | SuccessfulConfirmation & Required Parameter - DateTime (SuccessfulConfirmation und erforderlicher Parameter: DateTime)                           | Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann.    |   
   | Status nach der Ausführung | Ready for Completion (Zum Abschluss bereit)                                                             | Status des Benutzers nach dem Turn                   |

1. Fügen Sie eine erweiterte Regel hinzu, um eine abgelehnte Bestätigung (Benutzer hat mit „nein“ reagiert) zu verarbeiten

   | Einstellung               | Vorgeschlagener Wert                                                                  | BESCHREIBUNG                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Regelname             | Denied confirm (Abgelehnte Bestätigung)                                                                   | Ein Name, der den Zweck der Regel beschreibt.          |
   | Bedingungen            | DeniedConfirmation & Required Parameter - DateTime (DeniedConfirmation und erforderlicher Parameter: DateTime)                               | Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann.    |   
   | Aktionen               | ClearParameter - DateTime & SpeechResponse - "- No problem, what time then?" (ClearParameter: DateTime und SpeechResponse: "- Kein Problem, welche Uhrzeit?")     | Die durchzuführende Aktion, wenn die Bedingung der Regel erfüllt ist (true). |
   | Status nach der Ausführung | Warten auf Eingabe                                                                   | Status des Benutzers nach dem Turn                   |
   | Erwartungen          | ElicitParameters - DateTime                                                      | Erwartung für den nächsten Turn                      |

## <a name="try-it-out"></a>Ausprobieren

Wählen Sie den Testbereich aus, und testen Sie einige Interaktionen.

- Eingabe: Set alarm for tomorrow at noon (Alarm für morgen Mittag festlegen)
- Ausgabe: "Are you sure you want to set an alarm for 12/07/2019 12:00:00?" (Möchten Sie einen Alarm für 07.12.2019 12 Uhr festlegen?)
- Eingabe: Nein
- Ausgabe: "No problem, what time then?" (Kein Problem, welche Uhrzeit?)
- Eingabe: 5pm (17 Uhr)
- Ausgabe: "Are you sure you want to set an alarm for 12/06/2019 17:00:00?" (Möchten Sie einen Alarm für 06.12.2019 17 Uhr festlegen?)
- Eingabe: Ja
- Ausgabe: "Ok, alarm set for 12/06/2019 17:00:00" (OK, Alarm für 06.12.2019 17 Uhr festgelegt)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Vorgehensweise: Hinzufügen einer Korrektur mit einem Schritt zu einem benutzerdefinierten Befehl (Vorschau)](./how-to-custom-speech-commands-one-step-correction.md)