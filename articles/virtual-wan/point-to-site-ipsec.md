---
title: IPsec-Richtlinien für Point-to-Site-Verbindungen in Azure Virtual WAN
titleSuffix: Azure Virtual WAN
description: Erfahren Sie mehr über IPsec-Richtlinien für Point-to-Site-Verbindungen in Azure Virtual WAN.
services: virtual-wan
author: wellee
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 02/24/2021
ms.author: wellee
ms.openlocfilehash: d64748bf7719af52b76c5e4141bfbbabe80bbae2
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101743493"
---
# <a name="point-to-site-ipsec-policies"></a>IPsec-Richtlinien für Point-to-site-Verbindungen

In diesem Artikel werden die unterstützten IPSec-Richtlinienkombinationen für die Point-to-Site-VPN-Konnektivität in Azure Virtual WAN erläutert.

## <a name="default-ipsec-policies"></a>IPsec-Standardrichtlinien

In der folgenden Tabelle sind die IPsec-Standardparameter für Point-to-Site-VPN-Verbindungen aufgeführt. Diese Parameter werden automatisch vom Point-to-Site-VPN-Gateway im Azure Virtual WAN ausgewählt, wenn ein Point-to-Site-Profil erstellt wird.

| Einstellung | Parameter |
|--- |--- |
| Phase 1, IKE-Verschlüsselung | AES256 |
| Phase 1, IKE-Integrität |  SHA256 |
| DH-Gruppe | DHGroup24 |
| Phase 2, IPsec-Verschlüsselung | GCMAES256|
| Phase 2, IPsec-Integrität | GCMAES25 |
| PFS-Gruppe |PFS24|

## <a name="custom-ipsec-policies"></a>Benutzerdefinierte IPsec-Richtlinien

Beachten Sie bei der Arbeit mit benutzerdefinierten IPsec-Richtlinien die folgenden Anforderungen:

* **IKE**: Für IKE in Phase 1 können Sie einen beliebigen Parameter der IKE-Verschlüsselung, einen beliebigen Parameter der IKE-Integrität sowie einen beliebigen Parameter der DH-Gruppe auswählen.
* **IPsec**: Für IPsec in Phase 2 können Sie einen beliebigen Parameter der IPsec-Verschlüsselung, einen beliebigen Parameter der IPsec-Integrität sowie PFS auswählen. Wenn ein Parameter für IPsec-Verschlüsselung oder IPsec-Integrität GCM lautet, müssen für IPsec-Verschlüsselung und IPsec-Integrität derselbe Algorithmus verwendet werden. Wenn beispielsweise GCMAES128 für die IPsec-Verschlüsselung ausgewählt wird, muss GCMAES128 auch für die IPsec-Integrität ausgewählt werden.  

In der folgenden Tabelle sind die verfügbaren IPsec-Parameter für Point-to-Site-VPN-Verbindungen aufgeführt.

| Einstellung | Parameter |
|--- |--- |
| Phase 1, IKE-Verschlüsselung | AES128, AES256, GCMAES128, GCMAES256 |
| Phase 1, IKE-Integrität |  SHA256, SHA384 |
| DH-Gruppe | DHGroup14, DHGroup24, ECP256, ECP384 |
| Phase 2, IPsec-Verschlüsselung | GCMAES128, GCMAES256, SHA256|
| Phase 2, IPsec-Integrität | GCMAES128, GCMAES256 |
| PFS-Gruppe |PFS14, PFS24, ECP256, ECP384|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Herstellen einer Point-to-Site-Verbindung](virtual-wan-point-to-site-portal.md).

Weitere Informationen zu Virtual WAN finden Sie unter [Virtual WAN – Häufig gestellte Fragen](virtual-wan-faq.md).
