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
ms.openlocfilehash: ceef604fe07a11be89376e26c6fecc49298ebacf
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107778857"
---
# <a name="detect-adult-content"></a>Erkennen nicht jugendfreier Inhalte

Maschinelles Sehen kann nicht jugendfreie Inhalte in Bildern erkennen, damit Entwickler die Anzeige solcher Bilder in ihrer Software einschränken können. Inhaltsflags werden mit einem Wert von 0 bis 1 angewendet, damit Entwickler die Ergebnisse ihren eigenen Vorgaben entsprechend interpretieren können.

> [!NOTE]
> Viele dieser Funktionen werden auch vom Dienst [Azure Content Moderator](../content-moderator/overview.md) angeboten. Diese Alternative kann als Lösung für strengere Szenarien zur Inhaltsmoderation eingesetzt werden, wie etwa für die Textmoderation oder Workflows für die Überprüfung durch Personen.

## <a name="content-flag-definitions"></a>Definitionen für Inhaltsflags

Die Klassifizierung „Nicht jugendfrei“ enthält mehrere verschiedene Kategorien:

- **Nicht jugendfreie** Bilder sind explizit sexueller Natur und zeigen oft Nacktheit und sexuelle Handlungen.
- **Anzügliche** Bilder sind gemäß Definition sexuell suggestiv und enthalten häufig in sexueller Hinsicht weniger explizite Inhalte als Bilder, die als **Nicht jugendfrei** gekennzeichnet sind.
- **Gewaltverherrlichende** Bilder zeigen Blut bzw. blutrünstige Szenen.

## <a name="use-the-api"></a>Verwenden der API

Mit der API [Bild analysieren](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-ga/operations/56f91f2e778daf14a499f21b) können Sie nicht jugendfreie Inhalte erkennen. Wenn Sie den Wert von `Adult` zum **visualFeatures**-Abfrageparameter hinzufügen, gibt die API in ihrer JSON-Antwort drei boolesche Eigenschaften (`isAdultContent`, `isRacyContent` und `isGoryContent`) zurück. Die Methode gibt auch die entsprechenden Eigenschaften (`adultScore`, `racyScore` und `goreScore`) zurück, die Zuverlässigkeitsbewertungen zwischen 0 und 1 für jede Kategorie darstellen.

- [Schnellstart: Verwenden der Clientbibliothek für maschinelles Sehen](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
