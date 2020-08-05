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
ms.openlocfilehash: 7d6c0928196c9e8e1abf6aa7f724a58753ce3d2a
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87289042"
---
# <a name="add-interaction-rules"></a>Hinzufügen von Interaktionsregeln

In diesem Artikel erfahren Sie mehr über **Interaktionsregeln**. Dies sind zusätzliche Regeln für den Umgang mit spezifischeren oder komplexeren Situationen. Es steht Ihnen zwar frei, Ihre eigenen benutzerdefinierten Interaktionsregeln zu erstellen, in diesem Artikel verwenden Sie jedoch Interaktionsregeln für die folgenden Zielszenarien:

* Bestätigen von Befehlen
* Hinzufügen einer einstufigen Korrektur zu Befehlen

Weitere Informationen zu Interaktionsregeln finden Sie im Abschnitt [Referenzen](./custom-commands-references.md).

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen die Schritte in den folgenden Artikeln durchgeführt haben:
> [!div class="checklist"]
> * [Vorgehensweise: Erstellen einer Anwendung mit einfachen Befehlen](./how-to-custom-commands-create-application-with-simple-commands.md)
> * [Vorgehensweise: Hinzufügen von Parametern zu Befehlen](./how-to-custom-commands-add-parameters-to-commands.md)

## <a name="add-confirmations-to-a-command"></a>Hinzufügen von Bestätigungen zu einem Befehl

Verwenden Sie den Befehl **SetTemperature**, um eine Bestätigung hinzuzufügen. Um eine Bestätigung zu erhalten, erstellen Sie mit den folgenden Schritten Interaktionsregeln:

1. Wählen Sie im linken Bereich den Befehl **SetTemperature** aus.
1. Fügen Sie Interaktionsregeln hinzu, indem Sie im mittleren Bereich **Hinzufügen** auswählen. Wählen Sie dann **Interaktionsregeln** > **Befehl bestätigen** aus.

    Durch diese Aktion werden drei Interaktionsregeln hinzugefügt. Diese Regeln fordern den Benutzer dazu auf, das Datum und die Uhrzeit des Alarms zu bestätigen, und im nächsten Durchgang wird eine Bestätigung (Ja/Nein) erwartet.

    1. Ändern Sie die Interaktionsregel **Befehl bestätigen** gemäß der folgenden Konfiguration:
        1. Benennen Sie **Name** in **Temperatur bestätigen** um.
        1. Fügen Sie eine neue Bedingung mit **Erforderliche Parameter** > **Temperatur** hinzu.
        1. Fügen Sie eine neue Aktion als **Typ** > **Sprachantwort senden** > **Möchten Sie die Temperatur wirklich auf {Temperature} Grad festlegen?**
        1. Behalten Sie den Standardwert von **Bestätigung vom Benutzer erwarten** im Abschnitt **Erwartungen** bei.
      
         > [!div class="mx-imgBorder"]
         > ![Antwort für erforderlichen Parameter erstellen](media/custom-speech-commands/add-validation-set-temperature.png)
    

    1. Ändern Sie die Interaktionsregel **Bestätigung erfolgreich**, um eine erfolgreiche Bestätigung zu behandeln (Benutzer hat mit „Ja“ geantwortet).
      
          1. Ändern Sie **Name** in **Temperaturbestätigung erfolgreich**.
          1. Belassen Sie die bereits bestehende Bedingung **Bestätigung war erfolgreich**.
          1. Fügen Sie eine neue Bedingung als **Typ** > **Erforderliche Parameter** > **Temperatur** hinzu.
          1. Belassen Sie den Standardwert von **Status nach der Ausführung** als **Vervollständigungsregeln ausführen**.

    1. Ändern Sie die Interaktionsregel **Bestätigung verweigert** so, dass sie Szenarien verarbeitet, in denen die Bestätigung verweigert wird (der Benutzer hat mit „Nein“ geantwortet).

          1. Ändern Sie **Name** in **Temperaturbestätigung verweigert**.
          1. Belassen Sie die bereits bestehende Bedingung **Bestätigung wurde verweigert**.
          1. Fügen Sie eine neue Bedingung als **Typ** > **Erforderliche Parameter** > **Temperatur** hinzu.
          1. Fügen Sie eine neue Aktion als **Typ** > **Sprachantwort senden** > **Kein Problem. Welche Temperatur dann?** hinzu.
          1. Belassen Sie den Standardwert von **Status nach der Ausführung** als **Auf Benutzereingabe warten**.

> [!IMPORTANT]
> In diesem Artikel haben Sie die integrierte Bestätigungsfunktion verwendet. Sie können Interaktionsregeln auch einzeln manuell hinzufügen.
   

### <a name="try-out-the-changes"></a>Testen der Änderungen

Wählen Sie **Trainieren** aus, warten Sie auf den Abschluss des Trainings, und wählen Sie dann **Testen** aus.

- **Eingabe**: Stelle die Temperatur auf 80 Grad ein.
- **Ausgabe**: Sind Sie sicher, dass Sie die Temperatur auf 80 Grad einstellen möchten?
- **Eingabe**: Nein
- **Ausgabe**: Kein Problem. Welche Temperatur dann?
- **Eingabe**: 72 Grad
- **Ausgabe**: Sind Sie sicher, dass Sie die Temperatur auf 72 Grad einstellen möchten?
- **Eingabe**: Ja
- **Ausgabe**: OK, die Temperatur wird auf 83 Grad eingestellt.


## <a name="implement-corrections-in-a-command"></a>Implementieren von Korrekturen in einem Befehl

