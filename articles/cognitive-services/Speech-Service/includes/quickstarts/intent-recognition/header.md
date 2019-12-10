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
ms.date: 10/28/2019
ms.author: erhopf
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: a87ed9355a5939393fd5e20f395cc96f35e7f150
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74816082"
---
In dieser Schnellstartanleitung verwenden Sie das [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) zur interaktiven Erkennung von Sprache aus Audiodaten, die über ein Mikrofon erfasst wurden. Nach der Erfüllung einiger Voraussetzungen sind für die Spracherkennung über ein Mikrofon lediglich vier Schritte erforderlich:
> [!div class="checklist"]
>
> * Erstellen eines Objekts vom Typ ````SpeechConfig```` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region
> * Erstellen eines ````IntentRecognizer````-Objekts mit dem ````SpeechConfig````-Objekt von weiter oben
> * Starten des Erkennungsprozesses für eine einzelne Äußerung unter Verwendung des Objekts ````IntentRecognizer````
> * Überprüfen des zurückgegebenen Objekts ````IntentRecognitionResult````
