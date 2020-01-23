---
title: Ressourcenverfügbarkeit in verschiedenen Region
description: Verfügbarkeit von Compute- und Arbeitsspeicherressourcen für den Azure Container Instances-Dienst in verschiedenen Azure-Regionen
ms.topic: article
ms.date: 12/17/2019
ms.author: danlep
ms.openlocfilehash: 9de5b08eed3aa10015813cbb4724ef4e947005fb
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2020
ms.locfileid: "75888004"
---
# <a name="resource-availability-for-azure-container-instances-in-azure-regions"></a>Ressourcenverfügbarkeit für Azure Container Instances in Azure-Regionen

In diesem Artikel wird die Verfügbarkeit der Compute- und Arbeitsspeicherressourcen von Azure Container Instances in Azure-Regionen erläutert. 

Die angezeigten Werte stellen die maximalen Ressourcen dar, die pro Bereitstellung einer [Containergruppe](container-instances-container-groups.md) zur Verfügung stehen. Die Werte sind zum Zeitpunkt der Veröffentlichung aktuell. 

> [!NOTE]
> Containergruppen, die innerhalb dieser Ressourcenlimits erstellt werden, unterliegen der Verfügbarkeit in der Bereitstellungsregion. Wenn eine Region stark ausgelastet ist, kann bei der Bereitstellung von Instanzen ein Fehler auftreten. Um einen solchen Fehler bei der Bereitstellung zu beheben, versuchen Sie, Instanzen mit niedrigeren Ressourceneinstellungen bereitzustellen, oder führen Sie die Bereitstellung zu einem späteren Zeitpunkt durch.

Weitere Informationen zu Kontingenten und anderen Grenzwerten in Ihren Bereitstellungen finden Sie unter [Kontingente und Regionsverfügbarkeit für Azure Container Instances](container-instances-quotas.md).

## <a name="availability---general"></a>Verfügbarkeit: allgemein

Die folgenden Regionen und Ressourcen stehen Containergruppen mit Linux- und [unterstützten](container-instances-faq.md#what-windows-base-os-images-are-supported) Windows Server 2016-basierten Containern zur Verfügung.

| Regions | OS | Maximale CPU-Auslastung | Maximaler Speicher (GB) |
| -------- | -- | :---: | :-----------: |
| „Brasilien, Süden“, „Kanada, Mitte“, „Indien, Mitte“, „USA, Mitte“, „Asien, Osten“, „USA, Osten“, „USA, Osten 2“, „Europa, Norden“, „USA, Süden-Mitte“, „Asien, Südosten“, „Indien, Süden“, „Vereinigtes Königreich, Süden“, „Europa, Westen“, „USA, Westen“, „USA, Westen 2“ | Linux | 4 | 16 |
| „Australien, Osten“, „Japan, Osten“ | Linux | 2 | 8 |
| USA Nord Mitte | Linux | 2 | 3,5 |
| „Brasilien, Süden“, „Japan, Osten“, „Europa, Westen“ | Windows | 4 | 16 |
| USA, Osten; USA, Westen | Windows | 4 | 14 |
| „Australien, Osten“, „Kanada, Mitte“, „Indien, Mitte“, „USA, Mitte“, „Asien, Osten“, „USA, Osten 2“, „USA, Norden-Mitte“, „Europa, Norden“, „USA, Süden-Mitte“, „Asien, Südosten“, „Indien, Süden“, „Vereinigtes Königreich, Süden“, „USA, Westen 2“ | Windows | 2 | 3,5 |

## <a name="availability---windows-server-2019-ltsc-1809-deployments-preview"></a>Verfügbarkeit – Windows Server 2019 LTSC, 1809 Bereitstellungen (Vorschauversion)

Die folgenden Regionen und Ressourcen stehen Containergruppen mit Windows Server 2019-basierten Containern zur Verfügung (Vorschauversion).

| Regions | OS | Maximale CPU-Auslastung | Maximaler Speicher (GB) |
| -------- | -- | :---: | :-----------: |
| „Australien, Osten“, „Brasilien, Süden“, „Kanada, Mitte“, „Indien, Mitte“, „USA, Mitte“, „Asien, Osten“, „USA, Osten“, „USA, Norden-Mitte“, „Europa, Norden“, „USA, Süden-Mitte“, „Asien, Südosten“, „Indien, Süden“, „Vereinigtes Königreich, Süden“, „Europa, Westen“ | Windows | 4 | 16 |
| „USA, Osten 2“, „USA, Westen 2“ | Windows | 2 | 3,5 |


## <a name="availability---virtual-network-deployment"></a>Verfügbarkeit: Bereitstellung eines virtuellen Netzwerks

Für eine Containergruppe, die in einem [virtuellen Azure-Netzwerk](container-instances-vnet.md) bereitgestellt wird, sind die folgenden Regionen und Ressourcen verfügbar.

[!INCLUDE [container-instances-vnet-limits](../../includes/container-instances-vnet-limits.md)]

## <a name="availability---gpu-resources-preview"></a>Verfügbarkeit: GPU-Ressourcen (Vorschauversion)

Für eine Containergruppe, die mit [GPU-Ressourcen](container-instances-gpu.md) (Vorschau) bereitgestellt wird, sind die folgenden Regionen und Ressourcen verfügbar.

[!INCLUDE [container-instances-gpu-regions](../../includes/container-instances-gpu-regions.md)]
[!INCLUDE [container-instances-gpu-limits](../../includes/container-instances-gpu-limits.md)]

## <a name="next-steps"></a>Nächste Schritte

Teilen Sie dem Team auf [aka.ms/aci/feedback](https://aka.ms/aci/feedback) mit, wenn zusätzliche Regionen und eine Erhöhung der Ressourcenverfügbarkeit erforderlich sind.

Informationen zur Problembehandlung bei der Bereitstellung von Containerinstanzen finden Sie unter [Beheben von Bereitstellungsproblemen für Azure Container Instances](container-instances-troubleshooting.md).
