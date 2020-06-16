---
title: 'Schnellstart: Erstellen einer App für benutzerdefinierte Befehle mit Parametern (Vorschau) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel fügen Sie einer Anwendung für benutzerdefinierte Befehle Parameter hinzu, sodass sie mehrere Geräte ein- und ausschalten kann.
services: cognitive-services
author: nitinme
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: nitinme
ms.openlocfilehash: 940b3604487fbef7736c0d8f4f9299563b0b86d5
ms.sourcegitcommit: 5504d5a88896c692303b9c676a7d2860f36394c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2020
ms.locfileid: "84509303"
---
# <a name="quickstart-create-a-custom-commands-preview-application-with-parameters"></a>Schnellstart: Erstellen einer Anwendung für benutzerdefinierte Befehle (Vorschau) mit Parametern

Im [vorherigen Artikel](./quickstart-custom-speech-commands-create-new.md) haben Sie eine einfache Anwendung für benutzerdefinierte Befehle ohne Parameter erstellt.

In diesem Artikel erweitern Sie diese Anwendung mit Parametern, sodass sie mehrere Geräte ein- und ausschalten kann.

## <a name="create-parameters"></a>Erstellen von Parametern

1. Öffnen Sie das Projekt, [das Sie im vorherigen Artikel erstellt haben](./quickstart-custom-speech-commands-create-new.md).

   Wir bearbeiten den vorhandenen Befehl, damit er zum Ein- und Ausschalten mehrerer Geräte verwendet werden kann.
1. Da der Befehl jetzt das Ein- und Ausschalten übernimmt, benennen Sie den Befehl in **TurnOnOff** um.
   1. Wählen Sie im linken Bereich den Befehl **TurnOn** aus, und wählen Sie dann die Schaltfläche mit den Auslassungspunkten ( **...** ) neben **Neuer Befehl** am oberen Rand des Bereichs aus.
   
   1. Wählen Sie **Umbenennen** aus. Ändern Sie im Fenster **Befehl „Umbenennen“** den **Namen** in **TurOnOff**. Wählen Sie **Speichern** aus.

1. Erstellen Sie einen Parameter, der angibt, ob der Benutzer das Gerät ein- oder ausschalten möchte.
   1. Wählen Sie im mittleren Bereich **Hinzufügen** aus. Wählen Sie in der Dropdownliste die Option **Parameter** aus.
   1. Fügen Sie im rechten Bereich im Abschnitt **Parameter** einen Wert in das Feld **Name** ein.
   1. Wählen Sie **Erforderlich** aus. Wählen Sie im Fenster **Add response for a required parameter** (Antwort für einen erforderlichen Parameter hinzufügen) **Simple editor** (Einfacher Editor) aus. Geben Sie im Feld **First variation (Erste Variation)** diesen Text ein:
        ```
        On or Off?
        ```
   1. Wählen Sie **Update** aus.

       > [!div class="mx-imgBorder"]
       > ![Antwort für erforderlichen Parameter erstellen](media/custom-speech-commands/add-required-on-off-parameter-response.png)
   
1. Konfigurieren Sie die restlichen Eigenschaften des Parameters wie folgt:
       

    | Konfiguration      | Vorgeschlagener Wert     | Beschreibung                                                      |
    | ------------------ | ----------------| ---------------------------------------------------------------------|
    | **Name**               | **OnOff**           | Ein aussagekräftiger Name für den Parameter.                                                                  |
    | **Is Global**          | Gelöscht       | Ein Kontrollkästchen, das angibt, ob ein Wert für diesen Parameter global auf alle Befehle in der Anwendung angewendet wird.|
    | **Erforderlich**           | Aktiviert         | Ein Kontrollkästchen, das angibt, ob ein Wert für den Parameter erforderlich ist.  |
    | **Antwort auf erforderlichen Parameter**      |**Einfacher Editor > Ein oder Aus?**      | Eine Aufforderung, nach dem Wert des Parameters zu fragen, wenn er nicht bekannt ist. |
    | **Typ**               | **String**          | Der Typ des Parameters. Beispiel: Number, String, DateTime, Geography.   |
    | **Configuration**      | **Vordefinierte Eingabewerte aus einem internen Katalog akzeptieren** | Für Zeichenfolgen schränkt diese Einstellung Eingaben auf eine Reihe möglicher Werte ein. |
    | **Vordefinierte Eingabewerte**     | **on**, **off**             | Eine Menge möglicher Werte und ihrer Aliase.         |
       


    > [!div class="mx-imgBorder"]
    > ![Erstellen von Parametern](media/custom-speech-commands/create-on-off-parameter.png)

