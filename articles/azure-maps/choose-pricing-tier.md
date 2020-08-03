---
title: Auswählen des richtigen Tarifs für Microsoft Azure Maps
description: In diesem Artikel erfahren Sie mehr über die von Microsoft Azure Maps angebotenen Tarife.
author: anastasia-ms
ms.author: v-stharr
ms.date: 07/27/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 359c2270f3de269adae13ce976cedeb4248935d2
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87285763"
---
# <a name="choose-the-right-pricing-tier-in-azure-maps"></a>Auswählen des richtigen Tarifs in Azure Maps

Azure Maps bietet zwei Tarife: S0 und S1. Dieser Artikel soll Ihnen bei der Auswahl des richtigen Tarifs für Ihre Ansprüche helfen. Um den richtigen Tarif auszuwählen, sollten Sie sich die folgenden zwei Fragen stellen.

## <a name="how-many-concurrent-users-do-i-plan-to-support"></a>Wie viele gleichzeitige Benutzer planen Sie, zu unterstützen?

Mit den Tarifen S0 und S1 werden unterschiedliche Volumen an Datendurchsatz verarbeitet. Mit dem Tarif S0 werden bis zu **50 Abfragen pro Sekunde** verarbeitet. Während mit dem Tarif S1 **mehr als 50 Abfragen pro Sekunde** verarbeitet werden.

## <a name="what-geospatial-capabilities-do-i-plan-to-use"></a>Welche Geofunktionen planen Sie, zu verwenden?

Wenn die Kerngeofunktions-APIs Ihre Dienstanforderungen erfüllen, wählen Sie den Tarif S0 aus. Wenn Sie erweiterte Funktionen für Ihre Anwendung benötigen, sollten Sie den Tarif S1 auswählen. Erweiterte Funktionen umfassen: Luft- und Hybridaufnahmen, Abruf von Routenbereichen und Batchgeocodierung. Um den Tarif auszuwählen, der für Ihre Anwendung am besten geeignet ist, lesen Sie die folgende Tabelle **Tarife und Funktionen**:

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

Beachten Sie diese zusätzlichen Erwägungen:

* In welchem Bereich ist Ihr Unternehmen tätig?
* Wie wichtig ist Ihre Anwendung?

### <a name="pricing-tier-targeted-customers"></a>Tarife und Kundenzielgruppen

Einen besseren Eindruck von den Tarifen S0 und S1 verschafft Ihnen die Tabelle **Tarife und Kundenzielgruppen**. Weitere Informationen finden Sie unter [Azure Maps – Preise](https://azure.microsoft.com/pricing/details/azure-maps/). 

| Tarif  |     Tarife und Kundenzielgruppen                                                                |
|-----------------|:-----------------------------------------------------------------------------------------|
| S0            |    Der Tarif S0 kann für Anwendungen in allen Phasen der Produktion verwendet werden: von der Entwicklung einer Machbarkeitsstudie und frühen Testphasen bis hin zur Produktion und Bereitstellung von Anwendungen. Dieser Tarif ist jedoch für die Entwicklung im geringeren Umfang oder für Kunden mit einer geringen Anzahl gleichzeitiger Benutzer oder beides konzipiert. 
| S1            |    Der Tarif S1 richtet sich an Kunden mit großen Unternehmensanwendungen, unternehmenskritischen Anwendungen oder einer großen Anzahl gleichzeitiger Benutzer. Er ist auch für Kunden geeignet, die komplexe Geodienste benötigen.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das Anzeigen und Ändern von Tarifen:

> [!div class="nextstepaction"]
> [Verwalten eines Tarifs](how-to-manage-pricing-tier.md)
