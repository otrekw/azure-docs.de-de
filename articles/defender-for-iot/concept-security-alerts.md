---
title: Integrierte und benutzerdefinierte Warnlisten
description: Hier erhalten Sie Informationen zu Sicherheitswarnungen und empfohlenen Abhilfemaßnahmen unter Verwendung von Features und Diensten von Defender für IoT Hub.
ms.topic: conceptual
ms.date: 2/16/2021
ms.openlocfilehash: 73b3a1ca3e52f571ab7b531235650b6bda870691
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2021
ms.locfileid: "104784559"
---
# <a name="defender-for-iot-hub-security-alerts"></a>Sicherheitswarnungen für Defender für IoT Hub

Defender für IoT analysiert Ihre IoT-Lösung ständig mithilfe von Advanced Analytics- und Threat Intelligence-Funktionen, um Sie vor schädlichen Aktivitäten zu warnen.
Darüber hinaus können Sie – basierend auf Ihrer Kenntnis des erwarteten Geräteverhaltens – benutzerdefinierte Warnungen erstellen.
Eine Warnung fungiert als Indikator für eine potenzielle Kompromittierung, die untersucht und behoben werden sollte.

In diesem Artikel finden Sie eine Liste von integrierten Warnungen, die für Ihren IoT Hub ausgelöst werden können.
Neben integrierten Warnungen ermöglicht Ihnen Defender für IoT, benutzerdefinierte Warnungen zu erstellen, die auf dem erwarteten IoT Hub- und/oder Geräteverhalten basieren.
Weitere Informationen finden Sie unter [Konfigurierbare Warnungen](concept-customizable-security-alerts.md).

## <a name="built-in-alerts-for-iot-hub"></a>Integrierte Warnungen für IoT Hub

