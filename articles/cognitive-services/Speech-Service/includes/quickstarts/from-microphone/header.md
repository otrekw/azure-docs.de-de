---
title: 'Schnellstart: Erkennen von Spracheingaben per Mikrofon – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 11/20/2019
ms.author: erhopf
ms.openlocfilehash: ab51fe0323ea23c16cb52aa7a0570f8d51d40b2f
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75467852"
---
In diesem Schnellstart verwenden Sie das [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) zur interaktiven Erkennung von Sprache aus Mikrofoneingaben und erstellen ein Texttranskript der erfassten Audioinhalte. Es ist ganz einfach, dieses Feature in Ihre Apps oder Geräte zu integrieren, um häufige Erkennungsaufgaben wie das Transkribieren von Konversationen zu auszuführen. Es kann auch für komplexere Integrationen verwendet werden, z. B. das Verwenden von Bot Framework mit dem Speech SDK zum Erstellen von Sprachassistenten.

Nach der Erfüllung einiger Voraussetzungen sind für die Spracherkennung über ein Mikrofon lediglich vier Schritte erforderlich:

> [!div class="checklist"]
> * Erstellen eines Objekts vom Typ `SpeechConfig` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region
> * Erstellen eines Objekts vom Typ `SpeechRecognizer` unter Verwendung des obigen Objekts `SpeechConfig`
> * Starten des Erkennungsprozesses für eine einzelne Äußerung unter Verwendung des Objekts `SpeechRecognizer`
> * Überprüfen des zurückgegebenen Objekts `SpeechRecognitionResult`
