---
title: Senden der Aktivität „Benutzerdefinierte Befehle“ an Clientanwendung
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erfahren Sie, wie Sie Aktivitäten von einer Anwendung für benutzerdefinierte Befehle an eine Clientanwendung senden, die das Speech SDK ausführt.
services: cognitive-services
author: xiaojul
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: xiaojul
ms.openlocfilehash: 0a3e3455615006c0e93cf32eebcdaedac9960a79
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2020
ms.locfileid: "85307222"
---
# <a name="send-custom-commands-activity-to-client-application"></a>Senden der Aktivität „Benutzerdefinierte Befehle“ an Clientanwendung

In diesem Artikel erfahren Sie, wie Sie Aktivitäten von einer Anwendung für benutzerdefinierte Befehle an eine Clientanwendung senden, die das Speech SDK ausführt.

Sie führen die folgenden Aufgaben durch:

- Definieren und Senden einer benutzerdefinierten JSON-Nutzlast aus Ihrer Anwendung für benutzerdefinierte Befehle.
- Empfangen und Visualisieren der benutzerdefinierten JSON-Nutzlastinhalte aus einer C# UWP Speech SDK-Clientanwendung.

## <a name="prerequisites"></a>Voraussetzungen
> [!div class = "checklist"]
> * [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)
> * Ein Azure-Abonnementschlüssel für den Spracherkennungsdienst: [Beziehen Sie einen kostenlos](get-started.md), oder erstellen Sie ihn im [Azure-Portal](https://portal.azure.com)
> * Eine zuvor [erstellte Anwendung für benutzerdefinierte Befehle](quickstart-custom-commands-application.md)
> * Eine Speech SDK-fähige Clientanwendung: [Gewusst wie: Integrieren in eine Clientanwendung mithilfe des Speech SDK](./how-to-custom-commands-setup-speech-sdk.md)

## <a name="setup-send-activity-to-client"></a>Einrichten von „Aktivität an Client senden“ 
1. Öffnen Sie die Anwendung für benutzerdefinierte Befehle, die Sie zuvor erstellt haben.
1. Wählen Sie den Befehl **TurnOnOff**, dann **ConfirmationResponse** unter der Vervollständigungsregel und anschließend **Aktion hinzufügen** aus.
1. Wählen Sie unter **Neuer Aktionstyp** die Option **Aktivität an Client senden** aus.
1. Kopieren Sie den JSON-Code unten in **Aktivitätsinhalt**.
   ```json
   {
     "type": "event",
     "name": "UpdateDeviceState",
     "state": "{OnOff}",
     "device": "{SubjectDevice}"
   }
   ```
1. Klicken Sie auf **Speichern**, um eine neue Regel mit einer Aktion „Aktivität senden“ zu erstellen.

   > [!div class="mx-imgBorder"]
   > ![Vervollständigungsregel zum Senden von Aktivitäten](media/custom-commands/send-activity-to-client-completion-rules.png)

## <a name="integrate-with-client-application"></a>Integrieren in Clientanwendung

In [Vorgehensweise Einrichten einer Clientanwendung mit Speech SDK (Vorschau)](./how-to-custom-commands-setup-speech-sdk.md) haben Sie eine UWP-Clientanwendung mit dem Speech SDK erstellt, die Befehle wie `turn on the tv` und `turn off the fan` behandelt hat. Mit einigen hinzugefügten Visuals können Sie das Ergebnis dieser Befehle sehen.

Fügen Sie beschriftete Kästchen mit Text hinzu, der **Ein** (On) oder **Aus** (Off) anzeigt, wobei Sie den folgenden XML-Code verwenden, der zu `MainPage.xaml` hinzugefügt wurde.

```xml
<StackPanel Orientation="Horizontal" HorizontalAlignment="Center" Margin="20">
    <Grid x:Name="Grid_TV" Margin="50, 0" Width="100" Height="100" Background="LightBlue">
        <StackPanel>
            <TextBlock Text="TV" Margin="0, 10" TextAlignment="Center"/>
            <TextBlock x:Name="State_TV" Text="off" TextAlignment="Center"/>
        </StackPanel>
    </Grid>
    <Grid x:Name="Grid_Fan" Margin="50, 0" Width="100" Height="100" Background="LightBlue">
        <StackPanel>
            <TextBlock Text="Fan" Margin="0, 10" TextAlignment="Center"/>
            <TextBlock x:Name="State_Fan" Text="off" TextAlignment="Center"/>
        </StackPanel>
    </Grid>
</StackPanel>
```

### <a name="add-reference-libraries"></a>Hinzufügen von Referenzbibliotheken

Da Sie eine JSON-Nutzlast erstellt haben, müssen Sie einen Verweis auf die Bibliothek [JSON.NET](https://www.newtonsoft.com/json) hinzufügen, um die Deserialisierung zu behandeln.

1. Klicken Sie mit der rechten Maustaste auf Ihre Projektmappe.
1. Wählen Sie **Verwalten von NuGet-Paketen für die Projektmappe** und dann **Installieren** aus. 
1. Suchen Sie in der Liste der Updates nach **Newtonsoft.json**, und aktualisieren Sie **Microsoft.NETCore.UniversalWindowsPlatform** auf die neueste Version.

> [!div class="mx-imgBorder"]
> ![Nutzlast zum Senden von Aktivitäten](media/custom-commands/send-activity-to-client-json-nuget.png)

Fügen Sie in ‚MainPage.xaml.cs‘
- `using Newtonsoft.Json;` 
- `using Windows.ApplicationModel.Core;`

### <a name="handle-the-received-payload"></a>„Handle the received payload“ (Empfangene Nutzlast verarbeiten) hinzu

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
   > ![Nutzlast zum Senden von Aktivitäten](media/custom-commands/send-activity-to-client-turn-on-tv.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Vorgehensweise: Einrichten von Webendpunkten (Vorschau)](./how-to-custom-commands-setup-web-endpoints.md)