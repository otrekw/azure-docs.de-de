---
title: 'Tutorial: Language Understanding-Bot Node.js v4'
description: Erstellen Sie in diesem Tutorial mithilfe von Node.js einen in LUIS (Language Understanding Intelligent Service) integrierten Chatbot. Dieser Chatbot implementiert mithilfe der Human Resources-App schnell eine Bot-Lösung. Der Bot wird mit Bot Framework, Version 4, und dem Azure Web-App-Bot erstellt.
ms.topic: tutorial
ms.date: 02/03/2020
ms.openlocfilehash: 3ce12176957412a5599ced8b043f553969194efb
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76987838"
---
# <a name="tutorial-use-a-web-app-bot-enabled-with-language-understanding-in-nodejs"></a>Tutorial: Verwenden eines Web-App-Bots mit aktiviertem Language Understanding in Node.js

Erstellen Sie mithilfe von Node.js einen in Language Understanding (LUIS) integrierten Chatbot. Der Bot wird mit der [Web-App-Bot](https://docs.microsoft.com/azure/bot-service/)-Ressource von Azure und [Bot Framework](https://github.com/Microsoft/botbuilder-dotnet) V4 erstellt.

**In diesem Tutorial lernen Sie Folgendes:**

> [!div class="checklist"]
> * Erstellen eines Web-App-Bots. In diesem Vorgang wird eine neue LUIS-App erstellt.
> * Herunterladen des vom Webbotdienst erstellten Botprojekts
> * Lokales Starten von Bot und Emulator auf dem eigenen Computer
> * Anzeigen von Äußerungsergebnissen im Bot

## <a name="prerequisites"></a>Voraussetzungen

* [Bot-Emulator](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)


## <a name="create-a-web-app-bot-resource"></a>Erstellen einer Web-App-Bot-Ressource

1. Wählen Sie im [Azure-Portal](https://portal.azure.com)**Neue Ressource erstellen** aus.

1. Suchen Sie im Suchfeld nach **Web-App-Bot**, und wählen Sie den Eintrag aus. Klicken Sie auf **Erstellen**.

1. Geben Sie in **Botdienst** die erforderlichen Informationen ein:

    |Einstellung|Zweck|Empfohlene Einstellung|
    |--|--|--|
    |Botname|Ressourcenname|`luis-nodejs-bot-` + `<your-name>`, z.B. `luis-nodejs-bot-johnsmith`|
    |Subscription|Abonnement, in dem der Bot erstellt werden soll.|Ihr primäres Abonnement.
    |Resource group|Logische Gruppe von Azure-Ressourcen|Erstellen einer neuen Gruppe zum Speichern aller Ressourcen, die mit diesen Bot verwendet werden; benennen Sie die Gruppe `luis-nodejs-bot-resource-group`.|
    |Position|Azure-Region: muss nicht die gleiche wie die Erstellungs- oder Veröffentlichungsregion von LUIS sein.|`westus`|
    |Tarif|Für die Grenzwerte der Dienstanforderungen und die Abrechnung verwendet.|`F0` ist der kostenlose Tarif.
    |App-Name|Der Name wird als Unterdomäne verwendet, wenn Ihr Bot in der Cloud bereitgestellt wird (z.B. personalwesenbot.azurewebsites.net).|`luis-nodejs-bot-` + `<your-name>`, z.B. `luis-nodejs-bot-johnsmith`|
    |Botvorlage|Einstellungen für Bot Framework – siehe nächste Tabelle|
    |LUIS-App-Speicherort|Muss mit der LUIS-Ressourcenregion übereinstimmen|`westus`|
    |App Service-Plan/Standort|Ändern Sie den angegebenen Standardwert nicht.|
    |Application Insights|Ändern Sie den angegebenen Standardwert nicht.|
    |Microsoft-App-ID und Kennwort|Ändern Sie den angegebenen Standardwert nicht.|

1. Wählen Sie in **Bot template** (Botvorlage) die folgenden Werte und dann unterhalb dieser Einstellungen die Schaltfläche **Auswählen** aus:

    |Einstellung|Zweck|Auswahl|
    |--|--|--|
    |SDK-Version|Bot-Framework-Version|**SDK v4**|
    |SDK-Sprache|Programmiersprache des Bots|**Node.js**|
    |Bot|Bottyp|**Basisbot**|

1. Klicken Sie auf **Erstellen**. Dadurch wird der Botdienst erstellt und in Azure bereitgestellt. Im Rahmen dieses Vorgangs wird eine LUIS-App mit dem Namen `luis-nodejs-bot-XXXX` erstellt. Dieser Name basiert auf dem Namen der Azure Bot Service-App.

    > [!div class="mx-imgBorder"]
    > [![Erstellen des Web-App-Bots](./media/bfv4-nodejs/create-web-app-service.png)](./media/bfv4-nodejs/create-web-app-service.png#lightbox)

    Warten Sie, bis der Botdienst erstellt wurde, bevor Sie fortfahren.

## <a name="the-bot-has-a-language-understanding-model"></a>Der Bot beinhaltet ein Language Understanding-Modell

Bei der Erstellung des Botdiensts wird auch eine neue LUIS-App mit Absichten und Beispieläußerungen erstellt. Der Bot stellt die Zuordnung der Absichten zur neuen LUIS-App für die folgenden Absichten bereit:

|LUIS-Absichten für Basisbots|Beispieläußerung|
|--|--|
|Flugbuchung|`Travel to Paris`|
|Abbrechen|`bye`|
|GetWeather|`what's the weather like?`|
|Keine|Alles, was sich außerhalb der Domäne der App befindet.|

## <a name="test-the-bot-in-web-chat"></a>Testen des Bots im Webchat

1. Wählen Sie im Azure-Portal für den neuen Bot die Option **Test in Web Chat** (Test im Webchat) aus.
1. Geben Sie im Textfeld **Nachricht eingeben** den Text `Book a flight from Seattle to Berlin tomorrow` ein. Der Bot antwortet mit der Bestätigung, ob ein Flug gebucht werden soll.

    ![Screenshot des Azure-Portals, Eingeben des Texts „hello“](./media/bfv4-nodejs/ask-bot-question-in-portal-test-in-web-chat.png)

    Über die Testfunktionen können Sie Ihren Bot schnell testen. Laden Sie für umfangreichere Tests, einschließlich Debuggen, den Botcode herunter, und verwenden Sie Visual Studio.

## <a name="download-the-web-app-bot-source-code"></a>Herunterladen des Quellcodes für den Web-App-Bot
Um den Code des Web-App-Bots zu entwickeln, laden Sie den Code herunter, und verwenden Sie ihn auf Ihrem lokalen Computer.

1. Wählen Sie im Azure-Portal im Abschnitt **Bot Management** (Botverwaltung) **Erstellen** aus.

1. Wählen Sie **Bot-Quellcode herunterladen** aus.

    [![Herunterladen des Web-App-Bot-Quellcodes für den Basisbot](../../../includes/media/cognitive-services-luis/bfv4/download-code.png)](../../../includes/media/cognitive-services-luis/bfv4/download-code.png#lightbox)

1. Wenn im Popupdialogfeld die Frage **Include app settings in the downloaded zip file?** (App-Einstellungen in die heruntergeladene ZIP-Datei einfügen?) angezeigt wird, wählen Sie **Ja** aus. Dadurch werden die LUIS-Einstellungen bereitgestellt.

1. Wenn der Quellcode gezippt ist, wird in einer Meldung ein Link zum Herunterladen des Codes zur Verfügung gestellt. Wählen Sie den Link aus.

1. Speichern Sie die ZIP-Datei auf Ihrem lokalen Computer, und extrahieren Sie die Dateien. Öffnen Sie das Projekt in Visual Studio.

## <a name="review-code-to-send-utterance-to-luis-and-get-response"></a>Überprüfen des Codes zum Senden einer Äußerung an LUIS und Erhalten einer Antwort

1. Öffnen Sie zum Senden der Benutzeräußerung an den LUIS-Vorhersageendpunkt **dialogs > flightBookingRecognizer.js**. Das ist die Stelle, an der in den Bot eingegebene Benutzeräußerungen an LUIS gesendet werden. Die Antwort von LUIS wird über die Methode **executeLuisQuery** zurückgegeben.

    ````javascript
    class FlightBookingRecognizer {

        ...

        /**
         * Returns an object with preformatted LUIS results for the bot's dialogs to consume.
         * @param {TurnContext} context
         */
        async executeLuisQuery(context) {
            return await this.recognizer.recognize(context);
        }

        ...

    }
    ````

1. Unter **dialogs > mainDialog** wird die Äußerung erfasst und an „executeLuisQuery“ in der Methode „actStep“ gesendet.


    ````javascript
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.

    const { LuisRecognizer } = require('botbuilder-ai');

    class FlightBookingRecognizer {
        constructor(config) {
            const luisIsConfigured = config && config.applicationId && config.endpointKey && config.endpoint;
            if (luisIsConfigured) {
                this.recognizer = new LuisRecognizer(config, {}, true);
            }
        }

        get isConfigured() {
            return (this.recognizer !== undefined);
        }

        /**
         * Returns an object with preformatted LUIS results for the bot's dialogs to consume.
         * @param {TurnContext} context
         */
        async executeLuisQuery(context) {
            return await this.recognizer.recognize(context);
        }

        getFromEntities(result) {
            let fromValue, fromAirportValue;
            if (result.entities.$instance.From) {
                fromValue = result.entities.$instance.From[0].text;
            }
            if (fromValue && result.entities.From[0].Airport) {
                fromAirportValue = result.entities.From[0].Airport[0][0];
            }

            return { from: fromValue, airport: fromAirportValue };
        }

        getToEntities(result) {
            let toValue, toAirportValue;
            if (result.entities.$instance.To) {
                toValue = result.entities.$instance.To[0].text;
            }
            if (toValue && result.entities.To[0].Airport) {
                toAirportValue = result.entities.To[0].Airport[0][0];
            }

            return { to: toValue, airport: toAirportValue };
        }

        /**
         * This value will be a TIMEX. And we are only interested in a Date so grab the first result and drop the Time part.
         * TIMEX is a format that represents DateTime expressions that include some ambiguity. e.g. missing a Year.
         */
        getTravelDate(result) {
            const datetimeEntity = result.entities.datetime;
            if (!datetimeEntity || !datetimeEntity[0]) return undefined;

            const timex = datetimeEntity[0].timex;
            if (!timex || !timex[0]) return undefined;

            const datetime = timex[0].split('T')[0];
            return datetime;
        }
    }

    module.exports.FlightBookingRecognizer = FlightBookingRecognizer;
    ````
<a name="ask-bot-a-question-for-the-book-flight-intent"></a>

## <a name="use-the-bot-emulator-to-test-the-bot"></a>Verwenden des Bot-Emulators zum Testen des Bots

Stellen einer Frage zur Absicht „Flugbuchung“ im Bot

1. Starten Sie den Bot-Emulator, und wählen Sie **Open Bot** (Bot öffnen) aus.
1. Geben Sie im Popupdialogfeld **Open a bot** (Bot öffnen) die URL des Bots ein, z. B. `http://localhost:3978/api/messages`. Die Route `/api/messages` ist die Webadresse für den Bot.
1. Geben Sie die **Microsoft-App-ID** und das **Microsoft-App-Kennwort** ein, die sich in der Datei **env** im Stammverzeichnis des heruntergeladenen Botcodes befinden.

1. Geben Sie im Bot-Emulator `Book a flight from Seattle to Berlin tomorrow` ein. Sie erhalten dann die gleiche Antwort für den Basisbot, die Sie auch mit **Test in Web Chat** (Test im Webchat) erhalten haben.

    [![Antwort des Basisbots im Emulator](./media/bfv4-nodejs/ask-bot-emulator-a-question-and-get-response.png)](./media/bfv4-nodejs/ask-bot-emulator-a-question-and-get-response.png#lightbox)

1. Wählen Sie **Ja** aus. Der Bot antwortet mit einer Zusammenfassung seiner Aktionen.
1. Wählen Sie im Protokoll des Bot-Emulators die Zeile mit `Luis Trace` aus. Dadurch wird die JSON-Antwort von LUIS für die Absicht und die Entitäten der Äußerung angezeigt.

    [![Antwort des Basisbots im Emulator](./media/bfv4-nodejs/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png)](./media/bfv4-nodejs/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png#lightbox)


[!INCLUDE [Bot Information](../../../includes/cognitive-services-qnamaker-luis-bot-info.md)]

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich weitere [Beispiele](https://github.com/microsoft/botframework-solutions) mit Konversationsbots an.

> [!div class="nextstepaction"]
> [Erstellen einer Language Understanding-App mit einer benutzerdefinierten Motivdomäne](luis-quickstart-intents-only.md)