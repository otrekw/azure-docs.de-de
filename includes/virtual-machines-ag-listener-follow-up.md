---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 8ae22ec7f75b9cd0d7958977dfd97169706c9389
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "67178038"
---
Nachdem Sie den Verfügbarkeitsgruppenlistener erstellt haben, kann es erforderlich sein, die Clusterparameter „RegisterAllProvidersIP“ und „HostRecordTTL“ für die Listenerressource anzupassen. Diese Parameter können die Zeit für eine erneute Verbindung nach einem Failover verkürzen, was Verbindungstimeouts verhindern kann. Weitere Informationen zu diesen Parametern sowie Beispielcode finden Sie unter [Erstellen oder Konfigurieren eines Verfügbarkeitsgruppenlisteners](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).

