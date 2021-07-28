---
title: Livedemo des Azure Web PubSub-Diensts
description: Schnellstartanleitung für die ersten Schritte mit der Livedemo des Azure Web PubSub-Diensts
author: yjin81
ms.author: yajin1
ms.service: azure-web-pubsub
ms.topic: overview
ms.date: 04/26/2021
ms.openlocfilehash: e35ef66c038748e31ba218a56adb8111374d2339
ms.sourcegitcommit: 6bd31ec35ac44d79debfe98a3ef32fb3522e3934
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2021
ms.locfileid: "113219604"
---
# <a name="quickstart-get-started-with-chatroom-live-demo"></a>Schnellstart: Erste Schritte mit der Chatroom-Livedemo

Der Azure Web PubSub-Dienst unterstützt Sie beim einfachen Erstellen von Echtzeit-Messagingwebanwendungen mithilfe von WebSockets und des Veröffentlichen-Abonnieren-Musters. Die [Chatroom-Livedemo](https://azure.github.io/azure-webpubsub/demos/clientpubsub.html) veranschaulicht die Echtzeit-Messagingfunktion von Azure Web PubSub. Mit dieser Livedemo können Sie ganz einfach einer Chatgruppe beitreten und Echtzeitnachrichten an eine bestimmte Gruppe senden. 

:::image type="content" source="media/quickstart-live-demo/chat-live-demo.gif" alt-text="Verwenden der Chatroom-Livedemo":::

In dieser Schnellstartanleitung erfahren Sie, wie Sie mit einer Livedemo einfach die ersten Schritte ausführen.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [create-instance-portal](includes/create-instance-portal.md)]

## <a name="get-started-with-the-chatroom-live-demo"></a>Erste Schritte mit der Chatroom-Livedemo

### <a name="get-client-url-with-a-temp-access-token"></a>Abrufen der Client-URL mit einem temporären Zugriffstoken

Im ersten Schritt müssen Sie die Client-URL aus der Azure Web PubSub-Instanz abrufen. 

- Navigieren Sie zum Azure-Portal, und ermitteln Sie die Azure Web PubSub-Instanz.
- Navigieren Sie auf dem Blatt `Key` zu `Client URL Generator`. 
- Legen Sie die entsprechenden Rollen (`Roles`) fest: **An Gruppen senden** und **Gruppen beitreten/verlassen**.
- Generieren und kopieren Sie `Client Access URL`. 

:::image type="content" source="media/quickstart-live-demo/generate-client-url.png" alt-text="Screenshot des Generierens der Client-URL":::

### <a name="try-the-live-demo"></a>Ausprobieren der Livedemo 

Mit dieser Livedemo können Sie problemlos einer Gruppe beitreten oder diese verlassen und Nachrichten an die Gruppenmitglieder senden. 

- Öffnen Sie die [Chatroom-Livedemo](https://azure.github.io/azure-webpubsub/demos/clientpubsub.html), fügen Sie `Client Access URL` ein, und wählen Sie „Verbinden“ aus. 

:::image type="content" source="media/quickstart-live-demo/paste-client-access-url.png" alt-text="Screenshot des Hinzufügens der Client-URL mit der Livedemo":::

> [!NOTE]
>  Die **Clientzugriffs-URL** im Portal dient der Benutzerfreundlichkeit, um Ihnen den Einstieg zu vereinfachen. Sie können diese Clientzugriffs-URL auch für einige schnelle Verbindungstests verwenden. Wenn Sie eine eigene Anwendung schreiben möchten, stehen Ihnen dafür SDKs in vier Sprachen zur Verfügung, mit deren Hilfe Sie die URL generieren können. 

- Probieren Sie unterschiedliche Gruppen zum Beitreten sowie verschiedene Gruppen aus, an die Nachrichten gesendet werden sollen, und sehen Sie sich an, welche Nachrichten empfangen werden. Zum Beispiel:
    - Lassen Sie zwei Clients derselben Gruppe beitreten. Sie werden sehen, dass die Nachricht an die Gruppenmitglieder übertragen werden kann. 
    - Lassen Sie zwei Clients unterschiedlichen Gruppen beitreten. Sie werden sehen, dass der Client keine Nachricht empfangen kann, wenn er kein Gruppenmitglied ist. 
- Sie können beim Generieren der `Client Access URL` auch `Roles` deaktivieren, um zu sehen, was geschieht, wenn Sie einer Gruppe beitreten oder Nachrichten an eine Gruppe senden. Zum Beispiel:
    - Deaktivieren Sie die Berechtigung `Send to Groups`. Sie werden sehen, dass der Client keine Nachrichten an die Gruppe senden kann. 
    - Deaktivieren Sie die Berechtigung `Join/Leave Groups`. Sie werden sehen, dass der Client keiner Gruppe beitreten kann. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie die Echtzeit-Messagingfunktionen anhand der Chatroom-Livedemo kennengelernt. Sie können nun damit beginnen, Ihre eigene Anwendung zu erstellen. 

> [!div class="nextstepaction"]
> [Schnellstart: Veröffentlichen und Abonnieren von Nachrichten in Azure Web PubSub](https://azure.github.io/azure-webpubsub/getting-started/publish-messages/js-publish-message)

> [!div class="nextstepaction"]
> [Schnellstart: Erstellen eines einfachen Chatrooms mit Azure Web PubSub](https://azure.github.io/azure-webpubsub/getting-started/create-a-chat-app/js-handle-events)

> [!div class="nextstepaction"]
> [Schnellstart: Erstellen einer einfachen serverlosen Chatanwendung mit Azure Functions und dem Azure Web PubSub-Dienst](./quickstart-serverless.md)

> [!div class="nextstepaction"]
> [Weitere Beispiele für Azure Web PubSub](https://github.com/Azure/azure-webpubsub/tree/main/samples)