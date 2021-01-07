---
title: Durchführen des Onboardings für und Verwalten von Sensoren im Defender für IoT-Portal
description: Erfahren Sie, wie Sie im Defender für IoT-Portal das Onboarding für Sensoren durchführen und Sensoren anzeigen und verwalten.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/27/2020
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: a763d8b65049cd9f301379c2c038a1d799114653
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2020
ms.locfileid: "97836055"
---
# <a name="onboard-and-manage-sensors-in-the-defender-for-iot-portal"></a>Durchführen des Onboardings für und Verwalten von Sensoren im Defender für IoT-Portal

In diesem Artikel wird beschrieben, wie Sie im Defender für IoT-Portal das Onboarding für Sensoren durchführen und Sensoren anzeigen und verwalten.

## <a name="onboard-sensors"></a>Integrieren von Sensoren

Sie integrieren einen Sensor, indem Sie ihn bei Azure Defender für IoT registrieren und eine Sensoraktivierungsdatei herunterladen.

### <a name="register-the-sensor"></a>Registrieren des Sensors

So registrieren Sie sich:

1. Navigieren Sie im Defender für IoT-Portal zur **Willkommensseite**.
1. Wählen Sie **Sensor integrieren** aus.
1. Legen Sie einen Sensornamen fest. Es wird empfohlen, die IP-Adresse des installierten Sensors als Teil des Namens anzugeben oder einen leicht identifizierbaren Namen zu verwenden. Dadurch wird eine einfachere Nachverfolgung und konsistente Benennung zwischen dem Registrierungsnamen im Azure Defender für IoT-Portal und der IP-Adresse des bereitgestellten Sensors sichergestellt, der in der Sensorkonsole angezeigt wird.
1. Ordnen Sie den Sensor einem Azure-Abonnement zu.
1. Wählen Sie mithilfe des Umschalters **Mit der Cloud verbunden** einen Sensorverwaltungsmodus aus. Ist der Umschalter auf „Ein“ festgelegt, ist der Sensor mit der Cloud verbunden. Wenn der Umschalter auf „Aus“ festgelegt ist, wird der Sensor lokal verwaltet.

   - **Mit der Cloud verbundene Sensoren**: Die vom Sensor erkannten Informationen werden in der Sensorkonsole angezeigt. Warnungsinformationen werden über einen IoT-Hub übermittelt und können für andere Azure-Dienste wie Azure Sentinel freigegeben werden.

   - **Lokal verwaltete Sensoren**: Die von Sensoren erkannten Informationen werden in der Sensorkonsole angezeigt. Wenn Sie in einem Air-Gap-Netzwerk arbeiten und eine einheitliche Ansicht aller Informationen haben möchten, die von mehreren lokal verwalteten Sensoren erkannt wurden, arbeiten Sie mit der lokalen Verwaltungskonsole.

   Bei mit der Cloud verbundenen Sensoren wird in der Sensorkonsole der während des Onboardings definierte Name angezeigt. Sie können diesen Namen nicht direkt über die Konsole ändern. Bei lokal verwalteten Sensoren wird der während des Onboardings angewendete Name in Azure gespeichert und kann in der Sensorkonsole geändert werden.

