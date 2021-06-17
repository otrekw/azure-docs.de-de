---
author: v-dalc
ms.service: databox
ms.author: alkohli
ms.topic: include
ms.date: 06/08/2021
ms.openlocfilehash: 6bce3c5a01d182c67525caca9b75e3a03e59a4d9
ms.sourcegitcommit: e39ad7e8db27c97c8fb0d6afa322d4d135fd2066
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111983221"
---
Verwenden Sie die Runtimeantworten des IoT Edge-Agents, um auf Compute-Instanzen bezogene Fehler zu beheben. Im Folgenden sehen Sie eine Liste der möglichen Antworten:

* 200 – OK
* 400 – Die Bereitstellungskonfiguration ist falsch formatiert oder ungültig.
* 417 – Für das Gerät ist keine Bereitstellungskonfiguration festgelegt.
* 412 – Die Schemaversion der Bereitstellungskonfiguration ist ungültig.
* 406 – das IoT Edge-Gerät ist offline oder sendet keine Statusberichte.
* 500 – in der IoT Edge-Runtime ist ein Fehler aufgetreten.

Weitere Informationen finden Sie unter [IoT Edge-Agent](../articles/iot-edge/iot-edge-runtime.md?preserve-view=true&view=iotedge-2018-06#iot-edge-agent).
