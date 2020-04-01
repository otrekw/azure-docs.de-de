---
title: Abrufen des V3-Vorhersageendpunkts
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/14/2019
ms.author: diberry
ms.openlocfilehash: 23e7f88b23ac7af7470183877eeb1be89e79779c
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80287746"
---
1. Kopieren Sie im LUIS-Portal im Abschnitt **Verwalten** (Menü rechts oben) auf der Seite **Azure-Ressourcen** (linkes Menü) auf der Registerkarte **Vorhersageressourcen** die **Beispielabfrage** am unteren Seitenrand.

    Fügen Sie die URL in einen neuen Browsertab ein.

    Die URL enthält Ihre App-ID, den Schlüssel und den Slotnamen. Die URL für den V3-Vorhersageendpunkt sieht wie folgt aus:

    `https://REGION.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/APP-ID/slots/SLOT-NAME/predict?subscription-key=YOUR-KEY&<optional-name-value-pairs>&query=YOUR_QUERY_HERE`

