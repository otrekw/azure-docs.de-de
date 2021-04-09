---
ms.openlocfilehash: 3f92bae608284c8b619be34a0e08f15e831bf88e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101750097"
---
Erweitern Sie im Schritt [Generieren und Bereitstellen des IoT Edge-Bereitstellungsmanifests](../../../detect-motion-emit-events-quickstart.md#generate-and-deploy-the-deployment-manifest) in Visual Studio Code den Knoten **lva-sample-device** unter **AZURE IOT HUB** (im Abschnitt unten links). Die folgenden bereitgestellten Module sollten angezeigt werden:

* Live Video Analytics-Modul mit dem Namen `lvaEdge`
* Modul `rtspsim`, das einen RTSP-Server als Quelle eines Livevideofeeds simuliert

  ![Module](../../../media/quickstarts/lva-sample-device-node.png)

> [!NOTE]
> In den obigen Schritten wird vorausgesetzt, dass Sie den mit dem Setupskript erstellten virtuellen Computer verwenden. Gehen Sie wie folgt vor, falls Sie stattdessen Ihr eigenes Edgegerät verwenden: Navigieren Sie zum Gerät, und führen Sie die folgenden Befehle mit **Administratorrechten** aus, um die für diese Schnellstartanleitung verwendete Beispielvideodatei zu pullen und zu speichern:  

```
mkdir /home/lvaedgeuser/samples
mkdir /home/lvaedgeuser/samples/input    
curl https://lvamedia.blob.core.windows.net/public/camera-300s.mkv > /home/lvaedgeuser/samples/input/camera-300s.mkv  
chown -R lvalvaedgeuser:localusergroup /home/lvaedgeuser/samples/  
```
