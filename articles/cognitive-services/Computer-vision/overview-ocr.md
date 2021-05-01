---
title: Was ist Optische Zeichenerkennung?
titleSuffix: Azure Cognitive Services
description: Die optische Zeichenerkennung (OCR) extrahiert sichtbaren Text in einem Bild und gibt ihn als strukturierte Zeichen folgen zurück.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
ms.date: 03/29/2021
ms.author: pafarley
ms.custom: seodec18, devx-track-csharp
ms.openlocfilehash: da4ada8b505c747d24738e175a1701b5ea73b4e4
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107536733"
---
# <a name="what-is-optical-character-recognition"></a>Was ist Optische Zeichenerkennung?

Der Dienst für die optische Zeichenerkennung (OCR) ermöglicht es Ihnen, gedruckten oder handschriftlichen Text aus Bildern (z. B. Fotos von Straßenschildern und Produkten) sowie aus Dokumenten (etwa Rechnungen, Finanzberichte, Artikel und vieles mehr) zu extrahieren. Hierbei werden auf Deep Learning basierende Modelle verwendet, und es wird Text auf vielen verschiedenen Oberflächen und Hintergründen verarbeitet.

Die OCR-APIs unterstützen das Extrahieren von gedrucktem Text in [mehreren Sprachen](./language-support.md). Nutzen Sie einen [Schnellstart](./quickstarts-sdk/client-library.md), um erste Schritte auszuführen.

![OCR-Demos](./Images/ocr-demo.gif)

Diese Dokumentation enthält die folgenden Arten von Artikeln:
* In den [Schnellstarts](./quickstarts-sdk/client-library.md) finden Sie Schritt-für-Schritt-Anleitungen, mit denen Sie Aufrufe an den Dienst senden können und in kurzer Zeit Ergebnisse erhalten. 
* Die [Anleitungen](./Vision-API-How-to-Topics/call-read-api.md) enthalten Anweisungen zur spezifischeren oder individuelleren Verwendung des Diensts.
<!--* The [conceptual articles](Vision-API-How-to-Topics/call-read-api.md) provide in-depth explanations of the service's functionality and features.
* The [tutorials](./tutorials/storage-lab-tutorial.md) are longer guides that show you how to use this service as a component in broader business solutions. -->

## <a name="supported-languages"></a>Unterstützte Sprachen
Die Lese-APIs unterstützen insgesamt 73 Sprachen für gedruckten Text. Eine vollständige Liste finden Sie unter [Sprachunterstützung für maschinelles Sehen](./language-support.md#optical-character-recognition-ocr). OCR für handgeschriebenen Text wird ausschließlich für Englisch unterstützt.

## <a name="input-requirements"></a>Eingabeanforderungen

Der Aufruf **Read** nimmt Bilder und Dokumente als Eingabe entgegen. Es bestehen die folgenden Anforderungen:

* Unterstützte Dateiformate: JPEG, PNG, BMP, PDF und TIFF
* Für PDF- und TIFF-Dateien werden bis zu 2000 Seiten (nur die ersten beiden Seiten für den Free-Tarif) verarbeitet.
* Die Dateigröße muss weniger als 50 MB (4 MB für den Free-Tarif) betragen und eine Größe von mindestens 50 x 50 Pixel und höchstens 10000 × 10000 Pixel aufweisen. 

## <a name="read-api"></a>Lese-API 

Die [Read-API](https://centraluseuap.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2/operations/5d986960601faab4bf452005) für maschinelles Sehen ist die neueste OCR-Technologie von Azure ([Neuerungen kennenlernen](./whats-new.md)), mit der sich gedruckter Text in mehreren Sprachen, handschriftlicher Text (nur Englisch), Ziffern und Währungssymbole aus Bildern und mehrseitigen PDF-Dokumenten extrahieren lassen. Die API ist für das Extrahieren von Text aus textlastigen Bildern und mehrseitigen PDF-Dokumenten mit gemischten Sprachen optimiert. Sie unterstützt sowohl die Erkennung von gedrucktem als auch von handschriftlichem Text im selben Bild oder Dokument.

![So konvertiert die optische Zeichenerkennung Bilder und Dokumente in eine strukturierte Ausgabe mit extrahiertem Text](./Images/how-ocr-works.svg)

### <a name="key-features"></a>Wichtigste Funktionen

Die Lese-API umfasst die folgenden Funktionen: 

* Extrahieren von Drucktext in 73 Sprachen
* Extrahieren von handschriftlichem Text in englischer Sprache
* Textzeilen und Wörter mit Standort und Zuverlässigkeitsbewertungen
* Keine Sprachenerkennung erforderlich
* Unterstützung für gemischte Sprachen und gemischten Modus (gedruckter und handschriftlicher Text)
* Auswählen von Seiten und Seitenbereichen aus großen, mehrseitigen Dokumenten
* Natürliche Leserichtung für Textzeilen
* Handschriftklassifizierung für Textzeilen
* Verfügbar als Docker-Container ohne Distribution für die lokale Bereitstellung

[Hier](./vision-api-how-to-topics/call-read-api.md) erfahren Sie mehr über die Verwendung der OCR-Features.

## <a name="use-the-cloud-api-or-deploy-on-premise"></a>Verwenden der Cloud-API oder lokale Bereitstellung
Die Read 3.x-Cloud-APIs sind die bevorzugte Option der meisten Kunden, da sie sich einfach integrieren und schnell vorgefertigt produktiv einsetzen lassen. Azure und der Dienst für maschinelles Sehen verarbeiten Skalierungs-, Leistungs-, Datensicherheits- und Complianceanforderungen, während Sie sich auf die Erfüllung der Anforderungen Ihrer Kunden konzentrieren.

Für die lokale Bereitstellung ermöglicht der [Read-Docker-Container (Vorschau)](./computer-vision-how-to-install-containers.md) Ihnen die Bereitstellung der neuen OCR-Funktionen in Ihrer eigenen lokalen Umgebung. Container eignen sich hervorragend für bestimmte Sicherheits- und Datengovernanceanforderungen.

## <a name="ocr-api"></a>OCR-API

Die legacy-[OCR-API](https://centraluseuap.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2/operations/56f91f2e778daf14a499f20d) verwendet ein älteres Erkennungsmodell, unterstützt nur Bilder und wird synchron ausgeführt, sodass der erkannte Text direkt zurückgegeben wird. Eine Liste der unterstützten Sprachen finden Sie in der Spalte OCR der [unterstützten Sprachen](./language-support.md#optical-character-recognition-ocr) .

> [!WARNING]
> Die Operationen von Computer Vision 2.0 RecognizeText werden derzeit zugunsten der neuen [Lese-API](#read-api), die in diesem Artikel behandelt wird, veraltet. Bestehende Kunden sollten dazu [übergehen, Lesevorgänge zu nutzen](upgrade-api-versions.md).

## <a name="data-privacy-and-security"></a>Datenschutz und Sicherheit

Wie bei allen Cognitive Services-Diensten müssen Entwickler, die den Maschinelles Sehen-Dienst nutzen, die Microsoft-Richtlinien zu Kundendaten beachten. Weitere Informationen finden Sie im Microsoft Trust Center auf der [Seite zu Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices).

## <a name="next-steps"></a>Nächste Schritte

- Beginnen Sie mit den [Schnellstartanleitungen für die OCR (Read)-REST-API oder für die Clientbibliothek](./quickstarts-sdk/client-library.md).
- Erfahren Sie mehr über die [Read 3.2-REST-API](https://centraluseuap.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2/operations/5d986960601faab4bf452005).
