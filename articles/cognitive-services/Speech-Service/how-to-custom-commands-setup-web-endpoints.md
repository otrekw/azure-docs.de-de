---
title: Einrichten von Webendpunkten
titleSuffix: Azure Cognitive Services
description: Einrichten von Webendpunkten für benutzerdefinierte Befehle
services: cognitive-services
author: xiaojul
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: xiaojul
ms.openlocfilehash: 5f1d5318140dd14c5024e8dd3ad0def0afc7f378
ms.sourcegitcommit: 6f1aa680588f5db41ed7fc78c934452d468ddb84
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2021
ms.locfileid: "107725906"
---
# <a name="set-up-web-endpoints"></a>Einrichten von Webendpunkten

In diesem Artikel erfahren Sie, wie Sie Webendpunkte in einer Anwendung für benutzerdefinierte Befehle einrichten, die es Ihnen ermöglichen, HTTP-Anforderungen von einer Clientanwendung aus zu stellen. Sie führen die folgenden Aufgaben aus:

- Einrichten von Webendpunkten in einer Anwendung für benutzerdefinierte Befehle
- Aufrufen von Webendpunkten in einer Anwendung für benutzerdefinierte Befehle
- Empfangen der Antwort der Webendpunkte
- Integrieren der Antwort der Webendpunkte in eine benutzerdefinierte JSON-Nutzlast, Senden und Visualisieren der Webendpunkte aus einer C# UWP Speech SDK-Clientanwendung

## <a name="prerequisites"></a>Voraussetzungen

