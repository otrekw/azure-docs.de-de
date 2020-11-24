---
title: Aktualisieren eines Befehls über einen Webendpunkt
titleSuffix: Azure Cognitive Services
description: Aktualisieren eines Befehls über einen Webendpunkt
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/20/2020
ms.author: encorona
ms.openlocfilehash: 4432843ac93002bc92068db191706352234d76e6
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2020
ms.locfileid: "94572651"
---
# <a name="update-a-command-from-a-web-endpoint"></a>Aktualisieren eines Befehls über einen Webendpunkt

Wenn Ihre Clientanwendung es erfordert, den Zustand eines laufenden Befehls ohne Spracheingabe zu aktualisieren, können Sie einen Aufruf für einen Webendpunkt verwenden, um den Befehl zu aktualisieren.

In diesem Artikel erfahren Sie, wie Sie einen laufenden Befehl über einen Webendpunkt aktualisieren.

## <a name="prerequisites"></a>Voraussetzungen
> [!div class = "checklist"]
> * Eine zuvor [erstellte Anwendung für benutzerdefinierte Befehle](quickstart-custom-commands-application.md)

## <a name="create-an-azure-function"></a>Erstellen einer Azure Function 

Für dieses Beispiel benötigen wir eine über HTTP ausgelöste [Azure-Funktion](https://docs.microsoft.com/azure/azure-functions/), die die folgende Eingabe (oder eine Teilmenge dieser Eingabe) unterstützt.

```JSON
{
  "conversationId": "SomeConversationId",
  "currentCommand": {
    "name": "SomeCommandName",
    "parameters": {
      "SomeParameterName": "SomeParameterValue",
      "SomeOtherParameterName": "SomeOtherParameterValue"
    }
  },
  "currentGlobalParameters": {
      "SomeGlobalParameterName": "SomeGlobalParameterValue",
      "SomeOtherGlobalParameterName": "SomeOtherGlobalParameterValue"
  }
}
```

Lassen Sie uns die Schlüsselattribute dieser Eingabe überprüfen.

| attribute | Erklärung |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **conversationId** | „conversationId“ ist der eindeutige Bezeichner der Unterhaltung. Beachten Sie, dass diese ID von der Client-App generiert werden kann. |
| **currentCommand** | „currentCommand“ ist der aktuell aktive Befehl in der Unterhaltung. |
| **name** | „name“ ist der Name des Befehls und „parameters“ ist eine Zuordnung mit den aktuellen Werten der Parameter. |
| **currentGlobalParameters** | „currentGlobalParameters“ ist ebenfalls eine Zuordnung wie „parameters“, wird aber für globale Parameter verwendet. |

Die Ausgabe der Azure-Funktion muss das folgende Format unterstützen.

```JSON
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

Möglicherweise erkennen Sie dieses Format, da es dasselbe ist, das beim [Aktualisieren eines Befehls vom Client](./how-to-custom-commands-update-command-from-client.md) verwendet wird. 

Erstellen Sie jetzt eine Azure-Funktion auf der Basis von NodeJS. Anschließend kopieren Sie diesen Code und fügen ihn ein.

```nodejs
module.exports = async function (context, req) {
    context.log(req.body);
    context.res = {
        body: {
            updatedCommand: {
                name: "IncrementCounter",
                updatedParameters: {
                    Counter: req.body.currentCommand.parameters.Counter + 1
                }
            }
        }
    };
}
```

Wenn wir diese Azure-Funktion über benutzerdefinierte Befehle aufrufen, senden wir die aktuellen Werte der Unterhaltung und geben die Parameter zurück, die wir aktualisieren möchten oder wenn wir den aktuellen Befehl abbrechen möchten.

## <a name="update-the-existing-custom-commands-app"></a>Aktualisieren der vorhandenen App „Benutzerdefinierte Befehle“

Jetzt wollen wir die Azure-Funktion mit der vorhandenen App „Benutzerdefinierte Befehle“ verbinden.

1. Fügen Sie einen neuen Befehl mit dem Namen „IncrementCounter“ hinzu.
1. Fügen Sie nur einen Beispielsatz mit dem Wert „increment“ hinzu.
1. Fügen Sie einen neuen Parameter namens „Counter“ (gleicher Name wie oben in der Azure-Funktion angegeben) vom Typ „Number“ mit einem Standardwert von „0“ hinzu.
1. Fügen Sie einen neuen Webendpunkt namens „IncrementEndpoint“ mit der URL Ihrer Azure-Funktion und mit aktivierten Remoteupdates hinzu.
    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-commands/set-web-endpoint-with-remote-updates.png" alt-text="Festlegen des Webendpunkts mit Remoteupdates":::
1. Erstellen Sie eine neue Interaktionsregel namens „IncrementRule“, und fügen Sie eine Aktion zum Aufrufen des Webendpunkts hinzu.
    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-commands/increment-rule-web-endpoint.png" alt-text="Inkrementregel":::
1. Wählen Sie in der Aktionskonfiguration den „IncrementEndpunkt“ aus, konfigurieren Sie „Bei Erfolg“, um eine Sprachantwort mit dem Wert von „Counter“ zu senden, und „Bei Fehler“, um eine Fehlermeldung zu senden.
    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-commands/set-increment-counter-call-endpoint.png" alt-text="Festlegen des Aufrufendpunkts des Inkrementzählers":::
1. Legen Sie den Zustand nach der Ausführung der Regel so fest, dass auf die Benutzereingabe gewartet wird.

## <a name="test-it"></a>Testen

1. Speichern und Trainieren Sie Ihre App.
1. Klicken Sie auf „Testen“.
1. Senden Sie mehrmals „increment“ (dies ist der Beispielsatz für den Befehl „IncrementCounter“).
    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-commands/increment-counter-example.png" alt-text="Beispiel für Inkrementzähler":::

Beachten Sie, wie der Wert des Counter-Parameters bei jedem Durchgang von der Azure-Funktion inkrementiert wird.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Aktivieren eines CI/CD-Prozesses für die Anwendung für benutzerdefinierte Befehle](./how-to-custom-commands-deploy-cicd.md)