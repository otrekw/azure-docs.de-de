---
title: 'Schnellstart: Erkennen von Sprache aus einer Audiodatei – Speech-Dienst'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/28/2019
ms.author: erhopf
ms.openlocfilehash: e333b156eaf9c4b6e09e631513ea099018f0691b
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75466800"
---
In dieser Schnellstartanleitung wird das [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) verwendet, um Sprache aus einer Audiodatei zu erkennen. Nach der Erfüllung einiger Voraussetzungen sind für die Erkennung von Sprache aus einer Datei lediglich fünf Schritte erforderlich:
> [!div class="checklist"]
> * Erstellen eines Objekts vom Typ ````SpeechConfig```` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region
> * Erstellen eines Objekts vom Typ ````AudioConfig```` zum Angeben des Namens der WAV-Datei
> * Erstellen eines Objekts vom Typ ````SpeechRecognizer```` unter Verwendung der obigen Objekte ````SpeechConfig```` und ````AudioConfig````
> * Starten des Erkennungsprozesses für eine einzelne Äußerung unter Verwendung des Objekts ````SpeechRecognizer````
> * Überprüfen des zurückgegebenen Objekts ````SpeechRecognitionResult````
