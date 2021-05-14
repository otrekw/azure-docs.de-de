---
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 02/14/2020
ms.author: genli
ms.openlocfilehash: 280943bd965c4799ce294321129d1088be9c0caf
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "96015921"
---
## <a name="scenario"></a>Szenario

Zur besseren Veranschaulichung der Konfiguration einer statischen IP-Adresse für einen virtuellen Computer wird in diesem Dokument das folgende Szenario verwendet:

![Szenario für virtuelle Netzwerke: Front-End- und Back-End-Subnetze mit einer statischen IP-Adresse für das Front-End-Subnetz](./media/virtual-networks-static-ip-scenario-include/static-ip-scenario.png)

In diesem Szenario erstellen Sie im *FrontEnd*-Subnetz einen virtuellen Computer mit dem Namen *DNS01*, und dann legen Sie dafür fest, dass die statische IP-Adresse *192.168.1.101* verwendet wird.
