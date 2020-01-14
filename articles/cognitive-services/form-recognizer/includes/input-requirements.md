---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: pafarley
ms.openlocfilehash: 11783f8fac147e6fb2cf371ee1a588cf318a5e9c
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75379429"
---
Die Formularerkennung arbeitet mit Eingabedokumenten, die diese Anforderungen erfüllen:

* Das Format muss JPG, PNG, PDF (Text oder Scan) oder TIFF sein. In Text eingebettete PDFs eignen sich besser, da beim Extrahieren von Zeichen und Speicherort keine Fehler entstehen können.
* Wenn Ihre PDF-Dateien mit einem Kennwort geschützt sind, müssen Sie die Sperre entfernen, bevor Sie sie senden.
* PDF- und TIFF-Dokumente dürfen höchstens einen Umfang von 200 Seiten haben, und die Gesamtgröße des Trainingsdatasets darf höchstens 500 Seiten betragen.
* Bei Bildern müssen die Abmessungen zwischen 600 × 100 Pixel und 4200 × 4200 Pixel liegen.
* Werden Papierdokumente eingescannt, sollten dies Scans mit hoher Qualität sein.
* Für Text muss das lateinische Alphabet (englische Zeichen) verwendet werden.
* Für nicht überwachtes Lernen (ohne beschriftete Daten) müssen Daten Schlüssel und Werte enthalten.
* Für nicht überwachtes Lernen (ohne beschriftete Daten) müssen die Schlüssel oberhalb der Werte oder links davon angezeigt werden. Sie können nicht darunter oder rechts davon angezeigt werden.

Die Formularerkennung unterstützt derzeit nicht die folgenden Arten von Eingabedaten:

* Komplexe Tabellen (geschachtelte Tabellen, zusammengeführte Header oder Zellen usw.)
* Kontrollkästchen oder Optionsfelder
