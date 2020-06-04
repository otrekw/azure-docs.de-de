---
title: 'Schnellstart: Erstellen eines benutzerdefinierten Befehls mit Parametern (Vorschau) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel fügen Sie einer Anwendung für benutzerdefinierte Befehle Parameter hinzu.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: donkim
ms.openlocfilehash: bf77616123f9311f7384fea515f250e47b354c8c
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82853613"
---
# <a name="quickstart-create-a-custom-commands-application-with-parameters-preview"></a>Schnellstart: Erstellen einer Anwendung für benutzerdefinierte Befehle mit Parametern (Vorschau)

Im [vorherigen Artikel](./quickstart-custom-speech-commands-create-new.md) haben Sie eine einfache Anwendung für benutzerdefinierte Befehle ohne Parameter erstellt.

In diesem Artikel erweitern Sie diese Anwendung um Parameter, sodass sie das Ein- und Ausschalten mehrerer Geräte übernehmen kann.

## <a name="create-parameters"></a>Erstellen von Parametern

1. Öffnen Sie das Projekt, [das Sie zuvor erstellt haben](./quickstart-custom-speech-commands-create-new.md).
1. Bearbeiten wir den vorhandenen Befehl zum Ein- und Ausschalten mehrerer Geräte.
1. Da der Befehl jetzt das Ein- und Ausschalten übernimmt, benennen Sie den Befehl in `TurnOnOff` um.
   - Wählen Sie im linken Bereich den `TurnOn`-Befehl aus, und klicken Sie dann auf das `...`-Symbol neben `+ New command` oben im Bereich.
   
   - Wählen Sie das `Rename`-Symbol aus. Ändern Sie **Name** im Popupfenster **Befehl umbenennen** in `TurOnOff`. Wählen Sie dann **Speichern** aus.

1. Erstellen Sie anschließend einen neuen Parameter, der angibt, ob der Benutzer das Gerät ein- oder ausschalten möchte.
   - Wählen Sie das `+ Add`-Symbol aus, das oben im mittleren Bereich angezeigt wird. Wählen Sie in der Dropdownliste **Parameter** aus.
   - Im äußerst rechten Bereich können Sie den Konfigurationsabschnitt für **Parameter** sehen.
   - Fügen Sie einen Wert für **Name** hinzu.
   - Aktivieren Sie das Kontrollkästchen **Erforderlich**. Wählen Sie im Fenster **Add response for a required parameter** (Antwort für einen erforderlichen Parameter hinzufügen) **Simple editor** (Einfacher Editor) aus, und fügen Sie der **First variation** (Erste Variation) dies hinzu:
        ```
        On or Off?
        ```
   - Wählen Sie **Update** aus.

       > [!div class="mx-imgBorder"]
       > ![Antwort für erforderlichen Parameter erstellen](media/custom-speech-commands/add-required-on-off-parameter-response.png)
   
   - Konfigurieren wir als nächstes die restlichen Eigenschaften des Parameters wie folgt. Wählen Sie dann `Save` aus, um alle Konfigurationen des Parameters zu speichern.
       

       | Konfiguration      | Vorgeschlagener Wert     | BESCHREIBUNG                                                      |
       | ------------------ | ----------------| ---------------------------------------------------------------------|
       | Name               | OnOff           | Ein aussagekräftiger Name für den Parameter                                                                           |
       | Is Global          | Deaktiviert       | Kontrollkästchen, das angibt, ob ein Wert für diesen Parameter global auf alle Befehle in der Anwendung angewendet wird.|
       | Erforderlich           | Aktiviert         | Kontrollkästchen, das angibt, ob ein Wert für diesen Parameter vor dem Abschließen des Befehls erforderlich ist. |
       | Antwort auf erforderlichen Parameter      |Einfacher Editor > Ein oder Aus?      | Eine Aufforderung, nach dem Wert dieses Parameters zu fragen, wenn er nicht bekannt ist. |
       | type               | String          | Der Typ des Parameters, z. B. Zahl, Zeichenfolge, Datum/Uhrzeit oder Geografie.   |
       | Konfiguration      | Vordefinierte Eingabewerte aus einem internen Katalog akzeptieren | Für Zeichenfolgen, schränkt die Eingaben auf eine Reihe möglicher Werte ein |
       | Vordefinierte Eingabewerte     | on, off             | Menge möglicher Werte und ihrer Aliase         |
       
        > [!div class="mx-imgBorder"]
        > ![Erstellen von Parametern](media/custom-speech-commands/create-on-off-parameter.png)

   - Als nächstes wählen Sie erneut das Symbol `+ Add` aus, um einen zweiten Parameter mit der folgenden Konfiguration hinzuzufügen, der den Namen der Geräte darstellt.
   

       | Einstellung            | Vorgeschlagener Wert       | BESCHREIBUNG                                                                                               |
       | ------------------ | --------------------- | --------------------------------------------------------------------------------------------------------- |
       | Name               | SubjectDevice         | Ein aussagekräftiger Name für den Parameter                                                                     |
       | Is Global          | Deaktiviert             | Kontrollkästchen, das angibt, ob ein Wert für diesen Parameter global auf alle Befehle in der Anwendung angewendet wird. |
       | Erforderlich           | Aktiviert               | Kontrollkästchen, das angibt, ob ein Wert für diesen Parameter vor dem Abschließen des Befehls erforderlich ist.          |
       | Einfacher Editor      | Welches Gerät?    | Eine Aufforderung, nach dem Wert dieses Parameters zu fragen, wenn er nicht bekannt ist.                                       |
       | type               | String                | Der Typ des Parameters, z. B. Zahl, Zeichenfolge, Datum/Uhrzeit oder Geografie.                                                |
       | Konfiguration      | Vordefinierte Eingabewerte aus einem internen Katalog akzeptieren | Für Zeichenfolgen beschränkt eine Zeichenfolgenliste die Eingaben auf eine Reihe von möglichen Werten.       |
       | Vordefinierte Eingabewerte | Fernseher, Lüfter               | Menge möglicher Werte und ihrer Aliase                               |
       | Aliase (Fernseher)      | Fernsehgerät, TV     | Optionale Aliase für jeden möglichen Wert der vordefinierten Eingabewerte                                 |

