---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/14/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 089ad704a466590a9649107fef245a88d6a4d6e3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "102244717"
---
Die folgende Konfiguration wurde für die unten stehenden Schritte verwendet:

- Computer: Ubuntu Server 18.04
- Abhängigkeiten: strongSwan


Verwenden Sie die folgenden Befehle, um die erforderliche strongSwan-Konfiguration zu installieren:

```
sudo apt install strongswan
```

```
sudo apt install strongswan-pki
```

```
sudo apt install libstrongswan-extra-plugins
```

Verwenden Sie den folgenden Befehl, um die Azure-Befehlszeilenschnittstelle zu installieren:

```
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

[Weitere Anweisungen zum Installieren der Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli-apt)