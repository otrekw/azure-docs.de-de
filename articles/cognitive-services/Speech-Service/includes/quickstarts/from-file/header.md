---
title: 'Schnellstart: Erkennen von Sprache aus einer Audiodatei – Speech-Dienst'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/14/2020
ms.author: trbye
ms.openlocfilehash: a7c91775411f100a92dfcb0a7199dd4b25f6eca4
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81400490"
---
In dieser Schnellstartanleitung wird das [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) verwendet, um Sprache aus einer Audiodatei zu erkennen. Nach der Erfüllung einiger Voraussetzungen sind für die Erkennung von Sprache aus einer Datei nur wenige Schritte erforderlich:
> [!div class="checklist"]
> * Erstellen eines Objekts vom Typ `SpeechConfig` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region
> * Erstellen eines Objekts vom Typ `AudioConfig` zum Angeben des Namens der WAV-Datei
> * Erstellen eines Objekts vom Typ `SpeechRecognizer` unter Verwendung der obigen Objekte `SpeechConfig` und `AudioConfig`
> * Starten des Erkennungsprozesses für eine einzelne Äußerung unter Verwendung des Objekts `SpeechRecognizer`
> * Überprüfen des zurückgegebenen Objekts `SpeechRecognitionResult`
