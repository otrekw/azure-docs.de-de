---
title: 'Tutorial: Sprachaktivierung für Ihren Bot mithilfe des Speech SDK – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In diesem Tutorial erstellen Sie einen Echobot mithilfe von Microsoft Bot Framework, stellen ihn in Azure bereit und registrieren ihn beim Bot Framework-Direct Line Speech-Kanal. Anschließend konfigurieren Sie eine Beispiel-Client-App für Windows, über die Sie mit dem Bot sprechen und die Antwort hören können.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: trbye
ms.openlocfilehash: b2c119f6552773bce7bb93a503c22324278ac0bc
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399474"
---
# <a name="tutorial-voice-enable-your-bot-using-the-speech-sdk"></a>Tutorial: Sprachaktivierung für Ihren Bot mithilfe des Speech SDK

Sie können nun die Möglichkeiten des Speech-Diensts nutzen, um einen Chatbot auf einfache Weise für Sprache zu aktivieren.

In diesem Tutorial erstellen Sie einen Echobot mithilfe von Microsoft Bot Framework, stellen ihn in Azure bereit und registrieren ihn beim Bot Framework-Direct Line Speech-Kanal. Anschließend konfigurieren Sie eine Beispiel-Client-App für Windows, über die Sie mit dem Bot sprechen und die Antwort hören können.

Dieses Tutorial richtet sich an Entwickler, die gerade mit Azure, Bot Framework-Bots, Direct Line Speech oder dem Speech SDK beginnen und rasch ein funktionierendes System mit eingeschränkter Codierung erstellen möchten. Es bedarf keinerlei Erfahrung oder Vertrautheit mit diesen Diensten.

Am Ende dieser Übung haben Sie ein System eingerichtet, das folgendermaßen funktioniert:

1. Die Beispielclientanwendung ist so konfiguriert, dass eine Verbindung mit dem Direct Line Speech-Kanal und dem Echobot hergestellt wird.
1. Die Audioaufzeichnung erfolgt über das Standardmikrofon durch Klicken auf die entsprechende Schaltfläche (oder kontinuierlich, wenn ein benutzerdefiniertes Schlüsselwort aktiviert ist).
1. Optional erfolgt die Erkennung des benutzerdefinierten Schlüsselworts und die Übertragung des Audiostreamings in die Cloud.
1. Die App stellt mithilfe des Speech SDK eine Verbindung mit dem Direct Line Speech-Kanal her und streamt Audioinhalte.
1. Optional wird die Überprüfung des Schlüsselworts mit höherer Genauigkeit für den Dienst durchgeführt.
1. Die Audioinhalte werden an den Spracherkennungsdienst übermittelt und in Text umgewandelt.
1. Der erkannte Text wird als Bot Framework-Aktivität an den Echobot übermittelt. 
1. Der Antworttext wird vom Text-to-Speech-Dienst (TTS) in Audioinhalte umgewandelt und zur Wiedergabe zurück an die Clientanwendung gestreamt.

![Diagramm-Tag](media/tutorial-voice-enable-your-bot-speech-sdk/diagram.png "Flow des Speech-Kanals")

> [!NOTE]
> Für die in diesem Tutorial beschriebenen Schritte ist kein kostenpflichtiger Dienst erforderlich. Als neuer Azure-Benutzer können Sie Guthaben aus Ihrem kostenlosen Azure-Testabonnement und den Free-Tarif des Speech-Diensts für dieses Tutorial verwenden.

Hier ist angegeben, was in diesem Tutorial vermittelt wird:
> [!div class="checklist"]
> * Erstellen neuer Azure-Ressourcen
> * Erstellen, Testen und Bereitstellen des Echobotbeispiels in einer Azure App Service-Instanz
> * Registrieren des Bots beim Direct Line Speech-Kanal
> * Erstellen und Ausführen des Windows-Sprachassistent-Clients für die Interaktion mit dem Echobot
> * Hinzufügen der Aktivierung für benutzerdefinierte Schlüsselwörter
> * Ändern der Sprache der erkannten und gesprochenen Sprache

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

