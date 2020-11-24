---
title: Aktualisieren eines Befehls über die Client-App
titleSuffix: Azure Cognitive Services
description: Aktualisieren eines Befehls über die Client-App
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/20/2020
ms.author: encorona
ms.openlocfilehash: 1bffb09d0f49bbd0059e8a528d67bfe215f0650d
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2020
ms.locfileid: "94654346"
---
# <a name="update-a-command-from-the-client"></a>Aktualisieren eines Befehls vom Client

In diesem Artikel erfahren Sie, wie Sie einen laufenden Befehl von einer Clientanwendung aus aktualisieren.

## <a name="prerequisites"></a>Voraussetzungen
> [!div class = "checklist"]
> * Eine zuvor [erstellte Anwendung für benutzerdefinierte Befehle](quickstart-custom-commands-application.md)

## <a name="update-the-state-of-a-command"></a>Aktualisieren des Zustands eines Befehls

Wenn Ihre Clientanwendung eine Aktualisierung des Zustands eines laufenden Befehls ohne Spracheingabe erfordert, können Sie ein Ereignis senden, um den Befehl zu aktualisieren.

Um dieses Szenario zu veranschaulichen, können wir zur Aktualisierung des Zustands eines laufenden Befehls (TurnOnOff) die folgende Ereignisaktivität senden. 

```json
{
  "type": "event",
  "name": "RemoteUpdate",
  "value": {
    "updatedCommand": {
      "name": "TurnOnOff",
      "updatedParameters": {
        "OnOff": "on"
      },
      "cancel": false
    },
    "updatedGlobalParameters": {},
    "processTurn": true
  }
}
```

Lassen Sie uns die Schlüsselattribute dieser Aktivität überprüfen.

| attribute | Erklärung |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **Typ** | Die Aktivität ist vom Typ „Ereignis“, und der Name des Ereignisses muss „RemoteUpdate“ lauten. |
| **value** | Das Attribut „value“ enthält die zur Aktualisierung des aktuellen Befehls erforderlichen Attribute. |
| **updatedCommand** | Das Attribut „updatedCommand“ enthält den Namen des Befehls, „updatedParameters“ ist eine Zuordnung mit dem Namen der Parameter und ihren aktualisierten Werten. |
| **cancel** | Wenn der laufende Befehl abgebrochen werden muss, legen Sie das Attribut „cancel“ auf „true“ fest. |
| **updatedGlobalParameters** | Das Attribut „updatedGlobalParameters“ ist genau wie „updatedParameters“ ebenfalls eine Zuordnung, wird aber für globale Parameter verwendet. |
| **processTurn** | Wenn der Durchgang nach dem Senden der Aktivität verarbeitet werden muss, legen Sie das Attribut „processTurn“ auf „true“ fest. |

Sie können dieses Szenario im Portal für benutzerdefinierte Befehle testen.

1. Öffnen Sie die Anwendung für benutzerdefinierte Befehle, die Sie zuvor erstellt haben. 
1. Klicken Sie auf „Trainieren“ und dann auf „Testen“.
1. Senden Sie „turn“.
1. Öffnen Sie die Seitenleiste, und klicken Sie auf „Aktivitäts-Editor“.
1. Geben Sie das im vorigen Abschnitt angegebene RemoteCommand-Ereignis ein und senden Sie es.
    > [!div class="mx-imgBorder"]
    > ![Senden eines Remotebefehls](media/custom-commands/send-remote-command-activity.png)

Beachten Sie, wie der Wert für den Parameter „OnOff“ mithilfe einer Aktivität des Clients anstelle von Sprache oder Text auf „on“ festgelegt wurde.

## <a name="update-the-catalog-of-the-parameter-for-a-command"></a>Aktualisieren des Katalogs des Parameters für einen Befehl

Wenn Sie die Liste der gültigen Optionen für einen Parameter konfigurieren, werden die Werte für den Parameter global für die Anwendung definiert. 

In unserem Beispiel verfügt der Parameter „SubjectDevice“ unabhängig von der Unterhaltung über eine feste Liste von unterstützten Werten.

Falls Sie dem Katalog des Parameters pro Unterhaltung neue Einträge hinzufügen möchten, können Sie die folgende Aktivität senden.

```json
{
  "type": "event",
  "name": "RemoteUpdate",
  "value": {
    "catalogUpdate": {
      "commandParameterCatalogs": {
        "TurnOnOff": [
          {
            "name": "SubjectDevice",
            "values": {
              "stereo": [
                "cd player"
              ]
            }
          }
        ]
      }
    },
    "processTurn": false
  }
}
```
Mit dieser Aktivität haben wir dem Katalog des Parameters „SubjectDevice“ im Befehl „TurnOnOff“ einen Eintrag für „stereo“ hinzugefügt.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/custom-commands/update-catalog-with-remote-activity.png" alt-text="Updatekatalog":::

Beachten Sie ein paar Punkte.
1. Sie müssen diese Aktivität nur einmal senden (idealerweise gleich nach dem Herstellen einer Verbindung).
1. Nachdem Sie diese Aktivität gesendet haben, sollten Sie darauf warten, dass das Ereignis „ParameterCatalogsUpdated“ an den Client zurückgesendet wird.

## <a name="add-additional-context-from-the-client-application"></a>Hinzufügen von zusätzlichem Kontext aus der Clientanwendung

Sie können von der Clientanwendung aus pro Konversation zusätzlichen Kontext festlegen, der später in Ihrer Anwendung für benutzerdefinierte Befehle verwendet werden kann. 

Denken Sie z. B. an das Szenario, in dem Sie die ID und den Namen des Geräts senden möchten, das mit der Anwendung für benutzerdefinierte Befehle verbunden ist.

Um dieses Szenario zu testen, erstellen wir einen neuen Befehl in unserer aktuellen Anwendung.
1. Erstellen Sie einen neuen Befehl namens „GetDeviceInfo“.
1. Fügen Sie einen Beispielsatz mit „get device info“ hinzu.
1. Fügen Sie in der Vervollständigungsregel „Fertig“ eine Aktion „Sprachantwort senden“ hinzu, die die Attribute von „clientContext“ enthält.
    > ![Senden einer Sprachantwort mit Kontext](media/custom-commands/send-speech-response-context.png)
1. Speichern, trainieren und testen Sie Ihre Anwendung.
1. Sobald Sie sich im Testfenster befinden, senden Sie eine Aktivität, um den Clientkontext zu aktualisieren.
    > ```json
    >{
    >   "type": "event",
    >   "name": "RemoteUpdate",
    >   "value": {
    >     "clientContext": {
    >       "deviceId": "12345",
    >       "deviceName": "My device"
    >     },
    >     "processTurn": false
    >   }
    >}
    > ```
1. Senden Sie den Text „get device info“.
    > ![Aktivität zum Senden des Clientkontexts](media/custom-commands/send-client-context-activity.png)

Beachten Sie einige Punkte.
1. Sie müssen diese Aktivität nur einmal senden (idealerweise gleich nach dem Herstellen einer Verbindung).
1. Sie können komplexe Objekte für „clientContext“ verwenden.
1. Sie können „clientContext“ in Sprachantworten, zum Senden von Aktivitäten und beim Aufrufen von Webendpunkten verwenden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Aktualisieren eines Befehls über einen Webendpunkt](./how-to-custom-commands-update-command-from-web-endpoint.md)
