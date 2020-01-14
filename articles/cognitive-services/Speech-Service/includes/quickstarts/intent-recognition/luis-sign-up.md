---
title: 'Schnellstart: Erkennen von Sprache, Absichten und Entitäten, Python – Speech-Dienst'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 12/27/2019
ms.author: erhopf
ms.openlocfilehash: aee5d6e1f6ed0519df7d1059b39f215d9f0d9091
ms.sourcegitcommit: 51ed913864f11e78a4a98599b55bbb036550d8a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2020
ms.locfileid: "75660497"
---
Um die Schnellstartanleitung zur Absichtserkennung durchzuführen, müssen Sie über das LUIS-Vorschauportal ein LUIS-Konto und ein Projekt erstellen. Für diese Schnellstartanleitung benötigen Sie nur ein LUIS-Abonnement. Ein Abonnement der Speech-Dienste ist nicht erforderlich.

Zuerst müssen Sie über das LUIS-Vorschauportal ein LUIS-Konto und eine App erstellen. Die erstellte LUIS-App verwendet eine vordefinierte Domäne für die Gebäudeautomatisierung, die Absichten, Entitäten und Beispieläußerungen bereitstellt. Am Ende dieser Schnellstartanleitung verfügen Sie über einen LUIS-Endpunkt in der Cloud, den Sie über das Speech SDK aufrufen können. 

Führen Sie die folgenden Schritte aus, um Ihre LUIS-App zu erstellen: 

* <a href="https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-get-started-create-app" target="_blank">Schnellstart: Erstellen einer App für eine vordefinierte Domäne</a>

Wenn Sie damit fertig sind, benötigen Sie drei Dinge: 

* Ihren LUIS-Schlüssel
* Ihre LUIS-Region
* Ihre LUIS-App-ID

Diese Informationen finden Sie im [LUIS-Vorschauportal](https://preview.luis.ai/):

1. Wählen Sie im LUIS-Vorschauportal **Verwalten** und dann **Azure-Ressourcen** aus. Auf dieser Seite finden Sie Ihren LUIS-Schlüssel und -Speicherort (zuweilen auch als _Region_ bezeichnet).  

   > [!div class="mx-imgBorder"]
   > ![LUIS-Schlüssel und -Speicherort](../../../media/luis/luis-key-region.png)

2. Nachdem Sie Schlüssel und Speicherort abgerufen haben, benötigen Sie die App-ID. Wählen Sie **Anwendungseinstellungen** aus. Auf dieser Seite finden Sie Ihre App-ID.

   > [!div class="mx-imgBorder"]
   > ![LUIS-App-ID](../../../media/luis/luis-app-id.png)