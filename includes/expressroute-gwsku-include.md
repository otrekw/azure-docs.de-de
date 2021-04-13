---
title: include file
description: include file
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: include
ms.date: 04/05/2021
ms.author: duau
ms.custom: include file
ms.openlocfilehash: 27f5755ce8b7d204cad6cdc2281d7992bf86615a
ms.sourcegitcommit: c2a41648315a95aa6340e67e600a52801af69ec7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106504685"
---
Beim Erstellen eines Gateways des virtuellen Netzwerks müssen Sie die gewünschte Gateway-SKU angeben. Wenn Sie eine höhere Gateway-SKU wählen, werden dem Gateway mehr CPUs und eine höhere Bandbreite zugewiesen. Infolgedessen unterstützt das Gateway einen höheren Netzwerkdurchsatz im virtuellen Netzwerk. 

Virtuelle ExpressRoute-Netzwerkgateways können folgende SKUs verwenden:

|     | Gemeinsame Verwendung von VPN-Gateway und ExpressRoute | FastPath | Maximale Anzahl von Leitungsverbindungen |
| --- | --- | --- | --- |
| **Standard** | Ja | Nein | 4 |
| **HighPerformance** | Ja | Nein | 4 |
| **UltraPerformance** | Ja | Ja | 16 |