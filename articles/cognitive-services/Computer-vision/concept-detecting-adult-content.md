---
title: Nicht jugendfreie, anzügliche und gewaltverherrlichende Inhalte – maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Konzepte zur Erkennung nicht jugendfreier Inhalte in Bildern mithilfe der Maschinelles Sehen-API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 7e41eb0f6a61f7b195e251739ae93207c731cac5
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2020
ms.locfileid: "94535876"
---
# <a name="detect-adult-content"></a>Erkennen nicht jugendfreier Inhalte

Maschinelles Sehen kann nicht jugendfreie Inhalte in Bildern erkennen, damit Entwickler die Anzeige solcher Bilder in ihrer Software einschränken können. Inhaltsflags werden mit einem Wert von 0 bis 1 angewendet, damit Entwickler die Ergebnisse ihren eigenen Vorgaben entsprechend interpretieren können.

> [!NOTE]
> Viele dieser Funktionen werden auch vom Dienst [Azure Content Moderator](https://docs.microsoft.com/azure/cognitive-services/content-moderator/overview) angeboten. Diese Alternative kann als Lösung für strengere Szenarien zur Inhaltsmoderation eingesetzt werden, wie etwa für die Textmoderation oder Workflows für die Überprüfung durch Personen.

## <a name="content-flag-definitions"></a>Definitionen für Inhaltsflags

Innerhalb der Klassifizierung „Nicht jugendfrei“ gibt es mehrere verschiedene Kategorien:

- Als **Adult** (erwachsen) gekennzeichnete Bilder sind explizit sexueller Natur und stellen oft Nacktheit und sexuelle Handlungen dar.
- Als **Racy** (anzüglich) gekennzeichnete Bilder sind gemäß Definition sexuell suggestiv und enthalten häufig in sexueller Hinsicht weniger explizite Inhalte als Bilder, die als **Adult (erwachsen)** gekennzeichnet sind.
- **Gewaltverherrlichende** Bilder sind als Bilder definiert, die blutrünstige Szenen darstellen.

## <a name="use-the-api"></a>Verwenden der API

Mit der API [Bild analysieren](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f21b) können Sie nicht jugendfreie Inhalte erkennen. Wenn Sie den Wert von `Adult` zum **visualFeatures**-Abfrageparameter hinzufügen, gibt die API in ihrer JSON-Antwort drei boolesche Eigenschaften (`isAdultContent`, `isRacyContent` und `isGoryContent`) zurück. Die Methode gibt auch die entsprechenden Eigenschaften (`adultScore`, `racyScore` und `goreScore`) zurück, die Zuverlässigkeitsbewertungen zwischen 0 und 1 für jede Kategorie darstellen.

- [Schnellstart: Analysieren eines Bilds (.NET SDK)](./quickstarts-sdk/csharp-analyze-sdk.md)
- [Schnellstart: Analysieren eines Bilds (REST-API)](./quickstarts/csharp-analyze.md)
