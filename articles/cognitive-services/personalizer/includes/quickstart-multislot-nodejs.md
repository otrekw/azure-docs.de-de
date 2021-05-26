---
title: include file
description: Datei einfügen
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: include
ms.custom: cog-serv-seo-aug-2020
ms.date: 03/23/2021
ms.openlocfilehash: 551b6901b60e55e9496cf5c4483aa3a05dfd72a7
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110382272"
---
[Referenzdokumentation](/javascript/api/@azure/cognitiveservices-personalizer/) | [Multi-Slot-Konzept](..\concept-multi-slot-personalization.md) | [Beispiele](https://aka.ms/personalizer/ms-nodejs)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services)
* Die aktuelle Version von [Node.js](https://nodejs.org) und npm
* Sobald Sie über Ihr Azure-Abonnement verfügen, sollten Sie über <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer"  title="Erstellen einer Personalisierungsressource"  target="_blank"> im Azure-Portal eine Personalisierungsressource </a> erstellen, um Ihren Schlüssel und Endpunkt abzurufen. Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Personalisierungs-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    * Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.

## <a name="setting-up"></a>Einrichten

[!INCLUDE [Upgrade Personalizer instance to Multi-Slot](upgrade-personalizer-multi-slot.md)]

[!INCLUDE [Change model frequency](change-model-frequency.md)]

### <a name="create-a-new-nodejs-application"></a>Erstellen einer neuen Node.js-Anwendung

Erstellen Sie in einem Konsolenfenster (etwa cmd, PowerShell oder Bash) ein neues Verzeichnis für Ihre App, und rufen Sie es auf.

```console
mkdir myapp && cd myapp
```

Führen Sie den Befehl `npm init -y` aus, um die Datei `package.json` zu erstellen.

```console
npm init -y
```

Erstellen Sie in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE eine neue Node.js-Anwendung mit dem Namen `sample.js` sowie Variablen für den Endpunkt und Abonnementschlüssel Ihrer Ressource. 

[!INCLUDE [Personalizer find resource info](find-azure-resource-info.md)]

```javascript
const axios = require('axios');
const { v4: uuidv4 } = require('uuid');
const readline = require('readline-sync');
// The endpoint specific to your personalization service instance; 
// e.g. https://<your-resource-name>.cognitiveservices.azure.com
const PersonalizationBaseUrl = 'https://<REPLACE-WITH-YOUR-PERSONALIZER-ENDPOINT>.cognitiveservices.azure.com';
// The key specific to your personalization service instance; e.g. "0123456789abcdef0123456789ABCDEF"
const ResourceKey = '<REPLACE-WITH-YOUR-PERSONALIZER-KEY>';
```

### <a name="install-the-npm-packages-for-quickstart"></a>Installieren der NPM-Pakete für den Schnellstart

```console
npm install @azure/ms-rest-azure-js @azure/ms-rest-js readline-sync uuid axios --save
```

## <a name="object-model"></a>Objektmodell

Um das beste Element des Inhalts für jeden Slot zu erfragen, erstellen Sie eine [rankRequest] und senden Sie dann eine Postanforderung an den [multislot/rank]-Endpunkt (https://westus2.dev.cognitive.microsoft.com/docs/services/personalizer-api-v1-1-preview-1/operations/Rank). Die Antwort wird dann in eine [rankResponse] analysiert.

Um einen Belohnungsbewertung an den Personalizer zu senden, erstellen Sie eine [rewards] und senden dann eine Postanforderung an [multislot/events/{eventId}/reward](https://westus2.dev.cognitive.microsoft.com/docs/services/personalizer-api-v1-1-preview-1/operations/Events_Reward).

Im Rahmen dieser Schnellstartanleitung ist die Bestimmung der Relevanzbewertung ganz einfach. In einem Produktionssystem kann die Bestimmung der Einflussfaktoren für die [Relevanzbewertung](../concept-rewards.md) sowie deren jeweilige Gewichtung allerdings eine komplexe Angelegenheit sein und muss unter Umständen im Laufe der Zeit überarbeitet werden. Diese Entwurfsentscheidung sollte eine der wichtigsten Entscheidungen im Zusammenhang mit Ihrer Personalisierungsarchitektur sein.

## <a name="code-examples"></a>Codebeispiele

In diesen Codeausschnitten wird gezeigt, wie Sie die folgenden Aufgaben ausführen, indem Sie HTTP-Anforderungen für NodeJS senden:

* [Erstellen von Basis-URLs](#create-base-urls)
* [Rangfolge-API](#request-the-best-action)
* [Relevanz-API](#send-a-reward)


## <a name="create-base-urls"></a>Erstellen von Basis-URLs

In diesem Abschnitt führen Sie zwei Aktionen aus:
* Erstellen der Rangfolge- und Belohnungs-URLs
* Erstellen der Rang-/Belohnungsanforderungsheader

Erstellen Sie die Rangfolge-/Belohnungs-URLs mithilfe der Basis-URL und der Anforderungsheader mithilfe des Ressourcenschlüssels.

```javascript
const MultiSlotRankUrl = PersonalizationBaseUrl.concat('personalizer/v1.1-preview.1/multislot/rank');
const MultiSlotRewardUrlBase = PersonalizationBaseUrl.concat('personalizer/v1.1-preview.1/multislot/events/');
const Headers = {
    'ocp-apim-subscription-key': ResourceKey,
    'Content-Type': 'application/json'
};
```

## <a name="get-content-choices-represented-as-actions"></a>Abrufen von Inhaltsoptionen, dargestellt als Aktionen

Aktionen stellen die Inhaltsoptionen dar, aus denen die Personalisierung das beste Inhaltselement auswählen soll. Fügen Sie dem Skript die folgenden Methoden hinzu, um die Gruppe der Aktionen und deren Features darzustellen. 

```javascript
function getActions() {
    return [
        {
            'id': 'Red-Polo-Shirt-432',
            'features': [
                {
                    'onSale': 'true',
                    'price': 20,
                    'category': 'Clothing'
                }
            ]
        },
        {
            'id': 'Tennis-Racket-133',
            'features': [
                {
                    'onSale': 'false',
                    'price': 70,
                    'category': 'Sports'
                }
            ]
        },
        {
            'id': '31-Inch-Monitor-771',
            'features': [
                {
                    'onSale': 'true',
                    'price': 200,
                    'category': 'Electronics'
                }
            ]
        },
        {
            'id': 'XBox-Series X-117',
            'features': [
                {
                    'onSale': 'false',
                    'price': 499,
                    'category': 'Electronics'
                }
            ]
        }
    ];
}
```

## <a name="get-user-preferences-for-context"></a>Abrufen von Benutzerwünschen für den Kontext

Fügen Sie dem Skript die folgenden Methoden hinzu, um die Eingabe eines Benutzers über die Befehlszeile für die Tageszeit und den Gerätetyp des Benutzers zu erhalten. Diese werden als Kontextfeatures verwendet.

```javascript
function getContextFeatures() {
    const timeOfDayFeatures = ['morning', 'afternoon', 'evening', 'night'];
    const deviceFeatures = ['mobile', 'tablet', 'desktop'];

    let answer = readline.question('\nWhat time of day is it (enter number)? 1. morning 2. afternoon 3. evening 4. night\n');
    let selection = parseInt(answer);
    const timeOfDay = selection >= 1 && selection <= 4 ? timeOfDayFeatures[selection - 1] : timeOfDayFeatures[0];

    answer = readline.question('\nWhat type of device is the user on (enter number)? 1. mobile 2. tablet 3. desktop\n');
    selection = parseInt(answer);
    const device = selection >= 1 && selection <= 3 ? deviceFeatures[selection - 1] : deviceFeatures[0];

    console.log('Selected features:\n');
    console.log('Time of day: ' + timeOfDay + '\n');
    console.log('Device: ' + device + '\n');

    return [
        {
            'time': timeOfDay
        },
        {
            'device': device
        }
    ];
}
```

## <a name="get-slots"></a>Slots abrufen

Slots machen die Seite aus, mit der der Benutzer interagiert. Personalizer entscheidet, welche Aktion in jedem der definierten Slots angezeigt werden soll. Aktionen können aus bestimmten Slots ausgeschlossen werden, die als `ExcludeActions` angezeigt werden. `BaselineAction` ist die Standardaktion für den Slot, die ohne die Verwendung von Personalizer angezeigt worden wäre.


Diese Schnellstartanleitung enthält einfache Slotfeatures. In Produktionssystemen kann die Bestimmung und [Auswertung](../concept-feature-evaluation.md) von [Merkmalen](../concepts-features.md) allerdings eine komplexe Angelegenheit sein.

```javascript
function getSlots() {
    return [
        {
            'id': 'BigHeroPosition',
            'features': [
                {
                    'size': 'large',
                    'position': 'left',
                }
            ],
            'excludedActions': ['31-Inch-Monitor-771'],
            'baselineAction': 'Red-Polo-Shirt-432'
        },
        {
            'id': 'SmallSidebar',
            'features': [
                {
                    'size': 'small',
                    'position': 'right',
                }
            ],
            'excludedActions': ['Tennis-Racket-133'],
            'baselineAction': 'XBox-Series X-117'
        }
    ];
}
```

## <a name="make-http-requests"></a>Übermitteln von HTTP-Anforderungen

Senden Sie Postanforderungen an den Personalizer-Endpunkt für das Einstufen nach Rangfolge und Belohnen von Aufrufen mit mehreren Slots.

```javascript
async function sendMultiSlotRank(rankRequest) {
    try {
        let response = await axios.post(MultiSlotRankUrl, rankRequest, { headers: Headers })
        return response.data;
    }
    catch (err) {
        console.log(err);
    }
}
```

```javascript
async function sendMultiSlotReward(rewardRequest, eventId) {
    try {
        let rewardUrl = MultiSlotRewardUrlBase.concat(eventId, '/reward');
        let response = await axios.post(rewardUrl, rewardRequest, { headers: Headers })
    }
    catch (err) {
        console.log(err);
    }
}
```

## <a name="get-feedback-for-personalizer-decisions"></a>Feedback zu Personalizer-Entscheidungen


Fügen Sie dem Skript die folgende Methode hinzu. Sie signalisieren, ob Personalizer über die Befehlszeileneingabeaufforderung eine gute Entscheidung für jeden Slot getroffen hat.

```javascript
function getRewardForSlot() {
    let answer = readline.question('\nIs this correct? (y/n)\n').toUpperCase();
    if (answer === 'Y') {
        console.log('\nGreat! The application will send Personalizer a reward of 1 so it learns from this choice of action for this slot.\n');
        return 1;
    }
    else if (answer === 'N') {
        console.log('\nYou didn\'t like the recommended item.The application will send Personalizer a reward of 0 for this choice of action for this slot.\n');
        return 0;
    }
    console.log('\nEntered choice is invalid. Service assumes that you didn\'t like the recommended item.\n');
    return 0;
}
```

## <a name="create-the-learning-loop"></a>Erstellen der Lernschleife

Die Lernschleife der Personalisierung ist ein Zyklus aus [Rangfolge-](#request-the-best-action) und [Relevanzaufrufen](#send-a-reward). In dieser Schnellstartanleitung folgt auf jeden Rangfolgeaufruf zur Personalisierung des Inhalts ein Relevanzaufruf, um der Personalisierung mitzuteilen, wie gut der Dienst abgeschnitten hat.

Der folgende Code durchläuft einen Zyklus durch eine Schleife: Der Benutzer wird an der Befehlszeile nach seinen Präferenzen befragt, die Angaben werden zur Wahl der besten Aktion für jeden Slot an die Personalisierung gesendet, die Auswahl wird dem Kunden in einer Auswahlliste angezeigt, und anschließend wird eine Relevanzbewertung an die Personalisierung gesendet, die angibt, wie gut die Wahl des Diensts war.

```javascript
runLoop = true;

(async () => {
    do {

        let multiSlotRankRequest = {};

        // Generate an ID to associate with the request.
        multiSlotRankRequest.eventId = uuidv4();

        // Get context information from the user.
        multiSlotRankRequest.contextFeatures = getContextFeatures();

        // Get the actions list to choose from personalization with their features.
        multiSlotRankRequest.actions = getActions();

        // Get the list of slots for which Personalizer will pick the best action.
        multiSlotRankRequest.slots = getSlots();

        multiSlotRankRequest.deferActivation = false;

        //Rank the actions for each slot
        try {
            var multiSlotRankResponse = await sendMultiSlotRank(multiSlotRankRequest);
        }
        catch (err) {
            console.log(err);
        }

        let multiSlotrewards = {};
        multiSlotrewards.reward = [];

        for (i = 0; i < multiSlotRankResponse.slots.length; i++) {
            console.log('\nPersonalizer service decided you should display: '.concat(multiSlotRankResponse.slots[i].rewardActionId, ' in slot ', multiSlotRankResponse.slots[i].id, '\n'));

            let slotReward = {};
            slotReward.slotId = multiSlotRankResponse.slots[i].id;
            // User agrees or disagrees with Personalizer decision for slot
            slotReward.value = getRewardForSlot();
            multiSlotrewards.reward.push(slotReward);
        }

        // Send the rewards for the event
        try {
            await sendMultiSlotReward(multiSlotrewards, multiSlotRankResponse.eventId);
        }
        catch (err) {
            console.log(err);
        }

        let answer = readline.question('\nPress q to break, any other key to continue:\n').toUpperCase();
        if (answer === 'Q') {
            runLoop = false;
        }

    } while (runLoop);
})()
```

Sehen Sie sich die Rangfolge- und Relevanzaufrufe in den folgenden Abschnitten genauer an.

Fügen Sie die folgenden Methoden hinzu, die [die Inhaltsauswahl abrufen](#get-content-choices-represented-as-actions), [die Benutzereinstellungen für den Kontext abrufen](#get-user-preferences-for-context), [die Slots abrufen](#get-slots), [HTTP-Anforderungen stellen](#make-http-requests) und [Belohnung für jeden Slot abrufen](#get-feedback-for-personalizer-decisions), bevor Sie die Codedatei ausführen:

* getActions
* getContextFeatures
* getSlots
* sendRank
* sendReward
* getRewardForSlot

## <a name="request-the-best-action"></a>Anfordern der besten Aktion

Für die Rangfolgeanforderung erfragt das Programm die Präferenzen des Benutzers, um Inhaltsoptionen zu erstellen. Der Anforderungstext enthält die Kontextfeatures, Aktionen und ihre Features, Slots und ihre Features sowie eine eindeutige Ereignis-ID, um die Antwort zu empfangen. Die `sendMultiSlotRank`-Methode benötigt rankRequest, um die Rangfolgeanforderung mit mehreren Slots zu senden.

In dieser Schnellstartanleitung werden die einfachen Kontextmerkmale „Tageszeit“ und „Gerätewunsch des Benutzers“ verwendet. In Produktionssystemen kann die Bestimmung und [Auswertung](../concept-feature-evaluation.md) von [Aktionen und Merkmalen](../concepts-features.md) allerdings eine komplexe Angelegenheit sein.

```javascript
let multiSlotRankRequest = {};

// Generate an ID to associate with the request.
multiSlotRankRequest.eventId = uuidv4();

// Get context information from the user.
multiSlotRankRequest.contextFeatures = getContextFeatures();

// Get the actions list to choose from personalization with their features.
multiSlotRankRequest.actions = getActions();

// Get the list of slots for which Personalizer will pick the best action.
multiSlotRankRequest.slots = getSlots();

multiSlotRankRequest.deferActivation = false;

//Rank the actions for each slot
try {
    var multiSlotRankResponse = await sendMultiSlotRank(multiSlotRankRequest);
}
catch (err) {
    console.log(err);
}
```

## <a name="send-a-reward"></a>Senden einer Relevanz

Um die Relevanzbewertung zu erhalten, die in der Belohnungsanforderung gesendet wird, ruft das Programm die Auswahl des Benutzers für jeden Slot über die Befehlszeile ab, weist der Auswahl einen numerischen Wert zu und sendet dann die eindeutige Ereignis-ID, Slot-ID und die Relevanzbewertung für jeden Slot als numerischen Wert an die `sendMultiSlotReward`-Methode. Eine Belohnung muss nicht für jeden Slot definiert werden.

In dieser Schnellstartanleitung wird als Relevanzbewertung eine einfache Zahl (0 oder 1) zugewiesen. In Produktionssystemen ist die Entscheidung, was wann an den [Relevanzaufruf](../concept-rewards.md) gesendet werden soll (abhängig von Ihren spezifischen Anforderungen), unter Umständen etwas komplizierter.

```javascript
let multiSlotrewards = {};
multiSlotrewards.reward = [];

for (i = 0; i < multiSlotRankResponse.slots.length; i++) {
    console.log('\nPersonalizer service decided you should display: '.concat(multiSlotRankResponse.slots[i].rewardActionId, ' in slot ', multiSlotRankResponse.slots[i].id, '\n'));

    let slotReward = {};
    slotReward.slotId = multiSlotRankResponse.slots[i].id;
    // User agrees or disagrees with Personalizer decision for slot
    slotReward.value = getRewardForSlot();
    multiSlotrewards.reward.push(slotReward);
}

// Send the rewards for the event
try {
    await sendMultiSlotReward(multiSlotrewards, multiSlotRankResponse.eventId);
}
catch (err) {
    console.log(err);
}
```

## <a name="run-the-program"></a>Ausführen des Programms

Führen Sie die Anwendung mit dem Node.js-Beispiel aus dem Anwendungsverzeichnis aus.

```console
node sample.js
```

![Das Schnellstartprogramm stellt ein paar Fragen zum Sammeln von Benutzereinstellungen – auch bekannt als Features – und stellt dann die am besten bewertete Aktion bereit.](../media/csharp-quickstart-commandline-feedback-loop/multislot-quickstart-program-feedback-loop-example-1.png)


Der [Quellcode für diese Schnellstartanleitung](https://aka.ms/personalizer/ms-nodejs) ist verfügbar.
