---
title: Problembehandlung für den Defender-IoT-Micro-Agent (Vorschau)
description: Erfahren Sie, wie Sie unerwartete oder unerklärliche Fehler behandeln.
ms.date: 4/5/2021
ms.topic: reference
ms.openlocfilehash: 23722195f4a7f7a19f10962ce648bc1a6e2a4012
ms.sourcegitcommit: a038863c0a99dfda16133bcb08b172b6b4c86db8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2021
ms.locfileid: "113016369"
---
# <a name="defender-iot-micro-agent-troubleshooting-preview"></a>Problembehandlung für den Defender-IoT-Micro-Agent (Vorschau)

Wenn ein unerwarteter Fehler auftritt, können Sie mit diesen Problembehandlungsmethoden das Problem beheben. Sie können sich bei Bedarf auch an das Produktteam von Azure Defender für IoT wenden, um Unterstützung zu erhalten.   

## <a name="service-status"></a>Dienststatus 

So zeigen Sie den Status des Diensts an 

1. Führen Sie den folgenden Befehl aus.

    ```bash
    systemctl status defender-iot-micro-agent.service 
    ```

1. Überprüfen Sie, ob der Dienst stabil ist, indem Sie sicherstellen, dass er `active` ist und dass die Uptime angemessen ist.

    :::image type="content" source="media/troubleshooting/active-running.png" alt-text="Überprüfen der Dienststabilität durch aktiven Status und angemessene Uptime":::

Wenn für den Dienst nicht `inactive` angezeigt wird, führen Sie zum Starten des Diensts den folgenden Befehl aus:

```bash
systemctl start defender-iot-micro-agent.service 
```

Sie erkennen, dass der Dienst abgestürzt ist, wenn die Uptime des Prozesses weniger als zwei Minuten beträgt. Um dieses Problem zu beheben, [sehen Sie sich die Protokolle an](#review-the-logs).

## <a name="validate-micro-agent-root-privileges"></a>Überprüfen von Micro-Agent-Stammberechtigungen

Mit dem folgenden Befehl können Sie überprüfen, ob der Defender für IoT-Micro-Agent-Dienst mit Root-Berechtigungen ausgeführt wird.

```bash
ps -aux | grep " defender-iot-micro-agent"
```

:::image type="content" source="media/troubleshooting/root-privileges.png" alt-text="Überprüfen der Ausführung des Defender für IoT-Micro-Agent-Diensts mit Root-Berechtigungen":::
## <a name="review-the-logs"></a>Überprüfen der Protokolle 

Verwenden Sie zum Anzeigen der Protokolle den folgenden Befehl:  

```bash
sudo journalctl -u defender-iot-micro-agent | tail -n 200 
```

### <a name="quick-log-review"></a>Schnelle Protokollüberprüfung

Wenn beim Ausführen des Micro-Agents ein Problem auftritt, können Sie den Micro-Agent in einem temporären Zustand ausführen, sodass Sie die Protokolle mit dem folgenden Befehl anzeigen können:

```bash
sudo systectl stop defender-iot-micro-agent
cd /var/defender_iot_micro_agent/
sudo ./defender_iot_micro_agent
```

## <a name="restart-the-service"></a>Starten Sie den Dienst neu.

Führen Sie den folgenden Befehl aus, um den Dienst neu zu starten: 

```bash
sudo systemctl restart defender-iot-micro-agent 
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Funktionsunterstützung und -einstellung](edge-security-module-deprecation.md).