In diesem Abschnitt konfigurieren Sie eine einstufige Korrektur, die verwendet wird, nachdem der Erfüllungsvorgang bereits ausgeführt wurde. Es wird auch ein Beispiel dafür bereitgestellt, wie die Korrektur standardmäßig aktiviert wird, falls der Befehl noch nicht erfüllt ist. Um eine Korrektur hinzuzufügen, wenn der Befehl nicht abgeschlossen ist, fügen Sie den neuen **AlarmTone**-Parameter hinzu.

Wählen Sie im linken Bereich den Befehl **SetAlarm** aus, und fügen Sie den neuen **AlarmTone**-Parameter hinzu.
        
- **Name** > **AlarmTone**
- **Typ** > **Zeichenfolge**
- **Standardwert** > **Chimes**
- **Konfiguration** > **Vordefinierte Eingabewerte aus dem internen Katalog akzeptieren**
- **Vordefinierte Eingabewerte** > **Chimes**, **Jingle** und **Echo** als einzelne vordefinierte Eingaben


Aktualisieren Sie als Nächstes die Antwort für den **DateTime**-Parameter in **Bereit zum Festlegen des Alarms mit dem Ton {AlarmTone}. Welche Uhrzeit?** Ändern Sie dann die Vervollständigungsregel wie folgt:

1. Wählen Sie die vorhandene Vervollständigungsregel aus, **ConfirmationResponse**.
1. Zeigen Sie im rechten Bereich auf die vorhandene Aktion, und wählen Sie **Bearbeiten** aus.
1. Ändern Sie die Sprachantwort in **OK, Alarm für {DateTime} festgelegt. Der Alarmton ist {AlarmTone}.**

### <a name="try-out-the-changes"></a>Testen der Änderungen

Wählen Sie **Trainieren** aus, warten Sie auf den Abschluss des Trainings, und wählen Sie dann **Testen** aus.
Probieren Sie die folgenden Äußerungen aus:

- **Eingabe**: Alarm festlegen
- **Ausgabe**: Bereit zum Festlegen des Alarms mit dem Ton „Glockentöne“. Welche Uhrzeit?
- **Eingabe**: Alarm mit Ton „Jingle“ für morgen 9 Uhr festlegen
- **Ausgabe**: OK, Alarm für 30.05.2020 9 Uhr festgelegt. Der Alarmton ist „Jingle“.

> [!IMPORTANT]
> Der Alarmton könnte ohne explizite Konfiguration in einem laufenden Befehl geändert werden, z. B. wenn der Befehl noch nicht abgeschlossen war. *Eine Korrektur ist standardmäßig für alle Befehlsparameter aktiviert, unabhängig von der Rundennummer, wenn der Befehl noch nicht erfüllt ist.*

### <a name="correction-when-command-is-completed"></a>Korrektur nach Abschluss des Befehls

Die Plattform für benutzerdefinierte Befehle bietet auch die Möglichkeit einer einstufigen Korrektur, selbst wenn der Befehl abgeschlossen ist. Dieses Feature ist nicht standardmäßig aktiviert. Er muss explizit konfiguriert werden. Verwenden Sie die folgenden Schritte, um eine einstufige Korrektur zu konfigurieren.

1. Fügen Sie im Befehl **SetAlarm** eine Interaktionsregel vom Typ **Vorherigen Befehl aktualisieren** hinzu, um den zuvor festgelegten Alarm zu aktualisieren. Benennen Sie den Standardnamen **Name** der Interaktionsregel in **Vorherigen Alarm aktualisieren** um.
1. Belassen Sie die Standardbedingung **Vorheriger Befehl muss aktualisiert werden** unverändert.
1. Fügen Sie eine neue Bedingung als **Typ** > **Erforderliche Parameter** > **DateTime** hinzu.
1. Fügen Sie eine neue Aktion als **Typ** > **Sprachantwort senden** > **Einfacher Editor** > **Vorheriger Alarm wird auf {DateTime} aktualisiert** hinzu.
1. Behalten Sie den Standardwert für den Status **Nach der Ausführung** als **Befehl abgeschlossen** bei.

### <a name="try-out-the-changes"></a>Testen der Änderungen

Wählen Sie **Trainieren** aus, warten Sie auf den Abschluss des Trainings, und wählen Sie dann **Testen** aus.

- **Eingabe**: Alarm festlegen
- **Ausgabe**: Bereit zum Festlegen des Alarms mit dem Ton „Glockentöne“. Welche Uhrzeit?
- **Eingabe**: Alarm mit Ton „Jingle“ für morgen 9 Uhr festlegen
- **Ausgabe**: OK, Alarm für 21.05.2020 9 Uhr festgelegt. Der Alarmton ist „Jingle“.
- **Eingabe**: Nein, 8 Uhr.
- **Ausgabe**: Vorheriger Alarm wird auf 21.05.2020 8 Uhr aktualisiert

> [!NOTE]
> In einer realen Anwendung müssen Sie im Abschnitt **Aktionen** dieser Korrekturregel auch eine Aktivität an den Client zurücksenden oder einen HTTP-Endpunkt aufrufen, um die Alarmzeit in Ihrem System zu aktualisieren. Diese Aktion sollte nur für die Aktualisierung der Alarmzeit und nicht für ein anderes Attribut des Befehls zuständig sein. In diesem Beispiel ist dies der Alarmton.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Vorgehensweise: Hinzufügen von Sprachgenerierungsvorlagen für Sprachantworten](./how-to-custom-commands-add-language-generation-templates.md)
