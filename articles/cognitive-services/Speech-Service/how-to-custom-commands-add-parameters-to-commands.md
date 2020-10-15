---
title: 'Anleitung: Hinzufügen einfacher Befehle zur Anwendung für benutzerdefinierte Befehle – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erfahren Sie, wie Sie Parameter zu benutzerdefinierten Befehlen hinzufügen.
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: sausin
ms.openlocfilehash: 0ed237debc2395ed307658b2d57a541574f9478a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87284148"
---
# <a name="add-parameters-to-commands"></a>Hinzufügen von Parametern zu Befehlen

In diesem Artikel erfahren Sie, wie Sie Parameter zu benutzerdefinierten Befehlen hinzufügen. Parameter sind Informationen, die von den Befehlen zur Durchführung einer Aufgabe erforderlich sind. In komplexen Szenarien können Parameter auch zur Definition von Bedingungen verwendet werden, die benutzerdefinierte Aktionen auslösen.

## <a name="prerequisites"></a>Voraussetzungen

> [!div class="checklist"]
> * [Vorgehensweise: Erstellen einer Anwendung mit einfachen Befehlen](./how-to-custom-commands-create-application-with-simple-commands.md)

## <a name="configure-parameters-for-turnon-command"></a>Konfigurieren von Parametern für den Befehl „TurnOn“

Bearbeiten Sie den vorhandenen **TurnOn**-Befehl, um mehrere Geräte ein- und auszuschalten.

1. Da der Befehl jetzt die Szenarien zum Ein- und Ausschalten behandelt, benennen Sie den Befehl in **TurnOnOff** um.
   1. Wählen Sie im linken Bereich den Befehl **TurnOn** aus, und wählen Sie dann die Schaltfläche mit den Auslassungspunkten (...) neben **Neuer Befehl** am oberen Rand des Bereichs aus.
   
   1. Wählen Sie **Umbenennen** aus. Ändern Sie in den Fenstern **Befehl umbenennen** den Wert **Name** in **TurnOnOff**.

1. Als nächstes fügen Sie diesem Befehl einen neuen Parameter hinzu, der angibt, ob der Benutzer das Gerät ein- oder ausschalten möchte.
   1. Wählen Sie oben im mittleren Bereich **Hinzufügen** aus. Wählen Sie in der Dropdownliste **Parameter** aus.
   1. Fügen Sie im rechten Bereich im Abschnitt **Parameter** den Wert **OnOff** in das Feld **Name** ein.
   1. Wählen Sie **Erforderlich** aus. Wählen Sie im Fenster **Add response for a required parameter** (Antwort für einen erforderlichen Parameter hinzufügen) **Simple editor** (Einfacher Editor) aus. Im Feld **Erste Variation** fügen Sie Folgendes hinzu:
        ```
        On or Off?
        ```
   1. Wählen Sie **Update** aus.

       > [!div class="mx-imgBorder"]
       > ![Antwort für erforderlichen Parameter erstellen](media/custom-commands/add-required-on-off-parameter-response.png)
   
   1. Jetzt konfigurieren wir die Parametereigenschaften. Eine Erläuterung aller Konfigurationseigenschaften eines Befehls finden Sie unter [Referenzen](./custom-commands-references.md). Konfigurieren Sie die Eigenschaften des Parameters wie folgt:
      

       | Konfiguration      | Vorgeschlagener Wert     | BESCHREIBUNG                                                      |
       | ------------------ | ----------------| ---------------------------------------------------------------------|
       | Name               | `OnOff`           | Ein aussagekräftiger Name für den Parameter                                                                           |
       | Is Global          | Deaktiviert       | Kontrollkästchen, das angibt, ob ein Wert für diesen Parameter global auf alle Befehle in der Anwendung angewendet wird.|
       | Erforderlich           | Aktiviert         | Kontrollkästchen, das angibt, ob ein Wert für diesen Parameter vor dem Abschließen des Befehls erforderlich ist. |
       | Antwort auf erforderlichen Parameter      |Einfacher Editor > `On or Off?`      | Eine Aufforderung, nach dem Wert dieses Parameters zu fragen, wenn er nicht bekannt ist. |
       | type               | String          | Der Typ des Parameters, z. B. Zahl, Zeichenfolge, Datum/Uhrzeit oder Geografie.   |
       | Konfiguration      | Vordefinierte Eingabewerte aus einem internen Katalog akzeptieren | Für Zeichenfolgen, schränkt die Eingaben auf eine Reihe möglicher Werte ein |
       | Vordefinierte Eingabewerte     | `on`, `off`           | Menge möglicher Werte und ihrer Aliase         |
       
        
   1. Wenn Sie vordefinierte Eingabewerte hinzufügen möchten, wählen Sie **Vordefinierte Eingabe hinzufügen** aus, und geben Sie in Fenster **Neues Element** den **Namen** ein, wie in der Tabelle oben angegeben. In diesem Fall verwenden wir keine Aliase, Sie können ihn also leer lassen.
   
      > [!div class="mx-imgBorder"]
      > ![Erstellen von Parametern](media/custom-commands/create-on-off-parameter.png)

   1. Wählen Sie **Speichern** aus, um alle Konfigurationen des Parameters zu speichern.
 
 ### <a name="add-subjectdevice-parameter"></a>Hinzufügen des Parameters „SubjectDevice“ 

   1. Wählen Sie anschließend erneut **Hinzufügen** aus, um einen zweiten Parameter hinzuzufügen, der den Namen der Geräte darstellt, die mit diesem Befehl gesteuert werden können. Verwenden Sie die folgende Konfiguration.
   

       | Einstellung            | Vorgeschlagener Wert       |
       | ------------------ | --------------------- |
       | Name               | `SubjectDevice`         |
       | Is Global          | Deaktiviert             |
       | Erforderlich           | Aktiviert               |
       | Antwort auf erforderlichen Parameter     | Einfacher Editor > `Which device do you want to control?`    | 
       | type               | String                |          |
       | Konfiguration      | Vordefinierte Eingabewerte aus einem internen Katalog akzeptieren | 
       | Vordefinierte Eingabewerte | `tv`, `fan`               |
       | Aliase (`tv`)      | `television`, `telly`     |

   1. Wählen Sie **Speichern** aus.

