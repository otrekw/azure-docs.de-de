---
title: Übersicht über Sicherheits-Agents
description: In dieser Schnellstartanleitung erhalten Sie grundlegende Informationen zur Sicherheits-Agent-Architektur für die im Dienst „Azure Defender für IoT“ verwendeten Agents.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2021
ms.author: shhazam
ms.openlocfilehash: de8c52372b2ef92dcf2abe357a40ef8947439a8a
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2021
ms.locfileid: "103493958"
---
# <a name="quickstart-security-agent-reference-architecture"></a>Schnellstart: Sicherheits-Agent-Referenzarchitektur

Azure Defender für IoT bietet eine Referenzarchitektur für Sicherheits-Agents, die Sicherheitsdaten über IoT Hub protokollieren, verarbeiten, aggregieren und senden.

Sicherheits-Agents sind für den Einsatz in einer eingeschränkten IoT-Umgebung konzipiert und können in Bezug auf die Werte, die sie im Vergleich zu den von ihnen verbrauchten Ressourcen bereitstellen, sehr flexibel angepasst werden.

Sicherheits-Agents unterstützen die folgenden Features:

- Authentifizieren mit vorhandener Geräteidentität oder einer dedizierten Modulidentität. Weitere Informationen finden Sie unter  [Authentifizierungsmethoden des Sicherheits-Agents](concept-security-agent-authentication-methods.md).

- Sammeln von Rohdaten zu Sicherheitsereignissen aus dem zugrunde liegenden Betriebssystem (Linux, Windows). Weitere Informationen zu verfügbaren Sicherheitsdatensammlern finden Sie unter [für die IoT-Agent-Konfiguration](how-to-agent-configuration.md).

- Aggregieren von Rohdaten zu Sicherheitsereignissen in Nachrichten, die über IoT Hub gesendet werden.

- Remotekonfigurieren durch Verwendung des Modulzwillings **azureiotsecurity**. Weitere Informationen finden Sie unter [Konfigurieren eines Defender für IoT-Agents](how-to-agent-configuration.md).

Sicherheits-Agents von Defender für IoT werden als Open-Source-Projekte entwickelt und sind über GitHub verfügbar:

- [Defender für IoT – C-basierter Agent](https://github.com/Azure/Azure-IoT-Security-Agent-C)
- [Defender für IoT – C#-basierter Agent](https://github.com/Azure/Azure-IoT-Security-Agent-CS)

## <a name="prerequisites"></a>Voraussetzungen

Keine

## <a name="agent-supported-platforms"></a>Vom Agent unterstützte Plattformen

Defender für IoT bietet verschiedene Installer-Agents für 32-Bit- und 64-Bit-Windows sowie für 32-Bit- und 64-Bit-Linux. Stellen Sie anhand der folgenden Tabelle sicher, dass Sie den richtigen Agent-Installer für jedes Ihrer Geräte verwenden:

| Aufbau | Linux | Windows | Details |
|--|--|--|--|
| 32-Bit | C | C# |  |
| 64-Bit | C# oder C | C# | Wir empfehlen, den C-Agent für Geräte mit eingeschränkteren oder minimalen Geräteressourcen zu verwenden. |


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel erhalten Sie eine allgemeine Übersicht über die Architektur des Defender-IoT-Micro-Agents von Defender für IoT und die verfügbaren Installationsprogramme.

Um mit der Defender für IoT-Bereitstellung fortzufahren, lesen Sie die folgenden Artikel:

> [!div class="nextstepaction"]
> [Authentifizierungsmethoden des Sicherheits-Agents](concept-security-agent-authentication-methods.md)
