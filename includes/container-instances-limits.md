---
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 07/22/2020
ms.author: danlep
ms.openlocfilehash: c232875715253b3601c7f9a5ecc289705e3dcf40
ms.sourcegitcommit: a5dd9799fa93c175b4644c9fe1509e9f97506cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108212630"
---
| Resource | Begrenzung |
| --- | :--- |
| Standard-SKU-Containergruppen pro Region pro Abonnement | 100<sup>1</sup> |
| Dedizierte SKU-Containergruppen pro Region pro Abonnement | 0<sup>1</sup> |
| Anzahl von Containern pro Containergruppe | 60 |
| Anzahl von Volumes pro Containergruppe | 20 |
| Standard-SKU-Kerne (CPUs) pro Region pro Abonnement | 10<sup>1,2</sup> | 
| Standard-SKU-Kerne (CPUs) für K80-GPU pro Region pro Abonnement | 18<sup>1,2</sup> |
| Standard-SKU-Kerne (CPUs) für P100- oder V100-GPU pro Region pro Abonnement | 0<sup>1,2</sup> |
| Ports pro IP-Adresse | 5 |
| Containerinstanz-Protokollgröße – ausgeführte Instanz | 4 MB |
| Containerinstanz-Protokollgröße – angehaltene Instanz | 16 KB oder 1.000 Zeilen |
| Containergruppenerstellungen pro Stunde |300<sup>1</sup> |
| Containergruppenerstellungen pro 5 Minuten | 100<sup>1</sup> |
| Containergruppenlöschungen pro Stunde | 300<sup>1</sup> |
| Containergruppenlöschungen pro 5 Minuten | 100<sup>1</sup> |


<sup>1</sup> Erstellen Sie eine [Azure Supportanfrage][azure-support], um eine Erhöhung des Grenzwerts anzufordern. Kostenlose Abonnements, einschließlich [kostenloses Azure-Konto](https://azure.microsoft.com/offers/ms-azr-0044p/) und [Microsoft Azure for Students](https://azure.microsoft.com/offers/ms-azr-0170p/), sind nicht für Grenzwert- oder Kontingenterhöhungen qualifiziert. Wenn Sie über ein kostenloses Abonnement verfügen, können Sie ein [Upgrade](../articles/cost-management-billing/manage/upgrade-azure-subscription.md) auf ein Abonnement mit nutzungsbasierter Bezahlung durchführen.<br />
<sup>2</sup>Standardgrenzwert für Abonnement mit [nutzungsbasierter Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/). Der Grenzwert kann für andere Kategorietypen abweichen.<br/>

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
