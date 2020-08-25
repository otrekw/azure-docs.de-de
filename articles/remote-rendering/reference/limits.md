---
title: Einschränkungen
description: Codeeinschränkungen für SDK-Features
author: erscorms
ms.author: erscor
ms.date: 02/11/2020
ms.topic: reference
ms.openlocfilehash: af935aeaaeee11ab50b327b7f7b5b77246cb974b
ms.sourcegitcommit: 37afde27ac137ab2e675b2b0492559287822fded
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2020
ms.locfileid: "88566045"
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

| Servergröße | Maximale Anzahl von Polygonen |
|:--------|:------------------|
|Standard| 20 Mio. |
|premium| Keine Begrenzung |

Ausführlichere Informationen zu dieser Einschränkung finden Sie im Kapitel [Servergröße](../reference/vm-sizes.md).

## <a name="platform-limitations"></a>Plattformeinschränkungen

**Windows 10 Desktop**

* Win32/x64 ist die einzige unterstützte Win32-Plattform. Win32/x86 wird nicht unterstützt.

**Hololens 2**

* Die Funktion [Von PV-Kamera rendern](https://docs.microsoft.com/windows/mixed-reality/mixed-reality-capture-for-developers#render-from-the-pv-camera-opt-in) wird nicht unterstützt.
