---
title: 'Schnellstart: Erkennen von Sprache, Absichten und Entitäten – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 1/02/2019
ms.author: erhopf
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: a833d39ab91cd803f066d707306a6ff648d37e8f
ms.sourcegitcommit: 51ed913864f11e78a4a98599b55bbb036550d8a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2020
ms.locfileid: "75660494"
---
In dieser Schnellstartanleitung verwenden Sie das [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) und den LUIS-Dienst (Language Understanding), um Absichten aus Audiodaten zu erkennen, die von einem Mikrofon aufgezeichnet wurden. Genauer gesagt, verwenden Sie das Speech SDK zum Erfassen der Sprachausgabe und eine vordefinierte Domäne von LUIS zur Absichtserkennung für die Gebäudeautomatisierung, beispielsweise zum Ein- und Ausschalten einer Lampe. 

Nach der Erfüllung einiger Voraussetzungen sind für die Erkennung von Sprache und Absichten über ein Mikrofon lediglich einige wenige Schritte erforderlich:

> [!div class="checklist"]
>
> * Erstellen eines Objekts vom Typ ````SpeechConfig```` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region
> * Erstellen eines ````IntentRecognizer````-Objekts mit dem ````SpeechConfig````-Objekt von weiter oben
> * Starten des Erkennungsprozesses für eine einzelne Äußerung unter Verwendung des Objekts ````IntentRecognizer````
> * Überprüfen des zurückgegebenen Objekts ````IntentRecognitionResult````
