---
author: laujan
ms.service: applied-ai-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 05/25/2021
ms.author: lajanuar
ms.openlocfilehash: 32e6f5ab49c36b8675c30868ca8aa919f65b6878
ms.sourcegitcommit: 34feb2a5bdba1351d9fc375c46e62aa40bbd5a1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111891015"
---
Überprüfen Sie die `"confidence"`-Werte für jedes Schlüssel-Wert-Ergebnis unter dem Knoten `"pageResults"`. Betrachten Sie auch die Konfidenzscores im Knoten `"readResults"`, die dem Textlesevorgang entsprechen. Die Konfidenz der Leseergebnisse wirkt sich nicht auf die Konfidenz der Schlüssel-Wert-Extraktionsergebnisse aus, daher überprüfen Sie beide Bewertungen.

* Wenn die Konfidenzscores für den Lesevorgang niedrig sind, versuchen Sie, die Qualität Ihrer Eingabedokumente zu verbessern (siehe [Anforderungen an die Eingabe](../../overview.md#input-requirements)).

* Wenn die Konfidenzscores für den Schlüssel-Wert-Extraktionsvorgang niedrig sind, stellen Sie sicher, dass die analysierten Dokumente denselben Typ aufweisen wie die Dokumente, die im Trainingsset verwendet wurden. Wenn die Dokumente im Trainingsset Variationen im Aussehen aufweisen, erwägen Sie, die Dokumente auf verschiedene Ordner aufzuteilen und für jede Variation ein eigenes Modell zu trainieren.

Welche Konfidenzscores Sie benötigen, richtet sich nach Ihrem Anwendungsfall, aber im Allgemeinen hat es sich bewährt, einen Score von 80 % oder höher anzustreben. In Fällen, in denen ein höheres Maß an Vertraulichkeit erforderlich ist – beispielsweise beim Lesen von Patientenakten oder Abrechnungen –, wird ein Score von 100 % empfohlen.