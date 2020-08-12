---
title: Was ist Custom Vision?
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie mithilfe des Custom Vision-Diensts benutzerdefinierte Bildklassifizierungen in der Azure-Cloud erstellen.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: overview
ms.date: 08/05/2020
ms.author: pafarley
ms.openlocfilehash: 86e2d54cfe7559675e5e878cbc8a0df4b4eb3d0b
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87834285"
---
# <a name="what-is-custom-vision"></a>Was ist Custom Vision?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Azure Custom Vision ist ein Cognitive Services-Dienst zum Erstellen, Bereitstellen und Optimieren benutzerdefinierter Bildklassifizierungen. Eine Bildklassifizierung ist ein KI-Dienst, der entsprechend den visuellen Merkmalen in einem Bild Bezeichnungen (die _Klassen_ darstellen) auf Bilder anwendet. Anders als beim Dienst für [maschinelles Sehen](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home) können Sie bei Custom Vision die Klassifizierungen angeben, die Sie anwenden möchten.

## <a name="what-it-does"></a>Funktionsbeschreibung

Der Custom Vision-Dienst verwendet einen Machine Learning-Algorithmus, um Bezeichnungen auf Bilder anzuwenden. Entwickler müssen Gruppen von Bildern übermitteln, auf denen die betreffenden Merkmale vorhanden bzw. nicht vorhanden sind. Die Bezeichnungen wenden Sie selbst beim Übermitteln auf die Bilder an. Der Algorithmus wird dann mit diesen Daten trainiert und berechnet seine eigene Genauigkeit, indem er Tests anhand dieser Bilder durchführt. Nachdem der Algorithmus trainiert wurde, können Sie ihn testen, erneut trainieren und schließlich gemäß den Anforderungen Ihrer App zum Klassifizieren neuer Bilder verwenden. Darüber hinaus kann das Modell exportiert und offline verwendet werden.

### <a name="classification-and-object-detection"></a>Klassifizierung und Objekterkennung

Custom Vision bietet zwei Features: Die **Bildklassifizierung** wendet eine oder mehrere Bezeichnungen auf ein Bild an. Die **Objekterkennung** funktioniert ähnlich, gibt aber auch die Koordinaten zurück, an denen sich die angewendeten Bezeichnungen im Bild befinden.

### <a name="optimization"></a>Optimization

Der Custom Vision-Dienst ist für die schnelle Erkennung wesentlicher Unterschiede zwischen Bildern optimiert, sodass Sie anhand einer kleinen Datenmenge mit der Prototyperstellung für Ihr Modell beginnen können. 50 Bilder pro Bezeichnung sind im Allgemeinen ein guter Ausgangspunkt. Für die Erkennung geringfügiger Bildunterschiede (etwa kleinere Risse oder Dellen in Qualitätssicherungsszenarien) ist der Dienst dagegen weniger geeignet.

Darüber hinaus stehen verschiedene Varianten des Custom Vision-Algorithmus zur Verfügung, die jeweils für Bilder mit bestimmtem Material optimiert sind, z. B. für Wahrzeichen oder Einzelhandelsartikel. Weitere Informationen finden Sie im Leitfaden [Erstellen einer Klassifizierung](getting-started-build-a-classifier.md).

## <a name="what-it-includes"></a>Lieferumfang

Der Custom Vision-Dienst ist als Gruppe nativer SDKs sowie über eine webbasierte Oberfläche auf der [Custom Vision-Startseite](https://customvision.ai/) verfügbar. Beide Schnittstellen können einzeln oder zusammen zum Erstellen, Testen und Trainieren eines Modells verwendet werden.

![Custom Vision-Startseite in einem Chrome-Browserfenster](media/browser-home.png)

## <a name="data-privacy-and-security"></a>Datenschutz und Sicherheit

Wie bei allen Cognitive Services-Diensten müssen Entwickler, die den Custom Vision-Dienst nutzen, die Microsoft-Richtlinien zu Kundendaten beachten. Weitere Informationen finden Sie im Microsoft Trust Center auf der [Seite zu Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices).

## <a name="next-steps"></a>Nächste Schritte

Unter [Erstellen einer Klassifizierung](getting-started-build-a-classifier.md) werden die ersten Schritte mit Custom Vision im Web beschrieben. Sie können aber auch ein [Tutorial zur Bildklassifizierung](quickstarts/image-classification.md) absolvieren, um ein einfaches Szenario mit Code zu implementieren.
