---
title: Worum handelt es sich bei Translator? - Translator
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
ms.openlocfilehash: 206e1bc86404fa41b84647c50482dbe63301c0af
ms.sourcegitcommit: ac5cbef0706d9910a76e4c0841fdac3ef8ed2e82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2020
ms.locfileid: "89425958"
---
# <a name="what-is-translator"></a>Worum handelt es sich bei Translator?

Azure Cognitive Services Translator ist ein Clouddienst für maschinelle Übersetzung und gehört zur [Azure Cognitive Services](https://docs.microsoft.com/azure/?pivot=products&panel=ai)-Familie kognitiver APIs zur Erstellung intelligenter Apps. Translator lässt sich einfach in Ihre Anwendungen, Websites, Tools und Lösungen integrieren. Mit dieser App können Benutzererlebnisse in [mehr als 70 Sprachen](languages.md) bereitgestellt werden. Darüber hinaus kann sie auf jeder Hardwareplattform mit beliebigem Betriebssystem für Textübersetzung verwendet werden.

## <a name="about-microsoft-translator"></a>Informationen zu Microsoft Translator

Azure Cognitive Services Translator ist ein cloudbasierter Dienst für die maschinelle Übersetzung. Translator kommt in zahlreichen Produkten und Diensten von Microsoft zum Einsatz und wird in Anwendungen und Workflows von Tausenden von Unternehmen auf der ganzen Welt genutzt, die mit ihren Inhalten ein globales Publikum erreichen möchten.

Von Translator unterstützte Sprachübersetzung ist ebenfalls über den [Microsoft Speech-Dienst](https://docs.microsoft.com/azure/cognitive-services/speech-service/) verfügbar. Sie vereint Funktionen der Sprachübersetzungs-API und von Custom Speech Service  in einem einheitlichen und vollständig anpassbaren Dienst. 

## <a name="language-support"></a>Sprachunterstützung

Microsoft Translator unterstützt mehrere Sprachen für Übersetzung, Transliteration, Spracherkennung und Wörterbücher. Eine vollständige Liste finden Sie unter [Sprach- und Regionsunterstützung für die Textübersetzungs-API](language-support.md). Sie können auch programmgesteuert mit der [REST-API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-languages) auf die Liste zugreifen.  

## <a name="microsoft-translator-neural-machine-translation"></a>Neuronale maschinelle Übersetzungen von Microsoft Translator

Neuronale maschinelle Übersetzungen (Neural Machine Translation, NMT) sind der neue Standard für hochwertige, KI-basierte Übersetzungen. Sie ersetzen die veraltete SMT-Technologie (Statistical Machine Translation, statistische maschinelle Übersetzung), die Mitte der 2010er Jahre Ihren Zenit erreicht hat.

Im Vergleich zu SMT liefert NMT bessere Übersetzungen – nicht nur im Hinblick auf die grundsätzliche Übersetzungsqualität, sondern auch im Hinblick auf Textfluss und Natürlichkeit. Der Hauptgrund für diesen Textfluss besteht darin, dass NMT bei der Übersetzung von Wörtern den gesamten Kontext eines Satzes berücksichtigt. Bei SMT wird dagegen nur der unmittelbare Kontext einiger weniger Wörter vor und nach jedem Wort berücksichtigt.

NMT-Modelle sind das Herzstück der API und für Endbenutzer nicht sichtbar. Der einzige wahrnehmbare Unterschied ist die höhere Übersetzungsqualität – insbesondere für Sprachen wie Chinesisch, Japanisch und Arabisch.

Weitere Informationen zur Funktionsweise von NMT finden Sie [hier](https://www.microsoft.com/en-us/translator/mt.aspx#nnt).

## <a name="language-customization"></a>Sprachanpassung

Benutzerdefinierter Translator ist eine Erweiterung des Translator-Diensts und kann in Verbindung mit Translator verwendet werden, um das neuronale Übersetzungssystem anzupassen und die Übersetzung für Ihre spezifische Terminologie und Ihren individuellen Stil anzupassen.

Mit Custom Translator können Sie Übersetzungssysteme erstellen, die Ihre unternehmens- oder branchenspezifische Terminologie verwenden. Mithilfe des Kategorieparameters können Sie Ihr benutzerdefiniertes Übersetzungssystem dann ganz einfach über den regulären Translator-Dienst in Ihre vorhandenen Anwendungen, Workflows und Websites sowie über verschiedene Gerätetypen hinweg integrieren.

Weitere Informationen zur [Sprachanpassung](customization.md)

## <a name="next-steps"></a>Nächste Schritte

- [Registrieren Sie sich](translator-text-how-to-signup.md) für einen Zugriffsschlüssel.
- Die [API-Referenz](https://docs.microsoft.com/azure/cognitive-services/Translator/reference/v3-0-reference) bietet die technische Dokumentation für die APIs.
- [Preisübersicht](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
