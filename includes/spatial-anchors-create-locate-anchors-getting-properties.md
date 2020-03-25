---
ms.openlocfilehash: c5ca85c0dfe8d601821a78c02b2230c0909c8003
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "67177802"
---
Sie können den Speicherort eines Ankers nicht aktualisieren, nachdem er für den Dienst erstellt wurde – Sie müssen einen neuen Anker erstellen und den alten löschen, um eine neue Position zu verfolgen.

Wenn Sie einen Anker nicht suchen müssen, um seine Eigenschaften zu aktualisieren, können Sie die `GetAnchorPropertiesAsync()`-Methode verwenden, die ein `CloudSpatialAnchor`-Objekt mit Eigenschaften zurückgibt.