## <a name="add-example-sentences"></a>Hinzufügen von Beispielsätzen

Bei Befehlen mit Parametern ist es hilfreich, Beispielsätze hinzuzufügen, die alle möglichen Kombinationen abdecken. Beispiel:

1. Vollständige Parameterinformationen – `turn {OnOff} the {SubjectDevice}`
1. Teilweise Parameterinformationen – `turn it {OnOff}`
1. Keine Parameterinformationen – `turn something`

Beispielsätze mit unterschiedlichem Informationsumfang ermöglichen es der Anwendung für benutzerdefinierte Befehle, sowohl einmalige als auch mehrfach ausgeführte Lösungen mit Teilinformationen aufzulösen.

Bearbeiten Sie vor diesem Hintergrund die Beispielsätze, um die Parameter wie unten vorgeschlagen zu verwenden.

```
turn {OnOff} the {SubjectDevice}
{SubjectDevice} {OnOff}
turn it {OnOff}
turn something {OnOff}
turn something
```
> [!TIP]
> Verwenden Sie im Editor für Beispielsätze geschweifte Klammern, um auf Ihre Parameter zu verweisen. - `turn {OnOff} the {SubjectDevice}` Verwenden Sie die Tab-Taste für automatische Vervollständigung auf der Grundlage der zuvor erstellten Parameter.

## <a name="add-parameters-to-completion-rules"></a>Hinzufügen von Parametern zu Vervollständigungsregeln

Ändern Sie die Vervollständigungsregel, die wir im [vorherigen Schnellstart](./quickstart-custom-speech-commands-create-new.md) erstellt haben.

1. Fügen Sie im Abschnitt **Bedingungen** eine neue Bedingung hinzu, indem Sie **Bedingungen hinzufügen** auswählen.
1. Wählen Sie im neuen Popupelement **Neue Bedingung** in der Dropdownliste **Typ** `Required parameters` aus. Aktivieren Sie in der Prüfliste unten sowohl `OnOff` als auch `SubjectDevice`.
1. Klicken Sie auf **Erstellen**.
1. Bearbeiten Sie im Abschnitt **Aktionen** die vorhandene Aktion zum Senden von Sprachantworten, indem Sie mit der Maus auf die Aktion zeigen und auf das Bearbeiten-Symbol klicken. Verwenden wir diesmal die neu erstellten Parameter `OnOff` und `SubjectDevice`

    ```
    Ok, turning {OnOff} the {SubjectDevice}
    ```

## <a name="try-it-out"></a>Ausprobieren
1. Wählen Sie das `Train`-Symbol aus, das oben im rechten Bereich angezeigt wird.

1. Nachdem das Training abgeschlossen ist, wählen Sie `Test` aus.
    - Es wird ein neues Fenster **Testen Ihrer Anwendung** angezeigt.
    - Versuchen Sie, einige Interaktionen auszuführen.

        - Eingabe: Schalte Fernseher aus
        - Ausgabe: OK, schalte Fernseher aus        
        - Eingabe: Schalte Fernsehgerät aus
        - Ausgabe: OK, schalte Fernseher aus
        - Eingabe: Schalte es aus
        - Ausgabe: Welches Gerät?
        - Eingabe: den Fernseher
        - Ausgabe: OK, schalte Fernseher aus

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Schnellstart: Verwenden benutzerdefinierter Befehle mit Custom Voice (Vorschau)](./quickstart-custom-speech-commands-select-custom-voice.md)
