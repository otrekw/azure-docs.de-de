---
title: Beispielmodelle
description: Hier finden Sie eine Liste mit Quellen für Beispielmodelle.
author: florianborn71
ms.author: flborn
ms.date: 01/29/2020
ms.topic: sample
ms.openlocfilehash: 0c28d36c7934fbbac0ddd11562c8dc237b0360ee
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80678491"
---
# <a name="sample-models"></a>Beispielmodelle

In diesem Artikel werden einige Ressourcen für Beispieldaten aufgelistet, die zum Testen des Azure Remote Rendering-Diensts verwendet werden können.

## <a name="built-in-sample-model"></a>Integriertes Beispielmodell

Wir stellen ein integriertes Beispielmodell bereit, das immer mithilfe der URL **builtin://Engine** geladen werden kann.

![Beispielmodell](./media/sample-model.png "Beispielmodell")

Modellstatistik:

| Name | Wert |
|-----------|:-----------|
| [Erforderliche VM-Größe](../how-tos/session-rest-api.md#create-a-session) | Standard |
| Anzahl von Dreiecken | 18,7 Mio. |
| Anzahl beweglicher Teile | 2\.073 |
| Materialanzahl | 94 |

## <a name="third-party-data"></a>Drittanbieterdaten

Die Khronos Group pflegt eine Reihe von glTF-Beispielmodellen zu Testzwecken. Das glTF-Format wird von ARR sowohl in Textform ( *. gltf*) als auch in Binärform ( *.glb*) unterstützt. Die besten visuellen Ergebnisse lassen sich mit den PBR-Modellen erzielen:

* [glTF-Beispielmodelle](https://github.com/KhronosGroup/glTF-Sample-Models)

## <a name="next-steps"></a>Nächste Schritte

* [Schnellstart: Rendern eines Modells mit Unity](../quickstarts/render-model.md)
* [Schnellstart: Konvertieren eines Modells für das Rendering](../quickstarts/convert-model.md)
