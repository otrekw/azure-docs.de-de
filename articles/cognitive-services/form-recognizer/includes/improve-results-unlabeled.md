---
author: laujan
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: lajanuar
ms.openlocfilehash: 89b035397ea2050ae7e61f2a19310b6a7fb4192c
ms.sourcegitcommit: 3ea12ce4f6c142c5a1a2f04d6e329e3456d2bda5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2021
ms.locfileid: "103467247"
---
Überprüfen Sie die `"confidence"`-Werte für jedes Schlüssel-Wert-Ergebnis unter dem Knoten `"pageResults"`. Sie sollten sich auch die Konfidenzscores im Knoten `"readResults"` ansehen, die dem Textlesevorgang entsprechen. Die Konfidenz der Leseergebnisse wirkt sich nicht auf die Konfidenz der Schlüssel-Wert-Extraktionsergebnisse aus, daher sollten Sie beide Bewertungen überprüfen.
* Wenn die Konfidenzscores für den Lesevorgang niedrig sind, versuchen Sie, die Qualität Ihrer Eingabedokumente zu verbessern (siehe [Anforderungen an die Eingabe](../overview.md#input-requirements)).
* Wenn die Konfidenzscores für den Schlüssel-Wert-Extraktionsvorgang niedrig sind, stellen Sie sicher, dass die analysierten Dokumente denselben Typ aufweisen wie die Dokumente, die im Trainingsset verwendet wurden. Wenn die Dokumente im Trainingsset Variationen im Aussehen aufweisen, erwägen Sie, die Dokumente auf verschiedene Ordner aufzuteilen und für jede Variation ein eigenes Modell zu trainieren.

Welche Konfidenzscores Sie benötigen, richtet sich nach Ihrem Anwendungsfall, aber im Allgemeinen hat es sich bewährt, einen Score von 80 % oder höher anzustreben. In Fällen, in denen ein höheres Maß an Vertraulichkeit erforderlich ist – beispielsweise beim Lesen von Patientenakten oder Abrechnungen –, wird ein Score von 100 % empfohlen.