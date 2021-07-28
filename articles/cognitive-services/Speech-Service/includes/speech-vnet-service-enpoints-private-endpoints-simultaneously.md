---
author: alexeyo26
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 03/19/2021
ms.author: alexeyo
ms.openlocfilehash: 8a1253e7ac88d2fe8b557c48dd846b48de59fba3
ms.sourcegitcommit: 6ed3928efe4734513bad388737dd6d27c4c602fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "110059151"
---
## <a name="simultaneous-use-of-private-endpoints-and-vnet-service-endpoints"></a>Gleichzeitige Verwendung privater Endpunkte und VNet-Dienstendpunkte

[Private Endpunkte](../speech-services-private-link.md) und [VNet-Dienstendpunkte](../speech-service-vnet-service-endpoint.md) können für den gleichzeitigen Zugriff auf dieselbe Speech-Ressource verwendet werden. Um jedoch private Endpunkte und VNet-Dienstendpunkte gleichzeitig zu aktivieren, müssen Sie die Option **Ausgewählte Netzwerke und private Endpunkte** in den Netzwerkeinstellungen der Speech-Ressource im Azure-Portal verwenden. Alle anderen Optionen werden für dieses Szenario nicht unterstützt.
