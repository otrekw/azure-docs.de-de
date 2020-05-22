---
title: Abrufen des V3-Vorhersageendpunkts
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 05/05/2020
ms.author: diberry
ms.openlocfilehash: 18f5422202cf972d49349cc04b845c623cabffa3
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83589131"
---
1. Kopieren Sie im LUIS-Portal im Abschnitt **Verwalten** (Menü rechts oben) auf der Seite **Azure-Ressourcen** (linkes Menü) auf der Registerkarte **Vorhersageressourcen** die **Beispielabfrage** am unteren Seitenrand.

    Fügen Sie die URL in einen neuen Browsertab ein.

    Die URL enthält Ihre App-ID, den Schlüssel und den Slotnamen. Die URL für den V3-Vorhersageendpunkt sieht wie folgt aus:

    `https://YOUR-RESOURCE-NAME.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/APP-ID/slots/SLOT-NAME/predict?subscription-key=YOUR-PREDICTION-KEY&<optional-name-value-pairs>&query=YOUR_QUERY_HERE`

