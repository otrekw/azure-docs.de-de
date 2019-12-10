---
title: 'Schnellstart: Erkennen von Sprache aus einer Audiodatei – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/28/2019
ms.author: erhopf
ms.openlocfilehash: 2f7ec24667514fb131af29321a53f97210e58fc9
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74819424"
---
In dieser Schnellstartanleitung wird das [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) verwendet, um Sprache aus einer Audiodatei zu erkennen. Nach der Erfüllung einiger Voraussetzungen sind für die Erkennung von Sprache aus einer Datei lediglich fünf Schritte erforderlich:
> [!div class="checklist"]
> * Erstellen eines Objekts vom Typ ````SpeechConfig```` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region
> * Erstellen eines Objekts vom Typ ````AudioConfig```` zum Angeben des Namens der WAV-Datei
> * Erstellen eines Objekts vom Typ ````SpeechRecognizer```` unter Verwendung der obigen Objekte ````SpeechConfig```` und ````AudioConfig````
> * Starten des Erkennungsprozesses für eine einzelne Äußerung unter Verwendung des Objekts ````SpeechRecognizer````
> * Überprüfen des zurückgegebenen Objekts ````SpeechRecognitionResult````
