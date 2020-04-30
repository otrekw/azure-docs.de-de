---
title: 'Auswählen einer Domäne für ein Custom Vision-Projekt: maschinelles Sehen'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erfahren Sie, wie Sie eine Domäne für Ihr Projekt im Custom Vision Service auswählen.
services: cognitive-services
author: shonohs
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/06/2020
ms.author: shono
ms.openlocfilehash: 1fb30cc0634224213dc9a188a16902e07d379904
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82127771"
---
# <a name="select-a-domain-for-a-custom-vision-project"></a>Auswählen einer Domäne für ein Custom Vision-Projekt

Auf dem Blatt „Einstellungen“ für das Custom Vision-Projekt können Sie eine Domäne für das Projekt auswählen. Wählen Sie die Domäne, die Ihrem Szenario am nächsten kommt.

## <a name="image-classification"></a>Bildklassifizierung

|Domain|Zweck|
|---|---|
|__Allgemein__| Für eine breite Palette von Aufgaben in der Bildklassifizierung optimiert. Wenn keine der anderen Domänen geeignet erscheint oder Sie unsicher sind, welche Domäne Sie wählen sollen, verwenden Sie die allgemeine Domäne.|
|__Food (Lebensmittel)__|Für Fotos von Gerichten optimiert, wie sie beispielsweise auf der Speisekarte von Restaurants abgebildet werden. Wenn Sie Fotos von einzelnen Früchten oder Gemüsen klassifizieren möchten, verwenden Sie die Domäne „Food“.|
|__Landmarks (Wahrzeichen)__|Optimiert für erkennbare Wahrzeichen (Naturdenkmäler oder künstlich geschaffene Wahrzeichen). Diese Domäne funktioniert am besten, wenn das Wahrzeichen im Foto deutlich zu sehen ist. Die Domäne funktioniert auch, wenn das Wahrzeichen etwas von Personen im Vordergrund verdeckt wird.|
|__Retail (Einzelhandel)__|Für Bilder optimiert, wie man sie in einem Einkaufskatalog oder auf einer Einkaufswebsite findet. Wenn Sie eine präzise Klassifizierung zwischen Kleidern, Hosen und Hemden wünschen, verwenden Sie diese Domäne.|
|__Kompaktdomänen__| Für die Bedingungen der Echtzeitklassifizierung auf Edgegeräten optimiert.|

## <a name="object-detection"></a>Objekterkennung

|Domain|Zweck|
|---|---|
|__Allgemein__| Für eine Vielzahl von Aufgaben der Objekterkennung optimiert. Wenn keine der anderen Domänen geeignet erscheint oder Sie unsicher sind, welche Domäne Sie wählen sollen, verwenden Sie die Domäne „Generic“.|
|__Logo__|Für die Suche nach Markenlogos in Bildern optimiert.|
|__Produkte in Regalen__|Für die Erkennung und Klassifizierung von Produkten in Regalen optimiert.|
|__Kompaktdomänen__| Für die Bedingungen der Echtzeitobjekterkennung auf Edgegeräten optimiert.|

## <a name="compact-domains"></a>Kompaktdomänen

Die von Kompaktdomänen generierten Modelle können für die lokale Ausführung exportiert werden. Die Modellleistung variiert je nach ausgewählter Domäne. In der folgenden Tabelle geben wir die Modellgröße und die Rückschlusszeit auf der Intel Desktop-CPU und der NVIDIA-GPU \[1\] an. 

> [!NOTE]
> Diese Zahlen enthalten keine Vor- und Nachverarbeitungszeit.

|Aufgabe|Domain|Modellgröße|CPU-Rückschlusszeit|GPU-Rückschlusszeit|
|---|---|---|---|---|
|Klassifizierung|Allgemein (kompakt)|5 MB|13 ms|5 ms|
|Objekterkennung|Allgemein (kompakt)|45 MB|35 ms|5 ms|
|Objekterkennung|Allgemein (kompakt) [$1]|14 MB|27 ms|7 ms|

## <a name="vaidk-vision-ai-dev-kit"></a>VAIDK (Vision AI Dev Kit)

Wenn eine kompakte Domäne ausgewählt ist, wird die zusätzliche Option „Exportieren von Funktionen“ bereitgestellt, die das Unterscheiden zwischen „Basisplattformen“ und „Vision AI Dev Kit“ ermöglicht.

Unter _Exportieren von Funktionen_ sind diese beiden Optionen verfügbar:

- Basisplattformen (Tensorflow, CoreML, ONNX usw.)
- Vision AI Dev Kit.

Wenn _Vision AI Dev Kit_ ausgewählt ist, sind die kompakten Domänen _Generisch_, _Besondere Merkmale_ und _Einzelhandel_, aber nicht _Lebensmittel_ für die Bildklassifizierung verfügbar, während sowohl _Allgemein (kompakt)_ als _Allgemein (kompakt) [S1]_ für die Objekterkennung verfügbar sind.

>[!NOTE]
>Die Domäne __Allgemein (kompakt)__ für die Objekterkennung erfordert eine spezielle Nachverarbeitungslogik. Die Einzelheiten entnehmen Sie einem Beispielskript im exportierten ZIP-Paket. Wenn Sie ein Modell ohne die Nachverarbeitungslogik benötigen, verwenden Sie __Allgemein (kompakt) [S1]__ .

>[!IMPORTANT]
>Es gibt keine Garantie dafür, dass die exportierten Modelle genau das gleiche Ergebnis wie die Vorhersage-API in der Cloud liefern. Geringfügige Unterschiede bei der Ausführungsplattform oder der Vorverarbeitungsimplementierung können größere Unterschiede in den Modellausgaben verursachen. Die Einzelheiten zur Vorverarbeitungslogik finden Sie in [diesem Dokument](quickstarts/image-classification.md).

\[1\] Intel Xeon E5-2690 CPU und NVIDIA Tesla M60
