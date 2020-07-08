---
title: Lokale Sicherheits-Agent-Konfiguration (C)
description: Erfahren Sie mehr über den Azure Security Center-Sicherheits-Agent, und zwar lokale Konfigurationen für C.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 2cf6a49b-5d35-491f-abc3-63ec24eb4bc2
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2019
ms.author: mlottner
ms.openlocfilehash: 842a69c27ceb0d56df5a7b49eb9922b88d8d4b32
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85206936"
---
# <a name="understanding-the-localconfigurationjson-file---c-agent"></a>Grundlegendes zur Datei „LocalConfiguration.json“ – C-Agent

Der Azure Security Center für IoT-Sicherheits-Agent verwendet Konfigurationen aus einer lokalen Konfigurationsdatei.
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

- Lesen Sie die [Übersicht](overview.md) über den Dienst „Azure Security Center für IoT“.
- Machen Sie sich mit der [Architektur](architecture.md) von Azure Security Center für IoT vertraut.
- Aktivieren Sie den [Dienst](quickstart-onboard-iot-hub.md) „Azure Security Center für IoT“.
- Lesen Sie [Häufig gestellte Fragen (FAQ)](resources-frequently-asked-questions.md) zum Dienst „Azure Security Center für IoT“.
- Informieren Sie sich, wie Sie auf [Sicherheitsrohdaten](how-to-security-data-access.md) zugreifen.
- Machen Sie sich mit [Empfehlungen](concept-recommendations.md) vertraut.
- Machen Sie sich mit [Sicherheitswarnungen](concept-security-alerts.md) vertraut.
