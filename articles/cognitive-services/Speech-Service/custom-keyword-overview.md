---
title: Benutzerdefinierte Schlüsselwörter – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Ein Überblick über die Features, Funktionen und Einschränkungen für benutzerdefinierte Schlüsselwörter, die das Speech Software Development Kit (SDK) verwenden.
services: cognitive-services
author: hasyashah
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: hasshah
ms.openlocfilehash: dd5748cf8afe19a49e5ea406aea9b558432eeaf3
ms.sourcegitcommit: c8a0fbfa74ef7d1fd4d5b2f88521c5b619eb25f8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82802135"
---
# <a name="what-is-a-keyword"></a>Was ist ein Schlüsselwort?

Ein Schlüsselwort ist ein Wort oder ein kurzer Ausdruck, mit dem Ihr Produkt per Sprache aktiviert werden kann. „Hey Cortana“ ist z. B. das Schlüsselwort für den Cortana-Assistenten. Die Sprachaktivierung ermöglicht es Ihren Benutzern, völlig ohne Verwendung ihrer Hände mit Ihrem Produkt zu interagieren, indem sie einfach das Schlüsselwort aussprechen. Da Ihr Produkt kontinuierlich auf das Schlüsselwort lauscht, werden sämtliche Audiodaten lokal auf dem Gerät des Benutzers verarbeitet, bis eine Erkennung erfolgt, um sicherzustellen, dass seine Daten weitestgehend geschützt sind.

## <a name="core-features-of-custom-keyword"></a>Kernfunktionen von Custom Keyword

Mit den Features für Anpassung, Leistung und Integration von Custom Keyword können Sie die Sprachaktivierung so anpassen, dass sie der Vision Ihres Produkts und den Anforderungen der Benutzer am besten entspricht.

| Funktion | BESCHREIBUNG |
|----------|----------|
| Schlüsselwortanpassung | Als Erweiterung Ihrer Marke stärkt ein Schlüsselwort den Wert, den Sie bei Ihren Kunden aufgebaut haben. Im Custom Keyword-Portal von Speech Studio können Sie ein beliebiges Wort oder einen kurzen Ausdruck angeben, der Ihre Marke am besten repräsentiert. Sie können Ihr Schlüsselwort weiter personalisieren, indem Sie die richtigen Aussprachen wählen, die durch das generierte Schlüsselwortmodell berücksichtigt werden.
| Schlüsselwortüberprüfung | Wenn ein hohes Vertrauen dahingehend besteht, dass das Schlüsselwort lokal erkannt wurde, werden Audiodaten zur weiteren Überprüfung, ob ein Benutzer das Schlüsselwort gesagt hat, in die Cloud gesendet. Die Schlüsselwortüberprüfung bietet eine zusätzliche Sicherheitsebene, indem sie die Auswirkungen einer fehlerhaften lokalen Erkennung reduziert und die Privatsphäre der Benutzer schützt.
| Integration von Sprach-Assistent und Speech SDK | Über Custom Keyword in Speech Studio generierte Schlüsselwörter können einfach über das Speech SDK in Ihr Gerät oder Ihre Anwendung integriert werden. Verweisen Sie einfach das SDK auf das von Speech Studio bereitgestellte Schlüsselwortmodell, und Ihr Produkt wird per Sprache aktiviert, unterstützt durch eine Schlüsselwortüberprüfung. Sie können die Spracherlebnisse Ihres Produkts vervollständigen, indem Sie Ihren eigenen [Sprach-Assistenten](voice-assistants.md) erstellen.

## <a name="sample-code"></a>Beispielcode

Beispielcode für die Integration eines Schlüsselwortmodells ist auf GitHub verfügbar. Diese Beispiele decken die Clientanwendung für die Integration Ihres Schlüsselworts in mehrere gängige Programmiersprachen ab.

* [Tutorial: Sprachaktivierung für Ihr Produkt mit dem Speech SDK, C#](tutorial-voice-enable-your-bot-speech-sdk.md)
* [Beispiele für Sprachassistenten (SDK)](https://aka.ms/csspeech/samples)

## <a name="tutorial"></a>Lernprogramm

* Informationen zum [Erstellen eines benutzerdefinierten Schlüsselworts mithilfe von Speech Studio](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-create-kws).
* Informationen zur [Sprachaktivierung Ihres Produkts mit dem Speech SDK](tutorial-voice-enable-your-bot-speech-sdk.md).

## <a name="reference-docs"></a>Referenz

* [Benutzerdefinierte Benennungsrichtlinien für Schlüsselwörter](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-kws-guidelines)
* [Speech SDK](speech-sdk-reference.md)

## <a name="next-steps"></a>Nächste Schritte

* [Kostenloses Testen des Speech-Diensts](get-started.md)
* [Erstellen eines benutzerdefinierten Schlüsselworts](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-create-kws)
* [Abrufen des Speech SDK](speech-sdk.md)
* [Weitere Informationen zu Sprach-Assistenten](voice-assistants.md)
