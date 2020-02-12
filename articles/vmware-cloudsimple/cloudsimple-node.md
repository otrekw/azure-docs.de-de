---
title: 'Azure VMware Solutions (AVS): Übersicht über Knoten'
description: Hier erfahren Sie mehr über AVS-Knoten und -Konzepte.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 08d8fc3e6f1f2f83cf3c4fee3fdafb0bd07e336c
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024924"
---
# <a name="avs-nodes-overview"></a>Übersicht über AVS-Knoten

Knoten sind die Bausteine einer privaten AVS-Cloud. Ein Knoten ist:

* Ein dedizierter Bare-Metal-Computehost, auf dem ein VMware ESXi-Hypervisor installiert ist  
* Eine Computingeinheit, die Sie erwerben oder reservieren können, um private AVS-Clouds zu erstellen
* Verfügbar für den Kauf oder die Reservierung in einer Region, in der der AVS-Dienst verfügbar ist

Sie erstellen eine private AVS-Cloud aus den erworbenen Knoten. Zum Erstellen einer privaten AVS-Cloud benötigen Sie mindestens drei Knoten derselben SKU. Wenn Sie eine private AVS-Cloud erweitern möchten, fügen Sie zusätzliche Knoten hinzu. Sie können Knoten zu einem vorhandenen Cluster hinzufügen oder einen neuen Cluster erstellen, indem Sie Knoten im Azure-Portal erwerben und diese dem AVS-Dienst zuordnen. Alle erworbenen Knoten werden im AVS-Dienst angezeigt. 

## <a name="provisioned-nodes"></a>Bereitgestellte Knoten

Bereitgestellte Knoten bieten Kapazität mit nutzungsbasierter Bezahlung. Durch das Bereitstellen von Knoten können Sie Ihren VMware-Cluster nach Bedarf schnell skalieren. Sie können Knoten bedarfsgerecht hinzufügen, oder Sie können einen bereitgestellten Knoten löschen, um Ihren VMware-Cluster herunterzuskalieren. Bereitgestellte Knoten werden monatlich in Rechnung gestellt und dem Abonnement belastet, in dem sie bereitgestellt werden.

* Wenn Sie Ihr Azure-Abonnement per Kreditkarte bezahlen, wird die Karte sofort belastet.
* Wenn Sie jeweils eine Rechnung erhalten, werden die Gebühren auf Ihrer nächsten Rechnung aufgeführt.

## <a name="vmware-solution-by-avs-nodes-sku"></a>SKU für VMware Solution by AVS-Knoten

Es können Knoten der folgenden Typen bereitgestellt oder reserviert werden.

| SKU           | CS28-Knoten                 | CS36-Knoten                 | CS36m-Knoten                |
|---------------|-----------------------------|-----------------------------|-----------------------------|
| Region        | USA, Osten; USA, Westen            | USA, Osten; USA, Westen            | Europa, Westen                 |
| CPU           | 2x2,2 GHz, 28 Kerne (56 HT) | 2x2,3 GHz, 36 Kerne (72 HT) | 2x2,3 GHz, 36 Kerne (72 HT) |
| RAM           | 256 GB                      | 512 GB                      | 576 GB                      |
| Cachedatenträger    | 1,6 TB NVMe                 | 3,2 TB NVMe                 | 3,2 TB NVMe                 |
| Datenträgerkapazität | 5,625 TB unformatiert                | 11,25 TB unformatiert                | 15,36 TB unformatiert                |
| Speichertyp  | Nur Flash                   | Nur Flash                   | Nur Flash                   |

## <a name="limits"></a>Einschränkungen

Für private AVS-Clouds gelten die folgenden Knotengrenzwerte:

| Resource | Begrenzung |
|----------|-------|
| Mindestanzahl von Knoten zum Erstellen einer privaten AVS-Cloud | 3 |
| Maximale Anzahl von Knoten in einem Cluster in einer privaten AVS-Cloud | 16 |
| Maximale Anzahl von Knoten in einer privaten AVS-Cloud | 64 |
| Mindestanzahl von Knoten in einem neuem Cluster | 3 |

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [Knoten erwerben](create-nodes.md).
* Erfahren Sie mehr über [private AVS-Clouds](cloudsimple-private-cloud.md).
* Erfahren Sie, wie Sie [Knoten bereitstellen](create-nodes.md).
* Erfahren Sie mehr über [private Clouds](cloudsimple-private-cloud.md).
