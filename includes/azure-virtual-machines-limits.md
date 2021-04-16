---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 11/09/2018
ms.author: cynthn
ms.openlocfilehash: 994a7726adec07f2f6533d460d05469a0f3c7bf3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "87102692"
---
| Resource | Begrenzung |
| --- | --- |
| Virtuelle Computer pro Clouddienst<sup>1</sup> |50 |
| Eingabeendpunkte pro Clouddienst<sup>2</sup> |150 |

<sup>1</sup> Virtuelle Computer, die mithilfe des klassischen Bereitstellungsmodells anstelle von Azure Resource Manager erstellt werden, werden automatisch in einem Clouddienst gespeichert. Für den Lastenausgleich und die Verfügbarkeit können Sie diesem Clouddienst weitere virtuelle Computer hinzufügen. 

<sup>2</sup> Eingabeendpunkte ermöglichen die Kommunikation mit einem virtuellen Computer von außerhalb des Clouddiensts des virtuellen Computers. Virtuelle Computer im gleichen Clouddienst oder virtuellen Netzwerk können automatisch miteinander kommunizieren.  
