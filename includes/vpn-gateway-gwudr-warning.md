---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 09/28/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 97fde67c3ac7649418ed0239a2c7aa4f1a4b3f96
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "81275518"
---
Benutzerdefinierte Routen mit einem 0.0.0.0/0-Ziel und NSGs im Gatewaysubnetz **werden nicht unterstützt**. Die Erstellung von Gateways mit dieser Konfiguration wird blockiert. Gateways benötigen für die ordnungsgemäße Funktion Zugriff auf die Verwaltungscontroller. [BGP-Routenverteilung](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#border-gateway-protocol) sollte für das Gatewaysubnetz auf „Aktiviert“ festgelegt werden, um die Verfügbarkeit des Gateways zu gewährleisten. Ist diese Option deaktiviert, funktioniert das Gateway nicht.
