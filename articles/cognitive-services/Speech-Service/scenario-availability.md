---
title: 'Szenarioverfügbarkeit: Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Das Speech SDK unterstützt zahlreiche Szenarien für eine Vielzahl von Programmiersprachen und Umgebungen. Nicht alle Szenarien sind in allen Programmiersprachen oder in allen Umgebungen verfügbar. Die Verfügbarkeit der einzelnen Szenarien ist nachfolgend aufgeführt.
services: cognitive-services
author: chrisbasoglu
manager: xdh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: cbasoglu
ms.openlocfilehash: 02632e5bc84c7406ce3a42eb4be71cf6aaf75c15
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74075757"
---
# <a name="scenario-availability"></a>Szenarioverfügbarkeit

Das Speech SDK unterstützt zahlreiche Szenarien für eine Vielzahl von Programmiersprachen und Umgebungen. Nicht alle Szenarien sind in allen Programmiersprachen oder in allen Umgebungen verfügbar. Die Verfügbarkeit der einzelnen Szenarien ist nachfolgend aufgeführt.

- **Spracherkennung (SR), Begriffsliste, Absicht, Übersetzung und lokale Container**
  - Alle Programmiersprachen/Umgebungen mit einem Pfeillink <img src="media/index/link.jpg" height="15" width="15"></img> in der Schnellstarttabelle [hier](https://aka.ms/csspeech).
- **Sprachsynthese (TTS)**
  - C++/Windows und Linux
  - C#/Windows, UWP und Unity
  - Java (JRE und Android)
  - Python
  - Swift
  - Objective-C
  - Die TTS-REST-API kann in allen anderen Situationen verwendet werden.
- **Schlüsselworterkennung**
  - C++/Windows und Linux
  - C#/Windows und Linux
  - Python/Windows und Linux
  - Java/Windows und Linux und Android (Speech Devices SDK)
  - Die Funktionalität zur Schlüsselworterkennung (Keyword Spotting, KWS) kann eventuell mit allen Mikrofontypen verwendet werden, offiziell wird KWS derzeit jedoch nur für die Mikrofonarrays in der Azure Kinect DK-Hardware oder im Speech-Geräte-SDK unterstützt.
- **Sprachassistenten**
  - C++/Windows und Linux und macOS
  - C#/Windows
  - Java/Windows und Linux und macOS und Android (Speech Devices SDK)
- **Unterhaltungstranskription**
  - C++/Windows und Linux
  - C# (Framework und .NET Core)/Windows und UWP und Linux
  - Java/Windows und Linux und Android (Speech Devices SDK)
- **Callcentertranskription**
  - REST-API und kann in jeder Situation verwendet werden
- **Per Codec komprimierte Audioeingabe**
  - C++/Linux
  - C#/Linux
  - Java/Linux, Android und iOS
