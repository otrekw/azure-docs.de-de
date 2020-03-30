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
ms.openlocfilehash: ddab6927044ce638e50ff3ad79aa3c35d046c820
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "69520841"
---
Die folgende Konfiguration wurde für die unten stehenden Schritte verwendet:

  | | |
  |---|---|
  |Computer| Ubuntu Server 18.04|
  |Abhängigkeiten| strongSwan |


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

[Weitere Anweisungen zum Installieren der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli-apt?view=azure-cli-latest)
