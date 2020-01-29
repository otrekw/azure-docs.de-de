---
title: Auswählen des richtigen Tarifs | Microsoft Azure Maps
description: In diesem Artikel erfahren Sie mehr über die von Microsoft Azure Maps angebotenen Tarife.
author: walsehgal
ms.author: v-musehg
ms.date: 01/15/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 8764e9161f952118ca7ae28343dcd16477cf1eee
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76155756"
---
# <a name="choose-the-right-pricing-tier-in-azure-maps"></a>Auswählen des richtigen Tarifs in Azure Maps

Azure Maps bietet die beiden Tarife S0 und S1. Dieser Artikel soll Ihnen bei der Auswahl des richtigen Tarifs für Ihre Ansprüche helfen. Um den richtigen Tarif auszuwählen, sollten Sie sich die folgenden zwei Fragen stellen.

## <a name="what-geospatial-capabilities-do-i-plan-to-use"></a>Welche Geofunktionen planen Sie, zu verwenden?
Der Tarif S0 ist der richtige für Sie, wenn die Kerngeofunktions-APIs Ihre Dienstanforderungen erfüllen. Wenn Sie erweiterte Funktionen für Ihre Anwendung benötigen, sollten Sie den Tarif S1 in Erwägung ziehen. Beispiel für erweiterte Funktionen: Luft- und Hybridaufnahmen, Abruf von Routenbereichen und Batchgeocodierung. Die Tabelle **Tarife und Funktionen** kann Ihnen bei der Auswahl eines Tarifs helfen, der für Ihre Anwendung am besten geeignet ist.

## <a name="how-many-concurrent-users-do-i-plan-to-support"></a>Wie viele gleichzeitige Benutzer planen Sie, zu unterstützen? 
Mit den Tarifen S0 und S1 werden unterschiedliche Volumen an Datendurchsatz verarbeitet. Im Tarif S0 können bis zu **50 Abfragen pro Sekunde** verarbeitet werden, während im Tarif S1 **mehr als 50 Abfragen pro Sekunde** verarbeitet werden können.

### <a name="pricing-tier-capabilities"></a>Tarife und Funktionen

| Funktion                              |        S0           |  S1      |
|-----------------------------------------|:-------------------:|:--------:|
| Rendern von Karten                              | ✓                   | ✓       |
| Satellitenbilder                       |                     | ✓        |
| Suchen,                                  | ✓                    | ✓        |
| Batchsuche                            |                     | ✓        |
| Route                                   | ✓                    |✓        |
| Batchrouting                            |                    | ✓        |
| Matrixrouting                          |                     | ✓        |
| Routenbereich (Isochrone)                |                     | ✓        |
| Verkehr                                |✓                    |✓        |
| Zeitzone                               |✓                    |✓        |
| Geolocation (Vorschau)                    |✓                   |✓        |
| Räumliche Operationen                        |                    |✓        |
| Geofencing                                |                    |✓        |
| Azure Maps-Daten (Vorschau)                |                     | ✓        |
| Mobilität (Vorschau)                       |                     | ✓        |
| Wetter (Vorschau)                        |✓                    |✓        |

Diese zusätzliche Datenpunkte sollten in Betracht gezogen werden:
* In welchem Bereich ist Ihr Unternehmen tätig?
* Wie wichtig ist Ihre Anwendung?

### <a name="pricing-tier-targeted-customers"></a>Tarife und Kundenzielgruppen

Einen besseren Eindruck von den Tarifen S0 und S1 verschafft Ihnen die Tabelle **Tarife und Kundenzielgruppen**. Weitere Informationen finden Sie unter [Azure Maps – Preise](https://azure.microsoft.com/pricing/details/azure-maps/). 

| Tarif  |     Tarife und Kundenzielgruppen                                                                |
|-----------------|:-----------------------------------------------------------------------------------------|
| S0            |    <p>Der Tarif S0 kann für Anwendungen in allen Phasen der Produktion verwendet werden: von der Entwicklung einer Machbarkeitsstudie und frühen Testphasen bis hin zur Produktion und Bereitstellung von Anwendungen. Dieser Tarif ist jedoch für die Entwicklung im geringeren Umfang oder für Kunden mit einer geringen Anzahl gleichzeitiger Benutzer oder beides konzipiert. <p>|
| S1            |    <p>Der Tarif S1 richtet sich an Kunden, die Unterstützung für große Unternehmen, unternehmenskritische Anwendungen oder eine große Anzahl gleichzeitiger Benutzer benötigen. Er ist auch für Kunden geeignet, die komplexe Geodienste benötigen.</p>|

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das Anzeigen und Ändern von Tarifen:

> [!div class="nextstepaction"] 
> [Verwalten eines Tarifs](how-to-manage-pricing-tier.md)