### <a name="modify-example-sentences"></a>Ändern von Beispielsätzen

Bei Befehlen mit Parametern ist es hilfreich, Beispielsätze hinzuzufügen, die alle möglichen Kombinationen abdecken. Beispiel:

* Vollständige Parameterinformationen – `turn {OnOff} the {SubjectDevice}`
* Teilweise Parameterinformationen – `turn it {OnOff}`
* Keine Parameterinformationen – `turn something`

Beispielsätze mit unterschiedlichem Informationsumfang ermöglichen es der Anwendung für benutzerdefinierte Befehle, sowohl einmalige als auch mehrfach ausgeführte Lösungen mit Teilinformationen aufzulösen.

Bearbeiten Sie vor diesem Hintergrund die Beispielsätze, um die Parameter wie unten vorgeschlagen zu verwenden:

```
turn {OnOff} the {SubjectDevice}
{SubjectDevice} {OnOff}
turn it {OnOff}
turn something {OnOff}
turn something
```

Wählen Sie **Speichern** aus.

> [!TIP]
> Verwenden Sie im Editor für Beispielsätze geschweifte Klammern, um auf Ihre Parameter zu verweisen. - `turn {OnOff} the {SubjectDevice}` Verwenden Sie die Tab-Taste für automatische Vervollständigung auf der Grundlage der zuvor erstellten Parameter.

### <a name="modify-completion-rules-to-include-parameters"></a>Ändern der Vervollständigungsregeln zum Einbeziehen von Parametern

Ändern Sie die vorhandene Vervollständigungsregel **ConfirmationResponse**.

1. Wählen Sie im Abschnitt **Bedingung** die Option **Bedingung hinzufügen** aus.
1. Wählen Sie im Fenster **Neue Bedingung** in der Liste **Typ** die Option **Erforderliche Parameter**aus. Überprüfen Sie in der nachstehenden Prüfliste sowohl **OnOff** als auch **SubjectDevice**.
1. Lassen Sie **IsGlobal** deaktiviert.
1. Klicken Sie auf **Erstellen**.
1. Bearbeiten Sie im Abschnitt **Aktionen** die vorhandene Aktion **Sprachantwort senden**, indem Sie mit der Maus auf die Aktion zeigen und das Bearbeiten-Symbol auswählen. Nutzen Sie diesmal die neu erstellten Parameter **OnOff** und **SubjectDevice**.

    ```
    Ok, turning the {SubjectDevice} {OnOff}
    ```
1. Wählen Sie **Speichern** aus.

### <a name="try-it-out"></a>Ausprobieren
1. Wählen Sie oben im rechten Bereich das **Trainieren**-Symbol aus.

1. Nachdem das Training abgeschlossen ist, wählen Sie **Testen** aus. Es wird ein Fenster **Testen Ihrer Anwendung** angezeigt.
 Versuchen Sie, einige Interaktionen auszuführen.

    - Eingabe: Schalte Fernseher aus
    - Ausgabe: OK, schalte Fernseher aus
    - Eingabe: Schalte Fernsehgerät aus
    - Ausgabe: OK, schalte Fernseher aus
    - Eingabe: Schalte es aus
    - Ausgabe: Welches Gerät soll gesteuert werden?
    - Eingabe: den Fernseher
    - Ausgabe: OK, schalte Fernseher aus

## <a name="configure-parameters-for-settemperature-command"></a>Konfigurieren von Parametern für den Befehl „SetTemperature“

