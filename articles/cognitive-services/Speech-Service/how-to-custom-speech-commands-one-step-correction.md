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
ms.openlocfilehash: 86a12bd1dccc2b6ac15010546d7e990b768ebc02
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75453349"
---
# <a name="how-to-add-a-one-step-correction-to-a-custom-command-preview"></a>Anleitung: Hinzufügen einer Korrektur mit einem Schritt zu einem benutzerdefinierten Befehl (Vorschau)

In diesem Artikel erfahren Sie, wie Sie einem Befehl eine Bestätigung in einem Schritt hinzufügen.

Eine Korrektur in einem Schritt wird verwendet, um einen Befehl zu aktualisieren, der gerade abgeschlossen wurde.

Etwa, wenn Sie gerade einen Alarm eingerichtet haben, können Sie Ihre Meinung ändern und die Alarmzeit aktualisieren.

- Eingabe: Set alarm for tomorrow at noon (Alarm für morgen Mittag festlegen)
- Ausgabe: „Ok, alarm set for 12/06/2019 12:00:00“ (OK, Alarm für 06.12.2019 12 Uhr festgelegt)
- Eingabe: Nein, morgen um 13 Uhr
- Ausgabe: „OK“

Beachten Sie, dass dies impliziert, dass Sie als Entwickler über einen Mechanismus zum Aktualisieren des Alarms in der Back-End-Anwendung verfügen.

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen die Schritte in den folgenden Artikeln durchgeführt haben:

- [Schnellstart: Erstellen eines benutzerdefinierten Befehls (Vorschau)](./quickstart-custom-speech-commands-create-new.md)
- [Schnellstart: Erstellen eines benutzerdefinierten Befehls mit Parametern (Vorschau)](./quickstart-custom-speech-commands-create-parameters.md)
- [Vorgehensweise: Hinzufügen einer Bestätigung zu einem benutzerdefinierten Befehl (Vorschau)](./how-to-custom-speech-commands-confirmations.md)

## <a name="add-the-advanced-rules-for-one-step-correction"></a>Hinzufügen der erweiterten Regeln für eine Korrektur in einem Schritt 

Um die Korrektur in einem Schritt zu veranschaulichen, erweitern wir den Befehl **SetAlarm** in [Gewusst wie: Bestätigungen](./how-to-custom-speech-commands-confirmations.md).
 
1. Fügen Sie eine erweiterte Regel zum Aktualisieren des vorherigen Alarms hinzu. 

    Diese Regel fordert den Benutzer dazu auf, das Datum und die Uhrzeit des Alarms zu bestätigen, und erwartet im nächsten Durchgang eine Bestätigung (ja/nein).

   | Einstellung               | Vorgeschlagener Wert                                                  | BESCHREIBUNG                                        |
   | --------------------- | ---------------------------------------------------------------- | -------------------------------------------------- |
   | Regelname             | Vorherigen Alarm aktualisieren                                            | Ein Name, der den Zweck der Regel beschreibt.          |
   | Bedingungen            | UpdateLastCommand und erforderlicher Parameter – DateTime                | Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann.    |   
   | Aktionen               | SpeechResponse – „- Der vorherige Alarm wird auf {DateTime} aktualisiert“       | Die durchzuführende Aktion, wenn die Bedingung der Regel erfüllt ist (true). |
   | Zustand nach der Ausführung | Befehl abschließen                                                 | Zustand des Benutzers nach dem Durchgang                   |

1. Verschieben Sie die Regel, die Sie soeben erstellt haben, an die erste Stelle der erweiterten Regeln (scrollen Sie im Panel durch die Regel, und klicken Sie auf den Pfeil nach oben)
   > [!div class="mx-imgBorder"]
   > ![Hinzufügen einer Bereichsvalidierung](media/custom-speech-commands/one-step-correction-rules.png)

> [!NOTE]
> In einer realen Anwendung senden Sie im Abschnitt „Actions“ dieser Regel auch eine Aktivität an den Client zurück oder rufen einen HTTP-Endpunkt auf, um den Alarm in Ihrem System zu aktualisieren.

## <a name="try-it-out"></a>Ausprobieren

Wählen Sie den Testbereich aus, und testen Sie einige Interaktionen.

- Eingabe: Set alarm for tomorrow at noon (Alarm für morgen Mittag festlegen)
- Ausgabe: "Are you sure you want to set an alarm for 12/07/2019 12:00:00?" (Möchten Sie wirklich einen Alarm für den 07.12.2019 12 Uhr festlegen?)
- Eingabe: Ja
- Ausgabe: „Ok, alarm set for 12/07/2019 12:00:00“ (OK, Alarm für 06.12.2019 12 Uhr festgelegt)
- Eingabe: Nein, morgen um 13 Uhr
- Ausgabe: „Updating previous alarm to 12/07/2019 13:00:00“ (Vorheriger Alarm wird auf 12/07/2019 13:00:00 aktualisiert)
