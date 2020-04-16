---
title: Direct Line Speech (Speech-Dienst)
titleSuffix: Azure Cognitive Services
description: Ein Überblick über die Features, Funktionen und Einschränkungen für Sprachassistenten, die Direct Line Speech mit dem Speech Software Development Kit (SDK) verwenden.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: trbye
ms.openlocfilehash: 6dc125b68af8f9a07731aaae9d858c19c51f934f
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81402393"
---
# <a name="what-is-direct-line-speech"></a>Was ist Direct Line Speech?

**Direct Line Speech** ist eine robuste, End-to-End-Lösung zur Erstellung eines flexiblen, erweiterbaren Sprachassistenten. Es basiert auf dem Bot-Framework und seinem Direct Line Speech-Kanal, der für die Sprachein- und Sprachausgabeinteraktion mit Bots optimiert ist.

[Sprachassistenten](voice-assistants.md) hören den Benutzern zu und ergreifen als Reaktion eine Aktion, wobei sie dabei häufig antworten. Sie verwenden die [Spracherkennung](speech-to-text.md), um die Sprache des Benutzers zu erfassen, und ergreifen dann Maßnahmen zum in natürlicher Sprache verstandenen Text. Diese Aktion umfasst häufig die Sprachausgabe des Assistenten, die mit [Sprachsynthese](text-to-speech.md) generiert wurde.

Direct Line Speech bietet ein Höchstmaß an Anpassungsmöglichkeiten und Vielseitigkeit für Sprachassistenten. Es ist für interaktive Szenarien konzipiert, die offen, natürlich oder eine Kombination aus beidem sind und Aufgabenerledigung oder die Verwendung von Befehl-und-Steuerung (Command-and-Control) bieten. Dieser hohe Grad an Flexibilität ist mit einer höheren Komplexität verbunden, und Szenarien, die auf klar definierte Aufgaben unter Verwendung natürlicher Sprache ausgerichtet sind, können [Benutzerdefinierte Befehle (Vorschau)](custom-commands.md) für eine optimierte Benutzererfahrung berücksichtigen.

## <a name="getting-started-with-direct-line-speech"></a>Erste Schritte mit Direct Line Speech

Die ersten Schritte zum Erstellen eines Sprachassistenten mit Direct Line Speech sind das [Abrufen eines Speech-Abonnementschlüssels](get-started.md), das Erstellen eines neuen, diesem Abonnement zugeordneten Bots und das Verbinden des Bot mit dem Direct Line Speech-Kanal.

   ![Konzeptionelle Darstellung des Orchestrierungsdienst-Flows von Direct Line Speech](media/voice-assistants/overview-directlinespeech.png "Flow des Speech-Kanals")

Eine vollständige, schrittweise Anleitung zur Erstellung eines einfachen Sprachassistenten mit Direct Line Speech finden Sie in dem [Tutorial zur Sprachaktivierung Ihres Bot mit dem Speech SDK und dem Direct Line Speech-Kanal](tutorial-voice-enable-your-bot-speech-sdk.md).

Wir bieten außerdem Schnellstarts an, mit denen Sie Code ausführen und die APIs schnell erlernen können. Diese Tabelle enthält eine Liste mit Schnellstarts für Sprachassistenten, die nach Sprache und Plattform sortiert sind.

| Schnellstart | Plattform | API-Referenz |
|------------|----------|---------------|
| C#, UWP | Windows | [Durchsuchen](https://aka.ms/csspeech/csharpref) |
| Java | Windows, macOS, Linux | [Durchsuchen](https://aka.ms/csspeech/javaref) |
| Java | Android | [Durchsuchen](https://aka.ms/csspeech/javaref) |

## <a name="sample-code"></a>Beispielcode

Beispielcode für die Erstellung eines Sprachassistenten ist auf GitHub verfügbar. Diese Beispiele decken die Clientanwendung für die Verbindung mit Ihrem Assistenten in mehreren gängigen Programmiersprachen ab.

* [Beispiele für Sprachassistenten (SDK)](https://aka.ms/csspeech/samples)
* [Tutorial: Sprachaktivierung für Ihren Assistenten mit dem Speech SDK, C#](tutorial-voice-enable-your-bot-speech-sdk.md)

## <a name="customization"></a>Anpassung

Sprachassistenten, die mithilfe des Speech-Diensts erstellt wurden, können die gesamte Bandbreite der Anpassungsoptionen nutzen, die für [Spracherkennung](speech-to-text.md), [Sprachsynthese](text-to-speech.md) und die [Auswahl benutzerdefinierter Stichwörter](speech-devices-sdk-create-kws.md) zur Verfügung stehen.

> [!NOTE]
> Die Anpassungsoptionen unterscheiden sich je nach Sprache/Gebietsschema. Weitere Informationen erhalten Sie unter [Unterstützte Sprachen](supported-languages.md).

Direct Line Speech und die damit verbundene Funktionalität für Sprachassistenten sind eine ideale Ergänzung zur [Lösung für virtuelle Assistenten](https://docs.microsoft.com/azure/bot-service/bot-builder-enterprise-template-overview) und der Unternehmensvorlage. Direct Line Speech kann zwar mit jedem kompatiblen Bot verwendet werden, diese Ressourcen bieten jedoch eine wiederverwendbare Baseline für qualitativ hochwertige Konversationsumgebungen sowie gemeinsame unterstützende Fähigkeiten und Modelle für einen schnellen Einstieg.

## <a name="reference-docs"></a>Referenz

* [Speech SDK](speech-sdk-reference.md)
* [Azure Bot Service](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)

## <a name="next-steps"></a>Nächste Schritte

* [Kostenloses Testen des Speech-Diensts](get-started.md)
* [Abrufen des Speech SDK](speech-sdk.md)
* [Erstellen und Bereitstellen eines Basisbots](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)
* [Abrufen der Lösung für virtuelle Assistenten und der Unternehmensvorlage](https://github.com/Microsoft/AI)
