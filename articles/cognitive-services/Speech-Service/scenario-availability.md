---
title: 'Szenarioverfügbarkeit: Spracherkennungsdienst'
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
ms.openlocfilehash: a72e5055a3b0f858a732ec28eeab511b09dd450c
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400147"
---
# <a name="scenario-availability"></a>Szenarioverfügbarkeit

Das Speech SDK unterstützt zahlreiche Szenarien für eine Vielzahl von Programmiersprachen und Umgebungen. Nicht alle Szenarien sind in allen Programmiersprachen oder in allen Umgebungen verfügbar. Die Verfügbarkeit der einzelnen Szenarien ist nachfolgend aufgeführt.

- **Spracherkennung (SR), Begriffsliste, Absicht, Übersetzung und lokale Container**
  - C++/Windows und Linux und macOS
  - C# (Framework und .NET Core)/Windows, UWP, Unity, Xamarin, Linux und macOS
  - Java (JRE und Android)
  - JavaScript (Browser und NodeJS)
  - Python
  - Swift
  - Objective-C  
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
- **Konversation mit mehreren Geräten**
  - C++/Windows
  - C# (Framework und .NET Core)/Windows
- **Callcentertranskription**
  - REST-API und kann in jeder Situation verwendet werden
- **Per Codec komprimierte Audioeingabe**
  - C++/Linux
  - C#/Linux
  - Java/Linux, Android und iOS
