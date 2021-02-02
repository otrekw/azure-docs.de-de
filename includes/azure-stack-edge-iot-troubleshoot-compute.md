---
author: v-dalc
ms.service: databox
ms.author: alkohli
ms.topic: include
ms.date: 01/21/2021
ms.openlocfilehash: 3defa62c55bb5ab042ade816f611ea45b39a0117
ms.sourcegitcommit: 3c3ec8cd21f2b0671bcd2230fc22e4b4adb11ce7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98761544"
---
Verwenden Sie die Runtimeantworten des IoT Edge-Agents, um auf Compute-Instanzen bezogene Fehler zu beheben. Im Folgenden sehen Sie eine Liste der möglichen Antworten:

* 200 – OK
* 400 – Die Bereitstellungskonfiguration ist falsch formatiert oder ungültig.
* 417 – Für das Gerät ist keine Bereitstellungskonfiguration festgelegt.
* 412 – Die Schemaversion der Bereitstellungskonfiguration ist ungültig.
* 406 – das IoT Edge-Gerät ist offline oder sendet keine Statusberichte.
* 500 – in der IoT Edge-Runtime ist ein Fehler aufgetreten.

Weitere Informationen finden Sie unter [IoT Edge-Agent](/azure/iot-edge/iot-edge-runtime?view=iotedge-2018-06&preserve-view=true#iot-edge-agent).