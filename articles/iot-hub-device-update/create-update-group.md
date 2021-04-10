---
title: Erstellen einer Gerätegruppe in Device Update for Azure IoT Hub | Microsoft-Dokumentation
description: Erstellen einer Gerätegruppe in Device Update for Azure IoT Hub
author: vimeht
ms.author: vimeht
ms.date: 2/17/2021
ms.topic: how-to
ms.service: iot-hub-device-update
ms.openlocfilehash: a0894047db1ed7687a1a0f5f87fc4020ddf7c694
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101678486"
---
# <a name="create-device-groups-in-device-update-for-iot-hub"></a>Erstellen von Gerätegruppen in Device Update for IoT Hub
Device Update for IoT Hub ermöglicht die Bereitstellung eines Updates für eine Gruppe von IoT-Geräten.

## <a name="prerequisites"></a>Voraussetzungen

* [Zugriff auf einen IoT Hub mit aktiviertem Device Update for IoT Hub](create-device-update-account.md). Es wird empfohlen, dass Sie für Ihren IoT Hub einen S1-Tarif (Standard) oder höher verwenden. 
* Ein IoT-Gerät (oder Simulator), das/der für Device Update in IoT Hub bereitgestellt wird.
* [Mindestens ein Update wurde für das bereitgestellte Gerät erfolgreich importiert.](import-update.md)

## <a name="add-a-tag-to-your-devices"></a>Hinzufügen eines Tags zu Ihren Geräten  

Device Update for IoT Hub ermöglicht die Bereitstellung eines Updates für eine Gruppe von IoT-Geräten. Um eine Gruppe zu erstellen, müssen Sie im ersten Schritt dem Zielsatz von Geräten in IoT Hub ein Tag hinzufügen. Tags können Ihrem Gerät nur erfolgreich hinzugefügt werden, nachdem eine Verbindung mit Device Update hergestellt wurde.

In der Dokumentation unten wird das Hinzufügen und Aktualisieren eines Tags beschrieben.

### <a name="programmatically-update-device-twin"></a>Programmgesteuertes Aktualisieren des Gerätezwillings

