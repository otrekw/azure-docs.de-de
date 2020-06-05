---
title: Übereinstimmungsbedingungen der Regel-Engine für Azure CDN Premium von Verizon | Microsoft-Dokumentation
description: Referenzdokumentation zu den Übereinstimmungsbedingungen der Regel-Engine für Azure Content Delivery Network Premium von Verizon.
services: cdn
author: asudbring
ms.service: azure-cdn
ms.topic: article
ms.date: 05/26/2020
ms.author: allensu
ms.openlocfilehash: ecec7c3c5e4cc72ee637eb91033b9f2b0f158f1a
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83872556"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-match-conditions"></a>Übereinstimmungsbedingungen der Regel-Engine für Azure CDN Premium von Verizon

Dieser Artikel enthält ausführliche Beschreibungen der verfügbaren Übereinstimmungsbedingungen für die [Regel-Engine](cdn-verizon-premium-rules-engine.md) des Azure Content Delivery Network (CDN) Premium von Verizon.

Der zweite Teil einer Regel ist die Übereinstimmungsbedingung. Eine Übereinstimmungsbedingung gibt bestimmte Typen von Anforderungen an, für die verschiedene sog. Features ausgeführt werden.

Sie können eine Übereinstimmungsbedingung z.B. für Folgendes verwenden:

- Filtern von Anforderungen für Inhalte an einem bestimmten Standort
- Filtern von Anforderungen, die von einer bestimmten IP-Adresse oder einem bestimmten Land bzw. einer bestimmten Region generiert wurden
- Filtern von Anforderungen nach Headerinformationen

Die neuesten Übereinstimmungsbedingungen finden Sie in der [Dokumentation zur Verizon-Regel-Engine](https://docs.vdms.com/cdn/index.html#Quick_References/HRE_QR.htm#Conditio).


## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über Azure Content Delivery Network](cdn-overview.md)
- [Regel-Engine – Referenz](cdn-verizon-premium-rules-engine-reference.md)
- [Regel-Engine – bedingte Ausdrücke](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Regel-Engine – Features](cdn-verizon-premium-rules-engine-reference-features.md)
- [Überschreiben des HTTP-Standardverhaltens mithilfe der Regel-Engine](cdn-verizon-premium-rules-engine.md)
