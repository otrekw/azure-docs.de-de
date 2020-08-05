---
title: 'Schnellstart: Abrufen der Absicht mit REST-APIs – LUIS'
description: In dieser REST-API-Schnellstartanleitung bestimmen Sie mithilfe einer verfügbaren öffentlichen LUIS-App aus Konversationstext die Absicht eines Benutzers.
ms.topic: quickstart
ms.date: 05/18/2020
ms.custom: tracking-python, devx-track-javascript
zone_pivot_groups: programming-languages-set-one
ms.openlocfilehash: 0274cf1ac6a6d0cb37d2321f6fd02f98c784318f
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87405268"
---
# <a name="quickstart-get-intent-with-rest-apis"></a>Schnellstart: Abrufen der Absicht mit REST-APIs

In dieser Schnellstartanleitung verwenden Sie eine LUIS-App, um die Absicht eines Benutzers aus Unterhaltungstext zu bestimmen. Sie senden die Absicht des Benutzers als Text an den HTTP-Vorhersageendpunkt der Pizza-App. Auf dem Endpunkt wendet LUIS das Modell der Pizza-App an, um den Text in natürlicher Sprache im Hinblick auf seine Bedeutung zu analysieren. Dabei werden die allgemeine Absicht bestimmt und relevante Daten für die Antragstellerdomäne der App extrahiert.

Für diesen Artikel benötigen Sie ein kostenloses [LUIS](https://www.luis.ai)-Konto.

<a name="create-luis-subscription-key"></a>

::: zone pivot="programming-language-csharp"
[!INCLUDE [Get intent with C# and REST](./includes/get-started-get-intent-rest-csharp.md)]
::: zone-end

::: zone pivot="programming-language-go"
[!INCLUDE [Get intent with Go and REST](./includes/get-started-get-intent-rest-go.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Get intent with Java and REST](./includes/get-started-get-intent-rest-java.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [Get intent with Node.js and REST](./includes/get-started-get-intent-rest-nodejs.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Get intent with Python and REST](./includes/get-started-get-intent-rest-python.md)]
::: zone-end
