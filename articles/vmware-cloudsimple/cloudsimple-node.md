---
title: Azure VMware Solution by CloudSimple – Übersicht über Knoten
description: Hier erfahren Sie mehr über CloudSimple-Konzepte, u. a. zu Knoten, bereitgestellten Knoten, Private Cloud und SKUs für VMware Solution by CloudSimple-Knoten.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 7de4b5faa997d909089daedab7e48e5d5a6de2e7
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "88140835"
---
# <a name="cloudsimple-nodes-overview"></a>Übersicht über CloudSimple-Knoten

Knoten sind die Bausteine einer privaten Cloud. Ein Knoten ist:

* Ein dedizierter Bare-Metal-Computehost, auf dem ein VMware ESXi-Hypervisor installiert ist  
* Eine Recheneinheit, die Sie bereitstellen oder reservieren können, um Private Clouds zu erstellen
* Verfügbar, um in einer Region bereitgestellt oder reserviert zu werden, in der der CloudSimple-Service verfügbar ist

Sie erstellen eine Private Cloud aus den bereitgestellten Knoten. Um eine private Cloud zu erstellen, benötigen Sie mindestens drei Knoten derselben SKU. Wenn Sie eine private Cloud erweitern möchten, fügen Sie zusätzliche Knoten hinzu.  Sie können Knoten zu einem vorhandenen Cluster hinzufügen oder einen neuen Cluster erstellen, indem Sie Knoten im Azure-Portal bereitstellen und diese dem CloudSimple-Dienst zuordnen.  Alle bereitgestellten Knoten werden unter dem CloudSimple-Dienst angezeigt.  

## <a name="provisioned-nodes"></a>Bereitgestellte Knoten

Bereitgestellte Knoten bieten Kapazität mit nutzungsbasierter Bezahlung. Durch das Bereitstellen von Knoten können Sie Ihren VMware-Cluster nach Bedarf schnell skalieren. Sie können Knoten bedarfsgerecht hinzufügen, oder Sie können einen bereitgestellten Knoten löschen, um Ihren VMware-Cluster herunterzuskalieren. Bereitgestellte Knoten werden monatlich in Rechnung gestellt und dem Abonnement belastet, in dem sie bereitgestellt werden.

* Wenn Sie Ihr Azure-Abonnement per Kreditkarte bezahlen, wird die Karte sofort belastet.
* Wenn Sie jeweils eine Rechnung erhalten, werden die Gebühren auf Ihrer nächsten Rechnung aufgeführt.

## <a name="vmware-solution-by-cloudsimple-nodes-sku"></a>SKU für VMware Solution by CloudSimple-Knoten

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

Für private Clouds gelten die folgenden Knotengrenzwerte.

| Resource | Begrenzung |
|----------|-------|
| Mindestanzahl von Knoten zum Erstellen einer privaten Cloud | 3 |
| Maximale Anzahl von Knoten in einem Cluster in einer privaten Cloud | 16 |
| Maximale Anzahl von Knoten in einer privaten Cloud | 64 |
| Mindestanzahl von Knoten in einem neuem Cluster | 3 |

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [Knoten bereitstellen](create-nodes.md).
* Erfahren Sie mehr über [private Clouds](cloudsimple-private-cloud.md).
