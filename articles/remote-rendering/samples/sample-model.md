---
title: Beispielmodelle
description: Hier finden Sie eine Liste mit Quellen für Beispielmodelle.
author: florianborn71
ms.author: flborn
ms.date: 01/29/2020
ms.topic: sample
ms.openlocfilehash: 8e5dcb6c9dfa08efc0889fcab779d6cb333d2330
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2020
ms.locfileid: "88507534"
---
# <a name="sample-models"></a>Beispielmodelle

In diesem Artikel werden einige Ressourcen für Beispieldaten aufgelistet, die zum Testen des Azure Remote Rendering-Diensts verwendet werden können.

## <a name="built-in-sample-model"></a>Integriertes Beispielmodell

Wir stellen ein integriertes Beispielmodell bereit, das immer mithilfe der URL **builtin://Engine** geladen werden kann.

![Beispielmodell](./media/sample-model.png "Beispielmodell")

Modellstatistik:

| Name | Wert |
|-----------|:-----------|
| [Erforderliche Servergröße](../how-tos/session-rest-api.md#create-a-session) | Standard |
| Anzahl von Dreiecken | 18,7 Mio. |
| Anzahl beweglicher Teile | 2\.073 |
| Materialanzahl | 94 |

## <a name="third-party-data"></a>Drittanbieterdaten

Die Khronos Group pflegt eine Reihe von glTF-Beispielmodellen zu Testzwecken. Das glTF-Format wird von ARR sowohl in Textform ( *. gltf*) als auch in Binärform ( *.glb*) unterstützt. Die besten visuellen Ergebnisse lassen sich mit den PBR-Modellen erzielen:

* [glTF-Beispielmodelle](https://github.com/KhronosGroup/glTF-Sample-Models)

## <a name="next-steps"></a>Nächste Schritte

* [Schnellstart: Rendern eines Modells mit Unity](../quickstarts/render-model.md)
* [Schnellstart: Konvertieren eines Modells für das Rendering](../quickstarts/convert-model.md)
