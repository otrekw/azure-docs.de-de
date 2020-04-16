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
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "81275518"
---
Benutzerdefinierte Routen mit einem 0.0.0.0/0-Ziel und NSGs im Gatewaysubnetz **werden nicht unterstützt**. Die Erstellung von Gateways mit dieser Konfiguration wird blockiert. Gateways benötigen für die ordnungsgemäße Funktion Zugriff auf die Verwaltungscontroller. [BGP-Routenverteilung](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#border-gateway-protocol) sollte für das Gatewaysubnetz auf „Aktiviert“ festgelegt werden, um die Verfügbarkeit des Gateways zu gewährleisten. Ist diese Option deaktiviert, funktioniert das Gateway nicht.