| severity | Name | BESCHREIBUNG | Vorschlag zur Problemlösung |
|--|--|--|--|
| **Mittlerer** Schweregrad |  |  |  |
| Neues Zertifikat zu einem IoT Hub hinzugefügt | Medium | Ein Zertifikat mit dem Namen \'%{DescCertificateName}\' wurde dem IoT-Hub \'%{DescIoTHubName}\' hinzugefügt. Wenn diese Aktion nicht durch eine autorisierte Person erfolgt ist, kann dies auf schädliche Aktivitäten hinweisen. | 1. Stellen Sie sicher, dass das Zertifikat von einer autorisierten Partei hinzugefügt wurde. <br> 2. Wenn es nicht von einer autorisierten Partei hinzugefügt wurde, entfernen Sie das Zertifikat, und eskalieren Sie die Warnung an das Sicherheitsteam Ihrer Organisation. |
| Zertifikat von einem IoT Hub gelöscht | Medium | Ein Zertifikat mit dem Namen \'%{DescCertificateName}\' wurde vom IoT-Hub \'%{DescIoTHubName}\' gelöscht. Wenn diese Aktion nicht durch eine autorisierte Person erfolgt ist, kann dies auf schädliche Aktivitäten hinweisen. | 1. Stellen Sie sicher, dass das Zertifikat von einer autorisierten Partei entfernt wurde. <br> 2. Wenn das Zertifikat nicht von einer autorisierten Partei entfernt wurde, fügen Sie das Zertifikat erneut hinzu, und eskalieren Sie die Warnung an das Sicherheitsteam Ihrer Organisation. |
| Nicht erfolgreicher Versuch erkannt, ein Zertifikat zu einem IoT Hub hinzuzufügen | Medium | Es wurde erfolglos versucht, dem IoT-Hub \'%{DescIoTHubName}\' das Zertifikat \'%{DescCertificateName}\' hinzuzufügen. Wenn diese Aktion nicht durch eine autorisierte Person erfolgt ist, kann dies auf schädliche Aktivitäten hinweisen. | Stellen Sie sicher, dass die Berechtigungen zum Ändern von Zertifikaten nur autorisierten Parteien gewährt werden. |
| Nicht erfolgreicher Versuch erkannt, ein Zertifikat von einem IoT Hub zu löschen | Medium | Es wurde erfolglos versucht, das Zertifikat \'%{DescCertificateName}\' vom IoT-Hub \'%{DescIoTHubName}\' zu löschen. Wenn diese Aktion nicht durch eine autorisierte Person erfolgt ist, kann dies auf schädliche Aktivitäten hinweisen. | Stellen Sie sicher, dass die Berechtigungen zum Ändern von Zertifikaten nur autorisierten Parteien gewährt werden. |
| x.509-Gerätezertifikat-Fingerabdruck-Konflikt | Medium | Der x.509-Zertifikatfingerabdruck für das Gerät stimmt mit der Konfiguration nicht überein. | Überprüfen Sie die Warnungen auf den Geräten. Es ist keine weitere Aktion erforderlich. |
| x.509-Zertifikat abgelaufen | Medium | Das X.509-Gerätezertifikat ist abgelaufen. | Hierbei kann es sich um ein legitimes Gerät handeln, dessen Zertifikat einfach abgelaufen ist, oder um einen Versuch, die Identität eines legitimen Geräts anzunehmen. Wenn das legitime Gerät zurzeit ordnungsgemäß kommuniziert, ist dies wahrscheinlich der Versuch eines Identitätswechsels. |
| **Niedriger** Schweregrad |  |  |  |
| Versuch erkannt, eine Diagnoseeinstellung für einen IoT Hub hinzuzufügen oder zu bearbeiten | Niedrig | Ein Versuch wurde erkannt, eine Diagnoseeinstellung für einen IoT Hub hinzuzufügen oder zu bearbeiten. Mit Diagnoseeinstellungen können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist. Wenn diese Aktion nicht durch eine autorisierte Person erfolgt ist, kann dies auf schädliche Aktivitäten hinweisen. | 1. Stellen Sie sicher, dass das Zertifikat von einer autorisierten Partei entfernt wurde.<br> 2. Wenn das Zertifikat nicht von einer autorisierten Partei entfernt wurde, fügen Sie das Zertifikat erneut hinzu, und eskalieren Sie die Warnung an das IT-Sicherheitsteam. |
| Versuch erkannt, eine Diagnoseeinstellung von einen IoT Hub zu löschen | Niedrig | Es wurde versucht (%{DescAttemptStatusMessage}\'), die Diagnoseeinstellung \'%{DescDiagnosticSettingName}\' auf dem IoT-Hub \'%{DescIoTHubName}\' hinzuzufügen oder zu bearbeiten. Mit der Diagnoseeinstellung können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist. Wenn diese Aktion nicht durch eine autorisierte Partei erfolgt ist, kann dies auf eine schädliche Aktivität hinweisen. | Stellen Sie sicher, dass die Berechtigungen zum Ändern von Diagnoseeinstellungen nur für eine autorisierte Partei erteilt werden. |
| SAS-Token abgelaufen | Niedrig | Ein abgelaufenes SAS-Token wurde von einem Gerät verwendet. | Hierbei kann es sich um ein legitimes Gerät handeln, dessen Token einfach abgelaufen ist, oder um einen Versuch, die Identität eines legitimen Geräts anzunehmen. Wenn das legitime Gerät zurzeit ordnungsgemäß kommuniziert, ist dies wahrscheinlich der Versuch eines Identitätswechsels. |
| Ungültige SAS-Tokensignatur | Niedrig | Ein von einem Gerät verwendetes SAS-Token weist eine ungültige Signatur auf. Die Signatur stimmt entweder nicht mit dem primären oder nicht mit dem sekundären Schlüssel überein. | Überprüfen Sie die Warnungen auf den Geräten. Es ist keine weitere Aktion erforderlich. |

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht](overview.md) über den Defender für IoT-Dienst
- Erfahren Sie, wie Sie auf Ihre [Sicherheitsdaten zugreifen](how-to-security-data-access.md).
- Erfahren Sie mehr über das [Untersuchen eines Geräts](how-to-investigate-device.md).
