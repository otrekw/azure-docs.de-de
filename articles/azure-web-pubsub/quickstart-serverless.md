---
title: 'Schnellstart: Serverloser Azure Web PubSub-Dienst'
description: Eine Schnellstartanleitung zur Verwendung von Azure Web PubSub-Dienst und Azure Functions für eine serverlosen Anwendung.
author: yjin81
ms.author: yajin1
ms.service: azure-web-pubsub
ms.topic: overview
ms.date: 03/11/2021
ms.openlocfilehash: 43d702f3294d728b196de69790f543dc67491400
ms.sourcegitcommit: beff1803eeb28b60482560eee8967122653bc19c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2021
ms.locfileid: "113433934"
---
# <a name="quickstart-create-a-serverless-simple-chat-application-with-azure-functions-and-azure-web-pubsub-service"></a>Schnellstart: Erstellen einer einfachen serverlosen Chatanwendung mit Azure Functions und dem Azure Web PubSub-Dienst 

Der Azure Web PubSub-Dienst unterstützt Sie beim einfachen Erstellen von Echtzeit-Messagingwebanwendungen mithilfe von WebSockets und des Veröffentlichen-Abonnieren-Musters. Azure Functions ist eine serverlose Plattform, mit der Sie Ihren Code ohne Verwaltung von Infrastruktur ausführen können. In dieser Schnellstartanleitung erfahren Sie, wie Sie den Azure Web PubSub-Dienst und Azure Functions verwenden, um eine serverlose Anwendung mit Echtzeitmessaging und dem Veröffentlichen-Abonnieren-Muster zu erstellen.

