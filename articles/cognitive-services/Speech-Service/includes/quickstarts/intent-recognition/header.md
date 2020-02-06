---
author: IEvangelist
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/27/2020
ms.author: dapine
ms.openlocfilehash: d59a55a9b6d57d90ac6ae893b05e1064c11eb58b
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2020
ms.locfileid: "76900354"
---
In dieser Schnellstartanleitung verwenden Sie das [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) und den LUIS-Dienst (Language Understanding), um Absichten aus Audiodaten zu erkennen, die von einem Mikrofon aufgezeichnet wurden. Genauer gesagt, verwenden Sie das Speech SDK zum Erfassen der Sprachausgabe und eine vordefinierte Domäne von LUIS zur Absichtserkennung für die Gebäudeautomatisierung, beispielsweise zum Ein- und Ausschalten einer Lampe. 

Nach der Erfüllung einiger Voraussetzungen sind für die Erkennung von Sprache und Absichten über ein Mikrofon lediglich einige wenige Schritte erforderlich:

> [!div class="checklist"]
>
> * Erstellen eines Objekts vom Typ `SpeechConfig` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region
> * Erstellen eines `IntentRecognizer`-Objekts mit dem `SpeechConfig`-Objekt von weiter oben
> * Starten des Erkennungsprozesses für eine einzelne Äußerung unter Verwendung des Objekts `IntentRecognizer`
> * Überprüfen des zurückgegebenen Objekts `IntentRecognitionResult`
