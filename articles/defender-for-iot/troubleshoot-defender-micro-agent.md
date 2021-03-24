---
title: Problembehandlung für den Defender-IoT-Micro-Agent (Vorschau)
description: Erfahren Sie, wie Sie unerwartete oder unerklärliche Fehler behandeln.
ms.date: 1/24/2021
ms.topic: reference
ms.openlocfilehash: 51550a4d3e5042fed7cadc4eac10a0074e954f19
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2021
ms.locfileid: "104782451"
---
# <a name="defender-iot-micro-agent-troubleshooting-preview"></a>Problembehandlung für den Defender-IoT-Micro-Agent (Vorschau)

Verwenden Sie die folgenden Methoden zur Problembehandlung, um unerwartete oder unerklärliche Fehler zu beheben. Sie können sich bei Bedarf auch an das Produktteam von Azure Defender für IoT wenden, um Unterstützung zu erhalten.   

## <a name="service-status"></a>Dienststatus 

So zeigen Sie den Status des Diensts an 

1. Führen Sie den folgenden Befehl aus.

    ```azurecli
    systemctl status defender-iot-micro-agent.service 
    ```

1. Überprüfen Sie, ob der Dienst stabil ist, indem Sie sicherstellen, dass er `active` ist und dass die Uptime angemessen ist.

    :::image type="content" source="media/troubleshooting/active-running.png" alt-text="Überprüfen der Dienststabilität durch aktiven Status und angemessene Uptime":::

Wenn für den Dienst nicht `inactive` angezeigt wird, führen Sie zum Starten des Diensts den folgenden Befehl aus:

```azurecli
systemctl start defender-iot-micro-agent.service 
```

Sie erkennen, dass der Dienst abgestürzt ist, wenn die Uptime des Prozesses zu kurz ist. Um dieses Problem zu beheben, sehen Sie sich die Protokolle an.

## <a name="review-logs"></a>Überprüfen von Protokollen 

Mit dem folgenden Befehl können Sie überprüfen, ob der Defender für IoT-Micro-Agent-Dienst mit Root-Berechtigungen ausgeführt wird.

```azurecli
ps -aux | grep " defender-iot-micro-agent"
```

:::image type="content" source="media/troubleshooting/root-privileges.png" alt-text="Überprüfen der Ausführung des Defender für IoT-Micro-Agent-Diensts mit Root-Berechtigungen":::

Verwenden Sie zum Anzeigen der Protokolle den folgenden Befehl:  

```azurecli
sudo journalctl -u defender-iot-micro-agent | tail -n 200 
```

Führen Sie den folgenden Befehl aus, um den Dienst neu zu starten: 

```azurecli
sudo systemctl restart defender-iot-micro-agent  
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Funktionsunterstützung und -einstellung](edge-security-module-deprecation.md).
