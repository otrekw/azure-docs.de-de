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
ms.openlocfilehash: a852807ab685e85b76d26e5b39c99a32f645bbd7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "71838159"
---
Benutzerdefinierte Routen mit einem 0.0.0.0/0-Ziel und NSGs im Gatewaysubnetz-**werden nicht unterstützt**. Die Erstellung von Gateways mit dieser Konfiguration wird blockiert. Gateways benötigen für die ordnungsgemäße Funktion Zugriff auf die Verwaltungscontroller.
