---
title: Hinzufügen von Validierungen zu benutzerdefinierten Befehlen (Vorschau) – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie einem Befehlsparameter in einer App für benutzerdefinierte Befehle (Vorschau) Validierungen hinzufügen.
services: cognitive-services
author: nitinme
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: nitinme
ms.openlocfilehash: eb011510a9f636aea9910a4be445cd094acf0c21
ms.sourcegitcommit: 5504d5a88896c692303b9c676a7d2860f36394c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2020
ms.locfileid: "84509575"
---
# <a name="add-validations-to-a-command-parameter-in-a-custom-commands-preview-application"></a>Hinzufügen von Validierungen zu einem Befehlsparameter in einer Anwendung für benutzerdefinierte Befehle (Vorschau)

In diesem Artikel erfahren Sie, wie Sie Validierungen zu Parametern und Aufforderungen zur Korrektur hinzufügen.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte in den folgenden Artikeln aus:

> [!div class="checklist"]
 
> * [Schnellstart: Erstellen einer App für benutzerdefinierte Befehle (Vorschau)](./quickstart-custom-speech-commands-create-new.md)
> * [Schnellstart: Erstellen einer App für benutzerdefinierte Befehle (Vorschau) mit Parametern](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-settemperature-command"></a>Erstellen eines SetTemperature-Befehls

Um Validierungen zu veranschaulichen, erstellen wir einen neuen Befehl, mit dem Benutzer die Temperatur einstellen können.

1. Öffnen Sie in [Speech Studio](https://speech.microsoft.com/) die von Ihnen erstellte App für benutzerdefinierte Befehle (Vorschau).
1. Erstellen Sie einen neuen **SetTemperature**-Befehl.
1. Fügen Sie einen Temperaturparameter mit der folgenden Konfiguration hinzu:

   | Parameterkonfiguration           | Vorgeschlagener Wert    |Beschreibung                 |                                    
   | ----------------- | ----------------------------------| -------------|
   | **Name**              | **Temperature**                       | Ein aussagekräftiger Name für den Parameter                                |
   | **Erforderlich**          | Aktiviert                           | Kontrollkästchen, das angibt, ob ein Wert für diesen Parameter vor dem Abschließen des Befehls erforderlich ist |
   | **Antwort auf erforderlichen Parameter**     | **Einfacher Editor > What temperature would you like? (Welche Temperatur möchten Sie einstellen?)**  | Eine Aufforderung, nach dem Wert dieses Parameters zu fragen, wenn er nicht bekannt ist. |
   | **Typ**              | **Number**                            | Typ des Parameters, z. B. Number, String, DateTime oder Geography.   |

1. Fügen Sie dem Temperaturparameter eine Validierung hinzu.

    1. Wählen Sie auf der Konfigurationsseite **Parameter** für den Temperaturparameter **Validierung hinzufügen** im Abschnitt **Validierungen** aus.

    1. Konfigurieren Sie die Validierung im Popupfenster **Neue Validierung** wie folgt:
  
       | Parameterkonfiguration         | Vorgeschlagener Wert                                          | BESCHREIBUNG                                                                        |
       | ----------------- | -------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
       | **Mindestwert**        | **60**               | Bei Zahlenparameter der Minimalwert, den der Parameter annehmen kann |
       | **Höchstwert**        | **80**               | Bei Zahlenparameter der Maximalwert, den der Parameter annehmen kann |
       | **Fehlerantwort: Einfacher Editor**| **Erste Variation: Sorry, ich kann nur Werte zwischen 15° und 26° einstellen**      | Aufforderung zur Eingabe eines neuen Werts, wenn die Validierung fehlschlägt                                       |

       > [!div class="mx-imgBorder"]
       > ![Hinzufügen einer Bereichsvalidierung](media/custom-speech-commands/validations-add-temperature.png)

1. Klicken Sie auf **Erstellen**.

1. Fügen Sie einige Beispielsätze hinzu.

   ```
   set the temperature to {Temperature} degrees
   change the temperature to {Temperature}
   set the temperature
   change the temperature
   ```

1. Fügen Sie eine Vervollständigungsregel mit der folgenden Konfiguration hinzu. Mit dieser Regel wird das Ergebnis bestätigt.

   | Einstellung    | Vorgeschlagener Wert                                           |BESCHREIBUNG                                     |
   | ---------- | --------------------------------------------------------- |-----|
   | Name       | Bestätigungsmeldung                                      |Ein Name, der den Zweck der Regel beschreibt. |
   | **Bedingungen** | **Erforderliche Parameter – Temperatur**                       |Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann.    |   
   | **Aktionen**    | **Sprachantwort senden –> OK, stelle die Temperatur auf {Temperatur} Grad ein** | Die durchzuführende Aktion, wenn die Bedingung der Regel erfüllt ist (true). |

> [!TIP]
> In diesem Beispiel wird eine Sprachantwort verwendet, um das Ergebnis zu bestätigen. Beispiele für das Vervollständigen des Befehls mit einer Clientaktion finden Sie unter [Anleitung: Ausführen von Befehlen auf einem Client mit dem Speech SDK (Vorschau)](./how-to-custom-speech-commands-fulfill-sdk.md).

## <a name="try-it-out"></a>Ausprobieren

1. Wählen Sie **Trainieren** aus.

1. Nachdem das Training abgeschlossen ist, wählen Sie **Testen** aus, und probieren Sie diese Interaktionen aus:

    - Eingabe: Stelle die Temperatur auf 22 Grad ein
    - Ausgabe: OK, stelle die Temperatur auf 22 Grad ein
    - Eingabe: Stelle die Temperatur auf 7 Grad ein
    - Ausgabe: Sorry, ich kann nur Werte zwischen 15 und 26 Grad einstellen.
    - Eingabe: Stelle stattdessen 22 Grad ein
    - Ausgabe: OK, stelle die Temperatur auf 22 Grad ein

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen von Bestätigungen zu einem Befehl in einer App für benutzerdefinierte Befehle (Vorschau)](./how-to-custom-speech-commands-confirmations.md)
