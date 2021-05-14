---
author: baanders
description: Einschließen einer Datei zum Hochladen eines Modells in eine Instanz von Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 4/1/2021
ms.author: baanders
ms.openlocfilehash: 3a9ec169f91ebe048914c3ef2163e4fde7485389
ms.sourcegitcommit: 32ee8da1440a2d81c49ff25c5922f786e85109b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2021
ms.locfileid: "109783669"
---
Das Modell sieht so aus:
:::code language="json" source="~/digital-twins-docs-samples/models/Thermostat.json":::

Führen Sie den folgenden Azure CLI-Befehl aus, der das obige Modell als Inline-JSON hochlädt, um **dieses Modell in Ihre Twins-Instanz hochzuladen**. Sie können den Befehl in [Azure Cloud Shell](../articles/cloud-shell/overview.md) in Ihrem Browser (verwenden Sie die **Bash**-Umgebung) oder auf Ihrem Computer ausführen, wenn Sie die CLI [lokal installiert](/cli/azure/install-azure-cli) haben.

```azurecli-interactive
az dt model create --models '{  "@id": "dtmi:contosocom:DigitalTwins:Thermostat;1",  "@type": "Interface",  "@context": "dtmi:dtdl:context;2",  "contents": [    {      "@type": "Property",      "name": "Temperature",      "schema": "double"    }  ]}' --dt-name {digital_twins_instance_name}
```