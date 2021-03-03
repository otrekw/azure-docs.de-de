---
title: 'Device Update for Azure IoT Hub: Tutorial zum Verwenden des Ubuntu Server 18.04 x64 Package Agent | Microsoft-Dokumentation'
description: Erste Schritte mit Device Update for Azure IoT Hub mit dem Ubuntu Server 18.04 x64 Package Agent.
author: vimeht
ms.author: vimeht
ms.date: 2/16/2021
ms.topic: tutorial
ms.service: iot-hub-device-update
ms.openlocfilehash: 19be3b141fb663ea0f4f11d811bf6ffc33252504
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101658848"
---
# <a name="device-update-for-azure-iot-hub-tutorial-using-the-ubuntu-server-1804-x64-package-agent"></a>Device Update for Azure IoT Hub: Tutorial zum Verwenden des Ubuntu Server 18.04 x64 Package Agent

Device Update for IoT Hub unterstützt zwei Arten von Updates: imagebasiert und paketbasiert.

Paketbasierte Updates sind gezielte Updates, die nur eine bestimmte Komponente oder Anwendung auf dem Gerät ändern. Dies führt zu einem geringeren Verbrauch an Bandbreite und verringert die Zeit zum Herunterladen und Installieren des Updates. Paketupdates ermöglichen in der Regel eine geringere Ausfallzeit der Geräte beim Anwenden eines Updates und vermeiden den Mehraufwand beim Erstellen von Images.

In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie ein paketbasiertes End-to-End-Update über Device Update for IoT Hub ausführen. Für dieses Tutorial wird ein Beispiel-Paket-Agent für Ubuntu Server 18.04 x64 verwendet. Auch wenn Sie planen, eine andere Betriebssystem-Plattformkonfiguration zu verwenden, ist dieses Tutorial dennoch nützlich, um die Tools und Konzepte in Device Update for IoT Hub kennenzulernen. Vervollständigen Sie diese Einführung in einen End-to-End-Updateprozess, und wählen Sie dann Ihre bevorzugte Aktualisierungsform und Betriebssystemplattform aus, um sich mit den Details vertraut zu machen. Sie können Device Update for IoT Hub verwenden, um ein Azure IoT- oder Azure IoT Edge-Gerät mithilfe dieses Tutorials zu aktualisieren. 

In diesem Lernprogramm lernen Sie Folgendes:
> [!div class="checklist"]
> * Konfigurieren eines Geräteupdate-Paketrepositorys
> * Herunterladen und Installieren des Geräteupdate-Agents und seiner Abhängigkeiten
> * Hinzufügen eines Tags zu Ihrem IoT-Gerät
> * Importieren eines Updates
> * Erstellen einer Gerätegruppe
> * Bereitstellen eines Paketupdates
> * Überwachen der Updatebereitstellung

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen
* Zugriff auf einen IoT Hub. Es wird empfohlen, einen S1-Tarif (Standard) oder höher zu verwenden.
* Ein Azure IoT- oder Azure IoT Edge-Gerät mit Ubuntu Server 18.04 x64, das mit IoT Hub verbunden ist.
   * Wenn Sie ein Azure IoT Edge-Gerät verwenden, stellen Sie sicher, dass es mit v1.2.0 der Edge-Runtime oder höher ausgeführt wird. 
