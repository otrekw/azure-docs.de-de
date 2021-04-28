---
author: baanders
description: Einschließen einer Datei zum Hochladen eines Modells in eine Instanz von Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 4/1/2021
ms.author: baanders
ms.openlocfilehash: add49cabaece1187cb9bcda3a94c92feb08b2439
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107304295"
---
Das Modell sieht so aus:
:::code language="json" source="~/digital-twins-docs-samples/models/Thermostat.json":::

Führen Sie den folgenden Azure CLI-Befehl aus, der das obige Modell als Inline-JSON hochlädt, um **dieses Modell in Ihre Twins-Instanz hochzuladen**. Sie können den Befehl in [Azure Cloud Shell](../articles/cloud-shell/overview.md) in Ihrem Browser (verwenden Sie die **Bash**-Umgebung) oder auf Ihrem Computer ausführen, wenn Sie die CLI [lokal installiert](/cli/azure/install-azure-cli) haben.

```azurecli-interactive
az dt model create --models '{  "@id": "dtmi:contosocom:DigitalTwins:Thermostat;1",  "@type": "Interface",  "@context": "dtmi:dtdl:context;2",  "contents": [    {      "@type": "Property",      "name": "Temperature",      "schema": "double"    }  ]}' -n {digital_twins_instance_name}
```