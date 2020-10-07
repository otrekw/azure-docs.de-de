---
ms.openlocfilehash: e46a56742ab8b98c53c1cd05e840e8ad4b8a73da
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "88682158"
---
Erweitern Sie im Schritt [Generieren und Bereitstellen des IoT Edge-Bereitstellungsmanifests](../../../detect-motion-emit-events-quickstart.md#generate-and-deploy-the-deployment-manifest) in Visual Studio Code den Knoten **lva-sample-device** unter **AZURE IOT HUB** (im Abschnitt unten links). Die folgenden bereitgestellten Module sollten angezeigt werden:

* Live Video Analytics-Modul mit dem Namen `lvaEdge`
* Modul `rtspsim`, das einen RTSP-Server als Quelle eines Livevideofeeds simuliert

  ![Module](../../../media/quickstarts/lva-sample-device-node.png)

> [!NOTE]
> Wenn Sie anstelle des vom Setupskript bereitgestellten Edgegeräts Ihr eigenes Edgegerät verwenden, navigieren Sie zu diesem Gerät, und führen Sie die folgenden Befehle mit **Administratorrechten** aus, um die für diese Schnellstartanleitung verwendete Beispielvideodatei zu pullen und zu speichern.  

```
mkdir /home/lvaadmin/samples
mkdir /home/lvaadmin/samples/input    
curl https://lvamedia.blob.core.windows.net/public/camera-300s.mkv > /home/lvaadmin/samples/input/camera-300s.mkv  
chown -R lvaadmin /home/lvaadmin/samples/  
```