- Windows 10-PC mit einem funktionierenden Mikrofon und Lautsprechern (oder Kopfhörer)
- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) oder höher
- [.NET Framework Runtime 4.6.1](https://dotnet.microsoft.com/download) oder höher
- Ein Azure-Konto. [Registrieren Sie sich kostenlos](https://azure.microsoft.com/free/ai/).
- Ein [GitHub](https://github.com/) -Konto
- [Git für Windows](https://git-scm.com/download/win)

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

In der Client-App, die Sie in diesem Tutorial erstellen, werden einige Azure-Dienste verwendet. Um die Roundtripzeit für Antworten vom Bot zu verkürzen, sollten Sie sicherstellen, dass sich diese Dienste in derselben Azure-Region befinden. In diesem Abschnitt erstellen Sie eine Ressourcengruppe in der Region **USA, Westen**. Diese Ressourcengruppe wird beim Erstellen einzelner Ressourcen für das Bot-Framework, den Direct Line Speech-Kanal und den Speech-Dienst verwendet.

1. <a href="https://ms.portal.azure.com/#create/Microsoft.ResourceGroup" target="_blank">Erstellen einer Ressourcengruppe <span class="docon docon-navigate-external x-hidden-focus"></span></a>
1. Sie werden aufgefordert, einige Informationen anzugeben:
   * Legen Sie **Abonnement** auf **Kostenlose Testversion** fest (Sie können auch ein vorhandenes Abonnement verwenden).
   * Geben Sie einen Namen für die **Ressourcengruppe** ein. Wir empfehlen den Namen **SpeechEchoBotTutorial-ResourceGroup**.
   * Wählen Sie in der Dropdownliste **Region** den Eintrag **USA, Westen** aus.
1. Klicken Sie auf **Überprüfen und erstellen**. Es sollte das Banner **Überprüfung erfolgreich** angezeigt werden.
1. Klicken Sie auf **Erstellen**. Die Erstellung der Ressourcengruppe kann einige Minuten dauern.
1. Wie bei den Ressourcen, die Sie später in diesem Tutorial erstellen, empfiehlt es sich, diese Ressourcengruppe für den einfachen Zugriff an Ihr Dashboard anzuheften. Wenn Sie die Ressourcengruppe anheften möchten, klicken Sie auf das Stecknadelsymbol rechts oben im Dashboard.

### <a name="choosing-an-azure-region"></a>Auswählen einer Azure-Region

Wenn Sie für dieses Tutorial eine andere Region verwenden möchten, kann Ihre Wahl durch folgende Faktoren eingeschränkt werden:

* Stellen Sie sicher, dass Sie eine [unterstützte Azure-Region](regions.md#voice-assistants) verwenden.
* Im Direct Line Speech-Kanal wird der Text-to-Speech-Dienst verwendet, der Standard- und neuronale Stimmen enthält. Neuronale Stimmen sind [auf spezifische Azure-Regionen beschränkt](regions.md#standard-and-neural-voices).
* Kostenlose Testschlüssel können auf eine bestimmte Region beschränkt sein.

Weitere Informationen zu Regionen finden Sie unter [Azure-Standorte](https://azure.microsoft.com/global-infrastructure/locations/).

## <a name="create-resources"></a>Erstellen von Ressourcen

Nachdem Sie eine Ressourcengruppe in einer unterstützten Region erstellt haben, werden im nächsten Schritt einzelne Ressourcen für die in diesem Tutorial verwendeten Dienste erstellt.

### <a name="create-a-speech-service-resource"></a>Erstellen einer Speech-Dienstressource

Gehen Sie wie folgt vor, um eine Speech-Ressource zu erstellen:

1. <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Erstellen einer Speech-Dienstressource <span class="docon docon-navigate-external x-hidden-focus"></span></a>
4. Sie werden aufgefordert, einige Informationen anzugeben:
   * Geben Sie einen **Namen** für die Ressource an. Wir empfehlen den Namen **SpeechEchoBotTutorial-Speech**.
   * Vergewissern Sie sich, dass unter **Abonnement** die Option **Kostenlose Testversion** ausgewählt ist.
   * Wählen Sie unter **Standort** die Option **USA, Westen** aus.
   * Wählen Sie unter **Tarif** die Option **F0** aus. Dies ist der Free-Tarif.
   * Wählen Sie unter **Ressourcengruppe** die Gruppe **SpeechEchoBotTutorial-ResourceGroup** aus.
5. Klicken Sie nach der Eingabe aller erforderlichen Informationen auf **Erstellen**. Die Erstellung der Ressource kann einige Minuten dauern.
6. Später in diesem Tutorial benötigen Sie Abonnementschlüssel für diesen Dienst. Auf diese Schlüssel können Sie jederzeit über **Übersicht** (Schlüssel verwalten) oder über **Schlüssel** für Ihre Ressource zugreifen.

Überprüfen Sie hier, ob die Ressourcengruppe (**SpeechEchoBotTutorial-ResourceGroup**) eine Speech-Ressource enthält:

| Name | type  | Standort |
|------|-------|----------|
| SpeechEchoBotTutorial-Speech | Cognitive Services | USA (Westen) |

### <a name="create-an-azure-app-service-plan"></a>Erstellen eines Azure App Service-Plans

Im nächsten Schritt wird ein App Service-Plan erstellt. Mit einem App Service-Plan wird ein Satz mit Computeressourcen für eine auszuführende Web-App definiert.

1. <a href="https://ms.portal.azure.com/#create/Microsoft.AppServicePlanCreate" target="_blank">Erstellen eines Azure App Service-Plans <span class="docon docon-navigate-external x-hidden-focus"></span></a>
4. Sie werden aufgefordert, einige Informationen anzugeben:
   * Legen Sie **Abonnement** auf **Kostenlose Testversion** fest (Sie können auch ein vorhandenes Abonnement verwenden).
   * Wählen Sie unter **Ressourcengruppe** die Gruppe **SpeechEchoBotTutorial-ResourceGroup** aus.
   * Geben Sie einen **Namen** für die Ressource an. Wir empfehlen den Namen **SpeechEchoBotTutorial-AppServicePlan**.
   * Wählen Sie unter **Betriebssystem** die Option **Windows** aus.
   * Wählen Sie unter **Region** die Option **USA, Westen** aus.
   * Vergewissern Sie sich, dass unter **Tarif** die Option **Standard S1** ausgewählt ist. Dies sollte der Standardwert sein. Wenn dies nicht der Fall ist, stellen Sie sicher, dass Sie das **Betriebssystem** wie oben beschrieben auf **Windows** festlegen.
5. Klicken Sie auf **Überprüfen und erstellen**. Es sollte das Banner **Überprüfung erfolgreich** angezeigt werden.
6. Klicken Sie auf **Erstellen**. Die Erstellung der Ressourcengruppe kann einige Minuten dauern.

Überprüfen Sie hier, ob die Ressourcengruppe (**SpeechEchoBotTutorial-ResourceGroup**) zwei Ressourcen enthält:

| Name | Typ  | Standort |
|------|-------|----------|
| SpeechEchoBotTutorial-AppServicePlan | App Service-Plan | USA (Westen) |
| SpeechEchoBotTutorial-Speech | Cognitive Services | USA (Westen) |

## <a name="build-an-echo-bot"></a>Erstellen eines Echobots

Nachdem Sie einige Ressourcen erstellt haben, erstellen Sie nun einen Bot. Wir beginnen mit dem Echobotbeispiel, das, wie der Name impliziert, den eingegebenen Text als Antwort zurückgibt. Keine Sorge, Sie können den Beispielcode ohne Änderungen verwenden. Der Code ist für die Verwendung mit dem Direct Line Speech-Kanal konfiguriert, der verbunden wird, nachdem der Bot in Azure bereitgestellt wurde.

> [!NOTE]
> Die folgenden Anweisungen sowie weitere Informationen zum Echobot sind in der [Infodatei des Beispiels auf GitHub](https://github.com/microsoft/BotBuilder-Samples/blob/master/samples/csharp_dotnetcore/02.echo-bot/README.md) verfügbar.

### <a name="run-the-bot-sample-on-your-machine"></a>Führen Sie das Botbeispiel auf Ihrem Computer aus.

1. Klonen Sie das Beispielrepository:

   ```bash
   git clone https://github.com/Microsoft/botbuilder-samples.git
   ```

2. Starten Sie Visual Studio.
3. Wählen Sie auf der Symbolleiste **Datei** > **Öffnen** > **Projekt/Projektmappe** aus, und öffnen Sie die Echobot-Projektlösung:

   ```
   samples\csharp_dotnetcore\02.echo-bot\EchoBot.sln
   ```

4. Drücken Sie nach dem Laden des Projekts die Taste <kbd>F5</kbd>, um das Projekt zu erstellen und auszuführen.
5. Es sollte ein Browser mit einem ähnlichen Bildschirm wie diesem gestartet werden.
    > [!div class="mx-imgBorder"]
    > [![echobot-running-on-localhost](media/tutorial-voice-enable-your-bot-speech-sdk/echobot-running-on-localhost.png "Ausführen von EchoBot auf Localhost")](media/tutorial-voice-enable-your-bot-speech-sdk/echobot-running-on-localhost.png#lightbox)

### <a name="test-the-bot-sample-with-the-bot-framework-emulator"></a>Testen des Botbeispiels mit dem Bot Framework Emulator

[Bot Framework Emulator](https://github.com/microsoft/botframework-emulator) ist eine Desktopanwendung, mit der Botentwickler ihre Bots lokal oder remote über einen Tunnel testen und debuggen können. Der Emulator unterstützt eingegebenen Text als Eingabe (nicht per Stimme). Der Bot antwortet in Textform. Führen Sie diese Schritte aus, um Ihren lokal ausgeführten Echobot mit dem Bot Framework Emulator mit Texteingabe und Textausgabe zu testen. Nachdem wir den Bot in Azure bereitgestellt haben, werden wir ihn mit Spracheingabe und Sprachausgabe testen.

1. Installieren Sie [Bot Framework Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases/latest) Version 4.3.0 oder höher.
2. Starten Sie Bot Framework Emulator, und öffnen Sie Ihren Bot:
   * **File** -> **Open Bot** (Dabei -> Bot öffnen)
3. Geben Sie die URL für den Bot ein. Beispiel:

   ```
   http://localhost:3978/api/messages
   ```
   und drücken Sie auf „Verbinden“.
4. Der Bot sollte Sie sofort mit „Hallo und willkommen“ begrüßen. Meldung. Geben Sie eine beliebige Textnachricht ein und bestätigen Sie, dass Sie eine Antwort vom Bot erhalten.
5. So könnte ein Kommunikationsaustausch mit einer Echobot-Instanz aussehen: [![bot-framework-emulator](media/tutorial-voice-enable-your-bot-speech-sdk/bot-framework-emulator.png "Bot Framework Emulator")](media/tutorial-voice-enable-your-bot-speech-sdk/bot-framework-emulator.png#lightbox)

## <a name="deploy-your-bot-to-an-azure-app-service"></a>Bereitstellen des Bots in einer Azure App Service-Instanz

Im nächsten Schritt wird der Echobot in Azure bereitgestellt. Es gibt mehrere Möglichkeiten, einen Bot bereitzustellen, in diesem Tutorial wird er direkt über Visual Studio veröffentlicht.

> [!NOTE]
> Alternativ können Sie einen Bot mithilfe der [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/bot-service/bot-builder-deploy-az-cli) und über [Bereitstellungsvorlagen](https://github.com/microsoft/BotBuilder-Samples/tree/master/experimental/directline-speech/csharp_dotnetcore/02.echo-bot/DeploymentTemplates) bereitstellen.

1. Öffnen Sie in Visual Studio den Echobot, der zur Verwendung mit dem Direct Line Speech-Kanal konfiguriert ist:

   ```
   samples\csharp_dotnetcore\02.echo-bot\EchoBot.sln
   ```

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **EchoBot**, und wählen Sie **Veröffentlichen** aus.
1. Ein neues Fenster mit dem Titel **Veröffentlichungsziel auswählen** wird geöffnet.
1. Wählen Sie **App Service** aus der Navigation **Azure-Dienste** und dann **Neu erstellen** aus. Anschließend klicken Sie auf **Profil erstellen**.
1. Bei Anzeige des Fensters **App Service erstellen**:
   * Klicken Sie auf **Konto hinzufügen**, und melden Sie sich mit den Anmeldeinformationen für Ihr Azure-Konto an. Wenn Sie bereits angemeldet sind, wählen Sie in der Dropdownliste das gewünschte Konto aus.
   * Für den **App-Namen** müssen Sie einen global eindeutigen Namen für den Bot eingeben. Anhand dieses Namens wird eine eindeutige Bot-URL erstellt. Ein Standardwert, der Datum und Uhrzeit beinhaltet (z. B. „EchoBot20190805125647“), wird automatisch eingetragen. In diesem Tutorial können Sie den Standardnamen verwenden.
   * Legen Sie unter **Abonnement** die Option **Kostenlose Testversion** fest.
   * Wählen Sie unter **Ressourcengruppe** die Gruppe **SpeechEchoBotTutorial-ResourceGroup** aus.
   * Wählen Sie unter **Hostingplan** die Option **SpeechEchoBotTutorial-AppServicePlan** aus.
   * Für **Application Insights** belassen Sie **Kein(e)**
1. Klicken Sie auf **Erstellen**
1. In Visual Studio sollte eine Erfolgsmeldung angezeigt werden, die wie folgt aussieht:

   ```
   Publish Succeeded.
   Web App was published successfully https://EchoBot20190805125647.azurewebsites.net/
   ```

1. Im geöffneten Standardbrowser sollte die folgende Seite angezeigt werden: „Your bot is ready!“ (Ihr Bot wurde erstellt.).
1. Überprüfen Sie hier, ob die Ressourcengruppe **SpeechEchoBotTutorial-ResourceGroup** im Azure-Portal drei Ressourcen enthält:

| Name | Typ  | Standort |
|------|-------|----------|
| EchoBot20190805125647 | App Service | USA (Westen) |
| SpeechEchoBotTutorial-AppServicePlan | App Service-Plan | USA (Westen) |
| SpeechEchoBotTutorial-Speech | Cognitive Services | USA (Westen) |

## <a name="enable-web-sockets"></a>Aktivieren von WebSockets

Sie müssen eine kleine Konfigurationsänderung vornehmen, damit der Bot über WebSockets mit dem Direct Line Speech-Kanal kommunizieren kann. Gehen Sie zum Aktivieren von WebSockets wie folgt vor:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer App Service-Instanz. Die Ressource sollte ähnlich wie **EchoBot20190805125647** benannt sein (der eindeutige App-Name).
2. Klicken Sie im Navigationsbereich **Azure-Dienste** unter **Einstellungen** auf **Konfiguration**.
3. Wählen Sie die Registerkarte **Allgemeine Einstellungen** aus.
4. Suchen Sie die Umschaltfläche **Websockets**, und legen Sie sie auf **Ein** fest.
5. Klicken Sie auf **Speichern**.

> [!TIP]
> Über die Steuerelemente oben auf der Seite der Azure App Service-Instanz können Sie den Dienst beenden oder neu starten. Dies kann bei der Problembehandlung nützlich sein.

## <a name="create-a-channel-registration"></a>Erstellen einer Kanalregistrierung

Nach dem Erstellen einer Azure App Service-Instanz zum Hosten Ihres Bots wird im nächsten Schritt eine **Botkanalregistrierung** erstellt. Das Erstellen einer Kanalregistrierung ist eine Voraussetzung für die Registrierung des Bots bei Bot Framework-Kanälen, einschließlich des Direct Line Speech-Kanals.

> [!NOTE]
> Wenn Sie mehr darüber erfahren möchten, wie Bots Kanäle nutzen, finden Sie entsprechende Informationen unter [Verbinden eines Bots mit Kanälen](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels?view=azure-bot-service-4.0).


1. <a href="https://ms.portal.azure.com/#create/Microsoft.BotServiceConnectivityGalleryPackage" target="_blank">Erstellen einer Azure-Botkanalregistrierung <span class="docon docon-navigate-external x-hidden-focus"></span></a>
2. Sie werden aufgefordert, einige Informationen anzugeben:
   * Geben Sie unter **Bothandle** den Namen **SpeechEchoBotTutorial-BotRegistration** ein.
   * Wählen Sie unter **Abonnement** die Option **Kostenlose Testversion** aus.
   * Wählen Sie unter **Ressourcengruppe** die Gruppe **SpeechEchoBotTutorial-ResourceGroup** aus.
   * Wählen Sie unter **Standort** die Option **USA, Westen** aus.
     * Wählen Sie unter **Tarif** die Option **F0** aus.
     * Geben Sie unter **Messaging endpoint** (Messagingendpunkt) die URL für Ihre Web-App mit dem am Ende angefügten Pfad `/api/messages` ein. Wenn der global eindeutige App-Name beispielsweise **EchoBot20190805125647** ist, lautet der Messagingendpunkt `https://EchoBot20190805125647.azurewebsites.net/api/messages/`.
     * Für **Application Insights** können Sie diese Einstellung auf **Aus** festlegen. Weitere Informationen finden Sie unter [Botanalyse](https://docs.microsoft.com/azure/bot-service/bot-service-manage-analytics?view=azure-bot-service-4.0).
     * Ignorieren Sie **Auto create App ID and password** (App-ID und Kennwort automatisch erstellen).
5. Klicken Sie unten auf dem Blatt **Bot Channels Registration** (Botkanalregistrierung) auf **Erstellen**.

Überprüfen Sie hier die Ressourcengruppe **SpeechEchoBotTutorial-ResourceGroup** im Azure-Portal. Es sollten nun vier Ressourcen angezeigt werden:

| Name | Typ  | Standort |
|------|-------|----------|
| EchoBot20190805125647 | App Service | USA (Westen) |
| SpeechEchoBotTutorial-AppServicePlan | App Service-Plan | USA (Westen) |
| SpeechEchoBotTutorial-BotRegistration | Botkanalregistrierung | Global |
| SpeechEchoBotTutorial-Speech | Cognitive Services | USA (Westen) |

> [!IMPORTANT]
> In der Ressource der Botkanalregistrierung wird die globale Region angezeigt, auch wenn Sie „USA, Westen“ ausgewählt haben. Dies entspricht dem erwarteten Verhalten.

## <a name="register-the-direct-line-speech-channel"></a>Registrieren des Direct Line Speech-Kanals

Nun muss der Bot beim Direct Line Speech-Kanal registriert werden. Dieser Kanal dient zum Erstellen einer Verbindung zwischen dem Echobot und einer Client-App, die mit dem Speech SDK kompiliert wurde.

1. Suchen und öffnen Sie die Ressource **SpeechEchoBotTutorial-BotRegistration** im [Azure-Portal](https://portal.azure.com).
1. Wählen Sie im Navigationsbereich **Azure-Dienste** die Option **Kanäle** aus.
   * Suchen Sie nach **Weitere Kanäle**, und klicken Sie auf **Direct Line Speech**.
   * Überprüfen Sie den Text auf der Seite **Configure Direct line Speech** (Direct Line Speech konfigurieren), und erweitern Sie dann das Dropdownmenü „Cognitive Services-Konto“.
   * Wählen Sie im Menü die zuvor erstellte Speech-Ressource (z. B. **SpeechEchoBotTutorial-Speech**) aus, um Ihren Bot dem Speech-Abonnementschlüssel zuzuordnen.
   * Klicken Sie auf **Speichern**.

1. Klicken Sie im Navigationsbereich **Botverwaltung** auf **Einstellungen**.
   * Aktivieren Sie das Kontrollkästchen **Enable Streaming Endpoint** (Streamingendpunkt aktivieren). Dies ist erforderlich, um zwischen dem Bot und dem Direct Line Speech-Kanal ein Kommunikationsprotokoll auf der Basis von WebSockets zu aktivieren.
   * Klicken Sie auf **Speichern**.

> [!TIP]
> Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit Direct Line Speech für einen Bot](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech?view=azure-bot-service-4.0). Diese Seite enthält zusätzliche Informationen und bekannte Probleme.

## <a name="run-the-windows-voice-assistant-client"></a>Ausführen des Windows-Sprachassistent-Clients

In diesem Schritt führen Sie den Windows-Sprachassistent-Client aus. Der Client ist eine WPF-App (Windows Presentation Foundation) in C#, in der mithilfe des [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk) die Kommunikation mit dem Bot über den Direct Line Speech-Kanal verwaltet wird. Verwenden Sie ihn, um mit dem Bot zu interagieren und ihn zu testen, bevor Sie eine benutzerdefinierte Client-App schreiben. Es handelt sich um einen Open-Source-Client, deshalb können Sie entweder die ausführbare Datei herunterladen und ausführen oder ihn selbst kompilieren.

Der Windows-Sprachassistent-Client verfügt über eine einfache Benutzeroberfläche, über die Sie die Verbindung mit dem Bot konfigurieren und die Textunterhaltung sowie Bot Framework-Aktivitäten im JSON-Format und adaptive Karten anzeigen können. Außerdem wird die Verwendung von benutzerdefinierten Schlüsselwörtern unterstützt. Über diesen Client sprechen Sie mit dem Bot und erhalten eine Sprachantwort.

Stellen Sie sicher, dass das Mikrofon und die Lautsprecher aktiviert sind und funktionieren, bevor Sie fortfahren.

1. Navigieren Sie zum GitHub-Repository für den [Windows-Sprachassistent-Client](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant/blob/master/clients/csharp-wpf/README.md).
1. Befolgen Sie die Anweisungen zum Erstellen, um entweder
   * eine ZIP-Datei mit der ausführbaren Datei herunterzuladen oder
   * die ausführbare Datei selbst zu erstellen, indem Sie das Repository klonen und das Projekt kompilieren.

1. Starten Sie die Clientanwendung, und konfigurieren Sie sie.
1. Klicken Sie auf **Erneut verbinden**, und vergewissern Sie sich, dass die Meldung **Press the mic button, or type to start talking to your bot** (Klicken Sie auf die Schaltfläche für das Mikrofon, oder geben Sie Text ein, um mit dem Bot zu sprechen) angezeigt wird.
1. Testen wir den Bot. Klicken Sie auf die Schaltfläche für das Mikrofon, und sprechen Sie ein paar Worte auf Englisch. Der erkannte Text wird angezeigt, während Sie sprechen. Wenn Sie aufhören zu sprechen, antwortet der Bot mit eigener Stimme und sagt „Echo“, gefolgt von den erkannten Wörtern.
1. Sie können auch über Text mit dem Bot kommunizieren. Geben Sie den Text einfach auf der unteren Leiste ein. 

### <a name="troubleshooting-errors-in-windows-voice-assistant-client"></a>Behandeln von Problemen mit dem Windows-Sprachassistent-Client

Wenn im Hauptfenster der App eine Fehlermeldung angezeigt wird, können Sie den Fehler anhand dieser Tabelle identifizieren und beheben:

| Fehler | Vorgehensweise |
|-------|----------------------|
|Fehler AuthenticationFailure: Authentifizierungsfehler beim WebSocket-Upgrade (401). Überprüfen Sie, ob der Abonnementschlüssel (oder das Autorisierungstoken) und der Regionsname richtig sind.| Stellen Sie auf der Seite „Einstellungen“ der App sicher, dass Sie den Speech-Abonnementschlüssel und die zugehörige Region richtig eingegeben haben.<br>Vergewissern Sie sich, dass Ihr Sprachschlüssel und der Schlüsselbereich ordnungsgemäß eingegeben wurden. |
|Fehler ConnectionFailure: Verbindung wurde vom Remotehost geschlossen. Fehlercode: 1011. Fehlerdetails: Vor dem Senden einer Meldung konnte keine Verbindung mit dem Bot hergestellt werden. | Vergewissern Sie sich, dass Sie das Kontrollkästchen [„Enable Streaming Endpoint“ (Streamingendpunkt aktivieren) aktiviert](#register-the-direct-line-speech-channel) und/oder [**Websockets** auf „Ein“ festlegt](#enable-web-sockets) haben.<br>Stellen Sie sicher, dass Ihre Azure App Service-Instanz ausgeführt wird. Wenn dies der Fall ist, starten Sie die App Service-Instanz neu.|
|Fehler ConnectionFailure: Verbindung wurde vom Remotehost geschlossen. Fehlercode: 1011. Fehlerdetails: Der Antwortstatuscode gibt keinen Erfolg an: 500 (InternalServerError)| Im Bot ist im Feld [speak](https://github.com/microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md#speak) der Ausgabeaktivität eine neuronale Stimme angegeben, in der Azure-Region, die Ihrem Speech-Abonnementschlüssel zugeordnet ist, werden neuronale Stimmen jedoch nicht unterstützt. Siehe [Standard- und neuronale Stimmen](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#standard-and-neural-voices).|
|Fehler ConnectionFailure: Verbindung wurde vom Remotehost geschlossen. Fehlercode: 1.000. Fehlerdetails: Maximale Leerlaufzeit der WebSocket-Verbindung wurde überschritten (> 300.000 ms)| Dies ist ein erwarteter Fehler, wenn eine Verbindung mit dem Kanal länger als fünf Minuten geöffnet und inaktiv bleibt. |

Wenn Ihr Problem in der Tabelle nicht aufgeführt wird, finden Sie weitere Informationen unter [Sprachassistenten: Häufig gestellte Fragen (FAQs)](faq-voice-assistants.md).

### <a name="view-bot-activities"></a>Anzeigen von Botaktivitäten

Jeder Bot sendet und empfängt **Aktivitätsnachrichten**. Im Fenster **Aktivitätsprotokoll** des Windows-Sprachassistent-Clients werden für jede Aktivität, die der Client vom Bot empfängt, mit einem Zeitstempel versehene Protokolle angezeigt. Außerdem werden die Aktivitäten angezeigt, die der Client mithilfe der [`DialogServiceConnector.SendActivityAsync`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconnector.sendactivityasync)-Methode an den Bot gesendet hat. Wenn Sie ein Protokollelement auswählen, werden die Details der zugeordneten Aktivität im JSON-Format angezeigt.

JSON-Beispielcode einer Aktivität, die vom Client empfangen wurde:

```json
{
    "attachments":[],
    "channelData":{
        "conversationalAiData":{
             "requestInfo":{
                 "interactionId":"8d5cb416-73c3-476b-95fd-9358cbfaebfa",
                 "version":"0.2"
             }
         }
    },
    "channelId":"directlinespeech",
    "conversation":{
        "id":"129ebffe-772b-47f0-9812-7c5bfd4aca79",
        "isGroup":false
    },
    "entities":[],
    "from":{
        "id":"SpeechEchoBotTutorial-BotRegistration"
    },
    "id":"89841b4d-46ce-42de-9960-4fe4070c70cc",
    "inputHint":"acceptingInput",
    "recipient":{
        "id":"129ebffe-772b-47f0-9812-7c5bfd4aca79|0000"
    },
    "replyToId":"67c823b4-4c7a-4828-9d6e-0b84fd052869",
    "serviceUrl":"urn:botframework:websocket:directlinespeech",
    "speak":"<speak version='1.0' xmlns='https://www.w3.org/2001/10/synthesis' xml:lang='en-US'><voice name='Microsoft Server Speech Text to Speech Voice (en-US, AriaRUS)'>Echo: Hello and welcome.</voice></speak>",
    "text":"Echo: Hello and welcome.",
    "timestamp":"2019-07-19T20:03:51.1939097Z",
    "type":"message"
}
```

Weitere Informationen zu den in der JSON-Ausgabe zurückgegebenen Daten finden Sie unter [Felder in der Aktivität](https://github.com/microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md). Für dieses Tutorial können Sie sich auf die Felder [text](https://github.com/microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md#text) und [speak](https://github.com/microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md#speak) konzentrieren.

### <a name="view-client-source-code-for-calls-to-the-speech-sdk"></a>Anzeigen des Clientquellcodes für Aufrufe des Speech SDK

Der Windows-Sprachassistent-Client verwendet das NuGet-Paket [Microsoft.CognitiveServices.Speech](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/), das das Speech SDK enthält. Ein guter Ausgangspunkt für die Überprüfung des Beispielcodes ist die InitSpeechConnector()-Methode in der Datei [`VoiceAssistantClient\MainWindow.xaml.cs`](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant/blob/master/clients/csharp-wpf/VoiceAssistantClient/MainWindow.xaml.cs), mit der die folgenden zwei Speech SDK-Objekte erstellt werden:
- [`DialogServiceConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconfig): für Konfigurationseinstellungen (z. B. Speech-Abonnementschlüssel, Schlüsselregion)
- [`DialogServiceConnector`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconnector.-ctor): für die Verwaltung der Kanalverbindungs- und Clientabonnementereignisse zur Verarbeitung von erkannten Sprach- und Botantworten

## <a name="add-custom-keyword-activation"></a>Hinzufügen der Aktivierung für benutzerdefinierte Schlüsselwörter

Das Speech SDK unterstützt die Aktivierung durch benutzerdefinierte Schlüsselwörter. Ähnlich wie „Hey Cortana“ für den Microsoft-Assistenten können Sie eine App schreiben, die kontinuierlich auf ein Schlüsselwort Ihrer Wahl lauscht. Ein Schlüsselwort kann ein einzelnes Wort oder ein Ausdruck mit mehreren Wörtern sein.

> [!NOTE]
> Der Begriff *Schlüsselwort* wird häufig synonym mit dem Begriff *Aktivierungswort* verwendet. Sie werden feststellen, dass beide Begriffe in der Microsoft-Dokumentation verwendet werden.

Die Erkennung des Schlüsselworts erfolgt in der Client-App. Bei Verwendung eines Schlüsselworts werden Audioinhalte nur dann in den Direct Line Speech-Kanal gestreamt, wenn das Schlüsselwort erkannt wird. Der Direct Line Speech-Kanal umfasst die Komponente *Key Word Verification (KWV)* , über die eine komplexere Verarbeitung in der Cloud durchgeführt wird, um zu überprüfen, ob sich das ausgewählte Schlüsselwort am Anfang des Audiostreams befindet. Wenn die Überprüfung des Schlüsselworts erfolgreich durchgeführt wurde, kommuniziert der Kanal mit dem Bot.

Führen Sie die folgenden Schritte aus, um ein Schlüsselwortmodell zu erstellen, den Windows-Sprachassistent-Client zur Verwendung des Modells zu konfigurieren und das Modell schließlich mit Ihrem Bot zu testen.

1. Befolgen Sie die Anweisungen zum [Erstellen eines benutzerdefinierten Schlüsselworts mit dem Spracherkennungsdienst](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-create-kws).
2. Entzippen Sie die Modelldatei, die Sie im vorherigen Schritt heruntergeladen haben. Sie sollte nach Ihrem Schlüsselwort benannt werden. Suchen Sie nach der Datei mit dem Namen `kws.table`.
3. Navigieren Sie im Windows-Sprachassistent-Client zum Menü **Einstellungen** (das Zahnradsymbol oben rechts). Suchen Sie nach dem **Modelldateipfad**, und geben Sie den vollständigen Pfad für die Datei `kws.table` aus Schritt 2 ein.
4. Aktivieren Sie das Kontrollkästchen **Aktiviert**. Neben dem Kontrollkästchen sollte die folgende Meldung angezeigt werden: „Will listen for the wake word upon next connection“ (Lauscht bei der nächsten Verbindung auf das Schlüsselwort). Wenn Sie die falsche Datei oder einen ungültigen Pfad angegeben haben, sollte eine Fehlermeldung angezeigt werden.
5. Geben Sie Ihren Speech-**Abonnementschlüssel** und die **Abonnementschlüsselregion** ein, und klicken Sie dann auf **OK**, um das Menü **Einstellungen** zu schließen.
6. Klicken Sie auf **Erneut verbinden**. Folgende Meldung sollte angezeigt werden: „New conversation started - type, press the microphone button, or say the wake word.“ (Neue Unterhaltung begonnen. Geben Sie Text ein, klicken Sie auf die Mikrofonschaltfläche, oder sagen Sie das Schlüsselwort). Die App lauscht nun kontinuierlich.
7. Sagen Sie einen Ausdruck, der mit dem Schlüsselwort beginnt. Beispiel: „ **{Ihr Schlüsselwort}** , wie spät ist es?“. Sie müssen beim Sprechen keine Pause nach dem Schlüsselwort machen. Folgendes geschieht:
   * Die Transkription des Gesagten wird angezeigt.
   * Kurz darauf hören Sie die Antwort des Bots.
8. Testen Sie die drei Eingabetypen, die für den Bot unterstützt werden:
   * Eingabe von Text in der unteren Leiste
   * Klicken auf das Mikrofonsymbol und Sprechen
   * Sprechen eines Ausdrucks, der mit Ihrem Schlüsselwort beginnt

### <a name="view-the-source-code-that-enables-keyword"></a>Anzeigen des Quellcodes, mit dem das Schlüsselwort aktiviert wird

Sehen Sie sich im Quellcode des Windows-Sprachassistent-Clients in den folgenden Dateien den Code an, mit dem die Erkennung für Schlüsselwörter aktiviert wird:

1. [`VoiceAssistantClient\Models.cs`](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant/blob/master/clients/csharp-wpf/VoiceAssistantClient/Models.cs) enthält einen Aufruf der Speech SDK-Methode [`KeywordRecognitionModel.fromFile()`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/keywordrecognitionmodel?view=azure-node-latest#fromfile-string-), mit der das Modell über eine lokale Datei auf einem Datenträger instanziiert wird.
1. [`VoiceAssistantClient\MainWindow.xaml.cs`](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant/blob/master/clients/csharp-wpf/VoiceAssistantClient/MainWindow.xaml.cs) enthält einen Aufruf der Speech SDK-Methode [`DialogServiceConnector.StartKeywordRecognitionAsync()`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconnector.startkeywordrecognitionasync), mit der die kontinuierliche Erkennung für Schlüsselwörter aktiviert wird.

## <a name="optional-change-the-language-and-bot-voice"></a>(Optional:) Ändern von Sprache und Stimme des Bots

Der Bot, den Sie erstellt haben, lauscht und antwortet in englischer Sprache, mit einer standardmäßigen Sprachausgabe in amerikanischem Englisch. Sie sind jedoch nicht auf die Verwendung von Englisch oder eine Standardstimme beschränkt. In diesem Abschnitt erfahren Sie, wie Sie die Sprache ändern, auf die der Bot lauscht und in der er antwortet. Außerdem lernen Sie, wie Sie eine andere Stimme für diese Sprache auswählen können.

### <a name="change-the-language"></a>Ändern der Sprache

Sie können eine der Sprachen auswählen, die in der Tabelle [Spracherkennung](language-support.md#speech-to-text) erwähnt werden. Im Beispiel unten ändern wir die Sprache in Deutsch.

1. Öffnen Sie die App für den Windows-Sprachassistent-Client, klicken Sie auf die Schaltfläche für Einstellungen (Zahnradsymbol oben rechts), und geben Sie `de-de` in das Feld „Sprache“ ein (dies ist der Wert für das Gebietsschema, der in der Tabelle [Spracherkennung](language-support.md#speech-to-text) aufgeführt ist). Dadurch wird die zu erkennende gesprochene Sprache festgelegt und der Standardwert `en-us` überschrieben. Dadurch wird der Direct Line Speech-Client außerdem angewiesen, für die Bot-Antwort eine deutsche Standardstimme zu verwenden.
2. Schließen Sie die Einstellungsseite, und klicken Sie auf die Schaltfläche „Verbindung wiederherstellen“, um eine neue Verbindung mit Ihrem Echobot herzustellen.
3. Klicken Sie auf die Mikrofonschaltfläche, und sprechen Sie einen Ausdruck auf Deutsch. Sie sehen den erkannten Text und den Echobot, der mit der deutschen Standardstimme antwortet.

### <a name="change-the-default-bot-voice"></a>Ändern der Standardstimme des Bots

Die Auswahl der Sprachsynthesestimme und das Steuern der Aussprache können erfolgen, wenn der Bot die Antwort in einer [Markupsprache für Sprachsynthese](speech-synthesis-markup.md) (Speech Synthesis Markup Language, SSML) anstelle von einfachem Text angibt. Der Echobot verwendet SSML nicht, der Code lässt sich aber leicht ändern, um dies zu erreichen. Im Beispiel unten fügen wir der Antwort des Echobots SSML hinzu, sodass die deutsche Stimme Stefan Apollo (eine männliche Stimme) anstelle der standardmäßigen weiblichen Stimme verwendet wird. Weitere Informationen finden Sie in der Liste der für Ihre Sprache unterstützten [Standardstimmen](language-support.md#standard-voices) und [neuronalen Stimmen](language-support.md#neural-voices).

1. Öffnen Sie zunächst `samples\csharp_dotnetcore\02.echo-bot\echo-bot.cs`.
2. Suchen Sie diese zwei Zeilen:
    ```csharp
    var replyText = $"Echo: {turnContext.Activity.Text}";
    await turnContext.SendActivityAsync(MessageFactory.Text(replyText, replyText), cancellationToken);
    ```
3. Ersetzen Sie sie durch:
    ```csharp
    var replyText = $"Echo: {turnContext.Activity.Text}";
    var replySpeak = @"<speak version='1.0' xmlns='https://www.w3.org/2001/10/synthesis' xml:lang='de-DE'>
                    <voice name='Microsoft Server Speech Text to Speech Voice (de-DE, Stefan, Apollo)'>" +
                    $"{replyText}" + "</voice></speak>";
    await turnContext.SendActivityAsync(MessageFactory.Text(replyText, replySpeak), cancellationToken);
    ```
4. Erstellen Sie die Projektmappe in Visual Studio, und beheben Sie alle Buildfehler.

Mit dem zweiten Argument in der Methode ‚MessageFactory.Text‘ wird das [Activity Speak-Feld](https://github.com/Microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md#speak) (Sprachausgabe der Aktivität) in der Botantwort festgelegt. Mit der obigen Änderung wurde einfacher Text durch SSML ersetzt, um eine nicht standardmäßige deutsche Stimme anzugeben.

### <a name="redeploy-your-bot"></a>Erneutes Bereitstellen des Bots

Nachdem Sie die erforderliche Änderung an dem Bot vorgenommen haben, wird er im nächsten Schritt erneut in Ihrer Azure App Service-Instanz bereitgestellt und dann getestet:

1. Klicken Sie im Fenster des Projektmappen-Explorers mit der rechten Maustaste auf das Projekt **EchoBot**, und wählen Sie **Veröffentlichen** aus.
2. Ihre vorherige Bereitstellungskonfiguration wurde bereits als Standardkonfiguration geladen. Klicken Sie einfach neben **EchoBot20190805125647 - Web Deploy** auf **Veröffentlichen**.
3. Die Meldung **Veröffentlichung erfolgreich** wird im Visual Studio-Ausgabefenster angezeigt, und eine Webseite mit der Meldung „Your bot is ready!“ (Ihr Bot wurde erstellt) wird geöffnet.
4. Öffnen Sie die App für den Windows-Sprachassistent-Client, klicken Sie auf die Schaltfläche für Einstellungen (Zahnradsymbol oben rechts), und vergewissern Sie sich, dass immer noch `de-de` im Feld „Sprache“ angegeben ist.
5. Befolgen Sie die Anweisungen unter [Ausführen des Windows-Sprachassistent-Clients](#run-the-windows-voice-assistant-client), um eine Verbindung mit dem neu bereitgestellten Bot herzustellen, in der neuen Sprache zu sprechen und die Antwort des Bots in dieser Sprache mit einer neuen Stimme zu hören.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie den in diesem Tutorial bereitgestellten Echobot nicht mehr verwenden möchten, können Sie ihn und alle zugehörigen Azure-Ressourcen entfernen, indem Sie einfach die Azure-Ressourcengruppe **SpeechEchoBotTutorial-ResourceGroup** löschen.

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) im Navigationsbereich **Azure-Dienste** auf **Ressourcengruppen**.
2. Navigieren Sie zu der Ressourcengruppe mit dem Namen **SpeechEchoBotTutorial-ResourceGroup**. Klicken Sie auf die drei Punkte (...).
3. Wählen Sie die Option **Ressourcengruppe löschen**.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer eigenen Client-App mit dem Speech SDK](quickstart-voice-assistant-csharp-uwp.md)

## <a name="see-also"></a>Weitere Informationen

* Bereitstellung in einer [Azure-Region in Ihrer Nähe](https://azure.microsoft.com/global-infrastructure/locations/) zur Verbesserung der Antwortzeit des Bots
* Bereitstellung in einer [Azure-Region, in der neuronale TTS-Stimmen in hoher Qualität unterstützt werden](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#standard-and-neural-voices)
* Preisübersicht für den Direct Line Speech-Kanal:
  * [Botdienst – Preise](https://azure.microsoft.com/pricing/details/bot-service/)
  * [Speech-Dienst](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/)
* Erstellen und Bereitstellen eines eigenen sprachaktivierten Bots:
  * Erstellen eines [Bot Framework-Bots](https://dev.botframework.com/): Registrieren des Bots beim [Direct Line Speech-Kanal](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech?view=azure-bot-service-4.0) und [Anpassen des Bots für Sprache](https://docs.microsoft.com/azure/bot-service/directline-speech-bot?view=azure-bot-service-4.0)
  * Testen vorhandener [Bot Framework-Lösungen](https://microsoft.github.io/botframework-solutions/index): Erstellen Sie einen [virtuellen Assistenten](https://microsoft.github.io/botframework-solutions/overview/virtual-assistant-solution/), und erweitern Sie ihn um [Direct Line Speech](https://microsoft.github.io/botframework-solutions/clients-and-channels/tutorials/enable-speech/1-intro/).
