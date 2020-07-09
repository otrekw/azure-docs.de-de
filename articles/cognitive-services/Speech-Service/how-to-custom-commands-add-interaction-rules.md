---
title: 'Gewusst wie: Hinzufügen einer Bestätigung zu einem benutzerdefinierten Befehl'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erfahren Sie, wie Bestätigungen für einen Befehl in benutzerdefinierten Befehlen implementiert werden.
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: sausin
ms.openlocfilehash: 5f66e29e4c1bc85981202251e0de8288f4baee4e
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2020
ms.locfileid: "85307243"
---
# <a name="add-interaction-rules"></a>Hinzufügen von Interaktionsregeln

In diesem Artikel erfahren Sie mehr über **Interaktionsregeln**. Dies sind zusätzliche Regeln für den Umgang mit spezifischeren oder komplexeren Situationen. Es steht Ihnen zwar frei, Ihre eigenen benutzerdefinierten Interaktionsregeln zu erstellen, in diesem Artikel verwenden Sie jedoch Interaktionsregeln für die folgenden Zielszenarien:

* Bestätigen von Befehlen
* Hinzufügen einer einstufigen Korrektur zu Befehlen

Wechseln Sie zum Abschnitt [Referenzen](./custom-commands-references.md), um mehr über Interaktionsregeln zu erfahren.

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen die Schritte in den folgenden Artikeln durchgeführt haben:
> [!div class="checklist"]
> * [Vorgehensweise: Erstellen einer Anwendung mit einfachen Befehlen](./how-to-custom-commands-create-application-with-simple-commands.md)
> * [Vorgehensweise: Hinzufügen von Parametern zu Befehlen](./how-to-custom-commands-add-parameters-to-commands.md)

## <a name="add-confirmations-to-a-command"></a>Hinzufügen von Bestätigungen zu einem Befehl

Verwenden Sie den Befehl **SetTemperature**, um eine Bestätigung hinzuzufügen. Wenn Sie eine Bestätigung erreichen möchten, erstellen Sie Interaktionsregeln mit den folgenden Schritten.

1. Wählen Sie den Befehl **SetTemperature** im linken Bereich aus.
2. Fügen Sie Interaktionsregeln hinzu, indem Sie **Hinzufügen** im mittleren Bereich und dann **Interaktionsregeln** > **Befehl bestätigen** auswählen.

    Dadurch werden drei Interaktionsregeln hinzugefügt. Diese Regel fordert den Benutzer dazu auf, das Datum und die Uhrzeit des Alarms zu bestätigen, und erwartet im nächsten Durchgang eine Bestätigung (Ja/Nein).

    1. Ändern Sie die Interaktionsregel **Befehl bestätigen** gemäß der folgenden Konfiguration.
        1. Benennen Sie **Name** in **`Confirm Temperature`** um.
        1. Fügen Sie eine neue Bedingung als **Erforderliche Parameter > Temperatur** hinzu.
        1. Fügen Sie eine neue Aktion als **Typ > Sprachantwort senden > `Are you sure you want to set the temperature as {Temperature} degrees?`** hinzu.
        1. Belassen Sie den Standardwert von **Bestätigung vom Benutzer erwarten** im Abschnitt „Erwartungen“.
      
         > [!div class="mx-imgBorder"]
         > ![Antwort für erforderlichen Parameter erstellen](media/custom-speech-commands/add-validation-set-temperature.png)
    

    1. Ändern Sie die Interaktionsregel **Bestätigung erfolgreich**, um eine erfolgreiche Bestätigung zu behandeln (Benutzer hat mit „Ja“ geantwortet).
      
          1. Ändern Sie **Name** in **`Confirmation temperature succeeded`** .
          1. Belassen Sie die bereits bestehende Bedingung **Bestätigung war erfolgreich**.
          1. Fügen Sie eine neue Bedingung als **Typ > Erforderliche Parameter > Temperatur** hinzu.
          1. Belassen Sie den Standardwert von **Status nach der Ausführung** als **Vervollständigungsregeln ausführen**.

    1. Ändern Sie **Bestätigung verweigert** (Benutzer hat mit „Nein“ geantwortet), um Szenarien zu behandeln, wenn die Bestätigung verweigert wird.

          1. Ändern Sie **Name** in **`Confirmation temperature denied`** .
          1. Belassen Sie die bereits bestehende Bedingung **Bestätigung wurde verweigert**.
          1. Fügen Sie eine neue Bedingung als **Typ > Erforderliche Parameter > Temperatur** hinzu.
          1. Fügen Sie eine neue Aktion als **Typ > Sprachantwort senden > `No problem. What temperature then?`** hinzu.
          1. Belassen Sie den Standardwert von **Status nach der Ausführung** als **Auf Benutzereingabe warten**.

> [!IMPORTANT]
> In diesem Artikel haben Sie die integrierte Bestätigungsfunktion verwendet. Alternativ können Sie dasselbe auch erreichen, indem Sie die Interaktionsregeln nacheinander manuell hinzufügen.
   

### <a name="try-out-the-changes"></a>Testen der Änderungen

Wählen Sie **Trainieren** aus, warten Sie auf den Abschluss des Trainings, und wählen Sie dann **Testen** aus.

- Eingabe: Temperatur auf 27 Grad einstellen
- Ausgabe: OK 27?
- Eingabe: Nein
- Ausgabe: Kein Problem. Welche Temperatur dann?
- Eingabe: 28 Grad
- Ausgabe: OK 28?
- Eingabe: Ja
- Ausgabe: OK, stelle die Temperatur auf 28 Grad ein


