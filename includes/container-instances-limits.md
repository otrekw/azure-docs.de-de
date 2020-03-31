---
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 02/13/2019
ms.author: danlep
ms.openlocfilehash: 33a82a55b03cde79d2d80826041ee6d43565476a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334816"
---
| Resource | Begrenzung |
| --- | :--- |
| Standard-SKU-Containergruppen pro Region pro [Abonnement](../articles/billing-buy-sign-up-azure-subscription.md) | 100<sup>1</sup> |
| Dedizierte SKU-Containergruppen pro Region pro [Abonnement](../articles/billing-buy-sign-up-azure-subscription.md) | 0<sup>1</sup> |
| Anzahl von Containern pro Containergruppe | 60 |
| Anzahl von Volumes pro Containergruppe | 20 |
| Ports pro IP-Adresse | 5 |
| Containerinstanz-Protokollgröße – ausgeführte Instanz | 4 MB |
| Containerinstanz-Protokollgröße – angehaltene Instanz | 16 KB oder 1.000 Zeilen |
| Containererstellungen pro Stunde |300<sup>1</sup> |
| Containererstellungen pro 5 Minuten | 100<sup>1</sup> |
| Containerlöschungen pro Stunde | 300<sup>1</sup> |
| Containerlöschungen pro 5 Minuten | 100<sup>1</sup> |


<sup>1</sup> Erstellen Sie eine [Azure Supportanfrage][azure-support], um eine Erhöhung des Grenzwerts anzufordern.<br />

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
