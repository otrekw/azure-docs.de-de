---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 02/10/2020
ms.author: cynthn
ms.openlocfilehash: 8607fd6f01ea02841017d09dac7236812f2330b9
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77133727"
---
| Resource | Standardlimit |
| --- | --- |
| VMs pro [Abonnement](../articles/billing-buy-sign-up-azure-subscription.md) |25.000<sup>1</sup> pro Region |
| Gesamte Kerne pro [Abonnement](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> pro Region Wenden Sie sich an den Support, um diesen Grenzwert heraufzusetzen. |
| Azure Spot-VM: Kerne gesamt pro [Abonnement](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> pro Region Wenden Sie sich an den Support, um diesen Grenzwert heraufzusetzen. |
| VM-Kerne pro Serie (z. B. Dv2 und F) pro [Abonnement](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> pro Region Wenden Sie sich an den Support, um diesen Grenzwert heraufzusetzen. |
| [Verfügbarkeitsgruppen](../articles/virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) pro Abonnement |2\.000 pro Region |
| Virtuelle Computer pro Verfügbarkeitsgruppe | 200 |
| Zertifikate pro Abonnement |Unbegrenzt<sup>2</sup> |

<sup>1</sup>Standardgrenzwerte variieren nach angebotenem Kategorietyp, z.B. kostenlose Testversion, nutzungsbasierte Bezahlung, und nach Serie, wie z.B. Dv2, F und G. Der Standard für Enterprise Agreement-Abonnements ist z.B. 350.

<sup>2</sup> Beim Azure-Ressourcen-Manager werden Zertifikate im Azure-Schlüsseltresor gespeichert. Die Anzahl der Zertifikate für ein Abonnement ist unbegrenzt. Es besteht ein Grenzwert von 1 MB an Zertifikaten pro Bereitstellung, die entweder aus einer einzelnen VM oder einer Verfügbarkeitsgruppe besteht.

> [!NOTE]
> Für VM-Kerne gilt ein regionaler Gesamtgrenzwert. Außerdem gilt ein Grenzwert für die regionaler Serien pro Größe wie Dv2 und F. Diese Grenzwerte werden separat erzwungen. Angenommen, Sie verwenden ein Abonnement mit einem Kerngesamtgrenzwert von 30 für VMs in der Region „USA, Osten“, einem Kerngrenzwert von 30 für die A-Serie und einem Kerngrenzwert von 30 für die D-Serie. Für dieses Abonnement dürfen dann 30 virtuelle A1-Computer bzw. 30 virtuelle D1-Computer bereitgestellt werden – oder eine Kombination daraus, bei der die Gesamtanzahl von 30 Kernen nicht überschritten wird. Ein Beispiel wäre eine Kombination aus 10 A1-VMs und 20 D1-VMs.  
> <!-- -->
> 
