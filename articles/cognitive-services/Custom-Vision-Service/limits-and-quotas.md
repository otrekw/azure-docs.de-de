---
title: Grenzwerte und Kontingente – Custom Vision Service
titleSuffix: Azure Cognitive Services
description: In diesem Artikel werden die unterschiedlichen Typen von Lizenzschlüsseln sowie die Grenzwerte und Kontingente für den Custom Vision-Dienst erläutert.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: pafarley
ms.openlocfilehash: b79bf5e8ead16bbdf9c69e8d7faae43fa778ab3e
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91316578"
---
# <a name="limits-and-quotas"></a>Grenzen und Kontingente

Es gibt zwei Schlüsselebenen für Custom Vision Service. Sie können sich über das Azure-Portal für ein F0- (kostenlos) oder S0-Abonnement (Standard) registrieren. Auf der entsprechenden [Cognitive Services-Preisseite](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) finden Sie Informationen zu Preisen und Transaktionen.

Es wird erwartet, dass die Anzahl der Bilder pro Projekt und die Anzahl der Tags pro Projekt für S0-Projekte mit der Zeit ansteigen.

|Faktor|**F0**|**S0**|
|-----|-----|-----|
|Projekte|2|100|
|Trainingsbilder pro Projekt |5\.000|100.000|
|Vorhersagen pro Monat|10.000 |Unbegrenzt|
|Tags pro Projekt|50|500|
|Iterationen |10|10|
|Mindestanzahl der gekennzeichneten Bilder pro Tag, Klassifizierung (> 50 empfohlen) |5|5|
|Mindestanzahl der gekennzeichneten Bilder pro Tag, Objekterkennung (> 50 empfohlen)|15|15|
|Dauer der Speicherung von Vorhersagebildern|30 Tage|30 Tage|
|[Vorhersagevorgänge](https://go.microsoft.com/fwlink/?linkid=865445) mit Speicher (Transaktionen pro Sekunde)|2|10|
|[Vorhersagevorgänge](https://go.microsoft.com/fwlink/?linkid=865445) ohne Speicher (Transaktionen pro Sekunde)|2|20|
|[TrainProject](https://go.microsoft.com/fwlink/?linkid=865446) (API-Aufrufe pro Sekunde)|2|10|
|[Sonstige API-Aufrufe](https://go.microsoft.com/fwlink/?linkid=865446) (Transaktionen pro Sekunde)|10|10|
|Akzeptierte Bildtypen|JPG, PNG, BMP, GIF|JPG, PNG, BMP, GIF|
|Min. Bildhöhe/-breite in Pixel|256 (siehe Hinweis)|256 (siehe Hinweis)|
|Max. Bildhöhe/-breite in Pixel|10.240|10.240|
|Maximale Bildgröße (Upload des Trainingsbilds) |6 MB|6 MB|
|Maximale Bildgröße (Vorhersage)|4 MB|4 MB|
|Maximale Anzahl von Regionen pro Objekterkennungs-Trainingsimage|300|300|
|Maximale Anzahl von Tags pro Klassifizierungsimage|100|100|

> [!NOTE]
> Bilder unter 256 Pixel werden akzeptiert, jedoch hochskaliert.
> Das Seitenverhältnis des Bilds sollte nicht größer als 25 sein.
