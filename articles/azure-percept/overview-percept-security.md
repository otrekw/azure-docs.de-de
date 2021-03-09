---
title: Übersicht über die Azure Percept-Sicherheit
description: Hier finden Sie weitere Informationen zur Sicherheit von Azure Percept.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: conceptual
ms.date: 02/18/2021
ms.custom: template-concept
ms.openlocfilehash: a08876cde9fac64c3a361b469049b4e33678a86f
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102098144"
---
# <a name="azure-percept-security-overview"></a>Übersicht über die Azure Percept-Sicherheit

Azure Percept DK-Geräte sind mit einem Hardware-Vertrauensanker ausgestattet. Somit verfügt jedes Gerät über zusätzliche integrierte Sicherheit. Dies trägt dazu bei, datenschutzrelevante Sensoren wie Kameras und Mikrofone sowie Rückschlussdaten zu schützen und ermöglicht die Authentifizierung und Autorisierung von Geräten für Azure Percept Studio-Dienste.

> [!NOTE]
> Azure Percept DK ist nur für die Verwendung in Entwicklungs- und Testumgebungen lizenziert.

## <a name="devices"></a>Geräte

### <a name="azure-percept-dk"></a>Azure Percept DK

Azure Percept DK verfügt über ein TPM (Trusted Platform Module) der Version 2.0, das eine besonders sichere Verbindung zwischen dem Gerät und Azure-Gerätebereitstellungsdiensten ermöglicht. TPM ist ein branchenweiter ISO-Standard von Trusted Computing Group. Weitere Informationen zu TPM finden Sie in der [vollständigen TPM 2.0-Spezifikation](https://trustedcomputinggroup.org/resource/tpm-library-specification/) oder der ISO/IEC 11889-Spezifikation. Weitere Informationen zur sicheren Gerätebereitstellung durch DPS finden Sie unter [TPM-Nachweis](https://docs.microsoft.com/azure/iot-dps/concepts-tpm-attestation).

### <a name="azure-percept-system-on-module-som"></a>Azure Percept-SOM (System On Module)

Das Azure Percept DK-SOM (System On Module) für maschinelles Sehen und das Azure Percept-Audio-Zubehör-SOM enthalten beide einen Mikrocontroller (Micro Controller Unit, MCU), um den Zugriff auf die eingebetteten KI-Sensoren zu schützen. Bei jedem Start wird der KI-Beschleuniger durch die MCU-Firmware und unter Verwendung der DICE-Architektur (Device Identifier Composition Engine) mit Azure Percept Studio-Diensten authentifiziert und autorisiert. DICE unterteilt den Startvorgang in Ebenen und erstellt individuelle Geheimnisse für die jeweilige Ebene und Konfiguration auf der Grundlage eines eindeutigen Gerätegeheimnisses (Unique Device Secret, UDS). Sollte an einem beliebigen Punkt in der Kette ein anderer Code oder eine andere Konfiguration gestartet werden, stimmen die Geheimnisse nicht überein. Weitere Informationen zu DICE finden Sie in der [Spezifikation der DICE-Arbeitsgruppe](https://trustedcomputinggroup.org/work-groups/dice-architectures/). Informationen zum Konfigurieren des Zugriffs auf Azure Percept Studio und auf erforderliche Dienste finden Sie weiter unten im Abschnitt **Konfigurieren von Firewalls für Azure Percept DK**.

Von Azure Percept-Geräten wird der Hardware-Vertrauensanker verwendet, um die Firmware zu schützen. Das Start-ROM gewährleistet die Integrität der Firmware zwischen ROM und Betriebssystemladeprogramm, was wiederum die Integrität der anderen Softwarekomponenten gewährleistet. Dadurch entsteht eine Vertrauenskette.

## <a name="services"></a>Dienste

### <a name="iot-edge"></a>IoT Edge

Von Azure Percept DK wird unter Verwendung des TLS-Protokolls (Transport Layer Security) eine besonders sichere Verbindung mit Azure Percept Studio und anderen Azure-Diensten hergestellt. Bei Azure Percept DK handelt es sich um ein Gerät mit Azure IoT Edge-Unterstützung. Die IoT Edge-Runtime ist eine Sammlung von Programmen, die aus einem Gerät ein IoT Edge-Gerät machen. Die Sammlung der IoT Edge-Runtime-Komponenten ermöglicht IoT Edge-Geräten den Empfang von Code für die Ausführung im Edgebereich und das Kommunizieren der Ergebnisse. Azure Percept DK nutzt Docker-Container, um IoT Edge-Workloads vom Hostbetriebssystem und von Anwendungen mit Edge-Unterstützung zu isolieren. Weitere Informationen zum Azure IoT Edge-Sicherheitsframework finden Sie in der Dokumentation zu [IoT Edge Security Manager](https://docs.microsoft.com/azure/iot-edge/iot-edge-security-manager?view=iotedge-2018-06).

### <a name="device-update-for-iot-hub"></a>Device Update für IoT Hub

Device Update für IoT Hub ermöglicht sicherere, skalierbare und zuverlässige OTA-Updates (Over the Air; per Funk) und bietet somit erneuerbare Sicherheit für Azure Percept-Geräte. Hierzu stehen umfassende Verwaltungsfunktionen sowie Informationen zur Updatekonformität zur Verfügung. Azure Percept DK beinhaltet eine vorab integrierte Geräteupdatelösung für resiliente Updates (A/B) von -Firmware- bis hin zu Betriebssystemebenen.

<!---I think the below topics need to be somewhere else, (i.e. not on the main page)
--->

## <a name="configuring-firewalls-for-azure-percept-dk"></a>Konfigurieren von Firewalls für Azure Percept DK

Falls Verbindungen von Azure Percept DK-Geräten in Ihrem Netzwerksetup explizit zugelassen werden müssen, sehen Sie sich die folgende Komponentenliste an.

Diese Prüfliste ist ein Ausgangspunkt für Firewallregeln:

|URL (* = Platzhalter) |Ausgehende TCP-Ports|    Verwendung|
|-------------------|------------------|---------|
|*.auth.azureperceptdk.azure.net|   443|    Authentifizierung und Autorisierung für Azure DK-SOM|
|*.auth.projectsantacruz.azure.net| 443|    Authentifizierung und Autorisierung für Azure DK-SOM|

Machen Sie sich außerdem mit der Liste der [von Azure IoT Edge verwendeten Verbindungen](https://docs.microsoft.com/azure/iot-edge/production-checklist?view=iotedge-2018-06#allow-connections-from-iot-edge-devices) vertraut.

<!---
## Additional Recommendations for Deployment to Production

Azure Percept DK offers a great variety of security capabilities out of the box. In addition to those powerful security features included in the current release, Microsoft also suggests the following guidelines when considering production deployments:

- Strong physical protection of the device itself
- Ensuring data at rest encryption is enabled
- Continuously monitoring the device posture and quickly responding to alerts
- Limiting the number of administrators who have access to the device
--->


## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit den [verfügbaren Azure Percept-KI-Modellen](./overview-ai-models.md) vertraut.