## <a name="implementing-corrections-in-a-command"></a>Implementieren von Korrekturen in einem Befehl

In diesem Abschnitt konfigurieren Sie die einstufige Korrektur, die verwendet wird, wenn der Vervollständigungsvorgang bereits ausgeführt wurde. Es wird auch ein Beispiel dafür bereitgestellt, wie die Korrektur standardmäßig aktiviert wird, falls der Befehl noch nicht erfüllt ist. Um eine Korrektur hinzuzufügen, wenn der Befehl nicht abgeschlossen ist, fügen Sie einen neuen **AlarmTone**-Parameter hinzu.

Wählen Sie den Befehl **SetAlarm** im linken Bereich aus, und fügen Sie einen neuen **AlarmTone**-Parameter hinzu.
        
- **Name** > `AlarmTone`
- **Typ** > Zeichenfolge
- **Standardwert** > `Chimes`
- **Konfiguration** > Vordefinierte Eingabewerte aus einem internen Katalog akzeptieren
- **Vordefinierte Eingabewerte** > `Chimes`, `Jingle` und `Echo`. Jeder als einzelne vordefinierte Eingaben.


Aktualisieren Sie als nächstes die Antwort für den DateTime-Parameter zu `Ready to set alarm with tone as {AlarmTone}. For what time?`. Ändern Sie dann die Vervollständigungsregel wie folgt.

1. Wählen Sie die vorhandene Vervollständigungsregel aus, **ConfirmationResponse**.
1. Zeigen Sie im rechten Bereich auf die vorhandene Aktion, und wählen Sie die Schaltfläche **Bearbeiten** aus.
1. Aktualisieren Sie die Sprachantwort zu `Ok, alarm set for {DateTime}. The alarm tone is {AlarmTone}.`

### <a name="try-out-the-changes"></a>Testen der Änderungen

Wählen Sie **Trainieren** aus, warten Sie auf den Abschluss des Trainings, und wählen Sie dann **Testen** aus.
Probieren Sie die folgenden Äußerungen aus:

- Eingabe: Alarm festlegen
- Ausgabe: Bereit zum Festlegen des Alarms mit dem Ton „Glockentöne“. Welche Uhrzeit?
- Eingabe: Alarm mit Ton „Jingle“ für morgen 9 Uhr festlegen
- Ausgabe: OK, Alarm für 30.05.2020 9 Uhr festgelegt Der Alarmton ist „Jingle“.

> [!IMPORTANT]
> Beachten Sie, wie der Alarmton ohne explizite Konfiguration in einem laufenden Befehl geändert werden konnte, d. h. als der Befehl noch nicht abgeschlossen war. **Die Korrektur ist standardmäßig für alle Befehlsparameter aktiviert, unabhängig von der Rundennummer, wenn der Befehl noch nicht erfüllt ist.**

### <a name="correction-when-command-is-completed"></a>Korrektur nach Abschluss des Befehls

Die Plattform für benutzerdefinierte Befehle bietet auch die Möglichkeit der einstufigen Korrektur, selbst wenn der Befehl abgeschlossen ist. Dieses Feature ist jedoch nicht standardmäßig aktiviert und muss explizit konfiguriert werden. Verwenden Sie die folgenden Schritte, um eine einstufige Korrektur zu konfigurieren.

1. Fügen Sie im Befehl **SetAlarm** eine Interaktionsregel vom Typ **Vorherigen Befehl aktualisieren** hinzu, um den zuvor festgelegten Alarm zu aktualisieren. Benennen Sie den Standardnamen **Name** der Interaktionsregel in **Vorherigen Alarm aktualisieren** um.
1. Belassen Sie die Standardbedingung **Vorheriger Befehl muss aktualisiert werden** unverändert.
1.  Fügen Sie eine neue Bedingung als **Typ > Erforderliche Parameter > DateTime** hinzu.
1. Fügen Sie eine neue Aktion als **Typ > Sprachantwort senden > Einfacher Editor > `Updating previous alarm time to {DateTime}.`** hinzu.
1. Behalten Sie den Standardwert für den Status nach der Ausführung als **Befehl abgeschlossen** bei.

### <a name="try-out-the-changes"></a>Testen der Änderungen

Wählen Sie **Trainieren** aus, warten Sie auf den Abschluss des Trainings, und wählen Sie dann **Testen** aus.

- Eingabe: Alarm festlegen
- Ausgabe: Bereit zum Festlegen des Alarms mit dem Ton „Glockentöne“. „What time?“ (Welche Uhrzeit)
- Eingabe: Alarm mit Ton „Jingle“ für morgen 9 Uhr festlegen
- Ausgabe: OK, Alarm für 21.05.2020 9 Uhr festgelegt Der Alarmton ist „Jingle“.
- Eingabe: Nein, 8 Uhr
- Ausgabe: Vorheriger Alarm wird auf 21.05.2020 8 Uhr aktualisiert

> [!NOTE]
> In einer realen Anwendung müssen Sie im Abschnitt „Actions“ dieser Korrekturregel auch eine Aktivität an den Client zurücksenden oder einen HTTP-Endpunkt aufrufen, um die Alarmzeit in Ihrem System zu aktualisieren. Diese Aktion sollte nur für die Aktualisierung der Alarmzeit und nicht für ein anderes Attribut des Befehls, in diesem Fall den Alarmton, zuständig sein.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Vorgehensweise: Hinzufügen von Sprachgenerierungsvorlagen für Sprachantworten](./how-to-custom-commands-add-language-generation-templates.md)