Ändern Sie den Befehl **SetTemperature** so, dass er die Temperatur nach den Anweisungen des Benutzers einstellen kann.

Hinzufügen des neuen Parameters **Temperature** mit folgender Konfiguration

| Konfiguration      | Vorgeschlagener Wert     |
| ------------------ | ----------------|
| Name               | `Temperature`           |
| Erforderlich           | Aktiviert         |
| Antwort auf erforderlichen Parameter      | Einfacher Editor > `What temperature would you like?`
| type               | Number          |


Ändern Sie die Beispieläußerungen in die folgenden Werte.

```
set the temperature to {Temperature} degrees
change the temperature to {Temperature}
set the temperature
change the temperature
```

Bearbeiten Sie die vorhandenen Vervollständigungsregeln gemäß der folgenden Konfiguration.

| Konfiguration      | Vorgeschlagener Wert     |
| ------------------ | ----------------|
| Bedingungen         | Erforderlicher Parameter > Temperature           |
| Aktionen           | Sprachantwort senden > `Ok, setting temperature to {Temperature} degrees` |

### <a name="try-it-out"></a>Ausprobieren

**Trainieren** und **Testen** Sie die Änderungen mit einigen wenigen Interaktionen.

- Eingabe: Stelle die Temperatur ein
- Ausgabe: Welche Temperatur hätten Sie gerne?
- Eingabe: 10 Grad
- Ausgabe: OK, stelle die Temperatur auf 10 Grad ein

## <a name="configure-parameters-to-the-setalarm-command"></a>Konfigurieren von Parametern für den Befehl „SetAlarm“

Fügen Sie einen Parameter mit der Bezeichnung **DateTime** und der folgenden Konfiguration hinzu.

   | Einstellung                           | Empfohlener Wert                     | 
   | --------------------------------- | ----------------------------------------|
   | Name                              | `DateTime`                               |
   | Erforderlich                          | Aktiviert                                 |
   | Antwort auf erforderlichen Parameter   | Einfacher Editor > `For what time?`            | 
   | type                              | Datetime                                |
   | Datumsstandardwerte                     | If date is missing use today (Bei fehlendem Datum heutiges Datum verwenden)            |
   | Uhrzeitstandardwerte                     | If time is missing use start of day (Bei fehlender Uhrzeit Tagesbeginn verwenden)     |


> [!NOTE]
> In diesem Artikel haben wir vorwiegend die Parametertypen „Zeichenfolge“, „Zahl“ und „DateTime“ verwendet. Eine Liste aller unterstützten Parametertypen und deren Eigenschaften finden Sie unter [Referenzen](./custom-commands-references.md).


Ändern Sie die Beispieläußerungen in die folgenden Werte.

```
set an alarm for {DateTime}
set alarm {DateTime}
alarm for {DateTime}
```

Bearbeiten Sie die vorhandenen Vervollständigungsregeln gemäß der folgenden Konfiguration.

   | Einstellung    | Vorgeschlagener Wert                               |
   | ---------- | ------------------------------------------------------- |
   | Aktionen    | Sprachantwort senden: `Ok, alarm set for {DateTime}`  |


### <a name="try-it-out"></a>Ausprobieren

**Trainieren** und **Testen** Sie die Änderungen.
- Eingabe: Set alarm for tomorrow at noon (Alarm für morgen Mittag festlegen)
- Ausgabe: „Ok, alarm set for 2020-05-02 12:00:00“ (OK, Alarm für 02.05.2020 12 Uhr festgelegt)
- Eingabe: Set an alarm (Alarm festlegen)
- Ausgabe: „What time?“ (Welche Uhrzeit)
- Eingabe: 5pm (17 Uhr)
- Ausgabe: „Ok, alarm set for 2020-05-01 17:00:00“ (OK, Alarm für 01.05.2020 17 Uhr festgelegt)


## <a name="try-out-all-the-commands"></a>Probieren Sie alle Befehle aus

Testen Sie alle drei Befehle zusammen mit Äußerungen, die sich auf verschiedene Befehle beziehen. Beachten Sie, dass Sie zwischen den verschiedenen Befehlen wechseln können.

- Eingabe: Set an alarm (Alarm festlegen)
- Ausgabe: Welche Uhrzeit?
- Eingabe: Turn on the tv (Schalte den Fernseher ein)
- Ausgabe: OK, Fernseher wird eingeschaltet
- Eingabe: Set an alarm (Alarm festlegen)
- Ausgabe: Welche Uhrzeit?
- Eingabe: 5pm (17 Uhr)
- Ausgabe: „Ok, alarm set for 2020-05-01 17:00:00“ (OK, Alarm für 01.05.2020 17 Uhr festgelegt)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Vorgehensweise: Hinzufügen von Konfigurationen zu Befehlsparametern](./how-to-custom-commands-add-parameter-configuration.md)
