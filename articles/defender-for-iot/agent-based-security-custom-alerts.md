---
title: Benutzerdefinierte Warnungen bei Agent-basierter Sicherheit
titleSuffix: Azure Defender for IoT
description: Informieren Sie sich über anpassbare Sicherheitswarnungen und empfohlene Wartungsvorgänge unter Verwendung der Features und Dienste von Defender für IoT.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/16/2021
ms.author: shhazam
ms.openlocfilehash: e885566dd067d70fd4800ca96b8729494464da85
ms.sourcegitcommit: 58ff80474cd8b3b30b0e29be78b8bf559ab0caa1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100640997"
---
# <a name="defender-for-iot-devices-custom-security-alerts"></a>Defender für IoT-Geräte: benutzerdefinierte Sicherheitswarnungen

Defender für IoT analysiert Ihre IoT-Lösung ständig mithilfe von Advanced Analytics- und Threat Intelligence-Funktionen, um Sie vor schädlichen Aktivitäten zu warnen.

Es wird empfohlen, benutzerdefinierte Warnungen im Einklang mit Ihren Kenntnissen zum erwarteten Geräteverhalten zu erstellen, um sicherzustellen, dass Warnungen die effizientesten Indikatoren für potenzielle Gefährdung in Ihrer eigenen Organisationsbereitstellung und -landschaft bieten.

Sie können die Warnungen für Defender für IoT in den folgenden Listen basierend auf dem erwarteten Verhalten der IoT-Geräte definieren. Weitere Informationen zum Anpassen der einzelnen Warnungen finden Sie unter [Erstellen von benutzerdefinierten Warnungen](quickstart-create-custom-alerts.md).

## <a name="agent-based-security-custom-alerts"></a>Benutzerdefinierte Warnungen bei Agent-basierter Sicherheit

| severity | Warnungsname | Datenquelle | BESCHREIBUNG | Vorschlag zur Problemlösung |
|--|--|--|--|--|
| Niedrig | Benutzerdefinierte Warnung: Die Anzahl aktiver Verbindungen liegt außerhalb des zulässigen Bereichs. | Klassisches Sicherheitsmodul, Azure RTOS | Die Anzahl aktiver Verbindungen innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs. | Überprüfen Sie die Geräteprotokolle. Ermitteln Sie den Ursprung der Verbindung, und bestimmen Sie, ob er schädlich ist. Ist er schädlich, entfernen Sie potenzielle Schadsoftware, und versuchen Sie, die Herkunft nachzuvollziehen. Ist er nicht schädlich, fügen Sie die Quelle der Liste mit zulässigen Verbindungen hinzu. |
| Niedrig | Benutzerdefinierte Warnung: Es wurde eine Verbindung mit einer unzulässigen IP-Adresse erstellt. | Klassisches Sicherheitsmodul, Azure RTOS | Es wurde eine Verbindung mit einer IP-Adresse erstellt, die nicht in der Liste zulässiger IP-Adressen enthalten ist. | Überprüfen Sie die Geräteprotokolle. Ermitteln Sie den Ursprung der Verbindung, und bestimmen Sie, ob er schädlich ist. Ist er schädlich, entfernen Sie potenzielle Schadsoftware, und versuchen Sie, die Herkunft nachzuvollziehen. Ist er nicht schädlich, fügen Sie die Quelle der Liste mit zulässigen IP-Adressen hinzu. |
| Niedrig | Benutzerdefinierte Warnung: Die Anzahl nicht erfolgreicher lokaler Anmeldungen liegt außerhalb des zulässigen Bereichs. | Klassisches Sicherheitsmodul, Azure RTOS | Die Anzahl nicht erfolgreicher lokaler Anmeldungen innerhalb eines bestimmten Zeitfensters liegt außerhalb des zurzeit konfigurierten und zulässigen Bereichs. |  |
| Niedrig | Benutzerdefinierte Warnung: Anmeldung eines Benutzers, der nicht in der Liste mit zugelassenen Benutzern enthalten ist | Klassisches Sicherheitsmodul, Azure RTOS | Ein lokaler Benutzer, der nicht in Ihrer Liste mit zugelassenen Benutzern enthalten und beim Gerät angemeldet ist | Navigieren Sie beim Speichern von Rohdaten zu Ihrem Protokollanalysekonto, und verwenden Sie die Daten, um das Gerät zu untersuchen und die Quelle zu ermitteln. Korrigieren Sie anschließend die Positiv-/Sperrliste für diese Einstellungen. Speichern Sie derzeit keine Rohdaten, korrigieren Sie auf dem Gerät die Zulassungs-/Blockierungsliste für diese Einstellungen. |
| Niedrig | Benutzerdefinierte Warnung: Ein unzulässiger Prozesses wurde ausgeführt. | Klassisches Sicherheitsmodul, Azure RTOS | Auf dem Gerät wurde ein unzulässiger Prozess ausgeführt. | Navigieren Sie beim Speichern von Rohdaten zu Ihrem Protokollanalysekonto, und verwenden Sie die Daten, um das Gerät zu untersuchen und die Quelle zu ermitteln. Korrigieren Sie anschließend die Positiv-/Sperrliste für diese Einstellungen. Speichern Sie derzeit keine Rohdaten, korrigieren Sie auf dem Gerät die Zulassungs-/Blockierungsliste für diese Einstellungen. |
|

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [Warnungen anpassen](quickstart-create-custom-alerts.md).
- [Übersicht](overview.md) über den Defender für IoT-Dienst
- Erfahren Sie, wie Sie auf Ihre [Sicherheitsdaten zugreifen](how-to-security-data-access.md).
- Erfahren Sie mehr über das [Untersuchen eines Geräts](how-to-investigate-device.md).
