---
title: Lösungsarchitektur
description: Lernen Sie den Informationsfluss im Azure Defender für IoT-Dienst kennen.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: elazark
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/08/2020
ms.author: v-ekrieg
ms.openlocfilehash: 3fc695770350e5a60ae3da9ab1796da5cac99370
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91843412"
---
# <a name="azure-defender-for-iot-architecture"></a>Architektur von Azure Defender für IoT

In diesem Artikel wird die funktionale Systemarchitektur der Defender für IoT-Lösung erläutert.

## <a name="defender-for-iot-components"></a>Defender für IoT-Komponenten

Defender für IoT besteht aus den folgenden Komponenten:

- IoT Hub-Integration
- Geräte-Agents (optional)
- SDK zum Senden von Sicherheitsmeldungen
- Analytics-Pipeline

### <a name="defender-for-iot-workflows"></a>Defender für IoT-Workflows

Defender für IoT arbeitet in einem von zwei Featureworkflows: „Integriert“ und „Erweitert“

### <a name="built-in"></a>Integriert

Im Modus **Integriert** wird Defender für IoT aktiviert, wenn Sie in Ihrem IoT Hub die Option **Sicherheit** aktivieren. Durch Echtzeitüberwachung, Empfehlungen und Warnungen bietet der Modus „Integriert“ Gerätesichtbarkeit und beispiellose Sicherheit in einem Schritt. Der Modus „Integriert“ erfordert keine Agentinstallation auf Geräten, und er verwendet erweiterte Analysen für protokollierte Aktivitäten, um Ihr Feldgerät zu analysieren und zu schützen.

### <a name="enhanced"></a>Verbessert

Nach dem Aktivieren der Option **Sicherheit** in Ihrem IoT Hub und der Installation von Defender für IoT-Geräte-Agents auf Ihren Geräten erfassen, aggregieren und analysieren die Agents im Modus **Erweitert** Sicherheitsereignis-Rohdaten von Ihren Geräten. Sicherheitsereignis-Rohdaten können IP-Verbindungen, die Prozesserstellung, Benutzeranmeldungen und andere sicherheitsrelevante Informationen enthalten. Defender für IoT-Geräte-Agents verarbeiten auch die Ereignisaggregation, um hohe Netzwerkdurchsätze zu vermeiden. Die Agents sind in hohem Maße anpassbar, sodass Sie sie für bestimmte Aufgaben wie das Senden ausschließlich wichtiger Informationen mit dem dringlichsten SLA oder für das Aggregieren umfangreicher Sicherheitsinformationen und Kontextdaten zu größeren Segmenten verwenden und auf diese Weise höhere Servicekosten vermeiden können.

![Architektur von Defender für IoT](./media/architecture/azure-iot-security-architecture.png)

Geräte-Agents und andere Anwendungen verwenden das **Azure Security Center SDK zum Senden von Sicherheitsmeldungen**, um Sicherheitsinformationen an Azure IoT Hub zu senden. IoT Hub ruft diese Informationen ab und leitet sie an den Defender für IoT-Dienst weiter.

Sobald der Defender für IoT-Dienst aktiviert ist, versendet IoT Hub zusätzlich zu den weitergeleiteten Daten alle seine internen Daten für die Analyse durch Defender für IoT. Diese Daten umfassen Cloudvorgangsprotokolle von Geräten, Geräte-Identitäten und die Hub-Konfiguration. Alle diese Informationen tragen dazu bei, die Defender für IoT-Analysepipeline zu erstellen.

Die Defender für IoT-Analysepipeline erhält ferner zusätzliche Threat Intelligence-Streams aus verschiedenen Quellen innerhalb von Microsoft sowie von Microsoft-Partnern. Die gesamte Defender für IoT-Analysepipeline arbeitet mit sämtlichen für den Dienst erstellten Kundenkonfigurationen (wie benutzerdefinierte Warnungen und die Verwendung des SDK zum Versenden von Sicherheitsmeldungen).

Bei der Verwendung der Analysepipeline kombiniert Defender für IoT alle Informationsdatenströme, um direkt umsetzbare Empfehlungen und Warnungen zu generieren. Die Pipeline enthält sowohl benutzerdefinierte Regeln, die von Sicherheitsanalytikern und Experten erstellt wurden, als auch nach Abweichungen vom standardmäßigen Geräteverhalten suchende Machine Learning-Modelle und Risikoanalysen.

Defender für IoT-Empfehlungen und -Warnungen (Analysepipeline-Ausgabe) werden in den Log Analytics-Arbeitsbereich des einzelnen Kunden geschrieben. Durch die Einbeziehung der Ereignisrohdaten im Arbeitsbereich sowie der Warnungen und Empfehlungen werden eingehende Untersuchungen und Abfragen möglich, die die genauen Details der festgestellten verdächtigen Aktivitäten verwenden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie die grundlegende Architektur und den Workflow der Defender für IoT-Lösung kennengelernt. Weitere Informationen zu den Voraussetzungen sowie zum Einstieg in Ihre Sicherheitslösung in IoT Hub und zu deren Aktivierung finden Sie in den folgenden Artikeln:

- [ASC für IoT-Voraussetzungen](service-prerequisites.md)
- [Erste Schritte](getting-started.md)
- [Konfigurieren Ihrer IoT-Lösung](quickstart-configure-your-solution.md)
- [Enable security in IoT Hub (Aktivieren der Sicherheit in IoT Hub)](quickstart-onboard-iot-hub.md)
- [Häufig gestellte Fragen zu Defender für IoT](resources-frequently-asked-questions.md)
- [Defender für IoT-Sicherheitswarnungen](concept-security-alerts.md)
