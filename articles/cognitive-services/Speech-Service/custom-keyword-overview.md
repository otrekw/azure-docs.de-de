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
ms.openlocfilehash: d7eea031e50a0f54a57d8a9c857d52ca137f3283
ms.sourcegitcommit: 5a37753456bc2e152c3cb765b90dc7815c27a0a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/04/2020
ms.locfileid: "87760548"
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

## <a name="get-started-with-custom-keywords"></a>Erste Schritte mit benutzerdefinierten Schlüsselwörtern

* Grundlegende Verwendungs- und Entwurfsmuster finden Sie unter [Grundlagen zu benutzerdefinierten Schlüsselwörtern](custom-keyword-basics.md).
* Gewusst wie: [Sprachaktivierung für Ihr Produkt mit dem Speech SDK und C#](tutorial-voice-enable-your-bot-speech-sdk.md)

## <a name="choose-an-effective-keyword"></a>Auswählen eines effektiven Schlüsselworts

Das Erstellen eines effektiven Schlüsselworts ist wichtig, um sicherzustellen, dass Ihr Gerät konsistent und genau antwortet. Durch das Anpassen Ihres Schlüsselworts können Sie Ihr Gerät auf effektive Weise differenzieren und Ihr Branding stärken. Berücksichtigen Sie bei der Auswahl eines Schlüsselworts die folgenden Richtlinien:

> [!div class="checklist"]
> * Ihr Schlüsselwort sollte ein englisches Wort oder ein englischer Ausdruck sein.
> * Es sollte nicht länger als zwei Sekunden dauern, es auszusprechen.
> * Wörter mit 4 bis 7 Silben eignen sich am besten. So ist „Hey, Computer“ beispielsweise ein gutes Schlüsselwort. Nur „Hey“ ist ein schlechtes Aktivierungswort.
> * Bei Schlüsselwörtern sollten englische Ausspracheregeln befolgt werden.
> * Durch ein einzigartiges Wort oder ein Kunstwort, das die allgemeinen englischen Ausspracheregeln befolgt, könnten Ergebnisse mit falsch positiven Werten verringert werden. „Computerama“ würde sich beispielsweise gut als Schlüsselwort eignen.
> * Wählen Sie kein gängiges Wort aus. Die Wörter „eat“ und „go“ werden beispielsweise häufig in gewöhnlichen Konversationen gesagt. Sie könnten falsche Auslöser für Ihr Gerät sein.
> * Vermeiden Sie die Verwendung eines Schlüsselworts, das verschiedene Aussprachevarianten haben könnte. Benutzern müsste die „richtige“ Aussprache bekannt sein, um eine Antwort von ihrem Gerät erhalten zu können. „509“ kann beispielsweise als „five zero nine“, „five oh nine“ oder als „five hundred and nine“ ausgesprochen werden. „R.E.I.“ kann als „r-e-i“ oder als „ray“ ausgesprochen werden. „Live“ kann „/līv/“ oder „/liv/“ ausgesprochen werden.
> * Verwenden Sie keine Sonderzeichen, Symbole oder Ziffern. „Go#“ und „20 + cats“ können problematische Schlüsselwörter sein. „Go sharp“ oder „twenty plus cats“ kann jedoch funktionieren. Sie können weiterhin die Symbole in Ihrem Branding sowie Marketing und Dokumentation zur Untermauerung der richtigen Aussprache verwenden.

> [!NOTE]
> Wenn Sie als Schlüsselwort ein markenrechtlich geschütztes Wort auswählen, müssen Sie Eigentümer dieser Marke sein oder über eine Berechtigung vom Eigentümer der Marke zur Verwendung des Worts verfügen. Microsoft haftet nicht für rechtliche Probleme, die möglicherweise durch Ihre Auswahl eines Schlüsselworts auftreten.

## <a name="see-samples-on-github"></a>Weitere Informationen finden Sie in den Beispielen auf GitHub

* [Erkennen von Schlüsselwörtern mit dem Speech SDK auf der Universellen Windows-Plattform mithilfe von C#](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/csharp/uwp/keyword-recognizer)
* [Erkennen von Schlüsselwörtern mit dem Speech SDK unter Android mithilfe von Java](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/java/android/keyword-recognizer)

## <a name="next-steps"></a>Nächste Schritte

* [Kostenloses Testen des Speech-Diensts](get-started.md)
* [Abrufen des Speech SDK](speech-sdk.md)
* [Weitere Informationen zu Sprach-Assistenten](voice-assistants.md)
