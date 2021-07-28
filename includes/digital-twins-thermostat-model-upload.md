---
author: baanders
description: Einschließen einer Datei zum Hochladen eines Modells in eine Instanz von Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 4/1/2021
ms.author: baanders
ms.openlocfilehash: 9fcbfc0b0fccb5f130f0c554bac93748176387b7
ms.sourcegitcommit: 070122ad3aba7c602bf004fbcf1c70419b48f29e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111438976"
---
Das Modell sieht so aus:
:::code language="json" source="~/digital-twins-docs-samples/models/Thermostat.json":::

Führen Sie den folgenden Azure CLI-Befehl aus, der das obige Modell als Inline-JSON hochlädt, um **dieses Modell in Ihre Twins-Instanz hochzuladen**. Sie können den Befehl in [Azure Cloud Shell](../articles/cloud-shell/overview.md) in Ihrem Browser (verwenden Sie die **Bash**-Umgebung) oder auf Ihrem Computer ausführen, wenn Sie die CLI [lokal installiert](/cli/azure/install-azure-cli) haben.

```azurecli-interactive
az dt model create --dt-name <instance-name> --models '{  "@id": "dtmi:contosocom:DigitalTwins:Thermostat;1",  "@type": "Interface",  "@context": "dtmi:dtdl:context;2",  "contents": [    {      "@type": "Property",      "name": "Temperature",      "schema": "double"    }  ]}' 
```