---
title: 'Azure VMware Solution by CloudSimple: Dienst'
description: Hier erfahren Sie mehr über den CloudSimple-Dienst. Nach Erstellung des Diensts können Sie Knoten erwerben, Knoten reservieren und private Clouds erstellen.
author: Ajayan1008
ms.author: v-hborys
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 8f32197eda4fc7632e883fd21dd6e1caa0dbd24b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97898741"
---
# <a name="cloudsimple-service-overview"></a>Übersicht über den CloudSimple-Dienst

Der CloudSimple-Dienst ermöglicht es Ihnen, Azure VMware Solution by CloudSimple zu nutzen.  Nach Erstellung des Diensts können Sie Knoten erwerben, Knoten reservieren und private Clouds erstellen.  Sie erstellen den CloudSimple-Dienst in jeder Azure-Region, in der der CloudSimple-Dienst verfügbar ist. Der Dienst definiert das Umkreisnetzwerk von Azure VMware Solution by CloudSimple. Dieses Umkreisnetzwerk unterstützt unter anderem Dienste wie VPN, ExpressRoute und Internetkonnektivität mit Ihren privaten Clouds.

## <a name="gateway-subnet"></a>Gatewaysubnetz

Ein Gatewaysubnetz ist für jeden CloudSimple-Dienst erforderlich und für die Region spezifisch, in der es erstellt wird. Das Gatewaysubnetz wird beim Erstellen des Umkreisnetzwerks verwendet und erfordert einen CIDR-Block vom Typ „/28“.  Der Adressraum des Gatewaysubnetzes muss eindeutig sein. Er darf sich nicht mit einem Netzwerk überschneiden, das mit der CloudSimple-Umgebung kommuniziert. Zu den Netzwerken, die mit CloudSimple kommunizieren, gehören unter anderem lokale Netzwerke und das virtuelle Azure-Netzwerk.  Ein Gatewaysubnetz kann nach der Erstellung nicht mehr gelöscht werden.  Das Gatewaysubnetz wird entfernt, wenn der Dienst gelöscht wird.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über das [Erstellen eines CloudSimple-Diensts in Azure](quickstart-create-cloudsimple-service.md).
