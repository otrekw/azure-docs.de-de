---
title: Ressourcenverfügbarkeit in verschiedenen Region
description: Verfügbarkeit von Compute- und Arbeitsspeicherressourcen für den Azure Container Instances-Dienst in verschiedenen Azure-Regionen
ms.topic: article
ms.date: 04/27/2020
ms.author: danlep
ms.openlocfilehash: 119c1202fe59e02a1b6ff802e3dbdc4d52133abe
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "82583905"
---
# <a name="resource-availability-for-azure-container-instances-in-azure-regions"></a>Ressourcenverfügbarkeit für Azure Container Instances in Azure-Regionen

In diesem Artikel wird die Verfügbarkeit der Compute-, Arbeitsspeicher- und Speicherressourcen von Azure Container Instances in Azure-Regionen und nach Zielbetriebssystem erläutert. 

Die angezeigten Werte stellen die maximalen Ressourcen dar, die pro Bereitstellung einer [Containergruppe](container-instances-container-groups.md) zur Verfügung stehen. Die Werte sind zum Zeitpunkt der Veröffentlichung aktuell. 

> [!NOTE]
> Containergruppen, die innerhalb dieser Ressourcenlimits erstellt werden, unterliegen der Verfügbarkeit in der Bereitstellungsregion. Wenn eine Region stark ausgelastet ist, kann bei der Bereitstellung von Instanzen ein Fehler auftreten. Um einen solchen Fehler bei der Bereitstellung zu beheben, versuchen Sie, Instanzen mit niedrigeren Ressourceneinstellungen bereitzustellen, oder führen Sie die Bereitstellung zu einem späteren Zeitpunkt oder in einer anderen Region mit verfügbaren Ressourcen durch.

Weitere Informationen zu Kontingenten und anderen Grenzwerten in Ihren Bereitstellungen finden Sie unter [Kontingente und Regionsverfügbarkeit für Azure Container Instances](container-instances-quotas.md).

## <a name="availability---general"></a>Verfügbarkeit: allgemein

Die folgenden Regionen und maximalen Ressourcen stehen Containergruppen mit Containern auf der Basis von Linux und [unterstützten](container-instances-faq.md#what-windows-base-os-images-are-supported) Windows Server 2016-Versionen zur Verfügung.

| Regions | OS | Maximale CPU-Auslastung | Maximaler Speicher (GB) | Speicher (GB) |
| -------- | -- | :---: | :-----------: | :---: |
| „Brasilien, Süden“, „Kanada, Mitte“, „Indien, Mitte“, „USA, Mitte“, „Asien, Osten“, „USA, Osten“, „USA, Osten 2“, „Europa, Norden“, „USA, Süden-Mitte“, „Asien, Südosten“, „Indien, Süden“, „Vereinigtes Königreich, Süden“, „Europa, Westen“, „USA, Westen“, „USA, Westen 2“ | Linux | 4 | 16 | 50 |
| „Australien, Osten“, „Japan, Osten“ | Linux | 2 | 8 | 50 |
| USA Nord Mitte | Linux | 2 | 3,5 | 50 |
| „Brasilien, Süden“, „Japan, Osten“, „Europa, Westen“ | Windows | 4 | 16 | 20 |
| USA, Osten; USA, Westen | Windows | 4 | 14 | 20 |
| „Australien, Osten“, „Kanada, Mitte“, „Indien, Mitte“, „USA, Mitte“, „Asien, Osten“, „USA, Osten 2“, „USA, Norden-Mitte“, „Europa, Norden“, „USA, Süden-Mitte“, „Asien, Südosten“, „Indien, Süden“, „Vereinigtes Königreich, Süden“, „USA, Westen 2“ | Windows | 2 | 3,5 | 20 |

## <a name="availability---windows-server-2019-ltsc-1809-deployments-preview"></a>Verfügbarkeit – Windows Server 2019 LTSC, 1809 Bereitstellungen (Vorschauversion)

Die folgenden Regionen und maximalen Ressourcen stehen Containergruppen mit Windows Server 2019-basierten Containern zur Verfügung (Vorschauversion).

| Regions | OS | Maximale CPU-Auslastung | Maximaler Speicher (GB) | Speicher (GB) |
| -------- | -- | :---: | :-----------: | :---: |
| „Australien, Osten“, „Brasilien, Süden“, „Kanada, Mitte“, „Indien, Mitte“, „USA, Mitte“, „Asien, Osten“, „USA, Osten“, „USA, Norden-Mitte“, „Europa, Norden“, „USA, Süden-Mitte“, „Asien, Südosten“, „Indien, Süden“, „Vereinigtes Königreich, Süden“, „Europa, Westen“ | Windows | 4 | 16 | 20 |
| „USA, Osten 2“, „USA, Westen 2“ | Windows | 2 | 3,5 | 20 |


## <a name="availability---virtual-network-deployment"></a>Verfügbarkeit: Bereitstellung eines virtuellen Netzwerks

Für eine Containergruppe, die in einem [virtuellen Azure-Netzwerk](container-instances-vnet.md) bereitgestellt wird, sind die folgenden Regionen und maximalen Ressourcen verfügbar.

[!INCLUDE [container-instances-vnet-limits](../../includes/container-instances-vnet-limits.md)]

## <a name="availability---gpu-resources-preview"></a>Verfügbarkeit: GPU-Ressourcen (Vorschauversion)

Für eine Containergruppe, die mit [GPU-Ressourcen](container-instances-gpu.md) (Vorschauversion) bereitgestellt wird, sind die folgenden Regionen und maximalen Ressourcen verfügbar.

> [!IMPORTANT]
> GPU-Ressourcen sind nur auf Anforderung verfügbar. Um Zugriff auf GPU-Ressourcen anzufordern, senden Sie eine [Azure-Supportanfrage][azure-support].

[!INCLUDE [container-instances-gpu-regions](../../includes/container-instances-gpu-regions.md)]
[!INCLUDE [container-instances-gpu-limits](../../includes/container-instances-gpu-limits.md)]

## <a name="next-steps"></a>Nächste Schritte

Teilen Sie dem Team auf [aka.ms/aci/feedback](https://aka.ms/aci/feedback) mit, wenn zusätzliche Regionen und eine Erhöhung der Ressourcenverfügbarkeit erforderlich sind.

Informationen zur Problembehandlung bei der Bereitstellung von Containerinstanzen finden Sie unter [Beheben von Bereitstellungsproblemen für Azure Container Instances](container-instances-troubleshooting.md).


[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
