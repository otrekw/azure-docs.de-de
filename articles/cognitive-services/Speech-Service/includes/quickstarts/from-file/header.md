---
title: 'Schnellstart: Erkennen von Sprache aus einer Audiodatei – Speech-Dienst'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/14/2020
ms.author: dapine
ms.openlocfilehash: 2b6270535c0cf69549a7412bd38d9207454e5500
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "76037911"
---
In dieser Schnellstartanleitung wird das [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) verwendet, um Sprache aus einer Audiodatei zu erkennen. Nach der Erfüllung einiger Voraussetzungen sind für die Erkennung von Sprache aus einer Datei nur wenige Schritte erforderlich:
> [!div class="checklist"]
> * Erstellen eines Objekts vom Typ `SpeechConfig` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region
> * Erstellen eines Objekts vom Typ `AudioConfig` zum Angeben des Namens der WAV-Datei
> * Erstellen eines Objekts vom Typ `SpeechRecognizer` unter Verwendung der obigen Objekte `SpeechConfig` und `AudioConfig`
> * Starten des Erkennungsprozesses für eine einzelne Äußerung unter Verwendung des Objekts `SpeechRecognizer`
> * Überprüfen des zurückgegebenen Objekts `SpeechRecognitionResult`
