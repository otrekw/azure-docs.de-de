---
title: Ausführen von Befehlen auf einem Client mit dem Speech SDK
titleSuffix: Azure Cognitive Services
description: In diesem Artikel wird erläutert, wie Aktivitäten benutzerdefinierter Befehle auf einem Client mit dem Speech SDK verarbeitet werden.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: trbye
ms.openlocfilehash: f11f5f3c2ad4c9f0241d34edeb664f739f88d15c
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82871749"
---
# <a name="fulfill-commands-from-a-client-with-the-speech-sdk-preview"></a>Ausführen von Befehlen auf einem Client mit dem Speech SDK (Vorschau)

Um Aufgaben mit einer Anwendung für benutzerdefinierte Befehle abzuschließen, können Sie benutzerdefinierte Nutzlasten an ein verbundenes Clientgerät senden.

In diesem Artikel führen Sie Folgendes durch:

- Definieren und Senden einer benutzerdefinierten JSON-Nutzlast aus Ihrer Anwendung für benutzerdefinierte Befehle.
- Empfangen und Visualisieren der benutzerdefinierten JSON-Nutzlastinhalte aus einer C# UWP Speech SDK-Clientanwendung.

## <a name="prerequisites"></a>Voraussetzungen
> [!div class = "checklist"]
> * [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)
> * Ein Azure-Abonnementschlüssel für den Spracherkennungsdienst: [Beziehen Sie einen kostenlos](get-started.md), oder erstellen Sie ihn im [Azure-Portal](https://portal.azure.com)
> * Eine zuvor erstellte Anwendung für benutzerdefinierte Befehle: [Schnellstart: Erstellen eines benutzerdefinierten Befehls mit Parametern (Vorschau)](./quickstart-custom-speech-commands-create-parameters.md)
> * Eine Speech SDK-fähige Clientanwendung [Schnellstart: Herstellen einer Verbindung mit einer Anwendung für benutzerdefinierte Befehle mit dem Speech SDK (Vorschau)](./quickstart-custom-speech-commands-speech-sdk.md)

## <a name="optional-get-started-fast"></a>Optional: Schneller Einstieg

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie eine Clientanwendung erstellen, um mit Ihrer Anwendung für benutzerdefinierte Befehle zu sprechen. Wenn Sie lieber sofort loslegen möchten, finden Sie den kompilierbereiten Quellcode, der in diesem Artikel verwendet wird, in den [Speech SDK-Beispielen](https://aka.ms/csspeech/samples).

## <a name="fulfill-with-json-payload"></a>Ausführen mit JSON-Nutzlast

1. Öffnen Sie die Anwendung für benutzerdefinierte Befehle, die Sie zuvor in [Schnellstarts: Erstellen eines benutzerdefinierten Befehls mit Parametern](./quickstart-custom-speech-commands-create-parameters.md) erstellt haben.
1. Überprüfen Sie den Abschnitt **Vervollständigungsregeln**, um sicherzustellen, dass Sie über die zuvor erstellte Regel verfügen, die dem Benutzer antwortet.
1. Um eine Nutzlast direkt an den Client zu senden, erstellen Sie eine neue Regel mit einer Aktion zum Senden von Aktivitäten.

   > [!div class="mx-imgBorder"]
   > ![Vervollständigungsregel zum Senden von Aktivitäten](media/custom-speech-commands/fulfill-sdk-completion-rule.png)

   | Einstellung | Vorgeschlagener Wert | BESCHREIBUNG |
   | ------- | --------------- | ----------- |
   | Regelname | UpdateDeviceState | Ein Name, der den Zweck der Regel beschreibt. |
   | Bedingungen | Erforderlicher Parameter – `OnOff` und `SubjectDevice` | Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann. |
   | Aktionen | `SendActivity` (siehe unten) | Die durchzuführende Aktion, wenn die Bedingung der Regel erfüllt ist (true). |

1. Kopieren Sie den JSON-Code unten in **Aktivitätsinhalt**.
   ```json
   {
     "type": "event",
     "name": "UpdateDeviceState",
     "state": "{OnOff}",
     "device": "{SubjectDevice}"
   }
   ```
   > [!div class="mx-imgBorder"]
   > ![Nutzlast zum Senden von Aktivitäten](media/custom-speech-commands/fulfill-sdk-send-activity-action.png)

## <a name="create-visuals-for-device-on-or-off-state"></a>Erstellen von visuellen Elementen für den ein- oder ausgeschalteten Zustand des Geräts

In [Schnellstart: Herstellen einer Verbindung mit einer Anwendung für benutzerdefinierte Befehle mit dem Speech SDK](./quickstart-custom-speech-commands-speech-sdk.md) haben Sie eine Speech SDK-Clientanwendung erstellt, die Befehle wie `turn on the tv`, `turn off the fan` verarbeitet. Mit einigen hinzugefügten Visuals können Sie das Ergebnis dieser Befehle sehen.

Fügen Sie beschriftete Kästchen mit Text hinzu, der **Ein** (On) oder **Aus** (Off) anzeigt, wobei Sie den folgenden XML-Code verwenden, der zu `MainPage.xaml` hinzugefügt wurde.

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
### <a name="add-reference-libraries"></a>Hinzufügen von Referenzbibliotheken

Da Sie eine JSON-Nutzlast erstellt haben, müssen Sie einen Verweis auf die Bibliothek [JSON.NET](https://www.newtonsoft.com/json) hinzufügen, um die Deserialisierung zu behandeln.
- Klicken Sie mit der rechten Maustaste auf Ihre Projektmappe.
- Wählen Sie **Verwalten von NuGet-Paketen für die Projektmappe** und dann **Installieren** aus. 
- Suchen Sie in der Liste der Updates nach **Newtonsoft.json**, und aktualisieren Sie **Microsoft.NETCore.UniversalWindowsPlatform** auf die neueste Version.

> [!div class="mx-imgBorder"]
> ![Nutzlast zum Senden von Aktivitäten](media/custom-speech-commands/fulfill-sdk-json-nuget.png)

Fügen Sie in ‚MainPage.xaml.cs‘
- `using Newtonsoft.Json;` 
- `using Windows.ApplicationModel.Core;`

### <a name="handle-received-payload"></a>„Handle received payload“ (Empfangene Nutzlast verarbeiten) hinzu

Ersetzen Sie in `InitializeDialogServiceConnector` den `ActivityReceived`-Ereignishandler durch den folgenden Code. Der geänderte `ActivityReceived`-Ereignishandler extrahiert die Nutzlast aus der Aktivität und ändert den visuellen Zustand des Fernsehers oder Lüfters entsprechend.

```C#
connector.ActivityReceived += async (sender, activityReceivedEventArgs) =>
{
    NotifyUser($"Activity received, hasAudio={activityReceivedEventArgs.HasAudio} activity={activityReceivedEventArgs.Activity}");

    dynamic activity = JsonConvert.DeserializeObject(activityReceivedEventArgs.Activity);
    var name = activity?.name != null ? activity.name.ToString() : string.Empty;

    if (name.Equals("UpdateDeviceState"))
    {
        Debug.WriteLine("Here");
        var state = activity?.device != null ? activity.state.ToString() : string.Empty;
        var device = activity?.device != null ? activity.device.ToString() : string.Empty;

        if (state.Equals("on") || state.Equals("off"))
        {
            switch (device)
            {
                case "tv":
                    await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
                        CoreDispatcherPriority.Normal, () => { State_TV.Text = state; });
                    break;
                case "fan":
                    await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
                        CoreDispatcherPriority.Normal, () => { State_Fan.Text = state; });
                    break;
                default:
                    NotifyUser($"Received request to set unsupported device {device} to {state}");
                    break;
            }
        }
        else { 
            NotifyUser($"Received request to set unsupported state {state}");
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
   > [!div class="mx-imgBorder"]
   > ![Nutzlast zum Senden von Aktivitäten](media/custom-speech-commands/fulfill-sdk-turn-on-tv.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Vorgehensweise: Hinzufügen von Validierungen zu Parametern benutzerdefinierter Befehle (Vorschau)](./how-to-custom-speech-commands-validations.md)
