---
author: rothja
ms.service: cost-management-billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 3295c7e677e4c9bf7bff27614fe5fa4fa3200570
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334644"
---
| Resource | Begrenzung |
| --- | --- |
| [Web- oder Workerrollen pro Bereitstellung](../articles/cloud-services/cloud-services-choose-me.md)<sup>1</sup> |25 |
| [Instanz-Eingabeendpunkte](/previous-versions/azure/reference/gg557552(v=azure.100)#instanceinputendpoint) pro Bereitstellung |25 |
| [Eingabeendpunkte](/previous-versions/azure/reference/gg557552(v=azure.100)#inputendpoint) pro Bereitstellung |25 |
| [Interne Endpunkte](/previous-versions/azure/reference/gg557552(v=azure.100)#internalendpoint) pro Bereitstellung |25 |
| [Zertifikate für gehostete Dienste](../articles/cloud-services/cloud-services-certs-create.md#what-are-service-certificates) pro Bereitstellung |199 |

<sup>1</sup>Jeder Azure-Clouddienst mit Web- oder Workerrollen kann zwei Bereitstellungen haben: je eine für Produktions- und Stagingumgebung. Dieser Grenzwert bezieht sich auf die Anzahl der unterschiedlichen Rollen, also Konfigurationen. Dieser Grenzwert bezieht sich nicht auf die Anzahl der Instanzen pro Rolle, d.h. die Skalierung.

