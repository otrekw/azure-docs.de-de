---
author: trrwilson
ms.service: cognitive-services
ms.topic: include
ms.date: 05/25/2020
ms.author: trrwilson
ms.openlocfilehash: c43194db6d1d629003079db7248223aee52ffa70
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84609629"
---
## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Einrichten Ihrer Entwicklungsumgebung und Erstellen eines leeren Projekts](../../../../quickstarts/setup-platform.md)
> * Stellen Sie sicher, dass Sie Zugriff auf ein Mikrofon für die Audioaufnahme haben.

## <a name="setup-your-environment"></a>Einrichten Ihrer Umgebung

Aktualisieren Sie die go.mod-Datei mit der aktuellsten SDK-Version, indem Sie diese Zeile hinzufügen
```sh
require (
    github.com/Microsoft/cognitive-services-speech-sdk-go v1.12.1
)
```

## <a name="start-with-some-boilerplate-code"></a>Beginnen mit Codebausteinen
1. Ersetzen Sie den Inhalt Ihrer Quelldatei (z. B. `sr-quickstart.go`) durch den Inhalt unten, der Folgendes beinhaltet:

- Die „main“-Paketdefinition
- Das Importieren der erforderlichen Module aus dem Speech SDK
- Variablen zum Speichern der Abonnementinformationen, die später in dieser Schnellstartanleitung ersetzt werden
- Eine einfache Implementierung, bei der das Mikrofon für die Audioeingabe verwendet wird
- Ereignishandler für verschiedene Ereignisse, die während einer Spracherkennung stattfinden

```sh
package recognizer

import (
    "bufio"
    "fmt"
    "os"

    "github.com/Microsoft/cognitive-services-speech-sdk-go/audio"
    "github.com/Microsoft/cognitive-services-speech-sdk-go/speech"
)

func recognizingHandler(event speech.SpeechRecognitionEventArgs) {
    defer event.Close()
    fmt.Println("Recognizing:", event.Result.Text)
}

func recognizedHandler(event speech.SpeechRecognitionEventArgs) {
    defer event.Close()
    fmt.Println("Recognized:", event.Result.Text)
}

func cancelledHandler(event speech.SpeechRecognitionCanceledEventArgs) {
    defer event.Close()
    fmt.Println("Received a cancellation: ", event.ErrorDetails)
}

func main() {
    subscription :=  "YOUR_SUBSCRIPTION_KEY"
    region := "YOUR_SUBSCRIPTIONKEY_REGION"

    audioConfig, err := audio.NewAudioConfigFromDefaultMicrophoneInput()
    if err != nil {
        fmt.Println("Got an error: ", err)
        return
    }
    defer audioConfig.Close()
    config, err := speech.NewSpeechConfigFromSubscription(subscription, region)
    if err != nil {
        fmt.Println("Got an error: ", err)
        return
    }
    defer config.Close()
    speechRecognizer, err := speech.NewSpeechRecognizerFromConfig(config, audioConfig)
    if err != nil {
        fmt.Println("Got an error: ", err)
        return
    }
    defer speechRecognizer.Close()
    speechRecognizer.Recognizing(recognizingHandler)
    speechRecognizer.Recognized(recognizedHandler)
    speechRecognizer.Canceled(cancelledHandler)
    speechRecognizer.StartContinuousRecognitionAsync()
    defer speechRecognizer.StopContinuousRecognitionAsync()
    bufio.NewReader(os.Stdin).ReadBytes('\n')
}
```

Ersetzen Sie die Werte `YOUR_SUBSCRIPTION_KEY` und `YOUR_SUBSCRIPTIONKEY_REGION` durch tatsächliche Werte aus der Speech-Ressource.

- Navigieren Sie zum Azure-Portal, und öffnen Sie die Speech-Ressource.
- Auf der linken Seite unter **Schlüssel** sehen Sie zwei verfügbare Abonnementschlüssel.
    - Ersetzen Sie mit einem davon den Wert `YOUR_SUBSCRIPTION_KEY`.
- Sehen Sie sich auf der linken Seite unter **Übersicht** die Region an, und ordnen Sie sie dem Regionsbezeichner zu.
- Verwenden Sie den Regionsbezeichner als Ersatz für den Wert `YOUR_SUBSCRIPTIONKEY_REGION`, z. B. `"westus"` für **USA, Westen**.

## <a name="code-explanation"></a>Erläuterung zum Code
Für die Erstellung eines Sprachkonfigurationsobjekts sind der Speech-Abonnementschlüssel und die Region erforderlich. Das Konfigurationsobjekt wird zum Instanziieren eines Spracherkennungsobjekts benötigt.

Die Erkennungsinstanz bietet mehrere Spracherkennungsmöglichkeiten. In diesem Beispiel wird Sprache fortlaufend erkannt. Durch diese Funktion wird dem Speech-Dienst mitgeteilt, dass Sie viele Ausdrücke zur Erkennung senden, und wann das Programm beendet wird und die Spracherkennung einstellt. Erzielte Ergebnisse werden vom Code in die Konsole geschrieben.

## <a name="build-and-run"></a>Erstellen und Ausführen
Sie sind jetzt bereit, Ihre App zu erstellen und die Spracherkennung mithilfe des Speech-Diensts zu testen.
1. Erstellen Sie Ihr Projekt, beispielsweise mit **„go build“**
2. Führen Sie das Modul aus, und sprechen Sie einen Ausdruck oder einen Satz in das Mikrofon Ihres Geräts. Ihre Spracheingabe wird an den Spracherkennungsdienst übermittelt und in Text transkribiert, der in der Ausgabe angezeigt wird.


> [!NOTE]
> Das Speech SDK verwendet für die Erkennung standardmäßig amerikanisches Englisch (en-us). Informationen zum Auswählen der Ausgangssprache finden Sie unter [Angeben der Ausgangssprache für die Spracherkennung](../../../../how-to-specify-source-language.md).


## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]
