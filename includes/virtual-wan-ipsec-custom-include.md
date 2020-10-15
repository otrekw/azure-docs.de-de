---
title: include file
description: include file
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 10/07/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e2a950037aed2a8ded4d4e55920721285cbfc05c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "82204628"
---
Beachten Sie bei der Arbeit mit benutzerdefinierten IPsec-Richtlinien die folgenden Anforderungen:

* **IKE**: Für IKE können Sie einen beliebigen Parameter aus der IKE-Verschlüsselung, einen beliebigen Parameter aus der IKE-Integrität sowie einen beliebigen Parameter aus der DH-Gruppe auswählen.
* **IPsec**: Für IPsec können Sie einen beliebigen Parameter aus der IPsec-Verschlüsselung, einen beliebigen Parameter aus der IPsec-Integrität sowie PFS auswählen. Falls einer der Parameter für die IPsec-Verschlüsselung oder die IPsec-Integrität GCM ist, müssen die Parameter für beide Einstellungen GCM sein.

>[!NOTE]
> Bei benutzerdefinierten IPsec-Richtlinien besteht kein Konzept von Antwortdienst und Initiator (im Gegensatz zu IPsec-Standardrichtlinien). Beide Seiten (lokale und Azure-VPN-Gateways) verwenden die gleichen Einstellungen für IKE Phase 1 und IKE Phase 2. Es werden beide Protokolle – IKEv1 und IKEv2 – unterstützt. Azure in der ausschließlichen Verwendung als Antwortdienst wird nicht unterstützt.
>

**Verfügbare Einstellungen und Parameter**

| Einstellung | Parameter |
|--- |--- |
| IKE-Verschlüsselung | GCMAES256, GCMAES128, AES256, AES128 |
| IKE-Integrität | SHA384, SHA256 |
| DH-Gruppe | ECP384, ECP256, DHGroup24, DHGroup14 |
| IPsec-Verschlüsselung | GCMAES256, GCMAES128, AES256, AES128, Keine |
| IPsec-Integrität | GCMAES256, GCMAES128, SHA256 |
| PFS-Gruppe | ECP384, ECP256, PFS24, PFS14, Keine |
