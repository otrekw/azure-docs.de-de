---
title: 'Anleitung: Hinzufügen von Validierungen zu Parametern benutzerdefinierter Befehle'
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
ms.openlocfilehash: 2b7fd608156ab269cfc0c85c6c508fa9d5eebc83
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82857189"
---
# <a name="how-to-add-validations-to-custom-command-parameters-preview"></a>Anleitung: Hinzufügen von Validierungen zu Parametern benutzerdefinierter Befehle (Vorschau)

In diesem Artikel fügen Sie Validierungen zu Parametern und Korrekturaufforderungen hinzu.

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen die Schritte in den folgenden Artikeln durchgeführt haben:

> [!div class="checklist"]
> * [Schnellstart: Erstellen eines benutzerdefinierten Befehls](./quickstart-custom-speech-commands-create-new.md)
> * [Schnellstart: Erstellen eines benutzerdefinierten Befehls mit Parametern](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-settemperature-command"></a>Erstellen eines SetTemperature-Befehls

Um Validierungen zu veranschaulichen, erstellen wir einen neuen Befehl, mit dem Benutzer die Temperatur einstellen können.

1. Öffnen Sie Ihre zuvor erstellte Anwendung für benutzerdefinierte Befehle in [Speech Studio](https://speech.microsoft.com/).
1. Erstellen Sie einen neuen Befehl `SetTemperature`.
1. Fügen Sie einen Parameter für die Zieltemperatur hinzu.

   | Parameterkonfiguration           | Vorgeschlagener Wert    |BESCHREIBUNG                 |                                    
   | ----------------- | ----------------------------------| -------------|
   | Name              | Temperatur                       | Ein aussagekräftiger Name für den Parameter                                |
   | Erforderlich          | Aktiviert                           | Kontrollkästchen, das angibt, ob ein Wert für diesen Parameter vor dem Abschließen des Befehls erforderlich ist. |
   | Antwort auf erforderlichen Parameter     | Einfacher Editor > What temperature would you like? (Welche Temperatur möchten Sie einstellen?)  | Eine Aufforderung, nach dem Wert dieses Parameters zu fragen, wenn er nicht bekannt ist. |
   | type              | Number                            | Typ des Parameters, z. B. Zahl, Zeichenfolge, Datum/Uhrzeit oder Geografie.   |

1. Fügen Sie dem Temperaturparameter eine Validierung hinzu.

    - Wählen Sie auf der Konfigurationsseite **Parameter** für den `Temperature`-Parameter `Add a validation` aus dem Abschnitt „Validierungen“ hinzu.
    - Setzen Sie die Werte im Popupelement **Neue Validierung**  wie folgt ein, und wählen Sie **Erstellen** aus.

  
       | Parameterkonfiguration         | Vorgeschlagener Wert                                          | BESCHREIBUNG                                                                        |
       | ----------------- | -------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
       | Mindestwert        | 60               | Bei Zahlenparameter der Minimalwert, den der Parameter annehmen kann |
       | Höchstwert        | 80               | Bei Zahlenparameter der Maximalwert, den der Parameter annehmen kann |
       | Fehlerantwort: Einfacher Editor| Erste Variation: Sorry, ich kann nur Werte zwischen 15° und 26° einstellen      | Aufforderung zur Eingabe eines neuen Werts, wenn die Validierung fehlschlägt                                       |

       > [!div class="mx-imgBorder"]
       > ![Hinzufügen einer Bereichsvalidierung](media/custom-speech-commands/validations-add-temperature.png)

1. Fügen Sie einige Beispielsätze hinzu.

   ```
   set the temperature to {Temperature} degrees
   change the temperature to {Temperature}
   set the temperature
   change the temperature
   ```

1. Hinzufügen einer Vervollständigungsregel zur Bestätigung des Ergebnisses

   | Einstellung    | Vorgeschlagener Wert                                           |BESCHREIBUNG                                     |
   | ---------- | --------------------------------------------------------- |-----|
   | Name       | Bestätigungsmeldung                                      |Ein Name, der den Zweck der Regel beschreibt. |
   | Bedingungen | Erforderliche Parameter: `Temperature`                       |Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann.    |   
   | Aktionen    | Sprachantwort senden: `Ok, setting temperature to {Temperature} degrees` | Die durchzuführende Aktion, wenn die Bedingung der Regel erfüllt ist (true). |

> [!TIP]
> In diesem Beispiel wird eine Sprachantwort verwendet, um das Ergebnis zu bestätigen. Beispiele für das Vervollständigen des Befehls mit einer Clientaktion finden Sie unter: [Vorgehensweise: Ausführen von Befehlen auf dem Client mit dem Speech SDK](./how-to-custom-speech-commands-fulfill-sdk.md)


## <a name="try-it-out"></a>Ausprobieren
1. Wählen Sie das `Train`-Symbol aus, das oben im rechten Bereich angezeigt wird.

1. Sobald das Training abgeschlossen ist, wählen Sie `Test` aus, und probieren Sie ein paar Interaktionen aus.

    - Eingabe: Stelle die Temperatur auf 22 Grad ein
    - Ausgabe: OK, stelle die Temperatur auf 22 Grad ein
    - Eingabe: Stelle die Temperatur auf 7 Grad ein
    - Ausgabe: Sorry, ich kann nur Werte zwischen 15 und 26 Grad einstellen.
    - Eingabe: Stelle stattdessen 22 Grad ein
    - Ausgabe: OK, stelle die Temperatur auf 22 Grad ein

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Vorgehensweise: Hinzufügen einer Bestätigung zu einem Benutzerdefinierten Befehl (Vorschau)](./how-to-custom-speech-commands-confirmations.md)
