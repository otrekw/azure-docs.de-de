---
title: Ausführen der benutzerdefinierten Befehle auf dem Client mit dem Speech SDK
titleSuffix: Azure Cognitive Services
description: In diesem Artikel wird erläutert, wie Aktivitäten benutzerdefinierter Befehle auf dem Client mit dem Speech SDK verarbeitet werden.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: donkim
ms.openlocfilehash: b55bb1c8379cf0a80a95aa0ba1a29297154d5831
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76156504"
---
# <a name="how-to-fulfill-commands-on-the-client-with-the-speech-sdk-preview"></a>Anleitung: Ausführen der Befehle auf dem Client mit dem Speech SDK (Vorschau)

Um Aufgaben mit einer Anwendung für benutzerdefinierte Befehle abzuschließen, können Sie benutzerdefinierte Nutzlasten an ein verbundenes Clientgerät senden.

In diesem Artikel führen Sie Folgendes durch:

- Definieren und Senden einer benutzerdefinierten JSON-Nutzlast aus Ihrer Anwendung für benutzerdefinierte Befehle.
- Empfangen und Visualisieren der benutzerdefinierten JSON-Nutzlastinhalte aus einer C# UWP Speech SDK-Clientanwendung.

## <a name="prerequisites"></a>Voraussetzungen

