---
title: 'Schnellstart: Abrufen der Absicht mit REST-APIs – LUIS'
description: In dieser REST-API-Schnellstartanleitung bestimmen Sie mithilfe einer verfügbaren öffentlichen LUIS-App aus Konversationstext die Absicht eines Benutzers.
ms.topic: quickstart
ms.date: 02/03/2020
zone_pivot_groups: programming-languages-set-one
ms.openlocfilehash: 50d2161dc0fa738a70476fa3a9327d9d720e7c15
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2020
ms.locfileid: "76987771"
---
# <a name="quickstart-get-intent-with-rest-apis"></a>Schnellstart: Abrufen der Absicht mit REST-APIs

In dieser Schnellstartanleitung bestimmen Sie mithilfe einer verfügbaren öffentlichen LUIS-App aus Konversationstext die Absicht eines Benutzers. Sie senden die Absicht des Benutzers als Text an den HTTP-Vorhersageendpunkt der öffentlichen App. Auf dem Endpunkt wendet LUIS das Modell der öffentlichen App an, um den Text in natürlicher Sprache im Hinblick auf seine Bedeutung zu analysieren. Dabei werden die allgemeine Absicht bestimmt und relevante Daten für die Antragstellerdomäne der App extrahiert.

Diese Schnellstartanleitung verwendet die REST-API des Endpunkts. Weitere Informationen finden Sie in der [Dokumentation der Endpunkt-API](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78).

Für diesen Artikel benötigen Sie ein kostenloses [LUIS](https://www.luis.ai)-Konto.

<a name="create-luis-subscription-key"></a>

::: zone pivot="programming-language-csharp"

[!INCLUDE [Get intent with C# and REST](./includes/get-started-get-intent-rest-csharp.md)]

::: zone-end

::: zone pivot="programming-language-java"

[!INCLUDE [Get intent with Java and REST](./includes/get-started-get-intent-rest-java.md)]

::: zone-end

::: zone pivot="programming-language-python"

[!INCLUDE [Get intent with Python and REST](./includes/get-started-get-intent-rest-python.md)]

::: zone-end

::: zone pivot="programming-language-nodejs"

[!INCLUDE [Get intent with Node.js and REST](./includes/get-started-get-intent-rest-nodejs.md)]

::: zone-end

::: zone pivot="programming-language-go"

[!INCLUDE [Get intent with Go and REST](./includes/get-started-get-intent-rest-go.md)]

::: zone-end