## <a name="prerequisites"></a>Voraussetzungen

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Installieren Sie einen Code-Editor (beispielsweise [Visual Studio Code](https://code.visualstudio.com/)) sowie die Bibliothek [Node.js](https://nodejs.org/en/download/) (Version 10.x).

   > [!NOTE]
   > Weitere Informationen zu den unterstützten Versionen von Node.js finden Sie in der [Übersicht über die Runtimeversionen von Azure Functions](../azure-functions/functions-versions.md#languages).

Installieren Sie die [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools#installing) (ab Version 2.7.1505), um Azure Functions-Apps lokal auszuführen.

# <a name="c"></a>[C#](#tab/csharp)

Installieren Sie einen Code-Editor wie [Visual Studio Code](https://code.visualstudio.com/).

Installieren Sie die [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools#installing) (ab Version 3), um Azure Functions-Apps lokal auszuführen.

---

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [create-instance-portal](includes/create-instance-portal.md)]

## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Während der Dienst bereitgestellt wird, arbeiten wird mit dem Code. Klonen Sie zuerst die [Beispiel-App von GitHub](https://github.com/Azure/azure-webpubsub/tree/main/samples/functions/js/simplechat).

1. Öffnen Sie ein Git-Terminalfenster. Navigieren Sie zu einem Ordner, in dem Sie das Beispielprojekt klonen möchten.

1. Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. Dieser Befehl erstellt eine Kopie der Beispiel-App auf Ihrem Computer.

    ```bash
    git clone https://github.com/Azure/azure-webpubsub.git
    ```

## <a name="configure-and-run-the-azure-function-app"></a>Konfigurieren und Ausführen der Azure Functions-App

- Öffnen Sie im Browser das **Azure-Portal**, und vergewissern Sie sich, dass die zuvor bereitgestellte Instanz des Web PubSub-Diensts erfolgreich erstellt wurde. Navigieren Sie zu der Instanz.
- Wählen Sie **Schlüssel** aus, und kopieren Sie die Verbindungszeichenfolge.

:::image type="content" source="media/quickstart-serverless/copy-connection-string.png" alt-text="Screenshot: Kopieren der Web PubSub-Verbindungszeichenfolge":::

# <a name="javascript"></a>[JavaScript](#tab/javascript)

- Aktualisieren Sie die Funktionskonfiguration.

  Öffnen Sie im geklonten Repository den Ordner */samples/functions/js/simplechat*. Bearbeiten Sie *local.settings.js*, um eine Dienstverbindungszeichenfolge hinzuzufügen.
  Nehmen Sie in *local.settings.json* die folgenden Änderungen vor, und speichern Sie anschließend die Datei:
    - Ersetzen Sie für die Einstellung **`WebPubSubConnectionString`** den Platzhalter *<connection-string>* durch die aus dem **Azure-Portal** kopierte Verbindungszeichenfolge. 
    - Dies ist für die Einstellung **`AzureWebJobsStorage`** erforderlich, da [Azure Functions ein Azure Storage-Konto erfordert](../azure-functions/storage-considerations.md).
        - Wenn der [Azure-Speicheremulator](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409) in der lokalen Umgebung ausgeführt wird, lassen Sie die ursprüngliche Einstellung „UseDevelopmentStorage=true“ unverändert.
        - Wenn Sie über eine Azure Storage-Verbindungszeichenfolge verfügen, ersetzen Sie den Wert durch diese Zeichenfolge.
 
- JavaScript-Funktionen werden in Ordnern organisiert. In jedem Ordner befinden sich zwei Dateien: `function.json` definiert die Bindungen, die in der Funktion verwendet werden, und `index.js` ist der Textkörper der Funktion. Diese Funktions-App enthält mehrere ausgelöste Funktionen:

    - **login**: Dies ist die durch HTTP ausgelöste Funktion. Sie verwendet die Eingabebindung *webPubSubConnection*, um gültige Dienstverbindungsinformationen zu generieren und zurückzugeben.
    - **messages**: Dies ist die durch `WebPubSubTrigger` ausgelöste Funktion. Sie empfängt eine Chatnachricht im Anforderungstext und verwendet die Ausgabebindung `WebPubSub`, um die Nachricht an alle verbundenen Clientanwendungen zu senden.
    - **connect** und **connected**: Dies sind die durch `WebPubSubTrigger` ausgelösten Funktionen. Sie behandeln die Ereignisse „connect“ und „connected“.

- Vergewissern Sie sich im Terminal, dass Sie sich im Ordner */samples/functions/js/simplechat* befinden. Installieren Sie die Erweiterungen, und führen Sie die Funktions-App aus.

    ```bash
    func extensions install

    func start
    ```

# <a name="c"></a>[C#](#tab/csharp)

- Aktualisieren Sie die Funktionskonfiguration.

  Öffnen Sie im geklonten Repository den Ordner */samples/functions/csharp/simplechat*. Bearbeiten Sie *local.settings.js*, um eine Dienstverbindungszeichenfolge hinzuzufügen.
  Nehmen Sie in *local.settings.json* die folgenden Änderungen vor, und speichern Sie anschließend die Datei:
    - Ersetzen Sie für die Einstellung **`WebPubSubConnectionString`** den Platzhalter *<connection-string>* durch die aus dem **Azure-Portal** kopierte Verbindungszeichenfolge. 
    - Dies ist für die Einstellung **`AzureWebJobsStorage`** erforderlich, da [Azure Functions ein Azure Storage-Konto erfordert](../azure-functions/storage-considerations.md).
        - Wenn der [Azure-Speicheremulator](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409) in der lokalen Umgebung ausgeführt wird, lassen Sie die ursprüngliche Einstellung „UseDevelopmentStorage=true“ unverändert.
        - Wenn Sie über eine Azure Storage-Verbindungszeichenfolge verfügen, ersetzen Sie den Wert durch diese Zeichenfolge.

- C#-Funktionen sind nach der Datei „Functions.cs“ organisiert. Diese Funktions-App enthält mehrere ausgelöste Funktionen:

    - **login**: Dies ist die durch HTTP ausgelöste Funktion. Sie verwendet die Eingabebindung *webPubSubConnection*, um gültige Dienstverbindungsinformationen zu generieren und zurückzugeben.
    - **connected**: Dies ist die durch `WebPubSubTrigger` ausgelöste Funktion. Sie empfängt eine Chatnachricht im Anforderungstext und sendet die Nachricht mit mehreren Aufgaben an alle verbundenen Clientanwendungen.
    - **broadcast**: Dies ist die durch `WebPubSubTrigger` ausgelöste Funktion. Sie empfängt eine Chatnachricht im Anforderungstext und sendet die Nachricht mit einer Aufgabe an alle verbundenen Clientanwendungen.
    - **connect** und **disconnect**: Dies sind die durch `WebPubSubTrigger` ausgelösten Funktionen. Diese verarbeiten die Ereignisse „connect“ und „disconnect“ (Verbinden und Verbindung trennen).

- Vergewissern Sie sich im Terminal, dass Sie sich im Ordner */samples/functions/csharp/simplechat* befinden. Installieren Sie die Erweiterungen, und führen Sie die Funktions-App aus.

    ```bash
    func extensions install

    func start
    ```

---

- Von der lokalen Funktion wird der in der Datei `local.settings.json` definierte Port verwendet. Um sie im öffentlichen Netzwerk verfügbar zu machen, muss der Endpunkt mithilfe von [ngrok](https://ngrok.com) verfügbar gemacht werden. Führen Sie den folgenden Befehl aus, um einen Weiterleitungsendpunkt zu erhalten (beispielsweise „http://{ngrok-ID}.ngrok.io -> http://localhost:7071“).

    ```bash
    ngrok http 7071
    ``` 

- Legen Sie `Event Handler` im Azure Web PubSub-Dienst fest. Navigieren Sie im **Azure-Portal** zu Ihrer Web PubSub-Ressource, und wählen Sie **Einstellungen** aus. Fügen Sie eine neue Hubeinstellungszuordnung für die einzelne verwendete Funktion hinzu, und verwenden Sie dabei die folgenden Angaben. Ersetzen Sie „{ngrok-ID}“ durch Ihren Wert.

   - Hubname: simplechat
   - URL-Vorlage: **http://{ngrok-ID}.ngrok.io/runtime/webhooks/webpubsub**
   - Benutzerereignismuster: *
   - Systemereignisse: „connect“, „connected“ und „disconnected“

:::image type="content" source="media/quickstart-serverless/set-event-hanlder.png" alt-text="Screenshot: Festlegen des Ereignishandlers":::

## <a name="run-the-web-application"></a>Ausführen der Webanwendung

1. Öffnen Sie zur Vereinfachung des Clienttests Ihren Browser mit unserer exemplarischen [Single-Page-Webanwendung](http://jialinxin.github.io/webpubsub/). 

1. Geben Sie die Basis-URL der Funktions-App als lokal ein: `http://localhost:7071`.

1. Geben Sie einen Benutzernamen ein.

1. Von der Webanwendung wird die Funktion *login* in der Funktions-App aufgerufen, um die Verbindungsinformationen für die Verbindungsherstellung mit dem Azure Web PubSub-Dienst abzurufen. Wenn `Client websocket opened.` angezeigt wurde, wurde die Verbindung hergestellt. 

1. Geben Sie eine Nachricht ein, und drücken Sie die EINGABETASTE. Die Nachricht wird an die Funktion *messages* in der Azure-Funktions-App gesendet. Diese verwendet daraufhin die Web PubSub-Ausgabebindung, um die Nachricht an alle verbundenen Clients zu übertragen. Wenn alles ordnungsgemäß funktioniert, wird die Nachricht in der Anwendung angezeigt.

1. Öffnen Sie eine andere Instanz der Webanwendung in einem anderen Browserfenster. Sie können erkennen, dass alle gesendeten Nachrichten in allen Instanzen der Anwendung angezeigt werden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese App nicht mehr benötigen, löschen Sie alle im Rahmen dieser Dokumentation erstellten Ressourcen, damit keine Gebühren anfallen. Gehen Sie dazu wie folgt vor:

1. Klicken Sie ganz links im Azure-Portal auf **Ressourcengruppen** und anschließend auf die erstellte Ressourcengruppe. Sie können auch das Suchfeld verwenden, um nach dem Namen der Ressourcengruppe zu suchen.

1. Wählen im daraufhin angezeigten Fenster die Ressourcengruppe und anschließend die Option **Ressourcengruppe löschen** aus.

1. Geben Sie im neuen Fenster den Namen der Ressourcengruppe ein, die Sie löschen möchten, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie eine serverlose einfache Chatanwendung ausführen. Sie können nun damit beginnen, Ihre eigene Anwendung zu erstellen. 

> [!div class="nextstepaction"]
> [Schnellstart: Erstellen eines einfachen Chatrooms mit Azure Web PubSub](https://azure.github.io/azure-webpubsub/getting-started/create-a-chat-app/js-handle-events)

> [!div class="nextstepaction"]
> [Azure Web PubSub-Bindungen für Azure Functions](https://azure.github.io/azure-webpubsub/references/functions-bindings)

> [!div class="nextstepaction"]
> [Weitere Beispiele für Azure Web PubSub](https://github.com/Azure/azure-webpubsub/tree/main/samples)