---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/31/2020
ms.author: trbye
ms.openlocfilehash: 005cf83508d25e8f44190e07336fbb4e444f8e6b
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400376"
---
In dieser Schnellstartanleitung wird Text unter Verwendung des [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) in synthetisierte Sprache umgewandelt. Der Sprachsynthesedienst bietet zahlreiche Optionen für synthetisierte Stimmen. Weitere Informationen finden Sie unter [Text-zu-Sprache](../../../language-support.md#text-to-speech). Nachdem Sie einige Voraussetzungen erfüllt haben, erfordert das Rendern synthetisierter Sprache über die Standardlautsprecher nur vier Schritte:
> [!div class="checklist"]
> * Erstellen eines Objekts vom Typ `SpeechConfig` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region
> * Erstellen eines Objekts vom Typ `SpeechSynthesizer` unter Verwendung des obigen Objekts `SpeechConfig`
> * Verwenden Sie das `SpeechSynthesizer`-Objekt, um den Text zu sprechen.
> * Überprüfen Sie das zurückgegebene `SpeechSynthesisResult`-Objekt auf Fehler.
