---
author: baanders
description: 'Includedatei für Azure Digital Twins: Einrichten von Cloud Shell und IoT-Erweiterung'
ms.service: digital-twins
ms.topic: include
ms.date: 5/25/2020
ms.author: baanders
ms.openlocfilehash: 4aa016294f0ef3bd26f7f3ef6fa374e9367b672d
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2020
ms.locfileid: "85296966"
---
[!INCLUDE [cloud-shell-try-it.md](cloud-shell-try-it.md)]

### <a name="set-up-cloud-shell-session"></a>Einrichten einer Cloud Shell-Sitzung

Nach dem Öffnen eines Cloud Shell-Fensters müssen Sie sich zunächst anmelden und den Shellkontext für diese Sitzung auf Ihr Abonnement festlegen. Führen Sie diese Befehle in Ihrer Cloud Shell aus:

```azurecli
az login
az account set --subscription <your-Azure-subscription-ID>
```

Wenn Sie dieses Abonnement zum ersten Mal mit Azure Digital Twins nutzen, führen Sie diesen Befehl aus, um sich im Namespace von Azure Digital Twins zu registrieren. (Falls Sie sich nicht sicher sind, ist es in Ordnung, ihn erneut auszuführen, auch wenn Sie ihn irgendwann zuvor schon einmal ausgeführt haben.)

```azurecli
az provider register --namespace 'Microsoft.DigitalTwins'
```

Als Nächstes fügen Sie die [**Microsoft Azure IoT-Erweiterung für Azure CLI**](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot?view=azure-cli-latest) zu Ihrer Cloud Shell hinzu, um Befehle für die Interaktion mit Azure Digital Twins und anderen IoT-Diensten zu aktivieren. Verwenden Sie diesen Befehl zum Hinzufügen der Erweiterung:

   ```azurecli-interactive
   az extension add --name azure-iot
   ```

Wenn Sie die Erweiterung in der Vergangenheit installiert haben, lautet die Ausgabe möglicherweise: „Die Erweiterung ‚azure-iot‘ ist bereits installiert.“ Führen Sie in diesem Fall Folgendes aus, um sicherzustellen, dass Sie über das neueste Update verfügen: 

   ```azurecli-interactive
   az extension update --name azure-iot
   ```

Jetzt können Sie in der Cloud Shell mit Azure Digital Twins arbeiten.

Sie können dies durch Ausführung von `az dt -h` jederzeit überprüfen, um eine Liste der verfügbaren Azure Digital Twins-Befehle oberster Ebene anzuzeigen.