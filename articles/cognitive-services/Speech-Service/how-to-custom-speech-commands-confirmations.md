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
ms.openlocfilehash: bf1b79c1b5d7b9dfd93b354c6b6ff5a512bb74a5
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858235"
---
# <a name="how-to-add-a-confirmation-to-a-custom-command-preview"></a>Anleitung: Hinzufügen einer Bestätigung zu einem benutzerdefinierten Befehl (Vorschau)

In diesem Artikel erfahren Sie, wie Sie einem Befehl eine Bestätigung hinzufügen.

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen die Schritte in den folgenden Artikeln durchgeführt haben:
> [!div class="checklist"]
> *  [Schnellstart: Erstellen eines benutzerdefinierten Befehls (Vorschau)](./quickstart-custom-speech-commands-create-new.md)
> * [Schnellstart: Erstellen eines benutzerdefinierten Befehls mit Parametern (Vorschau)](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-setalarm-command"></a>Erstellen eines SetAlarm-Befehls

Um Bestätigungen zu veranschaulichen, erstellen wir einen neuen Befehl, mit dem der Benutzer einen Alarm festlegen kann.

1. Öffnen Sie Ihre zuvor erstellte Anwendung für benutzerdefinierte Befehle in [Speech Studio](https://speech.microsoft.com/).
1. Erstellen Sie einen neuen Befehl `SetAlarm`.
1. Fügen Sie einen Parameter mit der Bezeichnung `DateTime` und der folgenden Konfiguration hinzu.

   | Einstellung                           | Vorgeschlagener Wert                     |  BESCHREIBUNG                 |
   | --------------------------------- | -----------------------------------------------------| ------------|
   | Name                              | Datetime                                | Ein aussagekräftiger Name für den Parameter                                |
   | Erforderlich                          | Aktiviert                                 | Kontrollkästchen, das angibt, ob ein Wert für diesen Parameter vor dem Abschließen des Befehls erforderlich ist. |
   | Antwort auf erforderlichen Parameter   | Einfacher Editor > Welche Uhrzeit?                              | Eine Aufforderung, nach dem Wert dieses Parameters zu fragen, wenn er nicht bekannt ist. |
   | type                              | Datetime                                | Der Typ des Parameters, z. B. Zahl, Zeichenfolge, Datum/Uhrzeit oder Geografie.   |
   | Datumsstandardwerte                     | If date is missing use today (Bei fehlendem Datum heutiges Datum verwenden)            | Der Standardwert der zu verwendenden Variablen, falls nicht vom Benutzer angegeben.  |  
   | Uhrzeitstandardwerte                     | If time is missing use start of day (Bei fehlender Uhrzeit Tagesbeginn verwenden)     |  Der Standardwert der zu verwendenden Variablen, falls nicht vom Benutzer angegeben.|

1. Fügen Sie einige Beispielsätze hinzu.
   
    ```
    set an alarm for {DateTime}
    set alarm {DateTime}
    alarm for {DateTime}
   ```

1. Fügen Sie eine Vervollständigungsregel hinzu, um das Ergebnis zu bestätigen.

   | Einstellung    | Vorgeschlagener Wert                               |BESCHREIBUNG                                     |
   | ---------- | ------------------------------------------------------- |-----|
   | Regelname  | Set alarm (Alarm festlegen)                                               |    Ein Name, der den Zweck der Regel beschreibt. |
   | Aktionen    | Sprachantwort senden: „OK, Alarm festgelegt auf {DateTime}“    |Die durchzuführende Aktion, wenn die Bedingung der Regel erfüllt ist (true).

## <a name="try-it-out"></a>Ausprobieren

1. Wählen Sie das `Train`-Symbol aus, das oben im rechten Bereich angezeigt wird.

1. Nachdem das Training abgeschlossen ist, wählen Sie `Test` aus.
    - Eingabe: Set alarm for tomorrow at noon (Alarm für morgen Mittag festlegen)
    - Ausgabe: „Ok, alarm set for 2020-05-02 12:00:00“ (OK, Alarm für 02.05.2020 12 Uhr festgelegt)
    - Eingabe: Set an alarm (Alarm festlegen)
    - Ausgabe: „What time?“ (Welche Uhrzeit)
    - Eingabe: 5pm (17 Uhr)
    - Ausgabe: „Ok, alarm set for 2020-05-01 17:00:00“ (OK, Alarm für 01.05.2020 17 Uhr festgelegt)

## <a name="add-the-advanced-rules-for-confirmation"></a>Hinzufügen der erweiterten Regeln für die Bestätigung

Bestätigungen werden durch das Hinzufügen von Interaktionsregeln erreicht.

1. Fügen Sie dem vorhandenen Befehl `SetAlarm` eine **Interaktionsregel** hinzu, indem Sie das `+Add`-Symbol im mittleren Bereich und dann **Interaktionsregeln** -> **Befehl bestätigen** auswählen.

    Diese Regel fordert den Benutzer dazu auf, das Datum und die Uhrzeit des Alarms zu bestätigen, und erwartet im nächsten Durchgang eine Bestätigung (ja/nein).

   | Einstellung               | Vorgeschlagener Wert                                                                  | BESCHREIBUNG                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Regelname             | Confirm date time (Datum und Uhrzeit bestätigen)                                                                | Ein Name, der den Zweck der Regel beschreibt.          |
   | Bedingungen            | Required Parameter > DateTime (Erforderlicher Parameter > DateTime)                                                    | Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann.    |   
   | Aktionen               | Sprachantwort senden > „Are you sure you want to set an alarm for {DateTime}?“ (Möchten Sie einen Alarm für {DateTime} festlegen?)     | Die durchzuführende Aktion, wenn die Bedingung der Regel erfüllt ist (true). |
   | Erwartungen          | Erwartete Bestätigung vom Benutzer                                                 | Erwartung für den nächsten Turn                      |
   | Status nach der Ausführung  | Auf Benutzereingabe warten                                                            | Status für den Benutzer nach dem Turn                  |
  
      > [!div class="mx-imgBorder"]
      > ![Antwort für erforderlichen Parameter erstellen](media/custom-speech-commands/add-validation-set-temperature.png)

1. Fügen Sie eine weitere Interaktionsregel hinzu, um eine erfolgreiche Bestätigung (Benutzer hat mit „Ja“ reagiert) zu verarbeiten

   | Einstellung               | Vorgeschlagener Wert                                                                  | BESCHREIBUNG                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Regelname             | Accepted confirmation (Akzeptierte Bestätigung)                                                            | Ein Name, der den Zweck der Regel beschreibt.          |
   | Bedingungen            | Die Bestätigung war erfolgreich und erforderlicher Parameter > DateTime                      | Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann.    |   
   | Status nach der Ausführung | Fügen Sie Vervollständigungsregeln aus                                                          | Status des Benutzers nach dem Turn                   |

1. Fügen Sie eine erweiterte Regel hinzu, um eine abgelehnte Bestätigung (Benutzer hat mit „nein“ reagiert) zu verarbeiten

   | Einstellung               | Vorgeschlagener Wert                                                                  | BESCHREIBUNG                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Regelname             | Bestätigung abgelehnt                                                                   | Ein Name, der den Zweck der Regel beschreibt.          |
   | Bedingungen            | Die Bestätigung wurde abgelehnt und erforderlicher Parameter > DateTime                               | Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann.    |   
   | Aktionen               | Parameterwert löschen > DateTime und Sprachantwort senden > „Kein Problem, welche Zeit stattdessen?“  | Die durchzuführende Aktion, wenn die Bedingung der Regel erfüllt ist (true). |
   | Status nach der Ausführung | Warten auf Eingabe                                                                   | Status des Benutzers nach dem Turn                   |
   | Erwartungen          | Es werden Parametereingaben vom Benutzer erwartet > DateTime                           | Erwartung für den nächsten Turn                      |

## <a name="try-out-the-changes"></a>Testen der Änderungen

Wählen Sie `Train` aus, warten Sie auf den Abschluss des Trainings, und wählen Sie `Test` aus.

- Eingabe: Set alarm for tomorrow at noon (Alarm für morgen Mittag festlegen)
- Ausgabe: „Are you sure you want to set an alarm for 2020-05-02 12:00:00?“ (Möchten Sie einen Alarm für 02.05.2020 12 Uhr festlegen?)
- Eingabe: Nein
- Ausgabe: „No problem, what time then?“ (Kein Problem, welche Zeit stattdessen?)
- Eingabe: 5pm (17 Uhr)
- Ausgabe: „Are you sure you want to set an alarm for 2020-05-01 17:00:00?“ (Möchten Sie einen Alarm für den 01.05.2020 17 Uhr festlegen?)
- Eingabe: Ja
- Ausgabe: „Ok, alarm set for 2020-05-01 17:00:00“ (OK, Alarm für 01.05.2020 17 Uhr festgelegt)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Vorgehensweise: Hinzufügen einer Korrektur mit einem Schritt zu einem benutzerdefinierten Befehl (Vorschau)](./how-to-custom-speech-commands-one-step-correction.md)