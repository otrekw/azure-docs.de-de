---
title: Übersicht über Sicherheits-Agents
description: Informationen zum Verstehen, Konfigurieren, Bereitstellen und Verwenden der Sicherheitsdienst-Agents von Azure Defender für IoT auf Ihren IoT-Geräten.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 888fdbb2352c4ac972eac8b24c29d22ce973e04d
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931536"
---
# <a name="get-started-with-azure-defender-for-iot-device-security-agents"></a>Erste Schritte mit Azure Defender für IoT-Agents für Gerätesicherheit

Defender für IoT-Sicherheits-Agents bieten erweiterte Sicherheitsfunktionen wie etwa die Überwachung von Remoteverbindungen, aktiven Anwendungen, Anmeldeereignissen und bewährten Methoden für die Betriebssystemkonfiguration. Übernehmen Sie mit einem einzigen Dienst die Kontrolle über den Bedrohungsschutz und den Sicherheitsstatus Ihres Gerätefelds.

Sie erhalten eine Referenzarchitektur für Linux- und Windows-Sicherheits-Agents, die sowohl in C# als auch in C verfügbar ist.

Die Defender für IoT-Sicherheits-Agents verarbeiten unformatierte Ereigniserfassungen aus dem Betriebssystem des Geräts, ermöglichen eine Ereignisaggregation zur Kostensenkung und erlauben die Konfiguration über einen Modulzwilling des Geräts. Sicherheitsmeldungen werden über Ihre IoT Hub-Instanz an die Analysedienste von Defender für IoT gesendet.

Verwenden Sie den folgenden Workflow, um Ihre Defender für IoT-Sicherheits-Agents bereitzustellen und zu testen:

1. [Aktivieren des Defender für IoT-Diensts für Ihre IoT Hub-Instanz](quickstart-onboard-iot-hub.md)
1. Wenn für Ihre IoT Hub-Instanz keine Geräte registriert sind, [registrieren Sie ein neues Gerät](https://docs.microsoft.com/azure/iot-accelerators/quickstart-device-simulation-deploy).
1. [Erstellen Sie ein azureiotsecurity-Sicherheitsmodul](quickstart-create-security-twin.md) für Ihre Geräte.
1. Um den Agent auf einem simulierten Azure-Gerät anstatt auf einem echten Gerät zu installieren, [starten Sie eine neue Azure-VM](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal) in einer verfügbaren Zone.
1. [Stellen Sie einen Sicherheits-Agent von Defender für IoT auf Ihrem IoT-Gerät bereit](how-to-deploy-linux-cs.md), oder stellen Sie eine neue VM bereit.
1. Folgen Sie den Anweisungen für [trigger_events](https://aka.ms/iot-security-github-trigger-events), um eine harmlose Simulation eines Angriffs auszuführen.
1. Überprüfen Sie die Benachrichtigungen von Defender für IoT als Reaktion auf den im vorherigen Schritt simulierten Angriff. Beginnen Sie mit der Überprüfung erst fünf Minuten, nachdem Sie das Skript ausgeführt haben.
1. Gehen Sie [Benachrichtigungen](concept-security-alerts.md), [Empfehlungen](concept-recommendations.md) und die [ausführlichen Informationen zu Log Analytics](how-to-security-data-access.md) mit IoT Hub durch.

## <a name="next-steps"></a>Nächste Schritte

- [Quickstart: Configure your IoT solution](quickstart-configure-your-solution.md) (Schnellstart: Konfigurieren Ihrer IoT-Lösung)
- [Quickstart: Create an azureiotsecurity module twin](quickstart-create-security-twin.md) (Schnellstart: Erstellen eines azureiotsecurity-Modulzwillings)
- [Quickstart: Create custom alerts](quickstart-create-custom-alerts.md) (Schnellstart: Erstellen benutzerdefinierter Benachrichtigungen)
- [Select and deploy a security agent on your IoT device](how-to-deploy-agent.md) (Auswählen und Bereitstellen eines Sicherheits-Agents auf Ihrem IoT-Gerät)
