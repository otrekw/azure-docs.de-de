---
title: 'Schnellstart: Erkennen von Spracheingaben per Mikrofon – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 11/20/2019
ms.author: erhopf
ms.openlocfilehash: a72597163a8c11cd8b515d052dc69992784e61e0
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74818822"
---
In diesem Schnellstart verwenden Sie das [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) zur interaktiven Erkennung von Sprache aus Mikrofoneingaben und erstellen ein Texttranskript der erfassten Audioinhalte. Es ist ganz einfach, dieses Feature in Ihre Apps oder Geräte zu integrieren, um häufige Erkennungsaufgaben wie das Transkribieren von Konversationen zu auszuführen. Es kann auch für komplexere Integrationen verwendet werden, z. B. das Verwenden von Bot Framework mit dem Speech SDK zum Erstellen von Sprachassistenten.

Nach der Erfüllung einiger Voraussetzungen sind für die Spracherkennung über ein Mikrofon lediglich vier Schritte erforderlich:

> [!div class="checklist"]
> * Erstellen eines Objekts vom Typ `SpeechConfig` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region
> * Erstellen eines Objekts vom Typ `SpeechRecognizer` unter Verwendung des obigen Objekts `SpeechConfig`
> * Starten des Erkennungsprozesses für eine einzelne Äußerung unter Verwendung des Objekts `SpeechRecognizer`
> * Überprüfen des zurückgegebenen Objekts `SpeechRecognitionResult`
