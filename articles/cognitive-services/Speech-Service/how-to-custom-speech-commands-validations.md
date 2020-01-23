---
title: 'Anleitung: Hinzufügen von Validierungen zu Parametern benutzerdefinierter Befehle (Vorschau)'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel wird erläutert, wie Sie einem Parameter in Benutzerdefinierte Befehle Validierungen hinzufügen.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: donkim
ms.openlocfilehash: cf6e4e4f0bfab43fb738f8415022e55fcbcbd05a
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76156453"
---
# <a name="how-to-add-validations-to-custom-command-parameters-preview"></a>Anleitung: Hinzufügen von Validierungen zu Parametern benutzerdefinierter Befehle (Vorschau)

In diesem Artikel erfahren Sie, wie Sie Validierungen zu Parametern hinzufügen und zur Korrektur auffordern können.

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen die Schritte in den folgenden Artikeln durchgeführt haben:

- [Schnellstart: Erstellen eines benutzerdefinierten Befehls (Vorschau)](./quickstart-custom-speech-commands-create-new.md)
- [Schnellstart: Erstellen eines benutzerdefinierten Befehls mit Parametern (Vorschau)](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-settemperature-command"></a>Erstellen eines SetTemperature-Befehls

Um Validierungen zu veranschaulichen, erstellen wir einen neuen Befehl, mit dem der Benutzer die Temperatur einstellen kann.

1. Öffnen Sie Ihre zuvor erstellte Anwendung für benutzerdefinierte Befehle in [Speech Studio](https://speech.microsoft.com/).
1. Erstellen eines neuen **SetTemperature**-Befehls
1. Hinzufügen eines Parameters für die Zieltemperatur
1. Hinzufügen einer Validierung für den Temperaturparameter
   > [!div class="mx-imgBorder"]
   > ![Hinzufügen einer Bereichsvalidierung](media/custom-speech-commands/validations-add-temperature.png)

   | Einstellung           | Vorgeschlagener Wert                                          | Beschreibung                                                                                      |
   | ----------------- | -------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
   | Name              | Temperatur                                              | Ein aussagekräftiger Name für Ihren Befehlsparameter.                                                    |
   | Erforderlich          | true                                                     | Kontrollkästchen, das angibt, ob ein Wert für diesen Parameter vor dem Abschließen des Befehls erforderlich ist. |
   | Antwortvorlage | „- Welche Temperatur hätten Sie gerne?“                     | Eine Aufforderung, nach dem Wert dieses Parameters zu fragen, wenn er nicht bekannt ist.                              |
   | type              | Number                                                   | Der Typ des Parameters, z. B. Zahl, Zeichenfolge oder Datum/Uhrzeit.                                      |
   | Überprüfen        | Mindestwert: 60, Maximalwert: 80                             | Bei Zahlenparametern der zulässige Wertebereich für den Parameter                             |
   | Antwortvorlage | „- Es kann nur ein Wert zwischen 16 und 27 Grad eingestellt werden.“      | Aufforderung zur Eingabe eines aktualisierten Werts, wenn bei der Validierung ein Fehler auftritt.                                       |

1. Hinzufügen einiger Beispielsätze

   ```
   set the temperature to {Temperature} degrees
   change the temperature to {Temperature}
   set the temperature
   change the temperature
   ```

1. Hinzufügen einer Vervollständigungsregel zur Bestätigung des Ergebnisses

   | Einstellung    | Vorgeschlagener Wert                                           | Beschreibung                                        |
   | ---------- | --------------------------------------------------------- | -------------------------------------------------- |
   | Regelname  | Bestätigungsmeldung                                      | Ein Name, der den Zweck der Regel beschreibt.          |
   | Bedingungen | Erforderlicher Parameter – Temperature                          | Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann.    |
   | Aktionen    | SpeechResponse – „- OK, stelle {Temperature} Grad ein“ | Die durchzuführende Aktion, wenn die Bedingung der Regel erfüllt ist (true). |

> [!TIP]
> In diesem Beispiel wird eine Sprachantwort verwendet, um das Ergebnis zu bestätigen. Beispiele für das Vervollständigen des Befehls mit einer Clientaktion finden Sie unter: [Vorgehensweise: Ausführen der Befehle auf dem Client mit dem Speech SDK (Vorschau)](./how-to-custom-speech-commands-fulfill-sdk.md)

## <a name="try-it-out"></a>Ausprobieren

Wählen Sie den Testbereich aus, und testen Sie einige Interaktionen.

- Eingabe: Stelle die Temperatur auf 22 Grad ein
- Ausgabe: „OK, Einstellung auf 22 Grad“

- Eingabe: Stelle die Temperatur auf 7 Grad ein
- Ausgabe: „Es kann nur ein Wert zwischen 16 und 27 Grad eingestellt werden.“
- Eingabe: Stelle stattdessen 22 Grad ein
- Ausgabe: „OK, Einstellung auf 22 Grad“

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Vorgehensweise: Hinzufügen einer Bestätigung zu einem Benutzerdefinierten Befehl (Vorschau)](./how-to-custom-speech-commands-confirmations.md)
