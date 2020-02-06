---
title: Anleitung zu anpassbaren Sicherheitswarnungen für Azure Security Center für IoT | Microsoft-Dokumentation
description: Erfahren Sie etwas über anpassbare Sicherheitswarnungen und empfohlene Wartungsvorgänge unter Verwendung der Features und Dienste von Azure Security Center für IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/27/2020
ms.author: mlottner
ms.openlocfilehash: f10604d23901c2d7ed23f8b4f32a296f063d8620
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2020
ms.locfileid: "76776063"
---
# <a name="azure-security-center-for-iot-security-alerts"></a>Azure Security Center für IoT – Sicherheitswarnungen

Azure Security Center für IoT analysiert Ihre IoT-Lösung ständig mithilfe von Advanced Analytics- und Threat Intelligence-Funktionen, um Sie vor schädlichen Aktivitäten zu warnen.

Es wird empfohlen, benutzerdefinierte Warnungen im Einklang mit Ihren Kenntnissen zum erwarteten Geräteverhalten zu erstellen, um sicherzustellen, dass Warnungen die effizientesten Indikatoren für potenzielle Gefährdung in Ihrer eigenen Organisationsbereitstellung und -landschaft bieten. 

Die Azure Security Center für IoT-Warnungen in der folgenden Liste können Sie basierend auf dem erwarteten IoT Hub- und Geräteverhalten definieren. Weitere Informationen zum Anpassen der einzelnen Warnungen finden Sie unter [Erstellen von benutzerdefinierten Warnungen](quickstart-create-custom-alerts.md).

## <a name="azure-security-center-for-iot-alerts-available-for-customization"></a>Für die Anpassung verfügbare Warnungen in Azure Security Center für IoT 



