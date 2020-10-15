---
author: baanders
description: 'Includedatei für Azure Digital Twins: Einrichten von Cloud Shell und IoT-Erweiterung'
ms.service: digital-twins
ms.topic: include
ms.date: 7/17/2020
ms.author: baanders
ms.openlocfilehash: b664303d86f8588fc210b11b363b21d523d63295
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87507418"
---
Gehen Sie wie folgt vor, um Azure Digital Twins in einem geöffneten [Azure Cloud Shell](https://shell.azure.com)-Fenster zu verwenden: Melden Sie sich an, und legen Sie zunächst den Shellkontext für diese Sitzung auf Ihr Abonnement fest. Führen Sie diese Befehle in Ihrer Cloud Shell aus:

```azurecli
az login
az account set --subscription "<your-Azure-subscription-ID>"
```
> [!TIP]
> Sie können im obigen Befehl anstelle der ID auch Ihren Abonnementnamen verwenden. 

Wenn Sie dieses Abonnement zum ersten Mal mit Azure Digital Twins nutzen, führen Sie diesen Befehl aus, um sich im Namespace von Azure Digital Twins zu registrieren. (Falls Sie sich nicht sicher sind, ist es in Ordnung, ihn erneut auszuführen, auch wenn Sie ihn irgendwann zuvor schon einmal ausgeführt haben.)

```azurecli
az provider register --namespace 'Microsoft.DigitalTwins'
```

Als Nächstes fügen Sie die [**Microsoft Azure IoT-Erweiterung für Azure CLI**](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot?view=azure-cli-latest) zu Ihrer Cloud Shell hinzu, um Befehle für die Interaktion mit Azure Digital Twins und anderen IoT-Diensten zu aktivieren. 

[!INCLUDE [digital-twins-cloud-shell-extensions.md](digital-twins-cloud-shell-extensions.md)]

Jetzt können Sie in der Cloud Shell mit Azure Digital Twins arbeiten.

Sie können dies durch Ausführung von `az dt -h` jederzeit überprüfen, um eine Liste der verfügbaren Azure Digital Twins-Befehle oberster Ebene anzuzeigen.