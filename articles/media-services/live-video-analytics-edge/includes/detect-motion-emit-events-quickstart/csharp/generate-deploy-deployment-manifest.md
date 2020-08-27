---
ms.openlocfilehash: 38d96ccb9f2c7b24e57b1096996df1ea760aca37
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88690962"
---
## <a name="generate-and-deploy-the-deployment-manifest"></a>Generieren und Bereitstellen des Bereitstellungsmanifests

Mit dem Bereitstellungsmanifest wird definiert, welche Module auf einem Edgegerät bereitgestellt werden. Darüber hinaus werden auch die Konfigurationseinstellungen für diese Module definiert. 

Führen Sie die folgenden Schritte aus, um das Manifest auf der Grundlage der Vorlagendatei zu generieren und dann auf dem Edgegerät bereitzustellen.

1. Öffnen Sie Visual Studio Code.
1. Wählen Sie neben dem Bereich **AZURE IOT HUB** das Symbol **Weitere Aktionen** aus, um die IoT Hub-Verbindungszeichenfolge festzulegen. Sie können die Zeichenfolge aus der Datei *src/cloud-to-device-console-app/appsettings.json* kopieren. 

    ![Festlegen der IoT-Verbindungszeichenfolge](../../../media/quickstarts/set-iotconnection-string.png)
1. Klicken Sie mit der rechten Maustaste auf **src/edge/deployment.template.json**, und klicken Sie dann auf **IoT Edge-Bereitstellungsmanifest generieren**.

    ![IoT Edge-Bereitstellungsmanifest generieren](../../../media/quickstarts/generate-iot-edge-deployment-manifest.png)

    Mit dieser Aktion sollte eine Manifestdatei mit dem Namen *deployment.amd64.json* im Ordner *src/edge/config* erstellt werden.
1. Klicken Sie mit der rechten Maustaste auf **src/edge/config/deployment.amd64.json**, und wählen Sie **Create Deployment for Single Device** (Bereitstellung für einzelnes Gerät erstellen) und dann den Namen Ihres Edgegeräts aus.

    ![Bereitstellung für einzelnes Gerät erstellen](../../../media/quickstarts/create-deployment-single-device.png)

1. Wählen Sie bei Anzeige der Aufforderung zum Auswählen eines IoT Hub-Geräts im Dropdownmenü die Option **lva-sample-device** aus.
1. Aktualisieren Sie nach ungefähr 30 Sekunden unten links im Fenster den Dienst „Azure IoT Hub“. Das Edgegerät zeigt nun die folgenden bereitgestellten Module an:

    * Live Video Analytics in IoT Edge (Modulname: `lvaEdge`)
    * RTSP-Simulator (Real-Time Streaming Protocol) (Modulname: `rtspsim`)

Das RTSP-Simulatormodul simuliert einen Livevideostream. Hierfür wird eine Videodatei verwendet, die beim Ausführen des [Setupskripts für Live Video Analytics-Ressourcen](https://github.com/Azure/live-video-analytics/tree/master/edge/setup) auf Ihr Edgegerät kopiert wurde. 

> [!NOTE]
> Wenn Sie anstelle des vom Setupskript bereitgestellten Edgegeräts Ihr eigenes Edgegerät verwenden, navigieren Sie zu diesem Gerät, und führen Sie die folgenden Befehle mit **Administratorrechten** aus, um die für diese Schnellstartanleitung verwendete Beispielvideodatei zu pullen und zu speichern.  

```
mkdir /home/lvaadmin/samples      
mkdir /home/lvaadmin/samples/input    
curl https://lvamedia.blob.core.windows.net/public/camera-300s.mkv > /home/lvaadmin/samples/input/camera-300s.mkv  
chown -R lvaadmin /home/lvaadmin/samples/  
```
An diesem Punkt werden die Module bereitgestellt, aber es sind keine Mediengraphen aktiv.
