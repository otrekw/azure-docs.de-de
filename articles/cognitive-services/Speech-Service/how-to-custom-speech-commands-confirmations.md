---
title: Hinzufügen von Bestätigungen in einer App für benutzerdefinierte Befehle (Vorschau) – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie einer App für benutzerdefinierte Befehle (Vorschau) Bestätigungen hinzufügen.
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: encorona
ms.openlocfilehash: 1cb0624012b22b6cae2c98bfa6ddc9495e09615d
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2020
ms.locfileid: "84310467"
---
# <a name="add-confirmations-to-a-command-in-a-custom-commands-preview-application"></a>Hinzufügen von Bestätigungen zu einem Befehl in einer Anwendung für benutzerdefinierte Befehle (Vorschau)

In diesem Artikel erfahren Sie, wie Sie Bestätigungen für Befehle in einer App für benutzerdefinierte Befehle (Vorschau) erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte in den folgenden Artikeln aus:
> [!div class="checklist"]
> * [Schnellstart: Erstellen einer App für benutzerdefinierte Befehle (Vorschau)](./quickstart-custom-speech-commands-create-new.md)
> * [Schnellstart: Erstellen einer App für benutzerdefinierte Befehle (Vorschau) mit Parametern](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-setalarm-command"></a>Erstellen eines SetAlarm-Befehls

Um Bestätigungen zu veranschaulichen, erstellen Sie einen neuen Befehl, mit dem ein Alarm festgelegt wird.

1. Öffnen Sie in [Speech Studio](https://speech.microsoft.com/) die von Ihnen erstellte App für benutzerdefinierte Befehle (Vorschau).
1. Erstellen Sie einen neuen **SetAlarm**-Befehl.
1. Fügen Sie einen **DateTime**-Parameter mit der folgenden Konfiguration hinzu:

   | Einstellung                           | Vorgeschlagener Wert                     |  Beschreibung                 |
   | --------------------------------- | -----------------------------------------------------| ------------|
   | **Name**                              | **DateTime**                                | Aussagekräftiger Name für den Parameter                                |
   | **Erforderlich**                          | Aktiviert                                 | Kontrollkästchen, das angibt, ob ein Wert für diesen Parameter vor dem Abschließen des Befehls erforderlich ist. |
   | **Antwort auf einen erforderlichen Parameter**   | **Einfacher Editor > Welche Uhrzeit?**                              | Eine Aufforderung, nach dem Wert dieses Parameters zu fragen, wenn er nicht bekannt ist. |
   | **Typ**                              | **DateTime**                                | Typ des Parameters, z. B. Number, String, DateTime oder Geography   |
   | **Datumsstandardwerte**                     | Bei fehlendem Datum heutiges Datum verwenden            | Der Standardwert der zu verwendenden Variablen, falls nicht vom Benutzer angegeben.  |  
   | **Uhrzeitstandardwerte**                     | Bei fehlender Uhrzeit Tagesbeginn verwenden     |  Der Standardwert der zu verwendenden Variablen, falls nicht vom Benutzer angegeben.|

1. Fügen Sie einige Beispielsätze hinzu.
   
    ```
    set an alarm for {DateTime}
    set alarm {DateTime}
    alarm for {DateTime}
   ```

1. Fügen Sie eine Vervollständigungsregel hinzu, um das Ergebnis zu bestätigen. Verwenden Sie die folgende Konfiguration:

   | Einstellung    | Vorgeschlagener Wert                               |BESCHREIBUNG                                     |
   | ---------- | ------------------------------------------------------- |-----|
   | **Regelname**  | **Set alarm** (Alarm festlegen)                                               |    Ein Name, der den Zweck der Regel beschreibt. |
   | **Aktionen**    | **Sprachantwort senden: „OK, Alarm festgelegt auf {DateTime}“**    |Die durchzuführende Aktion, wenn die Bedingung der Regel erfüllt ist (true).

## <a name="try-it-out"></a>Ausprobieren

1. Wählen Sie oben im rechten Bereich **Trainieren** aus.

1. Nachdem das Training abgeschlossen ist, wählen Sie **Testen** aus, und probieren Sie dann die folgenden Interaktionen aus:
    - Eingabe: Set alarm for tomorrow at noon (Alarm für morgen Mittag festlegen)
    - Ausgabe: „Ok, alarm set for 2020-05-02 12:00:00“ (OK, Alarm für 02.05.2020 12 Uhr festgelegt)
    - Eingabe: Set an alarm (Alarm festlegen)
    - Ausgabe: „What time?“ (Welche Uhrzeit)
    - Eingabe: 5pm (17 Uhr)
    - Ausgabe: „Ok, alarm set for 2020-05-01 17:00:00“ (OK, Alarm für 02.05.2020 12 Uhr festgelegt)

## <a name="add-interaction-rules-for-the-confirmation"></a>Hinzufügen von Interaktionsregeln für die Bestätigung

Erstellen Sie Bestätigungen durch Hinzufügen von Interaktionsregeln.

1. Wählen Sie im **SetAlarm**-Befehl im mittleren Bereich **Hinzufügen** aus, und wählen Sie dann **Interaktionsregeln** > **Befehl bestätigen** aus.

    Mit dieser Regel wird der Benutzer aufgefordert, das Datum und die Uhrzeit des Alarms zu bestätigen. Verwenden Sie die folgenden Einstellungen:

   | Einstellung               | Vorgeschlagener Wert                                                                  | BESCHREIBUNG                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | **Regelname**             | **Confirm date time (Datum und Uhrzeit bestätigen)**                                                                | Ein Name, der den Zweck der Regel beschreibt.          |
   | **Bedingungen**            | **Required Parameter > DateTime (Erforderlicher Parameter > DateTime)**                                                    | Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann.    |   
   | **Aktionen**               | **Sprachantwort senden > „Are you sure you want to set an alarm for {DateTime}?“ (Möchten Sie einen Alarm für {DateTime} festlegen?)**     | Die durchzuführende Aktion, wenn die Bedingung der Regel erfüllt ist (true). |
   | **Erwartungen**          | **Erwartete Bestätigung vom Benutzer**                                                 | Erwartung für den nächsten Turn                      |
   | **Status nach der Ausführung**  | **Auf Benutzereingabe warten**                                                            | Status für den Benutzer nach dem Turn                  |
  
      > [!div class="mx-imgBorder"]
      > ![Antwort für erforderlichen Parameter erstellen](media/custom-speech-commands/add-validation-set-temperature.png)

1. Fügen Sie eine Interaktionsregel für eine akzeptierte Bestätigung hinzu (Benutzer hat „yes“ (ja) angegeben). Verwenden Sie die folgende Konfiguration:

   | Einstellung               | Vorgeschlagener Wert                                                                  | BESCHREIBUNG                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | **Regelname**             | **Accepted confirmation (Akzeptierte Bestätigung)**                                                            | Ein Name, der den Zweck der Regel beschreibt.          |
   | **Bedingungen**            | **Die Bestätigung war erfolgreich und erforderlicher Parameter -> DateTime**                      | Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann.    |   
   | **Status nach der Ausführung** | **Execute completion rules (Vervollständigungsregeln ausführen)**                                                          | Status des Benutzers nach dem Turn                   |

1. Fügen Sie eine Interaktionsregel für eine abgelehnte Bestätigung hinzu (Benutzer hat „no“ (nein) angegeben). Verwenden Sie die folgende Konfiguration:

   | Einstellung               | Vorgeschlagener Wert                                                                  | BESCHREIBUNG                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | **Regelname**             | **Bestätigung abgelehnt**                                                                   | Ein Name, der den Zweck der Regel beschreibt.          |
   | **Bedingungen**            | **Die Bestätigung wurde abgelehnt und erforderlicher Parameter -> DateTime**                               | Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann.    |   
   | **Aktionen**               | **Parameterwert löschen -> DateTime und Sprachantwort senden -> „Kein Problem, welche Zeit stattdessen?“**  | Die durchzuführende Aktion, wenn die Bedingung der Regel erfüllt ist (true). |
   | **Status nach der Ausführung** | **Warten auf Eingabe**                                                                   | Status des Benutzers nach dem Turn                   |
   | **Erwartungen**          | **Es werden Parametereingaben vom Benutzer erwartet -> DateTime**                           | Erwartung für den nächsten Turn                      |

## <a name="try-out-the-changes"></a>Testen der Änderungen

1. Wählen Sie **Trainieren** aus.

1. Nachdem das Training abgeschlossen ist, wählen Sie **Testen** aus, und probieren Sie diese Interaktionen aus:

    - Eingabe: Set alarm for tomorrow at noon (Alarm für morgen Mittag festlegen)
    - Ausgabe: „Are you sure you want to set an alarm for 2020-05-02 12:00:00?“ (Möchten Sie einen Alarm für 02.05.2020 12 Uhr festlegen?)
    - Eingabe: Nein
    - Ausgabe: „No problem, what time then?“ (Kein Problem, welche Zeit stattdessen?)
    - Eingabe: 5pm (17 Uhr)
    - Ausgabe: „Are you sure you want to set an alarm for 2020-05-01 17:00:00?“ (Möchten Sie einen Alarm für den 01.05.2020 17 Uhr festlegen?)
    - Eingabe: Ja
    - Ausgabe: „Ok, alarm set for 2020-05-01 17:00:00“ (OK, Alarm für 02.05.2020 12 Uhr festgelegt)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen einer Korrektur in einem Schritt zu einem Befehl in einer App für benutzerdefinierte Befehle (Vorschau)](./how-to-custom-speech-commands-one-step-correction.md)
