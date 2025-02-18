---
title: Authentifizierung des Sicherheits-Agents (Vorschau)
description: Führen Sie die Micro-Agent-Authentifizierung mit zwei möglichen Methoden aus.
ms.date: 1/20/2021
ms.topic: conceptual
ms.openlocfilehash: 8dd8abaedaaababf4d84330c5bf8cb030bac55bd
ms.sourcegitcommit: a038863c0a99dfda16133bcb08b172b6b4c86db8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2021
ms.locfileid: "113016109"
---
# <a name="micro-agent-authentication-methods-preview"></a>Authentifizierungsmethoden für den Micro-Agent (Vorschau)

Es gibt zwei Authentifizierungsoptionen mit dem Micro-Agent von Defender für IoT: 

- Verbindungszeichenfolge 

- Zertifikat 

## <a name="authentication-using-a-connection-string"></a>Authentifizierung mithilfe einer Verbindungszeichenfolge 

Um eine Verbindungszeichenfolge zu verwenden, müssen Sie eine Datei, die die mit UTF-8 codierte Verbindungszeichenfolge verwendet, im Verzeichnis des Defender-Agents einer Datei namens `connection_string.txt` hinzufügen. Beispiel:

```azurecli
echo “<connection string>” > connection_string.txt 
```

Anschließend müssen Sie den Dienst mit diesem Befehl neu starten.

```azurecli
sudo systemctl restart defender-iot-micro-agent.service
``` 

## <a name="authentication-using-a-certificate"></a>Authentifizierung mithilfe eines Zertifikats 


So erfolgt die Authentifizierung mithilfe eines Zertifikats 

1. Legen Sie den mit PEM codierten öffentlichen Teil eines Zertifikats im Verzeichnis des Defender-Agents in einer Datei mit dem Namen `certificate_public.pem` ab.
1. Legen Sie den mit PEM codierten privaten Schlüssel im Verzeichnis des Defender-Agents in einer Datei mit dem Namen `certificate_private.pem` ab.
1. Platzieren Sie die passende Verbindungszeichenfolge in der Datei `connection_string.txt`. Beispiel:

    ```azurecli
    HostName=<the host name of the iot hub>;DeviceId=<the id of the device>;ModuleId=<the id of the module>;x509=true 
    ```

    Diese Aktion zeigt an, dass der Defender-Agent zur Authentifizierung die Bereitstellung eines Zertifikats erwartet. 

1. Starten Sie den Dienst mithilfe des folgenden Codes neu: 

    ```azurecli
    sudo systemctl restart defender-iot-micro-agent.service 
    ```

## <a name="ensure-the-micro-agent-is-running-correctly"></a>Sicherstellen, dass der Micro-Agent ordnungsgemäß ausgeführt wird 

1. Führen Sie den folgenden Befehl aus: 
    ```azurecli
    systemctl status defender-iot-micro-agent.service 
    ```
1. Prüfen Sie, ob der Dienst stabil ist, indem Sie sicherstellen, dass er **aktiv** ist und dass die Betriebszeit des Prozesses angemessen ist: 

    :::image type="content" source="media/concept-security-agent-authentication/active.png" alt-text="Stellen Sie sicher, dass Ihr Dienst stabil ist, indem Sie sicherstellen, dass er aktiv ist.":::

## <a name="next-steps"></a>Nächste Schritte

Überprüfen Sie [Sicherheitsstatus: CIS-Benchmark](concept-security-posture.md).
