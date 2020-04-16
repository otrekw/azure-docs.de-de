---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/02/2020
ms.author: trbye
ms.openlocfilehash: 36243fd5db76a4706318f41b3e2cb3f557c17189
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400752"
---
In dieser Schnellstartanleitung verwenden Sie das [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) zur interaktiven Erkennung von Sprache aus Mikrofoneingaben und erstellen ein Texttranskript der erfassten Audioinhalte. Es ist ganz einfach, dieses Feature in Ihre Apps oder Geräte zu integrieren, um häufige Erkennungsaufgaben wie das Transkribieren von Konversationen zu auszuführen. Es kann auch für komplexere Integrationen verwendet werden, z. B. das Verwenden von Bot Framework mit dem Speech SDK zum Erstellen von Sprachassistenten.

Nach der Erfüllung einiger Voraussetzungen sind für die Spracherkennung über ein Mikrofon lediglich vier Schritte erforderlich:

> [!div class="checklist"]
> * Erstellen eines Objekts vom Typ `SpeechConfig` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region
> * Erstellen eines Objekts vom Typ `SpeechRecognizer` unter Verwendung des obigen Objekts `SpeechConfig`
> * Starten des Erkennungsprozesses für eine einzelne Äußerung unter Verwendung des Objekts `SpeechRecognizer`
> * Überprüfen des zurückgegebenen Objekts `SpeechRecognitionResult`
