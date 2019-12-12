---
title: Migrieren von der Bing-Spracheingabe zum Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie aus einem vorhandenen Bing-Spracheingabe-Abonnement zum Speech-Dienst von Azure Cognitive Services migrieren.
services: cognitive-services
author: wsturman
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: nitinme
ms.openlocfilehash: c0e1cc2fc1b3d4aed82c5442d2d3e23a1272fab5
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74805942"
---
# <a name="migrate-from-bing-speech-to-the-speech-service"></a>Migrieren von der Bing-Spracheingabe zum Speech-Dienst

Verwenden Sie diesen Artikel, um Ihre Anwendungen von der Bing-Spracheingabe-API zum Speech-Dienst zu migrieren.

In diesem Artikel werden die Unterschiede zwischen den Bing-Spracheingabe-APIs und dem Speech-Dienst beschrieben und Strategien für die Migration Ihrer Anwendungen vorgeschlagen. Ihr Abonnementschlüssel für die Bing-Spracheingabe-API funktioniert nicht mit dem Speech-Dienst. Sie benötigen ein neues Abonnement für den Speech-Dienst.

Mit einem einzelnen Abonnementschlüssel für den Speech-Dienst wird Zugriff auf die folgenden Funktionen gewährt. Da die Abrechnung jeweils separat erfolgt, werden Ihnen nur die genutzten Features berechnet.