- [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)
- Ein Azure-Abonnementschlüssel für den Spracherkennungsdienst
  - [Beziehen Sie einen kostenlos](get-started.md), oder erstellen Sie ihn im [Azure-Portal](https://portal.azure.com)
- Eine zuvor erstellte Anwendung für benutzerdefinierte Befehle
  - [Schnellstart: Erstellen eines benutzerdefinierten Befehls mit Parametern (Vorschau)](./quickstart-custom-speech-commands-create-parameters.md)
- Eine Speech SDK-fähige Clientanwendung
  - [Schnellstart: Herstellen einer Verbindung mit einer Anwendung für benutzerdefinierte Befehle mit dem Speech SDK (Vorschau)](./quickstart-custom-speech-commands-speech-sdk.md)

## <a name="optional-get-started-fast"></a>Optional: Schneller Einstieg

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie eine Clientanwendung erstellen, um mit Ihrer Anwendung für benutzerdefinierte Befehle zu sprechen. Wenn Sie lieber sofort loslegen möchten, finden Sie den kompilierbereiten Quellcode, der in diesem Artikel verwendet wird, in den [Speech SDK-Beispielen](https://aka.ms/csspeech/samples).

## <a name="fulfill-with-json-payload"></a>Ausführen mit JSON-Nutzlast

1. Öffnen Sie Ihre zuvor erstellte Anwendung für benutzerdefinierte Befehle über [Speech Studio](https://speech.microsoft.com/).
1. Überprüfen Sie den Abschnitt **Vervollständigungsregeln**, um sicherzustellen, dass Sie über die zuvor erstellte Regel verfügen, die dem Benutzer antwortet.
1. Um eine Nutzlast direkt an den Client zu senden, erstellen Sie eine neue Regel mit einer Aktion zum Senden von Aktivitäten.

   > [!div class="mx-imgBorder"]
   > ![Vervollständigungsregel zum Senden von Aktivitäten](media/custom-speech-commands/fulfill-sdk-completion-rule.png)

   | Einstellung | Vorgeschlagener Wert | Beschreibung |
   | ------- | --------------- | ----------- |
   | Regelname | UpdateDeviceState | Ein Name, der den Zweck der Regel beschreibt. |
   | Bedingungen | Erforderlicher Parameter – `OnOff` und `SubjectDevice` | Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann. |
   | Aktionen | `SendActivity` (siehe unten) | Die durchzuführende Aktion, wenn die Bedingung der Regel erfüllt ist (true). |

   > [!div class="mx-imgBorder"]
   > ![Nutzlast zum Senden von Aktivitäten](media/custom-speech-commands/fulfill-sdk-send-activity-action.png)

   ```json
   {
     "type": "event",
     "name": "UpdateDeviceState",
     "state": "{OnOff}",
     "device": "{SubjectDevice}"
   }
   ```

## <a name="create-visuals-for-device-on-or-off-state"></a>Erstellen von visuellen Elementen für den ein- oder ausgeschalteten Zustand des Geräts

In [Schnellstart: Herstellen einer Verbindung mit einer Anwendung für benutzerdefinierte Befehle mit dem Speech SDK (Vorschau)](./quickstart-custom-speech-commands-speech-sdk.md) haben Sie eine Speech SDK-Clientanwendung erstellt, die Befehle wie `turn on the tv`, `turn off the fan` verarbeitet. Fügen Sie jetzt einige visuelle Elemente hinzu, damit Sie das Ergebnis dieser Befehle erkennen können.

Fügen Sie beschriftete Kästchen mit Text hinzu, der **Ein** (On) oder **Aus** (Off) anzeigt, wobei Sie den folgenden XML-Code verwenden, der zu `MainPage.xaml.cs` hinzugefügt wurde.

```xml
<StackPanel Orientation="Horizontal" HorizontalAlignment="Center" Margin="20">
    <Grid x:Name="Grid_TV" Margin="50, 0" Width="100" Height="100" Background="LightBlue">
        <StackPanel>
            <TextBlock Text="TV" Margin="0, 10" TextAlignment="Center"/>
            <TextBlock x:Name="State_TV" Text="Off" TextAlignment="Center"/>
        </StackPanel>
    </Grid>
    <Grid x:Name="Grid_Fan" Margin="50, 0" Width="100" Height="100" Background="LightBlue">
        <StackPanel>
            <TextBlock Text="Fan" Margin="0, 10" TextAlignment="Center"/>
            <TextBlock x:Name="State_Fan" Text="Off" TextAlignment="Center"/>
        </StackPanel>
    </Grid>
</StackPanel>
```

## <a name="handle-customizable-payload"></a>Verarbeiten von anpassbaren Nutzlasten

Nachdem Sie jetzt eine JSON-Nutzlast erstellt haben, können Sie einen Verweis auf die Bibliothek [JSON.NET](https://www.newtonsoft.com/json) hinzufügen, um die Deserialisierung zu behandeln.

> [!div class="mx-imgBorder"]
> ![Nutzlast zum Senden von Aktivitäten](media/custom-speech-commands/fulfill-sdk-json-nuget.png)

Fügen Sie unter `InitializeDialogServiceConnector` Folgendes zu Ihrem `ActivityReceived`-Ereignishandler hinzu. Der zusätzliche Code extrahiert die Nutzlast aus der Aktivität und ändert den visuellen Zustand des Fernsehers oder Lüfters entsprechend.

```C#
connector.ActivityReceived += async (sender, activityReceivedEventArgs) =>
{
    NotifyUser($"Activity received, hasAudio={activityReceivedEventArgs.HasAudio} activity={activityReceivedEventArgs.Activity}");

    dynamic activity = JsonConvert.DeserializeObject(activityReceivedEventArgs.Activity);

    if(activity?.name == "SetDeviceState")
    {
        var state = activity?.state;
        var device = activity?.device;
        switch(device)
        {
            case "tv":
                State_TV.Text = state;
                break;
            case "fan":
                State_Fan.Text = state;
                break;
            default:
                NotifyUser($"Received request to set unsupported device {device} to {state}");
                break;
        }
    }

    if (activityReceivedEventArgs.HasAudio)
    {
        SynchronouslyPlayActivityAudio(activityReceivedEventArgs.Audio);
    }
};
```

## <a name="try-it-out"></a>Ausprobieren

1. Starten der Anwendung
1. Wählen Sie „Mikrofon aktivieren“ aus.
1. Wählen Sie die Schaltfläche „Sprechen“ aus.
1. Sagen Sie: `turn on the tv`.
1. Der visuelle Zustand des Fernsehers sollte zu „Ein“ (On) wechseln.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Vorgehensweise: Hinzufügen von Validierungen zu Parametern benutzerdefinierter Befehle (Vorschau)](./how-to-custom-speech-commands-validations.md)
