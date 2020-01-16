---
title: include file
description: include file
services: virtual-machines-linux
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 06/19/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 202bffb03a73acde67f0d4a03b31aa9a6fbf9824
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2020
ms.locfileid: "75902025"
---
## <a name="deployment-considerations"></a>Überlegungen zur Bereitstellung

* Informationen zur Verfügbarkeit von virtuellen Computern der N-Serie finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/).

* Virtuelle Computer der N-Serie können nur im Resourcen Manager-Bereitstellungsmodell bereitgestellt werden.

* Virtuelle Computer der Serie N unterscheiden hinsichtlich des Typs von Azure Storage, den sie für ihre Datenträger unterstützen. Virtuelle NC- oder NV-Computer unterstützen nur VM-Datenträger, die durch Standard Disk Storage (HDD) gesichert sind. Virtuelle NCv2-, NCv3-, ND-, NDv2- und NVv2-Computer unterstützen nur VM-Datenträger, die durch Premium Disk Storage (SSD) gesichert sind.

* Wenn Sie eine größere Anzahl von virtuellen Computern der N-Serie bereitstellen möchten, sollten Sie ein nutzungsbasiertes Abonnement oder andere Kaufoptionen in Erwägung ziehen. Bei Verwendung eines [kostenlosen Azure-Kontos](https://azure.microsoft.com/free/)können Sie nur eine begrenzte Anzahl von Azure-Compute-Kernen nutzen.

* Möglicherweise müssen Sie das Kernnutzungskontingent (pro Region) in Ihrem Azure-Abonnement sowie das separate Kontingent für NC-, NCv2-, NCv3-, ND-, NDv2-, NV- oder NVv2-Kerne erhöhen. In diesem Fall können Sie kostenlos [eine Anfrage an den Onlinekundensupport richten](../articles/azure-portal/supportability/how-to-create-azure-support-request.md) und eine Erhöhung des Kontingents anfordern. Standardgrenzwerte variieren unter Umständen je nach Abonnementkategorie.




