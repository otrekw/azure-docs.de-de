---
title: Benutzerdefinierte Sicherheitswarnungen für IoT Hub
description: Erfahren Sie mehr über anpassbare Sicherheitswarnungen und empfohlene Wartungsvorgänge unter Verwendung der Features und Dienste von Defender für IoT Hub.
ms.topic: conceptual
ms.date: 2/16/2021
ms.openlocfilehash: d7a58bcdb759c3f31290cc7930eba6ca52fcc17b
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104784729"
---
# <a name="defender-for-iot-hub-custom-security-alerts"></a>Defender für IoT Hub: benutzerdefinierte Sicherheitswarnungen

Defender für IoT analysiert Ihre IoT-Lösung ständig mithilfe von Advanced Analytics- und Threat Intelligence-Funktionen, um Sie vor schädlichen Aktivitäten zu warnen.

Es wird empfohlen, benutzerdefinierte Warnungen im Einklang mit Ihren Kenntnissen zum erwarteten Geräteverhalten zu erstellen, um sicherzustellen, dass Warnungen die effizientesten Indikatoren für potenzielle Gefährdung in Ihrer eigenen Organisationsbereitstellung und -landschaft bieten.

Sie können die Warnungen für Defender für IoT Hub in den folgenden Listen basierend auf dem erwarteten IoT Hub- Verhalten definieren. Weitere Informationen zum Anpassen der einzelnen Warnungen finden Sie unter [Erstellen von benutzerdefinierten Warnungen](quickstart-create-custom-alerts.md).

## <a name="built-in-custom-alerts-in-the-iot-hub"></a>Integrierte benutzerdefinierte Warnungen in IoT Hub

| severity | Warnungsname | Datenquelle | BESCHREIBUNG | Vorschlag zur Problemlösung |
|--|--|--|--|--|
| Niedrig | Benutzerdefinierte Warnung: Die Anzahl von Cloud-zu-Gerät-Nachrichten im AMQP-Protokoll liegt außerhalb des zulässigen Bereichs. | IoT Hub | Die Anzahl von Cloud-zu-Gerät-Nachrichten (AMQP-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs. |  |
| Niedrig | Benutzerdefinierte Warnung: Die Anzahl abgelehnter Cloud-zu-Gerät-Nachrichten im AMQP-Protokoll liegt außerhalb des zulässigen Bereichs. | IoT Hub | Die Anzahl der vom Gerät abgelehnten Cloud-zu-Gerät-Nachrichten (AMQP-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs. |  |
| Niedrig | Benutzerdefinierte Warnung: Die Anzahl von Gerät-zu-Cloud-Nachrichten im AMQP-Protokoll liegt außerhalb des zulässigen Bereichs. | IoT Hub | Die Menge der Gerät-zu-Cloud-Nachrichten (AMQP-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs. |  |
| Niedrig | Benutzerdefinierte Warnung: Die Anzahl direkter Methodenaufrufe liegt außerhalb des zulässigen Bereichs. | IoT Hub | Die Menge der direkten Methodenaufrufen innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs. |  |
| Niedrig | Benutzerdefinierte Warnung: Die Anzahl von Dateiuploads liegt außerhalb des zulässigen Bereichs. | IoT Hub | Die Menge von Dateiuploads innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs. |  |
| Niedrig | Benutzerdefinierte Warnung: Die Anzahl von Cloud-zu-Gerät-Nachrichten im HTTP-Protokoll liegt außerhalb des zulässigen Bereichs. | IoT Hub | Die Menge der Cloud-zu-Gerät-Nachrichten (HTTP-Protokoll) in einem Zeitfenster liegt nicht innerhalb des konfigurierten zulässigen Bereichs. |
| Niedrig | Benutzerdefinierte Warnung: Die Anzahl abgelehnter Cloud-zu-Gerät-Nachrichten im HTTP-Protokoll liegt nicht innerhalb des zulässigen Bereichs. | IoT Hub | Die Menge der Cloud-zu-Gerät-Nachrichten (HTTP-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs. |
| Niedrig | Benutzerdefinierte Warnung: Die Anzahl von Gerät-zu-Cloud-Nachrichten im HTTP-Protokoll liegt außerhalb des zulässigen Bereichs. | IoT Hub | Die Anzahl der Gerät-zu-Cloud-Nachrichten (HTTP-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs. |  |
| Niedrig | Benutzerdefinierte Warnung: Die Anzahl von Cloud-zu-Gerät-Nachrichten im MQTT-Protokoll liegt außerhalb des zulässigen Bereichs. | IoT Hub | Die Menge der Cloud-zu-Gerät-Nachrichten (MQTT-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs. |  |
| Niedrig | Benutzerdefinierte Warnung: Die Anzahl abgelehnter Cloud-zu-Gerät-Nachrichten im MQTT-Protokoll liegt außerhalb des zulässigen Bereichs. | IoT Hub | Die Anzahl der vom Gerät abgelehnten Cloud-zu-Gerät-Nachrichten (MQTT-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs. |
| Niedrig | Benutzerdefinierte Warnung: Die Anzahl von Gerät-zu-Cloud-Nachrichten im MQTT-Protokoll liegt außerhalb des zulässigen Bereichs. | IoT Hub | Die Menge der Gerät-zu-Cloud-Nachrichten (MQTT-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs. |
| Niedrig | Benutzerdefinierte Warnung: Die Anzahl von Befehlswarteschlangenbereinigungen liegt außerhalb des zulässigen Bereichs. | IoT Hub | Die Menge der Befehlswarteschlangenbereinigungen innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs. |  |
| Niedrig | Benutzerdefinierte Warnung: Die Anzahl von Modulzwillingsupdates liegt außerhalb des zulässigen Bereichs. | IoT Hub | Die Menge der Modulzwillingsupdates innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs. |
| Niedrig | Benutzerdefinierte Warnung: Die Anzahl nicht autorisierter Vorgänge liegt außerhalb des zulässigen Bereichs. | IoT Hub | Die Menge nicht autorisierter Vorgänge innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs. |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [Warnungen anpassen](quickstart-create-custom-alerts.md).
- [Übersicht](overview.md) über den Defender für IoT-Dienst
- Erfahren Sie, wie Sie auf Ihre [Sicherheitsdaten zugreifen](how-to-security-data-access.md).
- Erfahren Sie mehr über das [Untersuchen eines Geräts](how-to-investigate-device.md).
