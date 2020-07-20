---
title: Einschränkungen
description: Codeeinschränkungen für SDK-Features
author: erscorms
ms.author: erscor
ms.date: 02/11/2020
ms.topic: reference
ms.openlocfilehash: 17f98f452764abdc8458cdc38661d464ecb0a60d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84808541"
---
# <a name="limitations"></a>Einschränkungen

Eine Reihe von Features besitzen Einschränkungen hinsichtlich Größe, Anzahl oder anderer Aspekte.

## <a name="azure-frontend"></a>Azure-Front-End

* Gesamtanzahl der Azure-Front-End-Instanzen pro Prozess: 16.
* Gesamtanzahl der Azure-Sitzungsinstanzen pro Azure-Front-End: 16.

## <a name="objects"></a>Objekte

* Zulässige Gesamtanzahl zulässiger Objekte eines einzelnen Typs (Entität, CutPlaneComponent usw.): 16.777.215.
* Zulässige Gesamtanzahl aktiver Schnittebenen: 8.

## <a name="geometry"></a>Geometrie

* Zulässige Gesamtanzahl von Materialien in einer Ressource: 65.535.
* Maximale Dimension einer einzelnen Textur: 16.384 x 16.384. Größere Quelltexturen werden durch den Konvertierungsprozess herunterskaliert.

## <a name="overall-number-of-polygons"></a>Gesamtanzahl von Polygonen

Die zulässige Anzahl von Polygonen für alle geladenen Modelle ist von der Größe der VM abhängig, die an die [Sitzungsverwaltungs-REST-API](../how-tos/session-rest-api.md#create-a-session) übergeben wurde:

| Größe des virtuellen Computers | Maximale Anzahl von Polygonen |
|:--------|:------------------|
|Standard| 20 Mio. |
|premium| Keine Begrenzung |

Ausführlichere Informationen zu dieser Einschränkung finden Sie im Kapitel [VM-Größe](../reference/vm-sizes.md).

## <a name="platform-limitations"></a>Plattformeinschränkungen

**Windows 10 Desktop**

* UWP/x86 ist die einzige unterstützte UWP-Plattform. UWP/x64 wird nicht unterstützt.
* Win32/x64 ist die einzige unterstützte Win32-Plattform. Win32/x86 wird nicht unterstützt.

**Hololens 2**

* Die Funktion [Von PV-Kamera rendern](https://docs.microsoft.com/windows/mixed-reality/mixed-reality-capture-for-developers#render-from-the-pv-camera-opt-in) wird nicht unterstützt.
