---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 11/09/2018
ms.author: cynthn
ms.openlocfilehash: 994a7726adec07f2f6533d460d05469a0f3c7bf3
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87102692"
---
| Resource | Begrenzung |
| --- | --- |
| Virtuelle Computer pro Clouddienst<sup>1</sup> |50 |
| Eingabeendpunkte pro Clouddienst<sup>2</sup> |150 |

<sup>1</sup> Virtuelle Computer, die mithilfe des klassischen Bereitstellungsmodells anstelle von Azure Resource Manager erstellt werden, werden automatisch in einem Clouddienst gespeichert. Für den Lastenausgleich und die Verfügbarkeit können Sie diesem Clouddienst weitere virtuelle Computer hinzufügen. 

<sup>2</sup> Eingabeendpunkte ermöglichen die Kommunikation mit einem virtuellen Computer von außerhalb des Clouddiensts des virtuellen Computers. Virtuelle Computer im gleichen Clouddienst oder virtuellen Netzwerk können automatisch miteinander kommunizieren.  