1. Wählen Sie einen IoT-Hub aus, der als Gateway zwischen diesem Sensor und Azure Defender für IoT fungiert.
1. Wenn der Sensor mit der Cloud verbunden ist, ordnen Sie ihn einem IoT-Hub zu, und definieren Sie dann einen Standortnamen und eine Zone. Sie können auch beschreibende Tags hinzufügen. Der Standortname, die Zone und Tags sind beschreibende Einträge auf der [Seite „Standorte und Sensoren“](#view-onboarded-sensors).

### <a name="download-the-sensor-activation-file"></a>Herunterladen der Sensoraktivierungsdatei

Die Sensoraktivierungsdatei enthält Anweisungen zum Verwaltungsmodus des Sensors. Sie können für jeden von Ihnen bereitgestellten Sensor eine eindeutige Aktivierungsdatei herunterladen. Ein Benutzer, der sich zum ersten Mal bei der Sensorkonsole anmeldet, lädt die Aktivierungsdatei auf den Sensor hoch.

Gehen Sie wie folgt vor, um eine Aktivierungsdatei herunterzuladen:

1. Wählen Sie auf der Seite **Sensor integrieren** die Option **Aktivierungsdatei herunterladen** aus.
1. Stellen Sie die Datei für den Benutzer bereit, der sich zum ersten Mal bei der Sensorkonsole anmeldet.

## <a name="view-onboarded-sensors"></a>Anzeigen integrierter Sensoren

Im Defender für IoT-Portal können Sie grundlegende Informationen zu integrierten Sensoren anzeigen. 

1. Wählen Sie **Standorte und Sensoren** aus.
1. Verwenden Sie auf der Seite **Standorte und Sensoren** die Filter- und Suchtools, um nach den gewünschten Sensorinformationen zu suchen.

Die verfügbaren Informationen umfassen Folgendes:

- Die Anzahl der integrierten Sensoren
- Die Anzahl der mit der Cloud verbundenen und der lokal verwalteten Sensoren
- Der einem integrierten Sensor zugeordnete Hub
- Die einem Sensor hinzugefügten Details, z. B. der dem Sensor während des Onboardings zugewiesene Name, die dem Sensor zugeordnete Zone oder andere beschreibende Informationen, die mit Tags hinzugefügt wurden

## <a name="manage-onboarded-sensors"></a>Verwalten integrierter Sensoren

Sie verwenden das Defender für IoT-Portal zum Ausführen von Verwaltungsaufgaben für Sensoren.

### <a name="export"></a>Exportieren

Wählen Sie zum Exportieren von Informationen zum integrierten Sensor oben auf der Seite **Standorte und Sensoren** das Symbol **Exportieren** aus.

### <a name="edit"></a>Bearbeiten

Verwenden Sie die Bearbeitungstools **Standorte und Sensoren**, um den Standortnamen, die Zone und Tags hinzuzufügen und zu bearbeiten.

### <a name="delete"></a>Löschen

Wenn Sie einen mit der Cloud verbundenen Sensor löschen, werden keine Informationen an den IoT-Hub gesendet. Löschen Sie lokal verbundene/verwaltete Sensoren, wenn Sie nicht mehr damit arbeiten.

Gehen Sie wie folgt vor, um einen Sensor zu löschen:

1. Wählen Sie die Auslassungspunkte ( **...** ) für den zu löschenden Sensor aus. 
1. Bestätigen Sie den Löschvorgang.

### <a name="reactivate"></a>Erneut aktivieren

Möglicherweise möchten Sie den Modus aktualisieren, in dem Ihr Sensor verwaltet wird. Beispiel:

- **Arbeiten im mit der Cloud verbundenen Modus statt im lokal verwalteten Modus**: Aktualisieren Sie hierzu die Aktivierungsdatei für Ihren lokal verbundenen/verwalteten Sensor mit einer Aktivierungsdatei für einen mit der Cloud verbundenen Sensor. Nach der Reaktivierung werden Sensorerkennungen sowohl im Sensor als auch im Defender für IoT-Portal angezeigt. Nachdem die Reaktivierungsdatei erfolgreich hochgeladen wurde, werden neu erkannte Warnungsinformationen an Azure gesendet.

- **Arbeiten im lokal verwalteten Modus statt im mit der Cloud verbundenen Modus**: Aktualisieren Sie hierzu die Aktivierungsdatei für einen mit der Cloud verbundenen Sensor mit einer Aktivierungsdatei für einen lokal verwalteten Sensor. Nach der Reaktivierung werden Informationen zur Sensorerkennung nur im Sensor angezeigt.

- **Zuordnen des Sensors zu einem neuen IoT-Hub**:  Registrieren Sie dazu den Sensor erneut, und laden Sie eine neue Aktivierungsdatei hoch.

Gehen Sie wie folgt vor, um einen Sensor erneut zu aktivieren:

1. Wechseln Sie im Defender für IoT-Portal zur Seite **Standorte und Sensoren**.

2. Wählen Sie den Sensor aus, für den Sie eine neue Aktivierungsdatei hochladen möchten.

3. Löschen Sie den Sensor.

4. Integrieren Sie den Sensor über die Seite **Onboarding** im neuen Modus oder in einen neuen IoT-Hub.

5. Laden Sie die Aktivierungsdatei von der Seite **Aktivierungsdatei herunterladen** herunter.

6. Melden Sie sich bei der Sensorkonsole von Defender für IoT an.

7. Wählen Sie in der Sensorkonsole **Systemeinstellungen** und dann **Reaktivierung** aus.

   :::image type="content" source="media/how-to-manage-sensors-on-the-cloud/reactivate.png" alt-text="Hochladen Ihrer Aktivierungsdatei zum erneuten Aktivieren des Sensors":::

8. Wählen Sie **Hochladen** und dann die gespeicherte Datei aus.

9. Wählen Sie **Aktivieren** aus. 

## <a name="see-also"></a>Weitere Informationen

[Aktivieren und Einrichten Ihres Sensors](how-to-activate-and-set-up-your-sensor.md)
