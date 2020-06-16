---
title: Hinzufügen einer Korrektur in einem Schritt zu benutzerdefinierten Befehlen (Vorschau) – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie eine Korrektur in einem Schritt für einen Befehl in einer App für benutzerdefinierte Befehle (Vorschau) hinzufügen.
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: encorona
ms.openlocfilehash: 05f63ba4e70f649df33905f1e92fb1fab866a86c
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2020
ms.locfileid: "84310426"
---
# <a name="add-a-one-step-correction-to-a-custom-command-in-a-custom-commands-preview-application"></a>Hinzufügen einer Korrektur in einem Schritt zu einem benutzerdefinierten Befehl in einer Anwendung für benutzerdefinierte Befehle (Vorschau)

In diesem Artikel erfahren Sie, wie Sie eine Bestätigung in einem Schritt einem Befehl in einer App für benutzerdefinierte Befehle (Vorschau) hinzufügen.

Eine Korrektur in einem Schritt wird verwendet, um einen Befehl zu aktualisieren, der gerade abgeschlossen wurde. Wenn Sie einem Alarmbefehl eine Korrektur mit einem Schritt hinzufügen, können Sie Ihre Meinung ändern und die Zeit des Alarms aktualisieren. Beispiel:

- Eingabe: Set alarm for tomorrow at noon (Alarm für morgen Mittag festlegen)
- Ausgabe: „Ok, alarm set for 2020-05-02 12:00:00“ (OK, Alarm für 02.05.2020 12 Uhr festgelegt)
- Eingabe: Nein, morgen um 13 Uhr
- Ausgabe: OK,

> [!NOTE]
> In einem realen Szenario führt der Client eine Aktion als Ergebnis der Vervollständigung des Befehls aus. In diesem Artikel wird davon ausgegangen, dass Sie über einen Mechanismus zum Aktualisieren des Alarms in der Back-End-Anwendung verfügen.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte in den folgenden Artikeln aus:
> [!div class="checklist"]

> * [Schnellstart: Erstellen einer App für benutzerdefinierte Befehle (Vorschau)](./quickstart-custom-speech-commands-create-new.md)
> * [Schnellstart: Erstellen einer App für benutzerdefinierte Befehle (Vorschau) mit Parametern](./quickstart-custom-speech-commands-create-parameters.md)
> * [Vorgehensweise: Hinzufügen von Bestätigungen zu einem Befehl in einer App für benutzerdefinierte Befehle (Vorschau)](./how-to-custom-speech-commands-confirmations.md)

## <a name="add-interaction-rules-for-one-step-correction"></a>Hinzufügen von Interaktionsregeln für eine Korrektur in einem Schritt

Um die Korrektur in einem Schritt zu veranschaulichen, erweitern Sie den Befehl **SetAlarm**, den Sie in [Anleitung: Hinzufügen einer Bestätigung zu einem benutzerdefinierten Befehl (Vorschau)](./how-to-custom-speech-commands-confirmations.md) erstellt haben.

1. Fügen Sie dem **SetAlarm**-Befehl eine Interaktionsregel hinzu.

    Diese Regel fordert den Benutzer dazu auf, das Datum und die Uhrzeit des Alarms zu bestätigen, und erwartet im nächsten Durchgang eine Bestätigung (ja/nein).

   | Einstellung               | Vorgeschlagener Wert                                                  | BESCHREIBUNG                                        |
   | --------------------- | ---------------------------------------------------------------- | -------------------------------------------------- |
   | **Regelname**             | **Vorherigen Alarm aktualisieren**                                            | Ein Name, der den Zweck der Regel beschreibt.          |
   | **Bedingungen**            | **Der vorherige Befehl muss aktualisiert werden und „Required Parameter > DateTime“ (Erforderlicher Parameter > DateTime)**                | Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann.    |   
   | **Aktionen**               | **Sprachantwort senden > Einfacher Editor > Aktualisieren des vorherigen Alarms auf {DateTime}**      | Die durchzuführende Aktion, wenn die Bedingungen der Regel erfüllt sind (TRUE). |
   | **Status nach der Ausführung** | **Der Befehl ist abgeschlossen**        | Status des Benutzers nach dem Turn                   |

1. Wählen Sie im Bereich die Interaktionsregel aus, die Sie soeben erstellt haben. Wählen Sie in der linken oberen Ecke des Bereichs die Schaltfläche mit den Auslassungspunkten ( **...** ) aus. Verschieben Sie die Regel dann mit dem **Pfeil nach oben** an den Anfang der **Interaktionsregeln**-Liste.
   > [!div class="mx-imgBorder"]
   > ![Hinzufügen einer Bereichsvalidierung](media/custom-speech-commands/one-step-correction-rules.png)

    > [!NOTE]
    > In einer realen Anwendung senden Sie im Abschnitt **Aktionen** eine Aktivität an den Client zurück oder rufen einen HTTP-Endpunkt auf, um den Alarm in Ihrem System zu aktualisieren.

## <a name="try-it-out"></a>Ausprobieren

1. Wählen Sie **Trainieren** aus.

1. Nachdem das Training abgeschlossen ist, wählen Sie **Testen** aus, und probieren Sie diese Interaktionen aus:

   - Eingabe: Set alarm for tomorrow at noon (Alarm für morgen Mittag festlegen)
   - Ausgabe: "Are you sure you want to set an alarm for 2020-05-02 12:00:00?" (Möchten Sie einen Alarm für 02.05.2020 12 Uhr festlegen?)
   - Eingabe: Ja
   - Ausgabe: „Ok, alarm set for 2020-05-02 12:00:00“ (OK, Alarm für 02.05.2020 12 Uhr festgelegt)
   - Eingabe: Nein, morgen um 14 Uhr
   - Ausgabe: „Updating previous alarm to 2020-05-02 14:00:00“ (Vorheriger Alarm wird auf 02/05/2020 14:00:00 aktualisiert)
