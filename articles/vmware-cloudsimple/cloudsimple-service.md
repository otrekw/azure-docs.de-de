---
title: 'Azure VMware Solution by CloudSimple: Dienst'
description: Hier erfahren Sie mehr über den CloudSimple-Dienst. Nach Erstellung des Diensts können Sie Knoten erwerben, Knoten reservieren und private Clouds erstellen.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: ec77209c0995bf5aec0957c8d4d2269f27d743bc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88142127"
---
# <a name="cloudsimple-service-overview"></a>Übersicht über den CloudSimple-Dienst

Der CloudSimple-Dienst ermöglicht es Ihnen, Azure VMware Solution by CloudSimple zu nutzen.  Nach Erstellung des Diensts können Sie Knoten erwerben, Knoten reservieren und private Clouds erstellen.  Sie erstellen den CloudSimple-Dienst in jeder Azure-Region, in der der CloudSimple-Dienst verfügbar ist. Der Dienst definiert das Umkreisnetzwerk von Azure VMware Solution by CloudSimple. Dieses Umkreisnetzwerk unterstützt unter anderem Dienste wie VPN, ExpressRoute und Internetkonnektivität mit Ihren privaten Clouds.

## <a name="gateway-subnet"></a>Gatewaysubnetz

Ein Gatewaysubnetz ist für jeden CloudSimple-Dienst erforderlich und für die Region spezifisch, in der es erstellt wird. Das Gatewaysubnetz wird beim Erstellen des Umkreisnetzwerks verwendet und erfordert einen CIDR-Block vom Typ „/28“.  Der Adressraum des Gatewaysubnetzes muss eindeutig sein. Er darf sich nicht mit einem Netzwerk überschneiden, das mit der CloudSimple-Umgebung kommuniziert. Zu den Netzwerken, die mit CloudSimple kommunizieren, gehören unter anderem lokale Netzwerke und das virtuelle Azure-Netzwerk.  Ein Gatewaysubnetz kann nach der Erstellung nicht mehr gelöscht werden.  Das Gatewaysubnetz wird entfernt, wenn der Dienst gelöscht wird.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über das [Erstellen eines CloudSimple-Diensts in Azure](quickstart-create-cloudsimple-service.md).
