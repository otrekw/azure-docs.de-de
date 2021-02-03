---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: pafarley
ms.openlocfilehash: 7cf3d86eeea9d1b0f5fcbb757d3597e21cbcc369
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/27/2021
ms.locfileid: "98900952"
---
Die Formularerkennung arbeitet mit Eingabedokumenten, die diese Anforderungen erfüllen:

* Das Format muss JPG, PNG, PDF (Text oder Scan) oder TIFF sein. In Text eingebettete PDFs eignen sich besser, da beim Extrahieren von Zeichen und Speicherort keine Fehler entstehen können.
* Die Datei muss kleiner sein als 50 MB.
* Bei Bildern müssen die Abmessungen zwischen 50 × 50 Pixel und 10.000 × 10.000 Pixel liegen.
* Die Abmessungen bei PDFs dürfen maximal 17 x 17 Zoll betragen. Dies entspricht den Papiergrößen Legal oder A3 und kleineren Formaten.
* Bei PDF- und TIFF-Dateien werden nur die ersten 200 Seiten verarbeitet. (Bei einem Abonnement im Free-Tarif werden nur die ersten beiden Seiten verarbeitet.)
* Die Gesamtgröße des Trainingsdatasets darf maximal 500 Seiten betragen.
* Wenn Ihre PDF-Dateien mit einem Kennwort geschützt sind, müssen Sie die Sperre entfernen, bevor Sie sie senden.
* Werden Papierdokumente eingescannt, sollten dies Scans mit hoher Qualität sein.
* Für Text muss das lateinische Alphabet (englische Zeichen) verwendet werden.
* Für nicht überwachtes Lernen (ohne beschriftete Daten) müssen Daten Schlüssel und Werte enthalten.
* Für nicht überwachtes Lernen (ohne beschriftete Daten) müssen die Schlüssel oberhalb der Werte oder links davon angezeigt werden. Sie können nicht darunter oder rechts davon angezeigt werden.