1. Klicken Sie auf **Speichern**, um die Einstellungen zu speichern.

 1. Wählen Sie **Hinzufügen** erneut aus, um einen zweiten Parameter hinzuzufügen. Dieser Parameter entspricht dem Gerätenamen. Verwenden Sie die folgenden Einstellungen:
   

       | Einstellung            | Vorgeschlagener Wert       | Beschreibung                                                                                               |
       | ------------------ | --------------------- | --------------------------------------------------------------------------------------------------------- |
       | **Name**               | **SubjectDevice**         | Ein aussagekräftiger Name für den Parameter.                                                                     |
       | **Is Global**          | Gelöscht             | Ein Kontrollkästchen, das angibt, ob ein Wert für diesen Parameter global auf alle Befehle in der Anwendung angewendet wird. |
       | **Erforderlich**           | Aktiviert               | Ein Kontrollkästchen, das angibt, ob ein Wert für den Parameter erforderlich ist.          |
       | **Einfacher Editor**      | **Welches Gerät?**    | Eine Aufforderung, nach dem Wert des Parameters zu fragen, wenn er nicht bekannt ist.                                       |
       | **Typ**               | **String**                | Der Typ des Parameters. Beispiel: Number, String, DateTime, Geography.                                                |
       | **Configuration**      | **Vordefinierte Eingabewerte aus einem internen Katalog akzeptieren** | Für Zeichenfolgen schränkt diese Einstellung Eingaben auf eine Reihe möglicher Werte ein.       |
       | **Vordefinierte Eingabewerte** | **Fernseher**, **Lüfter**               | Eine Menge möglicher Werte und ihrer Aliase.                               |
       | **Aliase** (Fernseher)      | **Fernsehgerät**, **TV**     | Optionale Aliase für jeden der vordefinierten Eingabewerte.                                 |

## <a name="add-example-sentences"></a>Hinzufügen von Beispielsätzen

Bei Befehlen mit Parametern ist es hilfreich, Beispielsätze hinzuzufügen, die alle möglichen Kombinationen abdecken. Beispiel:

- Vollständige Parameterinformationen: `turn {OnOff} the {SubjectDevice}`
- Teilweise Parameterinformationen: `turn it {OnOff}`
- Keine Parameterinformationen: `turn something`

Beispielsätze mit unterschiedlichem Informationsumfang ermöglichen der Anwendung für benutzerdefinierte Befehle, sowohl einmalige als auch mehrere Lösungen mit Teilinformationen aufzulösen.

Bearbeiten Sie vor diesem Hintergrund die Beispielsätze, um die Parameter wie hier vorgeschlagen zu verwenden:

```
turn {OnOff} the {SubjectDevice}
{SubjectDevice} {OnOff}
turn it {OnOff}
turn something {OnOff}
turn something
```
> [!TIP]
> Verwenden Sie im Editor für Beispielsätze Klammern, um auf Ihre Parameter zu verweisen: `turn {OnOff} the {SubjectDevice}`.
>
> Verwenden Sie die Tab-Taste für automatische Vervollständigung auf der Grundlage der zuvor erstellten Parameter.

## <a name="add-parameters-to-completion-rules"></a>Hinzufügen von Parametern zu Vervollständigungsregeln

Ändern Sie die Vervollständigungsregel, die Sie im [vorherigen Schnellstart](./quickstart-custom-speech-commands-create-new.md) erstellt haben.

1. Wählen Sie im Abschnitt **Bedingung** die Option **Bedingung hinzufügen** aus.
1. Wählen Sie im Fenster **Neue Bedingung** in der Liste **Typ** die Option **Erforderliche Parameter**aus. Wählen Sie in der Liste sowohl **OnOff** als auch **SubjectDevice** aus.
1. Klicken Sie auf **Erstellen**.
1. Bearbeiten Sie im Abschnitt **Aktionen** die vorhandene Aktion **Sprachantwort senden**, indem Sie mit der Maus auf die Aktion zeigen und das Bearbeiten-Symbol auswählen. Verwenden Sie dieses Mal die neuen Parameter `OnOff` und `SubjectDevice`:

    ```
    Ok, turning {OnOff} the {SubjectDevice}
    ```

## <a name="try-it-out"></a>Ausprobieren
1. Wählen Sie oben im rechten Bereich **Trainieren** aus.

1. Wenn das Training abgeschlossen ist, wählen Sie **Testen** aus.
    
    Es wird ein Fenster **Testen Ihrer Anwendung** angezeigt.

1. Versuchen Sie, einige Interaktionen auszuführen.

        - Input: turn off the tv
        - Output: Ok, turning off the tv        
        - Input: turn off the television
        - Output: Ok, turning off the tv
        - Input: turn it off
        - Output: Which device?
        - Input: the tv
        - Output: Ok, turning off the tv

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Schnellstart: Verwenden benutzerdefinierter Befehle mit Custom Voice (Vorschau)](./quickstart-custom-speech-commands-select-custom-voice.md)
