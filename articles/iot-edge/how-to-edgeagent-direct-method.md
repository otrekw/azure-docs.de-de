---
title: Integrierte direkte edgeAgent-Methoden – Azure IoT Edge
description: Überwachen und Verwalten von IoT Edge-Bereitstellungen mithilfe der direkten Methoden, die im Laufzeitmodul des IoT Edge-Agents enthalten sind
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/02/2020
ms.topic: conceptual
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 57b9d46918414cef9e8cbcffb941b98c98f985ff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240347"
---
# <a name="communicate-with-edgeagent-using-built-in-direct-methods"></a>Kommunikation mit edgeAgent über integrierte direkte Methoden

Überwachen und verwalten Sie IoT Edge-Bereitstellungen mithilfe der direkten Methoden, die im IoT Edge-Agentmodul enthalten sind. Direkte Methoden werden auf dem Gerät implementiert und können dann über die Cloud aufgerufen werden. Der IoT Edge-Agent umfasst direkte Methoden, mit denen Sie Ihre IoT Edge-Geräte remote überwachen und verwalten können.

Weitere Informationen zu direkten Methoden sowie zu ihrer Verwendung und Implementierung in Ihren eigenen Modulen finden Sie unter [Verstehen und Aufrufen direkter Methoden von IoT Hub](../iot-hub/iot-hub-devguide-direct-methods.md).

Bei den Namen dieser direkten Methoden wird die Groß-/Kleinschreibung nicht beachtet.

## <a name="ping"></a>Pingen

Mit der **ping**-Methode kann überprüft werden, ob IoT Edge auf einem Gerät ausgeführt wird oder ob das Gerät über eine offene Verbindung mit IoT Hub verfügt. Verwenden Sie diese direkte Methode, um den IoT Edge-Agent zu pingen und seinen Status abzurufen. Bei einem erfolgreichen Ping wird eine leere Nutzlast und **"status": 200** zurückgegeben.

Beispiel:

```azurecli
az iot hub invoke-module-method --method-name 'ping' -n <hub name> -d <device name> -m '$edgeAgent'
```

Rufen Sie im Azure-Portal die Methode mit dem Namen `ping` und einer leeren JSON-Nutzlast `{}` auf.

![Aufruf der direkten Methode „ping“ im Azure-Portal](./media/how-to-edgeagent-direct-method/ping-direct-method.png)

## <a name="restart-module"></a>Neustart des Moduls

Die Methode **RestartModule** ermöglicht die Remoteverwaltung von Modulen, die auf einem IoT Edge-Gerät ausgeführt werden. Wenn ein Modul einen fehlerhaften Zustand oder ein fehlerhaftes Verhalten meldet, können Sie den Neustart des Moduls durch den IoT Edge-Agent auslösen. Bei einem erfolgreichen Befehl für den Neustart wird eine leere Nutzlast und **"status": 200** zurückgegeben.

Die RestartModule-Methode ist in IoT Edge-Version 1.0.9 und höher verfügbar. 

Sie können die direkte Methode „RestartModule“ auf jedem Modul verwenden, das auf einem IoT Edge-Gerät ausgeführt wird. Dazu zählt auch das edgeAgent-Modul selbst. Wenn Sie diese direkte Methode jedoch zum Herunterfahren des edgeAgent-Moduls verwenden, wird kein erfolgreiches Ergebnis zurückgegeben. Der Grund dafür ist, dass die Verbindung beim Neustart des Moduls unterbrochen wird.

Beispiel:

```azurecli
az iot hub invoke-module-method --method-name 'RestartModule' -n <hub name> -d <device name> -m '$edgeAgent' --method-payload \
'
    {
        "schemaVersion": "1.0",
        "id": "<module name>"
    }
'
```

Rufen Sie im Azure-Portal die Methode mit dem Namen `RestartModule` und der folgenden JSON-Nutzlast auf:

```json
{
    "schemaVersion": "1.0",
    "id": "<module name>"
}
```

![Aufruf der direkten Methode „RestartModule“ im Azure-Portal](./media/how-to-edgeagent-direct-method/restartmodule-direct-method.png)

## <a name="experimental-methods"></a>Experimentelle Methoden

Optionen für neue direkte Methoden können als experimentelle Features getestet werden. Dazu zählen u. a.:

* [UploadLogs](https://github.com/Azure/iotedge/blob/master/doc/built-in-logs-pull.md): Rufen Sie Modulprotokolle ab, und laden Sie sie in Azure Blob Storage hoch.
* [GetTaskStatus](https://github.com/Azure/iotedge/blob/master/doc/built-in-logs-pull.md#gettaskstatus): Überprüfen Sie den Status einer Anforderung zum Hochladen von Protokollen.
* [GetLogs](https://github.com/Azure/iotedge/blob/master/doc/built-in-logs-pull.md#getlogs): Rufen Sie Modulprotokolle inline in der Antwort der direkten Methode ab.

## <a name="next-steps"></a>Nächste Schritte

[Eigenschaften der Modulzwillinge von IoT Edge-Agent und IoT Edge-Hub](module-edgeagent-edgehub.md)
