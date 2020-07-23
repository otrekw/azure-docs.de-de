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
ms.openlocfilehash: 2d0e171807985deaebe8faa625d6b767c2a3efd7
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86218302"
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

[Weitere Anweisungen zum Installieren der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli-apt?view=azure-cli-latest)