> [!div class = "checklist"]
> * [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)
> * Ein Azure-Abonnementschlüssel für den Spracherkennungsdienst: [Beziehen Sie einen kostenlos](overview.md#try-the-speech-service-for-free), oder erstellen Sie ihn im [Azure-Portal](https://portal.azure.com)
> * Eine App für benutzerdefinierte Befehle (siehe [Erstellen eines Sprachassistenten mit Benutzerdefinierten Befehlen](quickstart-custom-commands-application.md))
> * Eine Speech SDK-fähige Clientanwendung (siehe[ Integrieren in eine Clientanwendung mithilfe des Speech SDK](how-to-custom-commands-setup-speech-sdk.md))

## <a name="deploy-an-external-web-endpoint-using-azure-function-app"></a>Bereitstellen eines externen Webendpunkts mithilfe der Azure-Funktions-App

Für dieses Tutorial benötigen Sie einen HTTP-Endpunkt, der Zustände für alle Geräte verwaltet, die Sie im **TurnOnOff**-Befehl Ihrer Benutzerdefinierten Befehle einrichten.

Wenn Sie bereits über einen Web-Endpunkt verfügen, den Sie aufrufen wollen, dann springen Sie zum [nächsten Abschnitt](#setup-web-endpoints-in-custom-commands). Alternativ dazu enthält der nächste Abschnitt Details zu einem standardmäßig gehosteten Web-Endpunkt, den Sie verwenden können, wenn Sie diesen Abschnitt überspringen möchten.

### <a name="input-format-of-azure-function"></a>Eingabeformat der Azure-Funktion

Als Nächstes stellen Sie einen Endpunkt mithilfe von [Azure Functions](../../azure-functions/index.yml) bereit.
Im Folgenden finden Sie das Format eines Benutzerdefinierten Befehls, das an Ihre Azure-Funktion übergeben wird. Verwenden Sie diese Informationen, wenn Sie Ihre Azure-Funktions-App schreiben.

```json
{
  "conversationId": "string",
  "currentCommand": {
    "name": "string",
    "parameters": {
      "SomeParameterName": "string",
      "SomeOtherParameterName": "string"
    }
  },
  "currentGlobalParameters": {
      "SomeGlobalParameterName": "string",
      "SomeOtherGlobalParameterName": "string"
  }
}
```

    
In der folgenden Tabelle sind die wichtigsten Attribute dieser Eingabe beschrieben:
        
| attribute | Erklärung |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **conversationId** | Der eindeutige Bezeichner der Unterhaltung. Beachten Sie, dass diese ID mit der Client-App generiert werden kann. |
| **currentCommand** | Der aktuell aktive Befehl in der Unterhaltung. |
| **name** | Der Name des Befehls. Das Attribut `parameters` ist eine Zuordnung mit den aktuellen Werten der Parameter. |
| **currentGlobalParameters** | Eine Zuordnung wie `parameters`, aber für globale Parameter verwendet. |


Für die Azure-Funktion **Gerätestatus** sieht ein Benutzerdefinierter Befehl wie folgt aus. Dies wird als **Eingabe** für die Funktions-App fungieren.
    
```json
{
  "conversationId": "someConversationId",
  "currentCommand": {
    "name": "TurnOnOff",
    "parameters": {
      "item": "tv",
      "value": "on"
    }
  }
}
```

### <a name="azure-function-output-for-a-custom-command-app"></a>Azure-Funktionsausgabe für eine App mit benutzerdefiniertem Befehl

Wenn die Ausgabe Ihrer Azure-Funktion von einer App für Benutzerdefinierte Befehle verwendet wird, sollte sie im folgenden Format angezeigt werden. Weitere Informationen erhalten Sie unter [Aktualisieren eines Befehls über einen Web-Endpunkt](./how-to-custom-commands-update-command-from-web-endpoint.md).

```json
{
  "updatedCommand": {
    "name": "SomeCommandName",
    "updatedParameters": {
      "SomeParameterName": "SomeParameterValue"
    },
    "cancel": false
  },
  "updatedGlobalParameters": {
    "SomeGlobalParameterName": "SomeGlobalParameterValue"
  }
}
```

### <a name="azure-function-output-for-a-client-application"></a>Azure Funktionsausgabe für eine Clientanwendung

Wenn die Ausgabe Ihrer Azure-Funktion von einer Clientanwendung verwendet wird, kann die Ausgabe in der von der Clientanwendung benötigten Form verwendet werden.

Für unseren **Gerätestatus**-Endpunkt wird die Ausgabe Ihrer Azure-Funktion von einer Clientanwendung und nicht von der App für Benutzerdefinierte Befehle verwendet. Die Beispielausgabe der Azure-Funktion sollte wie folgt aussehen:
    
```json
{
  "TV": "on",
  "Fan": "off"
}
``` 

Diese Ausgabe sollte in einen externen Speicher geschrieben werden, damit Sie den Zustand der Geräte verwalten können. Der externe Speicherstatus wird unten im Abschnitt [Integration in die Clientanwendung](#integrate-with-client-application) verwendet.


### <a name="deploy-azure-function"></a>Bereitstellen der Azure-Funktion

Wir stellen ein Beispiel zur Verfügung, das Sie konfigurieren und als Azure-Funktionen bereitstellen können. Führen Sie diese Schritte aus, um ein Konto für unser Beispiel zu erstellen.
 
1. Erstellen Sie Tabellenspeicher, um den Gerätestatus zu speichern. Erstellen Sie im Azure-Portal eine neue Ressource vom Typ **Speicherkonto** anhand des Namens **devicestate**.
1. Kopieren Sie den Wert der **Verbindungszeichenfolge** aus **devicestate -> Zugriffsschlüssel**. Sie müssen dieses Zeichenfolgengeheimnis dem heruntergeladenen Funktions-App-Beispielcode hinzufügen.
1. Laden Sie den [Funktions-App-Beispielcode](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant/tree/main/custom-commands/quick-start) herunter.
1. Öffnen Sie die heruntergeladene Lösung in Visual Studio 2019. Ersetzen Sie in **Connections.js** **STORAGE_ACCOUNT_SECRET_CONNECTION_STRING** durch das Geheimnis aus Schritt 2.
1.  Laden Sie den **DeviceStateAzureFunction**-Code herunter.

Führen Sie die folgenden Schritte aus, um die Beispiel-App für Azure Functions bereitzustellen.

1. [Bereitstellung](../../azure-functions/index.yml) der Azure Functions-App.
1. Warten Sie, bis die Bereitstellung erfolgreich war, und wechseln Sie zur bereitgestellten Ressource im Azure-Portal. 
1. Wählen Sie im linken Bereich **Funktionen** und dann **Gerätestatus** aus.
1.  Wählen Sie im neuen Fenster **Code + Test** und dann **Funktions-URL abrufen** aus.
 
## <a name="setup-web-endpoints-in-custom-commands"></a>Einrichten von Web-Endpunkten für benutzerdefinierte Befehle

Jetzt wollen wir die Azure-Funktion mit der vorhandenen Anwendung „Benutzerdefinierte Befehle“ verbinden.
In diesem Abschnitt verwenden Sie einen vorhandenen **Gerätestatus**-Standardendpunkt. Wenn Sie Ihren eigenen Web-Endpunkt mithilfe von Azure Function oder auf andere Weise erstellt haben, verwenden Sie diesen anstelle des Standard- `https://webendpointexample.azurewebsites.net/api/DeviceState`.

1. Öffnen Sie die Anwendung für benutzerdefinierte Befehle, die Sie zuvor erstellt haben.
1. Wechseln Sie zu **Web-Endpunkte**, und klicken Sie auf **Neuer Web-Endpunkt**.

   > [!div class="mx-imgBorder"]
   > ![Neuer Webendpunkt](media/custom-commands/setup-web-endpoint-new-endpoint.png)

   | Einstellung | Vorgeschlagener Wert | Beschreibung |
   | ------- | --------------- | ----------- |
   | Name | UpdateDeviceState | Der Name für den Webendpunkt. |
   | URL | https://webendpointexample.azurewebsites.net/api/DeviceState | Die URL des Endpunkts, mit dem Ihre App für benutzerdefinierte Befehle kommunizieren soll. |
   | Methode | POST | Die zulässigen Interaktionen (z. B. GET, POST) mit dem Endpunkt.|
   | Header | Schlüssel: app, Wert: Nehmen Sie die ersten 8 Stellen Ihrer applicationId | Die Headerparameter, die in den Anforderungsheader einbezogen werden sollen.|

    > [!NOTE]
    > - Der mit [Azure Functions](../../azure-functions/index.yml) erstellte Webendpunkt, der sich mit der Datenbank verbindet, die den Gerätezustand von Fernseher und Lüfter speichert.
    > - Der vorgeschlagene Header wird nur für den Beispielendpunkt benötigt.
    > - Um sicherzustellen, dass der Wert des Headers an Ihrem Beispielendpunkt eindeutig ist, nehmen Sie die ersten 8 Stellen Ihrer **Anwendungs-ID**.
    > - In der Praxis kann der Web-Endpunkt der Endpunkt für den [IoT-Hub](../../iot-hub/about-iot-hub.md) sein, der Ihre Geräte verwaltet.

1. Klicken Sie auf **Speichern**.

## <a name="call-web-endpoints"></a>Aufrufen von Webendpunkten

1. Wechseln Sie zum Befehl **TurnOnOff**, wählen Sie **ConfirmationResponse** unter der Vervollständigungsregel und dann **Aktion hinzufügen** aus.
1. Wählen Sie unter **Neuer Aktionstyp** die Option **Webendpunkt aufrufen** aus.
1. Wählen Sie unter **Aktion bearbeiten – Endpunkte** die Option **UpdateDeviceState** aus, die den von uns erstellten Webendpunkt darstellt.  
1. Fügen Sie in **Konfiguration** die folgenden Werte ein:
   > [!div class="mx-imgBorder"]
   > ![Aktionsparameter für Webendpunkte aufrufen](media/custom-commands/setup-web-endpoint-edit-action-parameters.png)

   | Einstellung | Vorgeschlagener Wert | BESCHREIBUNG |
   | ------- | --------------- | ----------- |
   | Endpunkte | UpdateDeviceState | Der Webendpunkt, den Sie in dieser Aktion aufrufen möchten. |
   | Abfrageparameter | item={SubjectDevice}&&value={OnOff} | Die Abfrageparameter, die an die URL des Webendpunkts angefügt werden sollen.  |
   | Textinhalt | – | Der Textinhalt der Anforderung. |

    > [!NOTE]
    > - Die vorgeschlagenen Abfrageparameter werden nur für den Beispielendpunkt benötigt.

1. Wählen Sie in **Bei Erfolg – Auszuführende Aktion** die Option **Sprachantwort senden** aus.

    Geben Sie im **einfachen Editor** `{SubjectDevice} is {OnOff}` ein.

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Bildschirm „Bei Erfolg – Auszuführende Aktion“](media/custom-commands/setup-web-endpoint-edit-action-on-success-send-response.png)

   | Einstellung | Vorgeschlagener Wert | BESCHREIBUNG |
   | ------- | --------------- | ----------- |
   | Auszuführende Aktion | Sprachantwort senden | Auszuführende Aktion, wenn die Anforderung an den Webendpunkt erfolgreich ist. |

   > [!NOTE]
   > - Sie können auch direkt auf die Felder in der HTTP-Antwort zugreifen, indem Sie `{YourWebEndpointName.FieldName}` verwenden. Beispiel: `{UpdateDeviceState.TV}`

1. Wählen Sie in **Bei Fehler – Auszuführende Aktion** die Option **Sprachantwort senden** aus.

    Geben Sie im **einfachen Editor** `Sorry, {WebEndpointErrorMessage}`ein.

   > [!div class="mx-imgBorder"]
   > ![Aktion für Webendpunkte bei Fehler aufrufen](media/custom-commands/setup-web-endpoint-edit-action-on-fail.png)

   | Einstellung | Vorgeschlagener Wert | BESCHREIBUNG |
   | ------- | --------------- | ----------- |
   | Auszuführende Aktion | Sprachantwort senden | Auszuführende Aktion, wenn bei der Anforderung an den Webendpunkt ein Fehler auftritt. |

   > [!NOTE]
   > - `{WebEndpointErrorMessage}` ist optional. Wenn Sie keine Fehlermeldung verfügbar machen möchten, können Sie sie entfernen.
   > - Innerhalb unseres Beispielendpunkts senden wir eine HTTP-Antwort mit detaillierten Fehlermeldungen für häufige Fehler wie fehlende Headerparameter zurück.

### <a name="try-it-out-in-test-portal"></a>Probieren Sie es im Testportal aus
- Speichern, trainieren und testen Sie unter Antwort bei Erfolg.
   > [!div class="mx-imgBorder"]
   > ![Screenshot: Antwort bei Erfolg](media/custom-commands/setup-web-endpoint-on-success-response.png)
- Entfernen Sie bei Antwort bei Fehler einen der Abfrageparameter, und führen Sie Speicherung, erneutes Training und Test durch.
   > [!div class="mx-imgBorder"]
   > ![Aktion für Webendpunkte bei Erfolg aufrufen](media/custom-commands/setup-web-endpoint-on-fail-response.png)

## <a name="integrate-with-client-application"></a>Integrieren in Clientanwendung

Bei [Aktivität an Clientanwendung senden](./how-to-custom-commands-send-activity-to-client.md) haben Sie die Aktion **Aktivtät an Client senden** hinzugefügt. Die Aktivität wird an die Clientanwendung gesendet, unabhängig davon, ob die Aktion **Webendpunkt aufrufen** erfolgreich durchgeführt wurde.
Typischerweise möchten Sie jedoch nur dann Aktivitäten an die Clientanwendung senden, wenn der Aufruf des Webendpunkts erfolgreich ist. In diesem Beispiel ist dies der Fall, wenn der Status des Geräts erfolgreich aktualisiert wurde.

1. Löschen Sie die Aktion **Aktivität an Client senden**, die Sie zuvor hinzugefügt haben.
1. Aufruf des Webendpunkts bearbeiten:
    1. Stellen Sie in **Konfiguration** sicher, dass **Abfrageparameter** auf `item={SubjectDevice}&&value={OnOff}` festgelegt ist.
    1. In **Bei Erfolg** ändern Sie **Auszuführende Aktion** in **Aktivität an Client senden**.
    1. Kopieren Sie den nachfolgenden JSON-Code in den **Aktivitätsinhalt**.
   ```json
   {
      "type": "event",
      "name": "UpdateDeviceState",
      "value": {
        "state": "{OnOff}",
        "device": "{SubjectDevice}"
      }
    }
   ```
Jetzt werden nur Aktivitäten an den Client gesendet, wenn die Anforderung an den Web-Endpunkt erfolgreich ist.

### <a name="create-visuals-for-syncing-device-state"></a>Erstellen von visuellen Elementen zum Synchronisieren des Gerätestatus

Fügen Sie die folgende XML zur `MainPage.xaml` über dem **EnableMicrophoneButton**-Block hinzu.

```xml
<Button x:Name="SyncDeviceStateButton" Content="Sync Device State"
        Margin="0,10,10,0" Click="SyncDeviceState_ButtonClicked"
        Height="35"/>
<Button x:Name="EnableMicrophoneButton" ......
        .........../>
```

### <a name="sync-device-state"></a>Zustand des Synchronisierungsgeräts

Fügen Sie in `MainPage.xaml.cs` den Verweis `using Windows.Web.Http;` hinzu. Fügen Sie der `MainPage` -Klasse den folgenden Code hinzu. Diese Methode sendet eine GET-Anforderung an den Beispielendpunkt und extrahiert den aktuellen Gerätestatus für Ihre App. Stellen Sie sicher, dass Sie `<your_app_name>` in das ändern, was Sie im **Header** im Web-Endpunkt für benutzerdefinierte Befehle verwendet haben.

```C#
private async void SyncDeviceState_ButtonClicked(object sender, RoutedEventArgs e)
{
    //Create an HTTP client object
    var httpClient = new HttpClient();

    //Add a user-agent header to the GET request.
    var your_app_name = "<your-app-name>";

    Uri endpoint = new Uri("https://webendpointexample.azurewebsites.net/api/DeviceState");
    var requestMessage = new HttpRequestMessage(HttpMethod.Get, endpoint);
    requestMessage.Headers.Add("app", $"{your_app_name}");

    try
    {
        //Send the GET request
        var httpResponse = await httpClient.SendRequestAsync(requestMessage);
        httpResponse.EnsureSuccessStatusCode();
        var httpResponseBody = await httpResponse.Content.ReadAsStringAsync();
        dynamic deviceState = JsonConvert.DeserializeObject(httpResponseBody);
        var TVState = deviceState.TV.ToString();
        var FanState = deviceState.Fan.ToString();
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
            CoreDispatcherPriority.Normal,
            () =>
            {
                State_TV.Text = TVState;
                State_Fan.Text = FanState;
            });
    }
    catch (Exception ex)
    {
        NotifyUser(
            $"Unable to sync device status: {ex.Message}");
    }
}
```

## <a name="try-it-out"></a>Ausprobieren

1. Starten Sie die Anwendung.
1. Klicken Sie auf „Gerätestatus synchronisieren“.\
Wenn Sie die App im vorigen Abschnitt mit `turn on tv` ausprobiert haben, würden die TV-Sendungen als **Ein** (On) angezeigt werden.
    > [!div class="mx-imgBorder"]
    > ![Zustand des Synchronisierungsgeräts](media/custom-commands/setup-web-endpoint-sync-device-state.png)
1. Wählen Sie **Mikrofon aktivieren** aus.
1. Wählen Sie die Schaltfläche **Sprechen** aus.
1. Sagen Sie `turn on the fan`. Der visuelle Zustand des Lüfters sollte zu **Ein** (On) wechseln.
    > [!div class="mx-imgBorder"]
    > ![Lüfter aktivieren](media/custom-commands/setup-web-endpoint-turn-on-fan.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Exportieren einer Benutzerdefinierte Befehle-Anwendung als Remoteskill](./how-to-custom-commands-integrate-remote-skills.md)