Nachdem Sie das Gerät bei Device Update registriert haben, können Sie den Gerätezwilling mit RegistryManager mit dem entsprechenden Tag aktualisieren. 
[Weitere Informationen zum Hinzufügen von Tags mithilfe einer .NET-Beispiel-App.](../iot-hub/iot-hub-csharp-csharp-twin-getstarted.md)  
[Weitere Informationen zu Tageigenschaften](../iot-hub/iot-hub-devguide-device-twins.md#tags-and-properties-format).

#### <a name="device-update-tag-format"></a>Device Update-Tagformat

```markdown
     "tags": {
              "ADUGroup": "<CustomTagValue>"
             }
```

### <a name="using-jobs"></a>Verwenden von Aufträgen

Es ist möglich, einen Auftrag auf mehreren Geräten zu planen, um ein Device Update-Tag hinzuzufügen oder zu aktualisieren, wie in [diesen](../iot-hub/iot-hub-devguide-jobs.md) Beispielen gezeigt wird. [Weitere Informationen](../iot-hub/iot-hub-csharp-csharp-schedule-jobs.md)

  > [!NOTE] 
  > Diese Aktion belastet Ihr aktuelles IoT Hub-Nachrichtenkontingent. Es wird empfohlen, nur bis zu 50.000 Tags von Gerätezwillingen gleichzeitig zu ändern, da Sie sonst möglicherweise mehr IoT Hub-Einheiten erwerben müssen, wenn Sie Ihr tägliches IoT Hub-Nachrichtenkontingent überschreiten. Details finden Sie unter [Kontingente und Drosselung](../iot-hub/iot-hub-devguide-quotas-throttling.md#quotas-and-throttling).

### <a name="direct-twin-updates"></a>Direkte Zwillingsupdates

Tags können auch direkt im Gerätezwilling hinzugefügt oder aktualisiert werden.

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrem IoT Hub.

2. Suchen Sie im linken Navigationsbereich unter „IoT Devices“ (IoT-Geräte) oder „IoT Edge“ nach Ihrem IoT-Gerät, und navigieren Sie zum Gerätezwilling.

3. Löschen Sie im Gerätezwilling alle vorhandenen Device Update-Tagwerte, indem Sie diese auf NULL festlegen.

4. Fügen Sie wie unten gezeigt einen neuen Device Update-Tagwert hinzu. [Gerätezwilling-JSON-Beispieldokument mit Tags.](../iot-hub/iot-hub-devguide-device-twins.md#device-twins)

```JSON
    "tags": {
            "ADUGroup": "<CustomTagValue>"
            }
```

### <a name="limitations"></a>Einschränkungen

* Sie können Ihrem Tag einen beliebigen Wert hinzufügen (mit Ausnahme von „Uncategorized“ (Nicht kategorisiert), da es sich dabei um einen reservierten Wert handelt).
* Der Tagwert darf maximal 255 Zeichen enthalten.
* Der Tagwert darf alphanumerische Zeichen und die folgenden Sonderzeichen enthalten: „.“, „-“, „_“, „~“.
* Für Tag- und Gruppennamen wird Groß-/Kleinschreibung beachtet.
* Ein Gerät kann nur über ein Tag mit dem Namen ADUGroup verfügen. Alle nachfolgenden Hinzufügungen eines Tags mit diesem Namen überschreiben den vorhandenen Wert für den Tagnamen ADUGroup.
* Ein Gerät kann nur zu einer Gruppe gehören.

## <a name="create-a-device-group-by-selecting-an-existing-iot-hub-tag"></a>Erstellen einer Gerätegruppe durch Auswählen eines vorhandenen IoT Hub-Tags

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).

2. Wählen Sie den IoT Hub aus, den Sie zuvor mit Ihrer Device Update-Instanz verbunden haben.

3. Wählen Sie in der linken Navigationsleiste unter „Automatic Device Management“ (Automatische Geräteverwaltung) die Option „Device Updates“ aus.

4. Wählen Sie oben auf der Seite die Registerkarte „Groups“ (Gruppen) aus. Sie können die Anzahl der Geräte anzeigen, die mit Device Update verbunden sind, aber noch nicht gruppiert wurden.
   :::image type="content" source="media/create-update-group/ungrouped-devices.png" alt-text="Screenshot: Nicht gruppierte Geräte." lightbox="media/create-update-group/ungrouped-devices.png":::

5. Wählen Sie die Schaltfläche „Add“ (Hinzufügen) aus, um eine neue Gruppe zu erstellen.
   :::image type="content" source="media/create-update-group/add-group.png" alt-text="Screenshot: Hinzufügen von Gerätegruppen." lightbox="media/create-update-group/add-group.png":::

6. Wählen Sie ein IoT Hub-Tag aus der Liste aus, und wählen Sie dann „Create update group“ (Updategruppe erstellen) aus.
   :::image type="content" source="media/create-update-group/select-tag.png" alt-text="Screenshot: Tagauswahl." lightbox="media/create-update-group/select-tag.png":::

7. Nachdem die Gruppe erstellt wurde, werden Sie feststellen, dass das Updatekonformitätsdiagramm und die Liste der Gruppen aktualisiert wurden.  Das Updatekonformitätsdiagramm gibt Aufschluss über die Anzahl von Geräten mit verschiedenen Konformitätszuständen: „On latest update“ (Mit aktuellem Update), „New updates available“ (Neue Updates verfügbar), „Updates in progress“ (Updates werden ausgeführt) und „Devices not yet grouped“ (Noch nicht gruppierte Geräte). [Weitere Informationen zur Updatekonformität.](device-update-compliance.md)
   :::image type="content" source="media/create-update-group/updated-view.png" alt-text="Screenshot: Updatekonformitätsansicht." lightbox="media/create-update-group/updated-view.png":::

8. Die neu erstellte Gruppe und alle verfügbaren Updates für die Geräte in der neuen Gruppe sollten angezeigt werden. Sie können das Update in dieser Ansicht für die neue Gruppe bereitstellen, indem Sie auf den Namen des Updates klicken. Weitere Informationen finden Sie unter „Nächster Schritt: Bereitstellen des Updates“.

## <a name="view-device-details-for-the-group-you-created"></a>Anzeigen von Gerätedetails für die von Ihnen erstellte Gruppe

1. Navigieren Sie zu ihrer neu erstellten Gruppe, und klicken Sie auf den Gruppennamen.

2. Eine Liste der Geräte, die Teil der Gruppe sind, wird zusammen mit den Geräteupdateeigenschaften angezeigt. In dieser Ansicht können Sie auch die Updatekonformitätsinformationen für alle Geräte anzeigen, die Mitglieder der Gruppe sind. Das Updatekonformitätsdiagramm gibt Aufschluss über die Anzahl von Geräten mit verschiedenen Konformitätszuständen: „On latest update“ (Mit aktuellem Update), „New updates available“ (Neue Updates verfügbar) und „Updates in progress“ (Updates werden ausgeführt).
   :::image type="content" source="media/create-update-group/group-details.png" alt-text="Screenshot: Ansicht der Gerätegruppendetails." lightbox="media/create-update-group/group-details.png":::

3. Sie können auch auf jedes einzelne Gerät innerhalb einer Gruppe klicken, um in IoT Hub auf die Seite mit den Gerätedetails umgeleitet zu werden.
   :::image type="content" source="media/create-update-group/device-details.png" alt-text="Screenshot: Ansicht der Gerätedetails." lightbox="media/create-update-group/device-details.png":::

## <a name="next-steps"></a>Nächste Schritte 

[Bereitstellen des Updates](deploy-update.md)

[Weitere Informationen zu Gerätegruppen](device-update-groups.md)

[Weitere Informationen zur Updatekonformität.](device-update-compliance.md)