* Wenn Sie kein Azure IoT Edge-Gerät verwenden, [installieren Sie das aktuellste `aziot-identity-service`-Paket (Vorschau) auf Ihrem IoT-Gerät](https://github.com/Azure/iot-identity-service/actions/runs/575919358). 
* [Device Update-Konto und -Instanz, die mit demselben IoT-Hub wie oben verknüpft sind.](create-device-update-account.md)

## <a name="configure-device-update-package-repository"></a>Konfigurieren eines Geräteupdate-Paketrepositorys

1. Installieren Sie die zum Betriebssystem Ihres Geräts passende Repositorykonfiguration. Für dieses Tutorial ist dies Ubuntu Server 18.04. 
   
   ```shell
      curl https://packages.microsoft.com/config/ubuntu/18.04/multiarch/prod.list > ./microsoft-prod.list
   ```

2. Kopieren Sie die generierte Liste in das Verzeichnis „sources.list.d“.

   ```shell
      sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```
   
3. Installieren Sie den öffentlichen Schlüssel von Microsoft GPG.

   ```shell
      curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
      sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

## <a name="install-device-update-deb-agent-packages"></a>Installieren von Device Update .deb-Agent-Paketen

1. Aktualisieren von Paketlisten auf Ihrem Gerät

   ```shell
      sudo apt-get update
   ```

2. Installieren des Pakets deviceupdate-agent und seiner Abhängigkeiten

   ```shell
      sudo apt-get install deviceupdate-agent deliveryoptimization-plugin-apt
   ```

Softwarepakete von Device Update for Azure IoT Hub unterliegen den folgenden Lizenzbedingungen:
   * [Lizenz für Device Update für IoT Hub](https://github.com/Azure/iot-hub-device-update/blob/main/LICENSE.md)
   * [Lizenz für den Übermittlungsoptimierungs-Client](https://github.com/microsoft/do-client/blob/main/LICENSE.md)
   
Lesen Sie die Lizenzbedingungen, bevor Sie ein Paket verwenden. Durch Ihre Installation und Verwendung eines Pakets erklären Sie Ihre Zustimmung zu diesen Bedingungen. Wenn Sie den Lizenzbedingungen nicht zustimmen, verwenden Sie das Paket nicht.

## <a name="configure-device-update-agent-using-azure-iot-identity-service-preview"></a>Konfigurieren von Device Update Agent mit dem Azure IoT Identity-Dienst (Vorschau)

Nachdem Sie die erforderlichen Pakete installiert haben, müssen Sie die Cloudidentität und die dazugehörigen Authentifizierungsinformationen für das Gerät bereitstellen.

1. Öffnen Sie die Konfigurationsdatei.

   ```shell
      sudo nano /etc/aziot/config.toml
   ```

2. Suchen Sie den Abschnitt zur Bereitstellungskonfiguration der Datei. Heben Sie die Auskommentierung des Abschnitts „Manual provisioning with connection string“ (Manuelle Bereitstellung mit Verbindungszeichenfolge) auf. Aktualisieren Sie den Wert von connection_string mit der Verbindungszeichenfolge Ihres IoT Edge-Geräts. Stellen Sie sicher, dass alle anderen Bereitstellungsabschnitte auskommentiert sind.


   ```toml
      # Manual provisioning configuration using a connection string
      [provisioning]
      source = "manual"
      iothub_hostname = "<REQUIRED IOTHUB HOSTNAME>"
      device_id = "<REQUIRED DEVICE ID PROVISIONED IN IOTHUB>"
      dynamic_reprovisioning = false 
   ```

3. Speichern und schließen Sie die Datei (STRG+X, Y).

4. Wenden Sie die Konfiguration an. 

   Wenn Sie ein IoT Edge-Gerät verwenden, verwenden Sie den folgenden Befehl. 
   
   ```shell
      sudo iotedge config apply
   ```
   
   Wenn Sie ein IoT-Gerät verwenden und das `aziot-identity-service`-Paket installiert ist, verwenden Sie den folgenden Befehl. 
      
   ```shell
      sudo aziotctl config apply
   ```

5. Optional können Sie folgendermaßen überprüfen, ob die Dienste ausgeführt werden:

    ```shell
       sudo systemctl list-units --type=service | grep 'adu-agent\.service\|deliveryoptimization-agent\.service'
    ```

    Die Ausgabe sollte wie folgt aussehen:

    ```markdown
       adu-agent.service                   loaded active running Device Update for IoT Hub Agent daemon.

       deliveryoptimization-agent.service               loaded active running deliveryoptimization-agent.service: Performs content delivery optimization tasks   `
    ```

## <a name="add-a-tag-to-your-device"></a>Hinzufügen eines Tags zu Ihrem Gerät

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zum IoT Hub.

2. Suchen Sie im linken Navigationsbereich unter „IoT Devices“ (IoT-Geräte) oder „IoT Edge“ nach Ihrem IoT-Gerät, und navigieren Sie zum Gerätezwilling.

3. Löschen Sie im Gerätezwilling alle vorhandenen Device Update-Tagwerte, indem Sie diese auf NULL festlegen.

4. Fügen Sie wie unten gezeigt einen neuen Device Update-Tagwert hinzu.

```JSON
    "tags": {
            "ADUGroup": "<CustomTagValue>"
            }
```

## <a name="import-update"></a>Importieren des Updates

1. Laden Sie die folgende [apt-Manifestdatei](https://github.com/Azure/iot-hub-device-update/tree/main/docs/sample-artifacts/libcurl4-doc-apt-manifest.json) herunter, und [importieren Sie die Manifestdatei](https://github.com/Azure/iot-hub-device-update/tree/main/docs/sample-artifacts/sample-package-update-1.0.1-importManifest.json). Mit diesem apt-Manifest wird die neueste verfügbare Version von `libcurl4-doc package` auf Ihrem IoT-Gerät installiert. 

   Alternativ können Sie diese [apt-Manifestdatei](https://github.com/Azure/iot-hub-device-update/tree/main/docs/sample-artifacts/libcurl4-doc-7.58-apt-manifest.json) herunterladen und die [Manifestdatei importieren](https://github.com/Azure/iot-hub-device-update/tree/main/docs/sample-artifacts/sample-package-update-2-2.0.1-importManifest.json). Dadurch wird die spezifische Version v7.58.0 von `libcurl4-doc package` auf Ihrem IoT-Gerät installiert. 

2. Wählen Sie im Azure-Portal in der linken Navigationsleiste in Ihrem IoT Hub unter „Automatic Device Management“ (Automatische Geräteverwaltung) die Option „Device Updates“ aus.

3. Wählen Sie die Registerkarte „Updates“ aus.

4. Wählen Sie „+ Import New Update“ (Neues Update importieren) aus.

5. Wählen Sie das Ordnersymbol oder das Textfeld unter „Select an Import Manifest File“ (Importmanifestdatei auswählen) aus. Es wird ein Dialogfeld für Dateiauswahl angezeigt. Wählen Sie das zuvor heruntergeladene Importmanifest aus. Wählen Sie dann das Ordnersymbol oder das Textfeld unter „Select one or more update files“ (Mindestens eine Updatedatei auswählen) aus. Es wird ein Dialogfeld für Dateiauswahl angezeigt. Wählen Sie die zuvor heruntergeladene apt-Manifestupdatedatei aus.
   
   :::image type="content" source="media/import-update/select-update-files.png" alt-text="Screenshot: Auswahl der Updatedatei." lightbox="media/import-update/select-update-files.png":::

6. Wählen Sie unter „Select a storage container“ (Speichercontainer auswählen) das Ordnersymbol oder das Textfeld aus. Wählen Sie dann das entsprechende Speicherkonto aus.

7. Wenn Sie bereits einen Container erstellt haben, können Sie ihn wiederverwenden. (Wählen Sie andernfalls „+ Container“ aus, um einen neuen Speichercontainer für Updates zu erstellen.)  Wählen Sie den Container aus, den Sie verwenden möchten, und klicken Sie auf „Select“ (Auswählen).

   :::image type="content" source="media/import-update/container.png" alt-text="Screenshot: Auswahl des Containers." lightbox="media/import-update/container.png":::

8. Wählen Sie „Submit“ (Übermitteln) aus, um den Importvorgang zu starten.

9. Der Importvorgang beginnt, und der Bildschirm ändert sich in den Abschnitt „Import History“ (Importverlauf). Wählen Sie „Refresh“ (Aktualisieren) aus, um den Fortschritt anzuzeigen, bis der Importvorgang abgeschlossen ist. Abhängig von der Größe des Updates kann dieser Vorgang in wenigen Minuten abgeschlossen werden, er kann aber auch länger dauern.
   
   :::image type="content" source="media/import-update/update-publishing-sequence-2.png" alt-text="Screenshot: Updateimportsequenz." lightbox="media/import-update/update-publishing-sequence-2.png":::

10. Wenn in der Spalte „Status“ angezeigt wird, dass der Import erfolgreich war, wählen Sie den Header „Ready to Deploy“ (Bereit für die Bereitstellung) aus. Ihr importiertes Update sollte nun in der Liste angezeigt werden.

[Weitere Informationen](import-update.md) zum Importieren von Updates.

## <a name="create-update-group"></a>Erstellen der Updategruppe

1. Navigieren Sie zu dem IoT Hub, den Sie zuvor mit Ihrer Device Update-Instanz verbunden haben.

2. Wählen Sie in der linken Navigationsleiste unter „Automatic Device Management“ (Automatische Geräteverwaltung) die Option „Device Updates“ aus.

3. Wählen Sie oben auf der Seite die Registerkarte „Groups“ (Gruppen) aus. 

4. Wählen Sie die Schaltfläche „Add“ (Hinzufügen) aus, um eine neue Gruppe zu erstellen.

5. Wählen Sie das IoT Hub-Tag aus der Liste aus, das Sie im vorherigen Schritt erstellt haben. Wählen Sie „Create update group“ (Updategruppe erstellen) aus.

   :::image type="content" source="media/create-update-group/select-tag.PNG" alt-text="Screenshot: Tagauswahl." lightbox="media/create-update-group/select-tag.PNG":::

[Weitere Informationen](create-update-group.md) zum Hinzufügen von Tags und zum Erstellen von Updategruppen


## <a name="deploy-update"></a>Bereitstellen des Updates

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

Sie haben nun ein erfolgreiches End-to-End-Paketupdate mit Device Update for IoT Hub auf einem Ubuntu Server 18.04 x64-Gerät abgeschlossen. 

## <a name="bonus-steps"></a>Bonusschritte

1. Laden Sie die folgende [apt-Manifestdatei](https://github.com/Azure/iot-hub-device-update/tree/main/docs/sample-artifacts/libcurl4-doc-remove-apt-manifest.json) herunter, und [importieren Sie die Manifestdatei](https://github.com/Azure/iot-hub-device-update/tree/main/docs/sample-artifacts/sample-package-update-1.0.2-importManifest.json). Dieses apt-Manifest entfernt das installierte `libcurl4-doc package` von Ihrem IoT-Gerät. 

2. Wiederholen Sie die in den Abschnitten „Importieren des Updates“ und Bereitstellen des Updates“ beschriebenen Aktionen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie es nicht mehr benötigten, bereinigen Sie Ihr Konto für das Geräteupdate sowie die IoT Hub-Instanz und das IoT-Gerät. Sie können zu diesem Zweck zu jeder einzelnen Ressource navigieren und dann „Löschen“ auswählen. Beachten Sie, dass Sie eine Geräteupdateinstanz bereinigen müssen, bevor Sie das Geräteupdatekonto bereinigen. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Imageupdate auf Raspberry Pi 3 B+](device-update-raspberry-pi.md)

