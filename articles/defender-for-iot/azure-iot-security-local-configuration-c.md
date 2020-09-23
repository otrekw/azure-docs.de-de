---
title: Lokale Sicherheits-Agent-Konfiguration (C)
description: Erfahren Sie mehr über die lokalen Konfigurationen des Defender für IoT-Sicherheits-Agents für C.
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
ms.date: 07/26/2019
ms.author: mlottner
ms.openlocfilehash: 7cc6886b5341d8bc8a82288ad8a2a699381a953c
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90930443"
---
# <a name="understanding-the-localconfigurationjson-file---c-agent"></a>Grundlegendes zur Datei „LocalConfiguration.json“ – C-Agent

Der Defender für IoT-Sicherheits-Agent verwendet Konfigurationen aus einer lokalen Konfigurationsdatei.
Der Sicherheits-Agent liest die Konfiguration einmal bei seinem Start.
Die Konfiguration in der lokalen Konfigurationsdatei enthält die Authentifizierungskonfiguration und andere Konfigurationen im Zusammenhang mit dem Agent.
Die Datei enthält Konfigurationen in „Schlüssel-Wert“-Paaren im JSON-Format, und die Konfigurationen werden bei der Installation des Agents aufgefüllt.

Die Datei befindet sich standardmäßig unter: „/var/ASCIoTAgent/LocalConfiguration.json“.

Änderungen an der Konfigurationsdatei erfolgen beim Neustart des Agents.

## <a name="security-agent-configurations-for-c"></a>Sicherheits-Agent-Konfigurationen für C

| Konfigurationsname | Mögliche Werte | Details |
|:-----------|:---------------|:--------|
| AgentId | GUID | Der eindeutige Agent-Bezeichner |
| TriggerdEventsInterval | ISO8601-Zeichenfolge | Scheduler-Intervall für die Sammlung von ausgelösten Ereignissen |
| ConnectionTimeout | ISO8601-Zeichenfolge | Zeitraum vor dem Ablauf der Verbindung zu IoT Hub |
| Authentifizierung | JsonObject | Authentifizierungskonfiguration. Dieses Objekt enthält alle Informationen, die für die Authentifizierung bei IoT Hub erforderlich sind. |
| Identity | „DPS“, „SecurityModule“, „Device“ | Authentifizierungsidentität – „DPS“, wenn die Authentifizierung über DPS erfolgt, „SecurityModule“, wenn sie über Anmeldeinformationen für das Sicherheitsmodul erfolgt, oder „Device“, wenn sie mit Geräteanmeldeinformationen erfolgt. |
| AuthenticationMethod | „SasToken“, „SelfSignedCertificate“ | Das Benutzergeheimnis zur Authentifizierung – Wählen Sie „SasToken“ aus, wenn das Benutzergeheimnis ein symmetrischer Schlüssel ist; wählen Sie „SelfSignedCertificate“ aus, wenn das Geheimnis ein selbstsigniertes Zertifikat ist.  |
| FilePath | Pfad zur Datei (Zeichenfolge) | Der Pfad zu der Datei, die das Authentifizierungsgeheimnis enthält. |
| HostName | Zeichenfolge | Der Hostname des Azure IoT Hubs. normalerweise <mein-hub>.azure-devices.net |
| deviceId | Zeichenfolge | Die ID des Geräts (wie in Azure IoT Hub registriert) |
| DPS | JsonObject | DPS-bezogene Konfigurationen |
| IDScope | Zeichenfolge | ID-Bereich von DPS |
| RegistrationId | Zeichenfolge  | Registrierungs-ID für DPS-Geräte |
| Protokollierung | JsonObject | Konfigurationen mit Agentprotokollierung |
| SystemLoggerMinimumSeverity | 0 < = Zahl < = 4 | Protokollmeldungen gleich und über diesem Schweregrad werden in „/var/log/syslog“ protokolliert („0“ ist der niedrigste Schweregrad). |
| DiagnosticEventMinimumSeverity | 0 < = Zahl < = 4 | Protokollmeldungen gleich und über diesem Schweregrad werden als Diagnoseereignisse gesendet („0“ ist der niedrigste Schweregrad). |

## <a name="security-agent-configurations-code-example"></a>Codebeispiel für Sicherheits-Agent-Konfigurationen

```json
{
    "Configuration" : {
        "AgentId" : "b97faf0a-0f57-471f-9dab-46a8e1764946",
        "TriggerdEventsInterval" : "PT2M",
        "ConnectionTimeout" : "PT30S",
        "Authentication" : {
            "Identity" : "Device",
            "AuthenticationMethod" : "SasToken",
            "FilePath" : "/path/to/my/SymmetricKey",
            "DeviceId" : "my-device",
            "HostName" : "my-iothub.azure-devices.net",
            "DPS" : {
                "IDScope" : "",
                "RegistrationId" : ""
            }
        },
        "Logging": {
            "SystemLoggerMinimumSeverity": 0,
            "DiagnoticEventMinimumSeverity": 2
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

- Lesen der [Übersicht](overview.md) über den Defender für IoT-Dienst
- Weitere Informationen zur Defender für IoT-[Architektur](architecture.md)
- Aktivieren des Defender für IoT-[Diensts](quickstart-onboard-iot-hub.md)
- Lesen der [Häufig gestellten Fragen](resources-frequently-asked-questions.md) über den Defender für IoT-Dienst
- Informieren Sie sich, wie Sie auf [Sicherheitsrohdaten](how-to-security-data-access.md) zugreifen.
- Machen Sie sich mit [Empfehlungen](concept-recommendations.md) vertraut.
- Machen Sie sich mit [Sicherheitswarnungen](concept-security-alerts.md) vertraut.
