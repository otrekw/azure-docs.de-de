---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 05/04/2021
ms.author: trbye
ms.openlocfilehash: 2ffcb79bb2bb192566e7f6b7e0f933cacce84a77
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114593205"
---
Um die Schnellstartanleitung zur Absichtserkennung durchzuführen, müssen Sie über das LUIS-Vorschauportal ein LUIS-Konto und ein Projekt erstellen. Für diesen Schnellstart benötigen Sie ein LUIS-Abonnement [in einer Region, in der die Absichtserkennung verfügbar ist](/azure/cognitive-services/speech-service/regions#intent-recognition). Ein Abonnement der Speech-Dienste ist *nicht* erforderlich.

Zuerst müssen Sie über das LUIS-Vorschauportal ein LUIS-Konto und eine App erstellen. Die erstellte LUIS-App verwendet eine vordefinierte Domäne für die Gebäudeautomatisierung, die Absichten, Entitäten und Beispieläußerungen bereitstellt. Am Ende dieser Schnellstartanleitung verfügen Sie über einen LUIS-Endpunkt in der Cloud, den Sie über das Speech SDK aufrufen können. 

Führen Sie die folgenden Schritte aus, um Ihre LUIS-App zu erstellen:

* <a href="/azure/cognitive-services/luis/luis-get-started-create-app" target="_blank">Schnellstart: Erstellen einer App für eine vordefinierte Domäne</a>

Wenn Sie damit fertig sind, benötigen Sie vier Dinge:

* Erneute Veröffentlichung mit aktivierter **Sprachvorbereitung**
* Ihren **Primärschlüssel** für LUIS
* Ihren **Standort** für LUIS
* Ihre **App-ID** für LUIS

Diese Informationen finden Sie im [LUIS-Vorschauportal](https://preview.luis.ai/):

1. Wählen Sie im LUIS-Vorschauportal Ihre App und dann die Schaltfläche **Veröffentlichen** aus.

2. Wählen Sie den Slot **Produktion** aus. Wählen Sie bei Verwendung von `en-US` die Option **Einstellungen ändern** aus, und legen Sie die Option **Spracherkennungsvorbereitung** auf **Ein** fest. Wählen Sie anschließend die Schaltfläche **Veröffentlichen** aus.

    > [!IMPORTANT]
    > Die Verwendung von **Sprachvorbereitung** wird dringend empfohlen, da hierdurch die Genauigkeit der Spracherkennung verbessert wird.

    > [!div class="mx-imgBorder"]
    > ![Veröffentlichen von LUIS auf dem Endpunkt](../../../media/luis/publish-app-popup.png)

3. Wählen Sie im LUIS-Vorschauportal **Verwalten** und dann **Azure-Ressourcen** aus. Auf dieser Seite finden Sie Ihren LUIS-Schlüssel und -Speicherort (zuweilen auch als _Region_ bezeichnet) für Ihre LUIS-Vorhersageressource.

   > [!div class="mx-imgBorder"]
   > ![LUIS-Schlüssel und -Speicherort](../../../media/luis/luis-key-region.png)

4. Nachdem Sie Schlüssel und Speicherort abgerufen haben, benötigen Sie die App-ID. Wählen Sie **Einstellungen** aus. Ihre App-ID ist auf dieser Seite verfügbar.

   > [!div class="mx-imgBorder"]
   > ![LUIS-App-ID](../../../media/luis/luis-app-id.png)