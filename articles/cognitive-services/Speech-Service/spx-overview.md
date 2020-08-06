---
title: Die Azure Speech-Befehlszeilenschnittstelle
titleSuffix: Azure Cognitive Services
description: Die Speech-Befehlszeilenschnittstelle ist ein Befehlszeilentool, mit dem der Speech-Dienst verwendet werden kann, ohne Code zu schreiben. Der Einrichtungsaufwand für die Speech-Befehlszeilenschnittstelle ist minimal, und Sie können sofort mit wichtigen Features des Speech-Diensts experimentieren, um zu ermitteln, ob die Anforderungen Ihrer Anwendungsfälle erfüllt werden können.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: trbye
ms.custom: devx-track-azurecli
ms.openlocfilehash: ab019250e03feb9a776d628c06e792b884252260
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87501813"
---
# <a name="what-is-the-speech-cli"></a>Worum handelt es sich bei der Speech-Befehlszeilenschnittstelle?

Die Speech-Befehlszeilenschnittstelle ist ein Befehlszeilentool, mit dem der Speech-Dienst verwendet werden kann, ohne Code zu schreiben. Der Einrichtungsaufwand für die Speech-Befehlszeilenschnittstelle ist minimal, und Sie können sofort mit wichtigen Features des Speech-Diensts experimentieren, um zu ermitteln, ob die Anforderungen Ihrer Anwendungsfälle erfüllt werden können. Innerhalb weniger Minuten können einfache Testworkflows wie etwa eine Batchspracherkennung auf der Grundlage eines Verzeichnisses mit Dateien oder eine Textsynthese für eine Sammlung von Zeichenfolgen aus einer Datei ausgeführt werden. Die Speech-Befehlszeilenschnittstelle kann jedoch nicht nur für einfache Workflows genutzt werden. Sie ist produktionsbereit und hochskalierbar, um umfangreichere Prozesse mit automatisierten Batches (`.bat`) oder Shellskripts auszuführen.

Über die Speech-Befehlszeilenschnittstelle steht zwar der Großteil der Hauptfeatures des Speech SDK zur Verfügung, einige erweiterte Features und Anpassungen sind in der Speech-Befehlszeilenschnittstelle allerdings vereinfacht. Im Anschluss erfahren Sie, wann die Speech-Befehlszeilenschnittstelle und wann das Speech SDK verwendet werden sollte.

Verwenden Sie in folgenden Fällen die Speech-Befehlszeilenschnittstelle:
* Sie möchten mit minimalem Einrichtungs- und ohne Programmieraufwand mit Features des Speech-Diensts experimentieren.
* Sie haben relativ einfache Anforderungen an eine Produktionsanwendung, von der der Speech-Dienst genutzt wird.

Verwenden Sie in folgenden Fällen das Speech SDK:
* Sie möchten Funktionen des Speech-Diensts innerhalb einer bestimmten Sprache oder Plattform (beispielsweise C#, Python oder C++) integrieren.
* Sie haben komplexe Anforderungen, für die unter Umständen erweiterte Service Requests erforderlich sind oder die die Entwicklung von benutzerdefiniertem Verhalten (einschließlich Antwortstreaming) erfordern.

## <a name="core-features"></a>Wichtige Funktionen

* Spracherkennung: Konvertieren Sie Sprache aus Audiodateien oder direkt über ein Mikrofon in Text, oder erstellen Sie ein Transkript einer aufgezeichneten Unterhaltung.

* Sprachsynthese: Konvertieren Sie Text in Sprache, und verwenden Sie als Eingabe entweder Textdateien oder direkte Befehlszeileneingaben. Passen Sie Merkmale der Sprachausgabe mithilfe von [SSML-Konfigurationen](speech-synthesis-markup.md) und [Standardstimmen oder neuronalen Stimmen](speech-synthesis-markup.md#standard-neural-and-custom-voices) an.

* Sprachübersetzung: Übersetzen Sie Audio aus einer Ausgangssprache in Text in einer Zielsprache.

* Ausführung in Azure-Computeressourcen: Senden Sie SPX-Befehle für die Ausführung in einer Azure-Remotecomputeressource unter Verwendung von `spx webjob`.

## <a name="get-started"></a>Erste Schritte

Informationen zu den ersten Schritten mit der Speech-Befehlszeilenschnittstelle finden Sie im Artikel [Erlernen der Grundlagen der Speech-Befehlszeilenschnittstelle](spx-basics.md). In diesem Artikel erfahren Sie, wie Sie einige einfache Befehle mit SPX ausführen, und es werden auch etwas komplexere Befehle zum Ausführen von Batchvorgängen für Spracherkennung und -synthese gezeigt. Nach der Lektüre des Grundlagenartikels sollten Sie ausreichend mit der SPX-Syntax vertraut sein, um einige benutzerdefinierte Befehle schreiben oder einfache Speech-Vorgänge automatisieren zu können.

## <a name="next-steps"></a>Nächste Schritte

- [Erlernen der Grundlagen der Speech-Befehlszeilenschnittstelle](spx-basics.md)
- Bei komplexeren Anwendungsfällen benötigen Sie das [Speech SDK](speech-sdk.md).
