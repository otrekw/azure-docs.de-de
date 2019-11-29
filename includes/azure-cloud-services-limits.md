---
author: rothja
ms.service: cost-management-billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 24e327a0b669df247d44e4bf8f05b693abb49990
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74224521"
---
| Resource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| [Web- oder Workerrollen pro Bereitstellung](../articles/cloud-services/cloud-services-choose-me.md)<sup>1</sup> |25 |25 |
| [Instanz-Eingabeendpunkte](/previous-versions/azure/reference/gg557552(v=azure.100)#instanceinputendpoint) pro Bereitstellung |25 |25 |
| [Eingabeendpunkte](/previous-versions/azure/reference/gg557552(v=azure.100)#inputendpoint) pro Bereitstellung |25 |25 |
| [Interne Endpunkte](/previous-versions/azure/reference/gg557552(v=azure.100)#internalendpoint) pro Bereitstellung |25 |25 |
| [Zertifikate für gehostete Dienste](../articles/cloud-services/cloud-services-certs-create.md#what-are-service-certificates) pro Bereitstellung |199 |199 |

<sup>1</sup>Jeder Azure-Clouddienst mit Web- oder Workerrollen kann zwei Bereitstellungen haben: je eine für Produktions- und Stagingumgebung. Dieser Grenzwert bezieht sich auf die Anzahl der unterschiedlichen Rollen, also Konfigurationen. Dieser Grenzwert bezieht sich nicht auf die Anzahl der Instanzen pro Rolle, d.h. die Skalierung.

