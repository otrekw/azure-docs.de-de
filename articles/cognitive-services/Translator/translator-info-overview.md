---
title: Was ist Translator? - Translator
titlesuffix: Azure Cognitive Services
description: Integrieren Sie Translator in Ihre Anwendungen, Websites, Tools und in andere Lösungen, um Benutzererlebnisse in mehreren Sprachen bereitzustellen.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: overview
ms.date: 05/26/2020
ms.author: swmachan
ms.custom: seodec18
ms.openlocfilehash: 7044954d2ba67e61172c08c0a77acaaaec3822c6
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83992673"
---
# <a name="what-is-the-translator"></a>Was ist Translator?

Translator lässt sich einfach in Ihre Anwendungen, Websites, Tools und Lösungen integrieren. Mit dieser API können Benutzererlebnisse in [mehr als 60 Sprachen](languages.md) bereitgestellt werden. Darüber hinaus kann sie auf jeder Hardwareplattform mit beliebigem Betriebssystem für Textübersetzung verwendet werden.

Translator gehört zur [Azure Cognitive Services](https://docs.microsoft.com/azure/?pivot=products&panel=ai)-Sammlung von Machine Learning- und KI-Algorithmen in der Cloud, die Sie direkt in Ihren Entwicklungsprojekten verwenden können.

## <a name="about-microsoft-translator"></a>Informationen zu Microsoft Translator

Translator ist ein cloudbasierter Dienst für die maschinelle Übersetzung. Das Herzstück dieses Diensts bildet der Translator-Dienst, der in zahlreichen Produkten und Diensten von Microsoft zum Einsatz kommt und in Anwendungen und Workflows von Tausenden von Unternehmen auf der ganzen Welt genutzt wird, die mit ihren Inhalten ein globales Publikum erreichen möchten.

Von Translator unterstützte Sprachübersetzung ist ebenfalls über den [Microsoft Speech-Dienst](https://docs.microsoft.com/azure/cognitive-services/speech-service/) verfügbar. Sie vereint Funktionen der Sprachübersetzungs-API und von Custom Speech Service  in einem einheitlichen und vollständig anpassbaren Dienst. Der Speech-Dienst ersetzt die Sprachübersetzungs-API, die am 15. Oktober 2019 eingestellt wird.

## <a name="language-support"></a>Sprachunterstützung

Microsoft Translator unterstützt mehrere Sprachen für Übersetzung, Transliteration, Spracherkennung und Wörterbücher. Eine vollständige Liste finden Sie unter [Sprach- und Regionsunterstützung für die Textübersetzungs-API](language-support.md). Sie können auch programmgesteuert mit der [REST-API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-languages) auf die Liste zugreifen.  

## <a name="microsoft-translator-neural-machine-translation"></a>Neuronale maschinelle Übersetzungen von Microsoft Translator

Neuronale maschinelle Übersetzungen (Neural Machine Translation, NMT) sind der neue Standard für hochwertige, KI-basierte Übersetzungen. Sie ersetzen die veraltete SMT-Technologie (Statistical Machine Translation, statistische maschinelle Übersetzung), die Mitte der 2010er Jahre Ihren Zenit erreicht hat.

Im Vergleich zu SMT liefert NMT bessere Übersetzungen – nicht nur im Hinblick auf die grundsätzliche Übersetzungsqualität, sondern auch im Hinblick auf Textfluss und Natürlichkeit. Der Hauptgrund für diesen Textfluss besteht darin, dass NMT bei der Übersetzung von Wörtern den gesamten Kontext eines Satzes berücksichtigt. Bei SMT wird dagegen nur der unmittelbare Kontext einiger weniger Wörter vor und nach jedem Wort berücksichtigt.

NMT-Modelle sind das Herzstück der API und für Endbenutzer nicht sichtbar. Der einzige wahrnehmbare Unterschied ist die höhere Übersetzungsqualität – insbesondere für Sprachen wie Chinesisch, Japanisch und Arabisch.

Weitere Informationen zur Funktionsweise von NMT finden Sie [hier](https://www.microsoft.com/en-us/translator/mt.aspx#nnt).

## <a name="language-customization"></a>Sprachanpassung

Benutzerdefinierter Translator ist eine Erweiterung des Microsoft Translator-Kerndiensts und kann in Verbindung mit Translator verwendet werden, um das neuronale Übersetzungssystem anzupassen und die Übersetzung für Ihre spezifische Terminologie und Ihren individuellen Stil anzupassen.

Mit Custom Translator können Sie Übersetzungssysteme erstellen, die Ihre unternehmens- oder branchenspezifische Terminologie verwenden. Mithilfe des Kategorieparameters können Sie Ihr benutzerdefiniertes Übersetzungssystem dann ganz einfach über den regulären Translator-Dienst in Ihre vorhandenen Anwendungen, Workflows und Websites sowie über verschiedene Gerätetypen hinweg integrieren.

Weitere Informationen zur [Sprachanpassung](customization.md)

## <a name="next-steps"></a>Nächste Schritte

- [Registrieren Sie sich](translator-text-how-to-signup.md) für einen Zugriffsschlüssel.
- Die [API-Referenz](https://docs.microsoft.com/azure/cognitive-services/Translator/reference/v3-0-reference) bietet die technische Dokumentation für die APIs.
- [Preisübersicht](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
