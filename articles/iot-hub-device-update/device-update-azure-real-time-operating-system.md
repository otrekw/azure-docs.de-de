---
title: Device Update für Azure-Echtzeitbetriebssystem | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu den ersten Schritten mit Device Update für das Azure-Echtzeitbetriebssystem.
author: ValOlson
ms.author: valls
ms.date: 3/18/2021
ms.topic: tutorial
ms.service: iot-hub-device-update
ms.openlocfilehash: d03ed1b23966d92a4e289534c427b84e5fc14789
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108141900"
---
# <a name="device-update-for-azure-iot-hub-tutorial-using-azure-real-time-operating-system-rtos"></a>Tutorial: Device Update for Azure IoT Hub mit Azure-Echtzeitbetriebssystem (Real Time Operating System, RTOS)

In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie den Device Update for IoT Hub-Agent in Azure RTOS NetX Duo erstellen. Darüber hinaus stehen einfache APIs zur Verfügung, mit denen Entwickler die Device Update-Funktion in ihre Anwendung integrieren können. Sehen Sie sich die [Beispiele](https://github.com/azure-rtos/samples/tree/PublicPreview/ADU) für die wichtigsten Halbleiter-Evaluierungsboards an. Sie enthalten Leitfäden zu den ersten Schritten, in denen Sie erfahren, wie Sie die OTA-Updates (Over-The-Air-Updates) für die Geräte konfigurieren, erstellen und bereitstellen.

In diesem Lernprogramm lernen Sie Folgendes:
> [!div class="checklist"]
> * Erste Schritte
> * Kennzeichnen Ihres Geräts
> * Erstellen einer Gerätegruppe
> * Bereitstellen eines Imageupdates
> * Überwachen der Updatebereitstellung

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen
* Zugriff auf einen IoT Hub. Es wird empfohlen, einen S1-Tarif (Standard) oder höher zu verwenden.
* Eine Device Update-Instanz und ein mit Ihrer IoT Hub-Instanz verknüpftes Konto Befolgen Sie die Anleitung zum [Erstellen und Verknüpfen eines Device Update-Kontos](create-device-update-account.md), sofern noch nicht geschehen.

## <a name="get-started"></a>Erste Schritte

Jedes boardspezifische Azure RTOS-Beispielprojekt enthält Code und eine Dokumentation zur jeweiligen Verwendung von Device Update for IoT Hub. 
1. Laden Sie die boardspezifischen Beispieldateien aus den [Beispielen für Azure RTOS und Device Update](https://github.com/azure-rtos/samples/tree/PublicPreview/ADU) herunter.
2. Navigieren Sie zum Dokumentationsordner des heruntergeladenen Beispiels.
3. Führen Sie die dokumentierten Schritte zum Vorbereiten der Azure-Ressourcen und des Kontos sowie zum Registrieren von IoT-Geräten aus.
5. Führen Sie als Nächstes die dokumentierten Schritte zum Erstellen eines neuen Firmwareimages sowie zum Importieren des Manifests für Ihr Board aus.
6. Veröffentlichen Sie dann das Firmwareimage und das Manifest für Device Update for IoT Hub.
7. Laden Sie abschließend das Projekt herunter, und führen Sie es auf Ihrem Gerät aus.

Weitere Informationen zu Azure RTOS finden Sie [hier](/azure/rtos/).  

## <a name="tag-your-device"></a>Kennzeichnen Ihres Geräts

1. Lassen Sie die Geräteanwendung aus dem vorherigen Schritt weiter laufen.
2. Melden Sie sich am [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zum IoT Hub.
3. Suchen Sie im linken Navigationsbereich unter IoT-Geräte nach Ihrem IoT-Gerät, und navigieren Sie zum Gerätezwilling.
4. Löschen Sie im Gerätezwilling alle vorhandenen Device Update-Tagwerte, indem Sie diese auf NULL festlegen.
5. Fügen Sie wie unten gezeigt einen neuen Device Update-Tagwert hinzu.

```JSON
    "tags": {
            "ADUGroup": "<CustomTagValue>"
            }
```

## <a name="create-update-group"></a>Erstellen der Updategruppe

1. Navigieren Sie zu dem IoT Hub, den Sie zuvor mit Ihrer Device Update-Instanz verbunden haben.
2. Wählen Sie in der linken Navigationsleiste unter „Automatic Device Management“ (Automatische Geräteverwaltung) die Option „Device Updates“ aus.
3. Wählen Sie oben auf der Seite die Registerkarte „Groups“ (Gruppen) aus. 
4. Wählen Sie die Schaltfläche „Add“ (Hinzufügen) aus, um eine neue Gruppe zu erstellen.
5. Wählen Sie das IoT Hub-Tag aus der Liste aus, das Sie im vorherigen Schritt erstellt haben. Wählen Sie „Create update group“ (Updategruppe erstellen) aus.

   :::image type="content" source="media/create-update-group/select-tag.PNG" alt-text="Screenshot: Tagauswahl." lightbox="media/create-update-group/select-tag.PNG":::

[Weitere Informationen](create-update-group.md) zum Hinzufügen von Tags und zum Erstellen von Updategruppen

## <a name="deploy-new-firmware"></a>Bereitstellen neuer Firmware

1. Nachdem die Gruppe erstellt wurde, sollte ein neues Update für Ihre Gerätegruppe mit einem Link zum Update unter „Pending Updates“ (Ausstehende Updates) angezeigt werden. Möglicherweise müssen Sie die Anzeige ein Mal aktualisieren. 
2. Klicken Sie auf das verfügbare Update.
3. Vergewissern Sie sich, dass die richtige Gruppe als Zielgruppe ausgewählt ist. Planen Sie die Bereitstellung, und wählen Sie dann „Deploy update“ (Update bereitstellen) aus.

   :::image type="content" source="media/deploy-update/select-update.png" alt-text="Auswählen des Updates" lightbox="media/deploy-update/select-update.png":::

4. Zeigen Sie das Konformitätsdiagramm an. Es sollte angezeigt werden, dass das Update jetzt ausgeführt wird. 

   :::image type="content" source="media/deploy-update/update-in-progress.png" alt-text="Update wird ausgeführt." lightbox="media/deploy-update/update-in-progress.png":::

5. Nachdem das Gerät erfolgreich aktualisiert wurde, sollten Sie sehen, dass Ihr Konformitätsdiagramm und die Details der Bereitstellung so aktualisiert werden, dass sie übereinstimmen. 

   :::image type="content" source="media/deploy-update/update-succeeded.png" alt-text="Update erfolgreich" lightbox="media/deploy-update/update-succeeded.png":::

## <a name="monitor-an-update-deployment"></a>Überwachen einer Updatebereitstellung

1. Wählen Sie oben auf der Seite die Registerkarte „Deployments“ (Bereitstellungen) aus.

   :::image type="content" source="media/deploy-update/deployments-tab.png" alt-text="Registerkarte „Deployments“ (Bereitstellungen)" lightbox="media/deploy-update/deployments-tab.png":::

2. Wählen Sie die erstellte Bereitstellung aus, um die Bereitstellungsdetails anzuzeigen.

   :::image type="content" source="media/deploy-update/deployment-details.png" alt-text="Bereitstellungsdetails" lightbox="media/deploy-update/deployment-details.png":::

3. Wählen Sie „Refresh“ (Aktualisieren) aus, um die neuesten Statusdetails anzuzeigen. Setzen Sie diesen Prozess fort, bis der Status in „Succeeded“ (Erfolgreich) geändert wurde.

Sie haben nun ein erfolgreiches End-to-End-Imageupdate mit Device Update for IoT Hub auf einem Raspberry Pi 3 B+-Gerät abgeschlossen. 

## <a name="cleanup-resources"></a>Bereinigen von Ressourcen

Wenn Sie es nicht mehr benötigten, bereinigen Sie Ihr Konto für das Geräteupdate sowie die Instanz, IoT Hub und das IoT-Gerät. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure RTOS sowie zur Verwendung von Azure RTOS mit Azure IoT finden Sie unter https://azure.com/rtos.