| severity | Warnungsname | Datenquelle | Beschreibung | Vorschlag zur Problemlösung|
|---|---|---|---|---|
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl von Cloud-zu-Gerät-Nachrichten im AMQP-Protokoll liegt außerhalb des zulässigen Bereichs.          | IoT Hub     | Die Anzahl von Cloud-zu-Gerät-Nachrichten (AMQP-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.||
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl abgelehnter Cloud-zu-Gerät-Nachrichten im AMQP-Protokoll liegt außerhalb des zulässigen Bereichs. | IoT Hub     | Die Anzahl der vom Gerät abgelehnten Cloud-zu-Gerät-Nachrichten (AMQP-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.||
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl von Gerät-zu-Cloud-Nachrichten im AMQP-Protokoll liegt außerhalb des zulässigen Bereichs.      | IoT Hub     | Die Menge der Gerät-zu-Cloud-Nachrichten (AMQP-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.|   |
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl direkter Methodenaufrufe liegt außerhalb des zulässigen Bereichs. | IoT Hub     | Die Menge der direkten Methodenaufrufen innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.||
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl von Dateiuploads liegt außerhalb des zulässigen Bereichs. | IoT Hub     | Die Menge von Dateiuploads innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.| |
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl von Cloud-zu-Gerät-Nachrichten im HTTP-Protokoll liegt außerhalb des zulässigen Bereichs. | IoT Hub     | Die Menge der Cloud-zu-Gerät-Nachrichten (HTTP-Protokoll) in einem Zeitfenster liegt nicht innerhalb des konfigurierten zulässigen Bereichs.                                  |
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl abgelehnter Cloud-zu-Gerät-Nachrichten im HTTP-Protokoll liegt nicht innerhalb des zulässigen Bereichs. | IoT Hub     | Die Menge der Cloud-zu-Gerät-Nachrichten (HTTP-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs. |
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl von Gerät-zu-Cloud-Nachrichten im HTTP-Protokoll liegt außerhalb des zulässigen Bereichs. | IoT Hub| Die Menge der Gerät-zu-Cloud-Nachrichten (HTTP-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.|    |
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl von Cloud-zu-Gerät-Nachrichten im MQTT-Protokoll liegt außerhalb des zulässigen Bereichs. | IoT Hub     | Die Menge der Cloud-zu-Gerät-Nachrichten (MQTT-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.|   |
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl abgelehnter Cloud-zu-Gerät-Nachrichten im MQTT-Protokoll liegt außerhalb des zulässigen Bereichs. | IoT Hub     | Die Anzahl der vom Gerät abgelehnten Cloud-zu-Gerät-Nachrichten (MQTT-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs. |
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl von Gerät-zu-Cloud-Nachrichten im MQTT-Protokoll liegt außerhalb des zulässigen Bereichs.          | IoT Hub     | Die Menge der Gerät-zu-Cloud-Nachrichten (MQTT-Protokoll) innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.|
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl von Befehlswarteschlangenbereinigungen liegt außerhalb des zulässigen Bereichs.                               | IoT Hub     | Die Menge der Befehlswarteschlangenbereinigungen innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.||
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl von Modulzwillingsupdates liegt außerhalb des zulässigen Bereichs.                                       | IoT Hub     | Die Menge der Modulzwillingsupdates innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.|
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl nicht autorisierter Vorgänge liegt außerhalb des zulässigen Bereichs.  | IoT Hub     | Die Menge nicht autorisierter Vorgänge innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.|
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl aktiver Verbindungen liegt außerhalb des zulässigen Bereichs.  | Agent       | Die Anzahl aktiver Verbindungen innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs.|  Überprüfen Sie die Geräteprotokolle. Ermitteln Sie den Ursprung der Verbindung, und bestimmen Sie, ob er schädlich ist. Ist er schädlich, entfernen Sie potenzielle Schadsoftware, und versuchen Sie, die Herkunft nachzuvollziehen. Ist er nicht schädlich, fügen Sie die Quelle der Liste mit zulässigen Verbindungen hinzu.  |
| Niedrig      | Benutzerdefinierte Warnung: Es wurde eine Verbindung mit einer unzulässigen IP-Adresse erstellt.                             | Agent       | Es wurde eine Verbindung mit einer IP-Adresse erstellt, die nicht in der Liste zulässiger IP-Adressen enthalten ist. |Überprüfen Sie die Geräteprotokolle. Ermitteln Sie den Ursprung der Verbindung, und bestimmen Sie, ob er schädlich ist. Ist er schädlich, entfernen Sie potenzielle Schadsoftware, und versuchen Sie, die Herkunft nachzuvollziehen. Ist er nicht schädlich, fügen Sie die Quelle der Liste mit zulässigen IP-Adressen hinzu.                        |
| Niedrig      | Benutzerdefinierte Benachrichtigung: Die Anzahl nicht erfolgreicher lokaler Anmeldungen liegt außerhalb des zulässigen Bereichs.                               | Agent       | Die Menge nicht erfolgreicher lokaler Anmeldungen innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs. |   |
| Niedrig      | Benutzerdefinierte Benachrichtigung: Anmeldung eines Benutzers, der nicht in der Liste mit zugelassenen Benutzern enthalten ist | Agent       | Ein lokaler Benutzer, der nicht in Ihrer Liste mit zugelassenen Benutzern enthalten und beim Gerät angemeldet ist|  Navigieren Sie beim Speichern von Rohdaten zu Ihrem Protokollanalysekonto, und verwenden Sie die Daten, um das Gerät zu untersuchen und die Quelle zu ermitteln. Korrigieren Sie anschließend die Zulassungs-/Blockierungsliste für diese Einstellungen. Speichern Sie derzeit keine Rohdaten, korrigieren Sie auf dem Gerät die Zulassungs-/Blockierungsliste für diese Einstellungen.|
| Niedrig      | Benutzerdefinierte Benachrichtigung: Ein unzulässiger Prozesses wurde ausgeführt. | Agent       | Auf dem Gerät wurde ein unzulässiger Prozess ausgeführt. |Navigieren Sie beim Speichern von Rohdaten zu Ihrem Protokollanalysekonto, und verwenden Sie die Daten, um das Gerät zu untersuchen und die Quelle zu ermitteln. Korrigieren Sie anschließend die Zulassungs-/Blockierungsliste für diese Einstellungen. Speichern Sie derzeit keine Rohdaten, korrigieren Sie auf dem Gerät die Zulassungs-/Blockierungsliste für diese Einstellungen.  |
|


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [Warnungen anpassen](quickstart-create-custom-alerts.md).
- Sehen Sie sich die [Übersicht](overview.md) über den Dienst Azure Security Center für IoT an.
- Erfahren Sie, wie Sie auf Ihre [Sicherheitsdaten zugreifen](how-to-security-data-access.md).
- Erfahren Sie mehr über das [Untersuchen eines Geräts](how-to-investigate-device.md).