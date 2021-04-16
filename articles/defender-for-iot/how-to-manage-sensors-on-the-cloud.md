---
title: Verwalten von Sensoren und Abonnements im Portal für Defender für IoT
description: Erfahren Sie, wie Sie im Defender für IoT-Portal das Onboarding für Sensoren durchführen und Sensoren anzeigen und verwalten.
ms.date: 2/18/2021
ms.topic: how-to
ms.openlocfilehash: 5b4c8b3d10fe88816e07eb775b2bf3827d578b17
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2021
ms.locfileid: "106383047"
---
# <a name="manage-sensors-and-subscriptions-in-the-defender-for-iot-portal"></a>Verwalten von Sensoren und Abonnements im Portal für Defender für IoT

In diesem Artikel wird beschrieben, wie Sie im [Defender für das IoT-Portal](https://portal.azure.com/#blade/Microsoft_Azure_IoT_Defender/IoTDefenderDashboard/Getting_Started) das Onboarding für Sensoren durchführen und Sensoren anzeigen und verwalten.

## <a name="onboard-sensors"></a>Integrieren von Sensoren

Sie integrieren einen Sensor, indem Sie ihn bei Azure Defender für IoT registrieren und eine Sensoraktivierungsdatei herunterladen.

### <a name="register-the-sensor"></a>Registrieren des Sensors

So registrieren Sie sich:

1. Navigieren Sie im **Defender für IoT-Portal** zur [Willkommensseite](https://portal.azure.com/#blade/Microsoft_Azure_IoT_Defender/IoTDefenderDashboard/Getting_Started).
1. Wählen Sie **Sensor integrieren** aus.
1. Legen Sie einen Sensornamen fest. Es wird empfohlen, die IP-Adresse des installierten Sensors als Teil des Namens anzugeben oder einen leicht identifizierbaren Namen zu verwenden. Dadurch wird eine einfachere Nachverfolgung und konsistente Benennung zwischen dem Registrierungsnamen im [Azure Defender für IoT-Portal](https://portal.azure.com/#blade/Microsoft_Azure_IoT_Defender/IoTDefenderDashboard/Getting_Started) und der IP-Adresse des bereitgestellten Sensors sichergestellt, der in der Sensorkonsole angezeigt wird.
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

Im [Defender für IoT-Portal](https://portal.azure.com/#blade/Microsoft_Azure_IoT_Defender/IoTDefenderDashboard/Getting_Started) können Sie grundlegende Informationen zu integrierten Sensoren anzeigen.

1. Wählen Sie **Standorte und Sensoren** aus.
1. Verwenden Sie die Filter- und Suchtools, um nach den gewünschten Sensoren- und Threat Intelligence-Informationen zu suchen.

- Die Anzahl der integrierten Sensoren
- Die Anzahl der mit der Cloud verbundenen und der lokal verwalteten Sensoren
- Der einem integrierten Sensor zugeordnete Hub
- Die einem Sensor hinzugefügten Details, z. B. der dem Sensor während des Onboardings zugewiesene Name, die dem Sensor zugeordnete Zone oder andere beschreibende Informationen, die mit Tags hinzugefügt wurden

## <a name="manage-onboarded-sensors"></a>Verwalten integrierter Sensoren

Verwenden Sie das [Defender für IoT-Portal](https://portal.azure.com/#blade/Microsoft_Azure_IoT_Defender/IoTDefenderDashboard/Getting_Started) zum Ausführen von Verwaltungsaufgaben für Sensoren.

Integrierte Sensoren können auf der Seite **Standorte und Sensoren** angezeigt werden. Sie können auf dieser Seite auch Sensorinformationen bearbeiten.

### <a name="export-sensor-details"></a>Exportieren von Sensordetails

Wählen Sie zum Exportieren von Informationen zum integrierten Sensor oben auf der Seite **Standorte und Sensoren** das Symbol **Exportieren** aus.

### <a name="edit-sensor-zone-details"></a>Bearbeiten von Sensorzonendetails

Verwenden Sie die Bearbeitungsoptionen auf der Seite **Standorte und Sensoren**, um den Sensornamen und die Zone zu bearbeiten.

So bearbeiten Sie die Informationen

1. Klicken Sie auf die **Auslassungspunkte** ( **...** ) für den zu bearbeitenden Sensor.
1. Wählen Sie **Bearbeiten** aus.
1. Aktualisieren Sie die Sensorzone, oder erstellen Sie eine neue Zone.

### <a name="delete-a-sensor"></a>Löschen eines Sensors

Wenn Sie einen mit der Cloud verbundenen Sensor löschen, werden keine Informationen an den IoT-Hub gesendet. Löschen Sie lokal verbundene/verwaltete Sensoren, wenn Sie nicht mehr damit arbeiten.

Gehen Sie wie folgt vor, um einen Sensor zu löschen:

1. Wählen Sie die Auslassungspunkte ( **...** ) für den zu löschenden Sensor aus.
1. Bestätigen Sie den Löschvorgang.

### <a name="reactivate-a-sensor"></a>Reaktivieren eines Sensors 

Möglicherweise müssen Sie den Sensor reaktivieren, wenn Sie folgende Schritte ausführen möchten:

- **Arbeiten im mit der Cloud verbundenen Modus statt im lokal verwalteten Modus**: Nach dem erneuten Aktivieren werden die Erkennungen des Sensors im Sensor angezeigt. Neu erkannte Warnungsinformationen werden über den IoT-Hub gesendet. Diese Informationen können über andere Azure-Dienste wie Azure Sentinel freigegeben werden.

- **Arbeiten im lokal verwalteten Modus statt im mit der Cloud verbundenen Modus:** Nach der Reaktivierung werden Informationen zur Sensorerkennung nur im Sensor angezeigt.

- **Zuordnen des Sensors zu einem neuen IoT-Hub**:  Registrieren Sie den Sensor dafür erneut bei einem neuen Hub, und laden Sie dann eine neue Aktivierungsdatei herunter.

Gehen Sie wie folgt vor, um einen Sensor erneut zu aktivieren:

1. Navigieren Sie im [Defender für IoT-Portal](https://portal.azure.com/#blade/Microsoft_Azure_IoT_Defender/IoTDefenderDashboard/Getting_Started) zur Seite **Standorte und Sensoren**.

2. Wählen Sie den Sensor aus, für den Sie eine neue Aktivierungsdatei hochladen möchten.

3. Löschen Sie den Sensor.

4. Wiederholen Sie das Onboarding des Sensors erneut im neuen Modus oder bei einem neuen IoT-Hub, indem Sie auf der Seite „Erste Schritte“ die Option **Sensor integrieren** auswählen.

5. Laden Sie die Aktivierungsdatei herunter.

1. Melden Sie sich bei der Sensorkonsole von Defender für IoT an.

7. Wählen Sie in der Sensorkonsole **Systemeinstellungen** und dann **Reaktivierung** aus.

   :::image type="content" source="media/how-to-manage-sensors-on-the-cloud/reactivate.png" alt-text="Hochladen Ihrer Aktivierungsdatei zum erneuten Aktivieren des Sensors":::

8. Wählen Sie **Hochladen** und dann die Datei aus, die Sie auf der Seite „Sensor integrieren“ gespeichert haben.

9. Wählen Sie **Aktivieren** aus.

## <a name="offboard-a-subscription"></a>Offboarding eines Abonnements

Abonnements werden auf monatlicher Basis verwaltet. Beim Offboarding eines Abonnements wird Ihnen die Nutzung dieses Abonnements bis zum Ende des Monats in Rechnung gestellt. 

Deinstallieren Sie vor dem Offboarding des Abonnements alle Sensoren, die dem Abonnement zugeordnet sind. Weitere Informationen zum Löschen eines Sensors finden Sie unter [Löschen eines Sensors](#delete-a-sensor). 

Offboarding eines Abonnements:

1. Navigieren Sie zur Seite **Preise**.
1. Wählen Sie das Abonnement aus, und klicken Sie dann auf das **Löschsymbol** :::image type="icon" source="media/how-to-manage-sensors-on-the-cloud/delete-icon.png" border="false":::.
1. Aktivieren Sie im Bestätigungspopupfenster das Kontrollkästchen, um zu bestätigen, dass Sie alle dem Abonnement zugeordneten Sensoren gelöscht haben.

    :::image type="content" source="media/how-to-manage-sensors-on-the-cloud/offboard-popup.png" alt-text="Aktivieren des Kontrollkästchens und Klicken auf „Offboard“ für das Offboarding des Sensors":::

1. Klicken Sie auf die Schaltfläche **Offboard**. 

Dies hat keine Auswirkungen auf die lokale Umgebung, aber Sie sollten den Sensor aus der lokalen Umgebung deinstallieren oder einem anderen Abonnement zuweisen, um zu verhindern, dass zugehörige Daten in die lokale Verwaltungskonsole übertragen werden. 

## <a name="next-steps"></a>Nächste Schritte

[Aktivieren und Einrichten Ihres Sensors](how-to-activate-and-set-up-your-sensor.md)
