---
title: Dienstkontingente und regionale Verfügbarkeit
description: Kontingente, Limits und Regionsverfügbarkeit des Azure Container Instances-Diensts.
ms.topic: article
ms.date: 07/22/2020
ms.openlocfilehash: eaaa8e0b2d72aaea546a1bc351da40932c1deb14
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "87384826"
---
# <a name="quotas-and-limits-for-azure-container-instances"></a>Kontingente und Limits für Azure Container Instances

Für die Ressourcen und Funktionen aller Azure-Dienste gelten bestimmte Standardlimits und Kontingente. In diesem Artikel werden die Standardkontingente und Limits für Azure Container Instances erläutert.

Die Verfügbarkeit von Compute-, Arbeitsspeicher- und Speicherressourcen für Azure Container Instances variiert abhängig von der Region und vom Betriebssystem. Einzelheiten finden Sie unter [Ressourcenverfügbarkeit für Azure Container Instances](container-instances-region-availability.md).

Verwenden Sie die [Listenverwendungs](/rest/api/container-instances/location/listusage)-API, um die aktuelle Kontingentverwendung in einer Region für ein Abonnement zu überprüfen.

## <a name="service-quotas-and-limits"></a>Dienstkontingente und Limits

[!INCLUDE [container-instances-limits](../../includes/container-instances-limits.md)]

## <a name="next-steps"></a>Nächste Schritte

Bestimmte Standardlimits und Kontingente können erhöht werden. Wenn Sie eine Erhöhung von Ressourcen anfordern möchten, für die dies unterstützt wird, müssen Sie eine [Azure-Supportanfrage][azure-support] übermitteln und dabei als **Problemtyp** die Option „Kontingent“ auswählen.

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