* [Spracherkennung](speech-to-text.md)
* [Benutzerdefinierte Spracherkennung](https://cris.ai)
* [Sprachsynthese](text-to-speech.md)
* [Benutzerdefinierte Stimmen für Sprachsynthese](how-to-customize-voice-font.md)
* [Sprachübersetzung](speech-translation.md) (ohne [Textübersetzung](../translator/translator-info-overview.md))

Das [Sprach-SDK](speech-sdk.md) ist ein funktionaler Ersatz für die Clientbibliotheken der Bing-Spracheingabe. Es verwendet aber eine andere API.

## <a name="comparison-of-features"></a>Vergleich der Features

Der Speech-Dienst ähnelt weitgehend der Bing-Spracheingabe, wobei die folgenden Unterschiede bestehen.

Feature | Bing-Spracheingabe | Speech-Dienst | Details
-|-|-|-
C++ SDK | :heavy_minus_sign: | :heavy_check_mark: | Der Speech-Dienst unterstützt Windows und Linux.
Java-SDK | :heavy_check_mark: | :heavy_check_mark: | Der Speech-Dienst unterstützt Android- und Speech-Geräte.
C# SDK | :heavy_check_mark: | :heavy_check_mark: | Der Speech-Dienst unterstützt Windows 10, UWP (Universelle Windows-Plattform) und .NET Standard 2.0.
Kontinuierliche Spracherkennung | 10 Minuten | Unbegrenzt (mit SDK) | Die WebSockets-Protokolle der Bing-Spracheingabe und des Speech-Diensts unterstützen bis zu 10 Minuten pro Aufruf. Das Spracherkennungs-SDK verbindet sich jedoch bei einem Timeout oder wenn die Verbindung getrennt wird automatisch erneut.
Teil- oder Zwischenergebnisse | :heavy_check_mark: | :heavy_check_mark: | Mit WebSockets-Protokoll oder SDK.
Benutzerdefinierte Spracherkennungsmodelle | :heavy_check_mark: | :heavy_check_mark: | Die Bing-Spracheingabe erfordert ein separates Custom Speech-Abonnement.
Benutzerdefinierte Voicefonts | :heavy_check_mark: | :heavy_check_mark: | Die Bing-Spracheingabe erfordert ein separates Custom Voice-Abonnement.
24-KHz-Stimmen | :heavy_minus_sign: | :heavy_check_mark:
Erkennung der Absicht bei gesprochenen Inhalten | Erfordert separaten LUIS-API-Aufruf | Integriert (mit SDK) |  Sie können einen LUIS-Schlüssel mit dem Speech-Dienst verwenden.
Einfache Absichtserkennung | :heavy_minus_sign: | :heavy_check_mark:
Batch-Transkription bei langen Audiodateien | :heavy_minus_sign: | :heavy_check_mark:
Erkennungsmodus | Manuell über den Endpunkt-URI | Automatisch | Der Erkennungsmodus ist im Speech-Dienst nicht verfügbar.
Endpunktregion | Global | Länderspezifisch | Regionale Endpunkte verbessern die Latenz.
REST-APIs | :heavy_check_mark: | :heavy_check_mark: | Die REST-APIs des Speech-Diensts sind kompatibel mit der Bing-Spracheingabe (unterschiedlicher Endpunkt). REST-APIs unterstützen Sprachsynthese- und eingeschränkte Spracherkennungsfunktionen.
WebSockets-Protokolle | :heavy_check_mark: | :heavy_check_mark: | Die WebSockets-API des Speech-Diensts ist kompatibel mit der Bing-Spracheingabe (unterschiedlicher Endpunkt). Migrieren Sie nach Möglichkeit zum Spracherkennungs-SDK, um Ihren Code zu vereinfachen.
Dienst-zu-Dienst-API-Aufrufe | :heavy_check_mark: | :heavy_minus_sign: | Wird bei der Bing-Spracheingabe über die C#-Dienstbibliothek bereitgestellt.
Open Source-SDK | :heavy_check_mark: | :heavy_minus_sign: |

Der Speech-Dienst verwendet ein zeitbasiertes Preismodell (anstelle eines transaktionsbasierten Modells). Ausführlichere Informationen finden Sie unter [Preise für den Speech-Dienst](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

## <a name="migration-strategies"></a>Migrationsstrategien

Wenn Sie oder Ihr Unternehmen Anwendungen in der Entwicklungs- oder Produktionsumgebung nutzen, die eine Bing-Spracheingabe-API verwenden, sollten Sie diese aktualisieren, um den Speech-Dienst so schnell wie möglich zu nutzen. Informationen zu verfügbaren SDKs, Codebeispielen und Tutorials finden Sie in der [Dokumentation zum Speech-Dienst](index.md).

Die [REST-APIs](rest-apis.md) des Speech-Diensts sind mit den Bing-Spracheingabe-APIs kompatibel. Wenn Sie derzeit die Bing-Spracheingabe-REST-APIs verwenden, müssen Sie nur den REST-Endpunkt ändern und zu einem Abonnementschlüssel des Speech-Diensts wechseln.

Die WebSockets-Protokolle des Speech-Diensts sind auch mit den Protokollen der Bing-Spracheingabe kompatibel. Wir empfehlen bei neuen Entwicklungen die Verwendung des Speech SDK anstelle von WebSockets. Außerdem empfiehlt es sich, den vorhandenen Code zum SDK zu migrieren. Wie bei den REST-APIs erfordert jedoch bestehender Code, der die Bing-Spracheingabe via WebSockets verwendet, nur eine Änderung des Endpunkts und einen aktualisierten Schlüssel.

Wenn Sie eine Bing-Spracheingabe-Clientbibliothek für eine bestimmte Programmiersprache verwenden, erfordert die Migration zum [Spracherkennungs-SDK](speech-sdk.md) Änderungen an Ihrer Anwendung, weil es sich um eine andere API handelt. Das Spracherkennungs-SDK kann Ihren Code einfacher gestalten und Ihnen auch Zugriff auf neue Features bieten.

Das Spracherkennungs-SDK unterstützt derzeit C# ([mehr darüber hier](https://aka.ms/csspeech)), Java (Android und benutzerdefinierte Geräte), Objective C (iOS), C++ (Windows und Linux) und JavaScript. APIs sind auf den verschiedenen Plattformen ähnlich, was die Entwicklung für mehrere Plattformen erleichtert.

Der Speech-Dienst bietet keinen globalen Endpunkt. Ermitteln Sie, ob Ihre Anwendung effizient funktioniert, wenn sie einen einzigen regionalen Endpunkt für den gesamten Datenverkehr verwendet. Wenn nicht, verwenden Sie die Geolokalisierung, um den effizientesten Endpunkt zu bestimmen. Sie benötigen für jede verwendete Region ein separates Abonnement für den Speech-Dienst.

Wenn Ihre Anwendung langlebige Verbindungen verwendet und kein verfügbares SDK nutzen kann, können Sie eine WebSockets-Verbindung verwenden. Steuern Sie das Zeitlimit von 10 Minuten, indem Sie die Verbindung zum jeweils richtigen Zeitpunkt wiederherstellen.

Erste Schritte mit dem Spracherkennungs-SDK:

1. Laden Sie das [Spracherkennungs-SDK](speech-sdk.md) herunter.
1. Arbeiten Sie die [Leitfäden für den Schnellstart](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet) und die [Tutorials](how-to-recognize-intents-from-speech-csharp.md) für den Speech-Dienst durch. Sehen Sie sich auch die [Codebeispiele](samples.md) an, um sich mit den neuen APIs vertraut zu machen.
1. Aktualisieren Sie Ihre Anwendung, um den Speech-Dienst verwenden zu können.

## <a name="support"></a>Support

Kunden der Bing-Spracheingabe sollten sich an den Kundendienst wenden, indem sie ein [Supportticket](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) öffnen. Sie können sich auch an uns wenden, wenn Ihr Supportbedarf einen [technischen Supportplan](https://azure.microsoft.com/support/plans/) erfordert.

Informationen zur Unterstützung von Speech-Dienst, SDK und API finden Sie auf der [Supportseite](support.md) des Speech-Diensts.

## <a name="next-steps"></a>Nächste Schritte

* [Speech-Dienst kostenlos testen](get-started.md)
* [Schnellstart: Erkennen von Sprache in einer UWP-App mit dem Speech SDK](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=uwp)

## <a name="see-also"></a>Weitere Informationen
* [Versionshinweise zum Speech-Dienst](releasenotes.md)
* [Worum handelt es sich beim Spracherkennungsdienst?](overview.md)
* [Dokumentation zum Speech-Dienst und dem Speech SDK](speech-sdk.md#get-the-sdk)
