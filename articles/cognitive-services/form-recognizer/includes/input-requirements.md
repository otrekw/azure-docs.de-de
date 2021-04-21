---
author: laujan
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: lajanuar
ms.openlocfilehash: dfd2b32094aae06675ea8ee9157370f9d2833e91
ms.sourcegitcommit: db925ea0af071d2c81b7f0ae89464214f8167505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2021
ms.locfileid: "107518223"
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
* Für nicht überwachtes Lernen (ohne beschriftete Daten) müssen Daten Schlüssel und Werte enthalten.
* Für nicht überwachtes Lernen (ohne beschriftete Daten) müssen die Schlüssel oberhalb der Werte oder links davon angezeigt werden. Sie können nicht darunter oder rechts davon angezeigt werden.
