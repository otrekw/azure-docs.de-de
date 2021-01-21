---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: include
ms.date: 07/17/2019
ms.author: pafarley
ms.openlocfilehash: d07a5da3b9013700694f6c20102ef2e8c5066087
ms.sourcegitcommit: c7153bb48ce003a158e83a1174e1ee7e4b1a5461
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2021
ms.locfileid: "98256461"
---
Es wird empfohlen, im ersten Trainingssatz mindestens 30 Bilder pro Tag zu verwenden. Sie sollten auch einige zusätzliche Bilder sammeln, um Ihr Modell zu testen, nachdem es trainiert wurde.

Verwenden Sie zum effektiven Trainieren Ihres Modells Bilder mit optischer Vielfalt. Wählen Sie Bilder aus, die sich nach folgenden Aspekten unterscheiden:
* Kamerawinkel
* Belichtung
* background
* Visueller Stil
* Einzelne/gruppierte Motive
* size
* type

Stellen Sie außerdem sicher, dass alle Ihre Trainingsbilder die folgenden Kriterien erfüllen:
* JPG-, PNG-, BMP- oder GIF-Format
* Höchstens 6 MB groß (4 MB für Vorhersagebilder)
* Mindestens 256 Pixel an der kürzesten Seite; kürzere Bilder werden von Custom Vision Service automatisch hochskaliert.

> [!NOTE]
> Mit Trove, einem Microsoft Garage-Projekt, können Sie Bilder zu Trainingszwecken sammeln und erwerben. Die gesammelten Bilder können Sie dann herunterladen und wie gewohnt in Ihr Custom Vision-Projekt importieren. Weitere Informationen finden Sie unter [Trove](https://www.microsoft.com/en-us/ai/trove?activetab=pivot1:primaryr3).