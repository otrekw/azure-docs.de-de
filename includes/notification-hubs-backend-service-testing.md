---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 5e75c5d5510f596eb7911cae0310e60b6bef67bf
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86146463"
---
### <a name="send-a-test-notification"></a>Senden einer Testbenachrichtigung

1. Öffnen Sie in [Postman](https://www.postman.com/downloads/) eine neue Registerkarte.

1. Legen Sie die Anforderung auf **POST** fest, und geben Sie die folgende Adresse ein:

    ```xml
    https://<app_name>.azurewebsites.net/api/notifications/requests
    ```

1. Wenn Sie sich entschieden haben, die Schritte im Abschnitt [Authentifizieren von Clients mit einem API-Schlüssel](#authenticate-clients-using-an-api-key-optional) durchzuführen, stellen Sie sicher, dass die Anforderungsheader Ihren **apikey**-Wert enthalten.

   | Schlüssel                            | Wert                          |
   | ------------------------------ | ------------------------------ |
   | apikey                         | <your_api_key>                 |

1. Wählen Sie die Option **raw** für den **Text** aus, wählen Sie dann **JSON** aus der Liste der Formatoptionen aus, und fügen Sie anschließend **JSON**-Platzhalterinhalt ein:

    ```json
    {
        "text": "Message from Postman!",
        "action": "action_a"
    }
    ```

1. Klicken Sie auf die Schaltfläche **Code** oben rechts im Fenster unter der Schaltfläche **Speichern**. Die Anforderung sollte ähnlich wie im folgenden Beispiel aussehen, wenn sie für **HTML** angezeigt wird (je nachdem, ob Sie einen **apikey**-Header eingefügt haben):

    ```html
    POST /api/notifications/requests HTTP/1.1
    Host: https://<app_name>.azurewebsites.net
    apikey: <your_api_key>
    Content-Type: application/json

    {
        "text": "Message from backend service",
        "action": "action_a"
    }
    ```

1. Führen Sie die Anwendung **PushDemo** auf einer oder beiden Zielplattformen (**Android** und **iOS**) aus.

    > [!NOTE]
    > Wenn Sie auf **Android** testen, stellen Sie sicher, dass Sie nicht in **Debug** ausgeführt werden, oder wenn die Anwendung durch Ausführen der Anwendung bereitgestellt wurde, erzwingen Sie das Schließen der Anwendung, und starten Sie sie erneut vom Startprogramm aus.

1. Tippen Sie in der **PushDemo**-App auf die Schaltfläche **Registrieren**.

1. Kehren Sie zu **[Postmann](https://www.postman.com/downloads)** zurück, schließen Sie das Fenster **Codeausschnitt generieren** (falls Sie dies noch nicht getan haben). und klicken Sie dann auf die Schaltfläche **Senden**.

1. Vergewissern Sie sich, dass Sie eine **200 OK**-Antwort in **[Postmann](https://www.postman.com/downloads)** erhalten, und dass die Warnung **-Aktion empfangen** in der App angezeigt wird.  

1. Schließen Sie die **PushDemo**-App, und klicken Sie dann erneut auf die Schaltfläche **Senden** in **[Postmann](https://www.postman.com/downloads)** .

1. Überprüfen Sie, dass Sie wieder eine **200 OK**-Antwort in **[Postmann](https://www.postman.com/downloads)** erhalten. Vergewissern Sie sich, dass im Benachrichtigungsbereich für die **PushDemo**-App eine Benachrichtigung mit der richtigen Meldung angezeigt wird.

1. Tippen Sie auf die Benachrichtigung, um zu bestätigen, dass die App geöffnet und die Warnung **ActionA-Aktion empfangen** angezeigt wurde.

1. Kehren Sie zu **[Postmann](https://www.postman.com/downloads)** zurück, und ändern Sie den vorherigen Anforderungstext so, dass eine Hintergrundbenachrichtigung gesendet wird, die *action_b* statt *action_a* als Wert für **action** angibt.

    ```json
    {
        "action": "action_b",
        "silent": true
    }
    ```

1. Klicken Sie, während die App noch geöffnet ist, auf die Schaltfläche **Senden** in **[Postman](https://www.postman.com/downloads)** .

1. Vergewissern Sie sich, dass Sie eine **200 OK**-Antwort in **[Postmann](https://www.postman.com/downloads)** erhalten, und dass die Warnung **ActionB-Aktion empfangen** anstelle von **ActionA-Aktion empfangen** in der App angezeigt wird.

1. Schließen Sie die **PushDemo**-App, und klicken Sie dann erneut auf die Schaltfläche **Senden** in **[Postmann](https://www.postman.com/downloads)** .

1. Vergewissern Sie sich, dass Sie eine **200 OK**-Antwort in **[Postmann](https://www.postman.com/downloads)** erhalten, und dass die Hintergrundbenachrichtigung nicht im Benachrichtigungsbereich angezeigt wird.
