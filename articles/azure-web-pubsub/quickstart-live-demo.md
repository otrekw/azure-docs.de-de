---
title: Livedemo des Azure Web PubSub-Diensts
description: Schnellstartanleitung für die ersten Schritte mit der Livedemo des Azure Web PubSub-Diensts
author: yjin81
ms.author: yajin1
ms.service: azure-web-pubsub
ms.topic: overview
ms.date: 04/26/2021
ms.openlocfilehash: a516d215a971b2a6cb54d4e73305cc69ce0edf8c
ms.sourcegitcommit: 62e800ec1306c45e2d8310c40da5873f7945c657
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108166889"
---
# <a name="quickstart-get-started-with-live-demo"></a>Schnellstart: Erste Schritte mit der Livedemo

Der Azure Web PubSub-Dienst unterstützt Sie beim einfachen Erstellen von Echtzeit-Messagingwebanwendungen mithilfe von WebSockets und des Veröffentlichen-Abonnieren-Musters. In dieser Schnellstartanleitung erfahren Sie, wie Sie mit einer Livedemo einfach die ersten Schritte ausführen.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [create-instance-portal](includes/create-instance-portal.md)]

## <a name="get-started-with-the-live-demo"></a>Erste Schritte mit der Livedemo

### <a name="get-client-url-with-a-temp-access-token"></a>Abrufen der Client-URL mit einem temporären Zugriffstoken

Im ersten Schritt müssen Sie die Client-URL aus der Azure Web PubSub-Instanz abrufen. 

- Navigieren Sie zum Azure-Portal, und ermitteln Sie die Azure Web PubSub-Instanz.
- Navigieren Sie auf dem Blatt `Key` zu `Client URL Generator`. 
- Legen Sie die entsprechenden Rollen (`Roles`) fest: **An Gruppen senden** und **Gruppen beitreten/verlassen**.
- Generieren und kopieren Sie `Client Access URL`. 

### <a name="try-the-live-demo"></a>Ausprobieren der Livedemo 

Mit dieser Livedemo können Sie problemlos einer Gruppe beitreten oder diese verlassen und Nachrichten an die Gruppenmitglieder senden. 

- Öffnen Sie die [Demo für clientseitiges Veröffentlichen/Abonnieren](https://azure.github.io/azure-webpubsub/demos/clientpubsub.html), fügen Sie `Client Access URL` ein, und stellen Sie eine Verbindung her. 
- Probieren Sie unterschiedliche Gruppen zum Beitreten sowie verschiedene Gruppen aus, an die Nachrichten gesendet werden sollen, und sehen Sie sich an, welche Nachrichten empfangen werden.
- Sie können beim Generieren von `Client Access URL` auch `Roles` deaktivieren, um zu sehen, was geschieht, wenn Sie einer Gruppe beitreten/eine Gruppe verlassen oder Nachrichten an eine Gruppe senden.