---
title: Abrufen des V3-Vorhersageendpunkts
titleSuffix: Azure Cognitive Services
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: include
ms.date: 05/05/2020
ms.openlocfilehash: a607ad71915606d6046b4c71291a49b8641bdcc9
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91545628"
---
1. Kopieren Sie im LUIS-Portal im Abschnitt **Verwalten** (Menü rechts oben) auf der Seite **Azure-Ressourcen** (linkes Menü) auf der Registerkarte **Vorhersageressourcen** die **Beispielabfrage** am unteren Seitenrand.

    Fügen Sie die URL in einen neuen Browsertab ein.

    Die URL enthält Ihre App-ID, den Schlüssel und den Slotnamen. Die URL für den V3-Vorhersageendpunkt sieht wie folgt aus:

    `https://YOUR-RESOURCE-NAME.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/APP-ID/slots/SLOT-NAME/predict?subscription-key=YOUR-PREDICTION-KEY&<optional-name-value-pairs>&query=YOUR_QUERY_HERE`

