---
title: 'Azure VMware Solutions (AVS): Dienst'
description: Hier finden Sie eine Übersicht über den AVS-Dienst und die zugehörigen Konzepte.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: d09c8c34093e7d33122f934138ff9fdf4842508e
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024951"
---
# <a name="avs-service-overview"></a>Übersicht über den AVS-Dienst

Der AVS-Dienst ermöglicht es Ihnen, Azure VMware Solution by AVS zu nutzen Nach der Erstellung des Diensts können Sie Knoten erwerben, Knoten reservieren und private AVS-Clouds erstellen. Sie erstellen den AVS-Dienst in jeder Azure-Region hinzu, in der der AVS-Dienst verfügbar ist. Der Dienst definiert das Umkreisnetzwerk von Azure VMware Solution by AVS. Dieses Umkreisnetzwerk unterstützt unter anderem Dienste wie VPN und ExpressRoute sowie Internetkonnektivität mit Ihren privaten AVS-Clouds.

## <a name="gateway-subnet"></a>Gatewaysubnetz

Ein Gatewaysubnetz ist für jeden einzelnen AVS-Dienst erforderlich und für die Region spezifisch, in der es erstellt wird. Das Gatewaysubnetz wird beim Erstellen des Umkreisnetzwerks verwendet und erfordert einen CIDR-Block vom Typ „/28“. Der Adressraum des Gatewaysubnetzes muss eindeutig sein. Er darf sich nicht mit dem eines Netzwerks überschneiden, das mit der AVS-Umgebung kommuniziert. Zu den Netzwerken, die mit AVS kommunizieren, gehören unter anderem lokale Netzwerke und virtuelle Azure-Netzwerke. Ein Gatewaysubnetz kann nach der Erstellung nicht mehr gelöscht werden. Das Gatewaysubnetz wird entfernt, wenn der Dienst gelöscht wird.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über das [Erstellen eines AVS-Diensts in Azure](quickstart-create-cloudsimple-service.md).
