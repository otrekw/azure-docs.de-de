---
title: 'Azure Front Door: Vergleich der Standard- und Premium-SKU'
description: Dieser Artikel enthält eine Übersicht über die Standard- und Premium-SKU von Azure Front Door sowie Informationen zu Featureunterschieden.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/18/2021
ms.author: amsriva
ms.openlocfilehash: 1753f2bb649e73d7a5fe6c1cc32361a418ea7f63
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102181804"
---
# <a name="overview-of-azure-front-door-standardpremium-sku-preview"></a>Übersicht über die Standard- und Premium-SKU von Azure Front Door (Vorschauversion)

> [!Note]
> Diese Dokumentation ist für Azure Front Door Standard/Premium (Vorschau) gedacht. Suchen Sie nach Informationen zu Azure Front Door? Informationen finden Sie [hier](../front-door-overview.md).

Azure Front Door wird für drei verschiedene SKUs angeboten: [Azure Front Door](../front-door-overview.md), Azure Front Door Standard (Vorschauversion) und Azure Front Door Premium (Vorschauversion). Die Standard- und Premium-SKUs von Azure Front Door kombinieren Funktionen von Azure Front Door, Azure CDN Standard von Microsoft und Azure WAF zu einer einzelnen sicheren CDN-Cloudplattform mit intelligentem Bedrohungsschutz.

> [!IMPORTANT]
> Azure Front Door Standard/Premium (Vorschau) befindet sich derzeit in der öffentlichen Vorschauversion.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie in den [**zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen**](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

* Für die **Standard-SKU von Azure Front Door** gilt Folgendes:

    * Für die Inhaltsbereitstellung optimiert
    * Ermöglicht die Beschleunigung statischer und dynamischer Inhalte
    * Globaler Lastenausgleich
    * SSL-Auslagerung
    * Domänen- und Zertifikatverwaltung
    * Erweiterte Datenverkehrsanalyse 
    * Grundlegende Sicherheitsfunktionen

* Die **Premium-SKU von Azure Front Door** baut auf den Funktionen der Standard-SKU auf und bietet zusätzlich Folgendes:

    * Umfassende WAF-weite Sicherheitsfunktionen
    * Bot-Schutz
    * Private Link-Unterstützung
    * Integration in Microsoft Threat Intelligence und Sicherheitsanalysen 

![Vergleichsdiagramm für die Front Door-SKUs](../media/tier-comparison/tier-comparison.png)

## <a name="feature-comparison"></a>Funktionsvergleiche

| Funktion |      Standard      |  Premium |
|----------|:-------------:|------:|
| Benutzerdefinierte Domänen | Ja | Ja |
| SSL-Auslagerung | Ja | Ja |
| Caching |  Ja  | Ja |
| Komprimierung | Ja | Ja   |
| Globaler Lastenausgleich | Ja  | Ja |
| Layer 7-Routing | Ja | Ja |
| URL Rewrite | Ja | Ja |
| Regelmodul | Ja | Ja |
| Privater Ursprung (Private Link) | Nein | Ja |
| WAF | Nur benutzerdefinierte Regeln | Ja |
| Bot-Schutz | Nein | Ja |
| Erweiterte Metriken und Diagnose | Ja | Ja |
| Datenverkehrsbericht | Ja | Ja |
| Sicherheitsbericht | Nein | Ja | 

## <a name="next-steps"></a>Nächste Schritte

[Schnellstart: Erstellen eines Azure Front Door Standard/Premium-Profils im Azure-Portal](create-front-door-portal.md)
