---
title: 'Schnellstart: Übersetzen von Sprache in Text – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 12/09/2019
ms.author: yulili
ms.openlocfilehash: d4781808ce8e80f62e86ce1d0c6db9c38b2636d0
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74981077"
---
In dieser Schnellstartanleitung wird gesprochene Sprache mithilfe des [Sprach-SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) interaktiv von einer Sprache in Text einer anderen Sprache übersetzt. Nach der Erfüllung einiger Voraussetzungen sind für die Übersetzung von Sprache in Text lediglich fünf Schritte erforderlich:
> [!div class="checklist"]
> * Erstellen Sie ein Objekt vom Typ ````SpeechConfig```` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region.
> * Aktualisieren Sie das ````SpeechConfig````-Objekt, um die Quell- und Zielsprachen anzugeben.
> * Erstellen Sie ein Objekt vom Typ ````TranslationRecognizer```` unter Verwendung des obigen ````SpeechConfig````-Objekts.
> * Starten des Erkennungsprozesses für eine einzelne Äußerung unter Verwendung des Objekts ````TranslationRecognizer````
> * Überprüfen des zurückgegebenen Objekts ````TranslationRecognitionResult````
