---
title: include file
description: include file
author: anthonychu
ms.service: signalr
ms.topic: include
ms.date: 03/04/2019
ms.author: antchu
ms.custom: include file
ms.openlocfilehash: 2af9c1dfd13d3aeafa7cf1ac76537117ecc15aff
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "84317726"
---
## <a name="run-the-web-application"></a>Ausführen der Webanwendung

1. Um Ihre Clienttests zu vereinfachen, öffnen Sie Ihren Browser mit unserer Beispielwebanwendung mit einer einzelnen Seite [https://azure-samples.github.io/signalr-service-quickstart-serverless-chat/demo/chat-v2/](https://azure-samples.github.io/signalr-service-quickstart-serverless-chat/demo/chat-v2/). 

    > [!NOTE]
    > Die HTML-Quelldatei befindet sich unter [/docs/demo/chat-v2/index.html](https://github.com/Azure-Samples/signalr-service-quickstart-serverless-chat/blob/master/docs/demo/chat-v2/index.html). Wenn Sie den HTML-Code selbst hosten möchten, starten Sie einen lokalen HTTP-Server wie [http-server](https://www.npmjs.com/package/http-server) im Verzeichnis */docs/demo/chat-v2*. Stellen Sie sicher, dass der Ursprung der Einstellung `CORS` in *local.settings.json* ähnlich dem Beispiel hinzugefügt wird.
    > 
    > ```javascript
    > "Host": {
    >  "LocalHttpPort": 7071,
    >  "CORS": "http://localhost:8080,https://azure-samples.github.io",
    >  "CORSCredentials": true
    > }
    >
    > ```

1. Wenn Sie zur Eingabe der Basis-URL für die Funktions-App aufgefordert werden, geben Sie `http://localhost:7071` ein.

1. Geben Sie einen Benutzernamen ein, wenn Sie dazu aufgefordert werden.

1. Die Webanwendung ruft die Funktion *GetSignalRInfo* in der Funktions-App auf, um die Verbindungsinformationen zum Herstellen einer Verbindung mit dem Azure SignalR-Dienst abzurufen. Nachdem die Verbindung hergestellt wurde, wird das Eingabefeld für Chatnachrichten angezeigt.

1. Geben Sie eine Nachricht ein, und drücken Sie die EINGABETASTE. Die Anwendung sendet die Nachricht an die *SendMessage*-Funktion in der Azure Functions-App, die dann die SignalR-Ausgabebindung verwendet, um die Nachricht an alle verbundenen Clients zu senden. Wenn alles ordnungsgemäß funktioniert, sollte die Nachricht in der Anwendung angezeigt werden.

    ![Ausführen der Anwendung](../media/signalr-quickstart-azure-functions-csharp/signalr-quickstart-run-application.png)

1. Öffnen Sie eine andere Instanz der Webanwendung in einem anderen Browserfenster. Sie können erkennen, dass alle gesendeten Nachrichten in allen Instanzen der Anwendung angezeigt werden.
