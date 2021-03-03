---
title: Bereitstellen eines Updates mithilfe von Device Update for Azure IoT Hub | Microsoft-Dokumentation
description: Bereitstellen eines Updates mithilfe von Device Update for Azure IoT Hub.
author: vimeht
ms.author: vimeht
ms.date: 2/11/2021
ms.topic: how-to
ms.service: iot-hub-device-update
ms.openlocfilehash: 0a11c8e8946229941c1fe60f7f2ce84d9fadb2ed
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678485"
---
# <a name="deploy-an-update-using-device-update-for-iot-hub"></a>Bereitstellen eines Updates mithilfe von Device Update for IoT Hub

Erfahren Sie, wie Sie mithilfe von Device Update for IoT Hub ein Update für ein IoT-Gerät bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

* [Zugriff auf einen IoT Hub mit aktiviertem Device Update for IoT Hub](create-device-update-account.md). Es wird empfohlen, dass Sie für Ihren IoT Hub einen S1-Tarif (Standard) oder höher verwenden. 
* [Mindestens ein Update wurde für das bereitgestellte Gerät erfolgreich importiert.](import-update.md) 
* Ein IoT-Gerät (oder Simulator), das/der für Device Update in IoT Hub bereitgestellt wird.
* [Dem IoT-Gerät, das aktualisiert werden soll,wurde ein Tag zugewiesen. Das Gerät gehört zu mindestens einer Aktualisierungsgruppe.](create-update-group.md)
* Unterstützte Browser:
  * [Microsoft Edge](https://www.microsoft.com/edge)
  * Google Chrome

## <a name="deploy-an-update"></a>Bereitstellen eines Updates

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).

2. Navigieren Sie zum Blatt „Device Update“ Ihres IoT Hub.

  :::image type="content" source="media/deploy-update/device-update-iot-hub.png" alt-text="IoT Hub" lightbox="media/deploy-update/device-update-iot-hub.png":::

3. Wählen Sie oben auf der Seite die Registerkarte „Groups“ (Gruppen) aus. [Weitere Informationen](device-update-groups.md) zu Gerätegruppen. 

  :::image type="content" source="media/deploy-update/updated-view.png" alt-text="Registerkarte „Gruppen“" lightbox="media/deploy-update/updated-view.png":::

4. Zeigen das Updatekompatibilitätsdiagramm und die Gruppenliste an. Es sollte ein neues Update für Ihre Gerätegruppe mit einem Link zum Update unter „Pending Updates“ (Ausstehende Updates) angezeigt werden (möglicherweise müssen Sie die Anzeige ein Mal aktualisieren). [Weitere Informationen zur Updatekonformität.](device-update-compliance.md) 

5. Wählen Sie das verfügbare Update aus.

6. Vergewissern Sie sich, dass die richtige Gruppe als Zielgruppe ausgewählt ist. Planen Sie die Bereitstellung, und wählen Sie dann „Deploy update“ (Update bereitstellen) aus.

   :::image type="content" source="media/deploy-update/select-update.png" alt-text="Auswählen des Updates" lightbox="media/deploy-update/select-update.png":::

7. Zeigen Sie das Konformitätsdiagramm an. Es sollte angezeigt werden, dass das Update jetzt ausgeführt wird. 

   :::image type="content" source="media/deploy-update/update-in-progress.png" alt-text="Update wird ausgeführt." lightbox="media/deploy-update/update-in-progress.png":::

8. Nachdem das Gerät erfolgreich aktualisiert wurde, sollten Sie sehen, dass Ihr Konformitätsdiagramm und die Details der Bereitstellung so aktualisiert werden, dass sie übereinstimmen. 

   :::image type="content" source="media/deploy-update/update-succeeded.png" alt-text="Update erfolgreich" lightbox="media/deploy-update/update-succeeded.png":::

## <a name="monitor-an-update-deployment"></a>Überwachen einer Updatebereitstellung

1. Wählen Sie oben auf der Seite die Registerkarte „Deployments“ (Bereitstellungen) aus.

   :::image type="content" source="media/deploy-update/deployments-tab.png" alt-text="Registerkarte „Deployments“ (Bereitstellungen)" lightbox="media/deploy-update/deployments-tab.png":::

2. Wählen Sie die erstellte Bereitstellung aus, um die Bereitstellungsdetails anzuzeigen.

   :::image type="content" source="media/deploy-update/deployment-details.png" alt-text="Bereitstellungsdetails" lightbox="media/deploy-update/deployment-details.png":::

3. Wählen Sie „Refresh“ (Aktualisieren) aus, um die neuesten Statusdetails anzuzeigen. Setzen Sie diesen Prozess fort, bis der Status in „Succeeded“ (Erfolgreich) geändert wurde.


## <a name="retry-an-update-deployment"></a>Wiederholen einer Updatebereitstellung

Wenn die Bereitstellung aus irgendeinem Grund fehlschlägt, können Sie die Bereitstellung für fehlgeschlagene Geräte wiederholen. 

1. Navigieren Sie zur Registerkarte „Deployments“ (Bereitstellungen), und wählen Sie die Bereitstellung aus, bei der ein Fehler aufgetreten ist. 

   :::image type="content" source="media/deploy-update/deployment-details.png" alt-text="Bereitstellungsdetails" lightbox="media/deploy-update/deployment-details.png":::

2. Klicken Sie im Bereich mit den detaillierten Bereitstellungsinformationen auf den Gerätestatus „Failed“ (Fehlgeschlagen).

3. Klicken Sie auf „Retry failed devices“ (Wiederholungsversuch für fehlgeschlagene Geräte), und bestätigen Sie die Bestätigungsbenachrichtigung. 

## <a name="next-steps"></a>Nächste Schritte

[Häufig auftretende Probleme und Problembehandlung](troubleshoot-device-update.md)
